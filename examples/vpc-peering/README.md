**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/examples/vpc-peering/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC Peering Example

This example creates an App VPC and a Mgmt VPC and shows how to use the [vpc-peering](/modules/vpc-peering) module to
create a VPC peering connections between them. We also launch a number of EC2 instances in each VPC to test the
peering connections.

Our goal is to setup the Mgmt VPC so that once you're logged in there, you have access to ALL resources in all VPCs.
But when you're logged into an App VPC, you may have access to the Mgmt VPC, but you do NOT also get access to any other
VPC. That is, VPC peering relationships are not "transitive".

## Quick start

To try these templates out you must have Terraform installed (minimum version: `0.6.11`):

1. Open `vars.tf`, set the environment variables specified at the top of the file, and fill in any other variables that
   don't have a default.
1. Run `terraform get`.
1. Run `terraform plan`.
1. If the plan looks good, run `terraform apply`.

## Our VPC Structure

To keep this example simple, we create just two VPCs. For a real-world use-case, we recommend following the[Reference
VPC Architecture](https://www.whaletech.co/2014/10/02/reference-vpc-architecture.html) and creating three VPCs:

- **Prod VPC**: For production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app) module,
  as shown in the [vpc-app](../vpc-app) example.
- **Stage VPC**: For non-production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app)
  module, as shown in the [vpc-app](../vpc-app) example.
- **Mgmt VPC**: Where operators run DevOps tooling and login. You can create this type of VPC using the
  [vpc-mgmt](/modules/vpc-mgmt) module, as shown in the templates in the [vpc-mgmt](../vpc-mgmt) example.

## VPC Isolation and Peering

Each VPC is completely isolated from the other, so if you connect to one VPC, there is no way to access another VPC.
This is part of the "defense-in-depth" philosophy: even if attackers breaches one level of our security, they still have
other problems to deal with at the next level. It's also useful to ensure that changes you make in one VPC don't
accidentally cause problems in another. However, it can be useful to permit limited, controlled access between VPCs,
such as allowing a DevOps tool in the Mgmt VPC to deploy code in the App VPCs. To enable this, the templates in this
example set up a [VPC Peering connection](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-peering.html)
between the Mgmt VPC and the App VPC.

Note that to make testing easier in this example, we create all the VPCs in the same Terraform template, but in
real-world usage you should create each VPC in a separate set of templates so that it gets a separate state file. That
way, you get more isolation, and if you somehow corrupt the state file while testing a change in one VPC, it does
not affect the state file for another VPC.

## EC2 instances in this example

This example launches the following resources for demonstration and testing purposes:

1. **App VPC instances**: Launch one EC2 Instance in a public subnet, one in a private app subnet, and one in a
   private persistence subnet.
1. **Mgmt VPC instances**: Launch one EC2 Instance in a public subnet and one in a private subnet.

## SSH access

Any instance launched in a private subnet will not have a public IP address. Therefore, the only way to SSH to an
instance in a private subnet is to first SSH to an instance in a public subnet and use it as a "jump host". With VPC
Peering enabled as shown in the templates in this example, you could launch a public host in the Mgmt VPC and use that
to connect to any instance in any of the VPCs. See the [Bastion Host
examples](https://github.com/gruntwork-io/module-server-public/tree/master/examples/bastion-host) for more info.

## Known Errors

This terraform template may intermittently trigger certain non-critical errors caused by eventual consistency bugs in
Terraform. These are usually harmless and all you need to do to get around them is to re-run `terraform apply`.
