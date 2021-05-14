
Name
Scalable and elastic AWS Infrastructure for deploying simple nodejs api that creates time stamp in Dynamo DB table.

Description
The project is created using the infrastructure as a code resourse - CloudFormation. The CloduFormation code creates VPC, internet gateway, routes, route tables, network access lists, security groups, bastion host, subnets, auto scaling group, application load balancer, EC2 instances, IAM role and profile as well as Dynamo DB table. The ssh key pair was created manually and used as a parameter within the code.
When deployed cloudformation stack is created and that provisions the infrastructure. The EC2 instances are deployed into public subnet for the sake of this exercise so that the api could be called in the format suggested by requirement which is:
curl -X POST http://<some_ip_address>/app

Deployment
Pull the code from this repository to the host that you may use for deployment. Making sure your environemtn is setup with the right access to aws environments i.e using access keys or role. You may need to change the CidrIp values  used in the security group block shown below to your IP in order to whitelist your IP to only access the webserver via ssh and http.


  VodafoneSGapp:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: AWS Assignment - App server security group
      VpcId: !Ref VodafoneVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 84.70.171.242/32
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 84.70.171.242/32
      Tags:
        - Key: environment
          Value: Vodafone-AWS-Assignment
        - Key: Name
          Value: !Join ["-", [AppServerSecurityGroup, !Ref CandidateName]]



 To deploy,  run the cloudformation cli command:

aws cloudformation create-stack --stack-name <stack_name> --template-body file://vodafone_api_infra.yml  --capabilities 
CAPABILITY_IAM

It will take up to twenty minutes for the entire stack to be deployed.


Usage
Once deployed you will have two auto-scaled instances behind  the load balancer. The load balancer isn't very useful at this point but will be when the application is moved to a private subnet and domain name and DNS bits are done.


Contributing
Project is far from finished. It will eventually need NAT instances or gateway, domain name, hosting and application load balancer to route traffic to the app via NGINX as reverse proxy.


License
Vodafone

