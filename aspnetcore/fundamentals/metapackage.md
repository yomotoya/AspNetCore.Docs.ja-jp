---
title: "ASP.NET core Microsoft.AspNetCore.All metapackage 2.x 以降"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage には、その依存関係と共に、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。"
keywords: ASP.NET Core,NuGet,package,Microsoft.AspNetCore.All,metapackage
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 23a07867874eb534c75c4e7b3be00c4a376f8a8b
ms.sourcegitcommit: 4e45fd4e3f1374cd51cc931cee93c9d72631d9fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>ASP.NET core Microsoft.AspNetCore.All metapackage 2.x

この機能では ASP.NET Core 2.x です。

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET core metapackage が含まれています。

* パッケージは、ASP.NET Core チームがすべてサポートされます。
* Entity Framework Core パッケージはすべてサポートされます。 
* ASP.NET Core および Entity Framework のコアで使用される内部およびサードパーティの依存します。 

ASP.NET Core のすべての機能 2.x および Entity Framework Core 2.x に含まれる、`Microsoft.AspNetCore.All`パッケージです。 既定のプロジェクト テンプレートは、このパッケージを使用します。

バージョン番号、 `Microsoft.AspNetCore.All` metapackage ASP.NET Core バージョンおよび Entity Framework Core バージョン (.NET Core バージョンで揃え) を表します。

使用するアプリケーション、 `Microsoft.AspNetCore.All` metapackage を自動的に利用、 [.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)です。 ランタイムのストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。 使用すると、 `Microsoft.AspNetCore.All` metapackage、**ありません**から ASP.NET Core NuGet パッケージの参照先のアセットは、アプリケーションと共に配置&mdash;.NET Core ランタイム ストアには、これらのアセットが含まれています。 アプリケーションの起動時間を向上させるためには、ランタイム ストア内の資産がプリコンパイル済みです。

使用しないパッケージを削除するパッケージのトリミング処理を行うこともできます。 トリミングされたパッケージは、公開されたアプリケーションの出力に除外されます。

次*.csproj*ファイル参照、 `Microsoft.AspNetCore.All` metapackage ASP.NET core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
