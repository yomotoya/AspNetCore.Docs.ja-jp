---
title: "キーの保存時の暗号化"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護キーの暗号化の実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 0a62a1a10e578e59e1d80579d80779d4dcf1658a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="key-encryption-at-rest"></a>キーの保存時の暗号化

<a name="data-protection-implementation-key-encryption-at-rest"></a>

既定では、データ保護システム[ヒューリスティックを使用して](xref:security/data-protection/configuration/default-settings)キー マテリアルを暗号化する方法を決定する残りの部分で暗号化する必要があります。 開発者は、ヒューリスティックをオーバーライドし、残りの部分でのキーの暗号化方法を手動で指定できます。

> [!NOTE]
> Rest メカニズムで明示的なキーの暗号化を指定する場合、データ保護システムは、ヒューリスティックが提供される既定のキー記憶域メカニズムを登録解除します。 行う必要があります[キー記憶域の明示的なメカニズムが指定](key-storage-providers.md#data-protection-implementation-key-storage-providers)、それ以外の場合、データ保護システムは起動しません。

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

データ保護システムは、次の 3 つのボックスでキーの暗号化メカニズムに付属します。

## <a name="windows-dpapi"></a>Windows DPAPI

*このメカニズムは、Windows でのみ使用可能です。*

キー マテリアルを使用して暗号化されます Windows DPAPI を使用すると[CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx)ストレージに永続化される前にします。 DPAPI は、現在のコンピューターの外部で読み込みは行われませんデータの適切な暗号化メカニズム (ただしこれは Active Directory までこれらのキーをバックアップすることです。 を参照してください[DPAPI および移動ユーザー プロファイル](https://support.microsoft.com/kb/309408/#6))。 たとえば DPAPI キーの暗号化を構成します。

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

場合`ProtectKeysWithDpapi`は、現在の Windows ユーザー アカウントは、永続化されたキー マテリアルを解読できますのみパラメーターなしで呼び出されます。 ように (だけでなく、現在のユーザー アカウント) のコンピューター上の任意のユーザー アカウントが、キー マテリアルを解読できないする必要があるを指定することができます必要に応じて、次の例です。

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>X.509 証明書

*このメカニズムでは利用できません`.NET Core 1.0`または`1.1`です。*

アプリケーションを複数のコンピューターに分散された場合、は、残りの部分でのキーの暗号化にこの証明書を使用するアプリケーションを構成して、コンピューター間で共有の X.509 証明書を配布する便利な場合があります。 例については、以下を参照してください。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

.NET Framework の制限により CAPI 秘密キーを持つ証明書のみがサポートされています。 参照してください[証明書ベースの暗号化に Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng)下これらの制限に対処する方法についてです。

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI NG

*このメカニズムは Windows 8 でのみ利用可能/Windows Server 2012 以降。*

Windows 8 以降では、オペレーティング システムは、DPAPI NG (CNG DPAPI とも呼ばれます) をサポートします。 Microsoft は、その使用方法のシナリオを次のようにレイアウトします。

   クラウド コンピューティング、ただし、多くの場合、必要がありますコンテンツでは暗号化された 1 つのコンピューターが別の復号化されます。 Microsoft はこのため、Windows 8 以降、クラウドのシナリオを使用するように、比較的簡単な API を使用する概念を拡張します。 DPAPI NG と呼ばれるこの新しい API では、一連の適切な認証および承認後に別のコンピューターに保護解除に使用できるプリンシパルを保護することによって、機密データ (キー、パスワード、キー マテリアル) とメッセージを安全に共有することができます。

   [CNG DPAPI について](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

プリンシパルは、保護記述子ルールとしてエンコードされます。 検討してください、次の例では、する暗号化キー マテリアルのみ、ドメインに参加しているユーザー指定の SID を持つには、キー マテリアルが復号化できるようにします。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

パラメーターなしのオーバー ロードもあります`ProtectKeysWithDpapiNG`です。 これは、ルールを指定するための便利なメソッド"SID とマイニング ="で、私は現在の Windows ユーザー アカウントの SID。

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

このシナリオでは、AD ドメイン コント ローラーは DPAPI NG 操作で使用される暗号化キーの配布を担当します。 ターゲット ユーザーは、(その id では、プロセスは実行されている) を指定した任意のドメインに参加しているコンピューターから暗号化されたペイロードを解読することはできます。

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>証明書ベースの暗号化に Windows DPAPI-NG

Windows 8.1 で実行している場合/Windows Server 2012 R2 以降を使用することもできます Windows DPAPI NG 証明書ベースの暗号化を実行するでアプリケーションが実行されている場合でも[.NET Core](https://www.microsoft.com/net/core)です。 これを利用するルール記述子文字列を使用"証明書 HashId:thumbprint を ="で、拇印は、16 進でエンコードされた SHA1 証明書の拇印を使用します。 例については、以下を参照してください。

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

このリポジトリに設定されているすべてのアプリケーションは、Windows 8.1 で実行されている必要があります/Windows Server 2012 R2 以降をこのキーを解読します。

## <a name="custom-key-encryption"></a>カスタムのキーの暗号化

カスタムを提供することによって、開発者が独自のキーの暗号化メカニズムを指定するインボックス メカニズムが適切でない場合`IXmlEncryptor`です。
