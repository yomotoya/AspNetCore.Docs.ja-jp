---
title: "ASP.NET Core と Entity Framework 6 の概要"
author: tdykstra
description: "この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。"
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>ASP.NET Core と Entity Framework 6 の概要

によって[Paweł Grudzień](https://github.com/pgrudzien12)、 [Damien Pontifex](https://github.com/DamienPontifex)、および[Tom Dykstra](https://github.com/tdykstra)

この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。

## <a name="overview"></a>概要

Entity Framework 6 を使用するのには、Entity Framework 6 .NET Core をサポートするいないとも、プロジェクトには .NET フレームワークに対してコンパイルがします。 クロスプラット フォームの機能が必要な場合にアップグレードする必要があります。 [Entity Framework Core](https://docs.microsoft.com/ef/)です。

ASP.NET Core アプリケーションで Entity Framework 6 を使用することをお勧め EF6 コンテキストには、プロジェクトを対象とする完全なフレームワークのクラス ライブラリでモデル クラス。 ASP.NET Core プロジェクトから、クラス ライブラリへの参照を追加します。 このサンプルを参照してください[EF6 および ASP.NET Core プロジェクトと Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)です。

.NET Core プロジェクトには、すべての機能 EF6 などのコマンドをサポートしないので、プロジェクトで ASP.NET Core EF6 コンテキストを配置することはできません*Enable-migrations*を必要とします。

EF6 コンテキストを検索するプロジェクトの種類に関係なく EF6 コマンド ライン ツールだけは EF6 コンテキストを使用します。 たとえば、`Scaffold-DbContext`は Entity Framework のコアでのみ使用します。 EF6 モデルにリバース エンジニア リングのデータベースについて実行する必要がある場合は、次を参照してください。[既存のデータベースに Code First](https://msdn.microsoft.com/jj200620)です。

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>参照を完全なフレームワークおよび ASP.NET Core プロジェクトに EF6

ASP.NET Core プロジェクトは、.NET framework と EF6 を参照する必要があります。 たとえば、 *.csproj* ASP.NET Core プロジェクトのファイルは次の例のようになります (ファイルの関連する部分のみが表示されます)。

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

新しいプロジェクトを作成するときに使用して、 **ASP.NET Core Web アプリケーション (.NET Framework)**テンプレート。

## <a name="handle-connection-strings"></a>接続文字列を処理します。

EF6 クラス ライブラリ プロジェクトを使用して、EF6 コマンド ライン ツールには、コンテキストのインスタンスを作成するための既定のコンス トラクターが必要があります。 ASP.NET Core のプロジェクトである場合、コンテキスト コンス トラクターを使用する接続文字列は、接続文字列に渡すことができますをパラメーターを持つ必要がありますを指定することが可能性があります。 次に例を示します。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

実装を提供する EF6 プロジェクトが、EF6 コンテキストでは、パラメーターなしのコンス トラクターがあるないため[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)です。 EF6 コマンド ライン ツールを見つけて、コンテキストをインスタンス化することができますのでこの実装を使用します。 次に例を示します。

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

このサンプル コードで、`IDbContextFactory`実装のハード コーディングされた接続文字列に渡します。 これは、コマンド ライン ツールで使用される接続文字列です。 クラス ライブラリで呼び出し元のアプリケーションを使用する同じ接続文字列を使用することを確認するための戦略を実装します。 たとえば、両方のプロジェクトでの環境変数から値を取得する可能性があります。

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>ASP.NET Core プロジェクトに依存関係の挿入を設定します。

中核となるプロジェクトの*Startup.cs*で依存性の注入 (DI) の EF6 コンテキストを設定して、ファイル`ConfigureServices`です。 EF コンテキスト オブジェクトは、要求ごとの有効期間中にスコープする必要があります。

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

DI を使用して、コント ローラーでのコンテキストのインスタンスを取得できます。 コードは、EF コア コンテキストを記述して新機能と同様です。

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>サンプル アプリケーション

実際のサンプル アプリケーションでは、次を参照してください。、[サンプル Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)この記事に付属しています。

このサンプルは、Visual Studio で次の手順で、ゼロから作成することができます。

* ソリューションを作成します。

* **新しいプロジェクトの追加 > Web > ASP.NET Core Web アプリケーション (.NET Framework)**

* **新しいプロジェクトの追加 > Windows クラシック デスクトップ > クラス ライブラリ (.NET Framework)**

* **Package Manager Console** (PMC) 両方のプロジェクトでは、コマンドを実行`Install-Package Entityframework`です。

* クラス ライブラリ プロジェクトで、データ モデルのクラスと、コンテキスト クラスの実装を作成する`IDbContextFactory`です。

* クラス ライブラリ プロジェクトの PMC でのコマンドを実行`Enable-Migrations`と`Add-Migration Initial`です。 スタートアップ プロジェクトとして ASP.NET Core プロジェクトを設定する場合、追加`-StartupProjectName EF6`これらのコマンドにします。

* コア プロジェクトでは、クラス ライブラリ プロジェクトへのプロジェクト参照を追加します。

* コア プロジェクト内で*Startup.cs*DI のコンテキストを登録します。

* コア プロジェクト内で*される appsettings.json*、接続文字列を追加します。

* コア プロジェクトでは、コント ローラーとの読み取りおよびデータを書き込むことを確認するビューを追加します。 (ASP.NET Core MVC のスキャフォールディングをクラス ライブラリから参照されている EF6 コンテキストで動作しないことに注意してください)。

## <a name="summary"></a>まとめ

この記事は、ASP.NET Core アプリケーションで Entity Framework 6 を使用するための基本的なガイダンスを提供しています。

## <a name="additional-resources"></a>その他の技術情報

* [Entity Framework のコード ベースの構成](https://msdn.microsoft.com/data/jj680699.aspx)
