# Decommission and Disposal

## nShield Connect and nShield Solo
When an HSM reaches the end of its operational life it should be securely decommissioned and disposed of.

1. The Security Procedures in the Customer’s Security Policy should describe the decommissioning process. To decommission the HSM, all secret information that is used to protect your Security World should be erased. See Remove modules and delete Security Worlds for details of the HSM factory reset procedure. If the Customer’s Security Procedures have specific requirements concerning the erasing of application key material, these procedures should be performed before the factory reset is performed.

!!! note "**Note**"

    An HSM factory reset will erase all application key material. On nShield Solo+ or nShield Solo XC this means erasing the Security World (e.g. with the initunit command). For nShield Connect+, nShield Connect XC, nShield 5s and nShield 5c this additionally means a full factory reset to ensure all other information is removed, such as erasure of the KNETI key and any files loaded from the RFS from Connect and 5c, and erasure of any loaded CodeSafe applications on 5s and 5c.

2. If no further operational requirements exist for the HSM, Customer Security Procedures should describe the disposal process. The Customer Security Procedures concerning the transportation of the unit should be adhered to.

3. The customer may have a secure destruction policy for decommissioned assets. As long as all secret information that is used to protect your Security World has been erased there is no requirement to securely destroy the HSM as it has been returned to its factory state.

4. However if the HSM has malfunctioned in a way that it is not possible to determine whether secret information used to protect the Security World has been erased, that is, the possibility exists that secret information may still present in the HSM, then the customer must refer to their Security Procedures to determine if the HSM should be destroyed. One option here is to use a data destruction service offered by private companies who can destroy the equipment in accordance with approved standards and provide a certificate of data destruction. Customer Security Procedures should describe the destruction process that ensures that all HSM components that contains secrets are completely destroyed.

5. Entrust will accept the return of decommissioned HSMs for secure destruction.

### Recycling and disposal information
For recycling and disposal guidance, see the nShield product’s Warnings and Cautions documentation.

### Security World
If the Security World resident on the decommissioned HSM is no longer required then the ACS and OCS cards should be erased by formatting them. Formatting cards using the nShield tools will zeroize any storage used for secrets.

- The ACS can only be erased by a different Security World, for example, a replacement Security World, or on an HSM with no Security World loaded. You can, and should, reuse the smart cards from a deleted Security Worlds ACS. If you do not reuse or destroy these cards, then an attacker with these smart cards, a copy of your data (for example, a weekly backup) and access to any nShield HSM can access your old keys.

- Legacy OCS cards can only be erased on the Security World that they were created for. Therefore, ensure that the OCS is erased as a final step before the HSM is decommissioned. The cards can then be used for a new Security World. Modern Remote Administration Ready cards can be erased forcibly even without access to their creating Security World by using the --ignoreauth option to slotinfo --format. Note that this option is only permitted when either there is no Security World loaded, or when not in a strict-FIPS world.

- Once the steps outlined above any keys that exist in backup data are no longer usable.

If a new Security World is not required uninstall the Security World software. However, we recommend that you do not uninstall the Security World software unless you are either certain it is no longer required, or you intend to upgrade it.