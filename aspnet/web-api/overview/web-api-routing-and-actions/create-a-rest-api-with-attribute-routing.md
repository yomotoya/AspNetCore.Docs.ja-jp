---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "属性のルーティングが ASP.NET Web API 2 の REST API の作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 9ecc233e595716a167ad800a0a21a6162b051648
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 のルーティングの属性を持つ REST API を作成します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

新しい型をサポートする web API 2 のルーティングでは、次のように呼び出されます。*属性がルーティング*です。 属性のルーティングの一般的な概要については、次を参照してください。 [Web API 2 での属性のルーティング](attribute-routing-in-web-api-2.md)です。 このチュートリアルではルーティングを使用して属性をブックのコレクション用の REST API を作成します。 この API は、次の操作がサポートされます。

| 操作 | URI の例 |
| --- | --- |
| すべての書籍の一覧を取得します。 | api/ブック |
| ID でブックを取得します。 | /api/books/1 |
| 書籍の詳細を取得します。 | /api/books/1/details |
| ジャンルの書籍の一覧を取得します。 | /api/books/fantasy |
| 公開日によって書籍の一覧を取得します。 | /api/books/date/2013-02-16/api/books/date/2013/02/16 (代替フォーム) |
| 特定の作成者によって著された書籍の一覧を取得します。 | /api/authors/1/books |

メソッドはすべて読み取り専用 (HTTP GET 要求) です。

データ層は、Entity Framework を使用します。 書籍のレコードが必要で、次のフィールドがあります。

- ID
- タイトル
- ジャンル
- 公開日
- 価格
- 説明
- 著者の Id (Authors テーブルへの外部キー)

ほとんどの要求のただし、API 戻ります (タイトル、作成者、およびジャンル) は、このデータのサブセット。 完全なレコードをクライアントに要求を取得する`/api/books/{id}/details`です。

## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional または Enterprise edition。

## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio を実行して開始します。 **ファイル**メニューの **新規**し、**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトに名前を&quot;BooksAPI&quot;です。

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。 「フォルダーの追加し、コアの参照」を選択、 **Web API**チェック ボックスをオンします。 をクリックして**プロジェクトを作成する**です。

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

これは、Web API の機能が構成されているスケルトンのプロジェクトを作成します。

### <a name="domain-models"></a>ドメイン モデル

次に、ドメイン モデル クラスを追加します。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 選択**追加**選択してから、**クラス**です。 クラスに `Author` という名前を付けます。

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Author.cs でコードを次のように置き換えます。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

という名前の別のクラスを今すぐ追加`Book`です。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

このステップでは、データ層として Entity Framework を使用する Web API コント ローラーを追加します。

Ctrl キーと Shift キーを押しながら B キーを押して、プロジェクトをビルドします。 Entity Framework では、リフレクションを使用して、コンパイルされたアセンブリをデータベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**選択してから、**コント ローラー**です。

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

**追加 Scaffold**ダイアログで、"Web API 2 Entity Framework を使用してコント ローラーを読み取り/書き込みアクションがある"。

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

**コント ローラーの追加**ダイアログ ボックスの**コント ローラー名**、入力&quot;BooksController&quot;です。 選択、&quot;非同期コント ローラー アクションを使用して&quot;チェック ボックスをオンします。 **モデル クラス** &quot;Book&quot;です。 (表示されない場合、`Book`クラスも、ドロップダウン リストに表示されて、プロジェクトを作成することを確認してください)。「+」ボタンをクリックします。

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

をクリックして**追加**で、**新しいデータ コンテキスト**ダイアログ。

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

をクリックして**追加**で、**コント ローラーの追加**ダイアログ。 スキャフォールディングがという名前のクラスを追加`BooksController`API コント ローラーを定義します。 という名前のクラスも追加`BooksAPIContext`Models フォルダーに Entity Framework のデータ コンテキストを定義します。

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>データベースをシードします。

ツール メニューから選択**ライブラリ パッケージ マネージャー**、し、 **Package Manager Console**です。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

このコマンドは、Migrations フォルダーを作成し、される Configuration.cs をという名前の新しいコード ファイルを追加します。 このファイルを開き、次のコードを追加、`Configuration.Seed`メソッドです。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

これらのコマンドは、ローカル データベースを作成し、データベースを設定したシード メソッドを呼び出します。

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>DTO クラスを追加します。

今すぐアプリケーションを実行して/api/books/1 に GET 要求を送信すると、応答は次のように検索します。 (読みやすくするためのインデントが追加されます)。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

代わりに、この要求のフィールドのサブセットを返す必要があります。 また、作成者 ID ではなく、作成者の名前を返す目的します。 コント ローラー メソッドの戻り値を変更してこれを実現する、*データ転送オブジェクト*(DTO) EF モデルではなくです。 DTO は、データを受け渡すためだけに設計されているオブジェクトです。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加** | **新しいフォルダー**です。 名前のフォルダー &quot;DTOs&quot;です。 という名前のクラスを追加`BookDto`DTOs フォルダーを次の定義に。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

という名前の別のクラスを追加`BookDetailDto`です。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

次に、更新、`BooksController`を返すクラス`BookDto`インスタンス。 使用して、 [Queryable.Select](https://msdn.microsoft.com/en-us/library/system.linq.queryable.select.aspx)プロジェクトにメソッド`Book`にインスタンス`BookDto`インスタンス。 コント ローラー クラスの更新されたコードを次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> 削除しました、 `PutBook`、 `PostBook`、および`DeleteBook`メソッドは、このチュートリアルで必要となるためです。


これでアプリケーションを実行し、/api/books/1 を要求した場合、応答本文が次のようになります。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>ルート属性を追加します。

次に、属性のルーティングを使用するコント ローラーに変換します。 最初に、追加、 **RoutePrefix**属性をコント ローラーにします。 この属性は、このコント ローラー上のすべてのメソッドの最初の URI セグメントを定義します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

追加し、 **[ルート]**コント ローラーのアクションを次のように属性します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

各コント ローラー メソッドのルート テンプレートは、プレフィックスとで指定された文字列、**ルート**属性。 `GetBook`メソッド、ルート テンプレートには、パラメーター化された文字列が含まれています。 &quot;{id: int}&quot;、URI セグメントには、整数値が含まれている場合に一致します。

| メソッド | ルート テンプレート | URI の例 |
| --- | --- | --- |
| `GetBooks` | "api/books" | `http://localhost/api/books` |
| `GetBook` | "api/ブック/{id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>書籍の詳細を取得します。

書籍の詳細を取得するには、クライアントがへの GET 要求を送信`/api/books/{id}/details`ここで、 *{id}*書籍の id を指定します。

`BooksController` クラスに次のメソッドを追加します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

要求した場合は`/api/books/1/details`、次のような応答。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>ジャンルでブックを取得します。

特定のジャンルの書籍の一覧を取得する、クライアントへの GET 要求を送信`/api/books/genre`ここで、*ジャンル*ジャンルの名前を指定します。 (たとえば、`/get/books/fantasy`)。

次のメソッドを追加`BooksController`です。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

ここで、URI テンプレートで {ジャンル} パラメーターを含むルートは定義します。 Web API がこれら 2 つの Uri を区別し、別のメソッドにルーティングできることに注意してください。

`/api/books/1`

`/api/books/fantasy`

これはため、`GetBook`メソッドには、"id"セグメントは整数値である必要がある、制約が含まれています。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

/Api/books/fantasy を要求すると、応答は次のようになります。

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>ブックの作成者の取得します。

に特定の作成者の、書籍の一覧を取得する、クライアントは、への GET 要求を送信`/api/authors/id/books`ここで、 *id*著者の id を指定します。

次のメソッドを追加`BooksController`です。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

興味深いは、この例ため&quot;ブック&quot;はの子リソースを扱う&quot;作成者&quot;です。 このパターンは、RESTful Api で非常に一般的です。

ルート テンプレートでは、チルダ (~) のオーバーライドでは、そのルート プレフィックス、 **RoutePrefix**属性。

## <a name="get-books-by-publication-date"></a>公開日によってブックを取得します。

公開日で書籍の一覧を取得する、クライアントへの GET 要求を送信`/api/books/date/yyyy-mm-dd`ここで、 *yyyy mm dd*日です。

これを行う 1 つの方法を次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}`パラメーターが一致するように制約、 **DateTime**値。 これは、動作は、ようよりも実際にはさらに小さくします。 たとえば、これらの Uri はまた、ルートを一致します。

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

これらの Uri の許可に問題があります。 ただし、ルート テンプレートを正規表現の制約を追加することで、ルートを特定の形式に制限できます。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

形式で日付を今すぐのみ&quot;yyyy mm dd&quot;は一致します。 実際の日付を取得したことを検証する正規表現を使用しないことに注意してください。 Web API への URI セグメントに変換しようとするときに処理する、 **DateTime**インスタンス。 無効な日付など ' 2012-47-99' は、変換に失敗し、クライアントには、404 エラーが発生します。

スラッシュの区切り記号をサポートすることも (`/api/books/date/yyyy/mm/dd`) 別の操作を追加することによって**[ルート]**属性さまざまな正規表現を使用します。

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

微妙でありながら重要な詳細をここがあります。 2 番目のルート テンプレートにはワイルドカード文字 (\*) {pubdate} パラメーターの開始時。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

こうこと、ルーティング エンジン {pubdate} は、URI の残りの部分と一致する必要があります。 既定では、テンプレート パラメーターは、単一の URI セグメントと一致します。 この例では、{pubdate} を目的にいくつかの URI セグメントを します。

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>コント ローラー コード

BooksController クラスの完全なコードを次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>概要

属性ルーティングでは、管理性と柔軟性が向上して API への Uri を設計するとき。
