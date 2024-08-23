
# Ansible Project 2: Automating Nginx Installation and Static Webpage Deployment on AWS EC2

This project demonstrates how to automate the installation of Nginx and the deployment of a static webpage on AWS EC2 instances using Ansible.

## Prerequisites

- AWS Account
- Ansible installed on a local machine or an AWS EC2 instance

## Project Overview

1. **Provision Ansible Controller and Hosts:**
   - Create an AWS EC2 instance named `ansible-controller`.
   - Download the SSH key pair `ansible-key` (RSA, `.pem` format).
   - Create two additional EC2 instances named `ansible-host1` and `ansible-host2`.
   - Use the same SSH key pair `ansible-key` for both instances and ensure they share the same security group.

2. **Convert SSH Key for PuTTY:**
   - Convert the `ansible-key.pem` file to `ansible-key.ppk` using PuTTYgen if you are using PuTTY for SSH access.

3. **Configure Ansible Controller:**
   - Copy the SSH key to the `ansible-controller` using the following command:
     ```bash
     scp -i "ansible-key.ppk" ansible-key.pem ubuntu@aws:/home/ubuntu
     ```
   - Adjust file permissions to remove inherited permissions and grant read access to the current user:
     ```bash
     icacls "ansible-key.pem" /inheritance:r
     icacls "ansible-key.pem" /grant:r "$($env:USERNAME):(R)"
     ```
   - SSH into the `ansible-controller`.

4. **Install Ansible on the Controller:**
   - Update the package list and install Ansible with the following commands:
     ```bash
     sudo apt update -y
     sudo apt install software-properties-common
     sudo add-apt-repository --yes --update ppa:ansible/ansible
     sudo apt install ansible -y
     sudo apt update -y
     ```

5. **Set Up Inventory:**
   - Create a `hosts` file on the `ansible-controller` to define your EC2 instances.

6. **Test SSH Connectivity:**
   - Test SSH connectivity between the controller and hosts using the ping module:
     ```bash
     ansible -i hosts all -m ping
     ```

7. **Deploy Nginx:**
   - Create a playbook named `ansible-install-nginx-ec2.yml` to automate the Nginx installation and static webpage deployment.
   - Run the playbook on all hosts:
     ```bash
     ansible-playbook -i hosts install-nginx-aws.yml
     ```
   - Alternatively, run the playbook on specific hosts:
     ```bash
     ansible-playbook -i hosts -l db install-nginx-aws.yml
     ```

## Conclusion

This project provides a streamlined approach to automating web server setup and deployment tasks on AWS using Ansible, reducing manual intervention and ensuring consistent configurations across multiple servers.
