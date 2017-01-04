## Run SAML compliance tests


The purpose of SAML compliance tests is to ensure that the service
you're building is compliant with the [SAML
profile](https://www.gov.uk/government/publications/identity-assurance-hub-service-saml-20-profile).
This guarantees that:

*   your service will work with GOV.UK Verify
*   user data will remain secure within the GOV.UK Verify federation

Your service and matching service need to consume and produce SAML
messages in a variety of different scenarios. The SAML compliance tool
allows you to test and prove that your service and matching service can
consume and produce all required SAML messages.

The compliance tool provides clear error messages. These will help with
iterative development of your service. We advise you to use the
compliance tool as part of your continuous integration pipeline. This
ensures that any changes maintain backwards compatibility.

Prerequisites:

*   [build your local matching service](#build-a-local-matching-service) – you can use the
    [example of the JSON request](#json-request) that the Matching
    Service Adapter posts to your service and the [JSON schema](#json-schema) for a matching request
*   [install](#install-the-matching-service-adapter) and [configure](#configure-the-matching-service-adapter) your
    Matching Service Adapter

Use the SAML compliance test to:

*   [test your service](#test-your-service-with-the-saml-compliance-tool)
*   [test your matching service](#test-your-matching-service-with-the-saml-compliance-tool)

After SAML compliance tests, you can run
[end-to-end testing](#run-end-to-end-testing) in the integration environment.

### Test your service with the SAML compliance tool


To use the compliance tool, you need configuration data (see step 4).
Generate a new set of configuration data for every test run.

1.  On completion of a successful [Stage
    3](http://alphagov.github.io/identity-assurance-documentation/stage3/Stage3.html)
    gate review, your GOV.UK Verify engagement lead will give you access
    to the compliance tool.
1.  Send the IP addresses of your hosts to the GOV.UK Verify support
    team: <idasupport+onboarding@digital.cabinet-office.gov.uk>. We will
    add a firewall rule allowing access to the compliance tool.

    <a name="generate-self-signed-certificates"></a>


1.  Generate self-signed certificates for use with the compliance tool
    only. You can use OpenSSL to generate self-signed certificates using
    the [guidelines provided by the Heroku Dev
    Center](https://devcenter.heroku.com/articles/ssl-certificate-self#prerequisites).
1.  POST the following JSON (via curl, or similar) to the
    `URI compliance-tool.RPPostUri` provided by your GOV.UK Verify
    engagement lead:
    
    ```
        Content-Type: application/json
        {
            "rpEntityId":"[entityID for your service]",
            "assertionConsumerServiceUrl":"[assertion consumer service URL: this is the URL that will consume responses from the GOV.UK Verify hub]",
            "signingPublicCert":"[Base64-encoded X509 signing certificate for your service]",
            "encryptionPublicCert":"[Base64-encoded X509 encryption certificate for your service]",
            "expectedPID":"[expected persistent identifier: this is the user id that the Matching Service Adapter returns in an assertion]",
            "matchingServiceEntityId":"[entityID for your Matching Service Adapter]",
            "matchingServiceSigningPrivateCert":"[Base64-encoded private signing key for the Matching Service Adapter, see below]",
            "userAccountCreationAttributes":"[optional list of attributes the government service requires for new user account creation, see below]",
            "useSimpleProfile":"[optional, to use a simpler SAML profile that works with Shibboleth - defaults to false]"
        }
    ``` 

    Replace the square brackets and their contents with your configuration data, taking account of the following:
     * the keys and certificates in the configuration data must be single-line strings of Base64-encoded data without the header and footer `BEGIN CERTIFICATE` and `END CERTIFICATE`
     * `matchingServiceSigningPrivateCert`: this is required because the compliance tool sends a response to your service which contains an assertion signed by the Matching Service Adapter

        This key must be in PKCS\#8 PEM format:
        
        ```
        cat server.crt server.key > server.pem

        openssl pkcs8 -nocrypt -in server.pem -out server.pk8.pem -outform PEM -topk8
        ```
     * `userAccountCreationAttributes`: provide this only if you want to test [new user account creation](#create-user-accounts-message-flow) – select from the [full list of attributes](#list-attributes)

1.  You receive a response similar to the following:

        Status 200 Created

1.  Consume the GOV.UK Verify hub's metadata from the URL
    `compliance-tool.MetadataUri` provided by your GOV.UK Verify
    engagement lead. This metadata contains the compliance tool single
    sign-on (SSO) URI.
1.  Generate an authentication request and POST it to the compliance
    tool's SSO URI. Follow the redirect in the response to retrieve the
    result.

    > **Note:** You may want to use an off-the-shelf tool to generate an
    > authentication request.

1.  If the result contains `PASSED`, access the URI provided in
    `responseGeneratorLocation`. A list of test scenarios is displayed.
1.  Access the `executeUri` for each test scenario you want to execute.
    The following test scenarios are provided:
    * Basic successful match
    * Basic no match
    * No authentication context (this is when the user cancels the process)
    * Authentication failed
    * Requester error (this is when the request is invalid)
    * Account creation

   The above scenarios are the possible responses for step 8 in the [SAML message flow](#how-saml-works-with-gov-uk-verify).

### Test your matching service with the SAML compliance tool


1.  To set up the SAML compliance tool for matching service tests, POST
    the following JSON (via curl or similar) to the URL
    `<compliance-tool-host>/ms-test-run` provided by your GOV.UK Verify
    engagement lead:
    
    ```
     Content-Type: application/json
       {
       "matchingServiceEntityId": "[entityID of the matching service]",
       "transactionEntityId": "[entityID of the transaction (service)]",
       "matchingServiceEndpoint": "[the matching service's endpoint]",
       "matchingServicePublicSigningCert": "[signing certificate to verify the response]",
       "matchingServicePublicEncryptionCert": "[encryption certificate to encrypt the assertions]"
       }
    ```

1.  You receive a response similar to the following:

        Status 201 Created
        Location: .../ms-test-run/8fd7782f-efac-48b2-8171-3e4da9553d19

1.  POST your test [matching dataset](#glossary-matching-dataset) (see example below) to the
    `Location` field in the above response
    (`.../ms-test-run/8fd7782f-efac-48b2-8171-3e4da9553d19` in the above
    example).

        {
            "levelOfAssurance": "LEVEL_2",
            "persistentId": "93E5910B-F4C2-4561-AEC5-C878AFEF25A3",
            "firstName": {
                "value": "Joe",
                "to": "",
                "from": "",
                "verified": true
            },
            "middleNames": {
                "value": "Bob Rob",
                "to": "",
                "from": "",
                "verified": true
            },
            "surnames": [
                {
                    "value": "Fred",
                    "to": "2010-01-20",
                    "from": "1980-05-24",
                    "verified": true
                },
                {
                    "value": "Dou",
                    "to": "",
                    "from": "2010-01-20",
                    "verified": true
                }
            ],
            "gender": {
                "value": "Male",
                "to": "",
                "from": "",
                "verified": true
            },
            "dateOfBirth": {
                "value": "1980-05-24",
                "to": "",
                "from": "",
                "verified": true
            },
            "addresses": [
                {
                    "lines": ["123 George Street"],
                    "postCode": "GB1 2PP",
                    "internationalPostCode": "GB1 2PP",
                    "uprn": "7D68E096-5510-B3844C0BA3FD",
                    "toDate": "2005-05-14",
                    "fromDate": "1980-05-24",
                    "verified": true
                },
                {
                    "lines": ["10 George Street"],
                    "postCode": "GB1 2PF",
                    "internationalPostCode": "GB1 2PF",
                    "uprn": "833F1187-9F33-A7E27B3F211E",
                    "toDate": null,
                    "fromDate": "2005-05-14",
                    "verified": true
                }
            ],
            "cycle3Dataset": {
                "key": "drivers_licence",
                "value": "4C22DA90A18A4B88BE460E0A3D975F68"
            }
        }

    where:
    * `persistentId` is mandatory
    * you must supply at least one other value in addition to `persistentId`
    * the values of `addresses` and `surnames` are arrays
    * fields have optional `from` and `to` attributes in which you can capture historical values – for example, if the user has changed their surname, there's an additional entry for the old surname with the `from` and `to` values defining the period for which the name was valid; the new surname only has the `from` attribute, containing the date from which it was valid
    * the `addresses` field that holds the current address contains a `fromDate` attribute for the date from which the address is valid; past addresses also contain the `toDate` attribute
    * the `cycle3Dataset` field is only present for a cycle 3 matching attempt
    * the `uprn` (Unique Property Reference Number) is a unique reference for each property in Great Britain, ensuring accuracy of address data. This is an optional attribute that can contain up to 12 characters and should not have any leading zeros

1.  When the SAML compliance tool receives your test matching dataset,
    it will POST an attribute query to your Matching Service Adapter.
    This corresponds to step 4 in the [SAML message flow](#how-saml-works-with-gov-uk-verify).
1.  Your Matching Service Adapter validates the query and sends a POST
    with a JSON request containing your test matching dataset to your
    local matching service. This corresponds to step 5 in the
    [SAML message flow](#how-saml-works-with-gov-uk-verify).

### Example of a JSON request to your local matching service

Below is a formatted example of a cycle 3 matching request that the
Matching Service Adapter sends to your local matching service:

    {
        "cycle3Dataset": {
            "attributes": {
                "drivers_licence": "4C22DA90A18A4B88BE460E0A3D975F68"
            }
        },
        "hashedPid": "8a5db0ad424efe4e09622cc4a876cc4c338558384752b483ff69dda4dca1ef04",
        "levelOfAssurance": "LEVEL_2",
        "matchId": "default-request-id",
        "matchingDataset": {
            "addresses": [
                {
                    "fromDate": "1980-05-24T00:00:00.000Z",
                    "internationalPostCode": "GB1 2PP",
                    "lines": [
                        "123 George Street"
                    ],
                    "postCode": "GB1 2PP",
                    "toDate": "2005-05-14T00:00:00.000Z",
                    "uprn": "7D68E096-5510-B3844C0BA3FD",
                    "verified": true
                },
                {
                    "fromDate": "2005-05-14T00:00:00.000Z",
                    "internationalPostCode": "GB1 2PF",
                    "lines": [
                        "10 George Street"
                    ],
                    "postCode": "GB1 2PF",
                    "uprn": "833F1187-9F33-A7E27B3F211E",
                    "verified": true
                }
            ],
            "dateOfBirth": {
                "value": "1980-05-24",
                "verified": true
            },
            "firstName": {
                "value": "Joe",
                "verified": true
            },
            "gender": {
                "value": "MALE",
                "verified": true
            },
            "middleNames": {
                "value": "Bob Rob",
                "verified": true
            },
            "surnames": [
                {
                    "from": "1980-05-24T00:00:00.000Z",
                    "to": "2010-01-20T00:00:00.000Z",
                    "value": "Fred",
                    "verified": true
                },
                {
                    "from": "2010-01-20T00:00:00.000Z",
                    "value": "Dou",
                    "verified": true
                }
            ]
        }
    }

where the `matchId` field is unique to a user's journey in the GOV.UK
Verify hub – you can log this for future reference.

Below is an example of a response from your local matching service:

    200 {
     result : match|no-match
     }`

This response corresponds to step 6 in the
[SAML message flow](#how-saml-works-with-gov-uk-verify).
