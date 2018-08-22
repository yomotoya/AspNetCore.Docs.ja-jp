---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: モデルの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823884"
---
<a name="adding-a-model"></a>モデルの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。 これらのクラスがありますが、&quot;モデル&quot;ASP.NET MVC アプリケーションの一部です。

呼ばれる .NET Framework データ アクセス テクノロジを使用します、 [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)を定義し、これらのモデル クラスを使用します。 開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。 まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。 (これらとも呼ばれる、POCO クラスから&quot;plain-old CLR object&quot;)。データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。

![](adding-a-model/_static/image1.png)

入力、*クラス*名前&quot;ムービー&quot;します。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

使用して、`Movie`をデータベースのムービーを表すクラス。 各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

同じファイルで次の追加`MovieDBContext`クラス。

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。 `MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。

参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完全な*Movie.cs*ファイルを次に示します。 (いくつかはステートメントを使用する必要がなくなりました)。

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB の使用

`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。 疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。 内の接続情報を追加することによって行います、 *Web.config*アプリケーションのファイル。

アプリケーションのルートを開く*Web.config*ファイル。 (いない、 *Web.config*ファイル、*ビュー*フォルダー)。開く、 *Web.config* red に記載されているファイル。

![](adding-a-model/_static/image2.png)

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

この少量のコードと XML は、すべてを表し、映画のデータをデータベースに格納するために記述する必要があります。

新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
