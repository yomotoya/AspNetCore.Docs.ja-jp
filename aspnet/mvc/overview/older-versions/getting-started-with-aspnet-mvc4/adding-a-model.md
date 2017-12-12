---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: "モデルを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>モデルを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。 これらのクラスになります、&quot;モデル&quot;ASP.NET MVC アプリケーションの一部です。

呼ばれる .NET Framework データ アクセス テクノロジを使用、 [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx)を定義し、これらのモデル クラスを使用します。 開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。 最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。 (これらとも呼ばれる POCO クラスから&quot;従来の CLR オブジェクト&quot;)。これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。

## <a name="adding-model-classes"></a>モデル クラスを追加します。

**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。

![](adding-a-model/_static/image1.png)

入力、*クラス*名前&quot;ムービー&quot;です。

次の 5 つのプロパティを追加、`Movie`クラス。

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

使用して、`Movie`データベースでムービーを表すクラス。 各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。

同じファイルで次のコードを追加`MovieDBContext`クラス。

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。 `MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。

参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

完全な*Movie.cs*ファイルを次に示します。 (いくつかはステートメントを使用する必要がなくなりました)。

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB 協力

`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。 1 つに質問する可能性がありますには、接続先データベースを指定する方法です。 内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。

アプリケーションのルートを開く*Web.config*ファイル。 (されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。開く、 *Web.config*赤に記載されているファイル。

![](adding-a-model/_static/image2.png)

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。

新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。

>[!div class="step-by-step"]
[前へ](adding-a-view.md)
[次へ](accessing-your-models-data-from-a-controller.md)
