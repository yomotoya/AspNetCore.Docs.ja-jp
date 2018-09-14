---
title: ASP.NET Core でのパスワードのハッシュ
author: rick-anderson
description: ASP.NET Core データ保護 Api を使用してパスワードをハッシュする方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538378"
---
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core でのパスワードのハッシュ

データ保護のコード ベースには、パッケージが含まれています。 *Microsoft.AspNetCore.Cryptography.KeyDerivation*暗号化キー派生関数が含まれています。 このパッケージは、スタンドアロンのコンポーネントであり、データ保護システムの他の依存関係がありません。 できましていない完全に独立しています。 便利なように基本のデータ保護コードと共にソースが存在します。

パッケージが現在のメソッドを提供しています`KeyDerivation.Pbkdf2`を使用してパスワードをハッシュすることができます、 [PBKDF2 アルゴリズム](https://tools.ietf.org/html/rfc2898#section-5.2)します。 この API は、既存の .NET Framework のとよく似ています[Rfc2898DeriveBytes 型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)が、次の 3 つの重要な違いがあります。

1. `KeyDerivation.Pbkdf2`メソッドは複数 PRFs の使用をサポート (現在`HMACSHA1`、 `HMACSHA256`、および`HMACSHA512`) であるのに対し、`Rfc2898DeriveBytes`サポートのみを入力`HMACSHA1`します。

2. `KeyDerivation.Pbkdf2`メソッドは、現在のオペレーティング システムを検出し、場合によっては優れたパフォーマンスを提供する、そのルーチンの最も最適化された実装を選択しようとしています。 (Windows 8 では約 10 倍のスループットが提供`Rfc2898DeriveBytes`)。

3. `KeyDerivation.Pbkdf2`メソッドが呼び出し元のすべてのパラメーターを指定する必要があります (ソルト、PRF、およびイテレーションの数)。 `Rfc2898DeriveBytes`型は、これらの既定値を提供します。

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

[ソース コード] を参照してください (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) ASP.NET Core Identity 用`PasswordHasher`ユース ケースの実際の型。
