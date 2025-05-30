Enrich and calculate reputation of a Certificate indicator.

## Script Data

---

| **Name** | **Description** |
| --- | --- |
| Script Type | python3 |
| Tags | reputation |
| Cortex XSOAR Version | 6.0.0 |

## Inputs

---

| **Argument Name** | **Description** |
| --- | --- |
| input | Value of the indicator. |
| update_indicator | Set validation checks in the indicator |

## Outputs

---

| **Path** | **Description** | **Type** |
| --- | --- | --- |
| Certificate.Name | Name \(CN or SAN\) appearing in the certificate. | String |
| Certificate.SubjectDN | The Subject Distinguished Name of the certificate.<br/>This field includes the Common Name of the certificate.<br/> | String |
| Certificate.PEM | Certificate in PEM format. | String |
| Certificate.IssuerDN | The Issuer Distinguished Name of the certificate. | String |
| Certificate.SerialNumber | The Serial Number of the certificate. | String |
| Certificate.ValidityNotAfter | End of certificate validity period. | Date |
| Certificate.ValidityNotBefore | Start of certificate validity period. | Date |
| Certificate.SubjectAlternativeName.Type | Type of the SAN. | String |
| Certificate.SubjectAlternativeName.Value | Name of the SAN. | String |
| Certificate.SHA512 | SHA512 Fingerprint of the certificate in DER format. | String |
| Certificate.SHA256 | SHA256 Fingerprint of the certificate in DER format. | String |
| Certificate.SHA1 | SHA1 Fingerprint of the certificate in DER format. | String |
| Certificate.MD5 | MD5 Fingerprint of the certificate in DER format. | String |
| Certificate.PublicKey.Algorithm | Algorithm used for public key of the certificate. | String |
| Certificate.PublicKey.Length | Length in bits of the public key of the certificate. | Number |
| Certificate.PublicKey.Modulus | Modulus of the public key for RSA keys. | String |
| Certificate.PublicKey.Exponent | Exponent of the public key for RSA keys. | Number |
| Certificate.PublicKey.PublicKey | The public key for DSA/Unknown keys. | String |
| Certificate.PublicKey.P | The P parameter for DSA keys. | String |
| Certificate.PublicKey.Q | The Q parameter for DSA keys. | String |
| Certificate.PublicKey.G | The G parameter for DSA keys. | String |
| Certificate.PublicKey.X | The X parameter for EC keys. | String |
| Certificate.PublicKey.Y | The Y parameter for EC keys. | String |
| Certificate.PublicKey.Curve | Curve of the Public Key for EC keys. | String |
| Certificate.SPKISHA256 | SHA256 fingerprint of the certificate Subject Public Key Info. | String |
| Certificate.Signature.Algorithm | Algorithm used in the signature of the certificate. | String |
| Certificate.Signature.Signature | Signature of the certificate. | String |
| Certificate.Extension.Critical | Critical flag of the certificate extension. | Bool |
| Certificate.Extension.OID | OID of the certificate extension. | String |
| Certificate.Extension.Name | Name of the certificate extension. | String |
| Certificate.Extension.Value | Value of the certificate extension. | Unknown |
| Certificate.Malicious.Vendor | The vendor that reported the file as malicious. | String |
| Certificate.Malicious.Description | A description explaining why the file was determined to be malicious. | String |
| DBotScore.Indicator | The indicator that was tested. | String |
| DBotScore.Type | The indicator type. | String |
| DBotScore.Vendor | The vendor used to calculate the score. | String |
| DBotScore.Score | The actual score. | Number |

## Script Example

```!CertificateReputation input="fead39be0bc680baaaf282d915b44c803e7ab66e61ff5afc356bcf0d12d73f2c" update_indicator=false```

## Context Example

```json
{
    "Certificate": {
        "Extension": [
            {
                "Critical": true,
                "Name": "basicConstraints",
                "OID": "2.5.29.19",
                "Value": {
                    "CA": false
                }
            },
            {
                "Critical": false,
                "Name": "extendedKeyUsage",
                "OID": "2.5.29.37",
                "Value": {
                    "Usages": [
                        "serverAuth",
                        "clientAuth"
                    ]
                }
            },
            {
                "Critical": true,
                "Name": "keyUsage",
                "OID": "2.5.29.15",
                "Value": {
                    "DigitalSignature": true,
                    "KeyEncipherment": true
                }
            },
            {
                "Critical": false,
                "Name": "cRLDistributionPoints",
                "OID": "2.5.29.31",
                "Value": [
                    {
                        "FullName": [
                            {
                                "Type": "uniformResourceIdentifier",
                                "Value": "http://crl.godaddy.com/gdig2s1-2000.crl"
                            }
                        ]
                    }
                ]
            },
            {
                "Critical": false,
                "Name": "certificatePolicies",
                "OID": "2.5.29.32",
                "Value": [
                    {
                        "PolicyIdentifier": "2.16.840.1.114413.1.7.23.1",
                        "PolicyQualifiers": [
                            "http://certificates.godaddy.com/repository/"
                        ]
                    },
                    {
                        "PolicyIdentifier": "2.23.140.1.2.1"
                    }
                ]
            },
            {
                "Critical": false,
                "Name": "authorityInfoAccess",
                "OID": "1.3.6.1.5.5.7.1.1",
                "Value": [
                    {
                        "AccessLocation": {
                            "Type": "uniformResourceIdentifier",
                            "Value": "http://ocsp.godaddy.com/"
                        },
                        "AccessMethod": "OCSP"
                    },
                    {
                        "AccessLocation": {
                            "Type": "uniformResourceIdentifier",
                            "Value": "http://certificates.godaddy.com/repository/gdig2.crt"
                        },
                        "AccessMethod": "caIssuers"
                    }
                ]
            },
            {
                "Critical": false,
                "Name": "authorityKeyIdentifier",
                "OID": "2.5.29.35",
                "Value": {
                    "KeyIdentifier": "40c2bd278ecc348330a233d7fb6cb3f0b42c80ce"
                }
            },
            {
                "Critical": false,
                "Name": "subjectAltName",
                "OID": "2.5.29.17",
                "Value": [
                    {
                        "Type": "dNSName",
                        "Value": "*.gpgyyjgyyg.gw.gpcloudservice.com"
                    },
                    {
                        "Type": "dNSName",
                        "Value": "gpgyyjgyyg.gw.gpcloudservice.com"
                    }
                ]
            },
            {
                "Critical": false,
                "Name": "subjectKeyIdentifier",
                "OID": "2.5.29.14",
                "Value": {
                    "Digest": "6e671e5eb8972804b77c47836f7b942aa13e76a7"
                }
            },
            {
                "Critical": false,
                "Name": "signedCertificateTimestampList",
                "OID": "1.3.6.1.4.1.11129.2.4.2",
                "Value": [
                    {
                        "EntryType": "PreCertificate",
                        "LogId": "f65c942fd1773022145418083094568ee34d131933bfdf0c2f200bcc4ef164e3",
                        "Timestamp": "2020-05-29T09:34:10.000Z",
                        "Version": 0
                    },
                    {
                        "EntryType": "PreCertificate",
                        "LogId": "5cdc4392fee6ab4544b15e9ad456e61037fbd5fa47dca17394b25ee6f6c70eca",
                        "Timestamp": "2020-05-29T09:34:11.000Z",
                        "Version": 0
                    }
                ]
            }
        ],
        "IssuerDN": "CN=Go Daddy Secure Certificate Authority - G2,OU=http://certs.godaddy.com/repository/,O=GoDaddy.com\\, Inc.,L=Scottsdale,ST=Arizona,C=US",
        "MD5": "68048c1404b81f9a262a3910ee5d4c82",
        "Name": [
            "*.gpgyyjgyyg.gw.gpcloudservice.com",
            "gpgyyjgyyg.gw.gpcloudservice.com"
        ],
        "PEM": "-----BEGIN CERTIFICATE-----\nMIIGdDCCBVygAwIBAgIIAQ2G6iqf0yAwDQYJKoZIhvcNAQELBQAwgbQxCzAJBgNV\nBAYTAlVTMRAwDgYDVQQIEwdBcml6b25hMRMwEQYDVQQHEwpTY290dHNkYWxlMRow\nGAYDVQQKExFHb0RhZGR5LmNvbSwgSW5jLjEtMCsGA1UECxMkaHR0cDovL2NlcnRz\nLmdvZGFkZHkuY29tL3JlcG9zaXRvcnkvMTMwMQYDVQQDEypHbyBEYWRkeSBTZWN1\ncmUgQ2VydGlmaWNhdGUgQXV0aG9yaXR5IC0gRzIwHhcNMjAwNTI5MDkzNDA0WhcN\nMjEwNTI5MDkzNDA0WjBQMSEwHwYDVQQLExhEb21haW4gQ29udHJvbCBWYWxpZGF0\nZWQxKzApBgNVBAMMIiouZ3BneXlqZ3l5Zy5ndy5ncGNsb3Vkc2VydmljZS5jb20w\nggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDcrQMUbheHQYPhUEAP+osO\nJfmJL+1/bZjOZN5XSw945kzCC3p90E3Sqh0beTn1eJH89j+jGREjnRk/75lEBbQG\nSMKKstra/zb6iiVkYn7DUTflt0AJVVgsXAQbB9VFLuTcSnOBCqUx+QTDMJR2efH6\nQsMB4Tln/IKdXiiuzk6hnv1G1CgBVOtlfOuVAEy2luIA49GmO4F4K6aupX2oeWnj\neGagWlMjb37Ar0NGA76vSBhJU/jE+1X28FV2kdC5qjC/PP9p04CnbnSm4brReijR\nbpCzgXNwhfY1hYK9S43S2ywI49OPKjOuu0rvElge311luJED4qxjb2JRx5mDReNB\nAgMBAAGjggLrMIIC5zAMBgNVHRMBAf8EAjAAMB0GA1UdJQQWMBQGCCsGAQUFBwMB\nBggrBgEFBQcDAjAOBgNVHQ8BAf8EBAMCBaAwOAYDVR0fBDEwLzAtoCugKYYnaHR0\ncDovL2NybC5nb2RhZGR5LmNvbS9nZGlnMnMxLTIwMDAuY3JsMF0GA1UdIARWMFQw\nSAYLYIZIAYb9bQEHFwEwOTA3BggrBgEFBQcCARYraHR0cDovL2NlcnRpZmljYXRl\ncy5nb2RhZGR5LmNvbS9yZXBvc2l0b3J5LzAIBgZngQwBAgEwdgYIKwYBBQUHAQEE\najBoMCQGCCsGAQUFBzABhhhodHRwOi8vb2NzcC5nb2RhZGR5LmNvbS8wQAYIKwYB\nBQUHMAKGNGh0dHA6Ly9jZXJ0aWZpY2F0ZXMuZ29kYWRkeS5jb20vcmVwb3NpdG9y\neS9nZGlnMi5jcnQwHwYDVR0jBBgwFoAUQMK9J47MNIMwojPX+2yz8LQsgM4wTwYD\nVR0RBEgwRoIiKi5ncGd5eWpneXlnLmd3LmdwY2xvdWRzZXJ2aWNlLmNvbYIgZ3Bn\neXlqZ3l5Zy5ndy5ncGNsb3Vkc2VydmljZS5jb20wHQYDVR0OBBYEFG5nHl64lygE\nt3xHg297lCqhPnanMIIBBAYKKwYBBAHWeQIEAgSB9QSB8gDwAHYA9lyUL9F3MCIU\nVBgIMJRWjuNNExkzv98MLyALzE7xZOMAAAFyX8ggnwAABAMARzBFAiAWHEdWCSQs\n8oeWb9HOOjV5zwCirDN7AIaNIzwlZL+CWwIhAKmnXAum7FHu2z65BLqUodFtbD13\npWg44Zlf3qAEUnVMAHYAXNxDkv7mq0VEsV6a1FbmEDf71fpH3KFzlLJe5vbHDsoA\nAAFyX8girQAABAMARzBFAiBYLbE+evWoNDZk0kQEfjTFzCWb/PD+bp7zpzfPodgS\nVgIhAL3/b3VPsgPbD3iZjbBk7WU8S1J5cI1UDANJGzYZNVySMA0GCSqGSIb3DQEB\nCwUAA4IBAQAnpbpip1Bl803IUzchh7/hjYu8U0IUNigG08CJo2ZYAIHuAXd4h5s4\ns7n59KM9oXcCo+jsD4dKGk08r13uMbSScaxQyJI3WNGVIonU8EvkwRmPQra3pLPy\nUQKMGldJnaUWNkaJ5/Gza/Z8jPg4J+U9OpXMe5NBILEFqCbpF4oZId+BK7D3l6P/\nQNOfg3wlT0TrwsUxyYVFe0B/5SeM0KRZmz34MZ+jE2mRor87e1JFeD2My2YxUMaA\n6vGpxPC0KxalQsB7P5A/B5j9Dbf/ATcy7cTqyRXdPr/6cNNzk2SRHXFzx7lHIkWb\n35JJ9NHFDwQrc2c6YK6xnDUoZUq6lOsi\n-----END CERTIFICATE-----\n",
        "PublicKey": {
            "Algorithm": "RSA",
            "Exponent": 65537,
            "Length": 2048,
            "Modulus": "dc:ad:03:14:6e:17:87:41:83:e1:50:40:0f:fa:8b:0e:25:f9:89:2f:ed:7f:6d:98:ce:64:de:57:4b:0f:78:e6:4c:c2:0b:7a:7d:d0:4d:d2:aa:1d:1b:79:39:f5:78:91:fc:f6:3f:a3:19:11:23:9d:19:3f:ef:99:44:05:b4:06:48:c2:8a:b2:da:da:ff:36:fa:8a:25:64:62:7e:c3:51:37:e5:b7:40:09:55:58:2c:5c:04:1b:07:d5:45:2e:e4:dc:4a:73:81:0a:a5:31:f9:04:c3:30:94:76:79:f1:fa:42:c3:01:e1:39:67:fc:82:9d:5e:28:ae:ce:4e:a1:9e:fd:46:d4:28:01:54:eb:65:7c:eb:95:00:4c:b6:96:e2:00:e3:d1:a6:3b:81:78:2b:a6:ae:a5:7d:a8:79:69:e3:78:66:a0:5a:53:23:6f:7e:c0:af:43:46:03:be:af:48:18:49:53:f8:c4:fb:55:f6:f0:55:76:91:d0:b9:aa:30:bf:3c:ff:69:d3:80:a7:6e:74:a6:e1:ba:d1:7a:28:d1:6e:90:b3:81:73:70:85:f6:35:85:82:bd:4b:8d:d2:db:2c:08:e3:d3:8f:2a:33:ae:bb:4a:ef:12:58:1e:df:5d:65:b8:91:03:e2:ac:63:6f:62:51:c7:99:83:45:e3:41"
        },
        "SHA1": "573a758128a4ff8663946852c46bc89a46f124af",
        "SHA256": "fead39be0bc680baaaf282d915b44c803e7ab66e61ff5afc356bcf0d12d73f2c",
        "SPKISHA256": "de80ccee6accfeee5e290affa142696d2bdaf954f59940e025608593f3ed621c",
        "SerialNumber": "75865109030753056",
        "Signature": {
            "Algorithm": "sha256",
            "Signature": "27a5ba62a75065f34dc853372187bfe18d8bbc534214362806d3c089a366580081ee017778879b38b3b9f9f4a33da17702a3e8ec0f874a1a4d3caf5dee31b49271ac50c8923758d1952289d4f04be4c1198f42b6b7a4b3f251028c1a57499da516364689e7f1b36bf67c8cf83827e53d3a95cc7b934120b105a826e9178a1921df812bb0f797a3ff40d39f837c254f44ebc2c531c985457b407fe5278cd0a4599b3df8319fa3136991a2bf3b7b5245783d8ccb663150c680eaf1a9c4f0b42b16a542c07b3f903f0798fd0db7ff013732edc4eac915dd3ebffa70d3739364911d7173c7b94722459bdf9249f4d1c50f042b73673a60aeb19c3528654aba94eb22"
        },
        "SubjectAlternativeName": [
            {
                "Type": "dNSName",
                "Value": "*.gpgyyjgyyg.gw.gpcloudservice.com"
            },
            {
                "Type": "dNSName",
                "Value": "gpgyyjgyyg.gw.gpcloudservice.com"
            }
        ],
        "SubjectDN": "CN=*.gpgyyjgyyg.gw.gpcloudservice.com,OU=Domain Control Validated",
        "ValidityNotAfter": "2021-05-29T09:34:04.000Z",
        "ValidityNotBefore": "2020-05-29T09:34:04.000Z"
    },
    "DBotScore": {
        "Indicator": "fead39be0bc680baaaf282d915b44c803e7ab66e61ff5afc356bcf0d12d73f2c",
        "Score": 2,
        "Type": "certificate",
        "Vendor": "CertificateReputation"
    }
}
```

## Human Readable Output

>Score for fead39be0bc680baaaf282d915b44c803e7ab66e61ff5afc356bcf0d12d73f2c is 2
>
>## Notes
>
>WILDCARD_CERTIFICATE Certificate contains at least one name with wildcard
>DOMAIN_CONTROL_VALIDATED Certificate is Domain Control Validated
