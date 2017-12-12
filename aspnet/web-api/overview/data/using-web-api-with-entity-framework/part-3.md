---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Code First Migrations を使用して、データベースのシードを |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Code First Migrations を使用して、データベースのシードを
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](https://github.com/MikeWasson/BookService)

このセクションでは、使用[Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621)をデータベースにテスト データをプレシードできる EF にします。

**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](part-3/samples/sample1.cmd)]

このコマンドは、プロジェクトに移行をという名前のフォルダーと、Migrations フォルダーでされる Configuration.cs をという名前のコード ファイルを追加します。

![](part-3/_static/image1.png)

される Configuration.cs ファイルを開きます。 次の追加**を使用して**ステートメントです。

[!code-csharp[Main](part-3/samples/sample2.cs)]

次のコードを追加、 **Configuration.Seed**メソッド。

[!code-csharp[Main](part-3/samples/sample3.cs)]

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](part-3/samples/sample4.cmd)]

最初のコマンドは、データベースを作成するコードを生成し、2 番目のコマンドは、そのコードを実行します。 データベースがローカルで作成されるを使用して[LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx)です。

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>(省略可能) API を探索します。

F5 キーを押してアプリケーションをデバッグ モードで実行します。 Visual Studio では、IIS Express を起動し、web アプリを実行します。 Visual Studio は、ブラウザーを起動し、アプリのホーム ページが開きます。

Visual Studio web プロジェクトの実行時にポート番号を割り当てます。 次の図では、ポート番号は、50524 です。 アプリケーションを実行するときに、別のポート番号が表示されます。

![](part-3/_static/image3.png)

ホーム ページは、ASP.NET MVC を使用して実装されます。 ページの上部には、"API"と表示されているリンクがあります。 このリンクを自動生成されたヘルプ ページに web API の表示されます。 (このヘルプ ページを生成する方法、およびページに、独自のドキュメントを追加する方法については、次を参照してください[ASP.NET Web API のヘルプ ページを作成する](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)。リンクをクリックして [ヘルプ] ページ、要求と応答の形式を含む、API の詳細を確認します。

![](part-3/_static/image4.png)

この API は、データベースでの CRUD 操作を使用します。 API を以下にまとめます。

| 作成者 |  |
| --- | -- |
| Api/作成者します。 | すべての著者を取得します。 |
| GET api/作成者/{id} | ID で作成者を取得します。 |
| POST api/作成者 | 新しい作成者を作成します。 |
| PUT/api の作成者/{id} | 既存の作成者を更新します。 |
| /Api の作成者/{id} の削除します。 | 作成者を削除します。 |

| ブック |  |
| --- | -- |
| /Api/books を取得します。 | すべてのブックを取得します。 |
| /Api ブック/{id} の取得します。 | ID でブックを取得します。 |
| Api/ブックを投稿します。 | 新しいブックを作成します。 |
| PUT/api ブック/{id} | 既存のブックを更新します。 |
| /Api ブック/{id} の削除します。 | ブックを削除します。 |

## <a name="view-the-database-optional"></a>(省略可能) のデータベースを表示します。

EF がデータベースを作成しと呼ばれる Update-database コマンドを実行したときに、`Seed`メソッドです。 EF を使用してアプリケーションをローカルで実行するときに[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)です。 Visual Studio でデータベースを表示することができます。 **ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー**です。

![](part-3/_static/image5.png)

**サーバーへの接続**ダイアログで、**サーバー名**編集ボックスに、"(localdb) \v11.0"を入力します。 ままにして、**認証**「Windows 認証」では、オプションです。 **[接続]**をクリックします。

![](part-3/_static/image6.png)

Visual Studio では、LocalDB に接続し、SQL Server オブジェクト エクスプ ローラー ウィンドウで、既存のデータベースを示しています。 EF を作成したテーブルを表示するノードを展開できます。

![](part-3/_static/image7.png)

表示するには、データ テーブルを右クリックし **ビュー データ**です。

![](part-3/_static/image8.png)

次のスクリーン ショットは、ブックのテーブルの結果を示します。 EF に、データベースにデータをシードと、テーブルが設定されている通知には、Authors テーブルへの外部キーが含まれています。

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[前へ](part-2.md)
[次へ](part-4.md)
