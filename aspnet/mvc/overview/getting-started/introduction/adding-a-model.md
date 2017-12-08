---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "モデルを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a>モデルを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。 これらのクラスになります、&quot;モデル&quot;ASP.NET MVC アプリの一部です。

呼ばれる .NET Framework データ アクセス テクノロジを使用、 [Entity Framework](https://docs.microsoft.com/ef/)を定義し、これらのモデル クラスを使用します。 開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。 最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。 (これらとも呼ばれる POCO クラスから&quot;従来の CLR オブジェクト&quot;)。これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。 まず、データベースを作成するために必要な場合は、MVC および EF アプリの開発に関する学習するのには、このチュートリアルをまだに従ってできます。 Tom Fizmakens を行うことができる、 [ASP.NET スキャフォールディング](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)チュートリアル」では、データベースの最初の方法について説明します。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。

![](adding-a-model/_static/image1.png)

入力、*クラス*名前&quot;ムービー&quot;です。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

使用して、`Movie`データベースでムービーを表すクラス。 各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

注: System.Data.Entity、および関連クラスを使用するためにインストールする必要が、 [Entity Framework の NuGet パッケージ](https://www.nuget.org/packages/EntityFramework/)です。 詳細については、リンクに従います。

同じファイルで次のコードを追加`MovieDBContext`クラス。

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。 `MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。

参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

使用して、手動で追加してこれを行うステートメント、またはすることができます赤の波線を合わせるをクリックして`Show potential fixes` をクリック`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

注: いくつか使用されていない`using`ステートメントが削除されました。 Visual Studio では、依存関係で未使用を灰色で表示します。 灰色の依存関係を合わせることで unnused 依存関係を削除するかをクリックして、 `Show potential fixes`  をクリック**未使用の Using の削除します。**

![](adding-a-model/_static/image3.png)

最後に、モデル (MVC で M) が追加されました。 次のセクションでは、データベース接続文字列を使用します。

>[!div class="step-by-step"]
[前へ](adding-a-view.md)
[次へ](creating-a-connection-string.md)
