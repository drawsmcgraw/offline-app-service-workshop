# Unpack The Demo App and cf push

## Unpack
Create a directory, named `workspace`. Place the demo app tarball in there and unpack it. 

This project has already been built and is ready for deployment. There is no need to build it.

## Push

Push the app:
```sh
cf push
```

## Profit
Congrats! You just built and deployed your first application! See below for example output and an explanation of what just happened.

## cf push example output
Below is an example of the output you will see when pushing your app. See the numbers in parenthesis and their corresponding explanation below the sample output for more details on this process.

```sh
Using manifest file /Users/username/Tools/PCF/demo2.0/attendees/manifest.yml // (1)

Creating app attendees in org pivot-username / space development as username@pivotal.io... // (2)
OK

Creating route attendees-naturopathic-souple.cfapps.pez.pivotal.io... // (3)
OK

Binding attendees-naturopathic-souple.cfapps.pez.pivotal.io to attendees... // (4)
OK

Uploading attendees...
Uploading app files from: /var/folders/k6/bg1q4mc50sl957cvyfw0s26r0000gp/T/unzipped-app942168724
Uploading 678.4K, 143 files // (5)
Done uploading
OK

Starting app attendees in org pivot-username / space development as username@pivotal.io...
Downloading python_buildpack...
Downloading staticfile_buildpack...
Downloading php_buildpack...
Downloading java_buildpack_offline_v4...
Downloading hwc_buildpack...
Downloaded java_buildpack_offline_v4
Downloading java_buildpack_offline...
Downloaded hwc_buildpack
Downloading ruby_buildpack...
Downloaded php_buildpack
Downloading nodejs_buildpack...
Downloaded python_buildpack
Downloaded staticfile_buildpack
Downloading go_buildpack...
Downloading null_buildpack...
Downloaded go_buildpack
Downloading binary_buildpack...
Downloaded java_buildpack_offline
Downloading dotnet_core_buildpack...
Downloaded ruby_buildpack
Downloading tc_server_buildpack_offline...
Downloaded null_buildpack
Downloading azq_nodejs...
Downloaded nodejs_buildpack
Downloaded binary_buildpack
Downloaded dotnet_core_buildpack
Downloaded tc_server_buildpack_offline
Downloaded azq_nodejs
Creating container
Successfully created container
Downloading app package...
Downloaded app package (34.4M)
Staging... // (6)
-----> Java Buildpack Version: v3.18 (offline) | https://github.com/cloudfoundry/java-buildpack.git#841ecb2
-----> Downloading Open Jdk JRE 1.8.0_131 from https://java-buildpack.cloudfoundry. org/openjdk/trusty/x86_64/openjdk-1.8.0_131.tar.gz (found in cache) // (7)
       Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.3s)
-----> Downloading Open JDK Like Memory Calculator 2.0.2_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/trusty/x86_64/memory-calculator-2.0.2_RELEASE.tar.gz (found in cache)
       Memory Settings: -Xss349K -Xmx681574K -XX:MaxMetaspaceSize=104857K -Xms681574K -XX:MetaspaceSize=104857K
-----> Downloading Container Security Provider 1.5.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.5.0_RELEASE.jar (found in cache)
-----> Downloading Spring Auto Reconfiguration 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/auto-reconfiguration/auto-reconfiguration-1.11.0_RELEASE.jar (found in cache)
Exit status 0
Staging complete
Uploading droplet, build artifacts cache... // (8)
Uploading build artifacts cache...
Uploading droplet...
Uploaded build artifacts cache (107B)
Uploaded droplet (79.9M)
Uploading complete
Destroying container
Successfully destroyed container

0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
1 of 1 instances running

App started


OK

App attendees was started using this command `CALCULATED_MEMORY=$($PWD/.java-buildpack/open_jdk_jre/bin/java-buildpack-memory-calculator-2.0.2_RELEASE -memorySizes=metaspace:64m..,stack:228k.. -memoryWeights=heap:65,metaspace:10,native:15,stack:10 -memoryInitials=heap:100%,metaspace:100% -stackThreads=300 -totMemory=$MEMORY_LIMIT) && JAVA_OPTS="-Djava.io.tmpdir=$TMPDIR -XX:OnOutOfMemoryError=$PWD/.java-buildpack/open_jdk_jre/bin/killjava.sh $CALCULATED_MEMORY -Djava.ext.dirs=$PWD/.java-buildpack/container_security_provider:$PWD/.java-buildpack/open_jdk_jre/lib/ext -Djava.security.properties=$PWD/.java-buildpack/security_providers/java.security" && SERVER_PORT=$PORT eval exec $PWD/.java-buildpack/open_jdk_jre/bin/java $JAVA_OPTS -cp $PWD/. org.springframework.boot.loader.JarLauncher` // (9)

Showing health and status for app attendees in org pivot-username / space development as username@pivotal.io...
OK

requested state: started
instances: 1/1
usage: 1G x 1 instances
urls: attendees-naturopathic-souple.cfapps.pez.pivotal.io
last uploaded: Mon Aug 7 21:47:10 UTC 2017
stack: cflinuxfs2
buildpack: container-security-provider=1.5.0_RELEASE java-buildpack=v3.18-offline-https://github.com/cloudfoundry/java-buildpack.git#841ecb2 java-main open-jdk-like-jre=1.8.0_131 open-jdk-like-memory-calculator=2.0.2_RELEASE open-jdk-like-security-providers secur...

     state     since                    cpu      memory         disk           details
#0   running   2017-08-07 04:48:08 PM   231.0%   498.1M of 1G   161.9M of 1G // (10)
```

## cf push, Explained

1. The CLI is using a manifest to provide necessary configuration details such as application name, memory to be allocated, and path to the application artifact.
Take a look at `manifest.yml` to see how.

1. In most cases, the CLI indicates each Cloud Foundry API call as it happens.
In this case, the CLI has created an application record for _attendees_ in your assigned space.
1. All HTTP/HTTPS requests to applications will flow through Cloud Foundry's front-end router called GoRouter.
Here the CLI is creating a route with random word tokens inserted (again, see `manifest.yml` for a hint!) to prevent route collisions across the default PCF domain.
1. Now the CLI is _binding_ the created route to the application.
Routes can actually be bound to multiple applications to support techniques such as blue-green deployments.
1. The CLI finally uploads the application bits to PCF. 
1. Now we begin the staging process. The Java Buildpack is responsible for assembling the runtime components necessary to run the application.
1. Here we see the version of the JRE that has been chosen and installed.
1. The complete package of your application and all of its necessary runtime components is called a _droplet_.
Here the droplet is being uploaded to PCF's internal blobstore so that it can be easily copied to one or more Diego Cells for execution.
1. The CLI tells you exactly what command and argument set was used to start your application.
1. Finally the CLI reports the current status of your application's health.
You can get the same output at any time by typing `cf app attendees`.



