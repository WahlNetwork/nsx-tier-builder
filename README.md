NSX Tier Builder
================

In a nutshell, the script builds switches, a router, and an edge for your VMware NSX application tiers (prod, test, dev, spongebob, etc). The script assumes that you've already deployed NSX and configured the cluster, segments, transport zone, and VXLAN tunnels.

![Create-NSXTier](https://github.com/WahlNetwork/nsx-tier-builder/blob/screenshots/create-nsx-tier.jpg)

All of the configuration for the application tier is held within a [json file](https://github.com/WahlNetwork/nsx-tier-builder/blob/master/prod-tier.json). You could, for example, create a json file for each tier for easy up/down provisioning, let Puppet swap in/out json files based on various modules, or just fork this and do whatever floats your boat. I've included some examples in this repo so you know how I'm parsing the configuration.

If you have an improvement idea or want to add more functionality to the config, shoot me a pull request.

**Note: Use at your own risk.** I've included a fair amount of error checking and status messages.

### Architecture

Here's roughly how this works:

**Logical Switches > Logical Router > Transit Switch > Edge Gateway > Physical**

1. Your logical switches are created, including a transit switch (required).
2. A logical router is created as the gateway for all of your logical switches (using the IP you assigned on each switch). After it is online, the routing config is applied. The router will advertise its directly connected routes via OSPF to the northbound transit switch where the edge lives.
3. A logical edge gateway is created for physical connectivity and to provide services. It has a southbound (internal) interface on the transit network which talks to the router, and a northbound (uplink) interface on a VDS port group. After it is online, the routing config is applied. The edge will advertise OSPF southbound by default, but requires you to manually or programmatically configure northbound OSPF (this seemed too dangerous for a generic config).
