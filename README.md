# Jenkins pipeline to run a quick ZAP scan for a given application URL on AWS floating slaves
1. An AWS account
2. Jenkins:
[Jenkins](https://www.jenkins.io/) is an open source continuous integration server. It builds and tests your software continuously and monitors the execution and status of jobs, making it easier for teams to obtain the latest stable code.
3. ZAP:
[Zed Attack Proxy (ZAP)](https://www.zaproxy.org/getting-started/) is a free, open-source penetration testing tool being maintained under the umbrella of the Open Web Application Security Project (OWASP). ZAP is designed specifically for testing web applications and is both flexible and extensible.

4. Pre-requisites: 
* Create Cloud Configuration on Jenkins Controller:
  - Login to your Jenkins controller through Jenkins URL. Go to Jenkins -> Manage Jenkins -> Manage Nodes and CLouds
  - Select Add a new cloud -> Amazon EC2
  - Enter name and region
  - Configure the AWS credentials(Access key ID and sSecret key) and private key. Test the connection
  ![AWS_Cloud_Connection](https://github.com/vbang247/images/master/aws_jenkins.png?raw=true)
  
* Add an init script to download OWASP-ZAP on slave:
  - Now add the AMI configuration to the Cloud created above.
  - Here we select AMI Amazon Linux AMI 2.0.20200603 x86_64 ECS HVM GP2 which comes with a built in docker installation from AWS market place.
  - Enter the ami id of the AMI: ami-0c20a67db6e1c7258 
  - Add the appropriate security group 
  ![AWS_Cloud_Connection](https://github.com/vbang247/images/master/aws_jenkins_2.png?raw=true)
  - Add an init script that will download OWASP-ZAP on our slave and will be used to run a scan. 
  ```bash
  #!/bin/bash
  yum update -y
  sudo yum install wget -y
  sudo wget -c https://github.com/zaproxy/zaproxy/releases/download/v2.9.0/ZAP_2.9.0_Linux.tar.gz
  tar -xvf *.gz
  ```



