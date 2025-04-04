---
title: Azure Container Apps dynamic sessions overview
description: Learn how when to use dynamic sessions in Azure Container Apps.
services: container-apps
author: craigshoemaker
ms.service: azure-container-apps
ms.topic: conceptual
ms.date: 11/04/2024
ms.author: cshoe
ms.custom: references_regions, ignite-2024
---

# Azure Container Apps dynamic sessions overview

Azure Container Apps dynamic sessions provide fast access to secure sandboxed environments that are ideal for running code or applications that require strong isolation from other workloads.

Sessions have the following attributes:

* **Strong isolation**: Sessions are isolated from each other and from the host environment. Each session runs in its own Hyper-V sandbox, providing enterprise-grade security and isolation. Optionally, you can enable network isolation to further enhance security.

* **Simple access**: Sessions are accessed through a REST API. A unique identifier marks each session. If a session with a given identifier doesn't exist, a new session is automatically allocated.

* **Fully managed**: The platform fully manages a session's lifecycle. Sessions are automatically cleaned up when no longer in use.

* **Fast startup**: A new session is allocated in milliseconds. Rapid start-ups are achieved by automatically maintaining a pool of ready but unallocated sessions.

* **Scalable**: Sessions can run at a high scale. You can run hundreds or thousands of sessions concurrently.

## Session types

Azure Container Apps supports two types of sessions:

| Type | Description | Billing model |
|------|-------------|---------------|
| **Code interpreter** | Fully managed code interpreter | Per session (consumption) |
| **Custom container** | Bring your own container | Container Apps Dedicated Plan |

### Code interpreter

Code interpreter sessions allow you to run code in a sandbox that is preinstalled with popular libraries. They're ideal for running untrusted code, such as code provided by users of your application or code generated by a large language model (LLM). Learn more about [code interpreter sessions](./sessions-code-interpreter.md).

### Custom container

Custom container sessions allow you to run your own container images in secure, isolated sandboxes. You can use them to run a custom code interpreter for a language that isn't supported out of the box, or to run workloads that require strong isolation. Learn more about [custom container sessions](./sessions-custom-container.md).

## Concepts

The key concepts in Azure Container Apps dynamic sessions are session pools and sessions.

### Session pools

To provide sub-second session allocation times, Azure Container Apps maintains a pool of ready but unallocated sessions. When you submit a request to a new session, the platform allocates a session from the pool to you. As sessions are allocated, the platform automatically replenishes the pool to maintain a constant number of ready sessions.

You can configure pools to set the maximum number of sessions that can be allocated concurrently via the  `maxConcurrentSessions` property. You can set the wait duration from the last request before a session is deleted the `cooldownPeriodInSeconds` property. For custom container sessions, you can also specify the container image and settings to use for the sessions in the pool, including the target number of sessions to keep ready in the pool via `readySessionInstances`.

### Sessions

A session is a sandboxed environment that runs your code or application. Each session is isolated from other sessions and from the host environment with a [Hyper-V](/windows-server/virtualization/hyper-v/hyper-v-technology-overview) sandbox. Optionally, you can enable network isolation to further enhance security.

You interact with sessions in a session pool by sending HTTP requests. Each session pool has a unique pool management endpoint.

For code interpreter sessions, you can also use an integration with an [LLM framework](./sessions-code-interpreter.md#llm-framework-integrations).

### Session identifiers

To send an HTTP request to a session, you must provide a session identifier in the request. You pass the session identifier in a query parameter named `identifier` in the URL when you make a request to a session.

* If a session with the identifier already exists, the request is sent to the existing session.
* If a session with the identifier doesn't exist, a new session is automatically allocated before the request is sent to it.

:::image type="content" source="media/sessions/sessions-overview.png" alt-text="Screenshot of session pool and sessions usage.":::

#### Identifier format

The session identifier is a free-form string, meaning you can define it in any way that suits your application's needs.

The session identifier is a string that you define that is unique within the session pool. If you're building a web application, you can use the user's ID as the session identifier. If you're building a chatbot, you can use the conversation ID.

The identifier must be a string that is 4 to 128 characters long and can contain only alphanumeric characters and special characters from this list: `|`, `-`, `&`, `^`, `%`, `$`, `#`, `(`, `)`, `{`, `}`, `[`, `]`, `;`, `<`, and `>`.

#### Protecting session identifiers

The session identifier is sensitive information which you must manage securely. Your application needs to ensure each user or tenant only has access to their own sessions.

The specific strategies that prevent misuse of session identifiers differ depending on the design and architecture of your app. However, your app must always have complete control over the creation and use of session identifiers so that a malicious user can't access another user's session.

Example strategies include:

* **One session per user**: If your app uses one session per user, each user must be securely authenticated, and your app must use a unique session identifier for each logged in user.
* **One session per agent conversation**: If your app uses one session per AI agent conversation, ensure your app uses a unique session identifier for each conversation that can't be modified by the end user.

> [!IMPORTANT]
> Failure to secure access to sessions may result in misuse or unauthorized access to data stored in your users' sessions.

### <a name="authentication"></a>Authentication and authorization

When you send requests to a session using the pool management API, authentication is handled using Microsoft Entra (formerly Azure Active Directory) tokens. Only Microsoft Entra tokens from an identity belonging to the *Azure ContainerApps Session Executor* role on the session pool are authorized to call the pool management API.

To assign the role to an identity, use the following Azure CLI command:

```bash
az role assignment create \
    --role "Azure ContainerApps Session Executor" \
    --assignee <PRINCIPAL_ID> \
    --scope <SESSION_POOL_RESOURCE_ID>
```

If you're using an [LLM framework integration](sessions-code-interpreter.md#llm-framework-integrations), the framework handles the token generation and management for you. Ensure that the application is configured with a managed identity with the necessary role assignments on the session pool.

If you're using the pool's management API endpoints directly, you must generate a token and include it in the `Authorization` header of your HTTP requests. In addition to the role assignments previously mentioned, token needs to contain an audience (`aud`) claim with the value `https://dynamicsessions.io`.

##### [Azure CLI](#tab/azure-cli)

To generate a token using the Azure CLI, run the following command:

```bash
az account get-access-token --resource https://dynamicsessions.io
```

##### [C#](#tab/csharp)

In C#, you can use the `Azure.Identity` library to generate a token. First, install the library:

```bash
dotnet add package Azure.Identity
```

Then, use the following code to generate a token:

```csharp
using Azure.Identity;

var credential = new DefaultAzureCredential();
var token = credential.GetToken(new TokenRequestContext(new[] { "https://dynamicsessions.io/.default" }));
var accessToken = token.Token;
```

##### [JavaScript](#tab/javascript)

In JavaScript, you can use the `@azure/identity` library to generate a token. First, install the library:

```bash
npm install @azure/identity
```

Then, use the following code to generate a token:

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { TokenCredential } = require("@azure/core-auth");

const credential = new DefaultAzureCredential();
const token = await credential.getToken("https://dynamicsessions.io/.default");
const accessToken = token.token;
```

##### [Python](#tab/python)

In Python, you can use the `azure-identity` library to generate a token. First, install the library:

```bash
pip install azure-identity
```

Then, use the following code to generate a token:

```python
from azure.identity import DefaultAzureCredential

credential = DefaultAzureCredential()
token = credential.get_token("https://dynamicsessions.io/.default")
access_token = token.token
```

---

> [!IMPORTANT]
> A valid token can be used to create and access any session in the pool. Keep your tokens secure and don't share them with untrusted parties. End users should access sessions through your application, not directly. They should never have access to the tokens used to authenticate requests to the session pool.

#### Lifecycle

The Container Apps runtime automatically manages the lifecycle for each session in a session pool.

* **Pending**: When a session is starting up, it's in the pending state. The amount of time a session spends in the pending state depends on the container image and settings you specify for the session pool. A pending session isn't added to the pool of ready sessions.

* **Ready**: When a session is done starting up and is ready, it's added to the pool. The session is available at this state for allocation. For custom container sessions, you can specify the target number of ready sessions to keep in the pool. Increase this number if sessions are allocated faster than the pool is being replenished.

* **Allocated**: When you send a request to a non-running session, the pool provides a new session and placed it in an allocated state. Subsequent requests with the same session identifier are routed to the same session.

* **Deleting**: When a session stop receiving requests during the time defined by the `cooldownPeriodInSeconds` setting, the session and its Hyper-V sandbox are completely and securely deleted.

## Security

Azure Container Apps dynamic sessions are built to run untrusted code and applications in a secure and isolated environment. While sessions are isolated from one another, anything within a single session, including files and environment variables, is accessible by users of the session. You should only configure or upload sensitive data to a session if you trust the users of the session.

By default, sessions are prevented from making outbound network requests. You can control network access by configuring network status settings on the session pool.

In addition, follow the guidance in the [authentication and authorization](#authentication) section to ensure that only authorized users can access sessions and in the [protecting session identifiers](#protecting-session-identifiers) section to ensure that session identifiers are secure.

## Region availability

Dynamic sessions are available in the following regions:

| Region | Code interpreter | Custom container |
|--------|------------------|------------------|
| Australia East | ✔ | ✔ |
| Central US EUAP | ✔ | ✔ |
| East US 2 EUAP | ✔ | ✔ |
| East US | ✔ | ✔ |
| East Asia | ✔ | ✔ |
| Germany West Central | ✔ | ✔ |
| Italy North | ✔ | ✔ |
| North Central US | ✔ | - |
| Poland Central | ✔ | ✔ |
| Switzerland North | ✔ | ✔ |
| West Central US | ✔ | ✔ |
| West US 2 | ✔ | ✔ |

## Related content

* **Session types**: Learn about the different types of dynamic sessions:
  * [Code interpreter sessions](./sessions-code-interpreter.md)
  * [Custom container sessions](./sessions-custom-container.md)

* **Tutorials**: Work directly with the REST API or via a LLM agent:
  * Use a LLM agent:
    * [AutoGen](./sessions-tutorial-autogen.md)
    * [LangChain](./sessions-tutorial-langchain.md)
    * [LlamaIndex](./sessions-tutorial-llamaindex.md)
    * [Semantic Kernel](./sessions-tutorial-semantic-kernel.md)
  * Use the REST API
    * [JavaScript Code interpreter](./sessions-tutorial-nodejs.md)
