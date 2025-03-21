# Microsoft Azure Cloud HSM SDK Preview
Microsoft Azure Cloud HSM is a highly available, FIPS 140-3 Level 3 validated single-tenant HSM service that is compliant with industry standards. Azure Cloud HSM grants customers complete administrative authority over their Hardware Security Module (HSM). It provides a secure and customer owned HSM cluster for storing cryptographic keys and performing cryptographic operations. It's the ideal solution for customers who require FIPS 140-3 Level 3 validated Hardware Security Modules and supporting various applications, including PKCS#11, offload SSL/TLS processing, certificate authority private key protection, transparent data encryption, including document and code signing.

**For more information, visit [Microsoft Azure Cloud HSM](https://learn.microsoft.com/azure/cloud-hsm/overview)**

---
**SUPPORTED OPERATING SYSTEMS**  
The Azure Cloud HSM SDK currently only supports the following Operating Systems.  

| Operating System | Package Type | Installation Package Name |
|:-----------------|:-----------------|:-----------------|
| Windows Server (2016, 2019, 2022) | MSI | Installation AzureCloudHSM-ClientSDK-Windows-*.msi |
| Ubuntu (22.04, 24.04) | DEB | AzureCloudHSM-ClientSDK-OpenSSL3-*.deb |
| Ubuntu 20.04 | DEB | AzureCloudHSM-ClientSDK-*.deb |
| RHEL 9 | RPM | AzureCloudHSM-ClientSDK-OpenSSL3*.rpm |
| RHEL (7, 8) | RPM | AzureCloudHSM-ClientSDK-*.rpm |
| CBL Mariner 2 | RPM | AzureCloudHSM-ClientSDK-*.rpm |

***Important Note:*** Any other operating systems unlisted above are not supported by Azure Cloud HSM currently. 
- Ubuntu 18.04 is not supported! Ubuntu no longer supports 18.04 as it reached end of life April 30th, 2023. 
- CentOS 7 is not supported! Red Hat no longer supports CentOS 7 as it reached end of life June 30th, 2024.
- CentOS 8 is not supported! Red Hat no longer supports CentOS 8 as it reached end of life December 31st, 2021.

**SUPPORTED SCENARIOS**  
Microsoft Azure Cloud HSM is most suitable for the following types of scenarios:
- Migrating applications from on-premises to Azure Virtual Machines.
- Migrating applications from Azure Dedicated HSM or AWS Cloud HSM.
- PKCS#11, OpenSSL, JCA/JCE, CNG/KSP
- ADCS (Active Directory Certificate Services)
- SSL/TLS Offloading (Apache/Nginx)
- MSSQL/Oracle TDE (Transparent Data Encryption)
- Document/File/Code Signing

**NOT SUPPORTED**  
Microsoft Azure Cloud HSM does not integrate with other PaaS/SaaS Azure services. Azure Cloud HSM is IaaS only.

Microsoft Azure Cloud HSM is not a good fit for the following type of scenarios: Microsoft Cloud services that require support for encryption with customer-managed keys (such as Azure Information Protection, Azure Disk Encryption, Azure Data Lake Store, Azure Storage, and Customer Key for Office 365). For those scenarios customers should use Azure Managed HSM.

- Azure Cloud HSM is Not a Bare-Metal HSM appliance.
- Azure Cloud HSM is Not a Secret Store.
- Azure Cloud HSM is Not a Certificate Lifecycle Management offering.
  
