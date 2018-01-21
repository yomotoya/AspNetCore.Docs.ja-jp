---
title: "ASP.NET core Microsoft.AspNetCore.All metapackage 2.x 以降"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage には、その依存関係と共に、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>ASP.NET core Microsoft.AspNetCore.All metapackage 2.x

この機能では ASP.NET Core 2.x の対象とする .NET 2.x のコアです。

ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。

* ASP.NET Core チームでサポートされるすべてのパッケージ。
* Entity Framework Core でサポートされるすべてのパッケージ。 
* ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。 

ASP.NET Core のすべての機能 2.x および Entity Framework Core 2.x に含まれる、`Microsoft.AspNetCore.All`パッケージです。 既定のプロジェクト テンプレートは、このパッケージを使用します。

バージョン番号、 `Microsoft.AspNetCore.All` metapackage ASP.NET Core バージョンおよび Entity Framework Core バージョン (.NET Core バージョンで揃え) を表します。

使用するアプリケーション、 `Microsoft.AspNetCore.All` metapackage を自動的に利用、 [.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)です。 ランタイムのストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。 使用すると、 `Microsoft.AspNetCore.All` metapackage、**ありません**から ASP.NET Core NuGet パッケージの参照先のアセットは、アプリケーションと共に配置&mdash;.NET Core ランタイム ストアには、これらのアセットが含まれています。 アプリケーションの起動時間を向上させるためには、ランタイム ストア内の資産がプリコンパイル済みです。

使用しないパッケージを削除するパッケージのトリミング処理を行うこともできます。 トリミングされたパッケージは、公開されたアプリケーションの出力に除外されます。

次*.csproj*ファイル参照、 `Microsoft.AspNetCore.All` metapackage ASP.NET core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
