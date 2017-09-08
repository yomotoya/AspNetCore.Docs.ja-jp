---
title: "パスワードのハッシュ"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 262eed1f310ff3216c893fdc8a738f2ba6ec6729
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="password-hashing"></a>パスワードのハッシュ

コード ベースをデータの保護には、パッケージが含まれています。 *Microsoft.AspNetCore.Cryptography.KeyDerivation*暗号化キー派生機能が含まれています。 このパッケージは、スタンドアロンのコンポーネントであり、データ保護システムの他の依存関係を持たない。 使用する完全に独立しています。 ソースは、データ保護コードの便宜を図ってベースと共に存在します。

パッケージが現在のメソッドを提供しています`KeyDerivation.Pbkdf2`これにより、パスワードを使用して、ハッシュ、 [PBKDF2 アルゴリズム](https://tools.ietf.org/html/rfc2898#section-5.2)です。 この API は、既存の .NET Framework のとよく似ています[Rfc2898DeriveBytes 型](https://msdn.microsoft.com/library/System.Security.Cryptography.Rfc2898DeriveBytes(v=vs.110).aspx)が 3 つの重要な違いがある場合。

1. `KeyDerivation.Pbkdf2`メソッドは複数 PRFs の使用をサポート (現在`HMACSHA1`、 `HMACSHA256`、および`HMACSHA512`) であるのに対し、`Rfc2898DeriveBytes`サポートのみを入力`HMACSHA1`です。

2. `KeyDerivation.Pbkdf2`メソッドは、現在のオペレーティング システムを検出し、特定の状況で多くのパフォーマンスの向上を提供する、そのルーチンの実装が最も最適化を選択しようとします。 (Windows 8 では約 10 倍のスループットを提供`Rfc2898DeriveBytes`)。

3. `KeyDerivation.Pbkdf2`メソッドが呼び出し元のすべてのパラメーターを指定する必要があります (salt 処理、PRF、および反復回数)。 `Rfc2898DeriveBytes`型は、これらの既定値を提供します。

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

ASP.NET Core Id について、ソース コードを参照してください`PasswordHasher`ユース ケースの実際の型。
