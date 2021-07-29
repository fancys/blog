```shell
vim zk.sh

#!/bin/sh
echo "start zookeeper server..."
if(($#==0));then
echo "no params";
exit;
fi

hosts="linux1 linux2 linux3"

for host in $hosts
do
ssh $host "source /etc/profile; /opt/zookeeper/bin/zkServer.sh $1"
done
```

