---
title: "パスワードのハッシュ"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: d4b39bf7feba8a29e0b5c9e56d53a85b82977b7e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="password-hashing"></a><span data-ttu-id="84f55-103">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="84f55-103">Password Hashing</span></span>

<span data-ttu-id="84f55-104">コード ベースをデータの保護には、パッケージが含まれています。 *Microsoft.AspNetCore.Cryptography.KeyDerivation*暗号化キー派生機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="84f55-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="84f55-105">このパッケージは、スタンドアロンのコンポーネントであり、データ保護システムの他の依存関係を持たない。</span><span class="sxs-lookup"><span data-stu-id="84f55-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="84f55-106">使用する完全に独立しています。</span><span class="sxs-lookup"><span data-stu-id="84f55-106">It can be used completely independently.</span></span> <span data-ttu-id="84f55-107">ソースは、データ保護コードの便宜を図ってベースと共に存在します。</span><span class="sxs-lookup"><span data-stu-id="84f55-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="84f55-108">パッケージが現在のメソッドを提供しています`KeyDerivation.Pbkdf2`これにより、パスワードを使用して、ハッシュ、 [PBKDF2 アルゴリズム](https://tools.ietf.org/html/rfc2898#section-5.2)です。</span><span class="sxs-lookup"><span data-stu-id="84f55-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="84f55-109">この API は、既存の .NET Framework のとよく似ています[Rfc2898DeriveBytes 型](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes)が 3 つの重要な違いがある場合。</span><span class="sxs-lookup"><span data-stu-id="84f55-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="84f55-110">`KeyDerivation.Pbkdf2`メソッドは複数 PRFs の使用をサポート (現在`HMACSHA1`、 `HMACSHA256`、および`HMACSHA512`) であるのに対し、`Rfc2898DeriveBytes`サポートのみを入力`HMACSHA1`です。</span><span class="sxs-lookup"><span data-stu-id="84f55-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="84f55-111">`KeyDerivation.Pbkdf2`メソッドは、現在のオペレーティング システムを検出し、特定の状況で多くのパフォーマンスの向上を提供する、そのルーチンの実装が最も最適化を選択しようとします。</span><span class="sxs-lookup"><span data-stu-id="84f55-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="84f55-112">(Windows 8 では約 10 倍のスループットを提供`Rfc2898DeriveBytes`)。</span><span class="sxs-lookup"><span data-stu-id="84f55-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="84f55-113">`KeyDerivation.Pbkdf2`メソッドが呼び出し元のすべてのパラメーターを指定する必要があります (salt 処理、PRF、および反復回数)。</span><span class="sxs-lookup"><span data-stu-id="84f55-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="84f55-114">`Rfc2898DeriveBytes`型は、これらの既定値を提供します。</span><span class="sxs-lookup"><span data-stu-id="84f55-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

<span data-ttu-id="84f55-115">[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]</span><span class="sxs-lookup"><span data-stu-id="84f55-115">[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]</span></span>

<span data-ttu-id="84f55-116">ASP.NET Core Id について、ソース コードを参照してください`PasswordHasher`ユース ケースの実際の型。</span><span class="sxs-lookup"><span data-stu-id="84f55-116">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
