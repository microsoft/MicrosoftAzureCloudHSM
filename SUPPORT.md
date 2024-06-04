# Support

## How do I file production support incidents or get help? 

All production incident support tickets for Azure Cloud HSM should be submitted through the Azure Portal under Help+Support. 

The Azure Cloud HSM SDK uses GitHub issues solely for tracking bugs and feature requests specific to the SDK. It does not handle production live site support incidents. To avoid duplicates, please search existing issues before creating a new one. For new issues, submit your bug report or feature request as a new Issue.

For help and questions about onboarding to Azure Cloud HSM or the Azure Cloud HSM SDK, please contact your Microsoft Account Manager. If you do not have an assigned Account Manager, please contact Microsoft Support.

## Microsoft Support Policy  

Support for the Azure Cloud HSM SDK are limited to the resources listed above.

# **Azure Cloud HSM Self Help**

## **Common Error Messages**

### **Why am I getting a Certificate ERROR: **[ certificate** signature **failure ]** when running **azcloudhsm_mgmt_util**?**

This error commonly occurs when a customer created and initialized their Azure Cloud HSM from another VM and attempts to install the Cloud HSM Client SDK and run from another VM that is missing or does not have the correct PO.crt from the Admin VM you initialized from. If you copy the PO.crt from your Admin VM to your new VM and rerun the azcloudhsm_mgmt_util you should see a successful connection to your HSM.

### **Why am I getting an INF: **shutdown_ssl_socket**: **SSL_shutdown** sent **close_notify** alert when running **azcloudhsm_client**?**

This error commonly occurs when a customer created and initialized their Azure Cloud HSM from another VM and attempts to install the Cloud HSM Client SDK and run from another VM that is missing or does not have the correct PO.crt from the Admin VM you initialized from. If you copy the PO.crt from your Admin VM to your new VM and rerun the azcloudhsm_client you should see a successful connection to your HSM.

### **Why am I getting error message **azcloudhsm_application.cfg** is not present?**

To ensure the successful execution of your application, two conditions must be met. Firstly, the azcloudhsm_application.cfg file must be present in the same directory as your application. Secondly, the azcloudhsm_client should be running. If the azcloudhsm_client is not active, you will encounter a failed socket connection. Likewise, the absence of the azcloudhsm_application.cfg file will result in an error message. Depending on the execution location of your application, you may need to copy the azcloudhsm_application.cfg file from the Azure Cloud HSM SDK directory to the directory where you are running your application.

### Why am I getting error message **HSM Error: This operation violates the current configured/FIPS policies.**

The error message provided suggests the current configured FIPS Mode (i.e FIPS State) of your Azure Cloud HSM that attempted to perform a cryptographic operation is not permitted to use that cryptographic function per the FIPS mode policy you configuration agaisnt your HSM. Azure Cloud HSM enables customers to designate the FIPS state during creation. The default setting for Azure Cloud HSM is Non-FIPS Mode enabled (unrestricted). If you configured your Azure Cloud HSM to operate in FIPS Approved Mode some cryptographic functions may be restricted if they are not FIPS-compliant.

## **Azure Cloud HSM OpenSSL Engine**

### **Does Azure Cloud HSM OpenSSL Engine support Windows?**

No. Azure Cloud HSM OpenSSL Engine supports Linux (Ubuntu 20.04, Ubuntu 22.04, RHEL 7, RHEL 8, CBL Mariner 2) only.

### **Why am I getting error message invalid engine "**azcloudhsm_openssl**" could not load the shared library?**

The error message suggests that the environment variable LD_LIBRARY_PATH has not been configured, and this configuration is necessary. To resolve this, you should set the LD_LIBRARY_PATH environment variable to the directory where the Azure Cloud HSM SDK is located.

* You will need to update your environment variables to reflect the correct SDK version running from the example below.

  ```
  export LD_LIBRARY_PATH=/usr/local/lib64/AzureCloudHSM-ClientSDK-1.0.3.0/:$LD_LIBRARY_PATH
  ```

### Why am I getting error message **environment variable **azcloudhsm_openssl_conf** is not set?**

The error message signals that the prerequisite environment variable azcloudhsm_openssl_conf has not been configured. To resolve this issue, you should set the azcloudhsm_openssl_conf environment variable to the file path location of your azcloudhsm_openssl_dynamic.conf file within the Azure Cloud HSM SDK directory.

* **You will need to update your environment variables to reflect the correct SDK version running from the example below.**

```
export azcloudhsm_openssl_conf=/usr/local/bin/AzureCloudHSM-ClientSDK-1.0.3.0/azcloudhsm_openssl_dynamic.conf 
```

### **Why am I getting error message environment variable **azcloudhsm_password** is not set?**

The provided error message suggests that the prerequisite environment variable azcloudhsm_password has not been configured. To resolve this issue, it is necessary to set the azcloudhsm_password environment variable to the value of your cryptouser:password.

* You will need to update your environment variables to reflect the correct crypto user and password from the example below.

```
	export azcloudhsm_password="cu1:user1234"
```

## **Azure Cloud HSM JCE Provider**

### **Does Azure Cloud HSM JCE support Windows?**

No. Azure Cloud HSM JCE supports Linux (Ubuntu 20.04, Ubuntu 22.04, RHEL 7, RHEL 8, CBL Mariner 2) only.

### **Why am I getting error messages that could not find credentials to login to the HSM?**

The error indicates a challenge in locating or authenticating the credentials required for logging into Cloud HSM. When utilizing the samples in this documentation or incorporating the Azure Cloud HSM JCE provider into your applications, it is essential to explicitly log in with credentials, as illustrated in the example below.

```
// Explicit Login with Credentials
LoginManager lm=LoginManager.getInstance();
lm.login("PARTITION_1","cu1","user1234");

// Logout of the HSM
lm.logout();
```

### Why am I getting error message attribute with value 0 for this operation. Attribute should set for User KEK?

The error indicates that a user-generated KEK was not created and designated as extractable and wrap_with_trusted set to 1. Please see creating a user-generated KEK in the JCE integration guide for appropriate settings when performing Key Import / Keywrap / Keyunwrap operations.

### **Why am I getting error message daemon socket connection error, API login failed?**

The error indicates a challenge in locating or authenticating the credentials required for logging into Cloud HSM. When utilizing the samples in the JCE integration guide or incorporating the Azure Cloud HSM JCE provider into your applications, it is essential to explicitly log in with credentials to ensure connection.

## **Azure Cloud HSM PKCS#11 Library**

### **C_Login** failed with error code: 0xa3.

Encountering (0xa3) indicates a login failure that the issue might be related to the format of the PIN parameter you're passing or incorrect PIN (password).

* The PIN should be provided in the following format: char pPin[256] = "crypto_user:user123"; The pPin value should follow the structure of "`<username>`:`<password>`". Please verify that you are providing the pPin in this format, as any deviation might result in a login failure.

### **C_Initialize**() failed with 00000005 (Failed to connect socket).

Encountering a failed socket connection during C_Initialize might be related to the azcloudhsm_client utility not running on your system. For your PKCS#11 applications to execute successfully the azcloudhsm_client must be running. You can start the azcloudhsm_client by running the following command in a separate terminal.

* **Linux:**

```
sudo ./azcloudhsm_client azcloudhsm_client.cfg
```

* **You may also choose as **an option** to run the client daemon in the background using the following command or run the client daemon as a service to ensure it is always running.**

```
sudo ./azcloudhsm_client azcloudhsm_client.cfg > /dev/null 2>&1 &
```

* **Windows**: There are two ways to launch the azcloudhsm_client for Windows.
  1. Launch the azcloudhsm_client manually (not recommended). Manual execution should only be used for testing purposes.

     ```
     .\azcloudhsm_client.exe azcloudhsm_client.cfg
     ```
  2. Run the azcloudhsm_client as a service (recommended). We suggest running azcloudhsm_client as a service in a production environment to ensure continuous and automated operation. When you install the Azure Cloud HSM SDK via the MSI package, it will configure the azcloudhsm_client to run as a service.

### How does the PKCS#11 library know how to find the client configuration for **Linux.**

The PKCS#11 library knows how to find the client configuration as you must have a copy of your partition owner certificate "PO.crt" on the application server that is running your application and using the PKCS#11 library. In addition to PO certificate you have to update /azcloudhsm_client/azcloudhsm_client.cfg on your application server that has the SDK installed to point to your Azure Cloud HSM (i.e hsm1.chsm-`<resourcename>`-`<uniquestring>`.privatelink.cloudhsm.azure.net). The azcloudhsm_client utility must be running on your application server which connects to your Azure Cloud HSM. Finally, you must specify a PIN within your PKCS#11 application using syntax `<username>`:`<password>` which is used when calling C_Login to your Azure Cloud HSM. You will also need to #include pkcs11_headers/include/cryptoki.h and pkcs11_headers/include/pkcs11t.h in your PKCS#11 application to use the Azure Cloud HSM PKCS#11 library.

### **How does the PKCS#11 library (**azcloudhsm_pkcs11.dll**) for Windows know how to find the client configuration?**

The azcloudhsm_pkcs11.dll under Azure Cloud HSM Windows SDK knows how to find the client configuration as you must have a copy of your partition owner certificate "PO.crt" on the application server that is running your application and using the PKCS#11 library. In addition to PO certificate you have to update /azcloudhsm_client/azcloudhsm_client.cfg on your application server that has the SDK installed to point to your Azure Cloud HSM (i.ehsm1.chsm-`<resourcename>`-`<uniquestring>`.privatelink.cloudhsm.azure.net). The azcloudhsm_client must run on your application server which connects to your Azure Cloud HSM. Finally, you must specify a PIN within your PKCS#11 application using syntax `<username>`:`<password>` which is used when calling C_Login to your Azure Cloud HSM. You will also need to #include pkcs11_headers\include\cryptoki.h and pkcs11_headers\include\pkcs11t.h in your PKCS#11 application to use the Azure Cloud HSM PKCS#11 library.

### **Why do I get PKCS#11 errors when trying to set key or keystore?**

Ensure that you have the Azure Cloud HSM client running [sudo ./azcloudhsm_client azcloudhsm_client.cfg]. An incorrect user, password or liquid security client not running or client failing to connect may result in the following error below.

```
Error Message:ERROR at line 1:
ORA-28407: Hardware Security Module failed with PKCS#11 error
CKR_GENERAL_**ERROR(5)
```

### **How does the PKCS#11 library (**azcloudhsm_pkcs11.dll**) integrate with **OpenSC** (Open-Source Smart Card Tools)?**

Download OpenSC MSI for Windows: [OpenSC: Open source smart card tools and middleware. PKCS#11/MiniDriver/Tokend (github.com)](https://github.com/OpenSC/OpenSC)

* Customers must use the “--sensitive" and "--private" arguments when using OpenSC tools as the sensitive and private attributes are not allowed to be 0. Without using those arguments, the sensitive and private attributes are set to 0 by OpenSC tooling and will cause an exception as Azure Cloud HSM always imposes sensitive and private behavior.

  * AES Wrapping Example: pkcs11-tool.exe --sensitive --private --module "C:\AzureCloudHSM-ClientSDK\AzureCloudHSM-ClientSDK-Windows-1.0.3.0\libs\pkcs11\azcloudhsm_pkcs11.dll" --login --login-type user --pin cu1:user1234 --keygen --key-type AES:32
  * ECC P521 Example: pkcs11-tool.exe --sensitive --private --module "C:\AzureCloudHSM-ClientSDK\AzureCloudHSM-ClientSDK-Windows-1.0.3.0\libs\pkcs11\azcloudhsm_pkcs11.dll" --login --login-type user --pin cu1:user1234 --keypairgen --key-type EC:prime256v1 --usage-sign

## **Azure Cloud HSM SSL/TLS Offloading**

### **Does Azure Cloud HSM OpenSSL Engine support PKCS#11 for SSL/TLS offloading?**

No. The requirement of OpenSSL supporting PKCS#11 for SSL/TLS offloading is not supported by Azure Cloud HSM. Customers that require PKCS#11 for SSL/TLS Offloading must use the [TLS Offload Library for Azure Managed HSM](https://github.com/microsoft/AzureManagedHsmTLSOffload).

### **Why am I getting error message Failed to connect socket, LIQUIDSECURITY: Daemon socket connection error when running **azcloudhsm_util**?**

The error message suggests that azcloudhsm_client is not running. You need to ensure that the azcloudhsm_client is running for the Azure Cloud HSM utilities to properly execute. You can start the client daemonby running the following command in the background.

```
sudo ./azcloudhsm_client azcloudhsm_client.cfg > /dev/null 2>&1 &
```

### **Why am I getting error message environment variable **azcloudhsm_password** is not set?**

The provided error message suggests that the prerequisite environment variable azcloudhsm_password has not been configured. To resolve this issue, it is necessary to set the azcloudhsm_password environment variable to the value of your cryptouser:password.

* You will need to update your environment variables to reflect the correct crypto user and password from the example below.

```
export azcloudhsm_password="cu1:user1234"
```

### Why and I getting error message **Key/Token not found or Invalid key-handle/token when **attempting** to Import Key?**

The provided error message suggests that the specified wrapping key handle (i.e KEK Handle ID) is incorrect. The parameters '-f' specifies the filename containing the key to import, while '-w' specifies the wrapping key handle. During the creation of a user generated KEK the '-w' value should be the key handle id of your user generated KEK id.

```
azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 importPrivateKey -l importTestKey -f key.pem -w 262150
```

### **Why am I getting error message /configure: error: C compiler cc not found when trying to configure the Nginx sources?**

The error message suggests that when you are trying to run configure command that you are missing C compiler from your system. Depending on your installation after running the following commands you may also need to install 'openssllibssl-dev' and 'zlib1g zlib1g-dev' which is a dependency.

* **For Red Hat based (using yum):**

```
sudo yum update
sudo yum groupinstall "Development Tools"
```

* **For Red Hat based systems (using dnf):**

```
sudo dnf update
sudo dnf install "Development Tools”
```

* **For Ubuntu based systems (using apt):**

```
sudo apt update  
sudo apt install build-essential 
```

### **Why am I getting error message /configure: error: SSL modules require the OpenSSL library?**

The error message indicates that the OpenSSL library is required to enable SSL modules for Nginx. To resolve this issue, you can run the following command and then try to run configure again.

```
sudo apt-get install libssl-dev
```

### **Why am I getting error message /configure: error: the HTTP **gzip** module requires the **zlib** library?**

The error message indicates that the zlib library is required for the HTTP gzip module in Nginx. To resolve this issue, you can run the following command and then try to run configure again.

```
sudo apt-get install zlib1g-dev
```

### **Why am I getting error message HSM Error: RET_USER_LOGIN_FAILURE?**

The error message indicates that the specified user does not exist on one or more nodes where you're attempting a cryptographic operation. Although Azure Cloud HSM operates with a cluster of three nodes, the operation was directed to a node where the login failed. In such cases, a critical_err_info_ file is generated within the AzureCloudHSM-ClientSDK directory that will show Status: HSM Error: This user doesn't exist.

* To mitigate this error, you need to create a 'user' with the Crypto User role that is replicated across all three nodes of your Azure Cloud HSM. This example shows starting azcloudhsm_mgmt_util and administrator logging on as CO, then proceeds to create 'cu1' user with crypto user role. 

```
sudo ./azcloudhsm_mgmt_util ./azcloudhsm_mgmt_util.cfg
loginHSM CO admin adminpassword 
createUser CU cu1 user1234
logoutHSM
loginHSM CU cu1 user1234
```

### **How do I display the contents of the CRT, CSR, and Key files I created?**

* Display the contents of the CRT, CSR, or Key file you created in your terminal.

```
cat filename.key
```

* Display detailed information about a X.509 certificate contained in the PEM file. If your PEM file contains a private key or other types of data, you might need to adjust the command accordingly.

```
openssl x509 -in filename.pem -text -noout
```

* Display detailed information about a CSR, including the subject, public key, and any attributes included in the request.

```
openssl req -in filename.csr -text -noout
```

* Display detailed information about an RSA private key, including its modulus, public exponent, and other parameters.

```
openssl rsa -in filename.key -text -noout
```

* Display detailed information about an EC private key, including the curve parameters, private key value, and other relevant information.

```
openssl ec -in filename.key -text -noout
```

### **How do I generate an ED25519 key pair in Azure Cloud HSM for signing?**

Creation of ED25519 is only permitted in non-FIPS mode. ED25519 keys are typically used for self-signed certificates or in certificate signing processes that directly use the private key.  You can generate an ED25519 key pair using azcloudhsm_util or the Azure Cloud HSM OpenSSL engine.

Important Note: openssl genpkey and openssl ecparam are both OpenSSL commands used for generating cryptographic keys, but they serve different purposes and support different key types. Use opensslgenpkey when you need to generate a wide range of asymmetric cryptographic keys, including RSA, DSA, ECDSA, and ED25519 keys. Use openssl ecparam specifically for generating elliptic curve cryptographic keys (ECDSA), such as prime256v1, secp384r1, or secp521r1.

**azcloudhsm_util**

```
./azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 genECCKeyPair -i 28 -l labelED25519Test
```

**Azure Cloud HSM OpenSSL Engine**

```
openssl genpkey -algorithm ed25519 -out web_server.key -engine azcloudhsm_openssl 
```

### **Can I use **azcloudhsm_util** to generate RSA and EC keys before using Azure Cloud HSM OpenSSL engine to generate CSR?**

Yes.  The following azcloudhsm_util commands can be run to create an RSA or EC key and then extract the private key to a fake pem format. Replace {PRIVATE_KEY_HANDLE} with the private key handle of the RSA or EC key you just created above. The private key meta file in PEM format does not contain any sensitive private key materials. It is just meta data that identifies the private key, and this meta file can only be understood by the Azure Cloud HSM OpenSSL engine.

**RSA Key:**

```
./azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 genRSAKeyPair -m 2048 -e 65537 -l labelRSATest 
./azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 getCaviumPrivKey -k {PRIVATE_KEY_HANDLE} -out web_server_fake_PEM.key 
openssl req -new -key web_server_fake_PEM.key -out web_server.csr -engine azcloudhsm_openssl
openssl x509 -req -days 365 -in web_server.csr -signkey web_server_fake_PEM.key -out web_server.crt -engine azcloudhsm_openssl 
```

**EC Key:**

```
./azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 genECCKeyPair -i 2 -l labelECTest 
./azcloudhsm_util singlecmd loginHSM -u CU -p user1234 -s cu1 getCaviumPrivKey -k {PRIVATE_KEY_HANDLE} -out web_server_fake_PEM.key
openssl req -new -key web_server_fake_PEM.key -out web_server.csr -engine azcloudhsm_openssl
openssl x509 -req -days 365 -in web_server.csr -signkey web_server_fake_PEM.key -out web_server.crt -engine azcloudhsm_openssl 
```

## **Azure Cloud HSM SQL EKM**

### **Does Azure Cloud HSM SQL EKM Provider support Symmetric Keys?**

No. Microsoft SQL Server does not support symmetric keys for SQL TDE. Only Asymmetric keys are supported!

### **Does Azure Cloud HSM SQL EKM Provider support SQL Server on Linux?**

No. Azure Cloud HSM SQL EKM provider only supports Windows Server.

### **Does Azure Cloud HSM SQL EKM Provider support SQL Server SaaS/PaaS?**

No. Azure Cloud HSM is IaaS only. Support for SaaS/PaaS customers should use Azure Managed HSM which supports SQL TDE for Azure SQL DB and Azure SQL Managed Instance.

### **Does Azure Cloud HSM SQL EKM support multiple logins, credentials, connections?**

Azure Cloud HSM SQL EKM the login or SQL connection is limited and can only have a single credential assigned.

### **Why am I getting Error: Cannot initialize cryptographic provider. Provider error code: 1. (Failure - Consult EKM Provider for details)?**

This error commonly occurs when the 'ekm-provider-setup.ps1' script is run after SQL Server is installed. This error goes away once you restart SQL Server as it will then pick up the environment variables and settings created when 'ekm-provider-setup-ps1' was executed.

## **Azure Cloud HSM ADCS Integration**

### **Why am I getting a **can't** open **openssl-ca.cnf** file when trying to migrate a certificate from Microsoft KSP from provided example?**

This error commonly occurs when the openssl-ca.cnf does not exist. You can create one yourself or obtain it from another source.

### **Can we use our own signing servers in our corporate network which consumes Azure Cloud HSM as SaaS solution?**

Azure Cloud HSM is IaaS only. You can use your own signing servers in your corporate network, however. You can configure Site-to-Site or Point-to-Site VPN connection from your local network gateway. This is the most common method, and it's suitable for most use cases. Set up a VPN connection between your corporate network's on-premises and Azure to where Azure Cloud HSM is deployed.

* [Tutorial - Connect an on-premises network and a virtual network: S2S VPN: Azure portal - Azure VPN Gateway | Microsoft Learn](https://learn.microsoft.com/en-us/azure/vpn-gateway/tutorial-site-to-site-portal)
* You can have ADCS for your signing servers on-prem. What is required is SDK must be on your on-prem host and the client be reachable to your Azure VNET Private IPs.
  * The customer needs to run the azcloudhsm_client on the same machine where Azure Cloud HSM CNG provider exists.
  * The azcloudhsm_client should be able to reach out to the Customers Azure vNET private IPs.

### **When testing ADCS we generated an **RSAKeyPair** then used CFM to Sign.**

* **Are the only accepted methods to sign/verify through Cloud HSM providers (i.e azcloudhsm_util, etc.)?**

  * No. Our recommendation is to use the Azure Cloud HSM SDK and the interfaces (PKCS#11, CNG, KSP, JCE, OpenSSL Engine, Etc.) it provides. You can use the Sign Tool for sign/verify operations as well.
* **ADCS is configured to use Azure Cloud HSM CNG/KSP. Can we use the signing / verify from Cloud HSM via signtool.exe?**

  * Yes. The Sign Tool will automatically go to your Cloud HSM. There is no need to use azcloudhsm_util for that. We just included azcloudhsm_util in the instructions above as a way for customers to quickly test and validate their ADCS configuration.

### **Can we use self-signed certs based on the key-pair in Cloud HSM for testing or have it **be** issued?**

Yes. You can use self-signed certificates based on the key-pair for testing purposes.

### **Is there an integration or way to make available the certs via the local windows certificate store in windows?**

There is a tool called certreq.exe that can be used for this purpose.

### **What algorithms do the Azure Cloud HSM CNG and KSP providers support?**

* RSA encryption/decryption.
* RSA signing with SHA1, SHA256, SHA384, SHA512, and MD5 Hash algorithms.
* ECC curves ECDSA_P256, ECDSA_P384, and ECDSA_P521.

### **How do I change or manage private key permissions to non-administrative users on Windows?**

When generating a key using the Azure Cloud HSM KSP or CNG, only the Administrator is granted access to the private key. You may encounter difficulties in viewing security information or modifying permissions via MMC. This behavior is intentional. By default, when utilizing KSP or CNG, access to the private key file for a new key is restricted to SYSTEM and Administrators. If there is a need to provide access to a non-administrative user for a specific private key file, you can achieve this by identifying the private key filename and assigning permissions directly. The private keys are in the directory C:\Users\Default\AppData\Roaming\Microsoft\Crypto\CaviumKSP on Windows for Cloud HSM keys.

### **What does azcloudhsm_ksp_import_key.exe do?**

azcloudhsm_ksp_import_key.exe allows customers to import or represent **an **asymmetric key/keys** in Cloud HSM KSP.** 

* Supported algorithms are RSA (2048 to 4096 in multiples of 256), ECDSA and ECDH. For ECDSA and ECDH supported curves are P256, P384 and P521. Once a key is imported or represented in Cloud HSM KSP, the key should only be managed from Cloud HSM KSP.

### **What is the recommended way of "connection" to Cloud HSM from the Corporate Network (Operation/Admin tasks)?**

Customers can connect through their VNET. 

* azcloudhsm_mgmt_util executes operations (admin tasks on the HSM) which requires PCO (partition crypto officer) credentials.
* azcloudhsm_client (client daemon) connects via PCU (partition crypto user).
  * The azcloudhsm_client and customers application need to both run on the same VM within that VNET. This is because the application connects to the client process via RPC.
    * The Azure Cloud HSM service listens on port 443 (azcloudhsm_client requests), 444 (azcloudhsm_mgmt_util requests) and 445 (server-server communication).
    * Front end ports are 2224, 2225
      * TCP over TLS protocol and Ports 2224 and 2225
      * Ports 2224 and 2225 are for customers only.

