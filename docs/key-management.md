Key Management
Key management is a critical component of a security product. A risk analysis will determine the strength of cryptographic algorithm and associated key sizes and lifetimes required to protect customer data.

Key management schema
Refer to Security World infrastructure for a description of the Security World infrastructure available for managing the secure life-cycle of cryptographic keys. See Security World Access Control Architecture for a description of the different keys available to protect application keys.

Security World security strengths
Security strength is represented by a number associated with the amount of work (that is, the number of guesses/operations) that is required to break a cryptographic algorithm or system. The security strength is specified in bits and is a specific value from the set {80, 96, 112, 128, 192, 256}. For example, a security strength of 112 bits would require on average 2(112-1) operations to brute force the algorithm or system.

Security World modes are selected when creating a Security World. The available modes, associated cipher suites, automatic FIPS mechanism compliance and security strengths are:

Modes	Ciphersuite	Automatic compliance of FIPS mechanisms with NIST SP800-131A Revision 1 algorithm/key sizes	Security Strength
FIPS 140 Level 3 (FIPS approved mode enforced)

ECp521mAES

DLf3072s256mAEScSP800131Ar1

Yes

128 bits

Unrestricted (default)

DLf3072s256mAEScSP800131Ar1

No (see note 1)

128 bits

Common Criteria CMTS (see note 2)

DLf3072s256mAEScSP800131Ar1

No

128 bits

In this mode, non-approved security functions, for example algorithms and primes, that are not compliant with NIST SP800-131A Revision 2 are available, however if selected then you will be operating outside of a FIPS approved mode of operation. See the FIPS-140-2/3 specification for a description of a FIPS approved mode of operation).

Common Criteria EN 419 221-5 Protection Profiles for TSP Cryptographic Modules - Part 5 Cryptographic Module for Trust Services.

Security Worlds created with a pre v12.50 release can be loaded. The cipher suites, automatic FIPS mechanism compliance and security strengths for these Worlds are:

Pre v12.50 Security Worlds ciphersuites, FIPS mechanism conformance and security strengths:

Modes	Ciphersuite	Automatic compliance of FIPS mechanisms with NIST SP800-131A Revision 1 algorithm/key sizes	Security Strength
Not Applicable

DLf3072s256mRijndael

No

128 bits (see note 3)

DLf1024s160mRijndael

No

80 bits

DLf1024s160mDES3

No

80 bits

3. Whilst the Ciphersuite provides 128 bits of security strength, some of the underlying (not selectable) cryptographic mechanisms use by this Ciphersuite are no longer FIPS approved. This is identified by the term "No" in the "Automatic compliance with FIPS mechanisms" column.

The tables above identify the ciphersuites, automatic compliance with NIST SP800-131A Revision 1 and security strength. The National Institute for Standards and Technology (NIST) have published, over the years, good key management guidance. NIST SP800-131A Revision 1 – Transitioning the Use of Cryptographic Algorithms and Key Lengths is referenced in the table as it provides guidance on suitable cryptographic algorithms and key sizes for protecting sensitive data today. This document, published in November 2015, advises that the minimum security strength for algorithms or keys is now 112 bits. Whilst the immediate audience is U.S. government agencies, NIST standards provide a global benchmark in security standards which many global product vendors adhere to, in order to provide their customers with appropriate levels of security assurance. Therefore, the industry standard minimum security strength is considered to be 112 bits today.

KML type and security strength
The Security World security strength may be impacted by selecting a KML type which is not the default for the cipher suite (see KML type selection). The motivation for this is to improve performances, but with a potential security tradeoff.

There are currently two valid scenarios:

Selecting the ECDSA NISTp256hSHA1 KML type with a DSA-3072 Security World:

There may be a performance boost from using NIST P-256 with no security tradeoff.

This configuration is not supported in FIPS 140 Level 3 mode.

Downgrading the KML type from NISTp521hSHA1 to NISTp256hSHA1 with an ECDSA Security World:

There is a performance boost from using NIST P-256 instead of P-521 but with a security tradeoff. This downgrades the KML security from 256-bit security to 128-bit security.

This configuration is approved in FIPS 140 Level 3 mode.

Application keys algorithms and key sizes
Depending on the application library used, a range of cryptographic algorithms are available for selection. The algorithm used and key size selected (if applicable) should be sufficient to protect customer data from threats identified in the deployed environment for their data. In line with standard security best practice, the security strength, as described in Security World security strengths, of the Security World ciphersuite will effectively limit the security strength that can be claimed for any key or algorithm used by the HSM. As advised in Security World security strengths a minimum security strength of 112 bits is considered the industry standard.

Cryptographic algorithms identifies all algorithms and key sizes available (both NIST approved and non approved). NIST SP 800-57 Part 1 Revision 4 has a section on Comparable Algorithm Strengths which provides guidance on identifying the security strength of different NIST approved algorithms and key sizes. BlueKrypt Cryptographic Key Length Recommendation could be a useful reference for determining required key sizes for common cryptographic functions:

Symmetric algorithms

Asymmetric algorithms, based on the following branches of cryptography:

Integer Factorization as a branch of Integer Factorization Cryptography as in NIST SP800-56B, such as RSA.

Discrete Logarithm as the branch of Discrete Logarithm Cryptography (see NIST SP800-56A), such as DSA, Diffie-Hellman.

Elliptic Curve, such as ECDSA.

Hashes.

The General Key Management Guidance section of NIST SP 800-57 Part 1 Revision 4 provides guidance on the risk factors that should be considered when assessing cryptoperiods and the selection of algorithms and keysize.

The nfkmverify command-line utility can be used to identify algorithms and key sizes (in bits). See Cryptographic algorithms for more information.

Cryptoperiods
A cryptoperiod is the time span during which a specific cryptographic key is authorized for use.

NIST SP 800-57 Part 1 Revision 4 has sections describing the rationale for cryptoperiods and the risk factors that should be considered when determining cryptoperiods. Setting a cryptoperiod limits, amongst other things, the amount of data exposure if a key is compromised. You are advised to perform a threat analysis, considering the sensitivity of your data and what controls you have in place to mitigate compromise, to determine appropriate cryptoperiods for keys protecting that data.

In terms of the key management schema the following cryptoperiod rules must be applied:

Application keys must have a cryptoperiod assigned to them

The Security World must have a cryptoperiod, as the Security World keys (for example, module keys) are used to protect application keys. The cryptoperiod of the Security World must therefore be greater than or equal to the cryptoperiod of any application key.

Whenever an application key reaches the end of its cryptoperiod it must be revoked. Whenever a Security World reaches the end of its cryptoperiod, a new Security World must be initialized under a new ACS and new application keys generated. The old Security World’s ACS must be destroyed.

Security World application keys can be created with a timeout and/or a usage value to help manage cryptoperiods. Additionally in Common Criteria CMTS mode application keys can use max timeout and max usage parameters to manage cryptoperiods. Note that softcard protected application keys can’t set a usage or timeout limit as there is no option to set this in the ppmk utility. The cryptoperiods for infrastructure keys can be managed manually by deleting a key when it has reached the end of its cryptoperiod. This may require re-initialization of the Security World.

Generating random numbers and keys
The nShield HSM includes a certified random number generator that uses a hardware based source of entropy. This provides greater security than a random number generator that uses a non-hardware based source of entropy that is typically provided by general purpose computers. Therefore, always use the nShield HSM to generate random numbers and keys.

Key backup
The sensitive Security World data, such as application key blobs, stored on the host or RFS is encrypted using the Security World key. You must regularly back up all the data stored in the Key Management Data directory with your normal backup procedures. It would not matter if an attacker obtained this data because all sensitive data is protected by the Security World key, stored in your HSM, and the Administrator cards for that Security World.

In terms of cryptoperiods (see above) keys that have reached the end of the cryptoperiod and therefore no longer exist on the nShield HSM may still exist on backups. If feasible then the backup data should also be deleted. However, if the backups have to be maintained for operational, resilience, or audit reasons, then ensure that the relevant procedural controls are implemented to mitigate attacks on retired keys.

Key import
We recommend generating a new key instead of importing an existing key whenever possible. The import operation does not delete any copies of the imported key material from the host, and as traces of this key material may still exist on disks or in backups, there is a risk that the key material may become compromised after it has been imported. It is your responsibility to ensure any unprotected key material is deleted. If a key was compromised before importation, then importing it does not make it secure again.

Key separation
Single purpose keys
Key separation, that is, when each key only has a single purpose, is an important security principle which is re-enforced in nShield by having different key types for different purposes, for example, ECDHPrivate/ECDHPublic for Elliptic Curve (EC) Key-Establishment keys and ECDSAPrivate/ECDSAPublic for EC signing/verification keys).

There is a use case where a static EC private Key-Establishment key will also be used to sign a CSR to request an (initial) certificate for the associated static EC public Key-Establishment key. For this particular use case, the keyType ECPrivate/ECPublic should be used for the EC Key-Establishment key, with a specialized ACL allowing the ECPrivate key to be used for a single signing (OpPermissions:Sign), and then key-establishment (OpPermissions:Decrypt).

When the ACL is used to enforce a single signature operation, this signature must be performed before the key is initially persisted/blobbed.
SSH secure channel keys
Access to each of the platform services is by a separate, mutually authenticated SSH secure channel. Entrust recommends that you use separate key pairs for each service to maintain the key separation principle. This reduces the impact if keys are compromised.

nShield JCA/JCE CSP
Installing the nShield JCA/JCE CSP
Security configuration guidance for using unlimited strength JCE jurisdiction policy files and the correct preference order for nShield in the Java security configuration file is provided in-situ in the nCipherKM JCA JCE CSP v13.9.3 API Guide. See Installing the nCipherKM JCA/JCE CSP for details.

nShield PKCS #11 library
Symmetric encryption
The nShield PKCS #11 library can use the nShield HSM to perform symmetric encryption with the following algorithms:

DES

Triple DES

AES.

Because of limitations on throughput, these operations can be slower on the nShield HSM than on the host computer. However, although the nShield HSM may be slower than the host under a light load, you may find that under a heavy load the advantage gained from off-loading the symmetric cryptography (which frees the host CPU for other tasks) means that you achieve better overall performance.

Performing symmetric encryption on the host increases the threat of key compromise as the security protection provided by the host will be less than the nShield HSM. Additionally there may be a lack of key lifecycle management of the application keys on the host.

For these reasons we recommend performing symmetric operations on the nShield HSM. If symmetric encryption is performed on the host, technical and procedural access controls should be deployed to protect the host, in order to mitigate the higher threat of key compromise.

PKCS #11 library with Security Assurance Mechanism
It is possible for an application to use the PKCS #11 API in ways that do not necessarily provide the expected security benefits, or which might introduce additional weaknesses.

The PKCS #11 library with the Security Assurance Mechanism (SAM), libcknfast, can help users to identify potential weaknesses, and help developers create secure PKCS #11 applications more easily.

The SAM in the PKCS #11 library is intended to detect operations that reveal questionable behaviour by the application. This includes applications that use an inadequate concept of key security.

Security guidance on using SAM to detect insecure behavior is provided in-situ in the PKCS #11 Reference Guide for nShield Security World v13.9.3. See PKCS#11 Developer libraries for details.

nShield PKCS #11 library environment variables
Security configuration guidance for various variables is provided in-situ in the PKCS #11 Reference Guide for nShield Security World v13.9.3. See nShield PKCS #11 library environment variables for details.