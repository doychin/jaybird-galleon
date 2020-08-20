# How to use it

For the moment this project is not available on public maven repository, so you need to clone and build it locally.

After you have installed it in your local maven repository you can add it to your maven **pom.xml**.
Here is a sample configuration for **wildfly-jar-maven-plugin** 

```
    <configuration>
        <feature-packs>
            <feature-pack>
                <location>wildfly@maven(org.jboss.universe:community-universe)#${version.wildfly}</location>
                <inherit-configs>false</inherit-configs>
                <inherit-packages>false</inherit-packages>
            </feature-pack>
            <feature-pack>
                <groupId>org.firebirdsql</groupId>
                <artifactId>jaybird-galleon</artifactId>
                <version>1.0-SNAPSHOT</version>
                <inherit-configs>false</inherit-configs>
                <inherit-packages>false</inherit-packages>
            </feature-pack>
        </feature-packs>
        <layers>
            <layer>jaybird-datasource</layer>
        </layers>
        <excluded-layers>
            <layer>deployment-scanner</layer>
        </excluded-layers>
    </configuration>
```
## Configuring JDBC URL and data source name

There is a set of configuration parameters that can be provided as system properties during application startup or as environment variables.

There are two ways to configure connection URL. You can specify it as a whole.
```
JAYBIRD_URL - the URL to use to connect to the database
```
Example: **JAYBIRD_URL=jdbc:firebird://localhost:3052/documents**
Or you can configure individual parts of it:
```
JAYBIRD_MODE - specifies connection mode. Default value is firebirdsql. 
JAYBIRD_SERVICE_HOST_PORT - the host and port part of the JDBC url. Default is localhost/3050
JAYBIRD_DATABASE - The database name part of the URL
```
For more information about possible values for JAYBIRD_MODE check the documentation on 
 https://github.com/FirebirdSQL/jaybird/wiki/JDBC-URLs. 
 
This URL uses the deprecated form: 
 ```
jdbc:<JAYBIRD_MODE>:<JAYBIRD_SERVICE_HOST_PORT>:<JAYBIRD_DATABASE>
```

```
JAYBIRD_USER - user name to use for database connection. Default is 'sysdba'
JAYBIRD_PASSWORD - password to use for database connection. Default is 'masterkey'
```

To configure data source name you have to provide value for one of the below listed environment 
variables/system properties
```
JAYBIRD_DATASOURCE - the name of the datasource. Default value is JaybirdDS
OPENSHIFT_JAYBIRD_DATASOURCE - same as above but can be provided only as environment variable
```

## Configure your data source as default

If you want to use data source configured with **JAYBIRD_DATASOURCE** as default you have to replace **jaybird-datasource** layer with **jaybird-default-datasource** in your pom.xml.
```
....
        <layers>
            <layer>jaybird-default-datasource</layer>
        </layers>
.....
```