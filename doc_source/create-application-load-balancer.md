# Create an Application Load Balancer<a name="create-application-load-balancer"></a>

A load balancer takes requests from clients and distributes them across targets in a target group\.

Before you begin, ensure that you have a virtual private cloud \(VPC\) with at least one public subnet in each of the Availability Zones used by your targets\.

To create a load balancer using the AWS CLI, see [Tutorial: Create an Application Load Balancer Using the AWS CLI](tutorial-application-load-balancer-cli.md)\.

To create a load balancer using the AWS Management Console, complete the following tasks\.

**Topics**
+ [Step 1: Configure a Load Balancer and a Listener](#configure-load-balancer)
+ [Step 2: Configure Security Settings for an HTTPS Listener](#configure-security-settings)
+ [Step 3: Configure a Security Group](#configure-security-group)
+ [Step 4: Configure a Target Group](#configure-target-group)
+ [Step 5: Configure Targets for the Target Group](#select-targets)
+ [Step 6: Create the Load Balancer](#create-load-balancer)

## Step 1: Configure a Load Balancer and a Listener<a name="configure-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener Configuration](load-balancer-listeners.md#listener-configuration)\.

**To configure your load balancer and listener**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. For **Application Load Balancer**, choose **Create**\.

1. For **Name**, type a name for your load balancer\. For example, **my\-alb**\.

1. For **Scheme**, an Internet\-facing load balancer routes requests from clients over the Internet to targets\. An internal load balancer routes requests to targets using private IP addresses\.

1. For **IP address type**, choose **ipv4** if your subnets use IPv4 addresses or **dualstack** if your subnets use both IPv4 and IPv6 addresses\.

1. For **Listeners**, the default is a listener that accepts HTTP traffic on port 80\. You can keep the default listener settings, modify the protocol, or modify the port\. Choose **Add** to add another listener \(for example, an HTTPS listener\)\.

1. For **Availability Zones**, select at least two Availability Zones from your VPC\. If there is one subnet for an Availability Zone, it is selected\. If there is more than one subnet for an Availability Zone, select one of the subnets\. Note that you can select only one subnet per Availability Zone\.

1. Choose **Next: Configure Security Settings**\.

## Step 2: Configure Security Settings for an HTTPS Listener<a name="configure-security-settings"></a>

If you created an HTTPS listener in the previous step, configure the required security settings\. Otherwise, go to the next page in the wizard\.

When you use HTTPS for your load balancer listener, you must deploy an SSL certificate on your load balancer\. The load balancer uses this certificate to terminate the connection and decrypt requests from clients before sending them to the targets\. For more information, see [SSL Certificates](create-https-listener.md#https-listener-certificates)\. You must also specify the security policy that the load balancer uses to negotiate SSL connections with the clients\. For more information, see [Security Policies](create-https-listener.md#describe-ssl-policies)\.

**To configure a certificate and security policy**

1. For **Select default certificate**, do one of the following:
   + If you created or imported a certificate using AWS Certificate Manager, select **Choose a certificate from ACM**, and then select the certificate from **Certificate name**\.
   + If you uploaded a certificate using IAM, select **Choose a certificate from IAM**, and then select the certificate from **Certificate name**\.

1. For **Security policy**, we recommend that you keep the default security policy\.

1. Choose **Next: Configure Security Groups**\.

## Step 3: Configure a Security Group<a name="configure-security-group"></a>

The security group for your load balancer must allow it to communicate with registered targets on both the listener port and the health check port\. The console can create a security group for your load balancer on your behalf with rules that allow this communication\. If you prefer, you can create a security group and select it instead\. For more information, see [Recommended Rules](load-balancer-update-security-groups.md#security-group-recommended-rules)\.

**To configure a security group for your load balancer**

1. Choose **Create a new security group**\.

1. Type a name and description for the security group, or keep the default name and description\. This new security group contains a rule that allows traffic to the port that you selected for your load balancer on the **Configure Load Balancer** page\.

1. Choose **Next: Configure Routing**\.

## Step 4: Configure a Target Group<a name="configure-target-group"></a>

You register targets with a target group\. The target group that you configure in this step is used as the target group in the default listener rule, which forwards requests to the target group\. For more information, see [Target Groups for Your Application Load Balancers](load-balancer-target-groups.md)\.

**To configure your target group**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the target group\.

1. For **Target type**, select **Instance** to register targets by instance ID, **IP** to register IP addresses, and **Lambda function** to register a Lambda function\.

1. \(Optional\) If the target type is **Instance** or **IP**, modify the port and protocol as needed\.

1. \(Optional\) If the target type is **Lambda function**, enable health checks as needed\.

1. For **Health checks**, keep the default health check settings\.

1. Choose **Next: Register Targets**\.

## Step 5: Configure Targets for the Target Group<a name="select-targets"></a>

With an Application Load Balancer, the target type of your target group determines how you register targets with the target group\.

**To register targets by instance ID**

1. For **Instances**, select one or more instances\.

1. Type the instance listener port, and then choose **Add to registered**\.

1. When you have finished registering instances, choose **Next: Review**\.

**To register IP addresses**

1. For each IP address to register, do the following:

   1. For **Network**, if the IP address is from a subnet of the target group VPC, select the VPC\. Otherwise, select **Other private IP address**\.

   1. For **IP**, type the IP address\.

   1. For **Port**, type the port\.

   1. Choose **Add to list**\.

1. When you have finished adding IP addresses to the list, choose **Next: Review**\.

**To register a Lambda function**

1. For **Lambda function**, do one of the following:
   + Select the Lambda function
   + Create a new Lambda function and select it
   + Register the Lambda function after you create the target group

1. Choose **Next: Review**\.

## Step 6: Create the Load Balancer<a name="create-load-balancer"></a>

After creating your load balancer, you can verify that your targets have passed the initial health check and then test that the load balancer is sending traffic to your targets\. When you are finished with your load balancer, you can delete it\. For more information, see [Delete an Application Load Balancer](load-balancer-delete.md)\.

**To create the load balancer**

1. On the **Review** page, choose **Create**\.

1. After the load balancer is created, choose **Close**\.

1. \(Optional\) To define additional listener rules that forward requests based on a path pattern or a hostname, see [Add a Rule](listener-update-rules.md#add-rule)\.