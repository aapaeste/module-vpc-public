**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/examples/vpc-app/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# App VPC Example

This shows an example of how to use the [vpc-app](/modules/vpc-app) module to launch a VPC that can be used as a
production or staging environment for your apps.

For more information on the core concepts behind the VPC, see the [vpc-app](/modules/vpc-app) module. For common
usage-patterns with vpc-app, such as running multiple VPCs, launching EC2 instances in a VPC, setting up Network ACLs,
and more, see the [vpc-network-acls](../vpc-network-acls) and [vpc-peering](../vpc-peering) examples.

## Quick start

To try these templates out you must have Terraform installed (minimum version: `0.6.11`):

1. Open `vars.tf`, set the environment variables specified at the top of the file, and fill in any other variables that
   don't have a default.
1. Run `terraform get`.
1. Run `terraform plan`.
1. If the plan looks good, run `terraform apply`.

## Our VPC Structure

Our canonical VPC setup, based on the blog post [A Reference VPC
Architecture](https://www.whaletech.co/2014/10/02/reference-vpc-architecture.html), is to have three VPCs:

- **Prod VPC**: For production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app) module,
  as shown in the templates in this example.
- **Stage VPC**: For non-production workloads. You can create this type of VPC using the [vpc-app](/modules/vpc-app)
  module, as shown in the templates in this example.
- **Mgmt VPC**: Where operators run DevOps tooling and login. You can create this type of VPC using the
  [vpc-mgmt](/modules/vpc-mgmt) module, as shown in the [vpc-mgmt](../vpc-mgmt) example.

## Subnet Tiers

This VPC defines different "tiers" of private and public subnets that strictly control inbound and outbound access.
See [vpc-app](/modules/vpc-app) and [vpc-network-acls](/examples/vpc-network-acls) for details.

## Known Errors

This terraform template may intermittently trigger certain non-critical errors caused by eventual consistency bugs in
Terraform. These are usually harmless and all you need to do to get around them is to re-run `terraform apply`.
