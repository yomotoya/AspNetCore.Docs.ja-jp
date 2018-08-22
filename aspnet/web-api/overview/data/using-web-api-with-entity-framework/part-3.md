---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Code First Migrations を使用して、データベースのシードを |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 533d3a6e9a69584fb2523b2c0a604755e372f031
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829270"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Code First Migrations を使用して、データベースのシード
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは使用して[Code First Migrations](https://msdn.microsoft.com/data/jj591621)テスト データでデータベースをシードする EF でします。

**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](part-3/samples/sample1.cmd)]

このコマンドは、プロジェクトに移行をという名前のフォルダーとという名前の Migrations フォルダーに Configuration.cs コード ファイルを追加します。

![](part-3/_static/image1.png)

Configuration.cs ファイルを開きます。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](part-3/samples/sample2.cs)]

次のコードを追加し、 **Configuration.Seed**メソッド。

[!code-csharp[Main](part-3/samples/sample3.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](part-3/samples/sample4.cmd)]

最初のコマンドは、データベースを作成するコードを生成し、2 番目のコマンドは、そのコードを実行します。 データベースがローカルで作成されるを使用して[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)します。

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>(省略可能) の API を確認します。

F5 キーを押して、デバッグ モードでアプリケーションを実行します。 Visual Studio では、IIS Express を起動し、web アプリを実行します。 Visual Studio は、ブラウザーを起動し、アプリのホーム ページを開きます。

Visual Studio で web プロジェクトを実行すると、ポート番号が割り当てられます。 次の図では、ポート番号は、50524 です。 アプリケーションを実行するときに、別のポート番号を確認します。

![](part-3/_static/image3.png)

ホーム ページは、ASP.NET MVC を使用して実装されます。 ページの上部にある"API"というリンクがあります。 このリンクを自動生成されたヘルプ ページに web API に表示されます。 (このヘルプ ページを生成する方法と、ページを独自のドキュメントを追加する方法については、次を参照してください[ASP.NET Web API のヘルプ ページを作成する](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)。リンクをクリックして ヘルプ ページを要求と応答の形式を含む、API の詳細を確認します。

![](part-3/_static/image4.png)

この API は、データベースに対する CRUD 操作を使用します。 次に、API の概要を示します。

| Authors |  |
| --- | -- |
| Api/作成者します。 | すべての著者を取得します。 |
| GET api の作成者/{id} | ID で作成者を取得します。 |
| 投稿の作成者/api | 新しい作成者を作成します。 |
| PUT /api/authors/{id} | 既存の作成者を更新します。 |
| DELETE /api/authors/{id} | 作成者を削除します。 |

| ブック |  |
| --- | -- |
| GET /api/books | すべてのブックを取得します。 |
| GET /api/books/{id} | ID での書籍を入手します。 |
| Api/ブックを投稿します。 | 新しいブックを作成します。 |
| PUT /api/books/{id} | 既存のブックを更新します。 |
| DELETE /api/books/{id} | ブックを削除します。 |

## <a name="view-the-database-optional"></a>(省略可能) データベースを表示します。

EF がデータベースを作成しと呼ばれる、Update-database コマンドを実行したときに、`Seed`メソッド。 EF を使用して、アプリケーションをローカルで実行するときに[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)します。 Visual Studio でデータベースを表示することができます。 **ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー**します。

![](part-3/_static/image5.png)

**サーバーへの接続**ダイアログで、**サーバー名**編集ボックスに、"(localdb) \v11.0"を入力します。 ままに、**認証**として [Windows 認証] オプション。 **[接続]** をクリックします。

![](part-3/_static/image6.png)

Visual Studio では、LocalDB に接続し、SQL Server オブジェクト エクスプ ローラー ウィンドウで、既存のデータベースを示しています。 EF を作成したテーブルを表示するノードを展開することができます。

![](part-3/_static/image7.png)

データを表示するには、テーブルを右クリックして**ビュー データ**します。

![](part-3/_static/image8.png)

次のスクリーン ショットは、ブックの「テーブルの結果を示しています。 EF がデータをシードとの表に、使用してデータベースを設定することに注意してくださいには、Authors テーブルへの外部キーが含まれています。

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [前へ](part-2.md)
> [次へ](part-4.md)
