---
title: "マシン全体のポリシー"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a>マシン全体のポリシー

<a name=data-protection-configuration-machinewidepolicy></a>

Windows で実行されるときに、データ保護システムにデータの保護を使用するすべてのアプリケーションの既定のコンピューター全体のポリシーを設定するためのサポートが制限されています。 一般的な考え方としては、管理者が、コンピューター上のすべてのアプリケーションを手動で更新する必要はありません (アルゴリズムまたはキーの使用有効期間) などの既定の設定を変更しようとする可能性があります。

>[!WARNING]
> システム管理者は、既定のポリシーを設定できますが、それを適用することはできません。 アプリケーション開発者は、独自の選択のいずれかの任意の値を常にオーバーライドできます。 既定のポリシーは、開発者が指定されていない場合、明示的な値をいくつか特定の設定をアプリケーションにのみ影響します。

## <a name="setting-default-policy"></a>既定のポリシーの設定

既定のポリシーを設定するには、管理者は、次のキーの下のシステム レジストリで既知の値を設定できます。

レジストリ キー:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

64 ビット オペレーティング システムで 32 ビット アプリケーションの動作に影響する場合、忘れずにも構成上のキーの Wow6432Node に相当します。

サポートされる値は次のとおりです。

* EncryptionType [文字列] には、アルゴリズムは、データ保護に使用する必要がありますを指定します。 この値は"CNG CBC"、"CNG-GCM"または"Managed"にする必要があります、さらに詳しく記載されて[下](#data-protection-encryption-types)です。

* DefaultKeyLifetime [DWORD] には、新しく生成されたキーの有効期間を指定します。 この値は日数で指定され、≥ 7 をする必要があります。

* KeyEscrowSinks [文字列] には、キー エスクローを使用する型を指定します。 この値は、IKeyEscrowSink を実装する型のアセンブリ修飾名を一覧内の各要素がここでは、キー エスクロー シンクのセミコロンで区切られたリストです。

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a>暗号化の種類

Windows CNG によって提供されるサービスとの信頼性の機密性、および HMAC CBC モード対称ブロック暗号を使用するように、システム構成は EncryptionType が"CNG CBC"の場合は、(を参照してください[のカスタムのWindowsCNGアルゴリズムを指定する](overview.md#data-protection-changing-algorithms-cng)詳細)。 次の追加の値がサポートされているそれぞれに対応して CngCbcAuthenticatedEncryptionSettings 型のプロパティ。

* [String] - EncryptionAlgorithm CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムは、CBC モードで表示します。

* [String] - EncryptionAlgorithmProvider アルゴリズム EncryptionAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。

* [DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称ブロック暗号アルゴリズムを派生させるキー。

* [String] - HashAlgorithm CNG で認識されるハッシュ アルゴリズムの名前。 このアルゴリズムは、HMAC モードで表示します。

* [String] - HashAlgorithmProvider アルゴリズムの HashAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。

Windows CNG によって提供されるサービスでの機密性、および信頼性 Galois/カウンター モード対称ブロック暗号を使用するように、システム構成は EncryptionType が"CNG GCM"の場合は、(を参照してください[のカスタムのWindowsCNGアルゴリズムを指定する](overview.md#data-protection-changing-algorithms-cng)詳細)。 次の追加の値がサポートされているそれぞれに対応して CngGcmAuthenticatedEncryptionSettings 型のプロパティ。

* [String] - EncryptionAlgorithm CNG で認識される対称ブロック暗号アルゴリズムの名前。 このアルゴリズムが Galois/カウンター モードで開きます。

* [String] - EncryptionAlgorithmProvider アルゴリズム EncryptionAlgorithm を生じる可能性が CNG プロバイダーの実装の名前。

* [DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称ブロック暗号アルゴリズムを派生させるキー。

EncryptionType が"Managed"場合、システムは、信頼性の機密性、および KeyedHashAlgorithm のマネージ対称アルゴリズムを使用する構成は (を参照してください[を指定するカスタム マネージ アルゴリズム](overview.md#data-protection-changing-algorithms-custom-managed)詳細)。 次の追加の値がサポートされているそれぞれに対応して ManagedAuthenticatedEncryptionSettings 型のプロパティ。

* [String] - EncryptionAlgorithmType 対称アルゴリズムを実装する型のアセンブリ修飾名。

* [DWORD] - EncryptionAlgorithmKeySize (bits) の長さ、対称暗号化アルゴリズムを派生させるキー。

* [String] - ValidationAlgorithmType KeyedHashAlgorithm を実装する型のアセンブリ修飾名。

EncryptionType がその他の値 (null 以外/空) の場合、データ保護システム起動時に例外がスローされます。

>[!WARNING]
> 型名 (EncryptionAlgorithmType、ValidationAlgorithmType、KeyEscrowSinks) は、既定のポリシー設定を構成するときに、型が、アプリケーションで使用できる場合があります。 実際には、これは、アプリケーションのデスクトップ CLR で実行されている場合、アセンブリをこれらの型を含む必要がある GACed ことを意味します。 実行されている ASP.NET Core アプリケーション[.NET Core](https://www.microsoft.com/net/core)、これらの型を含むパッケージをインストールする必要があります。
