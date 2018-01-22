---
title: "パスワードのハッシュ"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 Api を使用してパスワードをハッシュする方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: e97d17b5f6de2e0ddcde6d51618675388b43911a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="password-hashing"></a><span data-ttu-id="07f02-103">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="07f02-103">Password Hashing</span></span>

<span data-ttu-id="07f02-104">コード ベースをデータの保護には、パッケージが含まれています。 *Microsoft.AspNetCore.Cryptography.KeyDerivation*暗号化キー派生機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="07f02-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="07f02-105">このパッケージは、スタンドアロンのコンポーネントであり、データ保護システムの他の依存関係を持たない。</span><span class="sxs-lookup"><span data-stu-id="07f02-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="07f02-106">使用する完全に独立しています。</span><span class="sxs-lookup"><span data-stu-id="07f02-106">It can be used completely independently.</span></span> <span data-ttu-id="07f02-107">ソースは、データ保護コードの便宜を図ってベースと共に存在します。</span><span class="sxs-lookup"><span data-stu-id="07f02-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="07f02-108">パッケージが現在のメソッドを提供しています`KeyDerivation.Pbkdf2`これにより、パスワードを使用して、ハッシュ、 [PBKDF2 アルゴリズム](https://tools.ietf.org/html/rfc2898#section-5.2)です。</span><span class="sxs-lookup"><span data-stu-id="07f02-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="07f02-109">この API は、既存の .NET Framework のとよく似ています[Rfc2898DeriveBytes 型](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes)が 3 つの重要な違いがある場合。</span><span class="sxs-lookup"><span data-stu-id="07f02-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="07f02-110">`KeyDerivation.Pbkdf2`メソッドは複数 PRFs の使用をサポート (現在`HMACSHA1`、 `HMACSHA256`、および`HMACSHA512`) であるのに対し、`Rfc2898DeriveBytes`サポートのみを入力`HMACSHA1`です。</span><span class="sxs-lookup"><span data-stu-id="07f02-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="07f02-111">`KeyDerivation.Pbkdf2`メソッドは、現在のオペレーティング システムを検出し、特定の状況で多くのパフォーマンスの向上を提供する、そのルーチンの実装が最も最適化を選択しようとします。</span><span class="sxs-lookup"><span data-stu-id="07f02-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="07f02-112">(Windows 8 では約 10 倍のスループットを提供`Rfc2898DeriveBytes`)。</span><span class="sxs-lookup"><span data-stu-id="07f02-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="07f02-113">`KeyDerivation.Pbkdf2`メソッドが呼び出し元のすべてのパラメーターを指定する必要があります (salt 処理、PRF、および反復回数)。</span><span class="sxs-lookup"><span data-stu-id="07f02-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="07f02-114">`Rfc2898DeriveBytes`型は、これらの既定値を提供します。</span><span class="sxs-lookup"><span data-stu-id="07f02-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="07f02-115">ASP.NET Core Id について、ソース コードを参照してください`PasswordHasher`ユース ケースの実際の型。</span><span class="sxs-lookup"><span data-stu-id="07f02-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
