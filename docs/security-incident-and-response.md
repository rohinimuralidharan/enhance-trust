# Security Incident and Response

## Security incident monitoring
The following suspected or actual events or activities should be monitored for:

- Triggering of tamper evident or response functions in the HSM

- Physical non availability of HSM, card reader, card sets, client application, %NFAST_KMDATA% folder contents, nShield Connect config file, SIEM collector data, backup data

- Logical non availability of HSM, card reader, card sets, client application, %NFAST_KMDATA% folder contents, nShield Connect config file, SIEM collector data, backup data

- Gaps or unexplained entries in the logs, or suspected log tamper

- Evidence of access control violation contrary to any security policy, for example lost token and subsequent logon.

- Evidence of unauthorized use

- Evidence of network attacks on the HSM

- Evidence of excessive performance demands

- Evidence of violation of environmental controls

- Unauthorized changes to configuration settings for HSM and client application, such as updating the module’s clock.

- Non-compliance with security process, such as commissioning on an open network

- Non-compliance with security policy, such as using incorrect algorithm strength or continuing to use a key outside of its cryptoperiod.

## Security incident management
If a security incident is suspected the Company Security Officer should be alerted immediately and determine which actions must be implemented as advised by your Security Incident and Response Policy. This should cover the following areas:

- Quarantine area, isolate unit and evidence preservation – witnessed snapshot of unit (this should cover determining whether to power off the unit which may result in the loss of evidence against the need to isolate any potential malware resident on the unit)

- Investigation

- Reporting structure and timescales.

## Security incident impact and response
The sections below identifies the impact of various compromises on keys or secrets and the recovery action required.

Under Recovery Action the term revoke is used to indicate that the compromised key must no longer be trusted or used. The terms revoke or revocation are normally used in regard to digital certificates (normally containing public keys), where methods exist to indicate that a certified key can no longer be trusted. However, this manual will apply the term to all compromised keys.

### Compromised Key or Secret: A brute force attack on blobbed key outside of module

### Impact

Application key is compromised and must not be used. The application key will be one of the following types:

- OCS protected application keys

- Softcard protected application keys

- Module/Module Pool protected application keys

#### Recovery Action

Revoke application key and destroy the Security World, since all application keys in this Security World must now be considered as compromised.

Destruction of the Security World is achieved by erasing/destroying the ACS and re-initializing all the HSMs to a different Security World (with a new ACS).

Alternatively, to mitigate the present threat, the HSMs can be put into pre-initialization mode whilst business recovery procedures are implemented prior to creating a new Security World.

Note that erasing the ACS will prevent a lost/stolen backup being reloaded on to a new HSM.

### Compromised Key or Secret: Attacker has subverted memory of HSM

##### Impact

Application key is compromised and must not be used. The application key will be one of the following types:

- OCS protected application keys

- Softcard protected application keys

- Module/Module Pool protected application keys

#### Recovery Action

Revoke application key and destroy the Security World, since all application keys in this Security World must now be considered as compromised.

Destruction of the Security World is achieved by erasing/destroying the ACS.

Destroy the HSM as its integrity can no longer be guaranteed.

Create a different Security World on new/different HSMs (with a new ACS).

Note that erasing the ACS will prevent a lost/stolen backup being reloaded on to a new HSM.

### Compromised Key or Secret: passphrase for softcard is compromised

#### Compromise Type

Lost or observed

#### Impact

The application keys protected by the softcard should be considered as under the control of the attacker

#### Recovery Action

Revoke application key protected by softcard. If unable to revoke the key, isolate the HSM so that no system can use it.

Erase all copies of blobs associated with the application key protected by softcard in kmdata/rfs/backups to prevent attacker trying to use keys with stolen passphrase.

Create replacement application keys under new softcards.

### Compromised Key or Secret: A quorum of OCS cards is compromised

#### Compromise Type

Lost or stolen

#### Impact

The application keys protected by the OCS are under the control of the attacker

#### Recovery Action

Revoke application keys protected by OCS.

If unable to revoke keys isolate HSM so that no system can use it. Erase blobs associated with the application keys protected by the OCS in kmdata/rfs/backups to prevent attacker trying to use keys with stolen OCS.

Create replacement application keys under new OCS.

If the OCS cards are subsequently recovered they should either be erased or destroyed.

### Compromised Key or Secret: A quorum of ACS cards is compromised
#### Compromise Type

Lost or stolen

#### Impact

The Security World protected by the ACS should be considered as under the control of the attacker

#### Recovery Action

Revoke all Module-protected/module pool-protected application keys and recoverable application keys, as these are available to an attacker with a KMData archive, an HSM and the compromised ACS.

Revoke all non-recoverable OCS or Softcard protected keys because they cannot be used without the Security World, which has been compromised and should be destroyed.

Alternatively, to mitigate the present threat, the HSM can be put into pre-initialization mode whilst business recovery procedures are implemented prior to creating a new Security World.

The original Security World must not be used. Destroy the world file and any backups to prevent an attacker recovering the Security World with the ACS, world file and backups.

If the ACS cards are subsequently recovered they should be either erased or destroyed.

Create replacement application keys.

### Compromised Key or Secret: Soft KNETI

#### Compromise Type

Attacker has subverted client memory

#### Impact

KNETI is compromised and must not be used

#### Recovery Action

For every Connect that the affected client has communicated with, use the Front Panel or serial console to remove the client’s configuration data.

For any RFS that the affected client has communicated with, update the RFS’s configuration file to remove the client’s configuration data.

Manually delete the kneti file identified as kneti-hardserver.

- On Windows, it is stored in C:\ProgramData\nCipher\Key Management Data\hardserver.d\.

- On Linux, is stored in /opt/nfast/kmdata/hardserver.d/.

Reboot the client.

Isolate client and investigate compromise.

Once resolved re-configure the Connects/RFS that this client communicated with using same client’s IP address/ESN, but the new KNETI hash, and then re-establish the secure channel to the Connect(s)/RFS.

### Compromised Key or Secret: nToken KNETI

#### Compromise Type

A brute force attack on KNETI file held in the Connect’s KMData folder.

#### Impact

KNETI is compromised and must not be used

#### Recovery Action

For every Connect that the affected client has communicated with, use the Front Panel or serial console to remove the client’s configuration data

For any RFS that the affected client has communicated with, update the RFS’s configuration filer to remove the client’s configuration data.

Manually delete the kneti file identified as kneti-nToken ESN.

- On Windows, it is stored in: C:\ProgramData\nCipher\Key Management Data\hardserver.d\.

- On Linux, is stored in /opt/nfast/kmdata/hardserver.d/.

Destroy the nToken as its integrity can no longer be guaranteed.

If you have an alternative nToken available, configure it to communicate with an nShield Connect. If you do not have an alternative nToken available, you will need to use software-based authentication instead. See Basic HSM, RFS and client configuration for more information.

### Compromised Key or Secret: nShield Connect KNETI

#### Compromise Type

A brute force attack on KNETI file held in the nShield Connect’s KMData folder.

#### Impact

KNETI is compromised and must not be used

#### Recovery Action

Remove the compromised Connect’s data (IP address/ KNETI and H(KNETI)) from any client hardserver’s configuration file that has communicated with the compromised Connect.

Destroy the nShield Connect as its integrity can no longer be guaranteed.

Configure a new nShield Connect to communicate with a client.

### Compromised Key or Secret: Soft KNETI
#### Compromise Type

A brute force attack on KNETI file held in obfuscated form in the KMData folder

#### Impact

KNETI is compromised and must not be used

#### Recovery Action

For every Connect that the affected client has communicated with, use the Front Panel or serial console to remove the client’s configuration data.

For any RFS that the affected client has communicated with, update the RFS’s configuration filer to remove the client’s configuration data.

Manually delete the kneti file identified as kneti-hardserver.

- On Windows, is stored in C:\ProgramData\nCipher\Key Management Data\hardserver.d\.

- On Linux, is stored in /opt/nfast/kmdata/hardserver.d/.

Reboot the client.

Isolate client and investigate unauthorized access to the KMData file and the integrity of the client.

Once resolved re-configure the Connects/RFS that this client communicated with using same client’s IP address/ESN, but the new KNETI hash, and then re-establish the secure channel to the Connect(s)/RFS.

### Compromised Key or Secret: nToken KNETI
#### Compromise Type

A brute force attack on KNETI encrypted blob held in KNETI file in the KMData folder.

#### Impact

KNETI is compromised and must not be used

#### Recovery Action

For every Connect that the affected client has communicated with, use the Front Panel or serial console to remove the client’s configuration data.

For any RFS that the affected client has communicated with, update the RFS’s configuration filer to remove the client’s configuration data.

Manually delete the kneti file identified as kneti-nToken ESN.

- On Windows, it is stored in C:\ProgramData\nCipher\Key Management Data\hardserver.d\.

- On Linux, it is stored in /opt/nfast/kmdata/hardserver.d/.

Reset the nToken.

Isolate client and investigate unauthorized access to KMData file and integrity of client.

Once resolved re-configure the Connects/RFS that this client communicated with using same client’s IP address/ESN, but the new KNETI hash, and then re-establish the secure channel to the Connect(s)/RFS.

An attack on the client host system to disclose the client SSH private keys or recovering private SSH client keys from backups

#### Impact

An attacker can access the HSM and run platform services using the compromised keys.

#### Recovery Action

Destroy the nShield Connect because its integrity can no longer be guaranteed. Configure a new nShield Connect to communicate with a client. Recreate the client and server HSM keys.

Factory reset the HSM to prevent access with the compromised client SSH keys. Isolate the client host system and investigate the unauthorized access to the client host system or backup. Re-enroll the client host system with the HSM to create new client SSH keys.

### Compromised Key or Secret: Imported application keys
Application keys can also be imported into the HSM. Any secret/private application key imported in plaintext should be treated as potentially compromised if:

- The confidentiality and integrity of an imported secret/private key cannot be verified

- The provenance of the key is unknown (it has not come from a trusted party).

### Deleting a Security World
You can re-initialise an HSM to use a new Security World if, for example, you believe that your existing Security World has been compromised. This must be done for all HSMs that hosted the old Security World, however:

- You are not able to access any keys that you previously used in a deleted Security World

- It is recommended that you reformat any standard nShield cards that were used as Operator Cards within this Security World before you delete it.

!!! note "**Note**"
Except for nShield Remote Administration Cards, if you do not reformat the smart cards used as Operator Cards before you delete your Security World, you must throw them away because they cannot be used, erased, or reformatted without the old Security World key.

You must reformat, reuse or destroy the smart cards from a deleted Security World’s ACS. If these cards are not overwritten or destroyed, then an attacker with these smart cards, a copy of your data (for example, a weekly backup) and access to any nShield HSM can access your old keys.

### Module failure
If a module fails and cannot be factory reset then application keys protected by module keys and NVRAM keys are potentially vulnerable to attack. In this instance procedural and technical access controls should be deployed to protect the module until secure destruction of the module occurs as described in Decommission and Disposal.

### Tamper incident
Physical Security provides guidance on the physical security controls available on the different nShield platforms and the procedural controls required to maintain those physical security controls across the product’s lifecycle.

If a tamper incident is observed the guidance in Security Incident and Response should be followed to manage the incident. The investigation will determine the extent of the attack. Once an HSM has been confirmed as being tampered its integrity can no longer be assured and it should be decommissioned and disposed of — see Decommission and Disposal for more information.

However, there are two instances where it is possible to recover the module from a tamper event. These are:

- nShield Solo XC tamper events - see nShield Solo XC physical security controls for more information.

- nShield Connect lid is either open or closed - see Tamper event for guidance on how to investigate the tamper and the criteria required to recover from the tamper. The occurrence of the event should be recorded and recovery authorized in accordance with the Customer’s Security Incident and Response Policy.