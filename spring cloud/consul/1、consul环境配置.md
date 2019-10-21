# consul环境配置

## install consul

### windows

### centos

下载consul组件

`wget https://releases.hashicorp.com/consul/1.5.3/consul_1.5.3_linux_amd64.zip`

解压consul包

`unzip consul_1.5.3_linux_amd64.zip`

解压完成后，执行：`./consul`

启动consul

`./consul agent -dev -ui -bootstrap-expect=1 -data-dir=/tmp/consul -node=agent-dev -advertise=47.92.196.35 -bind=0.0.0.0 -client=0.0.0.0`

### macos