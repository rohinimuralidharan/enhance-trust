# Support and Maintenance
Customers are encouraged to obtain a support contract and regularly update their HSM to the latest firmware release.

!!! note

    Entrust strongly recommend familiarizing yourself with the information provided in the release notes before using any hardware and software related to your product.

## Security advisories
If Entrust becomes aware of a security issue affecting nShield HSMs, Entrust will publish a security advisory to customers. The security advisory will describe the issue and provide recommended actions. In some circumstances the advisory may recommend you upgrade the nShield firmware and or image file. In this situation you will need to re-present a quorum of administrator smart cards to the HSM to reload a Security World. As such, deployment and maintenance of your HSMs should consider the procedures and actions required to upgrade devices in the field.

!!! note
    The Remote Administration feature supports remote firmware upgrade of nShield HSMs, and remote ACS card presentation.

Entrust recommends that you monitor the Announcements & Security Notices section on Entrust nShield, https://trustedcare.entrust.com/, where any announcement of nShield Security Advisories will be made.

## Application and Operating System patching
To maintain protection against threats that occur in the system environment operating systems and applications should be updated in accordance with a patching policy as described in Patching Policy.

## Connect fan tray module and PSU maintenance
The nShield Connect contains only two user-replaceable parts:

- The PSUs

- The fan tray module.

Replacing a PSU or fan tray module does not affect FIPS 140 validations for the nShield Connect, or result in a tamper event. However, in the very rare event that a PSU or fan tray module requires replacement, contact Support before carrying out the replacement procedure.

!!! important "**Important**"

    Do not remove the fan tray for more than 30 minutes, otherwise a tamper event will occur.

For more information about replacing either a PSU or the fan tray module, see the Installation Sheet that accompanies the replacement part or Physical security of the HSM.

!!! important "**Important**"
    
    Breaking the security seal or dismantling the nShield Connect voids your warranty cover, and any existing maintenance and support agreements.

!!! important "**Important**"

    Mains power plugs on UK cordsets contain a 5A fuse (BS1362). Only replace with the same type and rating of fuse. If a replacement fuse fails immediately, contact Support. Do not replace with a higher value fuse.

If the product has to be moved for maintenance then all movement should occur in accordance with the HSM and Card Reader Location. Similarly, if the nShield Connect is stored before or after maintenance, then, it should be stored in accordance with the guidance outlined in Environment.

## Solo XC fan and battery maintenance
The fan and battery can be replaced should either malfunction or the battery has reached the end of its useful life. Replacing the battery and/or fan whilst the card is powered down will not cause a tamper of any sort. See Physical Security for guidance on tamper events. The replacement procedure is described in Battery replacement and Replace the fan (Solo XC).

If the product has to be moved for maintenance then all movement should occur in accordance with the HSM and Card Reader Location. Similarly, if the Solo XC is stored before or after maintenance, then, it should be stored in accordance with the guidance outlined in Environment.

## Maintenance mode
Firmware upgrades of the module are only allowed when the module is in maintenance mode. Refer to the nShield HSM and Security World product documentation for more information on setting the module into maintenance mode.

## Troubleshooting
In the event of problems with the nShield HSM, refer to the following pages:

- The Troubleshooting chapter for your HSM in the HSM User Guide.

- Logging, debugging, and diagnostics.

- The Status indicators chapter for your HSM in the HSM User Guide (for PCIe HSMs).

If the problem cannot be resolved contact support.

## Debugging information for Java

!!! important "**Important**"

    Debug output contains all commands and replies sent to the hardserver in their entirety, including all plain texts and the corresponding cipher texts as applicable.

## Contacting Entrust nShield Support
To obtain support for your product, contact Entrust nShield Support, https://trustedcare.entrust.com/.