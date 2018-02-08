---
title: "ASP.NET Core 2.x 以降用 Microsoft.AspNetCore.All メタパッケージ"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージがその依存関係と共に含まれています。"
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>ASP.NET Core 2.x 用 Microsoft.AspNetCore.All メタパッケージ

この機能では、.NET Core 2.x を対象とする ASP.NET Core 2.x が必要です。

ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。

* ASP.NET Core チームでサポートされるすべてのパッケージ。
* Entity Framework Core でサポートされるすべてのパッケージ。 
* ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。 

`Microsoft.AspNetCore.All` パッケージには、ASP.NET Core 2.x および Entity Framework Core 2.x のすべての機能が含まれます。 このパッケージは、既定のプロジェクト テンプレートで使用されます。

`Microsoft.AspNetCore.All` メタパッケージのバージョン番号は、(.NET Core バージョンと連携している) ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。

`Microsoft.AspNetCore.All` メタパッケージを使用するアプリケーションでは、[.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)が自動的に利用されます。 このランタイム ストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。 `Microsoft.AspNetCore.All` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**。 .NET Core ランタイム ストアにこれらの資産が含まれています。 ランタイム ストア内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。

使用しないパッケージは、パッケージのトリミング処理を使用して削除することができます。 トリミング処理されたパッケージは、公開済みのアプリケーション出力から除外されます。

次の *.csproj* ファイルは、ASP.NET Core の `Microsoft.AspNetCore.All` メタパッケージを参照しています。

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
