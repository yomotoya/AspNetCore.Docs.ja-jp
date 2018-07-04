---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: データベースを展開する (VB) |Microsoft Docs
author: rick-anderson
description: ASP.NET web アプリケーションを展開するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 Da. の
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dc5e9b4e189929b2b898b997b7577a623bdc8a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381323"
---
<a name="deploying-a-database-vb"></a>データベースを展開する (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> ASP.NET web アプリケーションを展開するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 データ ドリブン web アプリケーションには、データベース スキーマとデータが含まれます。 このチュートリアルでは、開発環境から運用環境にデータベースを正常に展開するために必要な手順について説明するシリーズの初回です。


### <a name="introduction"></a>はじめに

ASP.NET web アプリケーションを展開するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 過去の 6 つのチュートリアルの過程で書籍レビューの単純な web アプリケーションの展開について説明しました。 このデモ サイトが ASP.NET ページ、構成ファイルでは、サーバー側リソース数から成る、`Web.sitemap`イメージや CSS ファイルなどのファイルとクライアント側のリソースと一緒に - など。 しかし、web アプリケーションをデータ ドリブンのでしょうか。 どのような追加の手順は、データベースを使用する web アプリケーションを展開するためにする必要がありますか。

次のいくつかのチュートリアルでは、データ駆動型 web アプリケーションをデプロイするために必要な手順を対処します。 取得する方法、データベースのスキーマおよび内容、開発環境、運用環境に後続のチュートリアルは、必要な構成の変更中に調べることでこのチュートリアルを開始します。 次のこと (メンバーシップ、ロール、プロファイル、およびなど) は、アプリケーション サービスを使用するデータベースの展開の課題について説明します。

## <a name="examining-the-updated-book-reviews-web-application"></a>更新された書籍レビューの Web アプリケーションの検証

データ ドリブン web アプリケーションの展開を示すためには書籍レビューの web アプリケーションが簡単で静的な web サイトからデータに基づく 1 つを更新します。 前に、このチュートリアルのダウンロードに、アプリケーションの 2 つのバージョンがある: Web アプリケーション プロジェクト モデルと Web サイト プロジェクト モデルを使用する 1 つを使用する 1 つ。

更新された書籍レビューの web アプリケーションで使用する、 [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx)サイト %s に格納されているデータベース、`App_Data`フォルダー (`~/App_Data/Reviews.mdf`)。 SQL Server 2008 がコンピューターにインストールされている必要がある場合、デモは、エラーなし実行する必要があります。 以前のバージョンがある場合は、SQL Server のいずれかを実行できますが、無料の SQL Server 2008 Express Edition をインストールまたは自分でデータベースを作成するこのチュートリアルで使用可能なスクリプトをダウンロードするデータベースを使用することができます。

`Reviews.mdf`データベースには、4 つのテーブルが含まれています。

- `Genres` -各ジャンル、テクノロジ、Fiction、およびビジネスなどのレコードが含まれています。
- `Books` ような列を含む、レビューごとのレコードが含まれています`Title`、 `GenreId`、 `ReviewDate`、および`Review`、他のユーザーの間で。
- `Authors` -確認済みのブックに提供した各作成者に関する情報が含まれています。
- `BooksAuthors` -どのような作成者は、どのような書籍を執筆を指定する多対多結合テーブルです。
  

図 1 は、これら 4 つのテーブルの ER ダイアグラムを示します。


[![書籍レビューの Web アプリケーションのデータベースは 4 つのテーブルの構成](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**図 1**:、書籍レビューの Web アプリケーションのデータベースは 4 つのテーブルの構成 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image3.jpg))。


書籍レビューの web サイトの以前のバージョンでは、各書籍の個別の ASP.NET ページがありました。 たとえば、という名前のページが`~/Tech/TYASP35.aspx`のレビューに含まれている*教える自分で ASP.NET 3.5 in 24 時間*します。 この新しいデータ ドリブン バージョンを web サイトには、データベースと 1 つの ASP.NET ページ、Review.aspx?ID= に格納されているレビュー*bookId*、指定されたこの本のレビューが表示されます。 同様は、Genre.aspx?ID=*genreId*指定ジャンルの確認済みのブックを一覧表示されたページ。

図 2 および 3 つの表示、`Genre.aspx`と`Review.aspx`ページの動作。 各ページのアドレス バーの URL に注意してください。 図 2 it s Genre.aspx? ID = 85d164ba-1123-4 c 47-82a0-c8ec75de7e0e します。 85d164ba-1123-4c47-82a0-c8ec75de7e0e があるため、`GenreId`テクノロジのジャンル、「テクノロジのレビュー」ページの見出しの読み取りと箇条書きリストの値がこのジャンルに該当するサイトでのこれらのレビューを列挙します。


[![テクノロジのジャンル ページ](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**図 2**: The テクノロジ ジャンルのページ ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image6.jpg))。


[![に関するレビュー Teach Yourself ASP.NET 3.5 in 24 Hours](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**図 3**: のレビューを*教える自分で ASP.NET 3.5 in 24 時間*([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image9.jpg))。


書籍レビューの web アプリケーションには、管理者ことができます追加、編集、および削除ジャンル、レビュー、および情報を作成、管理セクションも含まれます。 現時点では、[管理] セクションのすべての利用者にアクセスできます。 今後のチュートリアルでユーザー アカウントのサポートを追加し、[管理] ページに承認されたユーザーのみが許可します。

ダウンロードする場合、書籍レビュー アプリケーションくださいの目的は、データ ドリブン アプリケーションの配置をデモンストレーションすることに注意しています。 アプリケーションの設計に関して言えば、ベスト プラクティスは発生しません。 たとえば、別のデータ アクセス層 (DAL); はありません。ASP.NET ページは、SqlDataSource コントロールまたはその分離コード クラスでの ADO.NET コードを使用して、データベースと直接通信します。 階層型アーキテクチャを使用してデータ駆動型アプリケーションの作成の詳細についてを参照してください、 [*データを扱う*チュートリアル](../../data-access/index.md)します。

## <a name="databases-on-development-versus-production"></a>運用環境と開発上のデータベース

データ ドリブン web アプリケーションの開発を開始すると、データベースに接続する方法については、アプリケーションの詳細を提供する、データベースの接続文字列を指定する必要があります。 この接続文字列など、データベース サーバー、データベース名、およびセキュリティ情報間を指定します。 開発時にアプリケーションによって使用されるデータベースのときに、データベースが使用されるものと異なる場合は、ほとんどの場合、運用環境で s。 開発と運用における異なるデータベースを使用する多くの利点があります。 Don t は異なる開発方法でデータベースを誤って変更またはライブ データを削除を心配する必要があります。 ダミーのテスト データに配置したり、実稼働環境でアプリケーションに与える影響について心配することがなくデータ モデルに重大な変更を加えることもできます。 データベースのスキーマにデータベースおよび関連する変更点が、増やすことの欠点別のデータベース開発および運用環境では、アプリケーションの場合に展開するか、データも展開する必要があります。

最初のデプロイの前に、データベースの 1 つだけのインスタンスがあるし、そのインスタンスが、開発環境では。 最初に実稼働環境にアプリケーションをデプロイするときに必要なサーバー側とクライアント側のファイルをコピーするだけでなく、開発環境から運用環境にデータベースをコピーすることもする必要があります。 これは、私たちの立場右側で、書籍レビューの web アプリケーション - とデータベースが存在するので、`App_Data`の開発環境でのフォルダーがされていない、実稼働環境にプッシュされていません。

アプリケーションをデプロイした後は、データベースのコピーが 2 つがあります。 アプリケーションに近づくにつれて、データ モデルへの変更を加えなくて、新機能を追加することがあります (既存のテーブルに新しい列を追加するなど、新しいテーブルを追加、既存の列に変更を加えるとなど)。 次に、web アプリケーションが展開されると、変更は、最後の配置を運用データベースに適用する必要があるため、開発環境でデータベースに適用されます。 今後のチュートリアルでは、このプロセスを管理するためのいくつかの方法が説明します。 このチュートリアルでは、運用環境に、開発環境からデータベース全体の展開について説明します。

## <a name="deploying-the-database-to-the-production-environment"></a>データベースを実稼働環境に展開します。

このチュートリアルの残りの部分は、開発環境から運用環境へのデータベースを展開する方法を検索します。 に沿ってをフォローしている場合、web ホスト プロバイダーを使用して、アカウントが Microsoft SQL Server が含まれているかどうかを確認するデータベースをサポートする必要です。 また、いくつかの情報、つまり、データベース サーバー名、データベース名、および、ユーザー名とパスワード、データベースに接続するために使用する必要があります。

このチュートリアルで前に書籍レビューの web サイトのデータベースは、SQL Server 2008 Express Edition データベースに格納されている、`App_Data`フォルダー。 このようなデータベースを展開することは、簡単なコピーを理由に、スタンバイは、`App_Data`フォルダーを開発環境から運用環境にします。 ただし、ほとんどの web ホスト プロバイダーがホストのデータベースをサポートして、`App_Data`セキュリティ上の理由のためのフォルダー。 代わりに、web ホストは、環境内の SQL Server データベース サーバーでアカウントを提供します。 開発環境から運用環境へのデータベースを配置するには、web ホストのデータベース サーバーに登録されているデータベースを取得する必要があります。

どのようになぜ、データベース、開発環境から運用環境にしますか。 どのようなサービス、web ホストのプランによってこれを実現するいくつかの方法はあります。 DiscountASP.NET などのいくつかのホストと、データベースまたは実際のバックアップを FTP できます`.mdf`web サイトにファイルし、[コントロール パネル] からバックアップ ファイルを復元またはアタッチ、`.mdf`ファイルを SQL Server データベース サーバー。 このようなツールではコピーだけです、データベースの配置、`App_Data`運用環境とし、コントロール パネルを使用して追加するフォルダー。 これは、おそらく、最初に、データベースをパブリッシュする最も簡単で最も簡単な方法です。

別の方法では、データベース パブリッシュ ウィザードを使用します。 データベース パブリッシュ ウィザードとは、そのテーブルで、-、テーブル、ストアド プロシージャ、ビュー、ユーザー定義関数、およびなどのデータベースのスキーマと、必要に応じて、データを作成する SQL コマンドを生成する Windows デスクトップ アプリケーションです。 SQL Server Management Studio を通じて web ホスト プロバイダーのデータベース サーバーに接続し、でき、運用上のデータベースを複製するには、このスクリプトを実行できます。 、、Web ホスト プロバイダーには、Microsoft の s がサポートしている場合、より高い[Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) 、あなたに代わってデータベース サーバーで自動的に実行されるデータベース公開ウィザードによって生成されたスクリプトがあることができます。 データベース パブリッシュ ウィザードでは、データベースのスキーマとデータを作成するスクリプトを生成するので、web ホスト プロバイダーは、アップロードのアタッチなどの機能を提供するかどうかに関係なく動作`.mdf`ファイル。

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>データベース スキーマおよびデータベース公開ウィザードを使用してデータを作成する SQL コマンドを生成します。

ブック_レビュー データベースを運用環境にデプロイするデータベース公開ウィザードを使用して行います s を使用できます。 Visual Studio 2008 を使用している場合、またはを超えて、Database Publishing Wizard は既にインストールされています。 Visual Studio 2005 を使用しているかどうかは、最初に必要[をダウンロードしてインストール](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)ウィザード。

Visual Studio を開きに移動し、`Reviews.mdf`データベース。 Visual Web Developer を使用している場合、データベース エクスプ ローラーに移動します。Visual Studio を使用している場合は、サーバー エクスプ ローラーを使用します。 図 4 は、 `Reviews.mdf` Visual Web Developer で データベース エクスプ ローラーでデータベース。 図 4 に示すよう、`Reviews.mdf`データベースはの 4 つのテーブル、次の 3 つのストアド プロシージャおよびユーザー定義関数で構成されます。


[![データベース エクスプ ローラーまたはサーバー エクスプ ローラーでデータベースを検索します。](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**図 4**: データベース エクスプ ローラーまたはサーバー エクスプ ローラーでデータベースを検索 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image12.jpg))。


データベース名を右クリックし、コンテキスト メニューから「プロバイダーにパブリッシュ」オプションを選択します。 データベース パブリッシュ ウィザードが起動 (図 5 を参照してください)。 スプラッシュ画面からの事前の横にあるをクリックします。


[![データベース パブリッシュ ウィザードのスプラッシュ スクリーン](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**図 5**: The データベース公開ウィザード スプラッシュ画面 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image15.jpg))。


ウィザードでは、2 番目の画面では、Database Publishing Wizard にアクセスできるデータベースの一覧し、選択したデータベース内のすべてのオブジェクトのスクリプトを作成するスクリプトにオブジェクトを選択するかを選択することができます。 適切なデータベースを選択し、「すべてのスクリプトは、選択したデータベースでオブジェクト」オプションをオンのままにします。

> [!NOTE]
> エラーが発生した場合"データベースにオブジェクトが存在しない*databaseName*このウィザードでスクリプトを作成できる型の"を図 6 に示す画面で [次へ] をクリックすると、データベース ファイルへのパスが長すぎると確認できます。 説明したとおり[このディスカッション項目](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014)データベース ファイルへのパスが長すぎる場合データベース パブリッシュ ウィザードの [プロジェクト] ページで、このエラーが発生することができます。


[![データベース パブリッシュ ウィザードのスプラッシュ スクリーン](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**図 6**: The データベース公開ウィザード スプラッシュ画面 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image18.jpg))。


次の画面からスクリプト ファイルを生成したり、web ホストがサポートしている場合は、web ホスト プロバイダーのデータベース サーバーに直接、データベースを発行できます。 図 7 に示すファイルに記述されたスクリプトが`C:\REVIEWS.MDF.sql`します。


[![ファイルにデータベースのスクリプトまたは Your Web ホスト プロバイダーに直接発行](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**図 7**: ファイルにデータベースのスクリプトを Web ホスト プロバイダーに直接公開したり ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image21.jpg))。


後続の画面には、スクリプト作成オプションのさまざまなことが要求されます。 スクリプトがこれらの既存のオブジェクトを削除する drop ステートメントを含めるかどうかを指定することができます。 既定値は true の場合、最初にデータベースをデプロイするときにこれは問題ありません。 ターゲット データベースが SQL Server 2000、SQL Server 2005、または SQL Server 2008 であるかどうかを指定することもできます。 最後に、スキーマとデータ、スクリプトを作成するかどうかを指定できますのデータまたはスキーマだけです。 スキーマは、データベース オブジェクト、テーブル、ストアド プロシージャ、ビュー、およびなどのコレクションです。 データは、テーブル内に存在する情報です。

図 8 に示すようには、SQL Server 2008 データベース用スクリプトの生成、スキーマとデータの両方を公開するために既存のデータベース オブジェクトを削除するように構成ウィザードがあります。


[![指定の発行オプション](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**図 8**: パブリッシング オプションを指定 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image24.jpg))。


最後の 2 つの画面は、実行すると、スクリプトのステータスを表示し、操作をまとめたものです。 ウィザードの実行の最終的な結果は、実稼働データベースを作成し、開発のと同じデータを追加するための SQL コマンドを含むスクリプト ファイルがあること。

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>実稼働環境のデータベースで SQL コマンドを実行します。

あるのでに残っているデータベースとそのデータを作成する SQL コマンドを含むスクリプトが、実稼働データベースでスクリプトを実行します。 一部の web ホスト プロバイダーは、データベースで実行する SQL コマンドを入力できますがコントロール パネルのテキスト ボックスを提供します。 非常に大きいスクリプト ファイルがあるかどうかは、このオプションは機能しない可能性があります (、 `REVIEWS.MDF.sql` 425 KB を超えるサイズは、スクリプト ファイル、インスタンスは)。

SQL Server Management Studio (SSMS) を使用して実稼働データベース サーバーに直接接続することをお勧めします。 非 Express エディションの SQL Server コンピューターにインストールした場合、可能性がありますが既にある SSMS をインストールします。 それ以外の場合、実行できます[をダウンロードしてインストール](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)SQL Server Management Studio Express Edition の無料コピーします。

SSMS を起動し、web ホスト プロバイダーによって提供される情報を使用して web ホストのデータベース サーバーに接続します。


[![Web ホスト プロバイダーのデータベース サーバーに接続します。](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**図 9**: Your Web ホスト プロバイダーのデータベース サーバーへの接続 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image27.jpg))。


[データベース] タブを展開し、データベースを探します。 ツールバーの左上隅にある 新しいクエリ ボタンをクリックします。 データベース公開ウィザードによって作成されたスクリプト ファイルから SQL コマンドに貼り付けますや実稼働データベース サーバーでこれらのコマンドを実行する実行 ボタンをクリックします。 スクリプト ファイルが非常に大きくなる場合、コマンドを実行するまでに数分かかる場合があります。


[![Web ホスト プロバイダーのデータベース サーバーに接続します。](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**図 10**: Your Web ホスト プロバイダーのデータベース サーバーへの接続 ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image30.jpg))。


すべてが s で終了です。 この時点で、開発用データベースが運用環境に複製されました。 SSMS でデータベースを更新する場合は、新しいデータベース オブジェクトが表示されます。 図 11 では、実稼働データベースのテーブル、ストアド プロシージャ、および開発用データベースのミラー化ユーザー定義関数を示します。 実稼働データベースの s テーブルに、ウィザードの実行時に、開発データベースのテーブルと同じデータがあるためデータをパブリッシュするデータベース公開ウィザードの指示に従って、します。 図 12 でデータを示しています、`Books`実稼働データベースのテーブル。


[![データベース オブジェクトが、実稼働データベースで重複しています](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**図 11**: [データベース オブジェクトがある重複しています、実稼働データベースで ([フルサイズの画像を表示する] をクリックします](deploying-a-database-vb/_static/image33.jpg))。


[![実稼働データベースには、開発用データベースのと同じデータが含まれています。](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**図 12**: 実稼働データベースには開発用データベースに同じデータが含まれています ([フルサイズの画像を表示する をクリックします](deploying-a-database-vb/_static/image36.jpg))。


この時点で、開発用データベースを運用環境にデプロイしましたがのみです。 Web アプリケーション自体の導入を検索されていないまたは実稼働アプリケーションで、実稼働データベースを使用するどのような構成の変更が必要かを確認しました。 次のチュートリアルではこれらの問題について説明します。

## <a name="summary"></a>まとめ

データ ドリブン web アプリケーションを配置するには、開発時に、実稼働環境に使用されるデータベースをコピーする必要があります。 多くの web ホスト プロバイダーは、データベースをデプロイするプロセスを簡略化するためのツールを提供します。 たとえば、DiscountASP.NET データベース FTP できます`.mdf`ファイル (またはバックアップ) と、コントロール パネルからデータベース サーバーにデータベースをアタッチします。 どのような機能に関係なく動作する別のオプション、web ホスト プロバイダーのプランは、開発データベースのスキーマとデータを作成する SQL コマンドのスクリプトを生成します Microsoft の Database Publishing Wizard ツールです。 このスクリプトが生成されたら、実稼働データベースで実行できます。

ブック_レビュー web アプリケーションのデータベースが実稼働になったできるアプリケーションをデプロイします。 ただし、web アプリケーションの構成情報が、データベースへの接続文字列を指定し、その接続文字列は、開発用データベースを参照します。 サイトを運用環境にデプロイするときに、この接続文字列情報を更新する必要があります。 次のチュートリアルを使用して、これらの構成の違いを調べ、運用環境へのデータ駆動型サイト、書籍レビューの公開に必要な手順について説明します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Microsoft SQL Server Database Publishing Wizard 1.1 のダウンロードします。](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Microsoft SQL Server Management Studio の Express Edition をダウンロードします。](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [前へ](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [次へ](configuring-the-production-web-application-to-use-the-production-database-vb.md)
