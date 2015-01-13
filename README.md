## OPSI Admin WUI

This application provides an alternative web user interface to OPSI and also enchants OPSI own possibilities. It is developed for the purpose of Institute of Computer Science of Masaryk University.

### Goals

* To allow less privileged admins to install only on their clients using only allowed netboot products.
* Provide simple and localized (Czech) UI with required functionality only.
* Organize clients into groups.
* Set access rights to groups, clients and netboot products.
* Import / Export client entries between OPSI servers.

### Build and run

This app uses Spring MVC, Hibernate, GWT 2.6.1, GXT 2.3.1a and some closed source libraries from the original author. In order to run or compile the app you need to gather all parts. You can then use Maven to build production war package:

```bash
mvn clean install -Pmysql
```

This app require connection to the working OPSI server, own DB (MySQL) for storing client and group information and LDAP server to provide authorization for users. App is expected to run in Apache Tomcat 6 container. If you have war packaged, simply deploy it to the Tomcat.

### Development

#### Source code

When contributing please respect following:

* Use Intellij IDEA auto formatting
* Opening brace goes in the same line as the statement
* Indent by tabs (NOT spaces)
* Document your code
* Use english in code, comments, commits

#### App structure

This app consist of 2 main parts:

* Client GWT/GXT web app, which provides simplified UI for OPSI and apps own purpose.
* Server part, which handles authorization of users to respective agendas, allow grouping of clients and communicate with the rest of systems.

#### Local run (GWT - DevMode)

You can use either standard DevMode or SuperDevMode of GWT to run the app. Because app needs you to log-in first, you must have compiled app already present. Run: ``mvn clean install -DskipTests -Pmyslq`` in sources parent folder.

Now you can start the app by running command inside *opsi-admin-wui/* folder:

```bash
mvn gwt:run -DskipTests -Pmysql
```

Login on displayed webpage and then fix the URL by adding codeserver reference to load it in devel mode.

If you wish to use new super-dev-mode, start also codeserver the same way:

```bash
mvn gwt:run-codeserver -DskipTests -Pmysql
```

You don't have to update link to the codeserver then, just use bookmarklets to trigger recompile of app when necessary.

##### Requirements

* Setup paths, names and passwords in configuration files located in sources.

Due to secured environment you must setup a lot of resources locally with valid server certificates in order to run the app in GWT development mode (own OPSI, MySQL, LDAP).
If you have any existing (pre-production) environment, we recommend to use it and connect by SSH tunnels (change hostname/port in config files to localhost).

```bash
# tunnel for OPSI server RPC
ssh -L 4447:localhost:4447 login@server.domain

# tunnel for MySQL DB
ssh -L 3306:localhost:3306 login@server.domain

# tunnel for LDAP
ssh -L 10636:localhost:636 login@server.domain
```

* You must choose port above 10000 (e.g. 10636) for LDAP since privileged ports (below 10000) can't be redirected.
* You must have a valid user entry in LDAP with password, because otherwise you won't pass apps login screen.

In order to skip server certificates validation (necessary when redirecting ports) uncomment following in bean definition of *contextSource* in *security.xml* file.
Mentioned java class is already present in sources.

```xml
<!-- Skip cert verification for devel purpose -->
<property name="baseEnvironmentProperties">
	<map>
		<entry key="java.naming.ldap.factory.socket" value="cz.muni.ucn.opsi.wui.security.BlindSSLSocketFactory"/>
	</map>
</property>
```

### License

* GNU GPL 3.0 for GXT framework
* Apache 2.0 for the rest of the code

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

- Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
- Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND
CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES,
INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS
BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.