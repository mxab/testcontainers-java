# Networking and communicating with containers

## Exposing container ports to the host

It is common to want to connect to a container from your test process, running on the test 'host' machine.
For example, you may be testing a class that needs to connect to a backend or data store container.

Generally, each required port needs to be explicitly exposed. For example, we can specify one or more ports as follows:

<!--codeinclude-->
[Exposing ports](../example/src/test/java/generic/MultiplePortsExposedTest.java) inside_block:rule
<!--/codeinclude-->

Note that this exposed port number is from the *perspective of the container*. 

*From the host's perspective* Testcontainers actually exposes this on a random free port.
This is by design, to avoid port collisions that may arise with locally running software or in between parallel test runs.

Because there is this layer of indirection, it is necessary to ask Testcontainers for the actual mapped port at runtime.
This can be done using the `getMappedPort` method, which takes the original (container) port as an argument:

<!--codeinclude-->
[Retrieving actual ports at runtime](../example/src/test/java/generic/MultiplePortsExposedTest.java) inside_block:fetchPortsByNumber
<!--/codeinclude-->

!!! warning
    Because the randomised port mapping happens during container startup, the container must be running at the time `getMappedPort` is called. 
    You may need to ensure that the startup order of components in your tests caters for this.

There is also a `getFirstMappedPort` method for convenience, for the fairly common scenario of a container that only exposes one port:

<!--codeinclude-->
[Retrieving the first mapped port](../example/src/test/java/generic/MultiplePortsExposedTest.java) inside_block:fetchFirstMappedPort
<!--/codeinclude-->

## Getting the container IP address

When running with a local Docker daemon, exposed ports will usually be reachable on `localhost`.
However, in some CI environments they may instead be reachable on a different host.

As such, Testcontainers provides a convenience method to obtain an IP address on which the container should be reachable from the host machine.

<!--codeinclude-->
[Getting the container IP address](../example/src/test/java/generic/MultiplePortsExposedTest.java) inside_block:getContainerIpAddressOnly
<!--/codeinclude-->

It is normally advisable to use `getContainerIpAddress` and `getMappedPort` together when constructing addresses - for example:

<!--codeinclude-->
[Getting the container IP address and mapped port](../example/src/test/java/generic/MultiplePortsExposedTest.java) inside_block:getContainerIpAddressAndMappedPort
<!--/codeinclude-->

## Exposing host ports to the container

<!--codeinclude-->
[Exposing the host port](../example/src/test/java/generic/HostPortExposedTest.java) inside_block:exposePort
<!--/codeinclude-->

and then

<!--codeinclude-->
[Accessing the exposed host port from a container](../example/src/test/java/generic/HostPortExposedTest.java) inside_block:useHostExposedPort
<!--/codeinclude-->

## Exposing containers to other containers

> TODO draft content

## Networks

> TODO draft content
