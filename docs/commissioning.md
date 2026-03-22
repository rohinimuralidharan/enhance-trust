# Commissioning
The commissioning process covers installing and configuring an nShield HSM for live operation. The commissioning process must be conducted by authorized individuals in a secure environment as described in HSM environment controls.

## Preparation
Prior to commissioning:

- Perform a threat analysis of your deployment environment or use an existing threat analysis.

- Review the guidance in this Security Manual and determine the procedures and configuration required for your systems to meet the threats identified in your threat analysis. The requirements should be identified in a Security Policy so that all users understand the controls that are necessary to operate the system securely.

- Any host operating systems should be a current, supported version with the latest patches applied.

- It is recommended that anti-virus software is installed on the host system and maintained with automatic updates.

## Installation
Entrust supplies standard component bundles that contain many of the necessary components for your installation and, in addition, individual components for use with supported applications. To be sure that all component dependencies are satisfied, you can install either:

- All the software components supplied

- Only the software components you require.

During the installation process, you will be asked to choose which bundles and components to install.

- Your choice depends on a number of considerations, including:

- The types of application that are to use the module

- The amount of disk space available for the installation

- Your policy on installing software. While it may be simpler to choose all software components, you may have a policy of not installing any software that is not required to reduce the threat level for vulnerabilities arising in software (especially services, even if unused).

The integrity of the software CDs have SHA256 checksums applied to them. If you have concerns over the integrity of a received software CD then the file checksums should be verified with Support.

## Hardware
### Trusted Verification Device
For use with the Remote Administration Client, Entrust supplies and strongly recommends the use of the nShield Trusted Verification Device (TVD). This specialized smart card reader allows the card holder to securely confirm the Electronic Serial Number (ESN) of the HSM to which they want to connect, using the TVD display.

!!! important

    Only use a TVD that has been obtained via a trusted supply chain.

The list of valid ESNs must be provided out-of-band for manual verification of the nShield HSM target. This list must be kept securely and made available only to authorized cardholders.

TVD users must be made aware that:

- Only the ESN of the HSM that was selected in the Remote Administration Client should be displayed.

- There will only be a single confirmation requested for setting up communication with that HSM.

- Any other text shown in the TVD display, or any other ESN that appears, regardless of pretext given in any user interface, is illegitimate and the card-loading should be cancelled if such text is present.

### Mode switch and jumper switches (nShield Solo only)
The mode switch on the back panel controls the mode of the module. See Checking and changing the mode on an nShield Solo module for more information about checking and changing the mode of an HSM.

You can set the physical mode override jumper switch on the circuit board of the nShield Solo to the ON position, to prevent accidental operation of the Mode switch. If this override jumper switch is on, the nShield Solo ignores the position of the Mode switch.

!!! Note

    You can set the remote mode override jumper switch on the circuit board of the nShield Solo to the ON position to prevent mode change using the nopclearfail command. This should be done if, for example, a threat analysis determines that it may be possible for a remote malicious or negligent user to interrupt operational service. In this instance the security policies of your organization should require that the physical mode switch must be used to authorize mode changes. For example a trusted role holder has to be locally present to authorize the change.
## Network configuration
### Firewall settings
When setting up your firewall, you should make sure that the port settings are compatible with the HSMs and allow access to the system components you are using. See Firewall settings for default port numbers. Only open up the ports you require and limit the IP addresses supported on those ports to the ones required.

### Simple Network Management Protocol (SNMP)
In the development of the nShield SNMP agent, we have used open-source tools that are part of the NET-SNMP project. More information on SNMP in general, and the data structures used to support SNMP installations, is available from the NET-SNMP project Web site: https://www.net-snmp.org/.

This site includes some support information and offers access to discussion email lists. You can use the discussion lists to monitor subjects that might affect the operation or security of the nShield SNMP agent or command-line utilities.

You should discuss any enquiries arising from information on the NET-SNMP Web site with Entrust nShield Support before posting potentially sensitive information to the NET-SNMP Web site.

The nShield SNMP Agent allows other computers on the network to connect to it and make requests for information. The nShield agent is based on the NET-SNMP code base, which has been tested but not fully reviewed by Entrust. We strongly recommend that you deploy the nShield SNMP agent only on a private network or a network protected from the global Internet by an appropriate firewall. The agent runs with reduced privileges on Windows and Linux in order to mitigate potential risks in the third party code.

The default nShield SNMP installation allows read-only access to the Management Information Base (MIB) with any community string. There is no default write access to any part of the MIB. The few Object Identifiers (OIDs) in the nShield MIB that are writable (should write access be enabled) specify ephemerally how the SNMP agent presents certain information. No software or HSM configuration changes can be made through the SNMP agent.

Every effort has been taken to ensure the confidentiality of cryptographic keys even when the SNMP agent is enabled. The SNMP agent runs as a client application of the HSM, and all the security controls provided and enforced by the HSM are in effect. In particular, the nShield module is designed to prevent the theft of keys even if the security of the host system is compromised, provided that the Administrator Cards are used only with trusted hosts.

We strongly advise that you set up suitable access controls in the snmpd.conf file rather than using the default settings in production. It is also recommended that a firewall is used to restrict access to the agent to legitimate client machines.

When configuring the nShield SNMP agent, make sure that you protect the configuration files if you are storing sensitive information in them.

On Linux systems, the SNMP agent will run automatically if the ncsnmp package is installed, once the install script has been run. Therefore configuration should be done before installation in a production environment to ensure the correct settings are in place from the outset. On Windows systems, the SNMP agent runs only after enabling the service with snmpd -register, so configuration changes should be applied before this step.

## Date and Time
The following sections provide procedural guidance about securely using date and time functions. Please see the HSM User Guide for information on how to operate these functions.

### Set the nShield Solo and nShield Connect Real-Time Clock (RTC)
Set the RTC using an accurate trusted local time source as part of the commissioning process. This must be set as early in the commissioning process as possible. The correct time must be set to support hardserver and audit logging.

!!! Note

    The backup battery for the RTC on the nShield Solo and nShield Solo+ will only last for two weeks when the module is not powered. The RTC must be reset on power up in such circumstances, that is, when RTC battery exhaustion occurs. See rtc for more information on resetting the clock.
### Set the nShield Connect date and time
Set the nShield Connect date and time using an accurate trusted local time source as part of the commissioning process. This must be set as early in the commissioning process as possible. The correct time must be set to support system, hardserver and tamper logging.

The nShield Connect supports a Network Time Protocol (NTP) client which if activated will synchronize the nShield Connect time to an NTP enabled time source.

!!! important

    NTP has featured many security vulnerabilities: https://www.cvedetails.com/vulnerability-list/vendor id-2153/NTP.html. The activation of NTP within the nShield Connect can increase the threats the nShield Connect is exposed to. Due to the nature of NTP design not all threats can be mitigated. NTP should only be used if your risk analysis identifies suitable controls to mitigate the impact of its operation. This could include:

    - Using only NTP servers that are under the control of the customer, that is, within the customer’s enterprise

    - Using multiple NTP sources to mitigate an attack on a single NTP source or failure of that source. NTP can provide an accurate time source through consensus with multiple input servers. It can also identify which available time servers are inaccurate

To further mitigate attacks on NTP the synchronized time should be compared against the Solo RTC time (including within the Connect) at regular intervals to ensure that a similar time, within a margin of error, is reported. Significant discrepancies should be investigated.

To identify NTP server failures or attacks, NTP servers should be monitored for availability on the network and an alert generated if the NTP server is unavailable.

### Set the Host date and time (nShield Solo only)
Set the host date and time using an accurate trusted local time source as part of the commissioning process. This must be set as early in the commissioning process as possible. The correct time must be set to support hardserver logging.

## nShield Connect and client configuration
### Configuring the Ethernet interfaces - IPv4 and IPv6
The nShield Connect has two Ethernet interfaces. Depending on the network configuration guidelines in the customer’s security policy, separated networks can be supported by assigning one interface an internal IP address, and the other an external IP address. When these Ethernet interfaces do not need to be used separately, they can be bonded to provide either redundancy or higher throughput. Configure an Ethernet bond interface provides more information on configuring Ethernet bonding. If an Ethernet interface is not used, disable it.

### Optionally configure the Connect’s hardserver interfaces
By default, the hardserver listens on all of its interfaces. However, you can alter the hardserver settings. Altering the hardserver settings would prove necessary if, for example, you wanted to only connect one of the Ethernet interfaces to external hosts.

Ensure that you have configured the Ethernet interfaces on the HSM before attempting to configure the hardserver. The hardserver should not be permitted to listen on any interface that is connected to the public Internet, as this could expose the HSM to attack from network based attackers. The HSM should also be placed behind a firewall during commissioning in order to restrict network traffic to only legitimate clients and RFS.

### Impath resilience
The nethsm_settings section in the client host hardserver config file defines settings for Impath resilience that are specific to the nShield Connect. Impath resilience is a session resumption feature which allows client hardservers to reconnect securely to their existing session with the Connect from a fresh TCP connection after a network outage. The session resumption requires both the usual mutual authentication as well as an additional authentication step to enable the client to access the previous session, that ties it to the same client hardserver instance. By default Impath resilience is turned on with a timeout of 1,800 seconds (30 minutes). This enables clients to reconnect in the event of network errors without needing to reload keys. An associated time-out can be configured to state when an Impath resilience session will expire after which all previously loaded objects must be reloaded. Your threat analysis of your environment, and knowledge of the reliability of your network, will determine if Impath resilience needs to be enabled, and what timeout should be set. For example, a five-minute Impath resilience timeout could give a reasonable trade-off between security and resilience to transient network issues.

### Configuring the RFS
The nShield RFS (Remote File System) is a protocol for securely sharing access to files between machines.

The two primary uses for the RFS are as a

1. file store for the nShield Connect config file, logs and as a source for nShield Connect upgrade images, feature files, CodeSafe application files and (where the nShield Connect front panel is being used for Security World operations) world and cardset files. This usage is often referred to simply as "the RFS".

2. way to share Security World files (including encrypted key blobs) between peer machines, usually using the rfs-sync tool. This usage is also referred to as cooperating clients.

By default, this protocol runs on port 9004, which will need to be enabled in the machine’s firewall for TCP communication. If a client machine is not being used as an RFS, it is recommended that port 9004 be disabled in the config file [server_remotecomms] section. It is also possible to restrict to listening only on a particular IP address or network interface using this configuration section.

### nShield Connect RFS
You should regularly back up the entire contents of the RFS as it is required to restore an nShield Connect or its replacement, to the current state in the case of failure.

To setup, the RFS requires certain information about the nShield Connect:

- IP Address

- ESN

- The hash of the KNETI (referred to as HKNETI)

Even with a trusted network, it is recommended that the ESN and KNETI reported by anonkneti be checked independently using the nShield Connect front panel, or from the serial console. If the network is untrusted, obtaining the ESN and KNETI information directly from the nShield Connect front panel is essential. This information is then used in the rfs-setup command to create the RFS. Specifically the --write-noauth option should not be used with the rfs-setup command over insecure networks as this does not authenticate the RFS which could give rise to a masquerade attack.

It is strongly recommended that mutual secure authentication is enabled between the nShield Connect and the RFS, that is both that rfs-setup records the authentication information for the nShield Connect in the RFS config file as described above, but also that the nShield Connect config file records the KNETI hash of the RFS in its configuration file. Authentication of the RFS by the nShield Connect can be specified when enrolling via the nShield Connect front panel, in which case the KNETI hash of the RFS machine that is displayed in the front panel must be checked against what is reported by enquiry -m 0 or by anonkneti -m 0 127.0.0.1 run locally on the RFS machine (for Software KNETI, change -m 0 to -m 1 etc. to query a module-protected KNETI). Authentication can also be specified in the serial console or in the nShield Connect config file. If the RFS is authenticated, by default it is also permitted as a privileged client, and as a config-push client, so it is the most convenient way to securely bootstrap all administration of the system.

Enabling 'secure authentication' to the RFS is described in Configuring the Remote File System (RFS) and Configuring the Remote File System (RFS) (both for network-attached HSMs).

### Configuring Client Cooperation
If your machine needs to access to a peer RFS for sharing Security World files (that is, it’s a cooperating client), it is strongly recommended to use each machine’s Software Key KNETI (or an nToken-protected or local HSM-protected KNETI if the machine has one fitted) to specify secure authentication between RFS peers over an insecure network.

This means that the rfs-setup --gang-client --write-noauth option should not be used when configuring the RFS, and instead rfs-setup must be run with the HKNETI of the peer that is allowed to initiate connections to the machine where rfs-setup (or the ESN and the HKNETI) if the peer is connecting with an nToken-protected or local HSM-protected KNETI).

On the initiating end of the connection where rfs-sync is being run, similarly the HKNETI (and ESN if module-protected) of the peer must be specified in order to authenticate it by specifying the --rfs-hkneti (and optionally --rfs-esn) parameters to rfs-sync --setup.

These options are outlined in Client cooperation.

### Remote configuration
The module (hardserver) configuration file can be used to enable:

- Remote reboot

- Remote mode changes

- Remote upgrade. If this functionality is not required then it must be disabled.

### Configuring the nShield Connect to allow a crypto client
Before a crypto client of the nShield Connect can connect, it must first be added to the permitted client list in one of two ways:

1. nShield Connect front panel. It is recommended that the secure authentication option be selected, which will require that port 9004 be temporarily accessible on the client machine so that its authentication options (Software KNETI and if available module-protected KNETI) can be queried automatically. This port can then be disabled in the client after enrollment is completed unless it is also being used as an RFS machine. The KNETI hash of the selected authentication option will be displayed in the front panel, and must be confirmed against the KNETI hash reported locally on the client machine.

2. Push an updated config file to the nShield Connect with a new entry in the [hs_clients] section. The HKNETI (keyhash field) (and esn if necessary for module-protected KNETI key) should be filled in, along with specifying clientperm with the required access level.

Privileged connections can be restricted to have source port numbered less than 1024 (low ports) as a minor additional defence in depth measure as these ports are privileged on Linux.

It is strongly recommended that all clients (privileged or unprivileged) are configured to be authenticated with a KNETI network authentication key. This client key can be a software key, or an nToken or local HSM-protected key.

Even if the clients will be dynamically created (e.g. containers that are spun up as required), it is more secure to allow access to a KNETI key associated with that container, than to simply allow any users of the network to connect without authentication.

### Configuring a client to communicate with an nShield Connect
A utility nethsmenroll is used to edit the configuration file of the client hardserver to add an nShield Connect with a given IP address or hostname. It is strongly recommended that the utility is used with the optional ESN and HKNETI parameters filled in. This content must be obtained from the nShield Connect’s front panel, or from the nShield Connect serial console (using the esn and kneti commands). An alternative method for using nethsmenroll is to omit the ESN and HKNETI parameters. In this situation, nethsmenroll will attempt to recover ESN and HKNETI parameters from the nShield Connect and then prompts the client for confirmation that the values are correct. Confirmation is achieved by verifying the ESN and HKNETI displayed on the front panel of the nShield Connect (or in the serial console) are the same values as those reported by nethsmenroll. This step must be completed when enrolling clients over a network to verify that the client is communicating with the valid, identified nShield Connect. Once the values are confirmed they are automatically written to the client-side configuration file and used for verification for all future connections. The HKNETI of an nShield Connect does not change in normal operation, although it does change in the event of factory reset. If the HKNETI ceases to match, resulting in existing client hardserver configurations refusing to connect to the nShield Connect, the cause of the change should be investigated, and if no satisfactory explanation is found, tampering should be suspected.

The nethsmenroll option -–no-hkneti-confirmation to enroll without either supplying the ESN and HKNETI as parameters, nor confirming it interactively, should not be used in production and is present only for test automation purposes.

### Configuring a client to communicate through an nToken
If an nToken is installed in a client, it can be used to both generate and protect a key that is then used for the Impath communication between the nShield Connect and the client. A dedicated hardware protected key is used at both ends of the Impath as a result. The nToken mitigates threats occurring in the client environment including vulnerabilities arising in generic software and operating systems.

When configuring an nShield Connect to use a client containing an nToken, you must obtain the nToken key hash from the client and then view the client’s configuration from the front panel of the nShield Connect and verify that the nToken key hash displayed there matches the nToken key hash obtained from the client. This makes sure that the correct nToken will be enrolled.

!!! note

    If an nToken isn’t installed, then it is recommended to use software-based authentication. If neither an nToken nor software-based authentication is available for secure authentication, the client connection relies solely on the nShield Connect validating the client’s IP address against the configured value to authenticate the client. In this instance the 'credentials' used by the nShield Connect to authenticate the client are weaker than the 'credentials' used by the client to authenticate nShield Connect. The method for authenticating the client may be vulnerable to its IP address being spoofed by an attacker.

### Configuring the serial console
The serial console on the nShield Connect is enabled by default and can be disabled from the front panel. Regardless of the serial console being enabled or disabled, factory resetting an nShield Connect will re-enable the serial console. Disable the serial console if serial console connectivity is not required to prevent unauthorized access attempts. This is important as the serial console is shipped with a known default password that could allow an unauthorized access if the serial console is enabled and the default password is not changed.

The Serial Console requires a serial cable connection to a serial port aggregator which in turn is connected to an Administrator console via a communication channel. The administrator must ensure that a secure channel is correctly setup between their console and an authenticated serial port aggregator. Physical access controls should be deployed to protect the following from unauthorized access attempts:

- Serial cable

- Serial port aggregator

- Administrator console

Logical access controls should be deployed to protect the following from unauthorized access attempts:

- Serial console

- Serial port aggregator

- Administrator console

All passwords should be protected from unauthorized access or viewing.

The username for accessing the serial console is cli and the default password is admin. On first login you will be prompted to change the password for the cli user. The minimum length of the new password is 5 characters. The functionality does not enforce strong passwords therefore these should be manually implemented – see the relevant password guidance contained in Access Control Chapter sections:

- Logical Token passphrase guidance

- Dos and don’ts for access control mechanisms

Equipment that supports the serial console connection must be maintained with the latest software and patches, such as the serial port aggregator or the administrator console.

## Logging and debugging
The following sections provide procedural guidance about securely using logging and debugging functions. Please see the HSM User Guide for information on how to operate these functions.

### Set up logging
Once the time has been set, your logging requirements should be identified and implemented.

Logs are available on nShield platforms:


|Type of Log        |Purpose and configuration           |nToken, Edge  |Solo+, Solo XC, nShield 5s|Connect+, Connect XC, nShield 5c|
|-------------------|---------------------------------------------------------|--------------|-------------|-------------------|
|Audit logging|Operational and key usage activity<br>Can only be enabled at Security World initialization|Yes|Yes|Yes|
|Hardserver log|Errors and troubleshooting<br>Controlled through environment variables. Default is to log nothing. Level of logging can be set.|Yes|Yes|Yes|
|Security World log used by API libraries and base services|Errors and troubleshooting<br>Controlled through environment variables. Default is to log nothing, unless this is overridden by any individual library. See Environment variables for more information of NFLOG_*` variables. If any of the four logging variables are set, all unset variables are given default values. Level of logging can be set.|Yes|Yes|Yes|
|System log|System events on nShield Connect<br>Always enabled|No|No|Yes|
|Tamper log|Tamper events on nShield Connect<br>Always enabled|No|No|Yes|
|Net-SNMP daemon log|Net-SNMP daemon activity<br>Starts with the SNMP service. The SNMP Agent contains a logfile switch to record warnings and messages.|Yes|Yes<sup>1</sup>|Yes<sup>1</sup>|

<sup>1</sup> The netsnmp daemon log is produced by the client.

### Audit Log
Only the Audit Log originates in the nShield Solo HSM (including the nShield Solo HSM installed in every nShield Connect). It uses a cryptographic mechanism to assure the integrity of the audit log distributed to third party SIEM Collectors.

- The SIEM Collectors should be located in a protected environment that limits physical access to the processing platform(s), on which the collector and validation applications are running, to authorized users.

- The availability of log messages delivered to the SIEM Collectors should be maintained through a set of controls including:

    - Configuring multiple SIEM Collectors for each HSM

    - Regular backups

    - Physical and logical access controls for accessing backups.

- A mechanism should be supplied to support the reliable delivery of logging messages to the SIEM Collectors.

- The network should be configured correctly to help prevent message corruption, congestion, forwarding loops and incorrect delivery.

- The Audit Logging Verification process provided by Entrust to support the authenticated supply of a Trusted Root to the customer should be followed. See Audit log verification for further information on Audit Logging Verification.

- Prior to shutting down the nShield Connect, a delay of at least 17 minutes should be made after the final log messages has been dispatched to the SIEM to ensure that the outstanding integrity verification message for those log messages is dispatched. This requires that no further commands are entered that generate log messages during this period. The receipt of the verification message on the SIEM should be confirmed prior to HSM shutdown.

- The integrity verification messages allows missing or altered log messages to be detected. In the event of a power failure, or an SOS on the nShield Connect, the loss /manipulation of any log message received after the last integrity verifications message cannot be detected, and therefore these log messages cannot be trusted.

### nShield Connect tamper log
The nShield Connect’s Tamper Log is located within the nShield Connect and protected by the nShield Connect’s tamper mechanisms. It cannot be erased using the Connect front panel or serial console.

The tamper log is an informational tool providing an indication of events in normal operation. It is not intended as a security control and cannot be relied on if there are reasons to believe that device has been tampered with (e.g. through examination of physical marks on the device).

It is essential that physical access controls are in place to prevent unauthorized access to the device to prevent tampering.

In addition to physical access controls, enabling and monitoring HSM audit logs (Security World audit logs enabled when creating the world, and additionally signed system logs in the case of the nShield 5s and 5c) are the effective measures to prevent and discover misuse rather than relying on the Connect tamper log.

### nShield Connect system and hardserver logs
The nShield Connect’s System and Hardserver Logs can be stored within the nShield Connect’s tamper response boundary or pushed out to the RFS or a remote syslog server. If the logs are stored locally then they will be protected by the tamper response boundary but will be lost if the nShield Connect is rebooted. Logging stops when the file system is full. Logs stored on in the nShield Connect can be viewed using the Front Panel or fetched on demand using the appliance-cli client tool from a privileged client.

If logs are configured to be pushed to the RFS, they will be sent securely to the RFS endpoint (with mutual authentication, provided this is configured, the secrecy and integrity of messages are protected in transit). Logs may be configured to be pushed every minute to minimize the loss of data in the event of reboot or power loss.

Alternatively, the logs can be streamed to a remote syslog server over UDP. However, in this situation no authentication, secrecy or integrity mechanism is applied to the logs. This is only recommended on a trusted network (such as a private network between one of the Connect interfaces and host machines).

### nToken, nShield Edge and nShield Solo hardserver logs
For nToken, nShield Edge and nShield Solo products the hardserver logs are stored in the associated host platform and do not have an integrity mechanism applied to the logs. Therefore, physical, procedural and technical controls should be applied to the host platform environment to protect the integrity of the logs.

### Hardserver and system log environment variables
For Hardserver and System logging, environment variables can be applied to control the amount of logging information. See Environment variables for more information. Your system management policy and security policy will determine the level of logging required.

### Debugging options
Debugging information is available for hardware, application, Application Programming Interfaces (APIs) and operating systems. Unless required for development debugging for these sources should not be enabled (it is disabled by default).

Some debug information is provided in command line responses and therefore cannot be disabled.

!!! note

    SEE debugging is disabled by default, but you can choose whether to enable it for all users or whether to make it available only through use of an ACS.

Debugging can leak potentially useful information to an attacker so should not be enabled in production environments.

## Configure access control
### Security World key protection options
For guidance on access control mechanisms available to protect application keys in a Security World see Access Control.

## Configure Security World
### Security World options

!!! note
    The HSM must be in a secure and trusted environment before a Security World is created or loaded.

Decide what kind of Security World you need before you create it. Depending on the kind of Security World you need, you can choose different options at the time of creation. For convenience, Security World options can be divided into the following groups:

- Basic options, which must be configured for all Security Worlds. This includes environment variables.

- Recovery and replacement options, which must be configured if the Security World, keys, or passphrases are to be recoverable or replaceable. If you disable OCS and softcard replacement, you can never replace lost or damaged OCSs generated for that Security World. Therefore, you could never recover any keys protected by lost or damaged OCSs, even if the keys themselves were generated as recoverable (which is the default for key generation). Replacing OCSs and softcards requires authorization. To prevent the duplication of an OCS or a softcard without your knowledge, the recovery keys are protected by the ACS. However, there is always some extra risk attached to the storage of any key-recovery or OCS and softcard replacement data. An attacker with the ACS and a copy of the recovery and replacement data could re-create your Security World. If you have some keys that are especially important to protect, you may decide:

    - To issue a new key if you lose the OCS that protects the existing key

    - Turn off the recovery and replacement functions for the Security World or the recovery feature for a specific key.

    The recovery data for application keys is kept separate from the recovery data for the Security World key. The Security World always creates recovery data for the Security World key. It is only the recovery of application keys that is optional. See Access Control for more information and guidance on the options available.

- SEE options, which only need be configured if you are using CodeSafe

- Options relating to the replacement of an existing Security World with a new Security World.

Security World options are highly configurable at the time of creation but, so that they will remain secure, not afterwards. For this reason, we recommend that you familiarize yourself with Security World options, especially those required by your particular situation, before you begin to create a Security World.

### Application interfaces
A Security World can protect keys for any applications correctly integrated with the Security World software. A wide range of application interfaces are supported. The application interfaces have many configuration settings. These should be reviewed to identify that only the required settings are activated and that all others are set to off.

### Nonvolatile memory (NVRAM) options
Application keys can be stored in the module’s NVRAM instead of in the Key Management Data directory of the host computer. However, the space in NVRAM is limited and does not provide greater security than storing keys in the Key Management Data directory of the host. Therefore, it is recommended that encrypted key blobs are stored in the Key Management Data directory where space is limited only by the capacity of the host computer.

### Loading a Security World after firmware update
Before initialising a Security World on an HSM after a firmware upgrade, it is recommended that the ACS card holder quorum check that the version of the firmware is correct. This will mitigate the possibility of rollback/downgrade attack.

## Remote services
### Remote Administration Service (RAS)
To use Remote Administration with nShield Connects, the RAS must be installed on a machine that is enrolled as a client of the nShield Connects.This may be the RFS machine.

To use Remote Administration with nShield Solo(s), the RAS must be installed on the host where the hardserver and nShield Solo(s) reside. If you have multiple nShield Solo hosts in a Security World, the RAS must be installed on each one. It will also be necessary to install the KLF2 warrants on the host machine if using Solo+ or Solo XC; this is not required for nShield 5s nor with nShield Connects where the KLF2 warrant is built-in.

The remote access solution that your organization normally uses, such as Secure Shell (SSH), remote PowerShell or a remote desktop application, is also required for running the nShield client applications that will use the remotely presented ACS or OCS cards.

A secure private communications channel, such as a Virtual Private Network (VPN), or an SSH tunnel (to TCP port 9005), must always be used for the connection between the Remote Administration Client (RAC) and the RAS if they are on separate computers.

You must use nShield Remote Administration Cards with Remote Administration. These are smart cards that are capable of negotiating mutually authenticated cryptographically secure connections with an HSM, using warrants as the root of trust.

Dynamic slot timeouts are available to define timeout values for expected smartcard responsiveness and round trip latency for all HSMs that are using the Remote Administration. Expected network delays need to be taken into account when setting this. The timeouts provide control over the period of time a smart card will remain responsive in the system that is experiencing unreasonable latency (where the system’s non-performance could be a consequence of an attack).

The cardlist file must be used to specify the serial numbers of remote cards that are allowed to be used by nShield client tools as an extra defence-in-depth facility to ensure only legitimate smartcards are presented.

If Remote Administration is not required then set the dynamic slots to 0 to disable Remote Administration.

### Remote Operator
The Remote Operator feature enables the secure transmission of the contents of a smart card inserted into the slot of one attended module to another unattended module. To transmit to a remote module, you must make sure that:

- The smart card is from a persistent OCS – see Access Control for guidance on persistent OCS.

- The attended and unattended modules are in the same Security World.

By default, modules are initialized into Security Worlds with remote card set reading enabled. Disable the Remote Operator feature if there is no requirement for it.

## SEE
When an HSM is used to protect application keys, the keys are only available in unencrypted form when they are loaded into the HSM. However, traditionally, the code that uses the keys remains on the server. This means that the code is open to attack. It is possible that the code could be modified in such a way as to leak important information or compromise your business rules.

By implementing a solution with the SEE, you not only protect your cryptographic keys but also extend the physical security boundary to include your security critical code and data. Using the techniques of code signing and secure storage, the SEE enables you to maintain the confidentiality and integrity of application code and data and to bind them together so that only code in which you have confidence has access to confidential data.

If your threat analysis determines that the application code is vulnerable to attack, then it is recommended that you use the integrity and confidentiality protections that are provided by an SEE machine. See the CodeSafe Developer Guide for more details.

### Security World SEE options
You must configure SEE options if you are using the nShield SEE. If you do not have SEE installed, the SEE options are not applicable.

### RTC options
RTC options are relevant if you have purchased and installed the CodeSafe Developer kit. If so, by default, Security Worlds are created with access to RTC operations enabled. However, in FIPS Level 2 and Level 3 Security Worlds an option is available during Security World initialization to control access to RTC operations by means of an ACS. A threat analysis can determine if access to the RTC requires ACS authorization.

## Set up SSH communications between host and nShield 5 and later modules
SSH secure channels protect communications between the host and nShield 5 and later HSMs. To allow mutual authentication of the endpoints, the SSH protocol uses separate key pairs in the host and the HSM. The functionality within the HSM is divided into different services that each use separate SSH channels. Before you can use these services, SSH client public keys must be installed on each service.

If allowed by your security policy, make a backup of your sshadmin private key. This means that you can re-establish communication with the HSM if any of the other installed service keys are erased or otherwise lost.

You must regard SSH keys that connect the host to the HSM as sensitive assets and protect them appropriately. On the HSM, the private keys are stored on flash memory. On the client, they are stored on the host and are protected by OS filesystem permissions where only root or local Administrators have write access, and only root, nfast user or permitted groups have read access depending on the type of service key. The client key files are stored in an encrypted format.

The default protection configuration and the available options to control how the client keys are protected are explained in detail in SSH Client Key Protection (nShield 5s HSMs). You must secure access to the client host machines, any backup files and any passphrases you optionally set on the keys, to prevent them from being used by an adversary to retrieve the client private keys stored within the files.