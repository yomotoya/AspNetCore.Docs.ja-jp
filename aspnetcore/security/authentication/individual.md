---
title: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクル
author: rick-anderson
description: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクルを検出します。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274428"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクル

ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。

認証テンプレートを利用できますと .NET Core CLI で`-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。

* [SMS での 2 要素認証](xref:security/authentication/2fa)
* [ASP.NET Core でのアカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
* [認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。](xref:security/authorization/secure-data)
