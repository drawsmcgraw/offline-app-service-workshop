# Namespacing

You can explicitly define the name of your application _without_ having a random phrase attached to it. For example, if you're testing a feature branch, and you've named the branch after an issue assigned to it (say, issue number 1331), you can deploy your feature branch right next to existing workloads with no interruption of the existing apps.

In the `manifest.yml`, be sure `random-route` is set to `false` and add a `-1331` to the end of your app name. Like so:

```yml
---
applications:
- name: attendees-1331
  random-route: false
  .
  .
  .
```

Now `cf push` again, and the platform will deploy a new application instance alongside your existing one. 

Here's an example output showing the two apps deployed.

```sh
cf apps
Getting apps in org user-org / space development as user@platform.com...
OK

name                                    requested state   instances   memory   disk   urls
attendees                               started           1/1         768M     1G     attendees.cfapps.io
attendees-1331                          started           1/1         768M     1G     attendees-1331.cfapps.io


```

Think about how you can take advantage of this in your CI/CD pipelines.
