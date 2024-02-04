### Micronaut nativeBuild Fails with JobRunr

I'm facing an issue when trying to build a Micronaut application with JobRunr in native mode using GraalVM. 
The application runs fine with `./gradlew run`, but when I attempt `./gradlew nativeRun`, I encounter an exception.

[Here's my configuration:](https://github.com/cmabdullah/example-micronaut/blob/native/build.gradle)

```gradle
graalvmNative {
    binaries {
        all {
            javaLauncher.set(javaToolchains.launcherFor {
                languageVersion.set(JavaLanguageVersion.of(21))
                vendor.set(JvmVendorSpec.GRAAL_VM)
            })
        }
    }
}
```

Any insights or solutions would be highly appreciated. Thanks!

Exception Trace
![Screenshot 2024-02-04 at 7.52.25 PM.png](Screenshot%202024-02-04%20at%207.52.25%20PM.png)
```
➜  example-micronaut git:(main) ✗ ./gradlew nativeRun

> Task :generateResourcesConfigFile
[native-image-plugin] Resources configuration written into /Users/cmabdullahkhan/Documents/workspace/example-micronaut/build/native/generated/generateResourcesConfigFile/resource-config.json

> Task :nativeCompile
[native-image-plugin] GraalVM Toolchain detection is enabled
[native-image-plugin] GraalVM uses toolchain detection. Selected:
[native-image-plugin]    - language version: 21
[native-image-plugin]    - vendor: GraalVM Community
[native-image-plugin]    - runtime version: 21.0.1+12-jvmci-23.1-b19
[native-image-plugin] Native Image executable path: /Users/cmabdullahkhan/Library/Java/JavaVirtualMachines/graalvm-jdk-21.0.1/Contents/Home/lib/svm/bin/native-image
========================================================================================================================
GraalVM Native Image: Generating 'example-jobrunr-micronaut' (executable)...
========================================================================================================================
For detailed information and explanations on the build output, visit:
https://github.com/oracle/graal/blob/master/docs/reference-manual/native-image/BuildOutput.md
------------------------------------------------------------------------------------------------------------------------
[1/8] Initializing...                                                                                   (20.8s @ 0.18GB)
 Java version: 21.0.1+12, vendor version: GraalVM CE 21.0.1+12.1
 Graal compiler: optimization level: 2, target machine: x86-64-v3
 C compiler: cc (apple, x86_64, 11.0.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 3 user-specific feature(s):
 - com.oracle.svm.thirdparty.gson.GsonFeature
 - io.micronaut.core.io.service.ServiceLoaderFeature
 - io.micronaut.jackson.JacksonDatabindFeature
------------------------------------------------------------------------------------------------------------------------
Build resources:
 - 6.05GB of memory (75.6% of 8.00GB system memory, determined at start)
 - 4 thread(s) (100.0% of 4 available processor(s), determined at start)
[2/8] Performing analysis...  [*****]                                                                  (299.0s @ 1.79GB)
   19,215 reachable types   (89.8% of   21,391 total)
   30,100 reachable fields  (62.2% of   48,422 total)
   99,800 reachable methods (63.3% of  157,725 total)
    6,169 types,   621 fields, and 5,954 methods registered for reflection
       67 types,    66 fields, and    59 methods registered for JNI access
        5 native libraries: -framework CoreServices, -framework Foundation, dl, pthread, z
[3/8] Building universe...                                                                              (67.6s @ 1.79GB)
[4/8] Parsing methods...      [*******]                                                                 (53.4s @ 1.37GB)
[5/8] Inlining methods...     [***]                                                                     (46.4s @ 2.02GB)
[6/8] Compiling methods...    [******************]                                                     (345.6s @ 2.78GB)
[7/8] Layouting methods...    [******]                                                                  (38.4s @ 1.81GB)
[8/8] Creating image...       [*****]                                                                   (30.3s @ 1.90GB)
  48.39MB (52.08%) for code area:    67,057 compilation units
  44.12MB (47.48%) for image heap:  423,883 objects and 547 resources
 420.30kB ( 0.44%) for other data
  92.92MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 origins of code area:                                Top 10 object types in image heap:
  13.77MB java.base                                           14.56MB byte[] for code metadata
   5.92MB h2-2.2.224.jar                                       6.02MB byte[] for java.lang.String
   3.72MB java.xml                                             5.61MB java.lang.Class
   2.08MB jackson-databind-2.15.3.jar                          3.95MB java.lang.String
   1.67MB svm.jar (Native Image)                               1.61MB com.oracle.svm.core.hub.DynamicHubCompanion
   1.45MB jobrunr-micronaut-feature-6.3.2.jar                  1.48MB byte[] for embedded resources
   1.07MB micronaut-inject-4.1.11.jar                          1.17MB byte[] for reflection metadata
   1.02MB reactor-core-3.5.11.jar                            827.57kB java.lang.String[]
 873.66kB micronaut-http-4.1.11.jar                          806.68kB byte[] for general heap data
 872.84kB micronaut-http-server-netty-4.1.11.jar             721.16kB java.lang.Object[]
  15.50MB for 78 more packages                                 7.42MB for 3813 more object types
------------------------------------------------------------------------------------------------------------------------
Recommendations:
 INIT: Adopt '--strict-image-heap' to prepare for the next GraalVM release.
 AWT:  Use the tracing agent to collect metadata for AWT.
 HEAP: Set max heap for improved and more predictable memory usage.
 CPU:  Enable more CPU features with '-march=native' for improved performance.
------------------------------------------------------------------------------------------------------------------------
                      159.4s (17.5% of total time) in 127 GCs | Peak RSS: 3.47GB | CPU load: 1.91
------------------------------------------------------------------------------------------------------------------------
Produced artifacts:
 /Users/cmabdullahkhan/Documents/workspace/example-micronaut/build/native/nativeCompile/example-jobrunr-micronaut (executable)
========================================================================================================================
Finished generating 'example-jobrunr-micronaut' in 15m 5s.
[native-image-plugin] Native Image written to: /Users/cmabdullahkhan/Documents/workspace/example-micronaut/build/native/nativeCompile

> Task :nativeRun FAILED
 __  __ _                                  _   
|  \/  (_) ___ _ __ ___  _ __   __ _ _   _| |_ 
| |\/| | |/ __| '__/ _ \| '_ \ / _` | | | | __|
| |  | | | (__| | | (_) | | | | (_| | |_| | |_ 
|_|  |_|_|\___|_|  \___/|_| |_|\__,_|\__,_|\__|
19:49:58.867 [main] INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Starting...
19:49:58.882 [main] INFO  com.zaxxer.hikari.pool.HikariPool - HikariPool-1 - Added connection conn0: url=jdbc:h2:file:/tmp/default user=SA
19:49:58.882 [main] INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Start completed.
19:49:58.887 [main] ERROR io.micronaut.runtime.Micronaut - Error starting Micronaut server: Error instantiating bean of type  [org.jobrunr.micronaut.autoconfigure.RecurringMethodProcessor]

Message: Error setting field value: No field 'applicationContext' found for type: org.jobrunr.micronaut.autoconfigure.JobRunrFactory
Path Taken: new RecurringMethodProcessor(JobScheduler jobScheduler) --> new RecurringMethodProcessor([JobScheduler jobScheduler])
io.micronaut.context.exceptions.DependencyInjectionException: Error instantiating bean of type  [org.jobrunr.micronaut.autoconfigure.RecurringMethodProcessor]

Message: Error setting field value: No field 'applicationContext' found for type: org.jobrunr.micronaut.autoconfigure.JobRunrFactory
Path Taken: new RecurringMethodProcessor(JobScheduler jobScheduler) --> new RecurringMethodProcessor([JobScheduler jobScheduler])
        at io.micronaut.context.AbstractInitializableBeanDefinition.setFieldWithReflection(AbstractInitializableBeanDefinition.java:957)
        at org.jobrunr.micronaut.autoconfigure.$JobRunrFactory$Definition.inject(Unknown Source)
        at org.jobrunr.micronaut.autoconfigure.$JobRunrFactory$Definition.instantiate(Unknown Source)
        at io.micronaut.context.DefaultBeanContext.resolveByBeanFactory(DefaultBeanContext.java:2307)
        at io.micronaut.context.DefaultBeanContext.doCreateBean(DefaultBeanContext.java:2277)
        at io.micronaut.context.DefaultBeanContext.doCreateBean(DefaultBeanContext.java:2289)
        at io.micronaut.context.DefaultBeanContext.createRegistration(DefaultBeanContext.java:3056)
        at io.micronaut.context.SingletonScope.getOrCreate(SingletonScope.java:81)
        at io.micronaut.context.DefaultBeanContext.findOrCreateSingletonBeanRegistration(DefaultBeanContext.java:2958)
        at io.micronaut.context.DefaultBeanContext.resolveBeanRegistration(DefaultBeanContext.java:2919)
        at io.micronaut.context.DefaultBeanContext.resolveBeanRegistration(DefaultBeanContext.java:2730)
        at io.micronaut.context.DefaultBeanContext.getBean(DefaultBeanContext.java:1693)
        at io.micronaut.context.DefaultBeanContext.getBean(DefaultBeanContext.java:1675)
        at org.jobrunr.micronaut.autoconfigure.$JobRunrFactory$JobScheduler0$Definition.instantiate(Unknown Source)
        at io.micronaut.context.DefaultBeanContext.resolveByBeanFactory(DefaultBeanContext.java:2307)

```

### Steps to Reproduce

Clone the project from https://github.com/jobrunr/example-micronaut
Run 
> ./gradlew nativeRun

### Expected Behavior
The application should run successfully in native mode.

### Actual Behavior
Encountering an exception during the native run process.

### Environment
- Micronaut Version: 2.5.11
- GraalVM Version: GraalVM CE 21.0.1+12.1
- JobRunr Version: jobrunr-micronaut-feature-6.3.2
- Operating System: 10.14.6