# tungstenfabric-efk-flow-log
vRouter flow log to EFK flow collector


## installation


### ES node

```
CentOS 7:

yum -y install python3
pip3 install docker-compose

yum -y install docker
systemctl start docker
systemctl enable docker

systemctl stop firewalld
systemctl disable firewalld


curl -O https://raw.githubusercontent.com/robcowart/elastiflow/master/docker-compose.yml
sed -i 's!elasticsearch.elasticsearch!elasticsearch/elasticsearch-oss!' docker-compose.yml
sed -i 's!kibana.kibana!kibana/kibana-oss!' docker-compose.yml


mkdir /var/lib/elastiflow_es && chown -R 1000:1000 /var/lib/elastiflow_es
setenforce 0 # FIXME
docker-compose -f docker-compose.yml up -d

Import dashboard from Antrea repo
 https://github.com/vmware-tanzu/antrea/blob/master/build/yamls/elk-flow-collector/kibana.ndjson
to kibana:
 Saved Object > Import

```

### vRouter node


```
To get flow entry in vRouter log,
please set flow-export-rate 100 at Configure > Global Config > edit > Flow Export Rate in the Tungsten Fabric webui,
and add
  SLO_DESTINATION=file
  SAMPLE_DESTINATION=file
to /etc/contrail/common_vrouter.env.


curl -O http://packages.treasuredata.com.s3.amazonaws.com/3/redhat/7/x86_64/td-agent-3.8.0-0.el7.x86_64.rpm


yum localinstall td-agent-3.8.0-0.el7.x86_64.rpm

vi /etc/td-agent/td-agent.conf
(paste fluentd.conf)


systemctl start td-agent
systemctl enable td-agent
```


## Screenshot

![FlowDashboard](https://github.com/tnaganawa/tungstenfabric-efk-flow-log/blob/main/flow-dashboard.png)
![SankeyDiagram](https://github.com/tnaganawa/tungstenfabric-efk-flow-log/blob/main/sankey-diagram.png)
![FlowLog](https://github.com/tnaganawa/tungstenfabric-efk-flow-log/blob/main/flow-log.png)
