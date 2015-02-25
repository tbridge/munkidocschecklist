Munki installs as part of its standard installation package (found at munkibuilds.org) four LaunchDaemons into /Library/LaunchDaemons, and three LaunchAgents into /Library/LaunchAgents. 

This document will describe these seven launchd objects and what they do as part of Munki. 

### /Library/LaunchDaemons/com.googlecode.munki.logouthelper.plist

This LaunchDaemon is invoked by managedsoftwareupdate when a force_install_after_date is due within four hours. This tool runs in the background once a minute, sleeping between runs. At the currently hardcoded intervals in minutes before the required logout: [240, 180, 120, 60, 90, 45, 30, 15, 5, 2], it notifies the user via Managed Software Update.app of an impending forced logout.

### /Library/LaunchDaemons/com.googlecode.munki.managedsoftwareupdate-check.plist

This is the standard munki check-in daemon. It runs once per hour by default at a random interval.  Unloading this daemon will prevent munki from running on a regular basis. You can still run munki manually with the automated check disabled.

### /Library/LaunchDaemons/com.googlecode.munki.managedsoftwareupdate-install.plist

Invoked by Managed Software Center most often, this triggers the equivalent of:

    managedsoftwareupdate --installwithnologout
    
It runs the MunkiStatus.app and associated update processes without logging the user out.

### /Library/LaunchDaemons/com.googlecode.munki.managedsoftwareupdate-manualcheck.plist

Invoked by Managed Software Center most often, this is triggered when a manual check is requested by the user. 

### /Library/LaunchAgents/com.googlecode.munki.ManagedSoftwareCenter.plist

Launches the Managed Software Center. Triggered by com.googlecode.munki.managedsoftwareupdate-check.plist.

### /Library/LaunchAgents/com.googlecode.munki.MunkiStatus.plist

Runs at the login window, and invokes the MunkiStatus.app binary. It checks for updates and runs the updater process when a user is logged out. 

### /Library/LaunchAgents/com.googlecode.munki.managedsoftwareupdate-loginwindow.plist

For software that is installed while a user is logged out, the process is run on top of the login window, or before the login window has loaded. As noted by the LimitLoadToSessionType key in the LaunchAgent, this is meant for running at the login window.