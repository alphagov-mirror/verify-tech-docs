##  Generate self-signed certificates


To communicate with GOV.UK Verify you need to use keys and certificates. If you're communicating with the GOV.UK Verify hub, your certificates
must be <a href="#request-certificates">issued by the IDAP certificate authority</a>. If you're communicating with the compliance tool, generate self-signed certificates as described here.

### Generate self-signed certificates with OpenSSL

You can generate keys and self-signed certificates in whatever way is most convenient and familiar for you.

The GOV.UK Verify team generally use the OpenSSL tool to do this using the guidance from the [Heroku Dev Center](<https://devcenter.heroku.com/articles/ssl-certificate-self#prerequisites>`_). 


Install OpenSSL if it isn't already installed:

* OS X (with homebrew) - `brew install openssl`
* Windows - download the "Complete package, except sources" from [http://gnuwin32.sourceforge.net/packages/openssl.htm](http://gnuwin32.sourceforge.net/packages/openssl.htm). 
* Ubuntu Linux - `apt-get install openssl`

Run the following commands in order, replacing:

* `$name` with the filename of the key or certificate you're generating - see [Keys and certificates for your MSA](#keys-and-certificates-for-your-msa) and [Keys and certificates for your service](#keys-and-certificates-for-your-service)
* `$commonName` with a description of how the certificate will be used (for example "My Service MSA Signing Primary").

```
    # Generate a private key:
    openssl genrsa -des3 -passout pass:x -out "$name.pass.key" 2048
    openssl rsa -passin pass:x -in "$name.pass.key" -out "$name.key"

    # Generate a certificate signing request (CSR):
    openssl req -batch -new -subj "/CN=$commonName" -key "$name.key" -out "$name.csr"

    # Generate a self signed certificate:
    openssl x509 -req -sha256 -in "$name.csr" -signkey "$name.key" -out "$name.crt"

    # Convert the private key to .pk8 format:
    openssl pkcs8 -topk8 -inform PEM -outform DER -in "$name.key" -out "$name.pk8" -nocrypt

    # Clean up the files you don’t need anymore:
    rm "$name.pass.key" "$name.csr" "$name.key"
```

### Keys and certificates for your MSA

With the default configuration in ``test-config.yml`` the MSA needs the following keys and certificates:

* `test_primary_signing{.pk8,.crt}` - primary signing private key and certificate
* `test_secondary_signing{.pk8,.crt}` - secondary signing private key and certificate
* `test_msa_encryption_1{.pk8,.crt}` - primary encryption private key and certificate
* `test_msa_encryption_2{.pk8,.crt}` - secondary encryption private key and certificate

> **Note:**
> The MSA needs primary and secondary keys to support <a href="#rotate-your-keys">key rotations</a> without causing service downtime for users.

### Keys and certificates for your service

Your service must sign SAML messages and decrypt assertions, so at a minimum you need a private key and certificate
for each of these roles. Your service should support multiple encryption keys to allow for <a href="#rotate-your-keys">key rotations</a>.

Depending on how your service is built you may need to provide keys and certificates in a different format, for example .jks or .pem.


