# Challenge
*Preparation*
- Laptop/PC with Linux installed, booted & logged in.
- Comes preinstalled with docker.io, docker-compose, libvirt, virt-manager
- User has sudo access (in case something is missing) and is in the "docker" group.
- "demo" network is pre-defined ("docker network create demo")
 
Tasks:
1. Start a webserver (e.g. nginx) in a Docker container and let it output a simple "hello world" HTML in Port 8080. Use the (pre-defined) "demo" network.
2. Build a Docker image (use any base image you like) that downloads and displays the HTML.
3. Setup and start a VM (any OS) that you download from the internet (could be done via virt-manager).

# Solution
## Tasks
- [x] Provision
- [x] Setup
- [] Deploy
    - [x] task 1
    - [x] task 2
    - [] task 3

## Prerequisites
- All deployment will be run on the default VPC. If you wish to build a new VPC you can use the repo below
    - https://github.com/muge13/ansible-provision-aws-vpc
- You have ansible installed
- You have your AWS ec2 key pair, access_key and Secret key
- You have a RHEL 7 instance provisioned on AWS. You can use the repos below to provision the instance(s).
    - https://github.com/muge13/ansible-provision-aws-ec2

## Guide
### Base
- On your system set your AWS credentials on your environment
```
export AWS_ACCESS_KEY_ID="" &&  export AWS_SECRET_ACCESS_KEY="" && export AWS_REGION=""
```
### Setup
- Modify the variables on the `Setup.yml` file to be in line with those in your environment, you can also override them on the command.
- configure the Instance
```
ansible-playbook  -u ec2-user  -i ec2.py  -T 60 -f 100 --private-key=~/{keypair}.pem Setup.yml
```
### Deploy
- Modify the variables on the `Deploy.yml` file to be in line with those in your environment, you can also override them on the command.
- Deploy the service
```
ansible-playbook  -u ec2-user  -i ec2.py  -T 60 -f 100 --private-key=~/{keypair}.pem Deploy.yml
```

- **Task 1**: This exposes a hello world page hosted on an nginx base image, reachable on port 8080.
- **Task 2**: This acts as a proxy fetching requests from the initial nginx hello-world container
- **Task 3**: 
- P.S. : You should allow the working ports on the AWS security Group to allow for access
    - 8080
    - 80
    - 22


