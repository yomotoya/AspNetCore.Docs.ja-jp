---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: モデルの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7326cefd52feb63fc2f602210ed560cdf28706a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830309"
---
<a name="adding-a-model"></a>モデルの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。 これらのクラスがありますが、&quot;モデル&quot;ASP.NET MVC アプリの一部です。

呼ばれる .NET Framework データ アクセス テクノロジを使用します、 [Entity Framework](https://docs.microsoft.com/ef/)を定義し、これらのモデル クラスを使用します。 開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。 まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。 (これらとも呼ばれる、POCO クラスから&quot;plain-old CLR object&quot;)。データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。 まずデータベースを作成するために必要な場合は、MVC と EF のアプリ開発の詳細については、このチュートリアルを引き続きに従ってできます。 その後 Tom Fizmakens [ASP.NET スキャフォールディング](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)チュートリアルでは、データベースの最初のアプローチを対象とします。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。

![](adding-a-model/_static/image1.png)

入力、*クラス*名前&quot;ムービー&quot;します。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

使用して、`Movie`をデータベースのムービーを表すクラス。 各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

注: System.Data.Entity、および関連するクラスを使用するためにインストールする必要が、 [Entity Framework NuGet パッケージ](https://www.nuget.org/packages/EntityFramework/)します。 詳細については、リンクに従います。

同じファイルで次の追加`MovieDBContext`クラス。

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。 `MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。

参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

使用して、手動で追加することでこれを行うステートメント、またはすることができます合わせると赤色の波線をクリックします`Show potential fixes` をクリック `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

注: いくつか使用されていない`using`ステートメントが削除されました。 Visual Studio は、未使用の依存関係を灰色で表示されます。 ポインターを合わせると灰色の依存関係で使用されていない依存関係を削除するか、をクリックして`Show potential fixes`クリック**未使用の Using の削除します。**

![](adding-a-model/_static/image3.png)

最後にモデル (MVC で M) が追加されました。 次のセクションでは、データベース接続文字列を使用します。

> [!div class="step-by-step"]
> [前へ](adding-a-view.md)
> [次へ](creating-a-connection-string.md)
