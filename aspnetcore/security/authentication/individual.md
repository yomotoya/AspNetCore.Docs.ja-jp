---
title: "個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル"
author: rick-anderson
description: "このドキュメントでは、個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクルが一覧表示します。"
keywords: "ASP.NET Core、承認、IAuthorizationService"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2017
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