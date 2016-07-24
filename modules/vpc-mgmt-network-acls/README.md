**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/modules/vpc-mgmt-network-acls/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC-Mgmt Network ACLs Terraform Module

This Terraform Module adds a default set of [Network
ACLs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) to a VPC created using the
[vpc-mgmt](../vpc-mgmt) module. The ACLs enforce the following security settings  (based on [A Reference VPC
Architecture](https://www.whaletech.co/2014/10/02/reference-vpc-architecture.html)):

- **Public subnet**: Allow all requests.
- **Private subnet**: Allow all requests to/from the public subnets. Allow all outbound TCP requests plus return traffic
  from any IP for those TCP requests on [ephemeral
  ports](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html#VPC_ACLs_Ephemeral_Ports).

## How do you use this module?

Check out the [vpc-network-acls example](/examples/vpc-network-acls).

Check out [vars.tf](vars.tf) for all the configuration options available.

## What's a VPC?

A [VPC](https://aws.amazon.com/vpc/) or Virtual Private Cloud is a logically isolated section of your AWS cloud. Each
VPC defines a virtual network within which you run your AWS resources, as well as rules for what can go in and out of
that network. This includes subnets, route tables that tell those subnets how to route inbound and outbound traffic,
security groups, firewalls for the subnet (known as "Network ACLs"), and any other network components such as VPN connections.

## What's a Network ACL?

[Network ACLs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) provide an extra layer of network
security, similar to a [security group](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html).
Whereas a security group controls what inbound and outbound traffic is allowed for a specific resource (e.g. a single
EC2 instance), a network ACL controls what inbound and outbound traffic is allowed for an entire subnet.

