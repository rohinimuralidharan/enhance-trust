# Operation
For additional procedural guidance on operational issues (describing how the HSM should be initially configured/setup), see Commissioning. Refer to that chapter as well for operational guidance.

## Patching policy
A patching policy should be defined and actively implemented. Specifically:

- The latest version of the nShield firmware/software should be installed.

- Any host operating systems should be a current, supported version with the latest patches applied.

- It is recommended that anti-virus software is installed on the host system and maintained with automatic updates.

## Set the RTC time
Set the nShield Edge, nShield Solo and nShield Connect RTC using an accurate trusted local time source at regular intervals to mitigate any clock drift.

## Operator Card Set (OCS) quorum configurations
### Share keys across multiple HSMs
An OCS can enable the same keys for use in a number of different HSMs at the same time.

However, it does mean that if you have a non-persistent OCS, you have to leave one of the cards in an appropriate card slot of each HSM. If you have created an OCS in which K is more than half of N, you can share the keys it protects simultaneously amongst multiple modules as long you have enough unused cards to form a K/N quorum for the additional HSMs. For example, with a 3/5 OCS, you can load keys onto 3 HSMs because, after loading the key on the first device, you still have 4 cards left. After loading the key on a second device, you still have 3 cards left. After loading the key onto a third device, you have only 2 cards left, which is not enough to create the quorum required to load the key onto a fourth device.

However, in this instance, the guidance outlined in Access Control regarding security controls for cards is not implemented. The loss or theft of the quorum of cards means that all keys protected by the OCS are vulnerable. Therefore, a threat analysis should identify the additional logical and physical controls required to protect the OCS left in the card reader.

Alternatively a less secure method of using OCS-protected keys across multiple HSMs is to set:

- K to 1

- N at least equal to the number of the HSMs you want to use.

You can then insert single cards from the OCS into the appropriate card slot of each HSM to authorize the use of that key.

To mitigate the risk of card failure consider setting N to a greater number than the number of HSMs. In the event of failure the spare OCS card is retrieved from its secure location and is deployed whilst arrangements are made to create a new OCS to replace the existing one.

However, the guidance outlined in Creating and maintaining a quorum regarding quorum ratios and security controls for cards is not implemented. The misuse, loss or theft of one card means that all keys protected by the OCS are vulnerable. Therefore, a threat analysis must identify the additional logical and physical access controls required to protect the OCS left in the card reader.

An alternative strategy to the configurations listed above is to use a persistent OCS or a persistent OCS with a time-out. However, both of these options reduce the control the user has over keys once the OCS has been loaded. A threat analysis should determine which configuration of persistence/non-persistence/time-out/no time-out is appropriate for the various sets of keys protected by OCSs.

## Share key between users
To share the same OCS-protected key to a set of users, set:

- K to 1

- N equal to the number of users.

You can then give each user a single card from the OCS, enabling those users to authorize the use of that key. However, in this instance, the guidance outlined in Creating and maintaining a quorum regarding quorum ratios for cards is not implemented. The misuse, loss or theft of one card means that all keys protected by the OCS are vulnerable. Therefore, a threat analysis should identify the additional logical and physical controls required to protect the loss of one card.

## NVRAM key storage
Application keys can be stored within the nonvolatile memory of a suitable HSM. This functionality is provided exclusively for regulatory reasons. NVRAM-stored keys provide no additional security benefits and their use exposes your ACS to increased risk. Storing keys in nonvolatile memory also reduces load-balancing and recovery capabilities. Because of these factors, we recommend you always use standard Security World keys unless you are explicitly required to use NVRAM-stored keys.

Any backup and recovery procedures for NVRAM-stored keys must be consistent with regulatory requirements. Do NOT back-up keys to a smart card, as the keys would no longer be stored solely within the physical boundary of the HSM.

## Config push feature
The config push feature allows the nShield Connect configuration to be updated remotely, that is, without access to the nShield Connect front panel. Therefore, anyone with access to the RFS and/or designated client can change the nShield Connect configuration using cfg-pushnethsm. If config push is not required, it should be disabled via the nShield Connect front panel. If this feature is required, conduct a threat analysis to determine if this option is a security risk and confirm if any security controls are required.

## Security World replacement options
If you replace an existing Security World, its %NFAST_KMDATA%\local directory is not overwritten but renamed %NFAST_KMDATA%\local_N (where N is an integer assigned depending on how many Security Worlds have been previously saved during overwrites). A new Key Management Data directory is created for the new Security World. If you do not wish to retain the %NFAST_KMDATA%\local_N directory from the old Security World, you must delete it manually.

## Host platform and client applications
The nShield security model assumes that the security of the client endpoint, including any client applications, is completely under the customer’s control, and that the host platform is physically protected and hardened in accordance with the customer’s Security Policy. Additional security controls may be put in place to reduce the client machine’s attack surface, including file encryption and implementation of mandatory access controls. However, these are entirely at the discretion of the customer deploying the system, and will be guided by their threat analysis.

The following assumptions are made of the host platform and client application to ensure secure operation of the HSM:

- Client applications, users running nShield utilities, and any user on the host platform are assumed to be trusted, and will not perform malicious actions which would be detrimental to the security of the HSM, or the security concerns of the HSM’s operator.

- The administrator of the host platform should ensure that only authorised and trusted users are allowed access to the HSM’s nCore API, and that access to the HSM’s privileged nCore API is only provided to users that definitely need access to these HSM security significant commands.

- The administrator of the host platform should implement platform service user separation (as outlined in SSH Client Key Protection (nShield 5s HSMs)), so that only specific authorised and trusted users will have access to nShield 5 platform services.

    - It is recommended that ssh private authentication keys, on the host platform, for the different nShield 5 platform services are encrypted, as outlined in SSH client key encryption.

- The administrator of the host platform should ensure that it has any necessary security patches loaded, is free from unapproved software, and is protected against malware.

- Any client application using the cryptographic services of the HSM should securely gather identification and authentication data from its users and securely transfer it to the HSM when authorizing the use of HSM assets and services.

- Any client application using the cryptographic functions of the HSM should ensure that the correct data are supplied in a secure manner (including any relevant requirements for authenticity, integrity and confidentiality).

- Any client application using the cryptographic functions of the HSM should protect the security concerns of its users, and comply with the user’s security policies concerning key and sensitive data management.

- The client should ensure that the host platform running their application contains an available random number generator of sufficient quality to allow the generation of keys that contain enough entropy as to be unpredictable.

- The client should ensure that all cryptographic keys generated or cryptographic algorithm used on the host platform conform to specifications that are endorsed by recognized security authorities.

- The strength of client keys used to build a secure channel to the HSM must meet the strength specified in NIST SP800-131A Revision 2.

## Preload utility
The preload utility preloads keys, for example, when using a PKCS #11 application. For a description, see Preload Utility.

The use of preload is not recommended if any of the following apply on the client/host platform:

- Standard user or group file permissions, that is, file discretionary access control, can be circumvented. For example, on Linux, via users being permitted to execute sudo commands.

- The threat exists that a malicious user (attacker) can gain unauthorized access to the user’s account on the client platform. For example, if the remote login authentication credentials are weak for the host platform.

- Malicious insider threat: A privileged user of the client platform is untrusted, and could bypass security controls that may lead to compromising the security of other users on that client platform.

## Discarding keys
To destroy a key permanently you must either erase the OCS that is used to protect it or erase the Security World completely. There are no other ways to destroy a key permanently.

## Erasing a module from a Security World
You do not need the ACS to erase a module. However, unless you have a valid ACS and the host data for this Security World, you cannot restore the Security World after you have erased it.

## Replacing an OCS
Replacing an OCS requires authorization from the ACS of the Security World to which it belongs. You cannot replace an OCS unless you have the required number of cards from the appropriate ACS.

Replacing one OCS with another OCS also transfers the keys protected by the first OCS to the protection of the new OCS.

When you replace an OCS or softcard and recover its keys to a different OCS or softcard, the key material is not changed by the process. The process deletes the original Security World data (that is, the encrypted version of the key or keys and the smart card or softcard data file) and replaces this data with host data protected by the new OCS or softcard.

Keys protected by an OCS can only be recovered to another OCS, and not to a softcard. Likewise, softcard-protected keys can only be recovered to another softcard, and not to an OCS.

We recommend that after you have replaced an OCS, you then erase the remaining cards so that the old card set’s logical token can never be recovered.

Deleting the information about an OCS from the client does not remove the data for keys protected by that card set, as this data may exist in a backup, or the RFS.

If you are sharing the Security World across several client computers, you must ensure that the changes to the host data are propagated to all your computers.

## Replacing the ACS
Replacing the ACS modifies the world file. In order to use the new ACS on other machines in the Security World, you must copy the updated world file to all the machines in the Security World after replacing the ACS. Failure to do so could result in loss of administrative access to the Security World.

We recommend that you erase your old Administrator Cards as soon as you have created the new ACS. An attacker with the old ACS and a copy of the old host data could still re-create all your keys. With a copy of a current backup, they could even access keys that were created after you replaced the ACS.

## Firmware upgrade
If you are upgrading a module which has SEE program data or NVRAM-stored keys in its nonvolatile memory, use the NVRAM-backup utility to backup your data first.

### Setting the minimum VSN after a firmware upgrade
For nShield 5, the hsmadmin setminvsn command will set the minimum VSN (minVSN) for the loaded firmware. If an attempt is made to perform a firmware update where the VSN of the updated firmware is less than the minVSN, the firmware update will be refused. The minVSN will be set at Module manufacture to the VSN of the Module’s initial firmware.

Whenever new authorised firmware has been loaded and tested, it is recommended that the hasmadmin setminvsm command is used to set the minVSN to the VSN of the newly loaded firmware. This will protect the HSM against a firmware rollback/downgrade attack. For more information about the nShield 5 hasmadmin setminvsm command, see hsmadmin setminvsn.

## Enabling and disabling remote upgrade
You can enable or disable remote upgrade for an nShield Connect. If remote upgrade is not required, this option must be disabled.

## Migrating keys to a v3 Security World

!!! note

    A v3 Security World is a Security World created using a v12.50 (or later) nShield release.

In additions to the guidance provided in the User Manual, it is strongly recommended that the following security related guidance is followed when performing key migration to a v3 Security World:

- Perform the key migration in a controlled environment, where the Source and Destination HSMs are logically and physically isolated from all possible external influences. This will reduce the risk of an external party manipulating the data that is defining the Destination Security World/HSM

- Explicitly initialize the Source and Destination Security Worlds onto the Source and Destination HSMs, so that both the migration tool operator and ACS quorums can have assurance that the correct Security Worlds are being used, and the keys will be migrated to the correct Destination Security World

- The ACS holders (for the Source Security World) should verify the parameters used in the migration tool to ensure those are correct, before allowing key migration to proceed. For example, it is very important that the identifiers for the Destination HSM are entered correctly, so that the keys are migrated to the correct Destination Security World.

If application key are migrated to a v3 Security World, it should be noted that the security strength of any migrated keys shall be equal to the minimum security strength of any of the following:

- The security strength of the application key to be migrated

- The security strength of the Source Security World

- The security strength of the Destination Security World (which for a v3 Security World will be 128bits)

It should also be noted that during the procedure of migrating keys to a v3 Security World, when the migrated keys are outside the nShield HSM’s protected boundary, these keys are always encrypted with keys of >112 bits security strength.

## Untrusted input validation
For example, HCM-encrypted data should contain an authentication tag, however if you receive it from an untrusted source, you should verify it against a known trusted value before use. In this situation the length of the data source authentication tag will normally be present in the received data, but this should not be implicitly trusted, but verified against a known trusted value before use. This would mitigate against any manipulation of the received ciphertext, which is attempting to reduce the security strength of the data source authentication protection.