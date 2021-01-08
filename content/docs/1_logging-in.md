# Logging In

There are two methods to log in. Use the method appropriate for your environment.

## SSO Login

Because we can't authenticate with name/password, we will use the `sso` flag to the `cf login` command. When we run the command, we'll be prompted to visit a site, authenticate via our browser, and collect an authentication token that we will provide to our `cf login` command.

Here is an example:
```sh
user@workstation:$ cf login --sso -a api.run.{{< param system_domain >}} -o demo-org -s demo-space
API endpoint: api.run.{{< param system_domain >}}

Temporary Authentication Code ( Get one at https://login.run.{{< param system_domain >}}/passcode )> XXXXX

Authenticating...
OK

Targeted org demo-org

Targeted space demo-space



API endpoint:   https://api.run.{{< param system_domain >}} (API version: 2.137.0)
User:           user@pivotal.io
Org:            demo-org
Space:          demo-space

```


## Name/Password Login

Simply log in with name & password.

```sh
user@workstation:$ cf login -a api.run.platform.com -u user@platform.com
API endpoint: api.run.platform.com

Password: 
Authenticating...
OK

Targeted org demo-org

Targeted space demo-space



API endpoint:   https://api.run.platform.com (API version: 2.137.0)
User:           user@platform.com
Org:            demo-org
Space:          demo-space
```


Confirm success by listing applications (since we're new, we likely won't have any apps so the list will be empty):
```sh
user@workstation:$ cf apps
Getting apps in org demo-org / space demo-space as user@pivotal.io...
OK

No apps found
```
