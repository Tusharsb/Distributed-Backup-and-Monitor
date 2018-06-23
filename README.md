#### this project is a work in progress. please read on to contribute.. ####

## Problem ##
System Admins are often asked to monitor uptime or take backup of applications built by a develoepr. Now everytime code improvises it is only the developer who knows the latest endpoints that need monitoring or new data locations that need backup. **System Admins often end up monitoring stale endpoints or backup old locations.**

## Solution ##
With Distributed-Backup-and-Monitor developers can now automate monitoring and backup by just updating `backup-and-monitor.yaml` file in their source tree.

## Existing Solution ##
Taking inspiration from [sourcegraph's checkup](https://about.sourcegraph.com/blog/why-we-open-sourced-our-uptime-monitoring-system/), their architecture lets developers keep `checkup.json` where developer mentions all the endpoints that need checked. The health of these endpoints are notified into a storage engine. A status page keeps checking the storage engine and notifies us of any downtime.

## ..but then ##
What however **seems** missing is :
1. Notification of downtime is sent from the checkup script that runs on same server as app being monitored. So if server goes down at the first place (making both app and checkup script instance going down) we would stop getting health alerts in storage engine. We however will be able to see the downtime if we open the Status page but we will fail to receive any downtime notification as it was the checkup script on the app server that went down. We are not sure (yet) on whether this problem exists for real or checkup provides a way around to this.
2. Checkup is meant for uptime monitoring. We need a combined solution that handles backup in similar distributed architecture like checkup. Where developer tells what needs to be backed up and some central backup server 'pulls' the configuration file and conducts the backup.

## Approach ##
1. We intend to implement pull backup mechanism and restrain ourselves from push backup mechanism. Read the [debate on push vs pull backups](https://news.ycombinator.com/item?id=8620236). Consider picking an open source suited for our needs from [here](https://github.com/restic/others)
2. We are open to fork the checkup open source such that we get downtime alerts when app server goes down (maybe this feature already exists in checkup, we must just verify). We also want 3rd party services (like pingdom) notifying us if our distributed uptime monitoring solution (like maybe the status page) has went down. This will verify that someone is [watching the watchman](https://www.urbandictionary.com/define.php?term=Who%20will%20watch%20the%20watchmen%3F). In this way we have Checkup script monitoring the App. Status page monitorign the Checkup script. And Pingdom monitoring the Status page.
3. We do not intend to create things from scratch. Instead of this project being a fork of other projects, it can also be just a very elaborate tutorial on how to accomplish our goals using existing open source tools.

## Contributing ##
1. Create an issue in github for each approach listed above. Expand the approaches further if required
2. Architect a solution. Document your architecture using [C4 Model](https://c4model.com/). We need architecture documented using :
    * System Context diagram (required immediately)
    * Container Diagram and Component Diagram (required after we have finished deciding on what open sources we will reuse to accomplish our goals) 
3. Suggest any problems and solutions that you face while taking backup and monitor uptime **in a distributed way**. Lets discuss them in github issues
