**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/modules/network-acl-inbound/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# Network ACL Inbound Terraform Module

This Terraform Module launches is a simple helper for adding inbound rules to a [Network
ACL](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html). Network ACLs can be a bit tricky to work
with because they are stateless, which means that opening an inbound port is often not enough; you also need to open
[ephemeral outbound ports](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html#VPC_ACLs_Ephemeral_Ports)
which your services use to respond. This can be very easy to forget, so this module adds not only the inbound ports to
an ACL, but also the ephemeral outbound ports for return traffic.

See the [network-acl-outbound](../network-acl-outbound) module for the analogous version of this module, but for opening
outbound ports.

## How do you use this module?

Check out the [vpc-app-network-acls](/modules/vpc-app-network-acls) and
[vpc-mgmt-network-acls](/modules/vpc-mgmt-network-acls) modules for examples.

Check out [vars.tf](vars.tf) for all the configuration options available.

## What's a Network ACL?

[Network ACLs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) provide an extra layer of network
security, similar to a [security group](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html).
Whereas a security group controls what inbound and outbound traffic is allowed for a specific resource (e.g. a single
EC2 instance), a network ACL controls what inbound and outbound traffic is allowed for an entire subnet.

