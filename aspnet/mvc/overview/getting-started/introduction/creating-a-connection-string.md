---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 接続文字列を作成し、SQL Server LocalDB の使用 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 73a3149c8c789221baa2e3804ebf15ed5a509ddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825959"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB の使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>接続文字列を作成し、SQL Server LocalDB の使用

`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。 疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。 使用するデータベースを指定することも実際には、Entity Framework は既定で使用する[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)します。 このセクションでは明示的に追加します内の接続文字列、 *Web.config*アプリケーションのファイル。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)は、SQL Server Express データベース エンジンの要求時に開始し、ユーザー モードで実行される軽量バージョンです。 LocalDB は SQL Server Express データベースとして使用することができますの特殊な実行モードで実行 *.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダー。

SQL Server Express は、実稼働 web アプリケーションでの使用は推奨されません。 LocalDB 特に使わないで web アプリケーションを運用環境のため、IIS を使用するものではありません。 ただし、SQL Server または SQL Azure に LocalDB データベースを簡単に移行できます。

Visual Studio 2017 では、LocalDB は、Visual Studio では既定でインストールされます。

既定では、Entity Framework は、オブジェクト コンテキスト クラスと同じ名前の接続文字列の検索 (`MovieDBContext`このプロジェクトの)。 詳細については、次を参照してください。 [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)します。

アプリケーションのルートを開く*Web.config*次に示すファイル。 (いない、 *Web.config*ファイル、*ビュー*フォルダー)。

![](creating-a-connection-string/_static/image1.png)

検索、`<connectionStrings>`要素。

![](creating-a-connection-string/_static/image2.png)

次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

2 つの接続文字列は、非常に似ています。 最初の接続文字列が名前付き`DefaultConnection`し、アプリケーションにアクセスできるユーザーを制御するメンバーシップ データベースを使用します。 追加した接続文字列をという名前の LocalDB データベースを指定します*Movie.mdf*内にある、*アプリ\_データ*フォルダー。 メンバーシップ、認証およびセキュリティの詳細については、このチュートリアルでは、メンバーシップ データベースを使用して、拙著のチュートリアルを参照してくださいしません[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。

接続文字列の名前の名前に一致する必要があります、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス。

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

実際に追加する必要はありません、`MovieDBContext`接続文字列。 Entity Framework がユーザーのディレクトリの完全修飾名に LocalDB データベースを作成、接続文字列を指定しない場合、 [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)クラス (ここで`MvcMovie.Models.MovieDBContext`)。 ことがデータベースの名前、好きなデザインがある限り、*します。MDF*サフィックス。 たとえば、データベースを名前*MyFilms.mdf*します。

新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。

> [!div class="step-by-step"]
> [前へ](adding-a-model.md)
> [次へ](accessing-your-models-data-from-a-controller.md)
