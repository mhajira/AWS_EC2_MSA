# AWS_EC2_MSA
This project explains the steps to host AWS EC2 instances for MSA application

Changes to Microservice in Java code before deployment in EC2:
1. Update the Discovery-server URL in each service
2. Override eurekaInstanceConfig method with the below code. This step helps EC2 to establish the DNS appropriately:

 @Bean
   @Autowired
   public EurekaInstanceConfigBean eurekaInstanceConfig(InetUtils inetUtils) {
    EurekaInstanceConfigBean config = new EurekaInstanceConfigBean(inetUtils);
    AmazonInfo info = AmazonInfo.Builder.newBuilder().autoBuild("eureka");
    config.setHostname(info.get(AmazonInfo.MetaDataKey.publicHostname));
    config.setIpAddress(info.get(AmazonInfo.MetaDataKey.publicIpv4));
    config.setNonSecurePort(appPort);
    config.setDataCenterInfo(info);
    return config;
   }   
   
Login to AWS account
Create ECC2 instance for each microservice
Steps to create EC2 instance:
1. Select EC2 linux server
2. choose Storage size, for free tier the eligible max size is 30 GB. This is just storage and RAM eligible for free teir is 1GB
3. Configure security group, choose an existing security group or create one. Add http, All traffic access. Make sure all traffic type configured
4. Create a key pair with a name, download and have it securely for putty connection, later the file cannot be downloaded. Also note that this key value pair can be reused for other EC2 instances.

5. Download and install PuTTY from the PuTTY download page
6. Download and install winscp from https://winscp.net/eng/download.php - one time installation
7. Follow the steps in the link to create private using puttygen and connect instance via putty. And also to connect with WINSCP to    transfer files to Amazon Ec2 instance https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html?icmpid=docs_ec2_console - one time generation
8. Install Java in putty:
9. Issue following commands in Putty terminal to install java
   sudo su
   yum update -y
   yum install -y java-1.8.0-openjdk.x86_64
10. Move Jar file to AWS instance using Winscp
11. run "nohup java -jar <<jar name>>.jar > nohupse.out 2> nohupse.err &"


