## Java调用SBO DI之Com异常处理

### 问题来源

​		最近有个项目有个SBO项目总是报错：`A COM exception has been encountered: At invoke of: ADD The server threw an Exception`，有时候甚至会导致服务宕掉。

​		项目说明：这个项目对外提供一个API服务，实现对SBO的数据同步。逻辑是接收外部系统的数据，然后判断公司代码和单据类型，连接账套并调用DI。所以这里涉及到的COM Exception就是SBO DI内部抛出的异常。

### SBO DI 的实现

在java中若要调用SBO DI，需先引入`sboapi.jar`和`sapwrapper.jar`两个jar包，但是在`sboapi.jar`和`sapwrapper.jar`中并没有直接采用`Java`来实现对SBO的操作，而是通过`JNI`调用SBO动态链接库（sbojni.dll）来实现的，真正实现对SBO的操作在`sbojni.dll`中。

```java
###com.sap.smb.sbo.wrapper.util.WrapperUtil
public static synchronized void loadSBOJNILibrary() {
    if (!isSBOJNILibraryLoaded) {
        String var0 = "sbojni";
        Properties var1 = new Properties();

        try {
            InputStream var2 = (class$com$sap$smb$sbo$wrapper$util$WrapperUtil == null ? (class$com$sap$smb$sbo$wrapper$util$WrapperUtil = class$("com.sap.smb.sbo.wrapper.util.WrapperUtil")) : class$com$sap$smb$sbo$wrapper$util$WrapperUtil).getResourceAsStream("/wrapper.properties");
            if (var2 != null) {
                var1.load(var2);
                var0 = var1.getProperty("sbo.jni.library.name", "sbojni");
            }
        } catch (IOException var3) {
            ;
        }

        System.loadLibrary(var0);
        isSBOJNILibraryLoaded = true;
    }

}
```

+ sboapi.jar

  `sboapi.jar`封装了SBO业务对象及业务操作，通常我们的业务逻辑部分都直接使用此jar包里面的对象。

+ sbowrapper.jar

  `sbowrapper.jar`是对com组件调用的一个包装集合，供`sboapi.jar`调用。

  ```java
  ###com.sap.smb.sbo.wrapper.com.Dispatch
  public void setDescription(String var1) {
      Dispatch.put(this.m_BinLocation, "Description", var1);
  }
  ```

  

### 解决过程

1. SBO DI相关问题，当然首先是去SAP官网搜索，找到一个回答说需要调整`stack size`。但是将`stack size`调整至1M，2M后仍然无效果。

2. 虽然调整了`stack size`无果，但是将思路引到了内存方面来，采用`jmap -head pid`，查看堆内存一切正常。同时服务刚启动时候和抛异常时进程的内存占用正常，而在正常调用SBO DI阶段，进程内存突然增加了几百M，而堆内存最大设置的256M，所以猜测问题应该是调用SBO的native方法引起的。

3. demo测试

   ```java
   private static ICompany company;
   
   public static void main(String[] args) {
       System.out.println("begin test..... ");
       int run = 0;
       while (run < 10000) {
           run ++ ;
           System.out.println("第" + run +"次test");
           createDocument(connect());
       }
       System.out.println("finished test..... ");
   }
   
   private static ICompany connect() {
       if(company != null && company.isconnect()){
           return company;
       }
        company = SBOCOMUtil.newCompany();
       company.setServer("localhost:30015");
       company.setCompanyDB("TEST");
       company.setUserName("Interf");
       company.setPassword("XXXXXX");
       company.setDbServerType(SBOCOMConstants.BoDataServerTypes_dst_HANADB);
       company.setUseTrusted(false);
       company.setLanguage(SBOCOMConstants.BoSuppLangs_ln_English);
       company.setDbUserName("SYSTEM");
       company.setDbPassword("SYSTEM");
       company.setSLDServer("XX");
       company.setLicenseServer("XXX");
   
       int connectionResult = company.connect();
       if (connectionResult == 0) {
           System.out.println("Successfully connected to " + company.getCompanyName());
       } else {
           SBOErrorMessage errMsg = company.getLastError();
           System.out.println("Cannot connect to server: "
                   + errMsg.getErrorMessage()
                   + " "
                   + errMsg.getErrorCode());
       }
       return company;
   }
   
   private static void createDocument(ICompany company)  {
       IDocuments documents = null;
       try{
           documents = SBOCOMUtil.newDocuments(company, SBOCOMConstants.BoObjectTypes_Document_oInvoices);
           documents.setCardCode("TB001");
           documents.setDocDate(new Date());
           documents.setDocDueDate(new Date());
           documents.setTaxDate(new Date());
           int i = 0;
           while (i < 10) {
               i++;
               documents.getLines().setItemCode("9781201389668");
               documents.getLines().setWarehouseCode("01");
               documents.getLines().setQuantity(10.0);
               documents.getLines().setPriceAfterVAT(12.0);
               documents.getLines().add();
           }
           int rntCode = documents.add();
           if (rntCode == 0) {
               System.out.println("create successful:" + company.getNewObjectKey());
           } else {
               //System.gc();
               System.out.println("create faild:" + company.getLastErrorCode() + ";" + company.getLastErrorDescription());
               //断开company连接
               company.disconnect();
               company.release();
               company = null;
           }
       }catch (Exception e){
           System.out.println(e);
       }finally{
           if(documents != null){
               documents.release();
           }
       }
   }
   ```

   

   由于以上指定物料仓库没有库存，所以添加单据不会成功，经过多次测试

   1. 如果在添加失败处将`System.gc()`方法注释，将`company.disconnect()`，`company.release()`注释打开，那么基本上运行60次左右会报异常；
   2. 如果在添加失败处不加`System.gc()`方法，那么基本上在运行700次左右后发生了异常。

   根据以上测试，尽管每次使用SBO对象后，都调用了release(),运行一定时间后仍然会报异常。猜测

   1. 当DI频繁断开/连接公司（company.connect()/company.disconnect()）时
   2. DI同步发生错误时候，即errocode <> 0时

   SBOJNI.dll中内存回收有bug（未继续分析，未证实，也有可能是此JDK版本对此内存回收有bug），很容易导致堆外内存不足。



### 建议

System.gc()不建议频繁使用，尤其是对外提供了API的应用，频繁GC会导致服务***卡顿***。

1. 对于单账套同步，使用静态变量存储Company对象即可；
2. 对于多账套同步，每次同步完后不需要断开连接，只需要在每次使用前判断下当前company对象所连接的公司DB与要连接的公司DB是否一致，如若不一致再进行新的连接，同时调用`System.gc()`；
3. 对外提供的API中直接同步的，千万不要忽略字段检查，在最外层排除错误的信息，尽量不要到了DI层面才判断；同时，如果DI同步失败了，显示调用`System.gc()`。

### 参考

[Java JNI实现原理初探](https://blog.csdn.net/hackooo/article/details/48395765/)