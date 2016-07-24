**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/examples/vpc-network-acls/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC Network ACLs Example

This example creates an App VPC and a Mgmt VPC and shows how to use the
[vpc-app-network-acls](/modules/vpc-app-network-acls) and [vpc-mgmt-network-acls](/modules/vpc-mgmt-network-acls)
modules to add Network ACLs that control what inbound and outbound network traffic is allowed in each subnet
of those VPCs. We also launch a number of EC2 instances in each VPC to test the ACLs.

## Quick start

To try these templates out you must have Terraform installed (minimum version: `0.6.11`):

1. Open `vars.tf`, set the environment variables specified at the top of the file, and fill in any other variables that
   don't have a default.
1. Run `terraform get`.
1. Run `terraform plan`.
1. If the plan looks good, run `terraform apply`.

## What's a Network ACL?

[Network ACLs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) provide an extra layer of network
security, similar to a [security group](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html).
Whereas a security group controls what inbound and outbound traffic is allowed for a specific resource (e.g. a single
EC2 instance), a network ACL controls what inbound and outbound traffic is allowed for an entire subnet.

## Our VPC Structure

To keep this example simple, we create just two VPCs. For a real-world use-case, we recommend following the[Reference
VPC Architecture](https://www.whaletech.co/2014/10/02/reference-vpc-architecture.html) and creating three VPCs:

- **Prod VPC**: For production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app) module,
  as shown in the [vpc-app](../vpc-app) example.
- **Stage VPC**: For non-production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app)
  module, as shown in the [vpc-app](../vpc-app) example.
- **Mgmt VPC**: Where operators run DevOps tooling and login. You can create this type of VPC using the
  [vpc-mgmt](/modules/vpc-mgmt) module, as shown in the templates in the [vpc-mgmt](../vpc-mgmt) example.

## VPC Isolation

Each VPC is completely isolated from the other, so if you connect to one VPC, there is no way to access another VPC.
This is part of the "defense-in-depth" philosophy: even if attackers breaches one level of our security, they still
have other problems to deal with at the next level. It's also useful to ensure that changes you make in one VPC don't
accidentally cause problems in another. However, it can be useful to permit limited, controlled access between VPCs,
such as allowing a DevOps tool in the Mgmt VPC to deploy code in the Stage and Prod VPCs. See the
[vpc-peering](../vpc-peering) example to learn how this can be done.

Note that to make testing easier in this example, we create all three VPCs in the same Terraform template, but in
real-world usage you should create each VPC in a separate set of templates so that it gets a separate state file. That
way, you get more isolation, and if you somehow corrupt the state file while testing a change in one VPC, it does
not affect the state file for another VPC.

## Subnets and Network ACLs

Each VPC defines different "tiers" of subnets, or logical subdivisions of the VPC. The [vpc-app](/modules/vpc/vpc-app)
module creates public, private app, and private persistence subnets. The [vpc-mgmt](/modules/vpc/vpc-mgmt) module
creates public and private subnets. By default, the only limitations on those subnets are that public subnets are
accessible from the public Internet, while private subnets are not.

The templates in this example show how we can improve security in these subnets by adding Network ACLs. In particular,
the ACLs ensure that each subnet can only receive and send traffic to "adjacent" subnets (e.g. the private persistence
subnet receive requests from the private app subnet, but not the public subnet) as well as the Mgmt VPC. See the
[vpc-app-network-acls](/modules/vpc-app-network-acls) and [vpc-mgmt-network-acls](/modules/vpc-mgmt-network-acls)
modules for details.

## EC2 instances in this example

This example launches the following resources for demonstration and testing purposes:

1. **App VPC instances**: Launch one EC2 Instance in a public subnet, one in a private app subnet, and one in a
   private persistence subnet.
1. **Mgmt VPC instances**: Launch one EC2 Instance in a public subnet and one in a private subnet.

## SSH access

Any instance launched in a private subnet will not have a public IP address. Therefore, the only way to SSH to an
instance in a private subnet is to first SSH to an instance in a public subnet and use it as a "jump host". Note that
the Network ACLs in the templates in this example prevent any connections in the App VPCs between a public
subnet and a private persistence subnet, unless that public subnet is in the Mgmt VPC. Therefore, the only way to SSH
to an instance in the private persistence subnet is to enable VPC peering as shown in the [vpc-peering
example](../vpc-peering) and to connect via an instance in the public subnet of the Mgmt VPC. See the [Bastion Host
examples](https://github.com/gruntwork-io/module-server-public/tree/master/examples/bastion-host) for more info.

## Known Errors

This terraform template may intermittently trigger certain non-critical errors caused by eventual consistency bugs in
Terraform. These are usually harmless and all you need to do to get around them is to re-run `terraform apply`.
