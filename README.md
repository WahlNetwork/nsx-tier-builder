NSX Tier Builder
================

In a nutshell, the script builds switches, a router, and an edge for your VMware NSX application tiers (prod, test, dev, spongebob, etc). The script assumes that you've already deployed NSX and configured the cluster, segments, transport zone, and VXLAN tunnels.

All of the configuration for the application tier is held within a json file. You could, for example, create a json file for each tier for easy up/down provisioning, let Puppet swap in/out json files based on various modules, or just fork this and do whatever floats your boat. I've included some examples in this repo so you know how I'm parsing the configuration.

If you have an improvement idea or want to add more functionality to the config, shoot me a pull request.

**Note: Use at your own risk.** I've included a fair amount of error checking and status messages.
