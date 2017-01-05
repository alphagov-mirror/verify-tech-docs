# Glossary of terms


<a name="glossary-assertion"></a>

**assertion**

A package of data created by an entity in the GOV.UK federation. Data in an assertion is signed by the creator of the assertion (for example, an identity provider or the [Matching Service Adapter](#glossary-MSA)) and encrypted for the recipient (for example, the [hub](#glossary-hub)).

<a name="glossary-assured-identity"></a>

**assured identity**

An identity that's been verified to the required [level of assurance](#glossary-level-of-assurance).

<a name="glossary-attributes"></a>

**attributes**

Data that's sent from one entity to another in a SAML [assertion](#glossary-assertion), for example, the [matching dataset](#glossary-matching-dataset) for use by the matching service.

<a name="glossary-authentication"></a>

**authentication** 

The process by which a system confirms the user is known to that system, usually through the use of 1 or more [credentials](#glossary-credentials).

<a name="glossary-authentication-request"></a>

**authentication request** 

The request that a service sends to the [hub](#glossary-hub), or the hub sends to an identity provider, to request the identification of a user trying to access a digital service.

<a name="glossary-certificate"></a>

**certificate**

A file that is issued and signed by a certificate authority. It contains a copy of the certificate owner’s public key, used for encryption or signing.

See [encryption certificate](#glossary-encryption-certificate) and [signing certificate](#glossary-signing-certificate).

<a name="glossary-certificate-authority"></a>

**certificate authority** 

An entity that issues and signs digital certificates within [public key infrastructure](#glossary-PKI) (PKI). The certificate authority acts as the [trust anchor](#glossary-trust-anchor) in the PKI.

<a name="glossary-certificate-signing-request-file"></a>

**certificate signing request file**

A file that contains all the information a certificate authority requires in order to issue a signed certificate.


<a name="glossary-compliance-tool"></a>

**compliance tool**

Enables services to carry out testing of SAML compliance before testing end-to-end user journeys in the [integration environment](#glossary-integration-environment). It allows isolated testing of both the connection from the service to the [hub](#glossary-hub) and the connection from the hub to the [Matching Service Adapter](#glossary-MSA).

<a name="glossary-credentials"></a>

**credentials** 

An identity provider issues credentials to a user to allow the user to be authenticated when accessing a government service. Examples of credentials are usernames, passwords and security codes.

<a name="glossary-cryptography"></a>

**cryptography** 

A set of techniques for guaranteeing the integrity and confidentiality of data transmitted over a public network. This is done by a combination of encryption and signing.

See [encryption certificate](#glossary-encryption-certificate) and [signing certificate](#glossary-signing-certificate).

<a name="glossary-encryption-certificate"></a>

**encryption certificate**

Contains the receiver’s public key that the sender uses to encrypt a message. The receiver decrypts the message using their corresponding private key. This provides the sender with assurance that only the receiver can decrypt the message.

<a name="glossary-entityID"></a>

**entityID**

A unique identifier for each entity within the GOV.UK Verify federation. Government services, Matching Service Adapters, the GOV.UK Verify [hub](#glossary-hub), and identity providers all have their own entityIDs. The entityID is used within messages and metadata to refer to an entity. The entityID is formatted as a URL but is not necessarily resolvable. For example, the entityID of the GOV.UK Verify hub is: https://signin.service.gov.uk.

<a name="glossary-data-matching"></a>

**data matching**

The process of finding a local identifier through matching that's useful to a government service when completing a transaction, for example confirming a National Insurance number so the user can amend their tax records.

<a name="glossary-Data-Protection-Act"></a>

**Data Protection Act 1998**

The Data Protection Act controls how personal information is used by organisations, businesses or the government. Read more about the [Data Protection Act](https://www.gov.uk/data-protection/the-data-protection-act).

<a name="glossary-DCS"></a>

**document checking service (DCS)**

A service supplementary to GOV.UK Verify that allows identity providers to validate user documents against government records.

<a name="glossary-GPG"></a>

**Good Practice Guides (GPGs 43, 44, 45, 46 and 56 in relation to identity assurance)**

Documents that have been developed collaboratively with HMG departments, private sector representatives and the UK National Technical Authority for Information Assurance to ensure that the business, technical and security demands across the sectors can be met. The guides are intended to ensure that the delivery of trusted online user transactions will take place in accordance with the [Identity Assurance Principles](#glossary-identity-assurance-principles).

**Good Practice Guide 43 (GPG 43)**

Also known as Requirements for Secure Delivery of Online Public Services (RSDOPS). This document sets out an approach to determining the components needed to securely deliver public services online to individuals and businesses.


<a name="glossary-government-service"></a>

**government service**

A service provided by a government department such as Check or update your company car tax (HMRC) or Claim your redundancy payment (BIS), that needs proof of a user’s identity to complete a transaction.

<a name="glossary-hashed-PID"></a>

**hashed persistent identifier**

A unique identifier which refers to a combination of:

* a user
* the identity provider that verified the user’s identity
* the government service that the user is trying to access.

The [Matching Service Adapter](#glossary-MSA) generates the hashed persistent identifier from the [persistent identifier](#glossary-persistent-identifier). This ensures that identifiers for user identity are unique to specific services and can’t be used across multiple services.

<a name="glossary-hub"></a>

**hub (hub service, GOV.UK Verify hub)** 

The infrastructure that manages interactions between users, government services, identity providers, and matching services for the purpose of authenticating a user who wants to use a government service. The [hub](#glossary-hub) protects privacy and ensures security during [authentication](#glossary-authentication).

<a name="glossary-identity"></a>

**identity**

In the case of identity assurance, this is the description of who or what an entity is, defined by a collection of [attributes](#glossary-attributes). 

<a name="glossary-identity-assurance"></a>

**identity assurance**

The ability to prove, to a certain level of confidence, that a user trying to access a digital service is who they say they are.

<a name="glossary-identity-assurance-principles"></a>

**Identity Assurance Principles**

The [Identity Assurance Principles](https://www.gov.uk/government/consultations/draft-identity-assurance-principles/privacy-and-consumer-advisory-group-draft-identity-assurance-principles#the-nine-identity-assurance-principles) set out how the government's identity assurance approach should be configured to meet the privacy and consumer expectations of its users. They are published by the [Privacy and Consumer Advisory Group](#glossary-privacy-consumer-advisory-group) and are amended or replaced from time to time.


<a name="glossary-identity-provider"></a>

**identity provider**

Private sector organisations, paid by the government, to verify that a user is who they say they are and assert verified data that identifies them to a government service.

The organisations are certified as meeting relevant industry security standards and identity assurance standards published by the Cabinet Office and the [National Cyber Security Centre](https://www.ncsc.gov.uk/) (NCSC).

<a name="glossary-integration-environment"></a>

**integration environment** 

A production-sized deployment of the entire GOV.UK Verify [hub](#glossary-hub) environment. This allows services to test full end-to-end flows and to showcase a working system including happy path and failure scenarios.

<a name="glossary-level-of-assurance"></a>

**level of assurance (LOA)**

The degree of confidence the government service requires that a user is who they say they are.

**level of assurance 1 (LOA1)**

Used when a government service needs to know that it's the same user returning to the service but doesn't need to know who that user is.

<a name="glossary-level-of-assurance-2"></a>

**level of assurance 2 (LOA2)**

Used when a government service needs to know on the balance of probabilities who the user is and that that they are a real person.

**level of assurance 3 (LOA3)**

Used when a government service needs to know beyond reasonable doubt who the user is and that that they are a real person.

**level of assurance 4 (LOA4)**

As level of assurance 3, but with a biometric profile captured at the point of registration.

<a name="glossary-local-matching-datastore"></a>

**local matching datastore** 

A datastore that is part of a government service’s local matching service. It stores correlations between hashed persistent identifiers and local identifiers for users.

<a name="glossary-local-matching-service"></a>

**local matching service** 

The part of a government service's matching service that runs matching cycles to find a match between a user’s [assured identity](#glossary-assured-identity) and a record in the government service’s data sources. This allows the government service to interact with the user. Government services build their own local matching service. 

<a name="glossary-matching-cylcles"></a>

**matching cycles**

Matching cycle numbers refer to the sequence of attempts to find a match between a user’s [assured identity](#glossary-assured-identity) and a record in a government service’s data sources.

**matching cycle 0**

Cycle 0 allows the matching service to match the hashed [hashed persistent identifier](#glossary-hashed-PID) of a verified user to a hashed persistent identifier held in their [local matching datastore](#glossary-local-matching-datastore). Cycle 0 is used for efficiency. A successful match at cycle 0 can be treated by the matching service as confirmation that the identity being presented is the same identity as previously matched by that service.

**matching cycle 1**

Checks if there’s a match for the user’s identity in the government service’s data sources, using the [matching dataset](#glossary-matching-dataset) to search for a match.

**matching cycle 2**

Additional matching cycle where trusted [attributes](#glossary-attributes) are used to enhance the matching process. Cycle 2 is currently not supported by GOV.UK Verify.


**matching cycle 3**

Asks the user for some additional information, for example, driving licence number, to complete a match. This cycle enhances cycle 1 and may not be required for all matches.

<a name="glossary-matching-dataset"></a>

**matching dataset**

A dataset containing a verified user’s:

* first name
* middle name (if provided)
* surname
* address
* date of birth
* gender (optional)

It may also contain historical values for these attributes. 

The [identity provider](#glossary-identity-provider) that verified the user's identity provides the matching dataset. The identity provider verifies the information in the matching dataset (except gender). The hub forwards the matching dataset to the government service’s [matching-service](#glossary-matching-service) so the government service can perform matching.

It’s optional for users to provide gender. Where provided, gender is not verified by the identity provider. It’s used for matching purposes only.

<a name="glossary-matching-service"></a>

**matching service**

The function of finding a local identifier for the individual (for example, an id for the user’s account) based on a match between a user’s verified identity and a record in a government service’s data sources. The matching service is composed of the Matching Service Adapter and the [local matching-service](#glossary-local-matching-service). In the SAML profile used by GOV.UK Verify, the matching service is referred to as the service provider matching service (SPMS). 

<a name="glossary-MSA"></a>

**Matching Service Adapter** 

A software tool provided by GOV.UK Verify that’s installed within the government service’s security domain. The Matching Service Adapter handles SAML requests for matching queries and sends responses to the GOV.UK Verify hub. The Matching Service Adapter and the [local matching-service](#glossary-local-matching-service) together comprise the matching service.


**onboarding**

The process through which new government services integrate with GOV.UK Verify. The onboarding process is divided into 6 stages. Each stage has a defined set of outputs which a service must meet before moving to the next stage. Onboarding ends with access to the full production environment where the government service deploys their live service.


<a name="glossary-persistent-identifier"></a>

**persistent identifier** 

A unique identifier which refers to a combination of:

* a user 
* the identity provider that verified the user’s identity

Identity providers generate persistent identifiers. They use pseudo-random values that have no discernible correspondence with the user’s actual identifier, for example, their email address.

See [hashed persistent identifier](#glossary-hashed-PID).

<a name="glossary-privacy-consumer-advisory-group"></a>

**Privacy and Consumer Advisory Group (PCAG)**

Established to help government develop an approach to identity assurance and develop the [Identity Assurance Principles](#glossary-identity-assurance-principles).

**private beta**

The initial version of a government service that is available to a small number of selected users, so a service can test and develop it further before rolling it out to a wider audience. Users have access through pre-approved tokens or personalised invitation.

<a name="glossary-private-key"></a>

**private key**

A long string of data used in [cryptography](#glossary-cryptography). Generating a certificate signing request creates a corresponding [public key](#glossary-public-key).

**public beta**

The full end-to-end version of a government service that is publicly available, to allow further testing and development.

<a name="glossary-PKI"></a>

**public key infrastructure (PKI)**

The purpose of a public key infrastructure is to implement secure electronic transactions over insecure networks such as the internet. It is used to authenticate identities for the purposes of data encryption and signing.

See [private key](#glossary-private-key) and [public key](#glossary-public-key).


<a name="glossary-public-key"></a>

**public key**

A long string of data derived from a private key. Data encrypted with a public key can only be decrypted with the corresponding private key, and vice versa.


<a name="glossary-relying-party"></a>

**relying party**

An entity in a system of federated identity that relies on a response from another entity, such as a successful [authentication request](#glossary-authentication-request). A relying party is also referred to as a service provider when it provides a service that the end user directly interacts with. Relying party is a term used in SAML specifications. In the GOV.UK Verify federation, relying parties are government services.

<a name="glossary-SAML"></a>

**SAML (Security Assertion Markup Language)**

An Extensible Markup Language (XML) open standard for the exchange of authentication and authorisation data between parties such as identity providers and government services. [OASIS](<https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security) governs the SAML standards. 

<a name="glossary-SAML-profile"></a>

**SAML profile**

A profile that specifies how to use SAML within a system of federated identity. The SAML profile for the GOV.UK Verify federation defines the SAML messages that are passed between entities within the federation and the rules for their use. SAML messages take the form of requests and responses, and can contain SAML assertions. The profile is built upon core SAML profiles as defined by [OASIS](<https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security).


**service provider matching service (SPMS)**

See [matching-service](#glossary-matching-service).


<a name="glossary-signing-certificate"></a> 

**signing certificate**

Contains a public key that the receiver uses to check the authenticity of a message signed with the sender’s private key. This provides the receiver with assurance of the sender’s identity.

<a name="glossary-SOAP"></a>

**Simple Object Access Protocol (SOAP)** 

A messaging protocol that allows programs that run on different operating systems to communicate.

<a name="glossary-TLS"></a>

**Transport Layer Security (TLS)**

A protocol used to provide secure communications across the internet, to websites or between services. A site using TLS has a URL beginning `https://`

<a name="glossary-URI"></a>

**Uniform Resource Identifier (URI)**

A character string that identifies a resource, for example an electronic document, an image, a source of information or a service. A URI could be a name, a location or both.

<a name="glossary-trust-anchor"></a>

**trust anchor**

An authoritative entity in cryptographic systems for which trust is pre-established and not derived. For example, the IDAP [certificate authority](#glossary-certificate-authority) is the trust anchor within the GOV.UK Verify federation.

<a name="glossary-truststore"></a>

**truststore**

A file containing certificates from parties that you expect to communicate with, or from certificate authorities that you trust to identify other parties.

<a name="glossary-URL"></a>

**Uniform Resource Locator (URL)**

A character string that identifies a resource by its location. A URL is typically a web address, particularly when used with http. A URL is held to be a subset of a URI, as it provides a way of identifying the resource by describing how to locate it (for example, its network location).



**user**

The person accessing the government service on GOV.UK.


<a name="glossary-whitelist"></a>

**whitelist (for IP addresses)**

A list or register of those permitted access. Whitelisting is the reverse of blacklisting, the practice of identifying those that are denied access, typically used in spam filters in email applications. A whitelist of IP (Internet Protocol) addresses allows data-traffic from / to those IP addresses to access a network by appropriate firewall configuration.



