---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server データベースの更新 - 12 の 11 を展開する |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2018b58207365a0a3829f1f359d46d765aad0a77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831979"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server データベースの更新 - 12 の 11 を展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Windows Azure Web サイトをデプロイする方法を示しますチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、完全な SQL Server データベースにデータベースの更新を展開する方法を示します。 Code First Migrations は、データベースの更新のすべての作業を行う、ため、プロセスの SQL Server Compact で行った場合とほぼ同じ、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアル。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="adding-a-new-column-to-a-table"></a>テーブルに新しい列の追加

このチュートリアルのこのセクションで変更をデータベースと対応するコードを変更し、テストおよび運用環境に展開するための準備で Visual Studio でテストを行います。 変更は、追加、`OfficeHours`列を`Instructor`エンティティとの新しい情報を表示する、 **Instructors** web ページ。

ContosoUniversity.DAL プロジェクトで開きます*Instructor.cs*間で、次のプロパティを追加し、`HireDate`と`Courses`プロパティ。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

テスト データを含む新しい列のシードを設定するために、初期化子クラスを更新します。 開いている*migrations \configuration.cs*と置換を開始するコード ブロック`var instructors = new List<Instructor>`と新しい列を含む次のコード ブロック。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

ContosoUniversity プロジェクトで開きます*Instructors.aspx*の業務時間終了直前に新しいテンプレート フィールドを追加および`</Columns>`最初のタグ`GridView`コントロール。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

ソリューションをビルドします。

開く、**パッケージ マネージャー コンソール**ウィンドウ、および select ContosoUniversity.DAL として、**既定のプロジェクト**します。

次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

アプリケーションを実行し、選択、 **Instructors**ページ。 ページでは読み込むには、Entity Framework がデータベースを再作成し、テスト データのシードを設定するために通常より少し長くなります。

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>データベースの更新をテスト環境に展開します。

Code First Migrations を使用する場合に SQL Server データベースの変更を展開するためのメソッドと同じ SQL Server Compact です。 ただし、テストを変更する必要があるため、SQL Server Compact から SQL Server に移行するよう設定されても、発行プロファイル。

最初の手順は、前のチュートリアルで作成した接続文字列の変換を削除します。 これらが不要になったため、発行プロファイルの接続文字列の変換を指定ように構成する前に行ったよう、**パッケージ化/発行 SQL** SQL Server への移行 タブ。

開く、 *Web.Test.config*ファイルし、削除、`connectionStrings`要素。 残り変換だけで、 *Web.Test.config*ファイルは、`Environment`値、`appSettings`要素。

今すぐテスト環境には、発行プロファイルと発行を更新できます。

開く、 **Web の発行**ウィザード、および、スイッチ、**プロファイル**タブ。

選択、**テスト**プロファイルを公開します。

選択、**設定**タブ。

クリックして**発行機能の向上、新しいデータベースを有効にする**します。

接続文字列 ボックスで**SchoolContext**で使用した同じ値を入力、 *Web.Test.config*前のチュートリアルでは変換ファイル。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。 (Visual Studio のバージョンによって、チェック ボックスをラベル可能性があります**適用の Code First Migrations**)。

接続文字列 ボックスで**DefaultConnection**で使用した同じ値を入力、 *Web.Test.config*前のチュートリアルでは変換ファイル。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

ままに**Update database**オフにします。

**[発行]** をクリックします。

Visual Studio では、コードの変更をテスト環境に展開し、Contoso University のホーム ページをブラウザーで開きます。

Instructors ページを選択します。

アプリケーションは、このページを実行すると、データベースにアクセスしようとします。 Code First Migrations は、データベースが現在があり、最新の移行がまだ適用されていないいる検索かどうかを確認します。 Code First Migrations は、最新の移行を適用する、実行、`Seed`メソッド、し、ページが正常に実行します。 シードされたデータで新しい Office 時間列が参照してください。

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>運用環境にデータベースの更新を展開します。

また、運用環境の発行プロファイルを変更する必要があります。 この場合、既存のプロファイルを削除し、更新された .publishsettings ファイルをインポートして、新しく作成します。 更新されたファイルは、Cytanium で SQL Server データベースの接続文字列が含まれます。

内の接続文字列の変換が不要になったテスト環境にデプロイしたときに、学習したように、 *Web.Production.config*変換ファイル。 開くファイルを削除、`connectionStrings`要素。 残りの変換は、`Environment`値、`appSettings`要素と`location`Elmah エラー レポートにアクセスを制限する要素。

運用環境用の新しい発行プロファイルを作成する前に、更新された .publishsettings ファイルをダウンロード前に行ったのと同様、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。 (Cytanium コントロール パネル で、 **Websites**、順にクリックします、 **contosouniversity.com** web サイト。 選択、 **Web 公開**タブをクリックし、をクリックし、**この web サイトの発行プロファイルのダウンロード**)。これを実行する理由は、.publishsettings ファイル内のデータベース接続文字列を取得します。 初めておらず、SQL Server データベース Cytanium にまだ作成して SQL Server Compact まだ使っていたため、ファイルのダウンロードを使用可能な接続文字列がありませんでした。

今すぐ、運用環境に発行プロファイルと発行を更新できます。

開く、 **Web の発行**ウィザード、および、スイッチ、**プロファイル**タブ。

クリックして**プロファイルの管理**、実稼働プロファイルを削除します。

閉じる、 **Web の発行**ウィザードでこの変更を保存します。

開く、 **Web の発行**クリックしてウィザードをもう一度、**インポート**します。

**接続** タブで、変更**送信先 URL**一時的な URL を使用している場合は、適切な値にします。

**[次へ]** をクリックします。

**設定**] タブで [**発行機能の向上、新しいデータベースを有効にする**します。

接続文字列のドロップダウン リストで**SchoolContext**、Cytanium 接続文字列を選択します。

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

選択**実行 Code First migrations (アプリケーションの起動時に実行)** します。

接続文字列のドロップダウン リストで**DefaultConnection**、Cytanium 接続文字列を選択します。

選択、**プロファイル** タブで、をクリックして**プロファイルの管理**、"Production"を"contosouniversity.com - Web Deploy"から、プロファイルの名前を変更します。

変更を保存する発行プロファイルを閉じてからもう一度開きます。

**[発行]** をクリックします。 (実際の運用 web サイトでコピーする*アプリ\_offline.htm*を本番環境と put、パブリッシュする前に、プロジェクト フォルダーで削除して、デプロイが完了します)。

Visual Studio では、コードの変更をテスト環境に展開し、Contoso University のホーム ページをブラウザーで開きます。

Instructors ページを選択します。

Code First Migrations は、テスト環境では、データベースを更新します。 シードされたデータで新しい Office 時間列が参照してください。

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

正常に配置されましたデータベースの変更を含むアプリケーションの更新プログラム、SQL Server データベースを使用しています。

## <a name="more-information"></a>説明

これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションを展開する方法のチュートリアルを完了します。 これらのチュートリアルで取り上げるトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN web サイト。

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な貢献を行った以下の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson、データ プラットフォームの開発の MVP、United States
- Harsh Mittal、Microsoft
- [Kristina Olson、Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教皇、Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava、Microsoft
- [Raffaele Rialdi (イタリア)](http://www.iamraf.net/)
- [Rick Anderson、Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [次へ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
