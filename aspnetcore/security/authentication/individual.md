---
title: "個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル"
author: rick-anderson
description: "このドキュメントでは、個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクルが一覧表示します。"
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル

ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。

認証テンプレートを利用できますと .NET Core CLI で`-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。

* [SMS での 2 要素認証](xref:security/authentication/2fa)
* [ASP.NET Core でのアカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
* [認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。](xref:security/authorization/secure-data)
