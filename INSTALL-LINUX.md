# End-user requirements

* A Flash runtime: Adobe Flash Player 10.0+ (discontinued) or [Ruffle](https://ruffle.rs). Running Weave under Ruffle hasn't been verified yet.

# Server requirements

* A Java servlet container that implements `javax.servlet` (Servlet 2.5 or later), such as Tomcat 9 or earlier, or Jetty. Tomcat 10 and later switched to `jakarta.servlet` and can't run `WeaveServices.war`.
* MySQL or PostgreSQL

Deploying the server hasn't been tested since this fork was revived in 2026.

# Building

The easiest way to build is through [WeaveJS](https://github.com/ekovac/WeaveJS): its `scripts/bootstrap.sh` installs everything below into `.toolchain/`, and `npm run compile-weave` runs `ant dist` in this repository. To build standalone:

1. Install a Java 7 JDK and Apache Ant 1.9. Linux distributions no longer package Java 7, but [Azul Zulu 7](https://www.azul.com/downloads/?version=java-7-lts&os=linux&package=jdk) is still available. Flex 4.5.1's compiler fails under Java 8 and later.
2. Download the [Adobe Flex 4.5.1A SDK](http://fpdownload.adobe.com/pub/flex/sdk/builds/flex4.5/flex_sdk_4.5.1.21328A.zip) and extract it to a directory in your home directory, something like `~/bin/flex`.
3. Download [servlet-api-2.5.jar](https://repo1.maven.org/maven2/javax/servlet/servlet-api/2.5/servlet-api-2.5.jar) and [junit-4.12.jar](https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar), and save them in one directory as `servlet-api-2.5.jar` and `junit4.jar`, for example `~/bin/javalibs`.
4. Add the following lines to your `.bashrc` or equivalent, modified as appropriate to the paths chosen above:

   ```sh
   export JAVA_HOME=~/bin/zulu7
   export FLEX_HOME=~/bin/flex
   export ANT_OPTS="-XX:MaxPermSize=1024m -Xms256M -Xmx2G -DJAVA_LIBS=$HOME/bin/javalibs"
   export PATH="$JAVA_HOME/bin:$PATH"
   ```

   Additionally, run these lines in your working terminal, or open a fresh terminal to ensure the environment variables are set.
5. Clone the Weave git repository and enter it:

   ```sh
   git clone https://github.com/ekovac/Weave.git
   cd Weave
   ```
6. To use `ant install`, create a `user.properties` file that sets `WEAVE_DOCROOT` to a directory writable by your user, from which your servlet container will serve the client:

   ```properties
   WEAVE_DOCROOT=/home/user/pub/app
   ```

   The `*_SWF` variables in `build.properties` already match the Flex 4.5.1A SDK.
7. Build and install:

   ```sh
   ant install
   ```

   This copies the client to `WEAVE_DOCROOT` and `WeaveServices.war` to the directory above it. Alternatively, `ant dist` builds a `weave.zip` containing both the `WeaveServices.war` and the Weave `ROOT` folder, so you can deploy it on another system.

# Deploying to Tomcat

1. Create `conf/Catalina/localhost/weave.xml` in your Tomcat directory (for distribution packages, under `/etc/tomcat9/`, for example), so that Tomcat serves `WEAVE_DOCROOT` at `/weave`:

   ```xml
   <Context docBase="/home/user/pub/app"/>
   ```

   Modify `docBase` to match the value of `WEAVE_DOCROOT`.
2. Copy `WeaveServices.war` into Tomcat's `webapps` directory (for example `/var/lib/tomcat9/webapps`).
3. Restart Tomcat as appropriate for your distribution, for example:

   ```sh
   sudo systemctl restart tomcat9
   ```
4. Open `http://localhost:8080/weave/weave.html` or `http://localhost:8080/weave/AdminConsole.html` in your browser to test.
