NSX Tier Builder
================

In a nutshell, the script builds switches, routers, and edges for VMware NSX application tiers. The script assumes that you've already deployed NSX and configured the cluster, segments, transport zone, and VXLAN tunnels.

Note: Use at your own risk. I've included a fair amount of error checking and status messages.

---
#### Create-NSXTier
1. Define your specific needs in the json file (don't change the format).
2. Edit the script with your vCenter, NSX, and json file path.
3. Run script, watch your environment come to life. Much like a chia pet. Sort of.

---
#### Remove-NSXtier
1. Define your specific needs in the json file (don't change the format).
2. Edit the script with your NSX and json file path. The script is really just looking for the names of your switches, router, and edge so it can delete them.
3. Run script. It will parse the json to determine which edge, router, and switches to remove (in that order).
