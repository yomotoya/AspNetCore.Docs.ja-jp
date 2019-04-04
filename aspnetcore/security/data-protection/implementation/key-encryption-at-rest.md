---
title: ASP.NET Core での保存時のキーの暗号化
author: rick-anderson
description: ASP.NET Core データ保護キーの保存時の暗号化の実装の詳細について説明します。
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219291"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>ASP.NET Core での保存時のキーの暗号化

データ保護システム[検出メカニズムを使用して、既定で](xref:security/data-protection/configuration/default-settings)キーを暗号化する方法を決定する残りの部分で暗号化する必要があります。 開発者は、検出メカニズムをオーバーライドし、保存時のキーの暗号化方法を手動で指定できます。

> [!WARNING]
> 明示的な指定した場合[キーの永続化される場所](xref:security/data-protection/implementation/key-storage-providers)、データ保護システム登録メカニズムの残りの部分で既定のキーの暗号化を解除します。 その結果、キーは、残りの部分では暗号化されません。 お勧めする[明示的なキーの暗号化メカニズムを指定](xref:security/data-protection/implementation/key-encryption-at-rest)運用環境のデプロイ。 保存時の暗号化メカニズムのオプションは、このトピックで説明します。

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

キーを保管する[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)、構成を使用してシステム[ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault)で、`Startup`クラス。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

詳細については、[ASP.NET Core データ保護の構成: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault)を参照してください。

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**Windows の展開にのみ適用されます。**

キー マテリアルがで暗号化された Windows DPAPI を使用すると、 [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata)ストレージに永続化される前にします。 DPAPI は、現在のコンピューターの外部ではない読み取り専用データの適切な暗号化メカニズム (が Active Directory までこれらのキーをバックアップすることは、参照してください[DPAPI および移動ユーザー プロファイル](https://support.microsoft.com/kb/309408/#6))。 DPAPI の保存時のキーの暗号化を構成するには、いずれかを呼び出して、 [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi)拡張メソッド。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

場合`ProtectKeysWithDpapi`は、現在の Windows ユーザー アカウントは、永続化されたキー リングを解読できますのみパラメーターなしで呼び出されます。 必要に応じて、(現在のユーザー アカウントだけでなく) コンピューター上の任意のユーザー アカウントが、キー リングを解読できることを指定することができます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>X.509 証明書

アプリは複数のマシンに分散され場合、マシン間で共有の X.509 証明書を配布して、保存時のキーの暗号化に証明書を使用するホスト型アプリを構成する便利な場合があります。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

.NET Framework の制限により、CAPI 秘密キーを含む証明書のみがサポートされています。 これらの制限に対処する方法については、次の内容を参照してください。

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows の DPAPI NG

**このメカニズムは、Windows 8/Windows Server 2012 以降でのみ使用できます。**

Windows 8 以降、Windows OS には、DPAPI NG (CNG DPAPI とも呼ばれます) がサポートしています。 詳細については、[CNG DPAPI について](/windows/desktop/SecCNG/cng-dpapi)を参照してください。

プリンシパルは、保護の記述子ルールとしてエンコードされます。 呼び出す次の例では[ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)、のみ、指定された SID を持つドメインに参加しているユーザーには、キー リングが復号化できます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

パラメーターなしのオーバー ロードも`ProtectKeysWithDpapiNG`します。 この便利なメソッドを使用して、ルールを指定する"SID = {CURRENT_ACCOUNT_SID}"ここで、 *CURRENT_ACCOUNT_SID*は現在の Windows ユーザー アカウントの SID。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

このシナリオでは、AD ドメイン コント ローラーは、DPAPI NG 操作によって使用される暗号化キーを配布する責任を負います。 ターゲット ユーザーは、(プロセスは、その id の下で実行している) を指定した任意のドメインに参加しているコンピューターから暗号化されたペイロードを解読できます。

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>証明書ベースの暗号化では、Windows DPAPI-NG

Windows 8.1/Windows Server 2012 R2 で、アプリが実行されている場合は後で、NG-Windows DPAPI を使用して証明書ベースの暗号化を実行することができます。 ルールの記述子の文字列を使用して"証明書 HashId:THUMBPRINT ="ここで、*拇印*証明書の 16 進エンコードされた SHA1 拇印します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Windows 8.1/windows Server 2012 R2、または、キーを解読するには、後で、このリポジトリで参照されているすべてのアプリを実行する必要があります。

## <a name="custom-key-encryption"></a>カスタム キーの暗号化

組み込みのメカニズムがない適切な場合、開発者がカスタムを提供することで独自のキーの暗号化メカニズムを指定できます[IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor)します。
