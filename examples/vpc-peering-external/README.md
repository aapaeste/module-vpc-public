**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/examples/vpc-peering-external/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC Peering External Example

This example shows how you can configure the routing and network ACLs for a VPC Peering connection to an external VPC
managed by a 3rd party (e.g. a SaaS service) using the [vpc-peering-external module](/modules/vpc-peering-external). In
the real world, a 3rd party would create their VPC and send you a VPC Peering Connection request, but to make this
example self-contained and testable, we are creating:

* An "internal" VPC which represents a VPC you own (e.g. stage or prod)
* An "external" VPC which represents a VPC the 3rd party owns
* The VPC Peering Connection
* The route tables and network ACLs that make the VPC Peering Connection work
* An EC2 Instance that runs in a public sunbet of the external VPC that represents a 3rd party SaaS product you may
  want to connect to
* Sample EC2 Instances that run in each subnet of the internal VPC that represent your servers

## Quick start

To try these templates out you must have Terraform installed (minimum version: `0.6.11`):

1. Open `vars.tf`, set the environment variables specified at the top of the file, and fill in any other variables that
   don't have a default.
1. Run `terraform get`.
1. Run `terraform plan`.
1. If the plan looks good, run `terraform apply`.

## Core concepts

For info on how VPC Peering works, the normal flow for establishing a VPC Peering Connection with a 3rd party, and the
Network ACLs enforced in these templates, see the [vpc-peering-external module docs](/modules/vpc-peering-external).