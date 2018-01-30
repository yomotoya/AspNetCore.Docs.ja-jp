---
title: "データの保護コンピューター全体のポリシーを ASP.NET Core のサポートします。"
author: rick-anderson
description: "ASP.NET Core データ保護を使用するすべてのアプリの既定のコンピューター全体のポリシーを設定するためのサポートについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 53ded37e9fd5f1a2eaa37935d1c52efb1e9231ac
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>データの保護コンピューター全体のポリシーを ASP.NET Core のサポートします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Windows で実行されるときに、データ保護システムに ASP.NET Core データ保護を使用するすべてのアプリの既定のコンピューター全体のポリシーを設定するためのサポートが制限されています。 一般的な考え方としては、管理者が、アルゴリズムの使用など、既定の設定またはコンピューター上のすべてのアプリを手動で更新する必要はありません、キーの有効期間を変更しようとする可能性があります。

> [!WARNING]
> システム管理者は、既定のポリシーを設定できますが、それを適用することはできません。 アプリの開発者は、独自の選択のいずれかの任意の値を常にオーバーライドできます。 既定のポリシーには、開発者が、設定の明示的な値を指定していないアプリのみに影響します。

## <a name="setting-default-policy"></a>既定のポリシーの設定

既定のポリシーを設定するには、管理者は次のレジストリ キーの下のシステム レジストリで既知の値を設定できます。

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

64 ビット オペレーティング システムで 32 ビット アプリの動作に影響する場合、上記のキーの Wow6432Node と同等の構成を注意してください。

サポートされる値は、以下に示します。

| [値]              | 型   | 説明 |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | アルゴリズムは、データ保護に使用する必要がありますを指定します。 値は、CNG CBC、CNG GCM、または管理対象にする必要がありで詳しく説明します。 |
| DefaultKeyLifetime | DWORD  | 新しく生成されたキーの有効期間を指定します。 値は日数で指定しする必要があります > 7 を = です。 |
| KeyEscrowSinks     | string | キー エスクローに使用される型を指定します。 値は、一覧内の各要素が実装する型のアセンブリ修飾名をここでは、キー エスクロー シンクのセミコロンで区切られたリスト[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)です。 |

## <a name="encryption-types"></a>暗号化の種類

Windows CNG によって提供されるサービスで、信頼性の機密性および HMAC CBC モード対称ブロック暗号を使用する、システムが構成されている EncryptionType が CNG CBC の場合は、(を参照してください[カスタム Windows CNG アルゴリズムを指定する](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)の詳細について)。 次の追加の値がサポートされている、CngCbcAuthenticatedEncryptionSettings 型のプロパティでは、それぞれがします。

| [値]                       | 型   | 説明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムは、CBC モードで開きます。 |
| EncryptionAlgorithmProvider | string | アルゴリズムの EncryptionAlgorithm を生成できる CNG プロバイダーの実装の名前。 |
| EncryptionAlgorithmKeySize  | DWORD  | (Bits) の長さ対称ブロック暗号アルゴリズムを派生させるキー。 |
| HashAlgorithm               | string | CNG で認識されるハッシュ アルゴリズムの名前。 このアルゴリズムは HMAC モードで開きます。 |
| HashAlgorithmProvider       | string | アルゴリズムの HashAlgorithm を作成できる CNG プロバイダーの実装の名前。 |

Windows CNG によって提供されるサービスでの機密性、および信頼性 Galois/カウンター モード対称ブロック暗号を使用する、システムが構成されている EncryptionType が CNG GCM の場合は、(を参照してください[カスタム Windows CNG アルゴリズムを指定する](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)詳細については詳細)。 次の追加の値がサポートされている、CngGcmAuthenticatedEncryptionSettings 型のプロパティでは、それぞれがします。

| [値]                       | 型   | 説明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムは Galois/カウンター モードで開きます。 |
| EncryptionAlgorithmProvider | string | アルゴリズムの EncryptionAlgorithm を生成できる CNG プロバイダーの実装の名前。 |
| EncryptionAlgorithmKeySize  | DWORD  | (Bits) の長さ対称ブロック暗号アルゴリズムを派生させるキー。 |

信頼性の機密性、および KeyedHashAlgorithm のマネージ対称アルゴリズムを使用する、システムが構成されている EncryptionType が管理されている場合 (を参照してください[を指定するカスタム マネージ アルゴリズム](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)詳細)。 次の追加の値がサポートされている、ManagedAuthenticatedEncryptionSettings 型のプロパティでは、それぞれがします。

| [値]                      | 型   | 説明 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | 対称アルゴリズムを実装する型のアセンブリ修飾名。 |
| EncryptionAlgorithmKeySize | DWORD  | (Bits) の長さ、対称暗号化アルゴリズムを派生させるキー。 |
| ValidationAlgorithmType    | string | KeyedHashAlgorithm を実装する型のアセンブリ修飾名。 |

EncryptionType が空か、null 以外の他の任意の値を持つ場合、データ保護システムは、起動時に例外をスローします。

> [!WARNING]
> (EncryptionAlgorithmType、ValidationAlgorithmType、KeyEscrowSinks) の型名を含む既定のポリシー設定を構成するときに型が、アプリに使用する必要があります。 意味アプリについてはデスクトップ CLR で実行されている、これらの型を含むアセンブリ必要がありますグローバル アセンブリ キャッシュ (GAC) 内に存在します。 ASP.NET Core アプリケーションで実行されている[.NET Core](https://www.microsoft.com/net/core)、これらの型を含むパッケージをインストールする必要があります。
