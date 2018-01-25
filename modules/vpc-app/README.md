**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-vpc>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-vpc/blob/master/modules/vpc-app/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# VPC-App Terraform Module

This Terraform Module launches a single VPC meant to house applications. By contrast, DevOps-related services such as
Jenkins or InfluxDB should be in a "mgmt" VPC. (See the [vpc-mgmt](../vpc-mgmt) module.)

## How do you use this module?

Check out the [examples folder](/examples).

## What's a VPC?

A [VPC](https://aws.amazon.com/vpc/) or Virtual Private Cloud is a logically isolated section of your AWS cloud. Each
VPC defines a virtual network within which you run your AWS resources, as well as rules for what can go in and out of
that network. This includes subnets, route tables that tell those subnets how to route inbound and outbound traffic,
security groups, firewalls for the subnet (known as "Network ACLs"), and any other network components such as VPN connections.

## Three Subnet Tiers

This VPC defines three "tiers" of subnets:

- **Public Subnets**: Resources in these subnets are directly addressable from the Internet. Only public-facing
  resources (typically just load balancers) should be put here.
- **Private/App Subnets**: Resources in these subnets are NOT directly addressable from the Internet but they can make
  outbound connections to the Internet through a NAT Gateway. You can connect to the resources in this subnet only from
  resources within the VPC, so you should put your app servers here and allow the load balancers in the Public Subnet
  to route traffic to them.
- **Private/Persistence Subnets**: Resources in these subnets are neither directly addressable from the Internet nor
  able to make outbound Internet connections. You can connect to the resources in this subnet only from within the VPC,
  so you should put your databases, cache servers, and other stateful resources here and allow your apps to talk to
  them.

## VPC Architecture

The three-tier VPC is inspired by the VPC Architecture described by Ben Whaley in his blog post [A Reference
VPC Architecture](https://www.whaletech.co/2014/10/02/reference-vpc-architecture.html). That blog post proposed the
following VPC structure:

![VPC Diagram](http://i.imgur.com/KC0OKZL.png)

To summarize:

- Each environment (prod, stage, etc.) is represented by a separate VPC.
- Each VPC has three "tiers" of subnets to allow AWS resources to be publicly addressable, addressable only from the
  public tier, or only from the private/app tier.
- In a given subnet tier, there are usually three or four actual subnets, one for each Availability Zone.
- Therefore, if we created a single VPC in the `us-west-2` region, which has Availability Zones `us-west-2a`,`us-west-2b`,
  and `us-west-2c`, each subnet tier would have three subnets (one per Availability Zone) for a total of 9 subnets in all.
- The only way to reach this VPC is from the public Internet via a publicly exposed sevice, or via the [mgmt VPC](../vpc-mgmt),
  which uses [VPC Peering](../vpc-peering) to make this VPC accessible from the mgmt VPC.
- Philosophically, everything in a VPC should be isolated from all resources in any other VPC. In particular, we want
  to ensure that our stage environment is completely independent from prod. This architecture helps to reinforce that.

## Gotchas

- If the `num_availability_zones` variable in the mgmt VPC and the `num_availability_zones` variable in the app VPC don't match, there are problems with the routes that are created between the two VPCs as part of setting up VPC Peering. If your use case requires different numbers of Availability Zones for each of these VPCs, please let us know and we'll investigate further!

## Other VPC Core Concepts

Learn about [Other VPC Core Concepts](../_docs/vpc-core-concepts.md) like subnets, NAT Gateways, and VPC Endpoints.
