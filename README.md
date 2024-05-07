# Test



<?xml version="1.0" encoding="UTF-8" standalone="yes"?>

<Configuration xmlns="http://bbn.com/marti/xml/config">

    <network multicastTTL="5" version="5.1-RELEASE-11-HEAD">

        <input auth="anonymous" _name="input" protocol="tcp" port="8089" archive="true" anongroup="true" archiveOnly="false" coreVersion="2" coreVersion2TlsVersions="TLSv1.2,TLSv1.3"/>

        <connector port="8443" _name="https"/>

        <connector port="8099" _name="https"/>

        <connector port="8444" useFederationTruststore="true" _name="fed_https"/>

        <connector port="8446" clientAuth="false" _name="cert_https"/>

        <announce/>

    </network>

    <auth>

        <File location="UserAuthenticationFile.xml"/>

    </auth>

    <submission ignoreStaleMessages="false" validateXml="false"/>

    <subscription reloadPersistent="false"/>

    <repository enable="true" numDbConnections="200" primaryKeyBatchSize="500" insertionBatchSize="500">

        <connection url="jdbc:postgresql://127.0.0.1:5432/cot" username="martiuser" password="hi6YhSpnjnf0oma"/>

    </repository>

    <repeater enable="true" periodMillis="3000" staleDelayMillis="15000">

        <repeatableType initiate-test="/event/detail/emergency[@type='911 Alert']" cancel-test="/event/detail/emergency[@cancel='true']" _name="911"/>

        <repeatableType initiate-test="/event/detail/emergency[@type='Ring The Bell']" cancel-test="/event/detail/emergency[@cancel='true']" _name="RingTheBell"/>

        <repeatableType initiate-test="/event/detail/emergency[@type='Geo-fence Breached']" cancel-test="/event/detail/emergency[@cancel='true']" _name="GeoFenceBreach"/>

        <repeatableType initiate-test="/event/detail/emergency[@type='Troops In Contact']" cancel-test="/event/detail/emergency[@cancel='true']" _name="TroopsInContact"/>

    </repeater>

    <filter>

        <thumbnail/>

        <urladd host="http://127.0.1.1:8080"/>

        <flowtag enable="true" text=""/>

        <streamingbroker enable="true"/>

        <scrubber enable="false" action="overwrite"/>

        <qos>

            <deliveryRateLimiter enabled="true">

                <rateLimitRule clientThresholdCount="500" reportingRateLimitSeconds="200"/>

                <rateLimitRule clientThresholdCount="1000" reportingRateLimitSeconds="300"/>

                <rateLimitRule clientThresholdCount="2000" reportingRateLimitSeconds="400"/>

                <rateLimitRule clientThresholdCount="5000" reportingRateLimitSeconds="800"/>

                <rateLimitRule clientThresholdCount="10000" reportingRateLimitSeconds="1200"/>

            </deliveryRateLimiter>

            <readRateLimiter enabled="false">

                <rateLimitRule clientThresholdCount="500" reportingRateLimitSeconds="200"/>

                <rateLimitRule clientThresholdCount="1000" reportingRateLimitSeconds="300"/>

                <rateLimitRule clientThresholdCount="2000" reportingRateLimitSeconds="400"/>

                <rateLimitRule clientThresholdCount="5000" reportingRateLimitSeconds="800"/>

                <rateLimitRule clientThresholdCount="10000" reportingRateLimitSeconds="1200"/>

            </readRateLimiter>

            <dosRateLimiter enabled="false" intervalSeconds="60">

                <dosLimitRule clientThresholdCount="1" messageLimitPerInterval="60"/>

            </dosRateLimiter>

        </qos>

    </filter>

    <buffer>

        <queue>

            <priority/>

        </queue>

        <latestSA enable="true"/>

    </buffer>

    <dissemination smartRetry="false"/>

    <security>

        <tls keystore="JKS" keystoreFile="certs/files/takserver.jks" keystorePass="atakatak" truststore="JKS" truststoreFile="certs/files/truststore-root.jks" truststorePass="atakatak" context="TLSv1.2" keymanager="SunX509"/>

    </security>

    <federation allowFederatedDelete="false" allowMissionFederation="true" allowDataFeedFederation="true" enableMissionFederationDisruptionTolerance="true" missionFederationDisruptionToleranceRecencySeconds="43200" enableFederation="false" enableDataPackageAndMissionFileFilter="false">

        <federation-server port="9000" coreVersion="2" v1enabled="false" v2port="9001" v2enabled="false" webBaseUrl="https://127.0.1.1:8443/Marti">

            <tls keystore="JKS" keystoreFile="certs/files/takserver.jks" keystorePass="atakatak" truststore="JKS" truststoreFile="certs/files/fed-truststore.jks" truststorePass="atakatak" context="TLSv1.2" keymanager="SunX509"/>

            <federation-port port="9000" tlsVersion="TLSv1.2"/>

            <v1Tls tlsVersion="TLSv1.2"/>

            <v1Tls tlsVersion="TLSv1.3"/>

        </federation-server>

        <fileFilter>

            <fileExtension>pref</fileExtension>

        </fileFilter>

    </federation>

    <plugins/>

    <cluster/>

    <vbm enabled="false"/>

</Configuration>
