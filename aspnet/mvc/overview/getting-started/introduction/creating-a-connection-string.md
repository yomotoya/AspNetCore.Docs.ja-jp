---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 接続文字列を作成し、SQL Server LocalDB 協力 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: edbd46ef8a03670f0cb7527142babe9bd5846c7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB 協力
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB 協力

`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。 1 つに質問する可能性がありますには、接続先データベースを指定する方法です。 使用するデータベースを指定することも実際には、Entity Framework を使用するのには既定で[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)です。 このセクションで明示的に追加の接続文字列、 *Web.config*アプリケーションのファイルです。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) SQL Server Express データベース エンジンの要求時に開始され、ユーザー モードで実行される軽量バージョンです。 LocalDB は、SQL Server Express データベースを対象として使用することができますの特殊な実行モードで実行*.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダーです。

SQL Server Express は、実稼働 web アプリケーションでの使用は推奨されません。 LocalDB 具体的には指定しないで web アプリケーションと運用環境のため、IIS を使用するものはありません。 ただし、SQL Server または SQL Azure に LocalDB データベースを簡単に移行できます。

Visual Studio 2017、LocalDB は既定では Visual Studio と共にインストールされます。

既定では、Entity Framework は、オブジェクト コンテキスト クラスと同じ名前の接続文字列の検索 (`MovieDBContext`このプロジェクトの)。 詳細については、次を参照してください。 [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)です。

アプリケーションのルートを開く*Web.config*次に示すファイル。 (されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。

![](creating-a-connection-string/_static/image1.png)

検索、`<connectionStrings>`要素。

![](creating-a-connection-string/_static/image2.png)

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

2 つの接続文字列は、非常に似ています。 最初の接続文字列が名前付き`DefaultConnection`メンバーシップ データベースがアプリケーションにアクセスできるユーザーを制御するために使用されます。 という名前の LocalDB データベースを指定する接続文字列を追加した*Movie.mdf*にある、*アプリ\_データ*フォルダーです。 メンバーシップ、認証およびセキュリティの詳細については、このチュートリアルで、メンバーシップ データベースを使用して、チュートリアルを参照してくださいおされません[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。

接続文字列の名前の名前に一致する必要があります、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラスです。

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

実際にでは、追加する必要はありません、`MovieDBContext`接続文字列。 Entity Framework が LocalDB データベースを作成するユーザーのディレクトリの完全修飾名で接続文字列を指定しない場合、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス (このケースで`MvcMovie.Models.MovieDBContext`)。 できる任意の名前をデータベースが必要ながある限り、*です。MDF*サフィックス。 たとえば、データベースの名前おでした*MyFilms.mdf*です。

新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-model.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
