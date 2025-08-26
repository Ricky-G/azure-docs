---
title: Configure object REST API access in Azure NetApp Files 
description: Learn how to configure object REST API access to manage S3 objects in Azure NetApp Files. 
services: azure-netapp-files
author: b-ahibbard
ms.service: azure-netapp-files
ms.topic: how-to
ms.date: 08/26/2025
ms.author: anfdocs
---

# Configure object REST API access in Azure NetApp Files (preview)

Azure NetApp Files supports read access to S3 objects with the [object REST API](object-rest-api-introduction.md) feature. With the object REST API feature, you can connect to services including Azure AI Search, Azure AI Foundry, Azure Databricks, OneLake, and others.

## Register the feature 

The object REST API access feature in Azure NetApp Files is currently in preview. You need to register the feature before using it for the first time. Feature registration can take up to 60 minutes to complete.

1. Register the feature

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFEnableObjectRESTAPI
    ```

2. Check the status of the feature registration: 

    > [!NOTE]
    > The **RegistrationState** can remain in the `Registering` state for up to 60 minutes before changing to `Registered`. Wait until the status is `Registered` before continuing.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFEnableObjectRESTAPI
    ```

You can also use [Azure CLI commands](/cli/azure/feature) `az feature register` and `az feature show` to register the feature and display the registration status. 

## Create the self-signed certificate

You must generate a PEM-formatted SSL certificate. For instructions. You can create the SSL certificate in the Azure portal or with a script.  

### [Portal](#tab/portal)

See the [Azure Key Vault documentation for creating a certificate](/azure/key-vault/certificates/tutorial-import-certificate). 

When creating the certificate, ensure the **Content Type** is set to PEM. In the **Subject** field, set the Common Name (CN) to the IP address or fully qualified domain name (FQDN) of your Azure NetApp Files object REST API-enabled endpoint.

### [Script](#tab/script)

This script creates a certificate locally. Set the computer name `CN=` and domain `DOMAIN=` to the IP address or full qualified domain name (FQDN) of your object REST API-enabled endpoint. This script creates a folder that includes the necessary PEM file and private keys. 

1. Create and run the following script:

```bash
#!/bin/sh
# Define certificate details 
CERT_DAYS=365 
RSA_STR_LEN=2048 
CERT_DIR="./certs" 
KEY_DIR="./certs/private" 
DOMAIN="mylocalsite.local" 

# Create directories if they don't exist 
mkdir -p $CERT_DIR 
mkdir -p $KEY_DIR 

# Generate private key 
openssl genrsa -out $KEY_DIR/server-key.pem $RSA_STR_LEN 

# Generate Certificate Signing Request (CSR) 
openssl req -new -key $KEY_DIR/server-key.pem -out $CERT_DIR/server-req.pem -subj "/C=US/ST=State/L=City/O=Organization/OU=Unit/CN=$DOMAIN" 

# Generate self-signed certificate 
openssl x509 -req -days $CERT_DAYS -in $CERT_DIR/server-req.pem -signkey $KEY_DIR/server-key.pem -out $CERT_DIR/server-cert.pem 

echo "Self-signed certificate created at $CERT_DIR/server-cert.pem" 
```
--- 

## Enable object REST API access

1. From your NetApp volume, select **Buckets**. 
1. To create a bucket, select **+Create**. 
1. Provide the following information for the bucket:
    * **Name**
        Specify the name for your bucket. Refer to [Naming rules and restrictions for Azure resources](../azure-resource-manager/management/resource-name-rules.md#microsoftnetapp) for naming conventions.
    * **Path**
        The subdirectory path for object REST API access. For full volume access, leave this field blank or use `/` for the root directory.
    * **UserID (UID)**
        The UID used to read the bucket.
        <!-- UID to match UI -->
    * **GroupID (GID)**
        The GID used to read the bucket.
        <!-- GUID to match UI -->
    * **Permissions**
        Select Read or Read-Write. 

    :::image type="content" source="./media/object-rest-api-access-configure/create-bucket.png" alt-text="Screenshot of create a bucket menu." lightbox="./media/object-rest-api-access-configure/create-bucket.png":::

1. If you haven't provided a certificate, also provide the following information:
    * **Fully qualified domain name**
        Enter the fully qualified domain name. 
    * **Certificate source**
        Upload the appropriate certificate. Only PEM files are supported. 

    :::image type="content" source="./media/object-rest-api-access-configure/certificate-management.png" alt-text="Screenshot of certificate management options." lightbox="./media/object-rest-api-access-configure/certificate-management.png":::

1. Select **Create**. 

<!--
ACCESS KEYS
1. In your NetApp account, navigate to **Buckets**. 
1. For the bucket you want to create an access key for, select **Generate keys**. 
1. In the **Access key lifespan** field, provide a numerical value for the number of days the key is valid. 
1. When the key successfully generates, the portal presents your masked Access key and Secret access key. Display and securely save the information. 
-->

## Edit a bucket

After you create a bucket, you have the option to modify the user identifier (UID or GUID) of the bucket.

1. In your NetApp account, navigate to **Buckets**. 
1. Select the three dots `...` at the end of the line next to the name of the bucket you want to modify then select **Edit**. 
<!-- confirm access keys -->
1. Enter the new User ID or Group User ID for the bucket. 
1. Select **Save**. 

## Delete a bucket

Deleting a bucket is a permanent operation. You can't recover the bucket once it's deleted. 

1. In your NetApp account, navigate to **Buckets**. 
1. Select the checkbox next to the bucket you want to delete. 
1. Select **Delete**. 
1. In the modal, select **Delete** to confirm you want to delete the bucket. 

## Next steps 

* [Understand REST API object](object-rest-api-introduction.md)
* [Connect to Azure Databricks](object-rest-api-databricks.md)
* [Connect to an S3 browser](object-rest-api-browser.md)
* [Connect to OneLake](object-rest-api-onelake.md)
