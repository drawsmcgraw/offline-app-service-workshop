# Logging and Scaling

With the Application Service, you can view the logs of your application in real time.

## Access Application Logs


Tail the logs:
```sh
cf logs attendees
```

Then open a browser and view the _attendees_ application. 


Observe the log output when the _attendees_ web page is refreshed. 
More logs are added!

To stop tailing logs, go to the terminal tailing the logs and send an
interrupt (Control + c).

## Access Application Events

Events for the application can also be used to compliment the logs in determining what has occurred with an application.
```sh
cf events attendees
```

## Scaling the Application

Start tailing the logs again.
```sh
cf logs attendees | grep "API\|CELL"
```

The above statement filters only matching log lines from the Cloud Controller 
and Cell components.

In another terminal window scale attendees. 
```sh
cf scale attendees -m 1G
```

Observe log output.
Stop tailing the logs.
Scale attendees back to our original settings.

```sh
cf scale attendees -m 768M
```

## Scale Out
Browse to the _Scale and HA_ page on the _attendees_ application.
Review the _Application Environment Information_. 


Press the `Refresh` button multiple times. All requests are going to one application instance.

Start tailing the logs.
```sh
[mac, linux]
 cf logs attendees | grep "API\|CELL"

[windows]
cf logs attendees | findstr "API CELL"
```

In another terminal windows, scale the _attendees_ application.

```sh
cf scale attendees -i 3
```

Observe log output. Then stop tailing the logs.
Return to `attendees` in a web browser. Press the `Refresh` button several times. Observe the `Addresses` and `Instance Index` changing.


