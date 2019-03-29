# Data Encryption at Storage {#concept_gpb_zs5_42b .concept}

Alibaba Cloud products provide various methods to encrypt static data, as shown in the following table:

|Product|Encryption Method|
|:------|:----------------|
|OSS|[OSS client-side encryption](#)|[OSS Server-side encryption](#)|
|RDS|[SSL encryption](#)|[TDE encryption](#)|
|ECS Disk|To encrypt the data stored on a disk, you can use the [ECS disk encryption](#) function to encrypt cloud disks and shared block storage.|

## OSS encryption {#section_l2p_435_m2b .section}

OSS supports client-side encryption and server-side encryption.

**OSS client-side encryption**

Client encryption means that the encryption is completed before the user data is sent to the remote server, whereas the plaintext of the key used for encryption is kept in the local computer only. Therefore, the security of user data can be ensured because others cannot decrypt the data to obtain the original data even if the data leaks.

-   Main private key hosted by using KMS
-   Private key managed by the user

**OSS server-side Encryption**

OSS supports server-side encryption for the data uploaded by users: When a user uploads data, OSS encrypts the user data and permanently stores the data with encryption; when the user downloads the data, OSS automatically decrypts the encrypted data, returns the original data to the user, and declares in the header of the returned HTTP request that the data has been encrypted on the server.

For the details, see [Server-side encryption](../../../../../intl.en-US/Developer Guide/Data encryption/Server-side encryption.md).

## RDS encryption {#section_qwn_ql5_m2b .section}

RDS supports SSL and TDE encryption.

**SSL encryption**

RDS provides Secure Sockets Layer \(SSL\) for MySQL and SQL Server. You can use the server root certificate provided by RDS to verify whether the database service with the target IP address and port is provided by RDS, which can effectively prevent man-in-the-middle attacks. To guarantee security and validity, RDS allows you to enable and update the SSL certificates for servers.

Though RDS can encrypt the connection between an application and a database, the SSL service can run properly only after the application enables authentication on the server. In addition, SSL results in extra CPU resource consumption and affects the throughput and response time of RDS instances to a certain degree. The specific impact varies depending on the number of user connection times and the data transfer frequency.

For more information about enabling and configuring SSL encryption service, see [Set SSL Encryption](https://www.alibabacloud.com/help/doc-detail/32474.htm).

**TDE encryption**

RDS provides transparent data encryption \(TDE\) for MySQL and SQL Server. The TDE function of RDS for MySQL is developed by Alibaba Cloud and the TDE function of RDS for SQL Server is based on the SQL Server Enterprise Edition.

You can specify the database or table to be encrypted in a TDE-enabled RDS instance. The data of the specified database or table is encrypted before being written to any device such as an HDD, SSD, or PCIe card, or to any service such as OSS or Archive Storage. Therefore, data files and backups of the instance are all ciphertext.

TDE adopts the Advanced Encryption Standard \(AES\) algorithm. The key length is 128 bits. The key for TDE is encrypted and stored by KMS, and RDS dynamically reads the key only once when the instance is started or migrated. You can replace the key as needed on the KMS console.

For more information about the TDE encryption servcie, see [Set Trasparent Date Encryption](https://www.alibabacloud.com/help/doc-detail/33510.htm).

## ECS disk encryption {#section_qty_bm5_m2b .section}

When you want to encrypt the data stored on a disk due to business needs or certification requirements, you can use ECS disk encryption function to encrypt cloud disks and shared block storage \(referred to collectively as cloud disks, unless otherwise specified\). This secure encryption feature allows you to encrypt new cloud disks. You do not have to create, maintain, or protect your own key management infrastructure, nor change any of your existing applications or maintenance processes. In addition, no extra decryption operations are required, so the operation of the disk encryption function is practically invisible to your applications or your operations.

Encryption and decryption barely degrades cloud disk performance. For information on the performance testing method, see Disk parameters and performance test.

After an encrypted cloud disk is created and attached to an ECS instance, the data in the following list can be encrypted:

-   Data on the cloud disk.
-   Data transmitted between the cloud disk and the instance. However, data in the instance operating system is not encrypted.
-   All snapshots created from the encrypted cloud disk. These snapshots are called encrypted snapshots.
-   Encryption and decryption are performed on the host that runs the ECS instance, so the data transmitted from the ECS instance to the cloud disk is encrypted.

The ECS disk encryption function handles key management for you. Each new cloud disk is encrypted using a unique 256-bit key \(derived from the CMK\). This key is also associated with all snapshots created from this cloud disk and any cloud disks subsequently created from these snapshots. These keys are protected by Alibaba Cloud's key management infrastructure \(provided by KMS\). This approach implements strong logical and physical security controls to prevent unauthorized access. Your data and the associated keys are encrypted using an industry standard AES-256 algorithm.

Alibaba Cloud's overall key management infrastructure conforms with the recommendations in \(NIST\) 800-57 and uses cryptographic algorithms that comply with the \(FIPS\) 140-2 standard.

Each Alibaba Cloud ECS account has a unique CMK in each region. This key is separate from the data and stored in a system protected by strict physical and logical security controls. Each encrypted disk uses an encryption key unique to the specific disk and its snapshots. The encryption key is created from and encrypted by the CMK for the current user in the current region. The disk encryption key is only used in the memory of the host that runs your ECS instance. The key is never stored in plaintext in any permanent storage media \(such as a disk\).

For more information about cloud disk encryption, see [ECs disc encryption](../../../../../intl.en-US/Block storage/Block storage/ECS disk encryption.md).

