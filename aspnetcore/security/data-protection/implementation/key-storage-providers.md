---
title: "キー記憶域プロバイダー"
author: rick-anderson
description: "キー記憶域プロバイダー"
keywords: "encryption,ASP.NET コア"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 952fc99d2486b8e78d97356c333257e581a79b42
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="fcc57-104">キー記憶域プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fcc57-104">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="fcc57-105">既定では、データ保護システム[ヒューリスティックを使用して](../configuration/default-settings.md#data-protection-default-settings)を暗号化キー マテリアルを保持するかを判断します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-105">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="fcc57-106">開発者は、ヒューリスティックをオーバーライドし、手動で、場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="fcc57-107">キーの永続化を明示的な場所を指定する場合、データ保護システムはの登録を解除ヒューリスティックが提供する rest メカニズムで既定のキーの暗号化キーは、残りの部分では暗号化されなくなりますようにします。</span><span class="sxs-lookup"><span data-stu-id="fcc57-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="fcc57-108">お勧めするさらに[明示的なキーの暗号化メカニズムが指定](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)実可動アプリケーション用。</span><span class="sxs-lookup"><span data-stu-id="fcc57-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="fcc57-109">データ保護システムは、いくつかのボックス内のキー記憶域プロバイダーが付属します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="fcc57-110">ファイル システム</span><span class="sxs-lookup"><span data-stu-id="fcc57-110">File system</span></span>

<span data-ttu-id="fcc57-111">多くのアプリがファイル システムに基づくキー リポジトリを使用すると予測されます。</span><span class="sxs-lookup"><span data-stu-id="fcc57-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="fcc57-112">これを構成するのには、呼び出し、 [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)次に示すように構成ルーチンです。</span><span class="sxs-lookup"><span data-stu-id="fcc57-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="fcc57-113">提供、`DirectoryInfo`キーを格納するリポジトリをポイントします。</span><span class="sxs-lookup"><span data-stu-id="fcc57-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="fcc57-114">`DirectoryInfo`ローカル コンピューター上のディレクトリを指すことができます、またはネットワーク共有上のフォルダーを指すことができます。</span><span class="sxs-lookup"><span data-stu-id="fcc57-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="fcc57-115">ローカル コンピューター上のディレクトリを指している場合 (およびシナリオは、ローカル コンピューター上のアプリケーションのみ このリポジトリを使用する必要があります)、使用を検討して[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="fcc57-116">それ以外の場合の使用を検討、 [X.509 証明書](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="fcc57-117">Azure と Redis</span><span class="sxs-lookup"><span data-stu-id="fcc57-117">Azure and Redis</span></span>

<span data-ttu-id="fcc57-118">`Microsoft.AspNetCore.DataProtection.AzureStorage`と`Microsoft.AspNetCore.DataProtection.Redis`パッケージは、Azure Storage または Redis cache で、データ保護キーの保存を許可します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="fcc57-119">キーは、web アプリの複数のインスタンスで共有することができます。</span><span class="sxs-lookup"><span data-stu-id="fcc57-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="fcc57-120">ASP.NET Core アプリでは、複数のサーバー認証 cookie または CSRF protection を共有できます。</span><span class="sxs-lookup"><span data-stu-id="fcc57-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="fcc57-121">Azure の構成する場合のいずれかを呼び出して、 [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs)次に示すようにオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="fcc57-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="fcc57-122">参照、 [Azure テスト コード](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs)です。</span><span class="sxs-lookup"><span data-stu-id="fcc57-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="fcc57-123">Redis の構成する場合にのいずれかを呼び出して、 [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs)次に示すようにオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="fcc57-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="fcc57-124">詳しくは、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fcc57-124">See the following for more information:</span></span>

- [<span data-ttu-id="fcc57-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="fcc57-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="fcc57-126">Azure Redis キャッシュ</span><span class="sxs-lookup"><span data-stu-id="fcc57-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="fcc57-127">[テスト コードを redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs)です。</span><span class="sxs-lookup"><span data-stu-id="fcc57-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="fcc57-128">レジストリ</span><span class="sxs-lookup"><span data-stu-id="fcc57-128">Registry</span></span>

<span data-ttu-id="fcc57-129">場合によって、アプリでは、ファイル システムへの書き込みアクセスがないこと。</span><span class="sxs-lookup"><span data-stu-id="fcc57-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="fcc57-130">(W3wp.exe のアプリケーション プール id) などの仮想サービス アカウントとして、アプリが実行されているシナリオを検討してください。</span><span class="sxs-lookup"><span data-stu-id="fcc57-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="fcc57-131">このような場合は、管理者をレジストリ キーは、サービス アカウントの資格情報の適切な ACLed をプロビジョニングしている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcc57-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="fcc57-132">呼び出す、 [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)次に示すように構成ルーチンです。</span><span class="sxs-lookup"><span data-stu-id="fcc57-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="fcc57-133">提供、`RegistryKey`キー/値の暗号化を格納する場所を指しています。</span><span class="sxs-lookup"><span data-stu-id="fcc57-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="fcc57-134">永続化メカニズムとして、システム レジストリを使用する場合は、使用を検討して[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="fcc57-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="fcc57-135">カスタム キー リポジトリ</span><span class="sxs-lookup"><span data-stu-id="fcc57-135">Custom key repository</span></span>

<span data-ttu-id="fcc57-136">カスタムを提供することによって、開発者が独自のキーの永続化メカニズムを指定するインボックス メカニズムが適切でない場合`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="fcc57-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
