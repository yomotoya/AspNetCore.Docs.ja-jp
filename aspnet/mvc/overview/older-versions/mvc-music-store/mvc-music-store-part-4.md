---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'パート 4: モデルとデータ アクセス |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルとデータ アクセスについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402039"
---
<a name="part-4-models-and-data-access"></a>パート 4: モデルとデータ アクセス
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。
> 
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルとデータ アクセスについて説明します。


これまでにしただけされて渡して「ダミー データ」、コント ローラーからテンプレートを表示します。 これで、実際のデータベースをフックする準備ができました。 このチュートリアルでのテーマは、SQL Server Compact Edition (SQL CE とよく呼ばれます) を使用する方法として、データベース エンジン。 SQL CE は、任意のインストールや構成は、ローカルの開発は非常に便利ですが不要の無料の埋め込み、ファイルのベース データベースです。

## <a name="database-access-with-entity-framework-code-first"></a>データベースへのアクセスに Entity Framework Code First

クエリを実行し、データベースを更新する ASP.NET MVC 3 プロジェクトに含まれている Entity Framework (EF) のサポートを使用します。 EF は、柔軟なオブジェクト リレーショナル マッピング (ORM) データ開発者は、オブジェクト指向の手法で、データベースに保存データのクエリと更新 API です。

Entity Framework バージョン 4 では、code first と呼ばれる開発パラダイムをサポートします。 Code first では、単純なクラスとも呼ばれる POCO ("plain-old"CLR object から) を作成してモデル オブジェクトを作成することができ、クラスからその場でデータベースを作成することもします。

### <a name="changes-to-our-model-classes"></a>モデル クラスへの変更

私たちを利用する Entity Framework でのデータベース作成機能このチュートリアルでは。 その前に、モデル クラスには、後で使用するものを追加するいくつかの軽微な変更を作成、みましょう。

#### <a name="adding-the-artist-model-classes"></a>アーティストのモデル クラスを追加します。

アルバムに関連付けるアーティスト、ため、アーティストを記述する単純なモデル クラスを追加します。 次に示すコードを使用して Artist.cs という名前の Models フォルダーに新しいクラスを追加します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>モデル クラスを更新しています

アルバム クラスは、次に示すように更新します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

次に、ジャンル クラスに次の更新プログラムを加えます。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>アプリの追加\_データ フォルダー

アプリを追加します\_データ ディレクトリ、SQL Server Express のデータベース ファイルを保持するために、プロジェクトにします。 アプリ\_データが既にデータベースへのアクセスの適切なセキュリティ アクセス許可のある ASP.NET の特殊なディレクトリ。 [プロジェクト] メニューから選択し、ASP.NET フォルダーの追加、アプリ\_データ。

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Web.config ファイルで接続文字列の作成

Entity Framework は、データベースに接続する方法を認識できるように、web サイトの構成ファイルに数行を追加します。 プロジェクトのルートにある Web.config ファイルをダブルクリックします。

![](mvc-music-store-part-4/_static/image2.png)

このファイルの一番下までスクロールし、追加、 &lt;connectionStrings&gt;次に示すように、最後の行の真上セクションします。

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>コンテキスト クラスの追加

Models フォルダーを右クリックし、MusicStoreEntities.cs をという名前の新しいクラスを追加します。

![](mvc-music-store-part-4/_static/image3.png)

このクラスは、Entity Framework データベース コンテキストを表すとは、作成を処理、読み取り、更新、および削除操作をします。 このクラスのコードは、以下に示します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

これで他の構成、特殊なインターフェイスなどはありません。DbContext 基底クラスを拡張することによって、MusicStoreEntities クラスは、私たちにとって、データベース操作を処理できません。 フックするが、これより多くのプロパティをデータベースにいくつかの追加情報を利用するモデル クラスに追加してみましょう。

### <a name="adding-our-store-catalog-data"></a>ストア カタログ データを追加します。

「シード」のデータを新しく作成したデータベースに追加する Entity Framework の機能を活用しましたかかります。 ジャンル、アーティスト、アルバムの一覧で、ストアのカタログが事前設定されます。 -これは、このチュートリアルで前に使用される、サイト設計ファイルが含まれている - MvcMusicStore Assets.zip ダウンロードには、コードをという名前のフォルダー内にある、このシード データをクラス ファイルがあります。

コード内で、Models フォルダーが SampleData.cs ファイルを探し、次に示すようにこのプロジェクトで、Models フォルダーにドロップします。

![](mvc-music-store-part-4/_static/image4.png)

その SampleData クラスを Entity Framework コードの 1 つの行を追加する必要があります。 開き、アプリケーションを先頭に次の行を追加するプロジェクトのルートでは、Global.asax ファイルをダブルクリックして\_メソッドを開始します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

この時点で、プロジェクトの Entity Framework を構成するために必要な作業が終了しました。

## <a name="querying-the-database"></a>データベースに対するクエリの実行

ここで「ダミー データ」を使用する代わりに代わりに呼び出すようにすべての情報を照会するマイクロソフトのデータベースに、StoreController を更新しましょう。 フィールドを宣言することから始めます、 **StoreController** storeDB という名前の MusicStoreEntities クラスのインスタンスを保持します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>データベースにクエリのストア インデックスの更新

MusicStoreEntities クラスは Entity Framework によって保持され、データベース内の各テーブルのコレクション プロパティを公開します。 データベース内のすべてのジャンルを取得する、StoreController の Index アクションを更新してみましょう。 以前これを行う文字列データをハード コーディングします。 代わりにだけを使えます Entity Framework コンテキスト Generes コレクション。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

だけを返しているライブ データ データベースから今すぐ前に、返された同じ StoreIndexViewModel まま返しているので、ビュー テンプレートで発生する変更が不要です。

プロジェクトをもう一度実行して、"/store"の URL を参照してください。 データベースにすべてのジャンルの一覧が表示されますようになりました。

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>ストアの参照や詳細情報を使用して、ライブ データを更新しています

ストア/参照で? ジャンル =*[一部ジャンル]* 、アクション メソッドを検索するジャンルを名前でします。 のみが期待 1 つの結果、これまでジャンルの同名の 2 つのエントリがあるべきではないため、使用できるようにします。このような適切なジャンル オブジェクトのクエリを LINQ で Single() 拡張機能 (入力せずにこのまだ)。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

1 つのメソッドは、その名前が定義した値と一致するよう、1 つのジャンル オブジェクトすることを指定するパラメーターとしてラムダ式を受け取ります。 上記のケースで Disco を一致する名前値の 1 つのジャンル オブジェクト読み込みです。

ジャンル オブジェクトを取得するときにも読み込まれたたいその他の関連エンティティを指定できるようにする Entity Framework 機能をご説明しましょう。 この機能は、クエリ結果のシェイプは呼び出され、により、すべての必要な情報を取得するデータベースにアクセスする必要があります回数の合計を小さくこと。 関連アルバムもすることを示す Genres.Include("Albums") からは、前述のクエリを更新するために、アルバム、ジャンルを取得しますをプリフェッチします。 これは、1 つのデータベースの要求で、ジャンルとアルバムの両方のデータを取得するためより効率的です。

説明を次の更新された参照のコント ローラー アクションの外観に示します。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

各ジャンルにあるアルバムを表示するストア参照ビューを更新できます。 ビュー テンプレート (内で見つかった/Views/Store/Browse.cshtml) を開き、次に示すように、アルバムの箇条書きを追加します。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

アプリケーションの実行、/ストア/参照に移動するとしますか? ジャンル = 結果は、選択されたジャンルのすべてのアルバムを表示する、データベースからプルされるようになりましたこと Jazz を示しています。

![](mvc-music-store-part-4/_static/image2.jpg)

同じ、/Store/詳細/[id] URL に変更し、ダミー データ ID を持つパラメーター値に一致するアルバムに読み込み、データベース クエリを置き換えることを行います。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

アプリケーションの実行、/Store/Details/1 に移動して、結果がデータベースから取得されるようになりましたことを示します。

![](mvc-music-store-part-4/_static/image5.png)

できたので、ストアの詳細ページは、アルバムの ID で、アルバムを表示する設定は、更新、**参照**詳細ビューへのリンクを表示します。 Html.ActionLink、ストアのインデックスから、前のセクションの末尾にストアを参照するリンクを扱ったように正確に使用します。 [参照] ビューの完全なソースは、以下が表示されます。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

ジャンル ページで、使用可能なアルバムの一覧を [ストア] ページから参照できないできましたし、アルバムをクリックしてそのアルバムの詳細を表示できます。

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-3.md)
> [次へ](mvc-music-store-part-5.md)
