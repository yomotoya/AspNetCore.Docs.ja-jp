---
title: "キー記憶域プロバイダー"
author: rick-anderson
description: "キー記憶域プロバイダー"
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 4f0e29a94593cd1cbb9890d7ee8bd09cddb4f69c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-providers"></a><span data-ttu-id="8b8f2-103">キー記憶域プロバイダー</span><span class="sxs-lookup"><span data-stu-id="8b8f2-103">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="8b8f2-104">既定では、データ保護システム[ヒューリスティックを使用して](xref:security/data-protection/configuration/default-settings)を暗号化キー マテリアルを保持するかを判断します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="8b8f2-105">開発者は、ヒューリスティックをオーバーライドし、手動で、場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="8b8f2-106">キーの永続化を明示的な場所を指定する場合、データ保護システムはの登録を解除ヒューリスティックが提供する rest メカニズムで既定のキーの暗号化キーは、残りの部分では暗号化されなくなりますようにします。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="8b8f2-107">お勧めするをさらに[明示的なキーの暗号化メカニズムが指定](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)実可動アプリケーション用。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-107">It's recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="8b8f2-108">データ保護システムは、いくつかのボックス内のキー記憶域プロバイダーが付属します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="8b8f2-109">ファイル システム</span><span class="sxs-lookup"><span data-stu-id="8b8f2-109">File system</span></span>

<span data-ttu-id="8b8f2-110">多くのアプリがファイル システムに基づくキー リポジトリを使用すると予測されます。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="8b8f2-111">これを構成するのには、呼び出し、 [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)次に示すように構成ルーチンです。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="8b8f2-112">提供、`DirectoryInfo`キーを格納するリポジトリをポイントします。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="8b8f2-113">`DirectoryInfo`ローカル コンピューター上のディレクトリを指すことができます、またはネットワーク共有上のフォルダーを指すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="8b8f2-114">ローカル コンピューター上のディレクトリを指している場合 (およびシナリオは、ローカル コンピューター上のアプリケーションのみ このリポジトリを使用する必要があります)、使用を検討して[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="8b8f2-115">それ以外の場合の使用を検討、 [X.509 証明書](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-115">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="8b8f2-116">Azure と Redis</span><span class="sxs-lookup"><span data-stu-id="8b8f2-116">Azure and Redis</span></span>

<span data-ttu-id="8b8f2-117">`Microsoft.AspNetCore.DataProtection.AzureStorage`と`Microsoft.AspNetCore.DataProtection.Redis`パッケージは、Azure Storage または Redis cache で、データ保護キーの保存を許可します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="8b8f2-118">キーは、web アプリの複数のインスタンスで共有することができます。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="8b8f2-119">ASP.NET Core アプリでは、複数のサーバー認証 cookie または CSRF protection を共有できます。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="8b8f2-120">Azure の構成する場合のいずれかを呼び出して、 [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs)次に示すようにオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="8b8f2-121">参照、 [Azure テスト コード](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs)です。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="8b8f2-122">Redis の構成する場合にのいずれかを呼び出して、 [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs)次に示すようにオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="8b8f2-123">詳しくは、次のトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-123">See the following for more information:</span></span>

- [<span data-ttu-id="8b8f2-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="8b8f2-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="8b8f2-125">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8b8f2-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="8b8f2-126">[テスト コードを redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs)です。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="8b8f2-127">レジストリ</span><span class="sxs-lookup"><span data-stu-id="8b8f2-127">Registry</span></span>

<span data-ttu-id="8b8f2-128">場合によって、アプリでは、ファイル システムへの書き込みアクセスがないこと。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="8b8f2-129">(W3wp.exe のアプリケーション プール id) などの仮想サービス アカウントとして、アプリが実行されているシナリオを検討してください。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="8b8f2-130">このような場合は、管理者をレジストリ キーは、サービス アカウントの資格情報の適切な ACLed をプロビジョニングしている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="8b8f2-131">呼び出す、 [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)次に示すように構成ルーチンです。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="8b8f2-132">提供、`RegistryKey`キー/値の暗号化を格納する場所を指しています。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="8b8f2-133">永続化メカニズムとして、システム レジストリを使用する場合は、使用を検討して[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)静止したキーを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="8b8f2-134">カスタム キー リポジトリ</span><span class="sxs-lookup"><span data-stu-id="8b8f2-134">Custom key repository</span></span>

<span data-ttu-id="8b8f2-135">カスタムを提供することによって、開発者が独自のキーの永続化メカニズムを指定するインボックス メカニズムが適切でない場合`IXmlRepository`です。</span><span class="sxs-lookup"><span data-stu-id="8b8f2-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
