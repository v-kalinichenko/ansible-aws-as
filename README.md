# EC2 Auto Scaling with Ansible
Ansible playbook that creates an auto scaling infastructure in aws. 

## Installing Ansible on target instances

Ansible can be installed as part of the bootstrapping of the instance or with Run Command. The following is some reference information you can use to install Ansible on different Linux distributions:

### Amazon Linux
For Amazon Linux Ansible can be installed using pip. You can use the following command.
```yaml
sudo pip install ansible
```
### Ubuntu
For Ubuntu you can install Ansible using the default package manager. Use this command.
```yaml
sudo apt-get install ansible -y
```
### RedHat 7
For RedHat 7 you can install Ansible by enabling the epel repo. Use the following commands:
```yaml
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum -y install ansible
```

## Setting up Ansible    


Before running put your ami keys in `amiKeys.yml` replacing the current info:

    ami_access: "YOUR_AWS_API_KEY"
    ami_secret: "YOUR_AWS_API_SECRET_KEY"

Then encrypt it with:

    ansible-vault encrypt amiKeys.yml

To get started with dynamic inventory management, you’ll need to grab the EC2.py script and the EC2.ini config file.
The EC2.py script is written using the Boto EC2 library and will query AWS for our running Amazon EC2 instances. The EC2.ini file is the config file for EC2.py, and can be used to limit the scope of Ansible's reach. We can specify the regions, instance tags, or roles that the EC2.py script will find, so we'll grab it from GitHub.
Place [ec2.py](https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.py) and [ec2.ini](https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.ini) into `/etc/ansible/inventory` (creating that directory if absent)
* Linked from this Ansible documentation: http://docs.ansible.com/ansible/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script

Modify `/etc/ansible/ansible.cfg` to use that directory as the inventory source:

```ini
# /etc/ansible/ansible.cfg
inventory = /etc/ansible/inventory
```
    
Then replace the info in `group_vars/all.yml` with the info of your aws project:


```yaml
# group_vars/all.yml

#your keypair
keypair: YOUR_KEYPAIR
#your security group
security_groups: YOUR_SECURITY_GROUP
#the subnet you want to use, found in the vpc menu
subnetID: YOUR_SUBNET_ID
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



Run the playbook with ` ansible-playbook deploy.yml --key-file=aws-key.pem --ask-vault-pass -e group_name= -vv`
