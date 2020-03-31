# JAMFSoftwareServer.log Messages
This repo contains a list of messages which can appear in the JAMFSoftwareServer.log, with a note as to what action might be required due to the messages being generated.

Please reach out to Jamf Support if any questions, & Pull Requests accepted to add more messages.

The log message snippets are displayed in the 1st column, scroll to the right for any recommended steps.

The messages have an assigned status, as per:

| Status | Description |
|:---:|---|
|:boom:|Severe events which needs immediate attention.|
|:bomb:|Events which if left unchecked can lead to service disruption.|
|:warning:|An event that needs attention, but is not service affecting.|
|:ok:| Non-issue, an informative event.|


|Log Message Snippets | Status | PI | Detail |
|:---|:---:|:---:|---|
|`Fatal error logged during server initialization`|:boom:||JPS failed to startup, further investigation needed as to why.|
|`APNs Certificate is expired.`|:boom:||Renew the APNS certificate.|
|`[llmentProgramDeviceHelper] - 403 The organization has not accepted latest Terms and Conditions of the program`|:bomb:||Login to ABM/ASM to accept the new terms & conditions.|
|`[ERROR] [duledPool-8] [ntInstanceSyncCommService] - com.jamfsoftware.jss.objects.streamlinedenrollment.service.DeviceEnrollmentProgramException: The DEP service reported an error.`|:bomb:||Download a new token from ABM/ASM & upload into the JPS.|
|`[ntInstanceSyncCommService] - com.jamfsoftware.jss.objects.streamlinedenrollment.service.DeviceEnrollmentProgramException: An error occurred during oauth token refresh`|:bomb:||Either renew DEP token or login to ABM/ASM to accept the new terms & conditions.|
|`[CMSCipher                ] - Unable to decrypt data`|:bomb:|PI-006374|macOS - Configuration Profile - Security & Privacy Payload - FileVault Recovery Keys cannot be viewed if Certificate expired. Observation : Filevault 2 recovery keys are displayed in the JPS as a long string with hundreds of characters starting with 'MIAGCSqGSIb3DQEHA6CAMIACAQAxgg'. Jamf Pro has a FILEVAULT2COMM certificate in the database, and it is valid for 5 years. In case it needs to be renewed we hit this issue. Fixed with JPS 10.10. Can be manually resolved by we creating a new configuration profile with Security & Privacy Payload with Enable Escrow Personal Recovery Key selected and use a reissue script (https://github.com/JAMFSupport/FileVault2_Scripts/blob/master/reissueKey.sh)|
|`[eEnrollmentProgramRequest] - javax.net.ssl.SSLHandshakeException: Received fatal alert: decode_error`|:bomb:|PI-007508|TLS 1.3 needs to be disabled in JAVA_OPTS or CATALINA_OPTS, fixed with OpenJDK 11.0.5|
|`[licationListUpdateService] - Unable to compare the catalog version '1.5.1' with the ApplicationVersion '' or ApplicationShortVersion '' for the app 'com.adobe.rush', so not updating`|:bomb:|PI-007928|Catalog version and app name can vary. VPP Apps are unable to be updated on devices due to unknown version comparision issue. Contact Jamf Support|
|`[VppCommService] Client context for admin account doesn't match`|:bomb:||The VPP token may be in use by another server. The VPP token can be reclaimed.|
|`[llmentProgramDeviceHelper] - 403: token_expiredForbidden`|:warning:||Remove or renew the offending token.|
|`[llmentProgramDeviceHelper] - 403: token_rejectedForbidden`|:warning:||Remove or renew the offending token.|
|`[llmentProgramDeviceHelper] - 400 response from Device Enrollment Program indicating one of the following:`|:warning:||DEP token has been downloaded from ABM/ASM but not uploaded to JPS. Remove or renew the offending token.|
|`[VppAccountCreator        ] - Failed to register user com.jamfsoftware.vpp.User@6f7dd030[id=xxx,managedAppleId=some@appleid] to VPP invitation`|:warning:||The some@appleid needs to login to ABM/ASM & accept the Terms & Conditions for their account |
|`[llmentProgramDeviceHelper] - 403: SERVER_INACTIVE` |:warning:||There is a DEP PreStage with a token in use which no longer has a location in ABM/ASM. Remove the offending PreStage|
|`[llmentProfileSynchronizer] - com.jamfsoftware.jss.objects.streamlinedenrollment.service.DeviceEnrollmentProgramException: The DEP service reported that invalid options were submitted.`|:warning:||An ABM DEP token has been assigned to a DEP PreStage for Shared iPad, correct the DEP token in use by the PreStage or remove the Shared iPad settings.|
|`Error parsing iTunes ID: String index out of range`|:warning:|PI-000867|This is due to self hosted iBooks being added to Jamf Pro. To stop getting these messages: Disable Populate Purchased VPP Content from Settings > Global > VPP > Content, for each token in use.|
|`[Root exception is javax.net.ssl.SSLHandshakeException: PKIX path validation failed: java.security.cert.CertPathValidatorException: validity check failed]`|:warning:||LDAPS certificate validity check failed. Check certificate presented to JPS|
|`javax.naming.AuthenticationNotSupportedException: [LDAP: error code 8 - 00002028: LdapErr: DSID-0C09023C, comment: The server requires binds to turn on integrity`|:warning:||LDAP connection requires SSL/TLS|
|`javax.naming.CommunicationException: <ldap.server:port> [Root exception is java.net.SocketTimeoutException: connect timed out]`|:warning:||Connection to LDAP <ldap.server:port> timed out. Up LDAP time out time & investigate if persists.|
|`javax.naming.CommunicationException: <ldap.server:port> [Root exception is java.net.SocketTimeoutException: Read timed out]`|:warning:||Connection to LDAP <ldap.server:port> timed out. Up LDAP time out time & investigate if persists.|
|`[ComputerHelper ] java.lang.IllegalArgumentException: Invalid UUID string:`|:warning:|| A macOS device has an invalid record in Jamf Pro.  If no smart groups calculate correctly, then this would be an issue with a managed device. If smart groups are fine, but searches fail then look for any unmanaged devices. Delete the offending device once found.|
|`[Root exception is java.net.SocketException: Connection reset]`|:warning:||Connectivity issue with to LDAP(s), investigation needed.|
|`[Root exception is java.net.ConnectException: Connection refused (Connection refused)]`|:warning:||Connectivity issue with to LDAP(s), investigation neded.|
|`[Root exception is javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target]`|:warning:||LDAP SSL certificate not trusted by JPS|
|`javax.naming.CommunicationException: <ldap server fqdn>:<ldap server port> [Root exception is javax.net.ssl.SSLHandshakeException: java.security.cert.CertificateException: javax.net.ssl.SSLException: hostname in certificate didn't match: <ldap server fqdn> != <ldap server fqdn> OR <ldap server fqdn>]`|:warning:||FQDN for LDAPS does not match FQDN entries within cert.|
|`[VppCommService           ] - Error clearing email for user:`|:warning:||Error is due to aother MDM trying to assign content to the user or having tried previously. This might be cleared via the VPP API directly but can also be ignored.
|`[lientCommunicationServlet] - Comm Device null.`|:warning:||A device which is no longer managed via the JPS is contacting the JPS. The message body cointains MAC & remote IP details so might be traceable if a concern, but can also be ignored.|
|`[JAMFPushProxyConnection ] - Jamf Push Proxy returned status of: 400`|:warning:||Informational message, can be ignored as long as Push Proxy is correctly renewing.|
|`[WARN ] [neralPool-0] [PushQueueManager ] - Unable to flush connection com.jamfsoftware.jss.pushnotification.connections.JAMFPushProxyConnection`|:warning:||Informational message, can be ignored as long as Push Proxy is correctly renewing.|
| `CSRF risk found. Denying request.` |:warning:|PI-007743|“Jamf Pro CloudDataSupplier configuration points to a URL that is no longer in service”  |
|`[VppEmailMonitor ] - Running email monitor`|:ok:||Informational message, not actionable.|
|`[VppEmailMonitor          ] - Email monitor completed after x.xx seconds`|:ok:||Informational message, the seconds taken can be of use when troubleshooting.|
|`[PushNotificationUtility  ] - x invalid tokens were submitted to be pushed.`|:ok:||Informational message, not actionable.|
|`[FeedbackTokenFactory     ] - 1 feedback tokens were found:`|:ok:||Informational message, not actionable.|
|`[EduMonitor1] [ucationProfileFactoryImpl] - Computer is not the correct macOS Version to have Edu Profile:`|:ok:||Informational message, not actionable.|
|`[EduMonitor1] [ucationProfileFactoryImpl] - Device is not the correct iOS Version to have Edu Profile:`|:ok:||Informational message, not actionable.|
|`[rentProfileCleanupMonitor] - Running parent profile cleanup`|:ok:||Informational message, not actionable.|


