# 采用nexus搭建maven仓库

## 安装



## 添加仓库

### 仓库类型



### repository-public配置


## 仓库上传

添加mvn配置
```xml
    <server>
      <id>edi.base</id>
      <username>admin</username>
      <password>admin</password>
    </server>
```

### 1、本地jar包上传

+ 上传mvn语法
```mvn
mvn deploy:deploy-file -DgroupId=xxx -DartifactId=xxx -Dversion=xxx -Dpackaging=jar -Dfile=xxx -Durl=http://xxx:8081/repository/3rdParty/ -DrepositoryId=xxx
```

+ 示例
```mvn
 mvn deploy:deploy-file -DgroupId=com.sap.jco -DartifactId=sapwrapper -Dversion=9.2 -Dpackaging=jar -Dfile=C:\\Users\\Admin\\Desktop\\sbo\\9.2\\sbowrapper.jar -Durl=http://47.92.196.35:8081/repository/maven-third-party/ -DrepositoryId=edi.base
```
***注意：DrepositoryId参数的值与配置文件中的server下的id值保持一致***


### 2、本地项目上传

+ 项目pom中添加配置

```maven
    <distributionManagement>
        <repository>
            <id>nexus-releases</id>
            <url>http://localhost:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>nexus-snapshots</id>
            <url>http://localhost:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
```

+ mvn编译并上传

```maven
    mvn clean deploy
```
