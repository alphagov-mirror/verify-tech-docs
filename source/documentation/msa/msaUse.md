Install the Matching Service Adapter
====================================

**Prerequisite:** The Matching Service Adapter requires Java 8. Make
sure you have the latest version of the JRE (Java Runtime Environment)
installed.

1.  When you have successfully completed a gate review for [Stage 3:
    Planning](http://alphagov.github.io/identity-assurance-documentation/stage3/Stage3.html),
    email
    [<idasupport+onboarding@digital.cabinet-office.gov.uk>](mailto:idasupport+onboarding@digital.cabinet-office.gov.uk)
    to request access to the secure site where you can download the
    Matching Service Adapter.
2.  Download the Matching Service Adapter to your host. It contains:

> -   a jar (java archive) file
> -   a truststore \<gloss\_truststore\> file and a metadata truststore
>     file for non-production environments (the SAML compliance tool and
>     the integration environment)
> -   a truststore file and a metadata truststore file for the
>     production environment

3.  To extract the files and move the truststore files to the
    environment where you want to use the Matching Service Adapter, run
    the following commands:

        tar -xf [filename].tar.gz
        unzip ida-msa-[xxx].zip
        mv prod_ida_truststore.ts prod_ida_metadata_truststore.ts [path to truststore directory in the production environment]
        mv test_ida_truststore.ts test_ida_metadata_truststore.ts [path to truststore directory in the integration environment]

> where:
>
> -   `[filename]` is the name of the tar file
> -   `[xxx]` is the build number
> -   `[path to truststore directory...]` is the location of the
>     truststore – you specify this when configuring the Matching
>     Service Adapter (see the configuration options `storeUri:` and
>     `trustStorePath:` in the YAML file \<yamlfile\>)

Versioning
----------

Typically, GOV.UK Verify releases a new version of the Matching Service
Adapter every 2 or 3 months. Some releases are essential updates, and we
may remove support for older versions. To keep updated, contact the
[GOV.UK Verify support
team](mailto:idasupport+onboarding@digital.cabinet-office.gov.uk) to
ensure you are on the Matching Service Adapter email distribution list.

Obtain certificates for your Matching Service Adapter
=====================================================

Your Matching Service Adapter needs a signing certificate and an
encryption certificate. You must use different certificates for each
environment.

To obtain certificates:

-   generate self-signed certificates \<samlCTselfsigncert\> for use
    with the compliance tool
-   request certificates \<pkiRequestCert\> from the IDAP certificate
    authority for the integration and production environments

Configure the Matching Service Adapter
======================================

Create a YAML configuration file
--------------------------------

When you start the Matching Service Adapter \<msa\_test\_msa\>, you need
to supply a YAML configuration file.

Create a YAML configuration file based on the example below. Then
adapt it \<msa\_adapt\_YAML\> for the SAML compliance tool or
appropriate environment.

    server:
     applicationConnectors:
       - type: http
         port: 50210
     adminConnectors:
       - type: http
         port: 50211
     requestLog:
       appenders:
         - type: file
           currentLogFilename: apps-home/msa.log
           archivedLogFilenamePattern: apps-home/msa.log.%d.gz
           logFormat: '%-5p [%d{ISO8601,UTC}] %c: %m%n%xEx'
         - type: logstash-file
           currentLogFilename: apps-home/logstash/msa.log
           archivedLogFilenamePattern: apps-home/logstash/msa.log.%d.gz
           archivedFileCount: 7
         - type: console

    logging:
     level: INFO
     appenders:
       - type: file
         currentLogFilename: apps-home/msa.log
         archivedLogFilenamePattern: apps-home/msa.log.%d.gz
         logFormat: '%-5p [%d{ISO8601,UTC}] %c: %X{logPrefix}%m%n%xEx'
       - type: logstash-file
         currentLogFilename: apps-home/logstash/msa.log
         archivedLogFilenamePattern: apps-home/logstash/msa.log.%d.gz
         archivedFileCount: 7
       - type: console
         logFormat: '%-5p [%d{ISO8601,UTC}] %c: %X{logPrefix}%m%n%xEx'

    assertionLifetime: 60m
    matchingServiceUri: http://local-matching-service.my.service.gov.uk/matching-request
    unknownUserCreationServiceUri: http://local-matching-service.my.service.gov.uk/unknown-user-request

    matchingServiceAdapterLocation: http://matching-service-adapter.my.service.gov.uk/matching-service/POST

    saml:
     entityId: http://matching-service-adapter.my.service.gov.uk/matching-service

    matchingServiceClient:
     timeout: 2s
     timeToLive: 10m
     cookiesEnabled: false
     connectionTimeout: 1s

    metadata:
     uri: https://www.[your-env].signin.service.gov.uk/SAML2/metadata/federation
     trustStorePath: [path to truststore directory...]/ida_metadata_truststore.ts
     trustStorePassword: puppet
     expectedEntityId: https://signin.service.gov.uk
     maxRefreshDelay: 100000
     minRefreshDelay: 10000
     client:
       timeout: 2s
       timeToLive: 10m
       cookiesEnabled: false
       connectionTimeout: 1s
       retries: 3
       keepAlive: 60s
       chunkedEncodingEnabled: false
       validateAfterInactivityPeriod: 5s
       tls:
         protocol: TLSv1.2
         verifyHostname: true
         trustSelfSignedCertificates: false

    hubSSOUri: https://www.[your-env].signin.service.gov.uk/SAML2/SSO

    requireHubCertificates: false

    serviceInfo:
     name: matching-service-adapter

    # If you would like the Matching Service Adapter to report to a Graphite instance.
    metrics:
     reporters:
       - type: graphite
         host: [graphite host]
         port: [graphite port]
         prefix: [some prefix of your choosing]
         frequency: 30s

    privateSigningKeyConfiguration:
     keyUri: [path to your keys directory...]/msa_signing.pk8

    privateEncryptionKeyConfiguration:
     keyUri: [path to your keys directory...]/msa_encryption.pk8

    publicSigningKeyConfiguration:
     keyUri: [path to your keys directory...]/msa_signing.crt
     keyName: http://matching-service-adapter.my.service.gov.uk/matching-service

    publicEncryptionKeyConfiguration:
     keyUri: [path to your keys directory...]/msa_encryption.crt
     keyName: http://matching-service-adapter.my.service.gov.uk/matching-service

    publicSecondarySigningKeyConfiguration
     keyUri: [path to your keys directory...]/msa_signing_secondary.crt
     keyName: http://matching-service-adapter.my.service.gov.uk/matching-service-secondary-sigining

    publicSecondaryEncryptionKeyConfiguration
     keyUri: [path to your keys directory...]/msa_encryption_secondary.crt
     keyName: http://matching-service-adapter.my.service.gov.uk/matching-service-secondary-encryption

    returnStackTraceInResponse: false

    clientTrustStoreConfiguration:
     storeUri: [path to truststore directory...]/ida_truststore.ts
     password: puppet

    featureFlagConfiguration:
     isCertificateChainValidationRequired: true
     encryptionDisabled: false

Adapt the YAML configuration file
---------------------------------

Make the following changes to the YAML configuration file. Variations
are indicated where appropriate for the SAML compliance tool and
integration and production environments.

1.  Enter port numbers for the server application and admin ports.

> > **note**
> >
> > If the Matching Service Adapter will be handling SSL termination
> > (typically this will be handled by a proxy/load balancer like
> > HAProxy), or if you don't trust the network between the SSL
> > termination endpoint and the Matching Service Adapter, then specify
> > `https` rather than `http` for the type of connection. For more
> > information, see the guidance in the [DropWizard configuration
> > manual](http://dropwizard.github.io/dropwizard/0.8.2/docs/manual/configuration.html#https).

2.  Enter the URIs for your matching service and Matching Service
    Adapter in `matchingServiceUri:` and
    `matchingServiceAdapterLocation:` respectively.
3.  If you're creating new user accounts when a match is not found (see
    ms\_cua), enter the user account creation URI in
    `unknownUserCreationServiceUri:`
4.  In `entity id`, enter the entityID for the Matching Service Adapter
    in URI format. You create your own URI, possibly to reflect the name
    of your service, for example: `https://<service name>/MSA`

> > **note**
> >
> > It's good practice to use the Matching Service Adapter's URI (i.e.
> > the URI where the hub will send Matching Requests) as its entity ID,
> > but this isn't mandatory.

5.  In `metadata:`, use the `uri:` parameter to specify the location
    where the Matching Service Adapter accesses the SAML metadata:

> -   for the SAML compliance tool, your GOV.UK Verify engagement lead
>     will give you the URL
> -   for the integration environment, enter:
>     `https://www.integration.signin.service.gov.uk/SAML2/metadata/federation`
> -   for the production environment, enter:
>     `https://www.signin.service.gov.uk/SAML2/metadata/federation`

6.  In `metadata:`, use the `trustStorePath:` parameter to specify the
    path to your metadata truststore for the appropriate environment:

> -   for the SAML compliance tool and the integration environment, use
>     `test_metadata_truststore.ts`
> -   for the production environment, use `prod_metadata_truststore.ts`

7.  In `clientTrustStoreConfiguration:`, use the `storeUri` parameter to
    specify the path of your general truststore for the appropriate
    environment:

> -   for the SAML compliance tool and the integration environment, use
>     `test_ida_truststore.ts`
> -   for the production environment, use `prod_ida_truststore.ts`

8.  Enter the paths of the SAML signing and encryption private keys for
    your Matching Service Adapter:

> -   `privateSigningKeyConfiguration:` – PKCS\#8 DER formatted
> -   `privateEncryptionKeyConfiguration:` – PKCS\#8 DER formatted
>
> > **note**
> >
> > To convert a private key to PKCS\#8 DER format, run the following
> > command:
> > `openssl pkcs8 -topk8 -nocrypt -in server.key -out server.pk8 -outform DER`
>
> For the compliance tool,
> generate self-signed certificates \<samlCTselfsigncert\>.
>
> You will use different keys and certificates for the integration and
> production environments. See pkiRequestCert.

9.  Enter the paths and names of the SAML signing and encryption
    certificates for your Matching Service Adapter. The names are used
    to identify the certificates in the metadata so should be meaningful
    and unique: `signing_1` and `encryption_1` would be acceptable.

> -   `publicSigningKeyConfiguration:` – public signing certificate, PEM
>     formatted
> -   `publicEncryptionKeyConfiguration:` – public encryption
>     certificate, PEM formatted

10. If you want to use Graphite software to monitor the Matching Service
    Adapter’s performance (optional), supply the required metrics: type,
    host, port, prefix and frequency of the reporter.

Start the Matching Service Adapter
==================================

To start using the Matching Service Adapter, run the following command,
supplying the path to your configuration file:

    java -jar [filename].jar server [path to configuration file].yml

You can now run
SAML compliance tests between the hub and your Matching Service Adapter \<samlCThubMSA\>.
To help build your local matching service \<msBuild\>, you can use the
example of the JSON request \<samlCT\_JSONeg\> that the Matching Service
Adapter posts to your service.

Monitoring
==========

Health checks run every 60 seconds to ensure that the Matching Service
Adapter is functioning correctly. They test:

-   connectivity
-   that the Matching Service Adapter accepts the hub signature
-   that the hub accepts the Matching Service Adapter signature

Configure HTTPS Proxies
=======================

The Matching Service Adapter supports HTTP and HTTPS proxies configured
by Java properties.

For information on configuring HTTPS proxies, see
[<http://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html>](http://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html).

Secure your Matching Service Adapter
====================================

Matching Service Adapter TLS certificates
-----------------------------------------

GOV.UK Verify trusts the following root certificate authorities for
HTTPS connections to your matching service:

  ------------------------------------------------------------------------
  Root certificate  Common name      X509v3 subject key identifier
  authority                          
  ----------------- ---------------- -------------------------------------
  AddTrust External AddTrust         AD:BD:98:7A:34:B4:26:F7:FA:C4:26:54:E
  CA Root           External CA Root F:03:BD:E0:24:CB:54:1A

  GeoTrust Global   GeoTrust Global  C0:7A:98:68:8D:89:FB:AB:05:64:0C:11:7
  CA                CA               D:AA:7D:65:B8:CA:CC:4E

  QuoVadis Root CA  QuoVadis Root CA 1A:84:62:BC:48:4C:33:25:04:D4:EE:D0:F
  2                 2                6:03:C4:19:46:D1:94:6B
  ------------------------------------------------------------------------

If you want to use a root certificate authority for your matching
service that isn’t in the above table, raise a ticket with us by sending
an email to <idasupport+onboarding@digital.cabinet-office.gov.uk>. We’ll
review your chosen root certificate authority before adding it to this
list.

When you raise a ticket, indicate the chain of trust with your SSL/TLS
certificate. You will also need the chain of trust when you configure
your server.

Connect your Matching Service Adapter to the internet securely
--------------------------------------------------------------

Your Matching Service Adapter must only respond to matching requests
from the GOV.UK Verify hub, otherwise there’s a risk of user data being
compromised.

The Matching Service Adapter checks that matching service requests are
genuine by checking their cryptographic signatures.

To ensure that only the GOV.UK Verify hub can access the Matching
Service Adapter, make sure your Matching Service Adapter:

-   is only exposed as HTTPS endpoints
-   only uses strong recent versions of TLS (for example TLS 1.2); turn
    off obsolete and insecure versions (for example SSLv1, SSLv2, and
    SSLv3)
-   supports multiple strong cipher suites

    > **note**
    >
    > GOV.UK Verify will remove support for TLS cipher suites if serious
    > weaknesses become known. Having multiple suites provides
    > resilience.

-   allows requests and health checks only from the IP addresses of hub
    services provided by your engagement lead

    > **note**
    >
    > Each Matching Service Adapter should communicate with only 1 hub
    > service (SAML compliance tool, integration environment, or
    > production environment).


