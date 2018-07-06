---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: モデル (VB) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827807"
---
<a name="adding-a-model-vb"></a>モデル (VB) を追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-model.md)このチュートリアルの。


## <a name="adding-a-model"></a>モデルの追加

このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。 これらのクラスには、ASP.NET MVC アプリケーションの「モデル」の部分があります。

Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジは、これらのモデル クラスの定義および操作を使用します。 開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。 まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。 (これらとも呼ばれます"plain-old CLR object"からの POCO クラス)データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。

![](adding-a-model/_static/image1.png)

「ビデオ」クラスの名前を付けます。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

使用して、`Movie`をデータベースのムービーを表すクラス。 各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

同じファイルで次の追加`MovieDBContext`クラス。

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。 `MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。 詳細については`DbContext`と`DbSet`を参照してください[Entity Framework の生産性の向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)します。

参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`imports`ファイルの上部にあるステートメント。

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

完全な*Movie.vb*ファイルを次に示します。

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>接続文字列を作成し、SQL Server Compact の使用

`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。 疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。 内の接続情報を追加することによって行います、 *Web.config*アプリケーションのファイル。

アプリケーションのルートを開く*Web.config*ファイル。 (いない、 *Web.config*ファイル、*ビュー*フォルダー)。次の図は、両方を表示*Web.config*ファイル; 開く、 *Web.config*ファイル赤い丸でします。

![](adding-a-model/_static/image2.png)

## 

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

この少量のコードと XML は、すべてを表し、映画のデータをデータベースに格納するために記述する必要があります。

新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
