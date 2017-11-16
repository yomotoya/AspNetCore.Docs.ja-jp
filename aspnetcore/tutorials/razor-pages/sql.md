---
title: "SQL Server LocalDB と ASP.NET Core の使用"
author: rick-anderson
description: "SQL Server LocalDB と ASP.NET Core の使用について説明します。"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC, SQL, LocalDB"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1e35ce49980bf026de35359cdf413961051e3bee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>SQL Server LocalDB と ASP.NET Core の使用

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette) 

`MovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。 データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

ASP.NET Core の[構成](xref:fundamentals/configuration)システムは `ConnectionString` を読み取ります。 ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

テストまたは実稼働サーバーにアプリを配置する場合は、環境変数または別の方法を使用して、実際の SQL Server に接続文字列を設定できます。 詳細については、[構成](xref:fundamentals/configuration)に関するページを参照してください。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。 LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。 既定では、LocalDB データベースは *C:/Users/\<user\>* ディレクトリに "\*.mdf" ファイルを作成します。

<a name="ssox"></a>
* **[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。

  ![[View] メニュー](sql/_static/ssox.png)

* `Movie` テーブルを右クリックし、**[デザイナーの表示]** を選択します。

  ![Movie テーブルで開かれたコンテキスト メニュー](sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](sql/_static/dv.png)

`ID` の横のキー アイコンに注意してください。 既定では、EF で主キーに `ID` という名前のプロパティが作成されます。

* `Movie` テーブルを右クリックし、**[データの表示]** を選択します。

  ![開いた Movie テーブルにテーブル データが表示されています](sql/_static/vd22.png)

## <a name="seed-the-database"></a>データベースのシード

*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。 生成されたコードを次のコードに置き換えます。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>シード初期化子の追加

次のように、*Program.cs* ファイルで `Main` メソッドの末尾にシード初期化子を追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

アプリのテスト

* DB 内のすべてのレコードを削除します。 これはブラウザーの削除リンクで行うか、[SSOX](xref:tutorials/razor-pages/new-field#ssox) から行うことができます。
* アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。 強制的に初期化するには、IIS Express を停止してから再起動する必要があります。 これは次の方法のいずれかを使用して行うことができます。

  * 通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。

    ![IIS Express システム トレイ アイコン](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](sql/_static/stopIIS.png)

   * 非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。
   * デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。
   
アプリにシードされたデータが表示されます。

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

次のチュートリアルでは、データの表示をクリーンアップします。

>[!div class="step-by-step"]
[前: スキャフォールディングされた Razor ページ](xref:tutorials/razor-pages/page)
[次: ページの更新](xref:tutorials/razor-pages/da1)
