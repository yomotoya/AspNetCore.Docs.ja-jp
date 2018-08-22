---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: 属性ルーティングで ASP.NET Web API 2 で REST API の作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: da3ca8f89f823fcb2c4ab74af6ddf4f61d4e663a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825947"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 で属性ルーティングで REST API を作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

新しい型をサポートする web API 2 のルーティングと呼ばれる*属性ルーティング*します。 属性ルーティングの一般的な概要については、次を参照してください。[属性は、Web API 2 でルーティング](attribute-routing-in-web-api-2.md)します。 このチュートリアルではルーティングを使用して属性のブックの「コレクションの REST API を作成します。 API は、次の操作をサポートします。

| アクション | URI の例 |
| --- | --- |
| すべての書籍の一覧を取得します。 | /api/books |
| ID での書籍を入手します。 | /api/books/1 |
| 書籍の詳細を取得します。 | /api/books/1/details |
| ジャンルによる書籍の一覧を取得します。 | /api/books/fantasy |
| 発行日によって、書籍の一覧を取得します。 | /api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form) |
| 特定の作成者によって書籍の一覧を取得します。 | /api/authors/1/books |

すべてのメソッドは、読み取り専用 (HTTP GET 要求) です。

データ層に Entity Framework を使用します。 書籍のレコードには、次のフィールドがあります。

- ID
- Title
- ジャンル
- 発行日
- 価格
- 説明
- 著者 (Authors テーブルへの外部キー) の Id

ほとんどの要求に対してただし、API では返します (タイトル、作成者、およびジャンル) は、このデータのサブセットです。 完全なレコードをクライアント要求を取得する`/api/books/{id}/details`します。

## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional または Enterprise edition。

## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio を実行して開始します。 **ファイル**メニューの **新規**選び**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトに名前を&quot;BooksAPI&quot;します。

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。 [フォルダーを追加およびコアの参照] を選択、 **Web API**チェック ボックスをオンします。 クリックして**プロジェクトを作成する**します。

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

これは、Web API の機能が構成されているスケルトン プロジェクトを作成します。

### <a name="domain-models"></a>ドメイン モデル

次に、ドメイン モデル クラスを追加します。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 選択**追加**を選択し、**クラス**します。 クラスに `Author` という名前を付けます。

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

次のように Author.cs 内のコードに置き換えます。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

という名前の別のクラスを追加するようになりました`Book`します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

この手順では、データ層として Entity Framework を使用する Web API コント ローラーを追加します。

Ctrl キーと Shift キーを押しながら B キーを押して、プロジェクトをビルドします。 Entity Framework では、リフレクションを使用して、コンパイル済みのアセンブリ、データベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**を選択し、**コント ローラー**します。

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

**スキャフォールディングの追加**ダイアログ ボックスで、"Web API 2 コント ローラーと読み取り/書き込みアクションでは、Entity Framework を使用します"。

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

**コント ローラーの追加**ダイアログ ボックスの**コント ローラー名**、入力&quot;BooksController&quot;します。 選択、&quot;非同期コント ローラー アクションを使用して、&quot;チェック ボックスをオンします。 **モデル クラス**、&quot;帳&quot;します。 (表示されない場合、`Book`クラスが、ドロップダウン リストに表示、プロジェクトをビルドするかどうかを確認してください)。「+」ボタンをクリックします。

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

クリックして**追加**で、**新しいデータ コンテキスト**ダイアログ。

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

クリックして**追加**で、**コント ローラーの追加**ダイアログ。 スキャフォールディングがという名前のクラスを追加します`BooksController`API コント ローラーを定義します。 という名前のクラスも追加`BooksAPIContext`Models フォルダーでの Entity Framework データ コンテキストを定義します。

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>データベースをシードします。

ツール メニューで、次のように選択します。**ライブラリ パッケージ マネージャー**、し、**パッケージ マネージャー コンソール**します。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

このコマンドは、Migrations フォルダーを作成し、Configuration.cs をという名前の新しいコード ファイルを追加します。 このファイルを開き、次のコードを追加、`Configuration.Seed`メソッド。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

これらのコマンドは、ローカル データベースを作成し、データベースを設定するシード メソッドを呼び出します。

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>DTO クラスを追加します。

これでアプリケーションを実行して、/api/books/1 に GET 要求を送信して、応答は次のような検索します。 (読みやすくするためのインデントが追加されます)。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

代わりに、この要求のフィールドのサブセットを返すにします。 作成者 ID ではなく、著者の名を返すさせたいことも、 コント ローラー メソッドの戻り値に変更します。 これを実現する、*データ転送オブジェクト*(DTO) EF モデルではなく。 DTO は、データを伝達するだけに設計されているオブジェクトです。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** | **新しいフォルダー**します。 フォルダーの名前&quot;Dto&quot;します。 という名前のクラスを追加`BookDto`次の定義で、Dto フォルダー。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

という名前の別のクラスを追加`BookDetailDto`します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

次に、更新、`BooksController`を返すクラス`BookDto`インスタンス。 使用して、 [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)メソッドをプロジェクトに`Book`インスタンスを`BookDto`インスタンス。 コント ローラー クラスの更新されたコードを次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> 削除し、 `PutBook`、 `PostBook`、および`DeleteBook`メソッド、このチュートリアルで必要としないためです。


これでアプリケーションを実行し、/api/books/1 を要求した場合これよう、応答本文になります。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>ルート属性を追加します。

次に、属性ルーティングを使用するコント ローラーを変換します。 最初に、追加、 **RoutePrefix**属性をコント ローラーにします。 この属性は、このコント ローラー上のすべてのメソッドの最初の URI セグメントを定義します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

追加し、 **[ルート]** 次のように、コント ローラー アクションに属性します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

各コント ローラー メソッドのルート テンプレートは、プレフィックスと、文字列に指定されて、**ルート**属性。 `GetBook`メソッド、ルート テンプレートには、パラメーター化された文字列が含まれています。 &quot;{0} id: int}&quot;、URI セグメントには、整数値が含まれている場合に一致します。

| メソッド | ルート テンプレート | URI の例 |
| --- | --- | --- |
| `GetBooks` | "api/books" | `http://localhost/api/books` |
| `GetBook` | "api/books/{id:int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>書籍の詳細を取得します。

書籍の詳細を取得するクライアントへの GET 要求を送信`/api/books/{id}/details`ここで、 *{id}* 書籍の ID です。

`BooksController` クラスに次のメソッドを追加します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

要求した場合`/api/books/1/details`、次のような応答。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>ジャンルでブックを取得します。

書籍の一覧には、特定のジャンルを取得するクライアントへの GET 要求を送信`/api/books/genre`ここで、*ジャンル*ジャンルの名前を指定します。 (たとえば、`/api/books/fantasy`)。

次のメソッドを追加`BooksController`します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

ここで、URI テンプレート内の {ジャンル} パラメーターを含むルート定義しています。 Web API がこれら 2 つの Uri を識別し、別のメソッドにルーティングできることに注意してください。

`/api/books/1`

`/api/books/fantasy`

だ、`GetBook`メソッドに"id"セグメントは整数値である必要があります、制約が含まれています。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

/Api/books/fantasy を要求すると、応答はようになります。

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>ブックの作成者を取得します。

に特定の作成者の、書籍の一覧を取得するクライアントは、への GET 要求を送信`/api/authors/id/books`ここで、 *id*作成者の ID です。

次のメソッドを追加`BooksController`します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

興味深いは、この例ため&quot;ブックの「&quot;はの子リソースを扱う&quot;作成者&quot;します。 このパターンは、RESTful Api で非常に一般的です。

ルート テンプレートにティルダ (~) のオーバーライドでは、そのルート プレフィックス、 **RoutePrefix**属性。

## <a name="get-books-by-publication-date"></a>発行日によってブックを取得します。

発行日、書籍の一覧を取得するクライアントへの GET 要求を送信`/api/books/date/yyyy-mm-dd`ここで、 *- yyyy-mm-dd*日です。

これを行う 1 つの方法を次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}`パラメーターが一致するように制約を**DateTime**値。 これは、動作より実際より制限の少ない。 たとえば、これらの Uri は、ルートと一致も。

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

これらの Uri を許可することに問題があります。 ただし、ルート テンプレートに正規表現の制約を追加することで、ルートを特定の形式に制限できます。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

形式で日付を今すぐのみ&quot;- yyyy-mm-dd&quot;は一致します。 実際の日付を入手したことを検証する正規表現を使用しない点に注意してください。 Web API に URI セグメントに変換しようとするときに処理する、 **DateTime**インスタンス。 無効な日付では、' 2012 などの 47-99' は、変換に失敗し、クライアントには、404 エラーが発生します。

スラッシュ区切り記号をサポートすることも (`/api/books/date/yyyy/mm/dd`) 別の操作を追加することによって **[ルート]** 属性はさまざまな正規表現を使用します。

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

若干の重要な詳細をここでは。 2 つ目のルート テンプレートがワイルドカード文字 (\*) {pubdate} パラメーターの先頭。

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

これは、ように、ルーティング エンジンで指示 {pubdate} は、URI の残りの部分と一致する必要があります。 既定では、テンプレート パラメーターは 1 つの URI セグメントと一致します。 この場合は、{pubdate} にいくつかの URI セグメントをまたがるします。

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>コント ローラー コード

BooksController クラスの完全なコードを次に示します。

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>まとめ

属性ルーティングでは、管理性と柔軟性、API の Uri を設計するときにします。
