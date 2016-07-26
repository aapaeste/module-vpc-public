**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC Modules

This repo contains modules for creating best-practices Virtual Private Clouds (VPCs) on AWS.

#### Main Modules

The two main modules are:

* [vpc-app](/modules/vpc-app): Launch a VPC meant to house applications. The VPC includes 3 "tiers" of subnets
  (public, private app, private persistence), routing rules, security groups, network ACLs, and NAT gateways.
* [vpc-mgmt](/modules/vpc-mgmt): Launch a VPC meant to house DevOps and other management services. The VPC includes
  2 "tiers" of subnets (public, private), routing rules, security groups, network ACLs, and NAT gateways.

#### Supporting Modules

There are also several supporting modules that add extra functionality on top of vpc-app and vpc-mgmt:

* [vpc-peering](/modules/vpc-peering): Creating peering connections between VPCs. Normally, VPCs are completely
  isolated from each other, but sometimes, you want to allow traffic to flow between them, such as allowing DevOps
  tools running in a Mgmt VPC to talk to apps in a Stage or Prod VPC. This module can create peering connections and
  route table entries that make this sort of cross-VPC communication possible.
* [vpc-app-network-acls](/modules/vpc-app-network-acls): Add a default set of Network ACLs to a VPC created using the
  [vpc-app](/modules/vpc-app) module that control what inbound and outbound network traffic is allowed in each subnet
  of that VPC.
* [vpc-mgmt-network-acls](/modules/vpc-mgmt-network-acls): Add a default set of Network ACLs to a VPC created using the
  [vpc-mgmt](/modules/vpc-mgmt) module that control what inbound and outbound network traffic is allowed in each subnet
  of that VPC.
* [network-acl-inbound](/modules/network-acl-inbound): A simple helper for adding inbound rules to a Network ACL, along
  with the corresponding outbound rules for return traffic .
* [network-acl-outbound](/modules/network-acl-outbound): A simple helper for adding outbound rules to a Network ACL,
  along with the correspoding inbound rules for return traffic.

Click on each module above to see its documentation. Head over to the [examples folder](/examples) for examples.

## What is a module?

At [Gruntwork](http://www.gruntwork.io), we've taken the thousands of hours we spent building infrastructure on AWS and
condensed all that experience and code into pre-built **packages** or **modules**. Each module is a battle-tested,
best-practices definition of a piece of infrastructure, such as a VPC, ECS cluster, or an Auto Scaling Group. Modules
are versioned using [Semantic Versioning](http://semver.org/) to allow Gruntwork clients to keep up to date with the
latest infrastructure best practices in a systematic way.

## How do you use a module?

Most of our modules contain either:

1. [Terraform](https://www.terraform.io/) code
1. Scripts & binaries

#### Using a Terraform Module

To use a module in your Terraform templates, create a `module` resource and set its `source` field to the Git URL of
this repo. You should also set the `ref` parameter so you're fixed to a specific version of this repo, as the `master`
branch may have backwards incompatible changes (see [module
sources](https://www.terraform.io/docs/modules/sources.html)).

For example, to use `v1.0.8` of the vpc-app module, you would add the following:

```hcl
module "ecs_cluster" {
  source = "git::git@github.com:gruntwork-io/module-vpc.git//modules/vpc-app?ref=v1.0.8"

  // set the parameters for the vpc-app module
}
```

*Note: the double slash (`//`) is intentional and required. It's part of Terraform's Git syntax (see [module
sources](https://www.terraform.io/docs/modules/sources.html)).*

See the module's documentation and `vars.tf` file for all the parameters you can set. Run `terraform get -update` to
pull the latest version of this module from this repo before runnin gthe standard  `terraform plan` and
`terraform apply` commands.

#### Using scripts & binaries

You can install the scripts and binaries in the `modules` folder of any repo using the [Gruntwork
Installer](https://github.com/gruntwork-io/gruntwork-installer). For example, if the scripts you want to install are
in the `modules/ecs-scripts` folder of the https://github.com/gruntwork-io/module-ecs repo, you could install them
as follows:

```bash
gruntwork-install --module-name "ecs-scripts" --repo "https://github.com/gruntwork-io/module-ecs" --tag "0.0.1"
```

See the docs for each script & binary for detailed instructions on how to use them.

## What's a VPC?

A [VPC](https://aws.amazon.com/vpc/) or Virtual Private Cloud is a logically isolated section of your AWS cloud. Each
VPC defines a virtual network within which you run your AWS resources, as well as rules for what can go in and out of
that network. This includes subnets, route tables that tell those subnets how to route inbound and outbound traffic,
security groups, firewalls for the subnet (known as "Network ACLs"), and any other network components such as VPN connections.

#### Learn More about VPCs

See the READMEs for the [vpc-app](/modules/vpc-app) and [vpc-mgmt](/modules/vpc-mgmt) modules for detailed info on a VPC,
along with best practices.

## Developing a module

#### Versioning

We are following the principles of [Semantic Versioning](http://semver.org/). During initial development, the major
version is to 0 (e.g., `0.x.y`), which indicates the code does not yet have a stable API. Once we hit `1.0.0`, we will
follow these rules:

1. Increment the patch version for backwards-compatible bug fixes (e.g., `v1.0.8 -> v1.0.9`).
2. Increment the minor version for new features that are backwards-compatible (e.g., `v1.0.8 -> 1.1.0`).
3. Increment the major version for any backwards-incompatible changes (e.g. `1.0.8 -> 2.0.0`).

The version is defined using Git tags.  Use GitHub to create a release, which will have the effect of adding a git tag.

#### Tests

See the [test](/test) folder for details.

## License

Please see [LICENSE.txt](/LICENSE.txt) for details on how the code in this repo is licensed.