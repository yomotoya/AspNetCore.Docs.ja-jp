---
title: "サブキーから派生し、認証済み暗号化"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 34bb58a3-5a9a-41e5-b090-08f75b4bbefa
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: e070742b5d9966c4772fd2f0a6d637d98a46137c
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>サブキーから派生し、認証済み暗号化

<a name="data-protection-implementation-subkey-derivation"></a>

キー リング内のほとんどのキー エントロピの何らかの形式にが含まれます、「CBC モードの暗号化 + HMAC 検証」ことを示すアルゴリズム情報を保持または「GCM 暗号化 + 検証」です。 このような場合は、埋め込みのエントロピをこのキーのマスター キー生成情報 (または KM) と呼びます、実際の暗号化操作に使用されるキーを派生させるキーの派生関数を実行します。

> [!NOTE]
> キーは抽象クラスで、次に示すように、カスタムの実装が動作しない可能性があります。 キーは、組み込みの工場のいずれかを使用するのではなく、IAuthenticatedEncryptor の独自の実装を提供する場合は、不要になったこのセクションで説明するメカニズムが適用されます。

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>追加の認証済みデータとサブキーの派生

IAuthenticatedEncryptor インターフェイスは、認証済み暗号化操作はすべてのコア インターフェイスとして機能します。 その暗号化メソッドは 2 つのバッファーを受け取ります。 プレーン テキストと additionalAuthenticatedData (AAD)。 プレーン テキストの内容のフローが IDataProtector.Protect への呼び出しを変更しませんが、AAD は、システムによって生成され、3 つのコンポーネントで構成されています。

1. 32 ビット マジック ヘッダー 09 F0 C9 F0、データ保護システムのこのバージョンを識別します。

2. 128 ビットのキー id です。

3. この操作を実行している IDataProtector を作成する目的のチェーンからの可変長の文字列形式。

AAD は 3 つの要素のタプルに対して一意であるためを KM から KM 自体すべてのページで、暗号化操作を使用する代わりに新しいキーを派生させるために使用できます。 IAuthenticatedEncryptor.Encrypt に各呼び出しでは、次のキー派生のプロセスが行わをれます。

(K_E、K_H) = SP800_108_CTR_HMACSHA512 (K_M、AAD を contextHeader | | keyModifier)

ここでは、NIST SP800 108 KDF カウンター モードにかけています (を参照してください[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)、秒 5.1) は次のパラメーター。

* キー派生キー (KDK) K_M を =

* PRF HMACSHA512 を =

* ラベル additionalAuthenticatedData を =

* コンテキスト contextHeader を = | |keyModifier

コンテキスト ヘッダーが可変長の本質的に拇印をしている派生 K_E と K_H アルゴリズムの役割を果たします。 キーの修飾子は暗号化を呼び出すたびにランダムに生成される 128 ビット文字列であり、確率 KE と KH が一意であるこの特定の認証暗号化操作の場合でも、その他のすべての入力を KDF には定数の過負荷を確実にします。

CBC モードの暗号化 + HMAC 検証操作では、|K_E |対称ブロック暗号キーの長さと |K_H |HMAC ルーチンのダイジェストのサイズです。 GCM 暗号化 + 検証操作 |K_H |= 0。

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC モードの暗号化 + HMAC 検証

K_E が上記のメカニズムによって生成されるはランダムな初期化ベクトルを生成し、プレーン テキストを暗号化する対称ブロック暗号アルゴリズムを実行します。 暗号化テキストと初期化ベクトル K_H MAC を生成するために、キーを使用して初期化 HMAC ルーチンを通じて実行されます。 このプロセスと戻り値は、次に示す視覚的に表されます。

![CBC モードのプロセスと戻り値](subkeyderivation/_static/cbcprocess.png)

*出力: keyModifier を = | |iv | |E_cbc (K_E、iv、データ) | |HMAC (K_H、iv | |E_cbc (K_E、iv、データ))*

> [!NOTE]
> IDataProtector.Protect 実装は[マジック ヘッダーとキーの id を付加](authenticated-encryption-details.md#data-protection-implementation-authenticated-encryption-details)を呼び出し元に返す前に出力します。 ヘッダーのマジックとキーの id は、暗黙的にするための一部[AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad)MAC で最後に返されるペイロードの各バイトの 1 つが認証されていること、キーの修飾子が、KDF への入力としてが取り込まれるため

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/カウンター モードの暗号化と検証

上記のメカニズムを使用して K_E が生成されると、おランダム 96 ビット nonce を生成し、プレーン テキストを暗号化し、128 ビット認証タグを生成する対称ブロック暗号アルゴリズムを実行します。

![GCM モードのプロセスと戻り値](subkeyderivation/_static/galoisprocess.png)

*出力: keyModifier を = | |nonce | |E_gcm (K_E、nonce、データ) | |authTag*

> [!NOTE]
> GCM ネイティブにサポートされていますが、AAD の概念、おいるまだ給紙 AAD 元の KDF にのみ、AAD パラメーターに GCM に空の文字列を渡すを無効にします。 この理由は、2 つの面です。 最初に、[機敏性をサポートするために](context-headers.md#data-protection-implementation-context-headers)暗号化キーとして直接 K_M を使用することはありません。 さらに、GCM には、その入力に非常に厳格な一意性の要件が適用されます。 GCM 暗号化ルーチンが 2 つのこれまで呼び出されたかより明確である確率を設定 (キー、nonce) 同じ入力データのペアは以内で 2 ^32 です。 K_E も修正する場合実行できませんでした。 複数の 2 ^ 32 の暗号化操作は、2 への実行前に ^-32 を制限します。 これは非常に多数の操作と同様にですが、高トラフィックの web サーバーは、これらのキーの通常の有効期間の範囲内で、単なる日以内に 40億要求を通過できます。 2 の準拠を維持する ^128 ビット キーの修飾子と 96 ビット nonce は、任意指定 K_M の使用可能な操作の数を大幅に拡張を使用してまいります-32 確率制限します。 CBC と GCM 操作間 KDF コード パスを共有おわかりやすくするためのデザインと AAD は既に、KDF で考慮ためには、GCM ルーチンに転送する必要はありません。
