---
title: ASP.NET Core でのデータの保護コンピューター全体のポリシーをサポートします。
author: rick-anderson
description: ASP.NET Core データ保護を使用するすべてのアプリの既定のコンピューター全体のポリシー設定のサポートについて説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897269"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>ASP.NET Core でのデータの保護コンピューター全体のポリシーをサポートします。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

Windows で実行するときに、データ保護システムに ASP.NET Core データ保護を使用するすべてのアプリの既定のコンピューター全体のポリシー設定のサポートが制限されています。 基本的な考え方は、管理者が、アルゴリズムの使用など、既定の設定またはコンピューターのすべてのアプリを手動で更新する必要はありません、キーの有効期間を変更するとすることもできます。

> [!WARNING]
> システム管理者は、既定のポリシーを設定できますが、それを適用することはできません。 アプリ開発者は、独自の選択のいずれかの任意の値を常にオーバーライドできます。 既定のポリシーには、開発者が明示的な値の設定を指定していないアプリのみに影響します。

## <a name="setting-default-policy"></a>既定のポリシーを設定

既定のポリシーを設定するには、管理者は、次のレジストリ キーの下のシステム レジストリで既知の値を設定できます。

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

64 ビットのオペレーティング システムで 32 ビット アプリの動作に影響する場合は、Wow6432Node と同等の上記のキーを構成してください。

サポートされている値は、以下に示します。

| [値]              | 型   | 説明 |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | データ保護にどのアルゴリズムを使用する必要がありますを指定します。 値は、CNG CBC、CNG の GCM、または管理されている必要があり、詳細については、以下が説明します。 |
| DefaultKeyLifetime | DWORD  | 新しく生成されたキーの有効期間を指定します。 値を選択しを日数で指定する必要があります > 7 を = です。 |
| KeyEscrowSinks     | string | キー エスクローの使用される型を指定します。 値は、キー エスクロー シンク、リスト内の各要素が実装する型のアセンブリ修飾名をセミコロンで区切られたリスト[IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink)します。 |

## <a name="encryption-types"></a>暗号化の種類

システムが Windows CNG が提供するサービスで、信頼性の機密性と HMAC の CBC モードの対称ブロック暗号を使用するよう構成 EncryptionType が CNG CBC の場合は、(を参照してください[カスタム Windows CNG アルゴリズムを指定する](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)の参照してください。) 次の追加の値がサポートされている、CngCbcAuthenticatedEncryptionSettings 型のプロパティにそれぞれ対応します。

| [値]                       | 型   | 説明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムは、CBC モードで開きます。 |
| EncryptionAlgorithmProvider | string | EncryptionAlgorithm アルゴリズムを作成できる CNG プロバイダーの実装の名前。 |
| EncryptionAlgorithmKeySize  | DWORD  | (Bits) の対称ブロック暗号アルゴリズムを派生させるキーの長さ。 |
| HashAlgorithm               | string | CNG で認識されるハッシュ アルゴリズムの名前。 このアルゴリズムは、HMAC のモードで開きます。 |
| HashAlgorithmProvider       | string | アルゴリズムの HashAlgorithm を作成できる CNG プロバイダーの実装の名前。 |

EncryptionType が CNG GCM の場合は、Windows CNG が提供するサービスに、機密性、完全性を Galois/カウンター モードの対称ブロック暗号を使用するシステムの構成 (を参照してください[カスタム Windows CNG アルゴリズムを指定する](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms)ご覧ください)。 次の追加の値がサポートされている、CngGcmAuthenticatedEncryptionSettings 型のプロパティにそれぞれ対応します。

| [値]                       | 型   | 説明 |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムが Galois/カウンター モードで開かれます。 |
| EncryptionAlgorithmProvider | string | EncryptionAlgorithm アルゴリズムを作成できる CNG プロバイダーの実装の名前。 |
| EncryptionAlgorithmKeySize  | DWORD  | (Bits) の対称ブロック暗号アルゴリズムを派生させるキーの長さ。 |

EncryptionType が管理されている場合の信頼性の機密性および KeyedHashAlgorithm マネージ SymmetricAlgorithm を使用するシステムの構成 (を参照してください[を指定するカスタム アルゴリズムのマネージ](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms)の詳細)。 次の追加の値がサポートされている、ManagedAuthenticatedEncryptionSettings 型のプロパティにそれぞれ対応します。

| [値]                      | 型   | 説明 |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | SymmetricAlgorithm を実装する型のアセンブリ修飾名。 |
| EncryptionAlgorithmKeySize | DWORD  | 対称暗号化アルゴリズムを派生させるキーの (bits) の長さ。 |
| ValidationAlgorithmType    | string | KeyedHashAlgorithm を実装する型のアセンブリ修飾名。 |

EncryptionType がその他の値以外は、null または空の場合、データ保護システムは起動時に例外をスローします。

> [!WARNING]
> 型名 (EncryptionAlgorithmType、ValidationAlgorithmType、KeyEscrowSinks) は、既定のポリシー設定を構成するときに、型が、アプリに対して使用できる場合があります。 意味デスクトップ CLR で実行されるアプリでは、これらの型を含むアセンブリがグローバル アセンブリ キャッシュ (GAC) に存在します。 ASP.NET Core アプリの .NET Core で実行されている場合は、これらの型が含まれているパッケージをインストールする必要があります。
