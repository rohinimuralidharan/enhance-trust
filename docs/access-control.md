# Access Control
An nShield HSM Security World can be configured and managed both remotely and locally using the supplied access control mechanisms.

## Security World access control architecture
This section describes the access control options available within a Security World and the pros and cons of each option. This guidance is provided to help the customer to determine the right options for their threat environment.

### User groups and users to install Security World software

!!! important

    On Linux and Windows, root or administrator privileges are required to install Security World software, and to configure access to local HSMs.

    On Linux, run nShield utilities as a normal user if only unprivileged nCore commands and read-only access to keys is needed, as a user in the group nfast for privileged nCore commands and ability to modify kmdata, or in the group nfastadmin for access to hsmadmin and csadmin operations. Do not run production workloads as root or as the nfast user.

    On Windows, ordinary local users can run unprivileged nCore commands and have read access to kmdata keys by default. Administrator privileges are required by default for privileged nCore commands and writing to the Key Management Data directory, but custom settings can be set in the config file for the groups allowed to make privileged and unprivileged connections, and either manual filesystem DACL changes or a non-default Key Management Data directory used to enable alternative access controls.

    See Roles for further details.

### Security World access control
All Security Worlds are protected by an ACS created at Security World initialization. The ACS is used to:

- Control access to Security World configuration

- Authorize recovery and replacement operations.

The ACS consists of a number of smart cards, N, of which a smaller or equal number, K, is required to authorize an action. The required number K is known as the quorum. The cards are distributed amongst authorized role holders so that a quorum of role holders are required to authorize the above operations.

See Administrator Card Set (ACS) protection for guidance on configuring and protecting the ACS.

### Application key access control
The Security World and nShield HSM provide the facility for different levels of application key protection. There are three levels of application key protection:

- Module protection

    - The key is simply protected by the HSMs in the Security World

    - Any application on the host can load and use the key (but not export from HSM, unless ACLs allow).

- Softcard protection

    - The key is additionally protected by a passphrase

    - An application will prompt you for the passphrase before loading the key.

- OCS (token) protection.

    - The key is protected by a card set

    - Each card set consists of a number of smart cards, N, of which a smaller or equal number, K, is required to authorize an action. The required number K is known as the quorum.

    - The smartcards are distributed amongst authorized role holders so that a quorum of role holders are required to authorize loading of an application key.

    - The role holders must present the required cards to authorize the key loading.

    - The smartcard can be optionally protected with a passphrase. This can be set at any time.

    - The smartcards can be created in persistent or non-persistent (default) mode and with or without an associated time-out to provide different user options dependent on the importance of the application keys, and physical and logical security controls available in the environment.

Module-protected keys have no passphrase and are usable by any instance of the application for which they were created, provided that the application is running on a server fitted with a HSM (or connected to an nShield Connect) which is initialized with the same Security World that was used to create these keys.

This level of protection is suitable for high-availability web servers that you want to recover immediately without intervention if the computer resets. However, the environment should be secure as the ability to use the application keys is dependent on any underlying access control provided by the associated operating systems hosting the application instances. See Module protection for guidance on the controls required to prevent unauthorized key access to module-protected keys.

The addition of a passphrase allows tighter control over key usage through explicit authorization. Controlling access to a key via a passphrase is achieved through creating a softcard. A softcard is a file containing a logical token that you cannot load without a passphrase. You must load the logical token to authorize the loading of any application key that is protected by the softcard. A softcard functions as a persistent 1/1 logical token. After a softcard’s logical token is loaded, it remains valid for loading its keys until its key handle is destroyed. A single softcard can protect multiple application keys. See Softcard protection for guidance on the controls required to prevent unauthorized access to keys protected by a softcard.

The highest level of protection available for application keys is provided by OCS-protection. It is recommended that OCS cards additionally be passphrase-protected. When keys are protected by an OCS, an attacker would need to obtain access to a quorum of the OCS smartcards, plus any associated smartcard passphrases, to be able to load the logical token associated with the OCS. Only when the OCS’s logical token has been loaded onto the HSM can the OCS-protected keys be loaded and used in operations. The OCS’s logical token will remain loaded until its reauthorization timeout is reached, the last OCS smartcard is removed (for non-persistent tokens), or it is destroyed. A single OCS can protect multiple application keys. See OCS protection for guidance on the controls required to prevent unauthorized access to keys protected by a OCS.

All Security Worlds rely on you using the security features of your operating system to control the users who can access the Security World and, for example, write data to the host.

Your security policy (in response to a threat analysis) will determine the level of protection required for your application keys.

## Security World access control guidance
### Administrator Card Set (ACS) protection
There is always only one ACS in a Security World. When creating a Security World you can configure the following options:

- Whether to enable key recovery

- If you disable the key recovery option, you cannot replace lost or damaged OCSs and, therefore, cannot access keys that are protected by such cards. This feature cannot be enabled later without reinitializing your Security World and discarding all your existing keys.

- The number of administrator cards required to authorize key recovery

- Whether to enable passphrase recovery

- The number of administrator cards required to authorize passphrase recovery

- The number of administrator cards required to load this Security World onto a new HSM.

- The number of administrator cards required for various features.

- Whether to disable certain features

- Whether to specify a passphrase for a card (passphrases are strongly recommended)

Your threat analysis should determine which features and/or options are enabled and ACS quorums required to authorize those features. It is recommended that the quorum for the ACS is larger than the quorum required to activate any feature. This will ensure that the ACS quorum is only ever used when legitimately required. Additionally the quorum size for Key and passphrase recovery features should be greater than 1 but less than the ACS quorum as these features can authorize access to OCS/Softcard-protected keys, and this quorum size protects against a single malicious administrator card holder.

### NSO-timeout guidance
When creating a new Security World, for example using the new-world utility, one of the parameters that can be explicitly specified is the NSO-timeout. The NSO-timeout is the maximum time that can elapse between ACS quorum authorization, and reauthorization being required. This is only normally relevant when the last administrator card of the quorum is left in the card reader of a Module, as normal security practice is to remove the last administrator card as soon as the current administrative task has been completed.

The setting of this value will depend on the administrative tasks that the ACS quorum will authorize to be performed in the Security World, and the results of the threat analysis for the nShield administrators/ACS quorum.

The following rules apply to the NSO-timeout:

- The minimum is 3 minutes per card

- The default is the greater of 6 minutes per card, and 1 hour

- The maximum is the greater of 10 minutes per card, and 2 hours

The following is general guidance for NSO-timeout setting. Refer to your own security policy to determine what value is acceptable:

- If the HSM is for general cryptographic use, and key migration may occur from another Security World, the default NSO-timeout is a good choice.

- If the risk of the last administrator card being mistakenly left in the HSM’s card reader is considered significant, a shorter value may be set for the NSO-timeout. The value must be set large enough to allow completion of the longest operation the ACS quorum authorize.

### Creating and maintaining a quorum
Each card set consists of a number of smart cards, N, of which a smaller number, K, is required to authorize an action. The required number K is known as the quorum. Each card in an ACS stores only a share of the ACS keys. You can only re-create these keys if you have access to enough of their shares. Because cards sometimes fail or are lost, the number of shares required to re-create the key (K) should be less than the total number of shares (N).

To make a robustly secure ACS, it is recommended that the value of K is relatively large and the value of N is less than twice that of K (for example, the values for K/N being 3/5 or 5/9). The customer security procedures should determine the values of K and N which should be based on a threat analysis of the protected data.

The customer security procedures should identify the role holders for the different cards. Roles should be assigned based on area of responsibility but also awareness of role holder’s availability as they may be required to recover a Security World at short notice in the case of a failure.

The customer’s security policy should determine whether the passphrases are required for the cards. passphrases provide an additional barrier to the attacker. This requirement may be necessary based on the value of the data protected and the security around the storage location of cards. A timing delay feature is applied to password retries to add further protection.

If an ACS quorum (K of N) is lost, the associated Security World becomes unmanageable as a quorum is needed to replace the old ACS card set. There is no possibility of recovering it.

If a card from an ACS is lost, you should replace the entire ACS as soon as possible.

### Card management guidance
- ACS cards should be assigned to different stakeholders to prevent a malicious administrator attempting to exploit the Security World

- ACS cards should be stored securely in separate locations to prevent an attacker obtaining a quorum of cards

- Passwords should be used to help prevent stolen card(s) being used. To prevent this threat arising do not store passwords with the stored card

- Strong passphrases must be used to prevent an attacker attempting to brute force the password. See Logical token passphrase guidance for guidance on choosing strong passphrases

- A register should be maintained of the administrators and the cards they hold to mitigate being unable to identify administrators and the cards they hold

- Cardsets should be regularly audited to make sure that they are still available and working. See Audit for further details

- The process for managing loss, theft or corruption of cards should be set out in your security procedures. If a quorum of cards is compromised the Security World protected by the ACS is vulnerable to attack. See Security Incident and Response for further guidance.

- The requirements for the correct identification, use, movement, storage and protection of cards by trusted, authorized individuals should be set out in your Security Procedures to mitigate administrators not adequately protecting the cards they are responsible for. See Audit for further details.

### Module protection
If module protection is selected then access to any application using the module provides access to module protected keys. Physical and/or logical controls must be applied to the platforms hosting the applications and their environment to prevent unauthorized key access.

### Softcard protection
If this option is selected then access to application keys is protected by a softcard logical token and an associated passphrase. The password policy used in accessing the softcards should be stated in the customer’s Security Procedures. See Logical token passphrase guidance for guidance on choosing strong passphrases.

A register should be maintained of the individuals who have access to softcards to support operation and any role transition.

The customer’s security procedures should determine whether a softcard can be recovered, if lost, to a new softcard.

This is enabled by default during Security World creation.

The customer’s security procedures should determine whether a softcard passphrase can be replaced if lost. This is disabled by default during Security World creation.

You can use a single softcard to protect multiple keys. A threat analysis will determine the number of keys that should be protected by a softcard.

It is possible to generate multiple softcards with the same passphrase. This option whilst convenient increases the attack surface as an attacker breaking the passphrase will then have access to all keys protected by the softcards. A threat analysis should be performed to determine a safe number of keys that are protected by any softcard and its associated passphrase.

Softcards are persistent; after a softcard is loaded, it remains valid for loading the keys it protects until its KeyID is destroyed. Ensure KeyIDs are destroyed once the required operations are complete.

### Logical token passphrase guidance
The following public sources of passphrase guidance are recommended as references for creating a user password policy:

- National Cyber Security Centre (UK) - Password Guidance Summary

- Appendix A of NIST Special Publication 800-63B - Digital Identity Guidelines

- SANS Password Construction Guidelines

A timing delay feature is applied to password retries to add further protection, however there is no retry lockout. Therefore, implementing a robust user password policy helps mitigate a determined passphrase attack. In support of this, a warning message can be configured if passphrases are too short and don’t comply with your security procedures. The warning message can only be enabled during Security World creation (but then applies to OCS and softcard passphrase entry too within that world). Note that this is an informational check only.

The process for managing forgotten passphrases should be set out in your security procedures.

The lifecycle for passphrases will be determined by your threat analysis and the resulting Security Policy.

### OCS protection
OCSs provide the tightest control over application key usage, especially when they are also passphrase protected. Token protected keys use physical tokens in the form of smart cards (ISO 7816 compliant). These belong to a specific Security World and only an HSM within the Security World to which the OCS belongs can read, erase or format the OCS. There is no limit to the number of OCSs that you can create within a Security World. OCSs can be created and deleted at any time.

#### Creating and maintaining a quorum
Each card set consists of a number of smart cards, N, of which a smaller number, K, is required to authorize an action. The required number K is known as the quorum. Each card in an OCS stores only a fragment of the OCS keys. You can only re-create these keys if you have access to enough of their fragments. Because cards sometimes fail or are lost, the number of fragments required to re-create the key (K) should be less than the total number of fragments (N).

To make a robustly secure OCS, it is recommended that the value of K is relatively large and the value of N is less than twice that of K (for example, the values for K/N being 3/5 or 5/9). The customer security procedures should determine the values of K and N which should be based on a threat analysis of the protected data.

The customer security procedures should identify the role holders for the different cards. Roles should be assigned based on area of responsibility.

The customer’s security policy should determine whether the passphrases are required for the cards. passphrases provide an additional barrier to the attacker. This requirement may be necessary based on the value of the data protected and the security around the storage location of cards. A timing delay feature is applied to password retries to add further protection.

The customer’s security policy should determine whether persistent or non-persistent (default) mode is required and whether a time-out is required. See Persistence and non-persistence for OCS for guidance on this option. Once set at creation the mode cannot be changed.

The customer’s security procedures should determine whether OCS can be recovered if lost to a new OCS. This is enabled by default during Security World recreation.

The customer’s security procedures should determine whether OCS passphrases can be replaced if lost. This is disabled by default during Security World recreation.

Lost or damaged cards should be replaced as you discover the loss or damage to prevent a potential scenario where a quorum of cards are not available to authorize operations.

For further guidance on using an OCS to share keys across HSMs and share keys between users, see Creating and maintaining a quorum.

#### Card management guidance
- When not in use OCS cards must be stored securely by each custodian.

- See Logical token passphrase guidance for guidance on choosing strong passphrases.

- passphrases or hints for each specific card should not be written down in the same location as the card.

- A register should be maintained of the custodians and the cards they hold to support operation and any role transition. In pursuit of this OCSs can be created with a name of the OCS and names for each card in the OCS.

- Cardsets should be regularly audited to make sure that they are still present. See Audit for further information.

- Sometimes an OCS may be stored in the card reader. See Persistence and non-persistence for OCS for guidance on this option.

- The process for managing loss, theft or corruption of cards should be set out in your security procedures. If a quorum of cards is compromised the application keys protected by the OCS are vulnerable to attack. See Security Incident and Response for further guidance.

- The requirements for the correct identification, use, movement, storage and protection of cards by trusted, authorized individuals should be set out in your security procedure. See Audit for further details.

#### Persistence and non-persistence for OCS
If you create a standard (non-persistent) OCS, the keys it protects can only be used while the last required card of the quorum remains loaded in the smart card reader of the nShield HSM. The keys protected by this card are removed from the memory of the HSM as soon as the card is removed from the smart card reader. This mode is more secure as the user directly controls key usage. If you want to be able to use the keys after you have removed the last card, you must make that OCS persistent. Keys protected by a persistent card set can be used for as long as the application that loaded the OCS remains connected to the HSM (unless that application removes the keys explicitly or any usage or time limit is reached). Persistent mode should only be used once a threat analysis of the environment has determined that it is safe for application keys to continue to be operationally usable once the last OCS card has been removed.

OCSs (both persistent and non-persistent) can also be created with a time-out, so that they can only be used for limited time after the OCS is loaded. Keys will be forcibly unloaded when the timeout expires. An OCS is loaded by most applications at start up or when the user supplies the final required passphrase. After an OCS has timed out, it is not loadable by another application unless it is removed and reinserted. The removal and then reinsertion of OCS cards is not enforced for dynamic slots. A TVD or passphrase has to be used ensure user interaction before the token is reloaded.

Time-outs operate independently of OCS persistence.

You can manually remove all keys protected by persistent cards by clearing the HSM. For example, you could:

1. Run the command:

```nopclearfail --clear --all```

2. Press the Clear button of the HSM

3. Turn off power to the HSM.

A persistent OCS with no timeout is suitable for a web server. However, using this option is dependent on the level of security of any running application. For example anyone that is able to gain unauthorized application access can use the key.

A non-persistent OCS with no or a short time-out would be suitable for a root Certificate Authority. It provides complete control over key availability – the key is unloaded when the card is removed from the card reader and becomes inactive after an assigned timeout, if it has been mistakenly left in the card reader.

A threat analysis should determine which configuration of persistence/non-persistence/time-out/no time-out is appropriate for the various sets of keys protected by OCSs.

#### Application independence
Although keys belong to specific client applications performing different functions (with possibly different sensitivities), OCSs do not. You can protect keys for different applications using the same OCS. However, you must not use the same OCS to protect keys for many different applications as a compromise of the OCS could lead to a compromise of all application keys protected by it. Assigning different OCSs to different applications mitigates this threat. Additionally assigning more than one OCS to an application key helps maintain operation in the event of a compromise against an OCS.

## Application keys
When you generate an nShield key (or create it from imported key material), that key is associated with an HSM-enforced Access Control List (ACL). This ACL prevents the key from being used for operations for which it is unsuited, and can enforce requirements that certain tokens be presented, before the key can be accessed. For example, the ACL can specify that a key can only be used for signing, with a specific signing mechanism/algorithm. Your threat analysis will determine what ACL settings are required for a particular key.

### ACL restrictions for key wrapping/encapsulation keys
Care must be taken with setting the ACL for all key wrapping/de-encapsulation keys, that they are assigned the single purpose of key wrapping/de-encapsulation, and not allowed to be used for any other purpose. If a wrapping (or de-encapsulation) key is also assigned the decrypt permission, this could lead to a wrapped/encapsulated key being exposed in plaintext in the client/host platform.

The ACL will allow other conditions to be specified for a wrapping/de-encapsulation key, that would further restrict its use to:

- A specific wrapping/de-encapsulation mechanism.

- A specific application key that can be wrapped/de-encapsulated.

- Specific parameters used for the wrapping/de-encapsulation mechanism.

## nShield Connect front panel
In the case of the nShield Connect, HSM configuration can occur through the front panel. You can control access to the menus on the unit and the Power button on the front panel by using **System > System configuration > Login settings**.

When **UI Lockout with OCS** has been enabled, you must log in with an authorized Operator Card before you can access the menus. You can still view information about the unit on the startup screen. When you are logged in, you can log out and leave the unit locked. An OCS to be used to authorize login on a unit must be persistent and not loadable remotely. In accordance with the principle of 'separation of duties', the OCS, which protects physical access to the nShield Connect HSM, should not be used to also protect application keys.

When **UI Lockout without OCS** has been enabled, you cannot access the menus, but you can still view information about the nShield Connect on the start-up screen.

The power button lockout can be enabled and disabled independently when UI Lockout allows access to the menus.

Customer security procedures should identify the settings for the front panel based on a threat analysis of the environment.

## Configuring remote administration access to nShield Connect
A privileged connection is required to perform certain administrative tasks on the nShield Connect, for example to initialize a Security World or to perform certain remote configuration operations. If privileged connections are allowed, the client can issue commands (such as clearing the HSM) which interfere with its normal operation. We recommend that you add only unprivileged clients for normal operational application usage unless the machines need access to administrative operations (such as clearing module, or remotely configuring CodeSafe 5 in the case of 5c). The RFS machine is a privileged client by default (provided it is authenticated with a KNETI network authentication key), and also has privilege to push updated nShield Connect config files (in addition to any config-push client specified in [config_op] configuration section). The RFS machine is therefore recommended to be used as the general administration client, and kept separate from normal operational usage for crypto operations, but it is also possible to disable the RFS being a privileged client and config-push client in the Connect config file.

## Role holder lifecycle guidance
### Roles
The roles that can access the applications that use the HSM and their access rights for using application keys should be identified in your security procedures. Access rights must be assigned as the minimum required for a role to be performed.

### Windows user privileges
Maintaining the integrity of your system against deliberate or accidental acts can be enhanced by appropriate use of (Operating System (OS)) user privileges. There are two levels of user distinguished by default in nShield Security World on Windows:

- Built-in Administrators group access, running with elevated privilege (note that the name of this group differs depending on the language of the Windows operating system installation)

- Normal local users.

Built-in Administrators group is required for such tasks as:

- Software installation, starting and stopping the hardserver and SNMP

- Privileged connections to nFast Server (hardserver) service

- nShield 5 administration operations with hsmadmin and csadmin

- Editing the config file

By default, normal users can not create Security Worlds and cannot modify other users keys, though they will do read access to them and can create their own. Encrypted copies of keys are held in Key Management Data (C:\ProgramData\nCipher\Key Management Data\local). File permissions can be altered to restrict access to specific keys to specific users/groups, or an alternative Key Management Data (kmdata) folder can be specified by using the NFAST_KMDATA_LOCAL environment variable, which would allow the Windows access controls of the directory to be controlled independently by the administrator of the system. Note that the nShield Security World installer sets access controls on installation directories, so changes may not persist when upgraded or re-installed. Both the unprivileged and privileged connections to the nFast Server (hardserver) can have their group access specified in the nt_pipe_users and nt_privpipe_users fields in the config file (note that these settings control access for clients using both domain sockets and local TCP connections). This enables the unprivileged access to be restricted (from all local users) and the privileged access relaxed (from Built-in Administrators running with elevated privilege).

nShield services in Windows run with the minimum privilege that they require. Due to the privileged operations done by the nFast Server (hardserver) service it runs as LocalSystem. Other nShield services (such as nFast Remote Administration Service and nCipher SNMP Agent) run as a virtual service account (automatically managed by the OS) and also drop all unnecessary Windows privileges during startup. The nFast Server (hardserver) service automatically gives the appropriate level of access to known nShield services even if the nt_pipe_users and nt_privpipe_users fields in the config file are overridden.

### Linux user privileges
Maintaining the integrity of your system against deliberate or accidental acts can be enhanced by appropriate use of (OS) user privileges. There are four types of user:

- Superuser (root)

- nfastadmin group user

- nfast group user

- Normal users.

Typically, normal users can carry out operations involving Security Worlds, cardsets and keys, but not create Security Worlds, keys and cardsets. nfast group users have enhanced access, enabling them to create Security Worlds, cardsets and keys. Encrypted copies of keys are held in kmdata (/opt/nfast/kmdata/local). Normal users only have read access to the files, whereas nfast group users have read and write access, enabling them to create and use keys. nfast group users can also make privileged connections to the hardserver, e.g. enabling them to clear or change the mode of an HSM, and perform some administration operations on an nShield Connect (provided that the client they are running on is also a privileged client of the nShield Connect). The access to kmdata role and the privileged (local) client role can be separated by using a non-default kmdata path by setting the NFAST_KMLOCAL environment variable, and setting alternative user or group access to the path that it specifies. nfastadmin group users by default are permitted to do certain hsmadmin configuration and monitoring operations for the nShield 5, and csadmin configuration and monitoring operations for CodeSafe 5. Non-default groups can be specified instead for the different nShield 5 service roles by setting override environment variables at install time.

Superuser access (root) is required for such tasks as software installation, and starting and stopping services such as the hardserver and SNMP.

### Access rights withdrawn
Customer Security Procedures should identify the requirements and timelines for rescinding access rights. These may be for the following reasons:

- When a role holder leaves the company

- Moves department

- Changes roles

- Is long term sick

- Is suspended from duties.

### Dos and don’ts for access control mechanisms
Most failures of security systems are not the result of inherent flaws in the system but result from user error. The following basic rules apply to any security system:

- Keep your smartcards safe.

- Always obtain smartcards from a trusted source: from Entrust or directly from the smartcard manufacturer.

    !!! note
    
    nShield Remote Administration Cards can only be supplied by Entrust.

- Never insert a smartcard used with nShield HSMs into a smartcard reader you do not trust.

- Never connect a smartcard reader you do not trust into your HSM.

- Do not enter passphrases into a server that you do not trust.

- Never tell anyone your passphrase.

- Use a strong passphrase, see Logical token passphrase guidance.

- It is recommended that Remote Administration cards have the optional passphrase implemented to prevent the Logical Token share on the remotely presented card being misused by a malicious or misconfigured client of the HSM.