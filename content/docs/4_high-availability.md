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
Showing health and status for app attendees in org demo-org / space demo-space as admin...

name:              attendees
requested state:   started
routes:            attendees-sleepy-bilby-di.cfapps.pcfeagledev.cf-app.com
last uploaded:     Wed
                   06
                   Jan
                   19:01:15
                   UTC
                   2021
stack:             cflinuxfs3
buildpacks:
	name                     version                                                                    detect output   buildpack name
	java_buildpack_offline   v4.34-offline-https://github.com/cloudfoundry/java-buildpack.git#04543c2   java            java

type:           web
sidecars:
instances:      1/3
memory usage:   768M
     state      since                  cpu    memory           disk           details
#0   running    2021-01-06T19:01:43Z   0.2%   277.9M of 768M   155.2M of 1G
#1   running    2021-01-06T19:11:58Z   0.0%   510.7K of 768M   155.2M of 1G
#2   starting   2021-01-06T19:11:58Z   0.2%   251M of 768M     155.2M of 1G
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
