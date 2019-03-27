Simple Cron Job Simulation
With the iOS daemon launcher launchd and a few scripts you can have a simple cron job simulation for iOS.

The jobs will execute hourly, daily, weekly and monthly or with a seconds counter. The cron jobs are scripts in e.g. /etc/cron/ .

It is up to you to fill the scripts with your commands.

Cyclic called Scripts
Please create following scripts:

/etc/cron/seconds.sh
/etc/cron/hourly.sh
/etc/cron/daily.sh
/etc/cron/weekly.sh
/etc/cron/monthly.sh
You can use following template for the scripts:

#!/bin/sh

#This script is executed by the daemon launcher every hour/day/week/month 

After you have created the scripts, please change the executable flag. Use the tool chmod to change the permission to 07xx.



Daemon Launcher Plist
To execute the above scripts cyclic you have to create following plist files in /Library/LaunchDaemons:


/Library/LaunchDaemons/com.cron.seconds.plist (here every 900 seconds = 15 minute)

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

  <dict>

        <key>Label</key>

          <string>cron job seconds</string>

        <key>ProgramArguments</key>

          <array>

            <string>/etc/cron/seconds.sh</string>

          </array>

        <key>StartInterval</key>

            <integer>900</integer>

        <key>LowPriorityIO</key>

          <true/>

        <key>StandardErrorPath</key>

          <string>/dev/null</string>

        <key>StandardOutPath</key>

          <string>/dev/null</string>

  </dict>

</plist>


/Library/LaunchDaemons/com.cron.hourly.plist

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

  <dict>

        <key>Label</key>

          <string>cron job hourly</string>

        <key>ProgramArguments</key>

          <array>

            <string>/etc/cron/hourly.sh</string>

          </array>

        <key>StartCalendarInterval</key>

          <dict>

            <key>Minute</key>

            <integer>0</integer>

          </dict>

        <key>LowPriorityIO</key>

          <true/>

        <key>StandardErrorPath</key>

          <string>/dev/null</string>

        <key>StandardOutPath</key>

          <string>/dev/null</string>

  </dict>

</plist>


/Library/LaunchDaemons/com.cron.daily.plist

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

  <dict>

        <key>Label</key>

          <string>cron job daily</string>

        <key>ProgramArguments</key>

          <array>

            <string>/etc/cron/daily.sh</string>

          </array>

        <key>StartCalendarInterval</key>

          <dict>

            <key>Hour</key>

            <integer>0</integer>

            <key>Minute</key>

            <integer>10</integer>

          </dict>

        <key>LowPriorityIO</key>

          <true/>

        <key>StandardErrorPath</key>

          <string>/dev/null</string>

        <key>StandardOutPath</key>

          <string>/dev/null</string>

  </dict>

</plist>


/Library/LaunchDaemons/com.cron.weekly.plist

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

  <dict>

        <key>Label</key>

          <string>cron job weekly</string>

        <key>ProgramArguments</key>

          <array>

            <string>/etc/cron/weekly.sh</string>

          </array>

        <key>StartCalendarInterval</key>

          <dict>

            <key>Weekday</key>

            <integer>1</integer>

            <key>Hour</key>

            <integer>1</integer>

            <key>Minute</key>

            <integer>20</integer>

          </dict>

        <key>LowPriorityIO</key>

          <true/>

        <key>StandardErrorPath</key>

          <string>/dev/null</string>

        <key>StandardOutPath</key>

          <string>/dev/null</string>

  </dict>

</plist>


/Library/LaunchDaemons/com.cron.monthly.plist

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

  <dict>

        <key>Label</key>

          <string>cron job monthly</string>

        <key>ProgramArguments</key>

          <array>

            <string>/etc/cron/monthly.sh</string>

          </array>

        <key>StartCalendarInterval</key>

          <dict>

            <key>Day</key>

            <integer>1</integer>

            <key>Hour</key>

            <integer>2</integer>

            <key>Minute</key>

            <integer>30</integer>

          </dict>

        <key>LowPriorityIO</key>

          <true/>

        <key>StandardErrorPath</key>

          <string>/dev/null</string>

        <key>StandardOutPath</key>

          <string>/dev/null</string>

  </dict>

</plist>



Start/Stop the Services
With the tool launchctl you can now start or stop the services.


Start

launchctl load  /Library/LaunchDaemons/com.cron.seconds.plist

launchctl load  /Library/LaunchDaemons/com.cron.hourly.plist

launchctl load  /Library/LaunchDaemons/com.cron.daily.plist

launchctl load  /Library/LaunchDaemons/com.cron.weekly.plist

launchctl load  /Library/LaunchDaemons/com.cron.monthly.plist


Stop

launchctl unload  /Library/LaunchDaemons/com.cron.seconds.plist

launchctl unload  /Library/LaunchDaemons/com.cron.hourly.plist

launchctl unload  /Library/LaunchDaemons/com.cron.daily.plist

launchctl unload  /Library/LaunchDaemons/com.cron.weekly.plist

launchctl unload  /Library/LaunchDaemons/com.cron.monthly.plist


Check

launchctl list
