---
title: ASP.NET Core での認証済み暗号化の詳細
author: rick-anderson
description: ASP.NET Core データの保護が認証された暗号化の実装の詳細を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: ac650e5c32e7eacc4088225e63f56340f95e1913
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275685"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="ed97d-103">ASP.NET Core での認証済み暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="ed97d-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="ed97d-104">IDataProtector.Protect への呼び出しは、認証済み暗号化操作です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="ed97d-105">保護する方法は、機密性と完全性の両方を提供し、IDataProtectionProvider、ルートから派生してこの特定の IDataProtector インスタンスに使用された目的のチェーンに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="ed97d-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="ed97d-106">IDataProtector.Protect は byte[] プレーン テキスト パラメーターを受け取りし、バイトに保護されているペイロード、その形式は以下の説明を生成します。</span><span class="sxs-lookup"><span data-stu-id="ed97d-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="ed97d-107">(も文字列のプレーン テキスト パラメーターを受け取りを文字列の保護されているペイロードを返す拡張メソッド オーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="ed97d-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="ed97d-108">この API を使用する場合、保護されているペイロード形式が残っている、構造体、以下がなります[base64url でエンコードされた](https://tools.ietf.org/html/rfc4648#section-5))。</span><span class="sxs-lookup"><span data-stu-id="ed97d-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="ed97d-109">保護されたペイロードの形式</span><span class="sxs-lookup"><span data-stu-id="ed97d-109">Protected payload format</span></span>

<span data-ttu-id="ed97d-110">保護されたペイロードの形式は、次の 3 つの主要なコンポーネントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ed97d-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="ed97d-111">データ保護システムのバージョンを識別する 32 ビット マジック ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="ed97d-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="ed97d-112">この特定のペイロードを保護するために使用するキーを識別する 128 ビット キー id です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="ed97d-113">保護されたペイロードの残りの部分は[このキーによってカプセル化された暗号化機能に固有](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="ed97d-114">次の例で、キー、AES 256-CBC + HMACSHA256 暗号化機能を表し、ペイロードは次のようにさらに分割します。 \* A 128 ビット キー修飾子です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="ed97d-115">\* は、128 ビットの初期化ベクトルです。</span><span class="sxs-lookup"><span data-stu-id="ed97d-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="ed97d-116">\* AES 256-CBC 出力の 48 バイトです。</span><span class="sxs-lookup"><span data-stu-id="ed97d-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="ed97d-117">\* HMACSHA256 認証タグ。</span><span class="sxs-lookup"><span data-stu-id="ed97d-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="ed97d-118">サンプルの保護されているペイロードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed97d-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="ed97d-119">最初の 32 ビット、または 4 バイトの上、ペイロード形式マジック ヘッダー バージョン (09 F0 C9 F0) を識別するには</span><span class="sxs-lookup"><span data-stu-id="ed97d-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="ed97d-120">[次へ] の 128 ビットまたは 16 バイトは、キー識別子 (80 9 C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="ed97d-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="ed97d-121">残りの部分では、ペイロードを含むあり、使用する形式に固有です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="ed97d-122">指定されたキーに対して保護されているすべてのペイロードは、同じ 20 バイト (マジック値、キー id) のヘッダーで開始されます。</span><span class="sxs-lookup"><span data-stu-id="ed97d-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="ed97d-123">管理者は、およそのペイロードが生成されたときに、診断目的のためには、このファクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ed97d-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="ed97d-124">たとえば、上記のペイロードは、{0c819c80-6619-4019-9536-53f8aaffee57} をキーに対応します。</span><span class="sxs-lookup"><span data-stu-id="ed97d-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="ed97d-125">キーのリポジトリを確認した後は、この特定のキーのアクティブ化された日付が 2015-01-01 と有効期限の日付が 2015-03-01、そのことがあると想定する不適切なを見つけられない場合、ペイロード (改ざんされない) 場合により、そのウィンドウ内で生成されたか、小さないずれかの側の一種です。</span><span class="sxs-lookup"><span data-stu-id="ed97d-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
