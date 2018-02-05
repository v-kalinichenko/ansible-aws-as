# EC2 Auto Scaling with Ansible
Ansible playbook that creates an auto scaling infastructure in aws. 

Before running put your ami keys in `amiKeys.yml` replacing the current info:

    ---

    ami_access: "XXXXXXXXXXXXXXXXXXXXXXX"
    ami_secret: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

Then encrypt it with:

    ansible-vault encrypt amiKeys.yml
    
Then replace the info in `group_vars/all.yml` with the info of your aws project:


```yaml
---
# group_vars/all.yml

region: us-east-1
zone: us-east-1a
keypair: YOUR_KEYPAIR
security_groups: YOUR_SECURITY_GROUP
instance_type: t2.micro
volumes:
  - device_name: /dev/sda1
    device_type: gp2
    volume_size: 10
    delete_on_termination: true
```

```yaml
---



Run the playbook with ` ansible-playbook deploy.yml --sudo --key-file=aws-key.pem --ask-vault-pass -vv` and a new instance will be launched. 
