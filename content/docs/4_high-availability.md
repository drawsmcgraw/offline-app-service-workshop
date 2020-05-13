# High Availability (HA)

Application Service has 4 levels of HA (High Availability) that keep your applications and the underlying platform running. 
1. App Instance
1. Availability Zone
1. Process
4. Machine

In this section, we will demonstrate one of them (App Instance). Failed application instances will be recovered.

At this time you should be running multiple instances of `attendees`. Confirm this with the following command:
```sh
cf app attendees
```

Return to `attendees` in a web browser and navigate to the Scale and HA page. Press the Refresh button. Confirm the application is running.
Kill the app. Press the Kill button!
Check the state of the app through the cf CLI.
```sh
cf app attendees
```

Sample output below (notice the requested state vs actual state). In this case,  Application Service had already detected the failure and is starting a new instance.
```sh
Showing health and status for app attendees in org mborges-org / space development as admin...

name:              attendees
requested state:   started
instances:         3/3
usage:             768M x 3 instances
routes:            attendees-doxastic-progenitiveness.apps.pcf.homelab.lan
last uploaded:     Wed 09 Aug 17:18:22 CDT 2017
stack:             cflinuxfs2
buildpack:         container-security-provider=1.5.0_RELEASE
                   java-buildpack=v3.18-offline-https://github.com/cloudfoundry/java-buildpack.git#841ecb2 java-main
                   open-jdk-like-jre=1.8.0_131 open-jdk-like-memory-calculator=2.0.2_RELEASE
                   open-jdk-like-security-providers secur...

     state      since                  cpu    memory           disk           details
#0   starting   2017-08-10T00:31:23Z   0.0%   272.3M of 768M   161.9M of 1G
#1   running    2017-08-10T00:30:24Z   6.3%   401.1M of 768M   161.9M of 1G
#2   running    2017-08-10T00:30:32Z   2.1%   408.8M of 768M   161.9M of 1G
```
Repeat this command as necessary until `state = running`.
In your browser, Refresh the attendees application.
The app is back up!

A new, healthy app instance has been automatically provisioned to replace the failing one.

View which instance was killed.
```sh
cf events attendees
```

Scale attendees back to our original settings.
```sh
cf scale attendees -i 1
```

## Questions
* How do you recover failing application instances today?
* What effect does this have on your application design?
* How could you determine if your application has been crashing?


