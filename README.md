## Ansible Project 2
Install Nginx and deploy a static webpage on an AWS EC2 using Ansible

### Steps:
1. Create aws ec2 instance ansible-controller

2. Download ssh-key ansible-key (RSA, pem)

3. Create aws ec2 instance ansible-host1, ansible-host2
* Use same ssh-key pair ansible-key
* Use same security group

4. Convert ssh-key ansible-key.pem to ansible-key.ppk using puttygen

5. Copy ssh-key to ansible-controller
* scp -i "ansible-key.ppk" ansible-key.pem ubuntu@aws:/home/ubuntu

6. Remove inherited permissions and grant read permissions to the current user
* icacls "ansible-key.pem" /inheritance:r
* icacls "ansible-key.pem" /grant:r "$($env:USERNAME):(R)"

8. SSH to ansible-controller

9. Install ansible on ansible controller
* sudo apt update -y
* sudo apt install software-properties-common
* sudo add-apt-repository --yes --update ppa:ansible/ansible
* sudo apt install ansible -y
* sudo apt update -y

10. Create hosts file on ansible-controller

11. Ping hosts to test ssh connectivity
* ansible -i hosts all -m ping

12. Create playbook ansible-install-nginx-ec2.yml

13. Run playbook
* ansible-playbook -i hosts install-nginx-aws.yml (all hosts)
* ansible-playbook -i hosts -l db install-nginx-aws.yml (specific hosts)
