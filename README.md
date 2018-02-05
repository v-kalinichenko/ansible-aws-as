# EC2 Auto Scaling with Ansible
Ansible playbook that creates an auto scaling infastructure in aws. 

Installing Ansible on target instances

Ansible can be installed as part of the bootstrapping of the instance or with Run Command. The following is some reference information you can use to install Ansible on different Linux distributions:

Amazon Linux
For Amazon Linux Ansible can be installed using pip. You can use the following command.
---
```yaml
---
sudo pip install ansible
``````
---
```
```yaml
---
```
Ubuntu
For Ubuntu you can install Ansible using the default package manager. Use this command.
```yaml
---
sudo apt-get install ansible -y
``````
---
```
```yaml
---
```
RedHat 7
For RedHat 7 you can install Ansible by enabling the epel repo. Use the following commands:
```yaml
---
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum -y install ansible
``````
---
```
```yaml
---
```
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



Run the playbook with ` ansible-playbook deploy.yml --sudo --key-file=aws-key.pem --ask-vault-pass -e group_name= -vv`
