Deprecated. Use ***EWS Extension Online Powershell v3*** instead.

This integration enables you to manage and interact with Microsoft O365 - Exchange Online from within XSOAR.
This integration was integrated and tested with version V1 of Exchange Online PowerShell.

## Enable or disable access to Exchange Online PowerShell

Exchange Online PowerShell enables you to manage your Exchange Online organization from the command line.
By default, all accounts you create in Microsoft 365 are allowed to use Exchange Online PowerShell.
Administrators can use Exchange Online PowerShell to enable or disable a user's ability to connect to Exchange Online PowerShell.
Note that access to Exchange Online PowerShell doesn't give users extra administrative powers in your organization.
A user's capabilities in Exchange Online PowerShell are still defined by a role based access control (RBAC) and the roles that are assigned to them.

For more [info](https://docs.microsoft.com/en-us/powershell/exchange/disable-access-to-exchange-online-powershell?view=exchange-ps)

## Configure O365 - EWS - Extension on Cortex XSOAR

1. Navigate to **Settings** > **Integrations** > **Servers & Services**.

2. Search for O365 - EWS - Extension.

3. Authentication / Authorization methods:

    1. OAuth2.0 authorization (recommended):

        1. Click **Add an instance** to create and configure a new integration instance.

           | **Parameter** | **Description**                                          | **Required** |
           | ------------- | -------------------------------------------------------- | ------------ |
           | url           | Echange online URL                                       | True         |
           | credentials   | Fill **only** Email (aka UPN), Password should be empty. | False        |
           | insecure      | Trust any certificate \(not secure\)                     | False        |

        2. Open playground -  War-room:

            1. Run the ***!ews-auth-start*** command and follow the instructions. Expected output is:

           > ## EWS extension - Authorize instructions
            >
            > 1. To sign in, use a web browser to open the page [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) and enter the code **XXXXXXX** to authenticate.
            > 2. Run the command ***!ews-auth-complete*** command in the War Room.

            2. Test - OAuth2.0 authorization, Run the ***!ews-auth-test*** command.

2. Basic authentication (Not recommended):

    1. Click **Add an instance** to create and configure a new integration instance.

       | **Parameter** | **Description** | **Required** |
       | ------------- | --------------- | ------------ |
       | url | Search and Compliance URL | True |
       | credentials | Fill Email (aka UPN) and password | False |
       | insecure | Trust any certificate \(not secure\) | False |

    2. Click **Test** to validate the URLs, token, and connection.

## Commands

You can execute these commands from the Cortex XSOAR CLI, as part of an automation, or in a playbook.
After you successfully execute a command, a DBot message appears in the War Room with the command details.

### ews-auth-start

***
OAuth2.0 - Start authorization.

#### Base Command

`ews-auth-start`

#### Input

There are no input arguments for this command.

#### Context Output

There is no context output for this command.

#### Command Example

```!ews-auth-start```

#### Human Readable Output

>## EWS extension - Authorize instructions
>
>1. To sign in, use a web browser to open the page [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) and enter the code **XXXXXXX** to authenticate.
>2. Run the ***!ews-auth-complete*** command in the War Room.

### ews-auth-complete

***
Completes the OAuth2.0 authorization process.

#### Base Command

`ews-auth-complete`

#### Input

There are no input arguments for this command.

#### Context Output

There is no context output for this command.

#### Command Example

```!ews-auth-complete```

#### Human Readable Output

>Your account **successfully** authorized!

### ews-auth-test

***
Tests the OAuth2.0 authorization process.

#### Base Command

`ews-auth-test`

#### Input

There are no input arguments for this command.

#### Context Output

There is no context output for this command.

#### Command Example

```!ews-auth-test```

#### Human Readable Output

>**Test ok!**

### ews-junk-rules-get

***
Gets junk rules for the specified mailbox.

#### Base Command

`ews-junk-rules-get`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| mailbox | ID of the mailbox for which to get junk rules. | Required |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.Rule.Junk.BlockedSendersAndDomains | String | Blocked senders and domains list. |
| EWS.Rule.Junk.ContactsTrusted | Boolean | If true, contacts are trusted by default. |
| EWS.Rule.Junk.Email | String | Junk rule mailbox. |
| EWS.Rule.Junk.Enabled | Boolean | If true, junk rule is enabled. |
| EWS.Rule.Junk.Identity | String | Junk rule identity. |
| EWS.Rule.Junk.MailboxOwnerId | String | Mail box owner ID. |
| EWS.Rule.Junk.TrustedListsOnly | Boolean | If true, only a list defined in the trusted lists are trusted. |
| EWS.Rule.Junk.TrustedRecipientsAndDomains | String | List of trusted recipients and domains. |
| EWS.Rule.Junk.TrustedSendersAndDomains | String | List of trusted senders and domains. |

#### Command Example

```!ews-junk-rules-get mailbox="xsoar@dev.onmicrosoft.com"```

#### Context Example

```json
{
    "EWS": {
        "Rule": {
            "Junk": {
                "BlockedSendersAndDomains": [
                    "user1@gmail.com",
                    "user2@gmail.com"
                ],
                "ContactsTrusted": false,
                "Enabled": false,
                "Identity": "xsoar",
                "MailboxOwnerId": "xsoar",
                "TrustedListsOnly": false,
                "TrustedRecipientsAndDomains": [
                  "user1@gmail.com",
                  "user2@gmail.com"
                ],
                "TrustedSendersAndDomains": [
                  "user1@gmail.com",
                  "user2@gmail.com"
                ]
            }
        }
    }
}
```

#### Human Readable Output

>### EWS extension - 'xsoar@dev.onmicrosoft.com' Junk rules
>
>| BlockedSendersAndDomains | ContactsTrusted | Enabled | TrustedListsOnly | TrustedSendersAndDomains
>| --- | --- | --- | --- | ---
>| \["user1@gmail.com","user2@gmail.com"\] | False | False | False | \["user1@gmail.com","user2@gmail.com"\]

### ews-junk-rules-set

***
Sets junk rules for the specified mailbox.

#### Base Command

`ews-junk-rules-set`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| mailbox | ID of the mailbox for which to set junk rules. | Required |
| add_blocked_senders_and_domains | Comma-separated list of blocked senders and domains to add to the mailbox. | Optional |
| remove_blocked_senders_and_domains | Comma-separated list of blocked senders and domains to remove from the mailbox. | Optional |
| add_trusted_senders_and_domains | Comma-separated list of trusted senders and domains to add to the mailbox. | Optional |
| remove_trusted_senders_and_domains | Comma-separated list of trusted senders and domains to remove from the mailbox. | Optional |
| trusted_lists_only | If true, trust only lists defined in the trusted lists. Can be "true" or "false". Possible values are: true, false. | Optional |
| contacts_trusted | If true, contacts are trusted by default. Can be "true" or "false". Possible values are: true, false. | Optional |
| enabled | If true, the junk rule is enabled. Can be "true" or "false". Possible values are: true, false. | Optional |

#### Context Output

There is no context output for this command.

#### Command Example

```!ews-junk-rules-set mailbox="xsoar@dev.onmicrosoft.com" add_blocked_senders_and_domains="test@gmail.com" add_trusted_senders_and_domains="dev.onmicrosoft.com"```

#### Human Readable Output

>EWS extension - 'xsoar@dev.onmicrosoft.com' Junk rules **modified**!

### ews-global-junk-rules-set

***
Sets junk rules in all managed accounts.

#### Base Command

`ews-global-junk-rules-set`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| add_blocked_senders_and_domains | Comma-separated list of blocked senders and domains to add to the mailbox. | Optional |
| remove_blocked_senders_and_domains | Comma-separated list of blocked senders and domains to remove from the mailbox. | Optional |
| add_trusted_senders_and_domains | Comma-separated list of trusted senders and domains to add to the mailbox. | Optional |
| remove_trusted_senders_and_domains | Comma-separated list of trusted senders and domains to remove from the mailbox. | Optional |
| trusted_lists_only | If true, trust only lists defined in the trusted lists. Can be "true" or "false". Possible values are: true, false. | Optional |
| contacts_trusted | If true, contacts are trusted by default. Can be "true" or "false". Possible values are: true, false. | Optional |
| enabled | If true, the junk rule is enabled. Can be "true" or "false". Possible values are: true, false. | Optional |

#### Context Output

There is no context output for this command.

#### Command Example

```!ews-global-junk-rules-set add_blocked_senders_and_domains="test@demisto.com" add_trusted_senders_and_domains="demisto.com"```

#### Human Readable Output

>EWS extension - Junk rules globally **modified**!

### ews-message-trace-get

***
Searches message data for the last 10 days. If you run this command without any arguments, only data from the last 48 hours is returned.
If you enter a start date that is older than 10 days, you will receive an error and the command will return no results.
This command returns a maximum of 1,000,000 results, and will timeout on very large queries. If your query returns too many results, consider splitting it up using shorter start_date and end_date intervals.

#### Base Command

`ews-message-trace-get`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| sender_address | The sender_address parameter filters the results by the sender's email address. You can specify multiple values separated by commas.<br/>. | Optional |
| recipient_address | The recipient_address parameter filters the results by the recipient's email address. You can specify multiple values separated by commas.<br/>. | Optional |
| from_ip | The from_ip parameter filters the results by the source IP address.<br/>For incoming messages, the value of from_ip is the public IP address of the SMTP email server that sent the message.<br/>For outgoing messages from Exchange Online, the value is blank.<br/>. | Optional |
| to_ip | The to_ip parameter filters the results by the destination IP address.<br/>For outgoing messages, the value of to_ip is the public IP address in the resolved MX record for the destination domain.<br/>For incoming messages to Exchange Online, the value is blank.<br/>. | Optional |
| message_id | The message_id parameter filters the results by the Message-ID header field of the message.<br/>This value is also known as the Client ID. The format of the Message-ID depends on the messaging server that sent the message.<br/>The value should be unique for each message. However, not all messaging servers create values for the Message-ID in the same way.<br/>Be sure to include the full Message ID string (which may include angle brackets) and enclose the value in quotation marks (for example,"d9683b4c-127b-413a-ae2e-fa7dfb32c69d@DM3NAM06BG401.Eop-nam06.prod.protection.outlook.com").<br/>. | Optional |
| message_trace_id | The message_trace_id parameter can be used with the recipient address to uniquely identify a message trace and obtain more details.<br/>A message trace ID is generated for every message that's processed by the system.<br/>. | Optional |
| page | The page number of the results you want to view.<br/>Can be an integer between 1 and 1000. The default value is 1.<br/>. Default is 1. | Optional |
| page_size | The maximum number of entries per page.<br/>Can be an integer between 1 and 5000. The default value is 100.<br/>. Default is 100. | Optional |
| start_date | The start date of the date range.<br/>Use the short date format that's defined in the Regional Options settings on the computer where you're running the command. For example, if the computer is configured to use the short date format mm/dd/yyyy,<br/>enter 09/01/2018 to specify September 1, 2018. You can enter the date only, or you can enter the date and time of day.<br/>If you enter the date and time of day, enclose the value in quotation marks ("), for example, "09/01/2018 5:00 PM".<br/>Valid input for this parameter is from 10 days - now ago. The default value is 48 hours ago.<br/>. | Optional |
| end_date | The end date of the date range.<br/>Use the short date format that's defined in the Regional Options settings on the computer where you're running the command.<br/>For example, if the computer is configured to use the short date format mm/dd/yyyy, enter 09/01/2018 to specify September 1, 2018.<br/>You can enter the date only, or you can enter the date and time of day.<br/>If you enter the date and time of day, enclose the value in quotation marks ("), for example, "09/01/2018 5:00 PM".<br/>Valid input for this parameter is from start_date - now. The default value is now.<br/>. | Optional |
| status | The status of the message. Can be one of the following:<br/>  *GettingStatus: The message is waiting for status update.<br/>* Failed: Message delivery was attempted and it failed or the message was filtered as spam or malware, or by transport rules.<br/>  *Pending: Message delivery is underway or was deferred and is being retried.<br/>* Delivered: The message was delivered to its destination.<br/>  *Expanded: There was no message delivery because the message was addressed to a distribution group and the membership of the distribution was expanded.<br/>* Quarantined: The message was quarantined.<br/>  * FilteredAsSpam: The message was marked as spam.<br/>. Possible values are: GettingStatus, Failed, Pending, Delivered, Expanded, Quarantined, FilteredAsSpam. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.MessageTrace.FromIP | String | The public IP address of the SMTP email server that sent the message. |
| EWS.MessageTrace.ToIP | String | The public IP address in the resolved MX record for the destination domain. For incoming messages to Exchange Online, the value is blank. |
| EWS.MessageTrace.Index | Number | Message index in pagination. \(Index starts from 0\) |
| EWS.MessageTrace.MessageId | String | Message-ID header field of the message. |
| EWS.MessageTrace.MessageTraceId | String | Message trace ID of the message. |
| EWS.MessageTrace.Organization | String | Message trace organization source. |
| EWS.MessageTrace.Received | Date | Message receive time. |
| EWS.MessageTrace.RecipientAddress | String | Message recipients address. |
| EWS.MessageTrace.SenderAddress | String | Message sender address. |
| EWS.MessageTrace.Size | Number | Message size in bytes. |
| EWS.MessageTrace.StartDate | Date | Message trace start date. |
| EWS.MessageTrace.EndDate | Date | Message trace end date. |
| EWS.MessageTrace.Status | String | Message status. |
| EWS.MessageTrace.Subject | String | Message subject. |

#### Command Example

```!ews-message-trace-get```

#### Context Example

```json
{
    "EWS": {
        "MessageTrace": [
            {
                "EndDate": "2021-01-03T06:14:14.9596257Z",
                "FromIP": "8.8.8.8",
                "Index": 1,
                "MessageId": "xxx",
                "MessageTraceId": "xxxx",
                "Organization": "dev.onmicrosoft.com",
                "Received": "2021-01-03T04:45:36.4662406",
                "RecipientAddress": "xsoar@dev.onmicrosoft.com",
                "SenderAddress": "xsoar@dev.onmicrosoft.com",
                "Size": 1882,
                "StartDate": "2021-01-01T06:14:14.9596257Z",
                "Status": "GettingStatus",
                "Subject": "Test mail",
                "ToIP": null
            },
            {
                "EndDate": "2021-01-03T06:15:14.9596257Z",
                "FromIP": "8.8.8.8",
                "Index": 2,
                "MessageId": "xxx",
                "MessageTraceId": "xxxx",
                "Organization": "dev.onmicrosoft.com",
                "Received": "2021-01-03T04:46:36.4662406",
                "RecipientAddress": "xsoar@dev.onmicrosoft.com",
                "SenderAddress": "xsoar@dev.onmicrosoft.com",
                "Size": 1882,
                "StartDate": "2021-01-01T06:15:14.9596257Z",
                "Status": "GettingStatus",
                "Subject": "Test mail",
                "ToIP": null
            }
        ]
    }
}
```

#### Human Readable Output

>### EWS extension - Messages trace
>
>| EndDate | FromIP | Index | MessageId | MessageTraceId | Organization | Received | RecipientAddress | SenderAddress | Size | StartDate | Status | Subject | ToIP
>| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---
>| 1/3/2021 6:14:14 AM | 8.8.8.8 | 0 | xxx | xxxx | microsoft.com | 1/3/2021 4:45:36 AM | xsoar@dev.microsoft.com | xsoar@dev.onmicrosoft.com | 6975 | 1/1/2021 6:14:14 AM | Delivered | Test mail |
>| 1/3/2021 6:15:14 AM | 8.8.8.8 | 1 | xxx | xxxx | microsoft.com | 1/3/2021 4:46:36 AM | xsoar@dev.microsoft.com | xsoar@dev.onmicrosoft.com | 6975 | 1/1/2021 6:15:14 AM | Delivered | Test mail |

### ews-federation-trust-get

***
Displays the federation trust configured for the Exchange organization.

#### Base Command

`ews-federation-trust-get`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| domain_controller | The domain controller identified by its fully qualified domain name (FQDN). For example, dc01.example.com. This argument is available only in on-premises Exchange. | Optional |
| identity | The federation trust ID. If not specified, the command returns all federation trusts configured for the Exchange organization. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.FederationTrust.AdminDisplayName | String | Administrator display name of the federation trust. |
| EWS.FederationTrust.ApplicationIdentifier | String | Application identifier of the federation trust. |
| EWS.FederationTrust.ApplicationUri | String | Application URI of the federation trust. |
| EWS.FederationTrust.DistinguishedName | String | Distinguished name of the federation trust. |
| EWS.FederationTrust.ExchangeObjectId | String | Exchange object ID of the federation trust. |
| EWS.FederationTrust.ExchangeVersion | String | Exchange version of the federation trust. |
| EWS.FederationTrust.Guid | String | GUID of the federation trust. |
| EWS.FederationTrust.Id | String | ID of the federation trust. |
| EWS.FederationTrust.Identity | String | Identity of the federation trust. |
| EWS.FederationTrust.IsValid | Boolean | Whether the federation trust is valid. |
| EWS.FederationTrust.MetadataEpr | String | Metadata EPR of the federation trust. |
| EWS.FederationTrust.MetadataPollInterval | Date | Metadata poll interval of the federation trust. |
| EWS.FederationTrust.MetadataPutEpr | Unknown | Metadata put EPR of the federation trust. |
| EWS.FederationTrust.Name | String | Name of the federation trust. |
| EWS.FederationTrust.NamespaceProvisioner | String | Namespace provisioner of the federation trust. |
| EWS.FederationTrust.ObjectCategory | String | Object category of the federation trust. |
| EWS.FederationTrust.ObjectClass | String | Object class of the federation trust. |
| EWS.FederationTrust.ObjectState | String | Object state of the federation trust. |
| EWS.FederationTrust.OrgCertificate.Archived | Boolean | Whether the organization certificate of the federation trust is archived. |
| EWS.FederationTrust.OrgCertificate.Extensions.Critical | Boolean | Whether the extensions of the organization certificate are critical. |
| EWS.FederationTrust.OrgCertificate.Extensions.Oid.FriendlyName | String | Friendly name of the OID of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.Oid.Value | String | Value of the OID of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.RawData | Number | Raw data of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.SubjectKeyIdentifier | String | Subject key identifier of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.KeyUsages | Number | Key usages of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.EnhancedKeyUsages.FriendlyName | String | Friendly name of the enhanced key usages of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.EnhancedKeyUsages.Value | String | Value of the enhanced key usages of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.Extensions.CertificateAuthority | Boolean | Whether the organization certificate extensions have a certificate authority. |
| EWS.FederationTrust.OrgCertificate.Extensions.HasPathLengthConstraint | Boolean | Whether the organization certificate extensions have a path length constraint. |
| EWS.FederationTrust.OrgCertificate.Extensions.PathLengthConstraint | Number | Path length constraint of the organization certificate extensions. |
| EWS.FederationTrust.OrgCertificate.FriendlyName | String | Friendly name of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.Handle.value | Number | The handle value of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.HasPrivateKey | Boolean | Whether the organization certificate has a private key. |
| EWS.FederationTrust.OrgCertificate.Issuer | String | Issuer of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.IssuerName.Name | String | Name of the issuer of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.IssuerName.Oid.FriendlyName | Unknown | Friendly Name of the OID of the issuer name of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.IssuerName.Oid.Value | Unknown | Value of the OID of the issuer name of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.IssuerName.RawData | Number | Raw data of the issuer name of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.NotAfter | Date | The date until when the organization certificate is valid. |
| EWS.FederationTrust.OrgCertificate.NotBefore | Date | The date the organization certificate became valid. |
| EWS.FederationTrust.OrgCertificate.PrivateKey | Unknown | Private key of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedKeyValue.Oid.FriendlyName | String | Friendly name of the OID of the encoded key value of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedKeyValue.Oid.Value | String | Value of the OID of the encoded key value of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedKeyValue.RawData | Number | Raw data of the encoded key value of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedParameters.Oid.FriendlyName | String | Friendly name of the OID of the encoded parameters of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedParameters.Oid.Value | String | Value of the OID of the encoded parameters of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.EncodedParameters.RawData | Number | Raw data of the encoded parameters of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Key.KeyExchangeAlgorithm | String | Key exchange algorithm of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Key.LegalKeySizes.MaxSize | Number | Maximum size of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Key.LegalKeySizes.MinSize | Number | Minimum size of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Key.LegalKeySizes.SkipSize | Number | SkipSize of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Key.SignatureAlgorithm | String | Signature algorithm of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Oid.FriendlyName | String | Friendly name of the OID of the public key. |
| EWS.FederationTrust.OrgCertificate.PublicKey.Oid.Value | String | Value of the OID of the public key. |
| EWS.FederationTrust.OrgCertificate.RawData | Number | Raw data of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.SerialNumber | String | Serial number of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.SignatureAlgorithm.FriendlyName | String | Friendly name of the signature algorithm. |
| EWS.FederationTrust.OrgCertificate.SignatureAlgorithm.Value | String | Value of the signature algorithm. |
| EWS.FederationTrust.OrgCertificate.Subject | String | Subject of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.SubjectName.Name | String | Name of the subject of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.SubjectName.Oid.FriendlyName | Unknown | Friendly name of the OID of the subject name. |
| EWS.FederationTrust.OrgCertificate.SubjectName.Oid.Value | Unknown | Value of the OID of the subject name. |
| EWS.FederationTrust.OrgCertificate.SubjectName.RawData | Number | Raw Data of the subject name. |
| EWS.FederationTrust.OrgCertificate.Thumbprint | String | Thumbprint of the organization certificate. |
| EWS.FederationTrust.OrgCertificate.Version | Number | Version of the organization certificate. |
| EWS.FederationTrust.OrgNextCertificate | Unknown | Next organization certificate. |
| EWS.FederationTrust.OrgNextPrivCertificate | String | Next organization private certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Archived | Boolean | Whether to archive the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.Critical | Boolean | Whether the extensions of the previous organization certificate are critical. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.Oid.FriendlyName | String | Friendly name of the OID of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.Oid.Value | String | Value of the OID of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.RawData | Number | Raw data of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.SubjectKeyIdentifier | String | Subject key identifier of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.KeyUsages | Number | Key usages of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.EnhancedKeyUsages.FriendlyName | String | Friendly name of the enhanced key usages of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.Extensions.EnhancedKeyUsages.Value | String | Value of the enhanced key usages of the previous organization certificate extensions. |
| EWS.FederationTrust.OrgPrevCertificate.FriendlyName | String | Friendly name of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Handle.value | Number | Value of the handle of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.HasPrivateKey | Boolean | Whether the previous organization certificate has a private key. |
| EWS.FederationTrust.OrgPrevCertificate.Issuer | String | Issuer of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.IssuerName.Name | String | Name of the issuer of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.IssuerName.Oid.FriendlyName | Unknown | Friendly name of the OID of the issuer of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.IssuerName.Oid.Value | Unknown | Value of the OID of the issuer of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.IssuerName.RawData | Number | Raw data of the issuer of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.NotAfter | Date | The date until when the previous organization certificate is valid. |
| EWS.FederationTrust.OrgPrevCertificate.NotBefore | Date | The date the previous organization certificate became valid. |
| EWS.FederationTrust.OrgPrevCertificate.PrivateKey | Unknown | Private Key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedKeyValue.Oid.FriendlyName | String | Friendly Name of the OID of the encoded key value of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedKeyValue.Oid.Value | String | Value of the OID of the encoded key value of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedKeyValue.RawData | Number | Raw Data of the encoded key value of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedParameters.Oid.FriendlyName | String | Friendly name of the OID of the encoded parameters of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedParameters.Oid.Value | String | Value of the OID of the encoded parameters of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.EncodedParameters.RawData | Number | Raw Data of the encoded parameters of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Key.KeyExchangeAlgorithm | String | Key exchange algorithm of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Key.LegalKeySizes.MaxSize | Number | Maximum size of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Key.LegalKeySizes.MinSize | Number | Minimum size of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Key.LegalKeySizes.SkipSize | Number | SkiPSize of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Key.SignatureAlgorithm | String | Signature algorithm of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Oid.FriendlyName | String | Friendly name of the OID of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.PublicKey.Oid.Value | String | Value of the OID of the public key of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.RawData | Number | Raw Data of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SerialNumber | String | Serial number of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SignatureAlgorithm.FriendlyName | String | Friendly name of the signature algorithm of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SignatureAlgorithm.Value | String | Value of the signature algorithm of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Subject | String | Subject of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SubjectName.Name | String | Name of the subject of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SubjectName.Oid.FriendlyName | Unknown | Friendly name of the OID of the subject of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SubjectName.Oid.Value | Unknown | Value of the OID of the subject name of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.SubjectName.RawData | Number | Raw Data of the subject name of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Thumbprint | String | Thumbprint of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevCertificate.Version | Number | Version of the previous organization certificate. |
| EWS.FederationTrust.OrgPrevPrivCertificate | String | Organization previous private certificate. |
| EWS.FederationTrust.OrgPrivCertificate | String | Organization private certificate. |
| EWS.FederationTrust.OrganizationId | String | Organization ID. |
| EWS.FederationTrust.OriginatingServer | String | Originating server. |
| EWS.FederationTrust.PSComputerName | String | PowerShell computer name. |
| EWS.FederationTrust.PSShowComputerName | Boolean | Whether to show the PowerShell computer name. |
| EWS.FederationTrust.PolicyReferenceUri | String | Policy Reference URI. |
| EWS.FederationTrust.RunspaceId | String | Runspace ID. |
| EWS.FederationTrust.TimesOfUnmatchPartner | Number | Times Of unmatch partner. |
| EWS.FederationTrust.TokenIssuerCertReference | String | Token issuer certificate reference. |
| EWS.FederationTrust.TokenIssuerCertificate.Archived | Boolean | Whether the token issuer certificate is archived. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.Critical | Boolean | Whether the extensions of the token issuer certificate are critical. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.Oid.FriendlyName | String | Friendly name of the OID of the extensions of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.Oid.Value | String | Value of the OID of the extensions of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.RawData | Number | Raw Data of the extensions of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.SubjectKeyIdentifier | String | Subject key identifier of the extensions of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Extensions.KeyUsages | Number | Key usages of the extensions of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.FriendlyName | String | Friendly name of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Handle.value | Number | Value of the handle of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.HasPrivateKey | Boolean | Whether the token issuer certificate has a private key. |
| EWS.FederationTrust.TokenIssuerCertificate.Issuer | String | Issuer of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.IssuerName.Name | String | Name of the issuer of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.IssuerName.Oid.FriendlyName | Unknown | Friendly name of the OID of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.IssuerName.Oid.Value | Unknown | Value of the OID of the issuer of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.IssuerName.RawData | Number | Raw data of the issuer of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.NotAfter | Date | The date until when the token issuer certificate is valid. |
| EWS.FederationTrust.TokenIssuerCertificate.NotBefore | Date | The date the token issuer certificate became valid. |
| EWS.FederationTrust.TokenIssuerCertificate.PrivateKey | Unknown | Private key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedKeyValue.Oid.FriendlyName | String | Friendly name of the OID of the encoded key value of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedKeyValue.Oid.Value | String | Value of the OID of the encoded key value of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedKeyValue.RawData | Number | Raw data of the encoded key value of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedParameters.Oid.FriendlyName | String | Friendly name of the OID of the encoded parameters of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedParameters.Oid.Value | String | Value of the OID of the encoded parameters of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.EncodedParameters.RawData | Number | Raw Data of the encoded parameters of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Key.KeyExchangeAlgorithm | String | Key exchange algorithm of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Key.LegalKeySizes.MaxSize | Number | Maximum size of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Key.LegalKeySizes.MinSize | Number | Minimum size of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Key.LegalKeySizes.SkipSize | Number | SkiPSize of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Key.SignatureAlgorithm | String | Signature algorithm of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Oid.FriendlyName | String | Friendly name of the OID of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.PublicKey.Oid.Value | String | Value of the OID of the public key of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.RawData | Number | Raw Data of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SerialNumber | String | Serial number of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SignatureAlgorithm.FriendlyName | String | Friendly name of the signature algorithm of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SignatureAlgorithm.Value | String | Value of the signature algorithm of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Subject | String | Subject of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SubjectName.Name | String | Name of the subject of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SubjectName.Oid.FriendlyName | Unknown | Friendly name of the OID of the subject of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SubjectName.Oid.Value | Unknown | Value of the OID of the subject of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.SubjectName.RawData | Number | Raw data of the subject of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Thumbprint | String | Thumbprint of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerCertificate.Version | Number | Version of the token issuer certificate. |
| EWS.FederationTrust.TokenIssuerEpr | String | Token issuer EPR. |
| EWS.FederationTrust.TokenIssuerMetadataEpr | String | Token issuer metadata EPR. |
| EWS.FederationTrust.TokenIssuerPrevCertReference | String | Token issuer previous certificate reference. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Archived | Boolean | Whether the token issuer previous certificate was archived. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.Critical | Boolean | Whether the extensions of the token issuer previous certificate was critical. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.Oid.FriendlyName | String | Friendly name of the OID of the extensions of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.Oid.Value | String | Value of the OID of the extensions of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.RawData | Number | Raw data of the extensions of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.SubjectKeyIdentifier | String | Subject key identifier of the extensions of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Extensions.KeyUsages | Number | Key usages of the extensions of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.FriendlyName | String | Friendly name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Handle.value | Number | The handle value of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.HasPrivateKey | Boolean | Whether the token issuer previous certificate has a private key. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Issuer | String | Issuer of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.IssuerName.Name | String | Name of the issuer of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.IssuerName.Oid.FriendlyName | Unknown | Friendly name of the OID of the issuer name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.IssuerName.Oid.Value | Unknown | Value of the OID of the issuer name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.IssuerName.RawData | Number | Raw Data of the issuer name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.NotAfter | Date | The date until when the token issuer previous certificate is valid. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.NotBefore | Date | The date the token issuer previous certificate became valid. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PrivateKey | Unknown | Private Key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedKeyValue.Oid.FriendlyName | String | Friendly name of the OID of the encoded key value of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedKeyValue.Oid.Value | String | Value of the OID of the encoded key value of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedKeyValue.RawData | Number | Raw data of the encoded key value of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedParameters.Oid.FriendlyName | String | Friendly name of the OID of the encoded parameters of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedParameters.Oid.Value | String | Value of the OID of the encoded parameters of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.EncodedParameters.RawData | Number | Raw data of the encoded parameters of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Key.KeyExchangeAlgorithm | String | Key exchange algorithm of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Key.LegalKeySizes.MaxSize | Number | Maximum size of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Key.LegalKeySizes.MinSize | Number | Minimum size of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Key.LegalKeySizes.SkipSize | Number | SkiPSize of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Key.SignatureAlgorithm | String | Signature algorithm of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Oid.FriendlyName | String | Friendly Name of the OID of the public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.PublicKey.Oid.Value | String | Value of the OID of teh public key of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.RawData | Number | Raw Data of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SerialNumber | String | Serial number of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SignatureAlgorithm.FriendlyName | String | Friendly name of the signature algorithm of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SignatureAlgorithm.Value | String | Value of the signature algorithm of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Subject | String | Subject of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SubjectName.Name | String | Name of the subject of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SubjectName.Oid.FriendlyName | Unknown | Friendly Name of the OID of the subject of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SubjectName.Oid.Value | Unknown | Value of the OID of the subject name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.SubjectName.RawData | Number | Raw data of the subject name of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Thumbprint | String | Thumbprint of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerPrevCertificate.Version | Number | Version of the token issuer previous certificate. |
| EWS.FederationTrust.TokenIssuerType | String | Token issuer type of the federation trust. |
| EWS.FederationTrust.TokenIssuerUri | String | Token Issuer UIR of the federation trust. |
| EWS.FederationTrust.WebRequestorRedirectEpr | String | Web requestor redirect EPR of the federation trust. |
| EWS.FederationTrust.WhenChanged | Date | The date the federation trust was changed. |
| EWS.FederationTrust.WhenChangedUTC | Date | The date in UTC format of when the federation trust was changed. |
| EWS.FederationTrust.WhenCreated | Date | The date the federation trust was created. |
| EWS.FederationTrust.WhenCreatedUTC | Date | The date in UTC format of when the federation trust was created. |

### ews-federation-configuration-get

***
Retrieves the Exchange organization's federated organization identifier and related details, such as federated domains, organization contact, and status.

#### Base Command

`ews-federation-configuration-get`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| domain_controller | The fully qualified domain name (FQDN) of the domain controller. For example, dc01.example.com. This argument is available only in on-premises Exchange. | Optional |
| identity | The federation trust ID. If not specified, all federation trusts configured for the Exchange organization are returned. | Optional |
| include_extended_domain_info | The IncludeExtendedDomainInfo switch specifies that the command query Microsoft Federation Gateway for the status of each accepted domain that's federated. The status is returned with each domain in the Domains property. Possible values: "true" and "false". Possible values are: true, false. Default is false. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.FederationConfiguration.AccountNamespace | String | Account namespace of the federation configuration. |
| EWS.FederationConfiguration.DefaultDomain | Unknown | Default domain of the federation configuration. |
| EWS.FederationConfiguration.DelegationTrustLink | String | Delegation trust link of the federation configuration. |
| EWS.FederationConfiguration.DistinguishedName | String | Distinguished name of the federation configuration. |
| EWS.FederationConfiguration.Domains | String | Domains of the federation configuration. |
| EWS.FederationConfiguration.Enabled | Boolean | Whether the federation configuration is enabled. |
| EWS.FederationConfiguration.ExchangeObjectId | String | Exchange object ID of the federation configuration. |
| EWS.FederationConfiguration.ExchangeVersion | String | Exchange version of the federation configuration. |
| EWS.FederationConfiguration.Guid | String | GUID of the federation configuration. |
| EWS.FederationConfiguration.Id | String | ID of the federation configuration. |
| EWS.FederationConfiguration.Identity | String | Identity of the federation configuration. |
| EWS.FederationConfiguration.IsValid | Boolean | Whether the federation configration is valid. |
| EWS.FederationConfiguration.Name | String | Name of the federation configuration. |
| EWS.FederationConfiguration.ObjectCategory | String | Object category of the federation configuration. |
| EWS.FederationConfiguration.ObjectClass | String | Object class of the federation configuration. |
| EWS.FederationConfiguration.ObjectState | String | Object state of the federation configuration. |
| EWS.FederationConfiguration.OrganizationContact | String | Organization contact of the federation configuration. |
| EWS.FederationConfiguration.OrganizationId | String | Organization ID of the federation configuration. |
| EWS.FederationConfiguration.OriginatingServer | String | Originating server of the federation configuration. |
| EWS.FederationConfiguration.PSComputerName | String | PowerShell computer name of the federation configuration. |
| EWS.FederationConfiguration.PSShowComputerName | Boolean | Whether to show the PowerShell computer name of the federation configuration. |
| EWS.FederationConfiguration.RunspaceId | String | Runspace ID of the federation configuration. |
| EWS.FederationConfiguration.WhenChanged | Date | The date the federation configuration was changed. |
| EWS.FederationConfiguration.WhenChangedUTC | Date | The date in UTC format of when the federation configuration was changed. |
| EWS.FederationConfiguration.WhenCreated | Date | The date the federation configuration was created. |
| EWS.FederationConfiguration.WhenCreatedUTC | Date | The date in UTC format of when the federation configuration was created. |

### ews-remote-domain-get

***
Gets the configuration information for the remote domains configured in your organization. This command is available only in the Exchange Online PowerShell V2 module.

#### Base Command

`ews-remote-domain-get`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| domain_controller | The fully qualified domain name (FQDN) of the domain controller. For example, dc01.example.com.<br/>This argument is available only in on-premises Exchange. | Optional |
| identity | The remote domain that you want to view. You can use the GUID, ID, or any other identifier. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.RemoteDomain.AdminDisplayName | String | Admin display name of the remote domain. |
| EWS.RemoteDomain.AllowedOOFType | String | Allowed OOF type of the remote domain. |
| EWS.RemoteDomain.AutoForwardEnabled | Boolean | Whether auto forward is enabled for the remote domain. |
| EWS.RemoteDomain.AutoReplyEnabled | Boolean | Whether auto reply is enabled for the remote domain.. |
| EWS.RemoteDomain.ByteEncoderTypeFor7BitCharsets | String | Byte encoder type For 7-bit charsets of the remote domain. |
| EWS.RemoteDomain.CharacterSet | String | Character set of the remote domain. |
| EWS.RemoteDomain.ContentType | String | Content type of the remote domain. |
| EWS.RemoteDomain.DeliveryReportEnabled | Boolean | Whether delivery report is enabled for the remote domain. |
| EWS.RemoteDomain.DisplaySenderName | Boolean | Whether to display the sender name for the remote domain. |
| EWS.RemoteDomain.DistinguishedName | String | Distinguished name of the remote domain. |
| EWS.RemoteDomain.DomainName | String | Domain name of the remote domain. |
| EWS.RemoteDomain.ExchangeObjectId | String | Exchange object ID of the remote domain. |
| EWS.RemoteDomain.ExchangeVersion | String | Exchange version of the remote domain. |
| EWS.RemoteDomain.Guid | String | GUID of the remote domain. |
| EWS.RemoteDomain.Id | String | ID of the remote domain. |
| EWS.RemoteDomain.Identity | String | Identity of the remote domain. |
| EWS.RemoteDomain.IsInternal | Boolean | Whether the remote domain is internal. |
| EWS.RemoteDomain.IsValid | Boolean | Whether the remote domain is valid. |
| EWS.RemoteDomain.LineWrapSize | String | Line wrap size for the remote domain. |
| EWS.RemoteDomain.MeetingForwardNotificationEnabled | Boolean | Whether meeting forward notification is enabled for the remote domain. |
| EWS.RemoteDomain.MessageCountThreshold | Number | Message count threshold  of the remote domain. |
| EWS.RemoteDomain.NDRDiagnosticInfoEnabled | Boolean | Whether NDR diagnostic information is enabled for the remote domain. |
| EWS.RemoteDomain.NDREnabled | Boolean | Whether NDR is enabled for the remote domain. |
| EWS.RemoteDomain.Name | String | Name of the remote domain. |
| EWS.RemoteDomain.NonMimeCharacterSet | String | Non-mime character set of the remote domain. |
| EWS.RemoteDomain.ObjectCategory | String | Object category of the remote domain. |
| EWS.RemoteDomain.ObjectClass | String | Object class of the remote domain. |
| EWS.RemoteDomain.ObjectState | String | Object state of the remote domain. |
| EWS.RemoteDomain.OrganizationId | String | Organization ID of the remote domain. |
| EWS.RemoteDomain.OriginatingServer | String | Originating server of the remote domain. |
| EWS.RemoteDomain.PSComputerName | String | PowerShell computer name of the remote domain. |
| EWS.RemoteDomain.PSShowComputerName | Boolean | Whether to show the PowerShell computer name for the remote domain. |
| EWS.RemoteDomain.PreferredInternetCodePageForShiftJis | String | Preferred internet code page for shift JIS for the remote domain. |
| EWS.RemoteDomain.RequiredCharsetCoverage | Unknown | Required charset coverage for the remote domain. |
| EWS.RemoteDomain.RunspaceId | String | Runspace ID for the remote domain. |
| EWS.RemoteDomain.TNEFEnabled | Unknown | Whether TNEF is enabled for the remote domain. |
| EWS.RemoteDomain.TargetDeliveryDomain | Boolean | Whether the remote domain is used for the target email address of mail users that represent the users in the other forest. |
| EWS.RemoteDomain.TrustedMailInboundEnabled | Boolean | Whether inbound trusted mail is enabled. |
| EWS.RemoteDomain.TrustedMailOutboundEnabled | Boolean | Whether outbound trusted mail is enabled. |
| EWS.RemoteDomain.UseSimpleDisplayName | Boolean | Whether to use the simple display name. |
| EWS.RemoteDomain.WhenChanged | Date | The date the remote domain was changed. |
| EWS.RemoteDomain.WhenChangedUTC | Date | The date in UTC format of when the remote domain was changed. |
| EWS.RemoteDomain.WhenCreated | Date | The date the remote domain was created. |
| EWS.RemoteDomain.WhenCreatedUTC | Date | The date in UTC format of when the remote domain was created. |

### ews-user-list

***
Displays the existing user objects in your organization.

#### Base Command

`ews-user-list`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| identity | The mailbox you want to view. | Optional |
| organizational_unit | The object's location in Active Directory by which to filter the results. | Optional |
| limit | Maximum number of users to get. A value of 0 means to get all users. Default is 10. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.User.AccountDisabled | Boolean | Whether the user account is disabled. |
| EWS.User.AllowUMCallsFromNonUsers | Boolean | Whether to allow Unified Messaging calls from non-users. |
| EWS.User.ArchiveRelease | String | The archive release of the user object. |
| EWS.User.AssistantName | String | The assistant name of the user object. |
| EWS.User.AuthenticationPolicy | Unknown | The authentication policy of the user object. |
| EWS.User.CanHaveCloudCache | Boolean | Whether the user object can have cloud cache. |
| EWS.User.City | String | The city of the user object. |
| EWS.User.CloudCacheAccountType | String | Cloud cache account type of the user object. |
| EWS.User.CloudCacheProvider | Number | Cloud cache provider of the user object. |
| EWS.User.CloudCacheRemoteEmailAddress | String | Cloud cache remote email address of the user object. |
| EWS.User.CloudCacheScope | Number | Cloud cache scope of the user object. |
| EWS.User.CloudCacheUserName | String | Cloud cache user name of the user object. |
| EWS.User.Company | String | Company of the user object. |
| EWS.User.ConsumerNetID | Unknown | Consumer net ID of the user object. |
| EWS.User.CountryOrRegion | String | Country or region of the . |
| EWS.User.DefaultMailboxWorkloadsMask | Unknown | Default mailbox workloads mask of the user object. |
| EWS.User.Department | String | Department of the user object. |
| EWS.User.DesiredMailboxWorkloads | Unknown | Desired mailbox workloads of the user object. |
| EWS.User.DesiredMailboxWorkloadsGracePeriod | Unknown | Desired mailbox workloads grace period of the user object. |
| EWS.User.DesiredMailboxWorkloadsModified | Unknown | Modified desired mailbox workloads of the user object. |
| EWS.User.DisplayName | String | Display name of the user object. |
| EWS.User.DistinguishedName | String | Distinguished name of the user object. |
| EWS.User.ExchangeObjectId | String | Exchange object ID of the user object. |
| EWS.User.ExchangeVersion | String | Exchange version of the user object. |
| EWS.User.ExternalDirectoryObjectId | String | External Directory Object ID of the user object. |
| EWS.User.Fax | String | Fax of the user object. |
| EWS.User.FirstName | String | First name of the user object. |
| EWS.User.GeoCoordinates | Unknown | Geo coordinates of the user object. |
| EWS.User.Guid | String | GUID of the user object. |
| EWS.User.HomePhone | String | Home phone of the user object. |
| EWS.User.Id | String | ID of the user object. |
| EWS.User.Identity | String | Identity of the user object. |
| EWS.User.Initials | String | Initials of the user object. |
| EWS.User.IsCloudCache | Boolean | Whether there is a cloud cache for the user object. |
| EWS.User.IsCloudCacheBlocked | Boolean | Whether the cloud cache is blocked. |
| EWS.User.IsCloudCacheProvisioningComplete | Boolean | Whether cloud cache provisioning is complete. |
| EWS.User.IsDirSynced | Boolean | Whether the directory is synched. |
| EWS.User.IsInactiveMailbox | Boolean | Whether the mailbox is inactive. |
| EWS.User.IsLinked | Boolean | Whether the user object is linked. |
| EWS.User.IsSecurityPrincipal | Boolean | Whether there is a security principal. |
| EWS.User.IsSoftDeletedByDisable | Boolean | Whether soft delete is disabled and hard \(permanent\) delete occurs. |
| EWS.User.IsSoftDeletedByRemove | Boolean | When the Exchange Online mailbox is deleted \(soft delete\), this property is set to True. |
| EWS.User.IsValid | Boolean | Whether the user object is valid. |
| EWS.User.LastName | String | Last name of the user object. |
| EWS.User.LegacyExchangeDN | String | Legacy exchange distinguished name of the user object. |
| EWS.User.LegalAgeGroup | Unknown | Legal age group of the user object. |
| EWS.User.LinkedMasterAccount | String | Linked master account of the user object. |
| EWS.User.MailboxLocations | String | Mailbox locations of the user object. |
| EWS.User.MailboxProvisioningConstraint | Unknown | Mailbox provisioning constraint of the user object. |
| EWS.User.MailboxRegion | Unknown | Mailbox region of the user object. |
| EWS.User.MailboxRegionLastUpdateTime | Unknown | Last time the mailbox region  of the user object was updated. |
| EWS.User.MailboxRegionSuffix | String | Mailbox region suffix of the user object. |
| EWS.User.MailboxRelease | String | Mailbox release of the user object. |
| EWS.User.MailboxWorkloads | String | Mailbox workloads of the user object. |
| EWS.User.Manager | Unknown | Manager of the user object. |
| EWS.User.MicrosoftOnlineServicesID | String | Microsoft Online Services ID of the user object. |
| EWS.User.MobilePhone | String | Mobile phone of the user object. |
| EWS.User.Name | String | Name of the user object. |
| EWS.User.NetID | String | Network ID of the user object. |
| EWS.User.Notes | String | Notes for the user object. |
| EWS.User.ObjectCategory | String | Object category of the user object. |
| EWS.User.ObjectClass | String | Object class of the user object. |
| EWS.User.ObjectState | String | Object state of the user object. |
| EWS.User.Office | String | Office of the user object. |
| EWS.User.OrganizationId | String | Organization ID of the user object. |
| EWS.User.OrganizationalUnit | String | Organizational unit of the user object. |
| EWS.User.OriginatingServer | String | Originating server of the user object. |
| EWS.User.PSComputerName | String | PowerShell computer name of the user object. |
| EWS.User.PSShowComputerName | Boolean | Whether to show the PowerShell computer name of the user object. |
| EWS.User.Pager | String | Pager of the user object. |
| EWS.User.Phone | String | Phone of the user object. |
| EWS.User.PhoneticDisplayName | String | Phonetic display name of the user object. |
| EWS.User.PostalCode | String | Postal Code of the user object. |
| EWS.User.PreviousRecipientTypeDetails | String | Details of the previous recipient type of the user object. |
| EWS.User.RecipientType | String | Recipient type of the user object. |
| EWS.User.RecipientTypeDetails | String | Details of the recipient type of the user object. |
| EWS.User.RemotePowerShellEnabled | Boolean | Whether remote PowerShell is enabled for the user object. |
| EWS.User.ResetPasswordOnNextLogon | Boolean | Whether to reset the password on next logon. |
| EWS.User.RunspaceId | String | Runspace ID of the user object. |
| EWS.User.SKUAssigned | Boolean | Whether SKU is assigned. |
| EWS.User.SamAccountName | String | sAMAccountName of the user object. |
| EWS.User.SeniorityIndex | Unknown | Seniority index of the user object. |
| EWS.User.Sid | String | SID of the user object. |
| EWS.User.SimpleDisplayName | String | Simple display name of the user object. |
| EWS.User.StateOrProvince | String | State or province of the user object. |
| EWS.User.StreetAddress | String | Street address of the user object. |
| EWS.User.StsRefreshTokensValidFrom | Date | The validation start date for the Security Token Service \(STS\) refresh tokens of the user object. |
| EWS.User.TelephoneAssistant | String | Telephone assistant of the user object. |
| EWS.User.Title | String | Title of the user object. |
| EWS.User.UMDialPlan | Unknown | Unified Messaging \(UM\) dial plan of the user object. |
| EWS.User.UMDtmfMap | String | Unified Messaging \(UM\) dual tone multi-frequency \(DTMF\) map of the user object. |
| EWS.User.UpgradeDetails | Unknown | Upgrade details of the user object. |
| EWS.User.UpgradeMessage | Unknown | Upgrade message of the user object. |
| EWS.User.UpgradeRequest | String | Upgrade request of the user object. |
| EWS.User.UpgradeStage | Unknown | Upgrade stage of the user object. |
| EWS.User.UpgradeStageTimeStamp | Unknown | Upgrade stage time stamp of the user object. |
| EWS.User.UpgradeStatus | String | Upgrade status of the user object. |
| EWS.User.UserAccountControl | String | User account control of the user object. |
| EWS.User.UserPrincipalName | String | User principal name of the user object. |
| EWS.User.WebPage | String | Web page of the user object. |
| EWS.User.WhenChanged | Date | The date the user object was changed. |
| EWS.User.WhenChangedUTC | Date | The date in UTC format of when the user object was changed. |
| EWS.User.WhenCreated | Date | The date the user object was created. |
| EWS.User.WhenCreatedUTC | Date | The date in UTC format of when the user object was created. |
| EWS.User.WhenSoftDeleted | Unknown | When the user object was soft deleted. |
| EWS.User.WindowsEmailAddress | String | Windows email address of the user object. |
| EWS.User.WindowsLiveID | String | Windows live ID of the user object. |
| EWS.User.DirectReports | String | Direct reports of the user object. |

### ews-mailbox-audit-bypass-association-list

***
Retrieves information about the AuditBypassEnabled property value for user accounts (on-premises Exchange and the cloud) and computer accounts (on-premises Exchange only).

#### Base Command

`ews-mailbox-audit-bypass-association-list`

#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| identity | The mailbox you want to view. | Optional |
| domain_controller | The domain controller that's used by this cmdlet to read data from or write data to Active Directory. You identify the domain controller by its fully qualified domain name (FQDN). This argument is available only in on-premises Exchange. | Optional |
| limit | Maximum number of users to get. A value of 0 means to get all users. Default is 10. | Optional |

#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| EWS.MailboxAuditBypassAssociation.AuditBypassEnabled | Boolean | Whether the mailbox audit bypass association is enabled. |
| EWS.MailboxAuditBypassAssociation.DistinguishedName | String | Distinguished name of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ExchangeObjectId | String | Exchange object ID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ExchangeVersion | String | The version of the exchanged server. |
| EWS.MailboxAuditBypassAssociation.Guid | String | The GUID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.Id | String | ID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.Identity | String | The unique identity of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.IsValid | Boolean | Whether the mailbox audit bypass association property is enabled. |
| EWS.MailboxAuditBypassAssociation.Name | String | Name of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ObjectCategory | String | Object category of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ObjectClass | String | Object class of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ObjectId | String | Object ID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.ObjectState | String | Object state of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.OrganizationId | String | Organization ID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.OriginatingServer | String | Originating server of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.PSComputerName | String | PowerShell computer name of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.PSShowComputerName | Boolean | Whether to show the computer name of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.RunspaceId | String | Runspace ID of the mailbox audit bypass association. |
| EWS.MailboxAuditBypassAssociation.WhenChanged | unknown | The date the mailbox audit bypass association was changed. |
| EWS.MailboxAuditBypassAssociation.WhenChangedUTC | Date | The date in UTC of when the mailbox audit bypass association was changed. |
| EWS.MailboxAuditBypassAssociation.WhenCreated | Date | The date the mailbox audit bypass association was created. |
| EWS.MailboxAuditBypassAssociation.WhenCreatedUTC | Date | The date in UTC format of when the mailbox audit bypass association was created. |
