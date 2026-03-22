# Environment

## HSM function and architecture
The nShield HSMs perform encryption, digital signing and key management on behalf of an extensive range of commercial and custom-built applications including Public Key Infrastructures (PKIs), identity management systems, application-level encryption and tokenization, SSL/TLS, and code signing.

nShield HSMs provide a hardened, tamper-resistant environment for secure cryptographic processing, key generation and protection and key, data and application encryption. They are available in three form factors to support a variety of deployment scenarios:

- nShield Solo and 5s local FIPS 140 certified PCIe cards

- nShield Connect and 5c network-attached appliances, that contain the nShield Solo or 5s FIPS 140 certified PCIe card along with surrounding networking and administration support facilities

- nShield Edge FIPS 140 certified USB device

All nShield HSMs integrate with the nShield Security World architecture. This supports combinations of different nShield HSM models to build a unified ecosystem that delivers scalability, seamless failover and load balancing. The nShield Security World architecture supports a specialized key management framework that spans the nShield HSM range.

nShield HSMs all define the physical FIPS-certified security boundary or HSM Layer within which Application Keys, Control Keys and Infrastructure Keys are protected. Using quorums of Administrator Card Set (ACS) cards, Infrastructure Keys can be securely backed up and shared across multiple HSMs. When this is performed, HSMs that share the same Infrastructure Keys develop a common Security World that provides an expanded logical security boundary that extends beyond the physical HSM Layer and overlaps into the enterprise IT environment or Application Layer. The abstraction of Application Keys into Application Key Tokens enables these tokens to be stored outside the physical HSM and within the corporate IT environment.

[security world deployment]

The Security World technology makes sure that keys remain secure throughout their life cycle. Every key in the Security World is always protected by another key, even during recovery and replacement operations. Because the Security World is built around nShield key-management modules, keys are only ever available in plain text on secure hardware.

nShield Connect and nShield Solo HSMs also provide a secure environment for running sensitive applications. The CodeSafe option lets you execute code within nShield boundaries, protecting your applications and the data they process. The CodeSafe area occurs outside of the module area that is FIPS 140 Level 2 and 3 approved.

The nShield HSM is used to protect sensitive keys, data and optionally applications. It can only operate securely if its environment provides the procedural security that it requires and if its security enforcing functions are utilized appropriately.

When configured correctly the nShield HSM provides encryption, digital signing and key management services in support of confidentiality and integrity requirements for your data. The nShield HSM is not designed to be completely resistant to denial of service attacks - these can be addressed by other aspects of the system design if warranted by the threat and impact assessments.

## HSM environment controls
You must exercise due diligence to ensure that the environment within which the nShield HSMs are deployed is configured properly and is regularly examined as part of a comprehensive risk mitigation program to assess both logical and physical threats. The client’s application and its environment must be protected from malware as they access the HSMs cryptographic services. Adequate logical and physical controls should be in place to ensure that malware is detected.

Your Security Procedures should identify the measures required to ensure the physical security (and counter any threats of theft or attack) of the nShield HSM, and associated host/client/Remote File System (RFS) platforms, backup data, Security Information and Event Management (SIEM) collectors and card readers.

Access to the nShield HSM, and associated host/client/RFS platforms, backup data, SIEM collectors and card readers secure areas must:

- Only be provided to authorized individuals

- Only be provided when necessary

- Subject to audit control.

The nShield HSM and any card readers integrate with your infrastructure/network. Therefore, any Security Policy requirements for the infrastructure/network must cover the nShield HSM and card readers as well when operating within that infrastructure/network.

The nShield HSM must be subject to protection against excessive processing demands.

The nShield HSM and any card readers must be shielded against electromagnetic emission detection to prevent potential side-channel attacks through emissions analysis, if this is considered a threat in the deployed environment.

Temperature range restrictions apply to the nShield Solo and the nShield Connect when in operation. The HSMs must be located in well ventilated locations (hosts or comms racks).

Voltage range restrictions apply to the nShield Solo XC, the nShield Connect XC, and the nShield 5c when in operation. The HSMs must be protected with surge protection equipment.

To keep track of the nShield HSM and any card readers in your environment and aid any investigation in the event of loss, an asset id should be assigned to the product and a record of the nShield HSM and any card readers description, serial number and location be entered against the asset id in an asset register.