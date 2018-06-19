---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'パート 4: モデルおよびデータ アクセス |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルおよびデータ アクセスについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879479"
---
<a name="part-4-models-and-data-access"></a>パート 4: モデルおよびデータ アクセス
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。
> 
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルおよびデータ アクセスについて説明します。


これまでおしただけされてデータを渡す"ダミー"、コント ローラーからテンプレートを表示します。 実際のデータベースをフックする準備ができました。 このチュートリアルでのテーマは、SQL Server Compact Edition (SQL CE と呼びます) を使用する方法、データベース エンジンとします。 SQL CE は、任意のインストールまたは構成で、ローカルの開発は本当に容易に必要としない、無料の埋め込み、ファイルのベース データベースです。

## <a name="database-access-with-entity-framework-code-first"></a>データベースへのアクセスと Entity Framework コード優先

クエリおよびデータベースを更新する ASP.NET MVC 3 プロジェクトに含まれている Entity Framework (EF) のサポートを使用します。 EF は、柔軟なオブジェクト リレーショナル マッピング (ORM) データを開発者が、オブジェクト指向の方法でデータベースに格納されているデータのクエリと更新プログラムを有効にする API です。

Entity Framework version 4 には、コード優先と呼ばれる開発パラダイムがサポートしています。 コード優先では、単純なクラスとも呼ばれる POCO ("プレーン old"CLR オブジェクトから) を作成してモデル オブジェクトを作成することができ、クラスから実行時にデータベースを作成することもできます。

### <a name="changes-to-our-model-classes"></a>モデル クラスへの変更

おを利用して、データベース作成機能 Entity Framework でこのチュートリアルでします。 その前は、後で使用するいくつかの点で追加するには、このモデル クラスをいくつかの軽微な変更を行うただし、みましょう。

#### <a name="adding-the-artist-model-classes"></a>アーティスト モデル クラスを追加します。

アルバムは単純なモデル クラスをアーティストに依頼を記述する追加されますので、アーティストに関連付けられます。 新しいクラスを次に示すコードを使用して Artist.cs をという名前のモデル フォルダーに追加します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>モデル クラスの更新

アルバム クラスは、次に示すように更新します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

次に、ジャンル クラスに次の更新を行います。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>アプリを追加する\_データ フォルダー

アプリを追加します\_SQL Server Express データベース ファイルを保持するために、プロジェクトにデータ ディレクトリ。 アプリ\_データが既にデータベースへのアクセスの適切なセキュリティ アクセス許可を持っている ASP.NET の特別なディレクトリ。 [プロジェクト] メニューから選択し、ASP.NET フォルダーの追加、アプリ\_データ。

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Web.config ファイルで接続文字列の作成

Entity Framework は、データベースに接続する方法を認識できるように、web サイトの構成ファイルにいくつかの行を追加おされます。 プロジェクトのルートにある Web.config ファイルをダブルクリックします。

![](mvc-music-store-part-4/_static/image2.png)

このファイルの一番下までスクロールし、追加、 &lt;connectionStrings&gt;次に示すように、最後の行の上に直接セクションです。

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>コンテキスト クラスの追加

モデル フォルダーを右クリックし、MusicStoreEntities.cs をという名前の新しいクラスを追加します。

![](mvc-music-store-part-4/_static/image3.png)

このクラスは、Entity Framework データベースのコンテキストを表すとは、作成を処理、読み取り、更新、および削除操作ご利用の米国。 このクラスのコードは、以下に示します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

それで完了です - その他の構成、特殊なインターフェイスなどはありません。DbContext の基本クラスを拡張するには、MusicStoreEntities クラスは、ご利用の米国のデータベース操作を処理することです。 すでに持っているフック、これで、いくつかのプロパティを利用するために必要な追加情報の一部のデータベースにモデル クラスに追加してみましょう。

### <a name="adding-our-store-catalog-data"></a>このストアのカタログ データを追加します。

新しく作成されたデータベースにデータを「シード」が追加される Entity Framework の機能を活用おかかります。 これが事前ジャンル、アーティスト、アルバムの一覧で、ストアのカタログに設定します。 -これは、このチュートリアルで以前に使用する、サイト設計ファイルが含まれている - MvcMusicStore Assets.zip ダウンロードには、コードをという名前のフォルダー内にあるこのシード データでクラス ファイルがあります。

コード内で/[モデル] フォルダー、SampleData.cs ファイルを見つけて次のように、プロジェクトで、Models フォルダーにドロップします。

![](mvc-music-store-part-4/_static/image4.png)

Entity Framework をその SampleData クラスに紹介するコードの 1 つの行を追加する必要があります。 開き、アプリケーションを先頭に次の行を追加するプロジェクトのルートで Global.asax ファイルをダブルクリックして\_メソッドを開始します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

この時点では、Entity Framework プロジェクトを構成するのに必要な作業が終了しました。

## <a name="querying-the-database"></a>データベースに対するクエリの実行

今すぐみましょうではなく「ダミー データ」を使用して、代わりに呼び出してすべての情報を照会するデータベース内にするように、StoreController を更新します。 まず始めにフィールドを宣言することによって、 **StoreController** storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>データベースを照会するストア インデックスを更新します。

MusicStoreEntities クラスは、Entity Framework で保持され、データベース内の各テーブルのコレクション プロパティを公開します。 弊社のデータベース内のすべてのジャンルを取得する、StoreController のインデックスの操作を更新してみましょう。 以前この文字列のデータをハード コーディングしていました。 今すぐお代わりにだけ使用できます、Entity Framework コンテキスト Generes コレクション。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

変更をまだだけを返しているライブ データ データベースから今すぐ前に、返されるお同じ StoreIndexViewModel を返しているので、ビュー テンプレートに発生する必要はありません。

プロジェクトをもう一度実行して、「/格納」url、データベースにすべてジャンルの一覧が表示おされますようになりました。

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>ストアを参照および更新の詳細をライブ データを使用するには

ストア/参照しますか? ジャンル =*[一部ジャンル]* 、アクション メソッドを検索するジャンル名でします。 のみが予定 1 つの結果、同じジャンル名に対して 2 つのエントリはならないことがあるため、および使用できるようにします。次のように適切なジャンル オブジェクトに対するクエリを LINQ で Single() 拡張機能 (入力せずにこのまだ)。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

1 つのメソッドは、その名前が定義した値と一致するよう、単一のジャンル オブジェクトすることを指定するパラメーターとしてラムダ式を受け取ります。 上記の例では、Disco と一致する名前値の 1 つのジャンル オブジェクトを読み込んでおいます。

使用すると、ジャンル オブジェクトを取得するときにも読み込まれたたい他の関連エンティティを示すために Entity Framework 機能を移動します。 この機能は、クエリ結果の整形が呼び出され、すべての必要な情報を取得するデータベースにアクセスする必要があります回数を削減することにより、します。 更新、クエリに関連するアルバムもすることを示すために Genres.Include("Albums") から含まれるように取得、ジャンルのアルバムをプリフェッチすることができます。 これは、1 つのデータベースの要求で、ジャンル、およびアルバムの両方のデータが取得されますのでより効率的です。

場所には、説明を次に、更新された参照コント ローラー アクションの外観を示します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

各ジャンルの使用可能なアルバムを表示するストア参照ビューを更新できます。 ビューのテンプレート (内で見つかった/Views/Store/Browse.cshtml) を開き、次に示すように、アルバムの箇条書きリストを追加します。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

このアプリケーションを実行し、ストア/[参照] を参照しますか? ジャンル = 結果が、選択したジャンルのすべてのアルバムを表示する、データベースからプルするようになりましたことジャズを示しています。

![](mvc-music-store-part-4/_static/image2.jpg)

しましょう同じ、/Store 詳細/[id] URL に変更し、ID を持つパラメーター値に一致するアルバムに読み込み、データベース クエリで、ダミーのデータを置き換えます。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

アプリケーションを実行して、/Store/Details/1 を参照して、結果がデータベースからプルするようになりましたことを示します。

![](mvc-music-store-part-4/_static/image5.png)

これで、ストアの詳細ページがセットアップでアルバム ID で、アルバムを表示するを更新してみましょう、**参照**詳細ビューへのリンクを表示します。 ストアのインデックスからリンク前のセクションの最後にストアを参照することとまったく同じ状態 Html.ActionLink を使用します。 [参照] ビューの完全なソースは、以下が表示されます。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

ストア ページで、使用可能なアルバムを表示するには、ジャンルのページを参照できないできましたおよびアルバムをクリックするとは、そのアルバムの詳細を表示できます。

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-3.md)
> [次へ](mvc-music-store-part-5.md)
