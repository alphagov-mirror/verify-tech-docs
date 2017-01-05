# Public Key Infrastructure

The purpose of a public key infrastructure (PKI) is to implement secure
electronic transactions over insecure networks such as the internet. A
PKI is used to authenticate identities for the purposes of data
encryption and signing:

* encryption – scrambles the data in a way that makes it unreadable except to intended recipients
* signing – proves who authored the data and guarantees that it hasn't been tampered with since

Encryption and signing are provided by public-key cryptography. A PKI
supports public-key cryptography by assuring the identities of the
entities that encrypt and sign data. It does this by issuing digital
certificates. A PKI is therefore used to set up and maintain a network
of trusted entities and services. This means that when you send
encrypted data you can be certain that only the intended recipient can
decrypt it and that when you receive signed data you can be certain who
authored it.

**Why does GOV.UK Verify use a PKI?**

The Identity Assurance Programme (IDAP) runs a PKI to enable secure
communication between the entities in the GOV.UK Verify federation, for
example, between government services and the [GOV.UK Verify hub](#what-does-the-gov-uk-verify-hub-do).
For an outline of the technical steps in the onboarding process, see
[Stage 4: Build and integration
testing](http://alphagov.github.io/identity-assurance-documentation/stage4/Stage4.html)
in the onboarding guide.

The entities in the GOV.UK Verify federation communicate with each other
using SAML. Public-key
cryptography secures the integrity and privacy of SAML messages sent
between the different entities.

**What do you need to do?**

As part of the GOV.UK Verify federation you need to [request certificates](#request-certificates) from the IDAP PKI certificate
authority. When your certificates are due to expire you need to run the [key rotation process](#rotate-your-keys) to update the keys in your
certificates.

For more information, see [Steps to integrate GOV.UK Verify into your service](#steps-to-integrate-gov-uk-verify-into-your-service).

You are responsible for ensuring that the terms of the IDAP PKI
Certification Policy are upheld. To do this, you and your service
manager need to refer to a set of documents. Your service manager must
request these documents from the IDAP PKI:

* **Certification Practice Statement for the Interim PKI for the IDAP Ecosystem** – sets out the practices governing cryptographic services for the IDAP federation
* **IDAP PKI Subscriber Agreement** – sets out the terms of use of PKI for those using certificates received from the IDAP PKI
* **IDAP PKI Relying Party Agreement** – sets out the terms for those who do not necessarily hold a certificate, but who, during the course of a transaction, may be a recipient of a certificate and place reliance on a certificate and/or digital signatures created using that certificate
* **GOV.UK Verify Certification Process for (Relying Party) Subscribers** – indicates the URLs where you can submit certificate requests to the IDAP certificate authority

