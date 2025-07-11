---
title: "Quickstart: Azure Blob Storage library - C++"
titleSuffix: Azure Storage
description: In this quickstart, you learn how to use the Azure Blob Storage client library for C++ to create a container and a blob in Blob (object) storage. Next, you learn how to download the blob to your local computer, and how to list all of the blobs in a container.
author: pauljewellmsft

ms.author: pauljewell
ms.date: 08/30/2023
ms.service: azure-blob-storage
ms.topic: quickstart
ms.devlang: cpp
ms.custom: mode-api
# Customer intent: As a C++ developer, I want to utilize the Azure Blob Storage client library so that I can efficiently manage unstructured data by creating containers, uploading and downloading blobs, and performing related operations in my applications.
---

# Quickstart: Azure Blob Storage client library for C++

Get started with the Azure Blob Storage client library for C++. Azure Blob Storage is Microsoft's object storage solution for the cloud. Follow these steps to install the package and try out example code for basic tasks.

| [API reference documentation](https://azure.github.io/azure-sdk-for-cpp/storage.html) | [Library source code](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage) | [Samples](../common/storage-samples-c-plus-plus.md?toc=/azure/storage/blobs/toc.json) |

## Prerequisites

- Azure subscription - [create one for free](https://azure.microsoft.com/free/)
- Azure storage account - [create a storage account](../common/storage-account-create.md)
- [C++ compiler](https://azure.github.io/azure-sdk/cpp_implementation.html#supported-platforms)
- [CMake](https://cmake.org/)
- [vcpkg - C and C++ package manager](https://vcpkg.io/en/getting-started.html)

## Setting up

This section walks you through preparing a project to work with the Azure Blob Storage client library for C++. The easiest way to acquire the Azure SDK for C++ is to use the `vcpkg` package manager.

### Install the packages

Use the `vcpkg install` command to install the Azure Blob Storage library for C++ and necessary dependencies:

```console
vcpkg.exe install azure-storage-blobs-cpp
```

The Azure Identity library is needed for passwordless connections to Azure services:

```console
vcpkg.exe install azure-identity-cpp
```

For more information on project setup and working with the Azure SDK for C++, see the [Azure SDK for C++ readme](https://github.com/Azure/azure-sdk-for-cpp#azure-sdk-for-c).

### Create the project

In Visual Studio, create a new C++ console application for Windows called *BlobQuickstart*.

:::image type="content" source="./media/quickstart-blobs-c-plus-plus/vs-create-project.jpg" alt-text="Visual Studio dialog for configuring a new C++ Windows console app":::

## Object model

Azure Blob Storage is optimized for storing massive amounts of unstructured data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data. Blob Storage offers three types of resources:

- The storage account
- A container in the storage account
- A blob in the container

The following diagram shows the relationship between these resources.

![Diagram of Blob Storage architecture](./media/storage-blobs-introduction/blob1.png)

Use these C++ classes to interact with these resources:

- [BlobServiceClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html): The `BlobServiceClient` class allows you to manipulate Azure Storage resources and blob containers.
- [BlobContainerClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html): The `BlobContainerClient` class allows you to manipulate Azure Storage containers and their blobs.
- [BlobClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html): The `BlobClient` class allows you to manipulate Azure Storage blobs. It's the base class for all specialized blob classes.
- [BlockBlobClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html): The `BlockBlobClient` class allows you to manipulate Azure Storage block blobs.

## Code examples

These example code snippets show you how to do the following tasks with the Azure Blob Storage client library for C++:

- [Add include files](#add-include-files)
- [Authenticate to Azure and authorize access to blob data](#authenticate-to-azure-and-authorize-access-to-blob-data)
- [Create a container](#create-a-container)
- [Upload blobs to a container](#upload-blobs-to-a-container)
- [List the blobs in a container](#list-the-blobs-in-a-container)
- [Download blobs](#download-blobs)
- [Delete a container](#delete-a-container)

### Add include files

From the project directory:

1. Open the *BlobQuickstart.sln* solution file in Visual Studio
1. Inside Visual Studio, open the *BlobQuickstart.cpp* source file
1. Remove any code inside `main` that was autogenerated
1. Add `#include` and `using namespace` statements

```cpp
#include <iostream>
#include <azure/core.hpp>
#include <azure/identity/default_azure_credential.hpp>
#include <azure/storage/blobs.hpp>

using namespace Azure::Identity;
using namespace Azure::Storage::Blobs;
```

### Authenticate to Azure and authorize access to blob data

Application requests to Azure Blob Storage must be authorized. Using the `DefaultAzureCredential` class provided by the Azure Identity client library  is the recommended approach for implementing passwordless connections to Azure services in your code, including Blob Storage.

You can also authorize requests to Azure Blob Storage by using the account access key. However, this approach should be used with caution. Developers must be diligent to never expose the access key in an unsecure location. Anyone who has the access key is able to authorize requests against the storage account, and effectively has access to all the data. `DefaultAzureCredential` offers improved management and security benefits over the account key to allow passwordless authentication. Both options are demonstrated in the following example.

### [Passwordless (Recommended)](#tab/managed-identity)

The Azure Identity library provides Microsoft Entra token authentication support across the Azure SDK. It provides a set of `TokenCredential` implementations which can be used to construct Azure SDK clients which support Microsoft Entra token authentication. `DefaultAzureCredential` supports multiple authentication methods and determines which method should be used at runtime.

<a name='assign-roles-to-your-azure-ad-user-account'></a>

#### Assign roles to your Microsoft Entra user account

[!INCLUDE [assign-roles](../../../includes/assign-roles.md)]

#### Sign in and connect your app code to Azure using DefaultAzureCredential

You can authorize access to data in your storage account using the following steps:

1. Make sure you're authenticated with the same Microsoft Entra account you assigned the role to on your storage account. You can authenticate via [Azure CLI](/cli/azure/install-azure-cli). Sign in to Azure through the Azure CLI using the following command:

    ```azurecli
    az login
    ```

2. To use `DefaultAzureCredential`, make sure that the **azure-identity-cpp** package is [installed](#install-the-packages) and the following `#include` is added:

    ```cpp
    #include <azure/identity/default_azure_credential.hpp>
    ```

3. Add this code to the end of `main()`. When the code runs on your local workstation, `DefaultAzureCredential` uses the developer credentials for Azure CLI to authenticate to Azure.

    ```cpp
    // Initialize an instance of DefaultAzureCredential
	auto defaultAzureCredential = std::make_shared<DefaultAzureCredential>();

	auto accountURL = "https://<storage-account-name>.blob.core.windows.net";
	BlobServiceClient blobServiceClient(accountURL, defaultAzureCredential);
    ```

4. Make sure to update the storage account name in the URI of your `BlobServiceClient` object. The storage account name can be found on the overview page of the Azure portal.

    :::image type="content" source="./media/storage-quickstart-blobs-dotnet/storage-account-name.png" alt-text="A screenshot showing how to find the storage account name.":::

    > [!NOTE]
    > When using the C++ SDK in a production environment, it's recommended that you only enable credentials that you know your application will use. Instead of using `DefaultAzureCredential`, you should authorize using a specific credential type, or by using `ChainedTokenCredential` with the supported credentials.

### [Connection String](#tab/connection-string)

A connection string includes the storage account access key and uses it to authorize requests. Always be careful to never expose the keys in an unsecure location.

> [!NOTE]
> To authorize data access with the storage account access key, you'll need permissions for the following Azure RBAC action: [Microsoft.Storage/storageAccounts/listkeys/action](../../role-based-access-control/resource-provider-operations.md#microsoftstorage). The least privileged built-in role with permissions for this action is [Reader and Data Access](../../role-based-access-control/built-in-roles.md#reader-and-data-access), but any role which includes this action will work.

[!INCLUDE [retrieve credentials](../../../includes/retrieve-credentials.md)]

#### Configure your storage connection string

After you copy the connection string, write it to a new environment variable on the local machine running the application. To set the environment variable, open a console window, and follow the instructions for your operating system. Replace `<yourconnectionstring>` with your actual connection string.

**Windows**:

```cmd
setx AZURE_STORAGE_CONNECTION_STRING "<yourconnectionstring>"
```

After you add the environment variable in Windows, you must start a new instance of the command window.

**Linux**:

```bash
export AZURE_STORAGE_CONNECTION_STRING="<yourconnectionstring>"
```

The following code example retrieves the connection string for the storage account from the environment variable created earlier, and uses the connection string to construct a service client object.

Add this code to the end of `main()`:

```cpp
    // Retrieve the connection string for use with the application. The storage
    // connection string is stored in an environment variable on the machine
    // running the application called AZURE_STORAGE_CONNECTION_STRING.
    // Note that _MSC_VER is set when using MSVC compiler.
    static const char* AZURE_STORAGE_CONNECTION_STRING = "AZURE_STORAGE_CONNECTION_STRING";

#if !defined(_MSC_VER)
    const char* connectionString = std::getenv(AZURE_STORAGE_CONNECTION_STRING);
#else
	// Use getenv_s for MSVC
	size_t requiredSize;
	getenv_s(&requiredSize, NULL, NULL, AZURE_STORAGE_CONNECTION_STRING);
	if (requiredSize == 0) {
		throw std::runtime_error("missing connection string from env.");
	}
	std::vector<char> value(requiredSize);
	getenv_s(&requiredSize, value.data(), value.size(), AZURE_STORAGE_CONNECTION_STRING);
	std::string connectionStringStr = std::string(value.begin(), value.end());
	const char* connectionString = connectionStringStr.c_str();
#endif

	auto blobServiceClient = BlobServiceClient::CreateFromConnectionString(connectionString);
```

> [!IMPORTANT]
> The account access key should be used with caution. If your account access key is lost or accidentally placed in an insecure location, your service may become vulnerable. Anyone who has the access key is able to authorize requests against the storage account, and effectively has access to all the data. `DefaultAzureCredential` provides enhanced security features and benefits and is the recommended approach for managing authorization to Azure services.

---

### Create a container

Decide on a name for the new container. Then create an instance of `BlobContainerClient` and create the container.

> [!IMPORTANT]
> Container names must be lowercase. For more information about naming containers and blobs, see [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Add this code to the end of `main()`:

```cpp
std::string containerName = "myblobcontainer";
auto containerClient = blobServiceClient.GetBlobContainerClient("myblobcontainer");

// Create the container if it does not exist
std::cout << "Creating container: " << containerName << std::endl;
containerClient.CreateIfNotExists();
```

### Upload blobs to a container

The following code snippet:

1. Declares a string containing "Hello Azure!"
2. Gets a reference to a [BlockBlobClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) object by calling [GetBlockBlobClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) on the container from the [Create a container](#create-a-container) section.
3. Uploads the string to the blob by calling the [​Upload​From](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) function. This function creates the blob if it doesn't already exist, or updates it if it does.

Add this code to the end of `main()`:

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_UploadBlob":::

### List the blobs in a container

List the blobs in the container by calling the [ListBlobs](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) function. Only one blob has been added to the container, so the operation returns just that blob.

Add this code to the end of `main()`:

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_ListBlobs":::

### Download blobs

Get the properties of the uploaded blob. Then, declare and resize a new `std::vector<uint8_t>` object by using the properties of the uploaded blob. Download the previously created blob into the new `std::vector<uint8_t>` object by calling the [​DownloadTo](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) function in the [BlobClient](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) base class. Finally, display the downloaded blob data.

Add this code to the end of `main()`:

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DownloadBlob":::

### Delete a Blob

The following code deletes the blob from the Azure Blob Storage container by calling the [BlobClient.Delete](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html) function.

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DeleteBlob":::

### Delete a container

The following code cleans up the resources the app created by deleting the entire container by using [BlobContainerClient.​Delete](https://azuresdkdocs.z19.web.core.windows.net/cpp/azure-storage-blobs/12.13.0/classes.html).

Add this code to the end of `main()`:

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DeleteContainer":::

## Run the code

This app creates a container and uploads a text file to Azure Blob Storage. The example then lists the blobs in the container, downloads the file, and displays the file contents. Finally, the app deletes the blob and the container.

The output of the app is similar to the following example:

```output
Azure Blob Storage - C++ quickstart sample
Creating container: myblobcontainer
Uploading blob: blob.txt
Listing blobs...
Blob name: blob.txt
Downloaded blob contents: Hello Azure!
Deleting blob: blob.txt
Deleting container: myblobcontainer
```

## Next steps

In this quickstart, you learned how to upload, download, and list blobs using C++. You also learned how to create and delete an Azure Blob Storage container.

To see a C++ Blob Storage sample, continue to:

> [!div class="nextstepaction"]
> [Azure Blob Storage client library for C++ samples](https://github.com/Azure/azure-sdk-for-cpp/tree/main/sdk/storage/azure-storage-blobs/samples)
