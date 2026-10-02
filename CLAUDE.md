# CLAUDE.md

Weave 1.9: a Flex/ActionScript 3 Flash client (`WeaveClient`, `WeaveAdmin` and their libraries) plus Java servlets (`WeaveServices`, `WeaveServletUtils`). Development stopped in 2017; it was revived in 2026 so it builds again. It is normally checked out as the `Weave` submodule of [WeaveJS](https://github.com/ekovac/WeaveJS), which provides the toolchain.

## Build

From the WeaveJS checkout:

```
./scripts/bootstrap.sh    # once
. .toolchain/env.sh
npm run compile-weave     # runs `ant dist` here, then restores weave_version.txt
```

Standalone, `ant dist` in this directory needs:

- **Java 7.** Under Java 8, WeaveData fails with "Parameter initializer unknown or is not a compile-time constant".
- `FLEX_HOME` pointing at the Adobe Flex 4.5.1 SDK (4.5.1.21328A). The framework SWF names in `build.properties` assume that exact build.
- `ANT_OPTS` including `-XX:MaxPermSize=1024m`. A clean build otherwise runs out of PermGen in WeaveAdmin.
- `-DJAVA_LIBS=<dir>` if `servlet-api-2.5.jar` and `junit4.jar` aren't in `/usr/share/java`.

Outputs, all ignored by git: `ROOT/` (Flash client and RSLs), `WeaveServices.war` and `weave.zip`. The build writes a version stamp into the tracked files `WeaveUISpark/src/weave/weave_version.txt` and `WeaveServices/src/weave/weave_version.txt`. Restore them with `git checkout --` before committing.

## Gotchas

- Many files here have CRLF line endings, including the `build.xml` files and the Markdown. Preserve each file's existing line endings when editing it.
- Ant skips projects whose output is newer than their sources. Use `ant clean` before a build you want to trust.
- `WeaveCore/libs/weave_flascc.swc` is compiled from the C sources in `WeaveCore/libs/weave_flascc/` with Adobe FlasCC/CrossBridge. That toolchain has no Linux build, so it is deliberately not rebuilt; edits to the C code won't take effect. The same goes for the committed `GeometryStreamConverter` and JTDS jars.
- WeaveDesktop (AIR) and WeaveMobileClient aren't part of `ant dist` and haven't been built since the revival.

## Testing

There are no automated tests. Serve `ROOT/` with a static web server and open `weave.html` or `AdminConsole.html` in [Ruffle](https://ruffle.rs). Running the client under Ruffle hasn't been verified yet. Data and admin features need `WeaveServices.war` deployed in a servlet container (Tomcat or Jetty) with a MySQL or PostgreSQL database.
