---
title: ASP.NET Core でのパスワードのハッシュ
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用してパスワードをハッシュする方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010963"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="66f28-103">ASP.NET Core でのパスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="66f28-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="66f28-104">データ保護のコード ベースには、パッケージが含まれています。 *Microsoft.AspNetCore.Cryptography.KeyDerivation*暗号化キー派生関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="66f28-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="66f28-105">このパッケージは、スタンドアロンのコンポーネントであり、データ保護システムの他の依存関係がありません。</span><span class="sxs-lookup"><span data-stu-id="66f28-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="66f28-106">できましていない完全に独立しています。</span><span class="sxs-lookup"><span data-stu-id="66f28-106">It can be used completely independently.</span></span> <span data-ttu-id="66f28-107">便利なように基本のデータ保護コードと共にソースが存在します。</span><span class="sxs-lookup"><span data-stu-id="66f28-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="66f28-108">パッケージが現在のメソッドを提供しています`KeyDerivation.Pbkdf2`を使用してパスワードをハッシュすることができます、 [PBKDF2 アルゴリズム](https://tools.ietf.org/html/rfc2898#section-5.2)します。</span><span class="sxs-lookup"><span data-stu-id="66f28-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="66f28-109">この API は、既存の .NET Framework のとよく似ています[Rfc2898DeriveBytes 型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)が、次の 3 つの重要な違いがあります。</span><span class="sxs-lookup"><span data-stu-id="66f28-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="66f28-110">`KeyDerivation.Pbkdf2`メソッドは複数 PRFs の使用をサポート (現在`HMACSHA1`、 `HMACSHA256`、および`HMACSHA512`) であるのに対し、`Rfc2898DeriveBytes`サポートのみを入力`HMACSHA1`します。</span><span class="sxs-lookup"><span data-stu-id="66f28-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="66f28-111">`KeyDerivation.Pbkdf2`メソッドは、現在のオペレーティング システムを検出し、場合によっては優れたパフォーマンスを提供する、そのルーチンの最も最適化された実装を選択しようとしています。</span><span class="sxs-lookup"><span data-stu-id="66f28-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="66f28-112">(Windows 8 では約 10 倍のスループットが提供`Rfc2898DeriveBytes`)。</span><span class="sxs-lookup"><span data-stu-id="66f28-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="66f28-113">`KeyDerivation.Pbkdf2`メソッドが呼び出し元のすべてのパラメーターを指定する必要があります (ソルト、PRF、およびイテレーションの数)。</span><span class="sxs-lookup"><span data-stu-id="66f28-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="66f28-114">`Rfc2898DeriveBytes`型は、これらの既定値を提供します。</span><span class="sxs-lookup"><span data-stu-id="66f28-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="66f28-115">参照してください、[ソース コード](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs)ASP.NET Core Identity 用`PasswordHasher`ユース ケースの実際の型。</span><span class="sxs-lookup"><span data-stu-id="66f28-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
