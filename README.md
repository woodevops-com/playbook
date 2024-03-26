# playbook

## Cloudwatch

### Get the agent
#### AMD 64
```
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
```
#### ARM 64
```
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/arm64/latest/amazon-cloudwatch-agent.deb
```
### Install the agent
```
sudo apt install ./amazon-cloudwatch-agent.deb
sudo vim /opt/aws/amazon-cloudwatch-agent/bin/config.json
```
### Configure
```
sudo vim /opt/aws/amazon-cloudwatch-agent/bin/config.json
```
```json
{
        "agent": {
                "metrics_collection_interval": 60,
                "region": "me-south-1",
                "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
                "run_as_user": "root"
        },
        "metrics": {
                "aggregation_dimensions": [
                        ["InstanceId"]
                ],
                "append_dimensions": {
                        "ImageId": "${aws:ImageId}",
                        "InstanceId": "${aws:InstanceId}",
                        "InstanceType": "${aws:InstanceType}"
                },
                "metrics_collected": {
                        "disk": {
                                "measurement": ["used_percent"],
                                "metrics_collection_interval": 60,
                                "resources": ["/"]
                        },
                        "mem": {
                                "measurement": ["mem_used_percent"],
                                "metrics_collection_interval": 60
                        }
                }
        }
}
```

### Start Agent
```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

### Chec status
```
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```
