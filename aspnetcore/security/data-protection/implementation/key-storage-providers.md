---
title: ASP.NET Core でのキー記憶域プロバイダー
author: rick-anderson
description: キー記憶域プロバイダーでは、ASP.NET Core とキー記憶域の場所を構成する方法について説明します。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e712ff09b5306bc4481c4cc105448d7cbfa39f3a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356767"
---
# <a name="key-storage-providers-in-aspnet-core"></a>ASP.NET Core でのキー記憶域プロバイダー

データ保護システム[検出メカニズムを使用して、既定で](xref:security/data-protection/configuration/default-settings)暗号化キーを保持するかを判断します。 開発者は、既定の検出メカニズムをオーバーライドし、場所を手動で指定できます。

> [!WARNING]
> キーの永続性の明示的な場所を指定する場合データ保護システムの登録を解除 rest のメカニズムで既定のキーの暗号化キーは保存時暗号化されなくようにします。 お勧めするさらに[明示的なキーの暗号化メカニズムを指定](xref:security/data-protection/implementation/key-encryption-at-rest)運用環境のデプロイ。

## <a name="file-system"></a>ファイル システム

ファイル システム ベースのキー リポジトリを構成するには、呼び出し、 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)次に示すように構成ルーチン。 提供、 [DirectoryInfo](/dotnet/api/system.io.directoryinfo)キーを格納するリポジトリをポイントします。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo`ローカル コンピューター上のディレクトリを指すことができます、またはネットワーク共有上のフォルダーを指すことができます。 ローカル コンピューター上のディレクトリを指している場合 (および、シナリオでは、ローカル コンピューター上のアプリのみが使用して、このリポジトリへのアクセスを必要と)、使用を検討して[Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest)保存時のキーを暗号化する (on Windows)。 それ以外の場合、使用を検討して、 [X.509 証明書](xref:security/data-protection/implementation/key-encryption-at-rest)保存時のキーを暗号化します。

## <a name="azure-and-redis"></a>Azure と Redis

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)と[Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) Azure Storage または Redis cache でデータ保護キーを格納するパッケージを使用します。 Web アプリの複数のインスタンスでは、キーを共有することができます。 アプリでは、複数のサーバー認証 cookie または CSRF protection を共有できます。 で、Azure Blob 記憶域プロバイダーを構成するには、のいずれかを呼び出して、 [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)オーバー ロードします。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

で Redis を構成するには、のいずれかを呼び出して、 [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis)オーバー ロードします。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

詳細については、次のトピックを参照してください。

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [aspnet/DataProtection サンプル](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>レジストリ

**Windows の展開にのみ適用されます。**

場合によって、アプリでは、ファイル システムへの書き込みアクセスがあります。 アプリが仮想サービス アカウントとして実行されているシナリオについて考えてみます (など*w3wp.exe*のアプリケーション プール id)。 このような場合は、管理者は、サービス アカウント id でアクセスできるレジストリ キーをプロビジョニングできます。 呼び出す、 [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry)次に示すように、拡張メソッド。 提供、 [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey)暗号化キーを格納する場所を指しています。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> 使用することをお勧めします。 [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest)保存時のキーを暗号化します。

## <a name="custom-key-repository"></a>カスタム キー リポジトリ

組み込みのメカニズムがない適切な場合、開発者がカスタムを提供することで独自のキーの永続化メカニズムを指定できます[IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)します。
