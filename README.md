NSX Tier Builder
================

In a nutshell, the script builds switches, a router, and an edge for your VMware NSX application tiers (prod, test, dev, spongebob, etc.).

The script assumes that you've already deployed NSX and configured the cluster, segments, transport zone, and VXLAN tunnels. All of the configuration for the application tier is held within the json file. You could, for example, create a json file for each tier for easy up/down provisioning, let Puppet swap in/out json files based on various modules, or just fork this and do whatever floats your boat.

If you have an improvement idea or want to add more functionality to the config, shoot me a pull request.

Note: Use at your own risk. I've included a fair amount of error checking and status messages.

---
#### Create-NSXTier
1. Define your specific needs in the json file.
2. Edit the script with your vCenter, NSX, and json file path.
3. Run script, watch your environment come to life. Much like a chia pet (sort of).

---
#### Remove-NSXTier
1. Define your specific needs in the json file.
2. Edit the script with your NSX and json file path. The script is really just looking for the names of your switches, router, and edge so it can delete them.
3. Run script. It will parse the json to determine which edge, router, and switches to remove (in that order).
