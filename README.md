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

#your keypair
keypair: YOUR_KEYPAIR
#your security group
security_groups: YOUR_SECURITY_GROUP
#the subnet you want to use, found in the vpc menu
subnetID: subnet-19096536
#the region of the subnet you are using
region: us-east-1
zone: us-east-1a
instance_type: t2.micro
volumes:
  - device_name: /dev/sda1
    device_type: gp2
    volume_size: 10
    delete_on_termination: true
```

```yaml
---



Run the playbook with ` ansible-playbook deploy.yml --sudo --key-file=aws-key.pem --ask-vault-pass -e group_name="" -vv`
