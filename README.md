# Ansible Apache + Nginx EC2 Lab

This project uses one Ansible control node and two worker nodes in AWS EC2:

- `apache` worker installs Apache
- `nginx` worker installs Nginx

## Project files

- `ansible.cfg` points Ansible to the dynamic inventory and SSH key
- `aws_ec2.yml` discovers running EC2 instances by tag
- `site.yml` runs the Apache and Nginx roles
- `roles/apache` installs and starts Apache
- `roles/nginx` installs and starts Nginx

## EC2 tagging

Tag the worker instances with:

- `Name=apache`
- `Name=nginx`

## Controller setup

On the EC2 master node:

```bash
sudo apt update
sudo apt install -y ansible python3-pip awscli
python3 -m pip install --user boto3 botocore
ansible-galaxy collection install -r requirements.yml
aws configure
chmod 400 /home/ubuntu/mywebserver-key.pem
```

## Run

```bash
cd ~/ansible-project
ansible-inventory -i aws_ec2.yml --graph
ansible tag_apache -m ping
ansible tag_nginx -m ping
ansible-playbook site.yml
```
