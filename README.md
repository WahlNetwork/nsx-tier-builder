NSX Tier Builder
================

<!-- MarkdownTOC autolink=true bracket=round -->

- [Introduction](#introduction)
- [Configuration](#configuration)
	- [Build Topology](#build-topology)
	- [Script Parameters](#script-parameters)
- [Workflow](#workflow)
- [Other Cool Projects](#other-cool-projects)

<!-- /MarkdownTOC -->

# Introduction

This project was created to quickly stand up and tear down virtual networking environments, which I refer to as application tiers, for VMware NSX. It assumes that you've already deployed the underlying infrastructure and software dependencies to run NSX - this includes the manager, controllers, VXLAN, segment range, and transport zone. The entire configuration for your application tier is held within a single JSON file, which is used for both creating and removing the virtual network.

![Create-NSXTier](https://github.com/WahlNetwork/nsx-tier-builder/blob/screenshots/create-nsx-tier.jpg)

# Configuration

Once you've grabbed the bits from this folder, start by editing the [JSON file](https://github.com/WahlNetwork/nsx-tier-builder/blob/master/prod-tier.json). I've provided an example within this repository.

Next, run the Create-NSXTier script to build an environment, or the Remove-NSXTier environment to remove an environment. You'll reference the same JSON file for both activities, which keeps things simple.

## Build Topology

The topology is as such:

Logical Switch(es) <--> Distributed Logical Router (router) <-OSPF-> Transport Switch <-OSPF-> Edge Services Gateway (edge) <-->

If you have an improvement idea or want to add more functionality to the config, shoot me a pull request. I will be adding more features to the JSON file as time progresses.

## Script Parameters

These are the parameters available within the scripts.

1. **NSX** = NSX Manager IP or FQDN
2. **NSXPassword** = NSX Manager credentials with administrative authority
5. **NSXUsername** (*optional*) = NSX Manager username. Defaults to "admin" if no value is specified.
3. **JSONPath** = Absolute path to your JSON configuration file
4. **vCenter** = vCenter Server IP or FQDN. Only required for the Create script.
6. **NoAskCreds** (*optional*) = Switch that informs the script to use your current Windows creds for vCenter login. Only available in the Create script.

# Workflow

The script follows a logical progression in order to gracefully build (and remove) the virtual network.

1. Your logical switches are created, including a transit switch (required).
2. A logical router is created as the gateway for all of your logical switches (using the IP you assigned on each switch). After it is online, the routing config is applied. The router will advertise its directly connected routes via OSPF to the northbound transit switch where the edge lives.
3. A logical edge gateway is created for physical connectivity and to provide services. It has a southbound (internal) interface on the transit network which talks to the router, and a northbound (uplink) interface on a VDS port group. After it is online, the routing config is applied. The edge will advertise OSPF northbound and southbound if it was configured.

# Other Cool Projects

* vtagion has a [neat project](https://github.com/vtagion/VMware-Products/blob/master/NSX%20Deploy.ps1) to deploy and link NSX Manager to vCenter Server.
