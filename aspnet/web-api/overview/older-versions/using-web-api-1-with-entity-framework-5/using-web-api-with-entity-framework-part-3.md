---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'パート 3: 管理コント ローラーを作成する |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: caf7649f2e420b4063966fd1a371c3d9a5eb4d04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815458"
---
<a name="part-3-creating-an-admin-controller"></a>パート 3: 管理コント ローラーを作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>管理者コント ローラーを追加します。

このセクションでは、CRUD をサポートする Web API コント ローラーを追加します (作成、読み取り、更新、および削除) の製品に操作します。 コント ローラーでは、データベース層との通信に Entity Framework を使用します。 管理者だけはこのコント ローラーを使用することになります。 顧客は、別のコント ローラーで、製品にアクセスします。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**し**コント ローラー**します。

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー`AdminController`します。 **テンプレート**、&quot;を Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー&quot;します。 **モデル クラス**、"Product (ProductStore.Models)"を選択します。 **データ コンテキスト**を選択します"&lt;新しいデータ コンテキスト&gt;"。

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> 場合、**モデル クラス**ドロップダウンは、モデル クラスを表示しないため、プロジェクトをコンパイルするかどうかを確認します。 Entity Framework は、コンパイルされたアセンブリが必要があるため、リフレクションを使用します。


選択すると"&lt;新しいデータ コンテキスト&gt;"が開き、**新しいデータ コンテキスト**ダイアログ。 データ コンテキストの名前を付けます`ProductStore.Models.OrdersContext`します。

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

をクリックして**OK**を閉じる、**新しいデータ コンテキスト**ダイアログ。 **コント ローラーの追加**ダイアログ ボックスで、をクリックして**追加**します。

プロジェクトに追加内容を次に示します。

- という名前のクラス`OrdersContext`から派生した**DbContext**します。 このクラスは、POCO モデルとデータベース間の結び付きを提供します。
- という名前の Web API コント ローラー`AdminController`します。 このコント ローラーで CRUD 操作をサポートする`Product`インスタンス。 使用して、 `OrdersContext` Entity Framework と通信するクラス。
- Web.config ファイルで新しいデータベース接続文字列。

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

OrdersContext.cs ファイルを開きます。 コンス トラクターが、データベース接続文字列の名前を指定することに注意してください。 この名前は、Web.config に追加された接続文字列を指します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

`OrdersContext` クラスに次のプロパティを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet**クエリを実行できるエンティティのセットを表します。 完全な一覧を次に示します、`OrdersContext`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController`クラスは、基本的な CRUD 機能を実装する 5 つのメソッドを定義します。 各メソッドは、クライアントが呼び出すことができる URI に対応します。

| コント ローラー メソッド | 説明 | URI | HTTP メソッド |
| --- | --- | --- | --- |
| GetProducts | すべての製品を取得します。 | api/製品 | GET |
| GetProduct | ID によって製品を検索します。 | api/products/*id* | GET |
| PutProduct | 製品を更新します。 | api/products/*id* | PUT |
| PostProduct | 新しい製品を作成します。 | api/製品 | POST |
| DeleteProduct | 製品を削除します。 | api/products/*id* | Del |

各メソッドの呼び出しに`OrdersContext`データベース クエリを実行します。 (PUT、POST、および DELETE) のコレクションを変更するメソッドを呼び出す`db.SaveChanges`データベースに変更を確定します。 コント ローラーは HTTP 要求ごとに作成され、メソッドが戻る前に、変更を保持する必要があるため、破棄します。

## <a name="add-a-database-initializer"></a>データベースの初期化子を追加します。

Entity Framework では、スタートアップ時に、データベースを設定し、モデルが変更されるたびに自動的にデータベースを再作成できる便利な機能があります。 この機能は、モデルを変更した場合でも、テスト データが常にあるので、開発時に便利です。

ソリューション エクスプ ローラーで Models フォルダーを右クリックし、という名前の新しいクラスを作成する`OrdersContextInitializer`します。 次の実装に貼り付けます。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

継承することによって、 **DropCreateDatabaseIfModelChanges**クラスようモデル クラスが変更されるたびに、データベースを削除する Entity Framework を示しています。 Entity Framework を作成します (または再作成されます)、データベース呼び出し、**シード**テーブルを設定するメソッド。 使用して、**シード**メソッドをいくつかの例の製品と注文の例を追加します。

この機能は、テストに優れていますが、使用しないでください、 **DropCreateDatabaseIfModelChanges**クラス、および運用環境でモデル クラスが変更された場合、データが失われる可能性があります。

次に、Global.asax を開くし、次のコードを追加、**アプリケーション\_開始**メソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>コント ローラーに要求を送信します。

この時点では、任意のクライアント コードでは、まだ作成されていないが、web などのツール、web ブラウザーまたは、HTTP デバッギングを使用して API を呼び出すことができます[Fiddler](http://www.fiddler2.com/fiddler2/)します。 Visual Studio で f5 キーを押してデバッグを開始します。 Web ブラウザーが開いて、`http://localhost:*portnum*/`ここで、*させる*いくつかのポート番号です。

HTTP 要求を送信"`http://localhost:*portnum*/api/admin`します。 Entify フレームワークを作成し、データベースをシードする必要があるために、最初の要求が、完了に時間がかかる可能性があります。 応答のように、次のとおりです。

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-2.md)
> [次へ](using-web-api-with-entity-framework-part-4.md)
