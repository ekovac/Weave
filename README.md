# Weave
## Visit us at [our site](http://iweave.com)

# Status of this fork
This is a fork of [WeaveTeam/Weave](https://github.com/WeaveTeam/Weave), whose development stopped in 2017. In 2026 it was revived just enough to build again with a reproducible toolchain. **That work has not been extensively tested.** Treat this as software archaeology, not a maintained product: this project is very, very stale, and I don't anticipate maintaining it.

What has been verified:

* `ant dist` completes on Fedora Linux x86_64 with a Zulu JDK 7 and the Adobe Flex 4.5.1 SDK. It produces the Flash client SWFs, `WeaveServices.war` and `weave.zip`.
* The built HTML pages and SWFs are served correctly by a static web server.

What has not been verified:

* Running the Flash client. Flash Player is discontinued; [Ruffle](https://ruffle.rs) is the likely way to run it today. Weave is a Flex 4.5 application that loads its framework as separate SWFs at startup (runtime shared libraries), which may be a problem for Ruffle.
* Deploying `WeaveServices.war` to a servlet container or connecting it to a database.
* WeaveDesktop (Adobe AIR) and WeaveMobileClient, which aren't part of `ant dist` and weren't built.

Known caveats:

* The build needs Java 7. Under Java 8, WeaveData fails to compile ("Parameter initializer unknown or is not a compile-time constant"). Ant also needs `-XX:MaxPermSize=1024m` to compile everything in one run.
* The C code in `WeaveCore/libs/weave_flascc` is not rebuilt. It needs Adobe FlasCC/CrossBridge, which only ships Windows and Mac builds. The committed `WeaveCore/libs/weave_flascc.swc` (built in 2015) is used as-is.
* `GeometryStreamConverter` and `JTDS_SqlServerDriver` are not rebuilt either; their committed jars are used.
* The bundled Java libraries and JDBC drivers date from 2010–2015 and have not been audited. Don't expose the services to untrusted networks.
* Many links further down (the info.iweave.com wiki, the asdoc pages, the development environment guide) no longer resolve.

The easiest way to build is through [WeaveJS](https://github.com/ekovac/WeaveJS), which includes this repository as a submodule. Its `scripts/bootstrap.sh` installs the whole toolchain, and `npm run compile-weave` runs `ant dist` here. To build this repository standalone, see [INSTALL-LINUX.md](INSTALL-LINUX.md). Also pass `-DJAVA_LIBS=<dir>` to Ant if `servlet-api-2.5.jar` and `junit4.jar` aren't in `/usr/share/java`.

## This repository is for Weave version 1.9 ([Wiki](http://info.iweave.com/projects/weave/wiki)).

## To View a running version of Weave click [here](http://weaveteam.github.io/Weave-Binaries/weave.html)

## For some examples of what you can do with Weave click [here](http://iweave.com/documentation.html#examples)

# License
Weave is distributed under the [MPL-2.0](https://www.mozilla.org/en-US/MPL/2.0/) license.

# Download
[Releases of this fork](https://github.com/ekovac/Weave/releases)

[Installation Guide](INSTALL-LINUX.md)

Older upstream builds: [releases](https://github.com/WeaveTeam/Weave-Binaries/releases) and [last nightly build](https://github.com/WeaveTeam/Weave-Binaries/zipball/master) from WeaveTeam/Weave-Binaries.

# Documentation
You can find the Admin Console User Guide [here](http://info.iweave.com/projects/weave/wiki/Weave_Administration_Console_User_Guide)

Weave supports integration from multiple data sources including: **CSV, GeoJSON, SHP/DBF, CKAN**  
  
Additional developer documentation can be found [here](http://WeaveTeam.github.com/Weave-Binaries/asdoc/)

Components in this repository:

 * WeaveAPI: ActionScript interface classes.
 * WeaveCore: Core sessioning framework.
 * WeaveData: Data framework. Non-UI features.
 * WeaveUISpark: User interface classes (Spark components).
 * WeaveUI: User interface classes (Halo components).
 * WeaveClient: Flex application for Weave UI.
 * WeaveDesktop: Adobe AIR application front-end for Weave UI.
 * WeaveAdmin: Flex application for admin activities.
 * WeaveServletUtils: Back-end Java webapp libraries.
 * WeaveServices: Back-end Java webapp for Admin and Data server features.
 * GeometryStreamConverter: Java library for converting geometries into a streaming format. Binary included in WeaveServices/lib.
 * JTDS_SqlServerDriver: Java library for handling connections to Microsoft SQL Server. Binary included in WeaveServletUtils/lib.

To build Weave you need a Java 7 JDK, Apache Ant 1.9 and the [Adobe Flex 4.5.1A SDK](http://fpdownload.adobe.com/pub/flex/sdk/builds/flex4.5/flex_sdk_4.5.1.21328A.zip). [WeaveJS](https://github.com/ekovac/WeaveJS)'s `scripts/bootstrap.sh` installs all three.

To build the projects on the command line, use the **build.xml** Ant script. To create a ZIP file for deployment on another system (much like the nightlies,) use the **dist** target.

See [INSTALL-LINUX.md](INSTALL-LINUX.md) for detailed Linux install instructions.

