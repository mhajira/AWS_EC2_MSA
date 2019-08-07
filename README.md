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

