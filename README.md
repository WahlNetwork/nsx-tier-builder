NSX Tier Builder
================

<!-- MarkdownTOC autolink=true bracket=round -->

- [Description](#description)
- [Installation](#installation)
- [Usage Instructions](#usage-instructions)
    - [Build Topology](#build-topology)
    - [Script Parameters](#script-parameters)
    - [Workflow](#workflow)
- [Future](#future)
- [Contribution](#contribution)
- [Licensing](#licensing)
- [Other Cool Projects](#other-cool-projects)

<!-- /MarkdownTOC -->

# Description

This project was created to quickly stand up and tear down virtual networking environments with VMware NSX. It's common for a network environment to be a tier of an application - such as production, stage, test, or development - and thus I've named this the NSX Tier Builder! Here are some common use cases:

1. Better understanding of the NSX API from a PowerShell coder's perspective.
2. Provision and remove network tiers for any sort of development work.
3. Combine vRealize Orchestration input/outputs with a generic framework and tweak it to your desired state.

# Installation

The code assumes that you've already deployed the underlying infrastructure and software dependencies to run NSX - this includes the manager, controllers, VXLAN, segment range, and transport zone. The entire configuration for your application tier is held within a single JSON file, which is used for both creating and removing the virtual network.

![Create-NSXTier](https://github.com/WahlNetwork/nsx-tier-builder/blob/screenshots/create-nsx-tier.jpg)

To get started, you'll need:

1. PowerShell version 3 or higher (I recommend version 4)
2. PowerCLI version 5.8 or higher

Grab the guts of this repo - mainly just the two PS1 scripts and the sample .JSON file - and run them on a computer with the previously mentioned software installed. Please note:

* If you plan to run from a remote location, you'll have to use Set-ExecutionPolicy bypass because I have not signed the scripts.
* If you plan to run from a local location, you can use Set-ExecutionPolicy RemoteSigned or bypass

# Usage Instructions

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

## Workflow

The script follows a logical progression in order to gracefully build (and remove) the virtual network.

1. Your logical switches are created, including a transit switch (required).
2. A logical router is created as the gateway for all of your logical switches (using the IP you assigned on each switch). After it is online, the routing config is applied. The router will advertise its directly connected routes via OSPF to the northbound transit switch where the edge lives.
3. A logical edge gateway is created for physical connectivity and to provide services. It has a southbound (internal) interface on the transit network which talks to the router, and a northbound (uplink) interface on a VDS port group. After it is online, the routing config is applied. The edge will advertise OSPF northbound and southbound if it was configured.

# Future

The ideal roadmap for this project is to:

1. Use a friendly GUI wrapper to import/export data for the JSON config. Hand editing the file isn't much fun.
2. Make the various calls with the script modular and functions, instead of one serialized script.

# Contribution

Create a fork of the project into your own reposity. Make all your necessary changes and create a pull request with a description on what was added or removed and details explaining the changes in lines of code. If approved, project owners will merge it.

# Licensing

Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

# Other Cool Projects

* vtagion has a [neat project](https://github.com/vtagion/VMware-Products/blob/master/NSX%20Deploy.ps1) to deploy and link NSX Manager to vCenter Server.
