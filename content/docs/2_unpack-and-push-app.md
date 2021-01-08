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
Pushing app attendees to org demo-org / space demo-space as admin...
Applying manifest file /some/path/manifest.yml...
Manifest applied
Packaging files to upload...
Uploading files...
 584.74 KiB / 584.74 KiB  100.00% 1s

Waiting for API to complete processing files...

Staging app and tracing logs...
   Downloading staticfile_buildpack...
   Downloading java_buildpack_offline...
   Downloading ruby_buildpack...
   Downloading nginx_buildpack...
   Downloaded nginx_buildpack
   Downloading nodejs_buildpack...
   Downloaded java_buildpack_offline
   Downloading go_buildpack...
   Downloaded staticfile_buildpack
   Downloading r_buildpack...
   Downloading python_buildpack...
   Downloaded ruby_buildpack
   Downloading php_buildpack...
   Downloaded nodejs_buildpack
   Downloading dotnet_core_buildpack...
   Downloaded php_buildpack
   Downloading binary_buildpack...
   Downloaded go_buildpack
   Downloaded r_buildpack
   Downloaded binary_buildpack
   Downloaded python_buildpack
   Downloaded dotnet_core_buildpack
   Cell 335964d3-9060-4850-a024-07865061207f creating container for instance 6239bfab-cffc-4227-bad1-8abe8aa52a6b
   Cell 335964d3-9060-4850-a024-07865061207f successfully created container for instance 6239bfab-cffc-4227-bad1-8abe8aa52a6b
   Downloading app package...
   Downloaded app package (38.5M)
   -----> Java Buildpack v4.34 (offline) | https://github.com/cloudfoundry/java-buildpack.git#04543c2
   -----> Downloading Jvmkill Agent 1.16.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.16.0-RELEASE.so (found in cache)
   -----> Downloading Open Jdk JRE 1.8.0_275 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u275%2B1-linux-amd64.tar.gz (found in cache)
          Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (0.9s)
          JVM DNS caching disabled in lieu of BOSH DNS caching
   -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (found in cache)
          Loaded Classes: 17322, Threads: 250
   -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (found in cache)
   -----> Downloading Container Security Provider 1.18.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.18.0-RELEASE.jar (found in cache)
   -----> Downloading Spring Auto Reconfiguration 2.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/auto-reconfiguration/auto-reconfiguration-2.11.0-RELEASE.jar (found in cache)
   Exit status 0
   Uploading droplet, build artifacts cache...
   Uploading droplet...
   Uploading build artifacts cache...
   Uploaded build artifacts cache (131B)

Waiting for app attendees to start...

Instances starting...
Instances starting...
Instances starting...
Instances starting...
Instances starting...
Instances starting...
Instances starting...
Instances starting...
Instances starting...

name:              attendees
requested state:   started
routes:            attendees-sleepy-bilby-di.cfapps.{{< param system_domain >}}
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

type:            web
sidecars:
instances:       1/1
memory usage:    768M
start command:   JAVA_OPTS="-agentpath:$PWD/.java-buildpack/open_jdk_jre/bin/jvmkill-1.16.0_RELEASE=printHeapHistogram=1
                 -Djava.io.tmpdir=$TMPDIR
                 -XX:ActiveProcessorCount=$(nproc)
                 -Djava.ext.dirs=$PWD/.java-buildpack/container_security_provider:$PWD/.java-buildpack/open_jdk_jre/lib/ext
                 -Djava.security.properties=$PWD/.java-buildpack/java_security/java.security
                 $JAVA_OPTS"
                 &&
                 CALCULATED_MEMORY=$($PWD/.java-buildpack/open_jdk_jre/bin/java-buildpack-memory-calculator-3.13.0_RELEASE
                 -totMemory=$MEMORY_LIMIT
                 -loadedClasses=18116
                 -poolType=metaspace
                 -stackThreads=250
                 -vmOptions="$JAVA_OPTS")
                 &&
                 echo
                 JVM
                 Memory
                 Configuration:
                 $CALCULATED_MEMORY
                 &&
                 JAVA_OPTS="$JAVA_OPTS
                 $CALCULATED_MEMORY"
                 &&
                 MALLOC_ARENA_MAX=2
                 SERVER_PORT=$PORT
                 eval
                 exec
                 $PWD/.java-buildpack/open_jdk_jre/bin/java
                 $JAVA_OPTS
                 -cp
                 $PWD/.
                 org.springframework.boot.loader.JarLauncher
     state     since                  cpu    memory          disk           details
#0   running   2021-01-06T19:01:43Z   0.0%   83.6M of 768M   155.2M of 1G
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
