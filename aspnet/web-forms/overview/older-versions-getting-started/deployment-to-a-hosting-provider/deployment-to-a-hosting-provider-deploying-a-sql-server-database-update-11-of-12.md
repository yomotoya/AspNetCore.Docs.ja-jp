---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server データベースの更新 - 11/12 を展開する |。Microsoft ドキュメント
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887331"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server データベースの更新 - 11/12 を展開する。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示しています、および Windows Azure Web サイトを展開する方法を示しますをチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、SQL Server データベースの完全にデータベースの更新を展開する方法を示します。 Code First Migrations は、データベースの更新のすべての作業を行う、ため、プロセスは SQL Server Compact での作業した内容とほぼ同じ、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアルです。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="adding-a-new-column-to-a-table"></a>テーブルに新しい列を追加します。

チュートリアルのこのセクションで、データベースの変更と対応するコードの変更、その後、テスト環境や実稼働環境に展開するための準備で Visual Studio でテストを作ります。 変更は、追加、`OfficeHours`列を`Instructor`エンティティとの新しい情報を表示する、**講習においてインストラクター** web ページ。

ContosoUniversity.DAL プロジェクトで開きます*Instructor.cs*間で、次のプロパティを追加し、`HireDate`と`Courses`プロパティ。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

テスト データを含む新しい列のシードを設定するように、初期化子のクラスを更新します。 開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`を新しい列を含む次のコード ブロックで。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

ContosoUniversity プロジェクトで開きます*Instructors.aspx* office 時間の終了の直前に新しいテンプレート フィールドを追加および`</Columns>`、最初のタグ`GridView`コントロール。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

ソリューションをビルドします。

開く、 **Package Manager Console**ウィンドウ、および select ContosoUniversity.DAL として、**既定のプロジェクト**です。

次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

アプリケーションの実行を選択して、**講習においてインストラクター**ページ。 ページが読み込むには、Entity Framework は、データベースを再作成され、テスト データのシードを設定するために通常よりも少し長くかかります。

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>データベースの更新をテスト環境に展開します。

Code First Migrations を使用して、SQL Server にデータベースの変更を展開するためのメソッドは、同じ SQL Server Compact です。 ただし、テストを変更する必要があるがまだ設定して SQL Server Compact から SQL Server に移行するために、プロファイルを発行します。

最初の手順では、前のチュートリアルで作成した接続文字列の変換を削除します。 これらは不要になったため、発行プロファイルで接続文字列の変換を指定ように構成する前に実行した方法で、**パッケージ化/発行 SQL** SQL Server への移行 タブ。

開く、 *Web.Test.config*ファイルし、削除、`connectionStrings`要素。 内でのみの残りの変換、 *Web.Test.config*用のファイルは、`Environment`値で、`appSettings`要素。

今すぐテスト環境に発行プロファイルと発行を更新できます。

開く、 **Web の発行**ウィザード、しに切り替えると、**プロファイル**タブです。

選択、**テスト**プロファイルを発行します。

選択、**設定**タブです。

をクリックして**の機能強化を公開する新しいデータベースを有効にする**です。

接続文字列 ボックスで**SchoolContext**で使用したものと同じ値を入力、 *Web.Test.config*前のチュートリアルでの変換ファイル。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

選択**実行 Code First Migrations (アプリケーション開始時に実行されます)** です。 (チェック ボックスにラベルを付けることがあります、バージョンの Visual Studio で**適用の Code First Migrations**)。

接続文字列 ボックスで**DefaultConnection**で使用したものと同じ値を入力、 *Web.Test.config*前のチュートリアルでの変換ファイル。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

ままにして**更新データベース**オフにします。

**[発行]** をクリックします。

Visual Studio では、コードの変更をテスト環境に展開し、Contoso 大学のホーム ページにブラウザーを開きます。

講習においてインストラクター ページを選択します。

アプリケーションがこのページを実行すると、データベースにアクセスしようとします。 Code First Migrations は、データベースが最新では、検索を最新の移行がまだ適用されていないかどうかを確認します。 Code First Migrations 最新の移行を適用する場合は、実行、`Seed`メソッド、し、ページが正常に実行します。 シードのデータの新しい勤務時間列を参照してください。

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>データベースの更新を実稼働環境に展開します。

また、運用環境の発行プロファイルを変更する必要があります。 ここでは、既存のプロファイルの削除を更新された .publishsettings ファイルをインポートして新しいものを作成します。 更新されたファイルには、Cytanium で SQL Server データベースの接続文字列が含まれます。

接続文字列の変換が不要になったテスト環境に展開したときに説明したとおり、 *Web.Production.config*変換ファイル。 ファイルし、削除を開く、`connectionStrings`要素。 残りの変換は、`Environment`値で、`appSettings`要素および`location`Elmah エラー レポートにアクセスを制限する要素。

実稼働環境用の新しい発行プロファイルを作成する前に、更新された .publishsettings ファイルをダウンロード前に実行したのと同様、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。 (Cytanium コントロール パネル で、 **Web サイト**、をクリックし、 **contosouniversity.com** web サイトです。 Select、 **Web 公開** タブをクリックして**この web サイトの発行プロファイルのダウンロード**)。これを行う理由は、.publishsettings ファイル内のデータベース接続文字列を取得します。 まだを使用していた SQL Server Compact しておらず、SQL Server データベース Cytanium にまだ作成しているので、ファイルをダウンロードする最初の時間を使用可能な接続文字列がありませんでした。

今すぐ、運用環境に発行プロファイルと発行を更新できます。

開く、 **Web の発行**ウィザード、しに切り替えると、**プロファイル**タブです。

をクリックして**プロファイルの管理**、実稼働プロファイルを削除します。

閉じる、 **Web の発行**ウィザードにこの変更を保存します。

開く、 **Web の発行**クリックしてウィザードをもう一度、**インポート**です。

**接続** タブで、変更**送信先 URL**一時的な URL を使用している場合は、適切な値にします。

**[次へ]** をクリックします。

**設定** タブで、をクリックして**の機能強化を公開する新しいデータベースを有効にする**です。

接続文字列 ドロップダウン リストに**SchoolContext**、Cytanium 接続文字列を選択します。

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

選択**実行 Code First migrations (アプリケーション開始時に実行されます)** です。

接続文字列 ドロップダウン リストに**DefaultConnection**、Cytanium 接続文字列を選択します。

選択、**プロファイル** タブで、をクリックして**プロファイルの管理**、"Production"に"contosouniversity.com - Web Deploy"から、プロファイルの名前を変更します。

発行プロファイルを変更を保存するを閉じてから、もう一度開きます。

**[発行]** をクリックします。 (コピーする実際の運用 web サイト、*アプリ\_offline.htm*と put の実稼働環境に発行する前に、プロジェクト フォルダーでを削除して、展開が完了するとします)。

Visual Studio では、コードの変更をテスト環境に展開し、Contoso 大学のホーム ページにブラウザーを開きます。

講習においてインストラクター ページを選択します。

Code First Migrations は、テスト環境で、同じ方法、データベースを更新します。 シードのデータの新しい勤務時間列を参照してください。

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

今すぐが正常に展開したデータベースの変更が含まれているアプリケーションの更新プログラム、SQL Server データベースを使用します。

## <a name="more-information"></a>説明

これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションの配置に関するチュートリアルを完了します。 これらのチュートリアルで説明されているトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN web サイトです。

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson、データ プラットフォームの開発 MVP、United States
- 過酷 Mittal、Microsoft
- [Kristina Olson、Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教皇、Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava、Microsoft
- [Raffaele Rialdi (イタリア)](http://www.iamraf.net/)
- [Rick Anderson、Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott ハンター、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [次へ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
