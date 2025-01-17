---
title: Working with Oracle in Metabase
redirect_from:
  - /docs/latest/administration-guide/databases/oracle
---

# Working with Oracle in Metabase

## Downloading the Oracle JDBC Driver JAR

You can download a JDBC driver from [Oracle's JDBC driver downloads page](https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html).

The minimum driver version should be 19c, regardless of which Java version or Oracle Database version you have.

We recommend using the `ojdbc8.jar` JAR.

## Adding the Oracle JDBC Driver JAR to the Metabase plugins directory

In your Metabase directory (the directory where you keep and run your metabase.jar), create a directory called `plugins` (if it doesn't already exist.

Move the JAR you just downloaded (`ojdbc8.jar`) into the plugins directory, and restart Metabase. Metabase will automatically make the Oracle driver available when it starts back up. 

## Connecting with SSL

To connect to Oracle via SSL and enable encryption, check the `Use a secure connection (SSL)` option on the connection setup page. You can add other SSL features (including client and/or server authentication) as explained below. You can
use both client and server authentication (known as mutual authentication).
 
### Server authentication with a truststore

To configure the client (Metabase) to authenticate the identity of the server (the Oracle server), you may need to
configure a truststore file that includes the server's root CA, so that the JVM running Metabase trusts its
certificate chain. Refer to the
[Oracle documentation](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) on using `keytool` to manage key and truststore files, importing certificates, etc.

For more information on setting up a truststore for AWS RDS Oracle instances, see the
[instructions provided by Amazon](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.SSL.html#Appendix.Oracle.Options.SSL.JDBC).

If you need to connect to other databases using SSL, instead of creating a new truststore, you'll probably want to add the RDS CA to your existing truststore file (likely called `cacerts`).

### Client authentication with a keystore

To configure the server (the Oracle server) to authenticate the identity of the client (Metabase), you need to
configure a keystore file that includes the client's private key. You'll import the client's private key into the keystore (rather than a root CA into a truststore file). Add the following JVM options for Metabase:

```
-Djavax.net.ssl.keyStore=/path/to/keystore.jks
-Djavax.net.ssl.keyStoreType=JKS \
-Djavax.net.ssl.keyStorePassword=<keyStorePassword>
```

With this done, the Oracle server will authenticate Metabase using the private key when Metabase tries to connect over SSL.

## When running from a JAR

By default, the plugins directory is called `plugins`, and lives in the same directory as the Metabase JAR.

For example, if you're running Metabase from a directory called `/app/`, you should move the Oracle JDBC driver JAR to `/app/plugins/`:

```bash
# example directory structure for running Metabase with Oracle support
/app/metabase.jar
/app/plugins/ojdbc8.jar
```

## When running from Docker

The process for adding plugins when running via Docker is similar, but you'll need to mount the `plugins` directory. Refer to instructions [here](../../operations-guide/running-metabase-on-docker.html#adding-external-dependencies-or-plugins) for more details.
