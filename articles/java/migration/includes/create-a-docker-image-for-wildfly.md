---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 6d6f0f701e9507f5dee065c14b48b30be423e100
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81672848"
---
### <a name="create-a-docker-image-for-wildfly"></a>WildFly 用の Docker イメージを作成する

Dockerfile を作成するには、次の前提条件が必要です。

* サポートされている JDK
* WildFly のインストール
* JVM ランタイム オプション。
* 環境変数を渡す方法 (該当する場合)。

その後、以下のセクションで説明されている手順を適宜実行できます。 Dockerfile と Web アプリケーションの開始点として [WildFly コンテナーのクイック スタート リポジトリ](https://github.com/Azure/wildfly-container-quickstart)を使用できます。

1. [KeyVault FlexVolume を構成する](#configure-keyvault-flexvolume)
2. [データ ソースを設定する](#set-up-data-sources)
3. [JNDI リソースを設定する](#set-up-jndi-resources)
4. [WildFly の構成を確認する](#review-wildfly-configuration)

#### <a name="configure-keyvault-flexvolume"></a>KeyVault FlexVolume を構成する

Azure KeyVault を作成し、必要なすべてのシークレットを設定します。 詳細については、「[クイック スタート: Azure CLI を使用して Azure Key Vault との間でシークレットの設定と取得を行う](/azure/key-vault/quick-create-cli)」を参照してください。 次に、[KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md) を構成して、これらのシークレットにポッドがアクセスできるようにします。

WildFly のブートストラップに使用されるスタートアップ スクリプトを更新する必要もあります。 このスクリプトでは、サーバーを起動する前に、WildFly によって使用されるキーストアに証明書をインポートする必要があります。

#### <a name="set-up-data-sources"></a>データ ソースを設定します

データ ソースにアクセスするように WildFly を構成するには、JDBC ドライバー JAR を Docker イメージに追加してから、適切な JBoss CLI コマンドを実行する必要があります。 これらのコマンドでは、Docker イメージをビルドするときにデータ ソースを設定する必要があります。

次の手順では、PostgreSQL、MySQL、SQL Server について説明します。

1. [PostgreSQL](https://jdbc.postgresql.org/download.html)、[MySQL](https://dev.mysql.com/downloads/connector/j/)、または [SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) 用の JDBC ドライバーをダウンロードします。

    ダウンロードしたアーカイブをアンパックして、ドライバーの .jar ファイルを取得します。

1. `module.xml` のような名前のファイルを作成し、次のマークアップを追加します。 `<module name>`プレースホルダー (山かっこを含む) を、`org.postgres` (PostgreSQL の場合)、`com.mysql` (MySQL の場合)、または `com.microsoft` (SQL Server の場合) に置き換えます。 `<JDBC .jar file path>` を、前のステップの .jar ファイルの名前に置き換えます。ご自分の Docker イメージ内のファイルを配置する場所への完全なパス (`/opt/database` など) を含めます。

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

1. `datasource-commands.cli` のような名前のファイルを作成し、次のコードを追加します。 `<JDBC .jar file path>` を前のステップで使った値に置き換えます。 `<module file path>` を、前のステップのファイル名とパス (`/opt/database/module.xml` など) に置き換えます。

    **PostgreSQL**

    ```console
    batch
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)
    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker
    reload
    run batch
    shutdown
    ```

    **MySQL**

    ```console
    batch
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)
    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter
    reload
    run batch
    shutdown
    ```

    **SQL Server**

    ```console
    batch
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>
    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)
    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter
    reload
    run batch
    shutdown
    ```

1. アプリケーションの JTA データソース構成を更新します。

    アプリの `src/main/resources/META-INF/persistence.xml` ファイルを開き、`<jta-data-source>` 要素を見つけます。 その内容を次に示すように置き換えます。

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

1. Docker イメージをビルドするときにデータ ソースが作成されるように、以下を `Dockerfile` に追加します

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/database/datasource-commands.cli && \
    sleep 30
    ```

1. 使用する `DATABASE_CONNECTION_URL` はデータベース サーバーごとに異なり、Azure portal 上の値とは異なるため、これを特定します。 WildFly では、ここで示す URL の形式を使う必要があります。

    **PostgreSQL**

    ```console
    jdbc:postgresql://<database server name>:5432/<database name>?ssl=true
    ```

    **MySQL**

    ```console
    jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT
    ```

    **SQL Server**

    ```console
    jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

1. 後の段階でデプロイ YAML を作成するときに、適切な値と共に環境変数 `DATABASE_CONNECTION_URL`、`DATABASE_SERVER_ADMIN_FULL_NAME`、および `DATABASE_SERVER_ADMIN_PASSWORD` を渡す必要があります。

WildFly でのデータベース接続の構成について詳しくは、[PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7)、[MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource)、または [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898) をご覧ください。

#### <a name="set-up-jndi-resources"></a>JNDI リソースを設定する

WildFly 上で構成する必要がある各 JNDI リソースを設定するには、一般に次の手順を使用します。

1. 必要な JAR ファイルをダウンロードし、Docker イメージにコピーします。
2. これらの JAR ファイルを参照する WildFly *module.xml* ファイルを作成します。
3. 特定の JNDI リソースに必要な構成を作成します。
4. Docker ビルド中に JNDI リソースを登録するために使用する JBoss CLI スクリプトを作成します。
5. Dockerfile にすべてのものを追加します。
6. デプロイ YAML に適切な環境変数を渡します。

次の例は、Azure Service Bus への JMS 接続用の JNDI リソースを作成するために必要な手順を示しています。

1. [Apache Qpid JMS プロバイダー](https://qpid.apache.org/components/jms/index.html)をダウンロードします

    ダウンロードしたアーカイブをアンパックして、.jar ファイルを取得します。

1. `module.xml` のような名前のファイルを作成し、次のマークアップを `/opt/servicebus` に追加します。 JAR ファイルのバージョン番号が、前のステップの JAR ファイルの名前と一致していることを確認します。

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="org.jboss.genericjms.provider">
     <resources>
      <resource-root path="proton-j-0.31.0.jar"/>
      <resource-root path="qpid-jms-client-0.40.0.jar"/>
      <resource-root path="slf4j-log4j12-1.7.25.jar"/>
      <resource-root path="slf4j-api-1.7.25.jar"/>
      <resource-root path="log4j-1.2.17.jar"/>
      <resource-root path="netty-buffer-4.1.32.Final.jar" />
      <resource-root path="netty-codec-4.1.32.Final.jar" />
      <resource-root path="netty-codec-http-4.1.32.Final.jar" />
      <resource-root path="netty-common-4.1.32.Final.jar" />
      <resource-root path="netty-handler-4.1.32.Final.jar" />
      <resource-root path="netty-resolver-4.1.32.Final.jar" />
      <resource-root path="netty-transport-4.1.32.Final.jar" />
      <resource-root path="netty-transport-native-epoll-4.1.32.Final-linux-x86_64.jar" />
      <resource-root path="netty-transport-native-kqueue-4.1.32.Final-osx-x86_64.jar" />
      <resource-root path="netty-transport-native-unix-common-4.1.32.Final.jar" />
      <resource-root path="qpid-jms-discovery-0.40.0.jar" />
     </resources>
     <dependencies>
      <module name="javax.api"/>
      <module name="javax.jms.api"/>
     </dependencies>
    </module>
    ```

1. `jndi.properties` ファイルを `/opt/servicebus` に作成します。

    ```console
    connectionfactory.${MDB_CONNECTION_FACTORY}=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}
    queue.${MDB_QUEUE}=${SB_QUEUE}
    topic.${MDB_TOPIC}=${SB_TOPIC}
    ```

1. `servicebus-commands.cli` のような名前のファイルを作成し、次のコードを追加します。

    ```console
    batch
    /subsystem=ee:write-attribute(name=annotation-property-replacement,value=true)
    /system-property=property.mymdb.queue:add(value=myqueue)
    /system-property=property.connection.factory:add(value=java:global/remoteJMS/SBF)
    /subsystem=ee:list-add(name=global-modules, value={"name" => "org.jboss.genericjms.provider", "slot" =>"main"}
    /subsystem=naming/binding="java:global/remoteJMS":add(binding-type=external-context,module=org.jboss.genericjms.provider,class=javax.naming.InitialContext,environment=[java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory,org.jboss.as.naming.lookup.by.string=true,java.naming.provider.url=/opt/servicebus/jndi.properties])
    /subsystem=resource-adapters/resource-adapter=generic-ra:add(module=org.jboss.genericjms,transaction-support=XATransaction)
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:add(class-name=org.jboss.resource.adapter.jms.JmsManagedConnectionFactory, jndi-name=java:/jms/${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=ConnectionFactory:add(value=${MDB_CONNECTION_FACTORY})
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=JndiParameters:add(value="java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory;java.naming.provider.url=/opt/servicebus/jndi.properties")
    /subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:write-attribute(name=security-application,value=true)
    /subsystem=ejb3:write-attribute(name=default-resource-adapter-name, value=generic-ra)
    run-batch
    reload
    shutdown
    ```

1. Docker イメージをビルドするときに JNDI ソースが作成されるように、以下を `Dockerfile` に追加します

    ```console
    RUN /bin/bash -c '<WILDFLY_INSTALL_PATH>/bin/standalone.sh --start-mode admin-only &' && \
    sleep 30 && \
    <WILDFLY_INSTALL_PATH>/bin/jboss-cli.sh -c --file=/opt/servicebus/servicebus-commands.cli && \
    sleep 30
    ```

1. 後の段階でデプロイ YAML を作成するときに、適切な値と共に環境変数 `MDB_CONNECTION_FACTORY`、`DEFAULT_SBNAMESPACE`、`SB_SAS_POLICY`、`SB_SAS_KEY`、`MDB_QUEUE`、`SB_QUEUE`、`MDB_TOPIC`、および `SB_TOPIC` を渡す必要があります。

#### <a name="review-wildfly-configuration"></a>WildFly の構成を確認する

「[WildFly 管理者ガイド](https://docs.wildfly.org/18/Admin_Guide.html)」を確認して、前のガイダンスで説明されていないその他の移行前手順をカバーします。
