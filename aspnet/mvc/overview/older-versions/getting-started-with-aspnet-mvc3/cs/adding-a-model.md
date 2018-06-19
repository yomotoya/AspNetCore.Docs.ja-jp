---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: モデル (c#) を追加する |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879349"
---
<a name="adding-a-model-c"></a>モデル (c#) を追加します。
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
> C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/adding-a-model.md)このチュートリアルのです。


## <a name="adding-a-model"></a>モデルを追加します。

このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。 これらのクラスは、ASP.NET MVC アプリケーションの「モデル」部分になります。

定義し、これらのモデル クラスを使用するには、Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジを使用します。 開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。 最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。 (これらとも呼ばれる従来の CLR オブジェクト"の"から、POCO クラス)これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。

![](adding-a-model/_static/image1.png)

名前、*クラス*「ムービー」です。

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

使用して、`Movie`データベースでムービーを表すクラス。 各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

同じファイルで次のコードを追加`MovieDBContext`クラス。

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。 `MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。 詳細については`DbContext`と`DbSet`を参照してください[Entity Framework 用の生産性向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)です。

参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完全な*Movie.cs*ファイルを次に示します。

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>接続文字列を作成し、SQL Server Compact 協力

`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。 1 つに質問する可能性がありますには、接続先データベースを指定する方法です。 内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。

アプリケーションのルートを開く*Web.config*ファイル。 (されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。次の図を 両方表示*Web.config* ; 開いているファイル、 *Web.config*赤い円で囲まれたファイルです。

![](adding-a-model/_static/image4.png)

### 

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。

新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
