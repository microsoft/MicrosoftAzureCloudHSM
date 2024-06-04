# **Frequently Asked Questions (FAQ)**

## **General Questions**

### **What is Azure Cloud HSM offering?**

Microsoft Azure Cloud HSM is a service that provides secure storage for cryptographic keys using hardware security modules that meet the FIPS 140-3 Level 3 security standard. It is a customer-managed single-tenant, highly available service that is compliant with industry standards.

Microsoft Azure Cloud HSM provides secure and customer managed HSM for storing cryptographic keys and performing cryptographic operations. It supports various applications, including PKCS#11, offload SSL/TLS processing, certificate authority private key protection, transparent data encryption, including document and code signing.

Microsoft Azure Cloud HSM provides high availability and redundancy by grouping multiple HSMs into an HSM cluster and automatically synchronizing across 3 HSM instances. The HSM cluster supports load balancing of cryptographic operations, and periodic HSM backups ensure secure and simple data recovery.

### **What is a hardware security module (HSM)?**

A Hardware Security Module (HSM) is a physical computing device designed to protect and administer cryptographic keys. Within HSMs, keys are securely stored and utilized for cryptographic operations, ensuring their confidentiality and integrity through tamper-resistant and tamper-evident hardware modules. Access to the keys is restricted to authenticated and authorized applications, ensuring that the key material always remains within the protected boundary of the HSM.

### **What hardware is used for Azure Cloud HSM?**

Microsoft Azure Cloud uses Marvell LiquidSecurity Hardware Security Modules.

### **What software is provided with Azure Cloud HSM?**

Microsoft supplies all software and utilities for Azure Cloud HSM through its SDK. Customers can download the Azure Cloud HSM SDK from GitHub @ [microsoft/MicrosoftAzureCloudHSM: Azure Cloud HSM SDK](https://github.com/microsoft/MicrosoftAzureCloudHSM/releases)

### **Do I need to manage the firmware on my HSM?**

No, Microsoft oversees the firmware on the hardware, which is maintained by a third-party (HSM manufacturer). Each firmware undergoes evaluation and must be signed by NIST to ensure compliance with FIPS 140-3 Level 3 standards.

### **How do I decide whether to use Azure Cloud HSM or Azure Managed HSM?**

Azure offers multiple solutions for cryptographic key storage and management in the cloud: Azure Key Vault (standard and premium offerings), Azure Managed HSM, Azure Cloud HSM, and Azure Payment HSM. It may be overwhelming for customers to decide which key management solution is correct for them. To help customers navigate this decision-making process a flowchart based on common high-level requirements and key management scenarios is available to understand [how to choose the right key management solution](https://learn.microsoft.com/en-us/azure/security/fundamentals/key-management-choose).

### **What usage scenarios best suit Azure **Cloud** HSM?**

Azure Cloud HSM is the most suitable for migration scenarios. This means that if you are migrating on-premises applications to Azure that are already using HSMs. This provides a low-friction option to migrate to Azure with minimal changes to the application. If cryptographic operations are performed in the application's code running in Azure VM or Web App, Cloud HSM may be used. In general, shrink-wrapped software running in IaaS (infrastructure as a service) models that support HSMs as a key store can use Cloud HSM, such as:

* ADCS (Active Directory Certificate Services)
* SSL/TLS Offload for Nginx and Apache
* Tools/applications used for document signing.
* Code signing.
* Java applications that require JCE provider
* Microsoft SQL Server TDE (IaaS) through EKM.
* Oracle TDE

### **Will Azure **Cloud** HSM host my HSMs for me?**

No. Microsoft does not support 'Bring Your Own HSM'. Azure Cloud HSM cannot host any customer-provided devices.

### **Can my keys in Azure Dedicated HSM be migrated to Azure Cloud HSM**?

Yes, it depends on your architecture and configuration. If your Dedicated HSM is configured in HA grouping, then you cannot migrate keys as Key Export is disabled to allow for Key Cloning (HA Grouping) and it's a destructive process to change those attributes. Customers configured in a HA Grouping will have to create new keys when migrating to Azure Cloud HSM.

## **Customer Onboarding**

### **Does Azure Cloud HSM have a monetary policy for onboarding?**

No, Azure Cloud HSM does not have a monetary policy. Onboarding Azure Cloud HSM is open to all customers.

## **Billing**

### **How will I be charged and billed for my use of the Azure Cloud HSM?**

Customers will incur an hourly fee for each Azure Cloud HSM cluster (consisting of 3 nodes). Once provisioned, Cloud HSM resource remains continuously active (always on). Billing commences upon resource provisioning, rather than when initializing the HSM resource completes.

### **What extra costs may be incurred with Azure Cloud HSM service?**

Azure Cloud HSM requires networking infrastructure (VNET, Private Endpoint, Etc.) and resources such as virtual machines for device configuration. These additional resources will incur extra costs and are not included in the Azure Cloud HSM service pricing.

### **Is there a Free Tier for the Azure Cloud HSM?**

No, there is no free tier available for Azure Cloud HSM.

## **Interoperability**

### **What operating systems are supported by Azure Cloud HSM SDK?**

* Windows (Server 2016, 2019, 2022)
* Linux (Ubuntu 20.04, Ubuntu 22.04, RHEL 7, RHEL 8)
* CBL Mariner 2

### **How do I manage **Azure Cloud** HSM?**

You can manage Azure Cloud HSMs by accessing your Cloud HSM cluster using SSH and our provided Azure Cloud HSM SDK from GitHub @ [microsoft/MicrosoftAzureCloudHSM: Azure Cloud HSM SDK](https://github.com/microsoft/MicrosoftAzureCloudHSM/releases)

### **How does my application connect to Azure Cloud HSM?**

Users **leverage** the Azure Cloud HSM SDK, **comprising** software, tools, and utilities, to execute cryptographic operations within their applications. Azure Cloud HSM supports various interfaces including PKCS#11, OpenSSL, JCE, KSP/CNG, and offers a range of tools and utilities enabling seamless interaction with their HSM. Customers can download the Azure Cloud HSM SDK from GitHub @ [microsoft/MicrosoftAzureCloudHSM: Azure Cloud HSM SDK](https://github.com/microsoft/MicrosoftAzureCloudHSM/releases)

### **Does Azure Cloud HSM support offer Password-based and PED-based authentication?**

No. Azure Cloud HSM only supports password-based authentication.

### Can an application connect to Azure **Cloud HSM from a different VNET in or across regions?**

Yes, you will need to use [VNET peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) within a region to establish connectivity across virtual networks. For cross-region connectivity, you can use [global VNET peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview/) or [VPN gateway](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways). 

### Does Azure **Cloud** HSM work with **on-premises HSMs?**

No. Although Azure CloudHSM doesn't directly interoperate with on-premises HSMs, you can securely transfer exportable keys between Azure CloudHSM and most commercial HSMs by employing one of several supported RSA key wrap methods. 

### **Can I encrypt data used by other Azure services using keys stored in Azure Cloud HSM?**

No. Azure Cloud HSM clusters are only accessible from inside your virtual network.

### Can Azure Cloud HSM be used with Office 365 Customer Key, Azure Information Protection, Azure Data Lake Store, Disk Encryption or **Azure Storage encryption?**

No. Azure Cloud HSM is provisioned directly into a customer's private IP Address space, so it is not accessible by other Azure or Microsoft services.

### **Can I import keys from existing **o**n-premises HSM**s to Azure Cloud HSM?

Yes. There are multiple methods for BYOK and have on-premises HSMs that allow key export (key wrap).

### **Can I install functionality modules to Azure **Cloud** HSMs?**

No. Azure Cloud HSM service does not support functionality modules.

### **Can I update the Partition Owner **Certificate** once it has been uploaded**?

No. Users cannot change the partition owner certificate once it has been uploaded.  If you uploaded the PO.crt in error, you would need to delete your Azure Cloud HSM resource and deploy again. 

## **Security and Compliance**

### Do I share my **Azure **Cloud**HSM with other **Azure** customers?**

No, with Azure Cloud HSM, you have exclusive administrative access to your HSM as a single tenant.

### **Can Microsoft or anyone at Microsoft access keys in my **Azure Cloud** HSM?**

No. Microsoft does not have any access to the keys stored in customer allocated HSMs.

### **How does **Microsoft** manage the HSM without having access to my encryption keys?**

In the architecture of Azure Cloud HSM, separation of duties and role-based access control are fundamental principles. Microsoft does not have any cryptographic control over customer allocated HSMs or control over the HSM's users other than its own limited Appliance User. Microsoft possesses restricted permissions to the HSM, allowing monitoring, maintenance of health and availability, encrypted backups, and extraction and publication of immutable audit logs to the customers specified storage. Microsoft's permissions on the HSM do not allow it to use CU-owned keys to perform cryptographic operations.

### **Does Azure Cloud HSM store customer data?**

No, Azure Cloud HSM does not retain customer data. All key materials and data are housed within the customers' HSM. Each Azure Cloud HSM cluster is exclusively designated for a single customer, granting them sole administrative control.

### Does Azure Cloud HSM **support **FIPS** 140-**3** Level 3?**

Yes, Azure Cloud HSM offers HSMs validated to meet the FIPS 140-3 Level 3 standards. You can refer to the onboarding guide for procedures to verify the authenticity of your HSM, including checking the [FIPS 140-3 Level 3 certification from NIST](https://csrc.nist.gov/projects/cryptographic-module-validation-program/certificate/4700). 

### **How do I operate an Azure Cloud HSM in Non-FIPS or FIPS Approved Mode?**

Azure Cloud HSM enables customers to designate the FIPS mode of operation during creation. The default setting for Azure Cloud HSM is Non-FIPS Mode enabled. By default, Azure Cloud HSM operates in Non-FIPS Mode, meaning it is in a Non-FIPS state, while the partitions remain in a FIPS state. When Azure Cloud HSM is in FIPS Approved Mode, the partitions are also in a FIPS state, but some cryptographic functions may be restricted if they are not FIPS-compliant. In both modes, authentication is single factor, using a password. This can be verified by using the Azure Cloud HSM tools documented.

| **FIPS State** | **Value** | **Description**                                                                              |
| -------------------- | --------------- | -------------------------------------------------------------------------------------------------- |
| Non-FIPS Mode        | 0               | When HSM is initialized, it will be in non-FIPS state, but partition will always be in FIPS state. |
| FIPSApproved Mode    | 2               | When HSM is initialized, it will be in FIPS state, and partition will be in FIPS state.            |

### **What happens if someone tampers with the HSM hardware?**

Azure Cloud HSM incorporates both physical and logical tamper detection and response mechanisms that initiate key deletion (zeroization) of the hardware. These measures are designed to detect tampering if the physical barrier is compromised. Additionally, HSMs are safeguarded against brute-force login attacks, with the system locking out Crypto Officers (COs) after a set number of unsuccessful access attempts. Similarly, repeated unsuccessful attempts to access an HSM with Crypto User (CU) credentials will result in the user being locked out, requiring a CO to unlock them. Unlocking a CO user requires getChallenge, signing the challenge with your PO.key using OpenSSL, followed by unlockCO and changePswd commands.

## **Support**

### **How do I get support for Azure Cloud HSM?**

All support for Azure Cloud HSM is facilitated exclusively through Microsoft. Should you encounter any issues related to hardware, software, HSM configuration, or network access, such as the examples provided, please submit a support request to Microsoft.

### **How are the HSMs used in Azure **Cloud** HSM protected?**

Azure datacenters have extensive physical and procedural security controls. In addition to that Azure Cloud HSMs are hosted in a further restricted access area of the datacenter. These areas have additional physical access controls and video camera surveillance for added security.

### **Can Microsoft recover my keys if I lose my credentials to my HSM?**

No, Microsoft does not possess access to your keys or credentials, and as a result, cannot recover your keys if you lose your credentials.

### **Does Azure Cloud HSM have scheduled maintenance windows?**

No, although Microsoft may need to perform maintenance in cases of necessary upgrades or faulty hardware. We will endeavor to notify customers in advance if any impact is anticipated.

### **What is the SLA for **Azure Cloud** HSM?**

Azure Cloud HSM provides an SLA of 99.9% uptime availability.
