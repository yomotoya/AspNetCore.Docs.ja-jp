---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'パート 3: 作成、管理コント ローラー |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870545"
---
<a name="part-3-creating-an-admin-controller"></a>パート 3: 作成、管理コント ローラー
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>管理コント ローラーを追加します。

このセクションでは、CRUD をサポートする Web API コント ローラーが追加されます (作成、読み取り、更新、および削除) の製品に操作します。 コント ローラーでは、Entity Framework をデータベース層との通信に使用します。 管理者のみがこのコント ローラーを使用することができます。 顧客は、製品を別のコント ローラーからアクセスします。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**し**コント ローラー**です。

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー`AdminController`です。 **テンプレート** &quot;Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー&quot;です。 **モデル クラス**、"Product (ProductStore.Models)"を選択します。 **データ コンテキスト**"&lt;新しいデータ コンテキスト&gt;"です。

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> 場合、**モデル クラス**ドロップダウンはすべてのモデル クラスを表示しないため、プロジェクトをコンパイルするかどうかを確認します。 Entity Framework は、ため、コンパイルされたアセンブリが必要があります、リフレクションを使用します。


選択すると"&lt;新しいデータ コンテキスト&gt;"が開き、**新しいデータ コンテキスト**ダイアログ。 データ コンテキストの名前を付けます`ProductStore.Models.OrdersContext`です。

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

をクリックして**OK**を消去する、**新しいデータ コンテキスト**ダイアログ。 **コント ローラーの追加**ダイアログ ボックスで、をクリックして**追加**です。

プロジェクトに追加された新機能が次に示します。

- という名前のクラス`OrdersContext`から派生した**DbContext**です。 このクラスは、POCO モデルとデータベース間接着剤を提供します。
- という名前の Web API コント ローラー`AdminController`です。 このコント ローラーでの CRUD 操作をサポートしている`Product`インスタンス。 使用して、 `OrdersContext` Entity Framework と通信するクラス。
- Web.config ファイルに新しいデータベース接続文字列。

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

OrdersContext.cs ファイルを開きます。 コンス トラクターが、データベース接続文字列の名前を指定することに注意してください。 この名前は、Web.config に追加された接続文字列を参照します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

`OrdersContext` クラスに次のプロパティを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet**問い合わせることができるエンティティのセットを表します。 ここでは、完全な一覧で、`OrdersContext`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController`クラスは、基本的な CRUD 機能を実装する 5 つのメソッドを定義します。 各メソッドは、クライアントが呼び出すことのできる URI に対応しています。

| コント ローラー メソッド | 説明 | URI | HTTP メソッド |
| --- | --- | --- | --- |
| GetProducts | すべての製品を取得します。 | api/products | GET |
| GetProduct | ID によって製品を検索します。 | api/products/*id* | GET |
| PutProduct | 製品を更新します。 | api/products/*id* | PUT |
| PostProduct | 新しい製品を作成します。 | api/products | POST |
| DeleteProduct | 製品を削除します。 | api/products/*id* | Del |

各メソッドの呼び出しに`OrdersContext`データベースを照会します。 (PUT、POST、および DELETE) のコレクションを変更するメソッドを呼び出す`db.SaveChanges`データベースに変更を保持します。 コント ローラーは HTTP 要求ごとに作成され、メソッドから返される前に、変更を永続化する必要があるため、その後破棄します。

## <a name="add-a-database-initializer"></a>データベース初期化子を追加します。

Entity Framework では、スタートアップ時に、データベースを設定し、モデルが変更されるたびに自動的にデータベースを再作成できる便利な機能があります。 この機能は、モデルを変更した場合でも、テスト データが常にあるため、開発では便利です。

ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前の新しいクラスを作成する`OrdersContextInitializer`です。 次の実装に貼り付けます。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

継承することによって、 **DropCreateDatabaseIfModelChanges**クラスを指示することに Entity Framework モデル クラスは変更されるたびに、データベースを削除します。 Entity Framework を作成 (または再作成)、データベース呼び出し、**シード**テーブルに挿入する方法です。 使用して、**シード**メソッドをいくつかの例の製品と使用例注文を追加します。

この機能は、テストの多くが使用しない、 **DropCreateDatabaseIfModelChanges**モデル クラスを変更した場合、データが失われる可能性がありますので、実稼働環境でクラスです。

次に、Global.asax を開くし、次のコードを追加、**アプリケーション\_開始**メソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>コント ローラーに要求を送信します。

この時点で、すべてのクライアント コードでは、まだ作成されていないが、web API、web ブラウザーや、HTTP デバッグを使用してツールなどを呼び出すことができます[Fiddler](http://www.fiddler2.com/fiddler2/)です。 Visual Studio で f5 キーを押してデバッグを開始します。 Web ブラウザーで開かれます`http://localhost:*portnum*/`ここで、*させる*いくつかのポート番号です。

HTTP 要求を送信"`http://localhost:*portnum*/api/admin`です。 最初の要求は、Entify フレームワークを作成し、データベースのシードする必要があるためにを完了に時間がかかることがあります。 応答には、以下のように、次の必要があります。

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-2.md)
> [次へ](using-web-api-with-entity-framework-part-4.md)
