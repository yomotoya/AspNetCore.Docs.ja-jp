---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: モデル (VB) を追加する |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868140"
---
<a name="adding-a-model-vb"></a>モデル (VB) を追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-model.md)このチュートリアルのです。


## <a name="adding-a-model"></a>モデルを追加します。

このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。 これらのクラスは、ASP.NET MVC アプリケーションの「モデル」部分になります。

定義し、これらのモデル クラスを使用するには、Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジを使用します。 開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。 最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。 (これらとも呼ばれる従来の CLR オブジェクト"の"から、POCO クラス)これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。

![](adding-a-model/_static/image1.png)

クラス「ムービー」の名前を付けます。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

使用して、`Movie`データベースでムービーを表すクラス。 各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

同じファイルで次のコードを追加`MovieDBContext`クラス。

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。 `MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。 詳細については`DbContext`と`DbSet`を参照してください[Entity Framework 用の生産性向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)です。

参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`imports`ファイルの上部にあるステートメント。

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

完全な*Movie.vb*ファイルを次に示します。

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>接続文字列を作成し、SQL Server Compact 協力

`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。 1 つに質問する可能性がありますには、接続先データベースを指定する方法です。 内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。

アプリケーションのルートを開く*Web.config*ファイル。 (されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。次の図を 両方表示*Web.config* ; 開いているファイル、 *Web.config*赤い円で囲まれたファイルです。

![](adding-a-model/_static/image2.png)

## 

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。

新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
