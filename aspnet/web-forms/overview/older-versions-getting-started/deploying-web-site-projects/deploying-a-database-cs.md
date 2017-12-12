---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: "データベース (c#) を展開する |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET web アプリケーションを配置するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 Da しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: f71e3cd1e81644df7b3dfed363b6f2ca826e610d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-c"></a>データベース (c#) の配置
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> ASP.NET web アプリケーションを配置するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 データ ドリブン web アプリケーション用には、データベース スキーマとデータが含まれます。 このチュートリアルでは、開発環境から実稼働環境にデータベースを正常に展開するために必要な手順について説明するシリーズの最初。


## <a name="introduction"></a>はじめに

ASP.NET web アプリケーションを配置するには、開発環境から運用環境に必要なファイルとリソースを取得する必要があります。 過去 6 つのチュートリアルの過程で単純な書評 web アプリケーションの配置について説明しました。 このデモ サイトがサーバー側リソース - ASP.NET ページ、構成ファイルの数から成る、`Web.sitemap`画像や CSS ファイルなどのファイルとなどのクライアント側のリソースと一緒にします。 しかし、web アプリケーションをデータ ドリブンのでしょうか。 データベースを使用する web アプリケーションを展開するには、どのような余分な手順を実行する必要がありますか。

次のいくつかのチュートリアルでは、データ ドリブン web アプリケーションを展開するために必要な手順を説明します。 このチュートリアルを確認するにを取得する方法データベースのスキーマおよび内容の開発環境から実稼働環境に後続のチュートリアルは、必要な構成の変更中に開始します。 次のアプリケーション サービス (メンバーシップ、ロール、プロファイル、およびなど) を使用するデータベースを展開する次の課題を検討しましょう。

## <a name="examining-the-updated-book-reviews-web-application"></a>更新されたブック レビュー Web アプリケーションを調べています

データ ドリブン web アプリケーションの配置を示すためには ve がデータ ドリブンのいずれかに簡単で静的な web サイトから書評 web アプリケーションを更新します。 前に、このチュートリアルのダウンロードに、アプリケーションの 2 つのバージョンがある: 1 つを Web アプリケーション プロジェクト モデルと Web サイト プロジェクトのモデルを使用する 1 つを使用します。

更新された書評の web アプリケーションで使用する、 [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx)サイト %s に格納されているデータベース`App_Data`フォルダー (`~/App_Data/Reviews.mdf`)。 お使いのコンピューターにインストールされている SQL Server 2008 がある場合、デモはエラーなし実行する必要があります。 以前のバージョンがある場合は、SQL Server のいずれかを実行できますが、無料の SQL Server 2008 Express Edition をインストールまたはこのチュートリアルで使用できるスクリプトのダウンロードを自分でデータベースを作成するデータベースを使用することができます。

`Reviews.mdf`データベースには、4 つのテーブルが含まれています。

- `Genres`-各ジャンル、テクノロジ、架空の状態、およびビジネスなどのレコードが含まれています。
- `Books`-のように列を含む、各レビューのためのレコードが含まれています`Title`、 `GenreId`、 `ReviewDate`、および`Review`、その他。
- `Authors`-確認済みのブックへの投稿者の各作成者に関する情報が含まれています。
- `BooksAuthors`-どの作成者は、どのようなブックを記述したを指定する多対多の結合テーブルです。
  

図 1 は、これら 4 つのテーブルの ER ダイアグラムを示します。


[![Book レビュー Web アプリケーションのデータベースは 4 つのテーブルの構成](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**図 1**: Book レビュー Web アプリケーション データベースが 4 つのテーブルの構成 ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image3.jpg))


以前のバージョンの書評の web サイトには、書籍ごとに個別の ASP.NET ページが必要があります。 たとえば、という名前のページが発生しました`~/Tech/TYASP35.aspx`の確認に含まれている*学べる自分で ASP.NET 3.5 24 時間以内に*です。 この新しいデータ ドリブンのバージョンの web サイトは、データベースと 1 つの ASP.NET ページ、Review.aspx?ID= に格納されているレビュー*bookId*を指定したブックのレビューが表示されます。 同様は、Genre.aspx?ID=*genreId*指定ジャンルの確認済みのブックを一覧するページ。

図 2 および 3 の表示、`Genre.aspx`と`Review.aspx`ページの動作です。 各ページのアドレス バーの URL に注意してください。 図 2 it s Genre.aspx? ID = 85d164ba-1123-4 c 47-82a0-c8ec75de7e0e です。 85d164ba-1123-4c47-82a0-c8ec75de7e0e があるため、`GenreId`テクノロジ ジャンル、「技術確認」ページの見出しの読み取りおよび箇条書きリストの値がこのジャンルの下にあるサイトでのこれらのレビューを列挙します。


[![テクノロジのジャンル ページ](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**図 2**:「テクノロジ ジャンルのページ ([フルサイズのイメージを表示するには、をクリックして](deploying-a-database-cs/_static/image6.jpg))


[![ASP.NET 3.5 に 24 時間を学ぶのレビュー](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**図 3**: の レビュー*学べる自分で ASP.NET 3.5 24 時間以内に*([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image9.jpg))


Book レビュー web アプリケーションには、管理 セクションで管理者ことができますを追加、編集、および ジャンル、レビューを削除および情報を作成も含まれます。 現時点では、[管理] セクションのすべての利用者にアクセスできます。 今後のチュートリアルでは、ユーザー アカウントのサポートを追加し、[管理] ページに承認されたユーザーにのみ許可。

ダウンロードした場合は書評アプリケーションしてください。 その目的は、データ ドリブン アプリケーションの配置を示すために留意してください。 アプリケーションの設計枝葉と同じベスト プラクティスは発生しません。 たとえば、別のデータ アクセス層 (DAL); はありません。ASP.NET ページは、SqlDataSource コントロールまたはその分離コード クラスの ADO.NET コードを使用して、データベースと直接通信します。 階層化されたアーキテクチャを使用するデータ ドリブン アプリケーションの作成をより詳しくを参照してください  [*データを扱う*チュートリアル](../../data-access/index.md)です。

## <a name="databases-on-development-versus-production"></a>運用と開発上のデータベース

データ ドリブン web アプリケーションの開発を開始するときに、データベースに接続する方法でアプリケーションの詳細を提供する、データベース接続文字列を指定する必要があります。 この接続文字列の間など、データベース サーバー、データベース名、およびセキュリティ情報を指定します。 開発時にアプリケーションによって使用されるデータベースは別のデータベースが使用される場合よりも多くの場合、その実稼働環境で s。 運用と開発の異なるデータベースを使用する利点もあります。 Don t が誤って変更されたり、ライブ データを削除する心配が異なる開発の方法でデータベースします。 ダミーのテスト データに配置したり、実稼働環境でアプリケーションに与える影響を心配しなくても、データ モデルに互換性に影響する変更を加えることもできます。 発生している別のデータベース開発と実稼働環境では、アプリケーションはされますが、データベースのスキーマをデータベースおよび関連する変更点を展開またはデータも展開する必要があります。

最初の展開前に、データベースの 1 つだけのインスタンスが存在し、開発環境では、そのインスタンス。 最初に実稼働環境にアプリケーションを展開するときに必要なサーバー側とクライアント側のファイルをコピーするだけでなくもデータベースを開発環境から実稼働環境にコピーお必要があります。 これは、次のよう適切に書評 web アプリケーションとデータベースが存在するので、`App_Data`開発環境でのフォルダーがされていない運用環境にプッシュされていません。

アプリケーションが展開された後、データベースのコピーが 2 つがあります。 アプリケーションに近づくにつれて、データ モデルへの変更を加えなくて、新機能を追加することがあります (既存のテーブルに新しい列を追加するなど、新しいテーブルを追加する既存の列に変更を加えるとなど)。 次に、web アプリケーションが展開されると、最後の展開は、実稼働データベースに適用する必要がありますので、変更が開発環境でデータベースに適用します。 このプロセスを管理するためのいくつかの方法については、今後のチュートリアルで説明します。 このチュートリアルは、データベース全体、開発環境から実稼働環境に展開するについて説明します。

## <a name="deploying-the-database-to-the-production-environment"></a>データベースを実稼働環境に展開します。

このチュートリアルの残りの部分は、開発環境から運用環境にデータベースを展開する方法を検索します。 に沿ってしている場合、web ホスト プロバイダーを使用してアカウントに Microsoft SQL Server が含まれているかどうかを確認するデータベースをサポートする必要です。 また、情報がある、つまり、データベース サーバー名、データベース名と、ユーザー名とパスワードのデータベースに接続するために使用する必要があります。

述べたようにこのチュートリアルで前に、書評 web サイトのデータベースは、SQL Server 2008 Express Edition データベースに格納されている、`App_Data`フォルダーです。 理由でこのようなデータベースを展開することが簡単にコピーするスタンバイには、`App_Data`開発環境から運用環境にフォルダーです。 ただし、ほとんどの web ホスト プロバイダーが内のホスティング データベースをサポートして、`App_Data`セキュリティ上の理由のためのフォルダーです。 代わりに、web ホストは、その環境内の SQL Server データベース サーバーでアカウントを指定します。 開発環境から運用環境にデータベースを展開するには、web ホストのデータベース サーバーに登録されているデータベースを取得する必要があります。

どのように行うから取得する、データベース開発環境、運用環境にしますか。 これを実現するサービス、web ホスト サービスの新機能によって、いくつかの方法はあります。 DiscountASP.NET などのいくつかのホストとは、データベース、または実際のバックアップを FTP できます`.mdf`web サイトにファイルしその後、[コントロール パネル] から、バックアップ ファイルを復元またはアタッチ、`.mdf`ファイルを SQL Server データベース サーバーです。 このようなツールを使用して、データベースを展開するほど単純ではコピーとして、`App_Data`フォルダーを実稼働環境と、コントロール パネルを使用してアタッチすることにします。 これが最初に、データベースをパブリッシュする最も簡単で高速な方法ではおそらくです。

別の方法では、データベース パブリッシュ ウィザードを使用します。 データベース パブリッシュ ウィザードは、Windows デスクトップ アプリケーションをそのテーブル内のテーブル、ストアド プロシージャ、ビュー、ユーザー定義関数、およびなどのデータベースのスキーマと、必要に応じて、データを作成する SQL コマンドを生成します。 SQL Server Management Studio で web ホスト プロバイダーのデータベース サーバーに接続でき、運用上のデータベースを複製するには、このスクリプトを実行できます。 、、Web ホスト プロバイダーは、Microsoft の s をサポートしている場合、より高い[Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home)あなたの代理として、データベース サーバーで自動的に実行するデータベースの公開ウィザードによって生成されたスクリプトを持つことができます。 データベース パブリッシュ ウィザードでは、データベースのスキーマとデータを作成するスクリプトを生成するので、web ホスト プロバイダーは、アップロード済みのアタッチなどの機能を提供するかどうかに関係なく動作する`.mdf`ファイル。

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>データベース スキーマおよびデータベースの公開ウィザードを使用してデータを作成する SQL コマンドを生成します。

実稼働環境に書評にデータベースを配置するデータベース パブリッシュ ウィザードを使用していきます s を使用できます。 Visual Studio 2008 を使用している場合、またはを超えて、Database Publishing Wizard が既にインストールされています。 Visual Studio 2005 を使用しているかどうかは、最初に必要になります[をダウンロードしてインストール](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)ウィザード。

Visual Studio を開きに移動、`Reviews.mdf`データベース。 Visual Web Developer を使用している場合、データベース エクスプ ローラーに移動します。Visual Studio を使用している場合は、サーバー エクスプ ローラーを使用します。 図 4 は、 `Reviews.mdf` Visual Web Developer で データベース エクスプ ローラーでデータベース。 図 4 に示す、`Reviews.mdf`データベースはの 4 つのテーブル、次の 3 つのストアド プロシージャ、およびユーザー定義関数で構成されます。


[![データベース エクスプ ローラーまたはサーバー エクスプ ローラーで、データベースを検索します。](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**図 4**: データベース エクスプ ローラーまたはサーバー エクスプ ローラーで、データベースを探します ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image12.jpg))


データベース名を右クリックし、コンテキスト メニューから、「プロバイダーにパブリッシュ」オプションを選択します。 データベース パブリッシュ ウィザードが起動 (図 5 を参照してください)。 スプラッシュ画面からの事前の横にあるをクリックします。


[![データベース パブリッシュ ウィザードのスプラッシュ スクリーン](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**図 5**:「データベース パブリッシュ ウィザード スプラッシュ スクリーン ([フルサイズ イメージを表示するに、をクリックして](deploying-a-database-cs/_static/image15.jpg))


ウィザードでは、2 番目の画面は、データベース パブリッシュ ウィザードにアクセスできるデータベースを一覧表示され、選択したデータベース内のすべてのオブジェクトのスクリプトを作成するスクリプトを作成するオブジェクトを選択するかを選択できます。 適切なデータベースを選択し、「すべてのスクリプトは、選択したデータベースにオブジェクト」オプションがオンのままにします。

> [!NOTE]
> エラーが発生した場合は、"データベースにオブジェクトが存在しない*databaseName*このウィザードでスクリプトを作成できる型の"図 6 に示す画面で、[次へ] をクリックすると、データベース ファイルへのパスが長すぎることを確認してください。 データベース ファイルへのパスが長すぎる場合にこのエラーが生じることが検出されました。


[![データベース パブリッシュ ウィザードのスプラッシュ スクリーン](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**図 6**:「データベース パブリッシュ ウィザード スプラッシュ スクリーン ([フルサイズ イメージを表示するに、をクリックして](deploying-a-database-cs/_static/image18.jpg))


次の画面からスクリプト ファイルを生成したり、web ホストでサポートされる場合、web ホスト プロバイダーのデータベース サーバーに直接、データベースを公開できます。 図 7 に示すファイルに記述されたスクリプトが`C:\REVIEWS.MDF.sql`です。


[![ファイルにデータベースをスクリプト化または、Web ホスト プロバイダーに直接発行](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**図 7**: ファイルにデータベースをスクリプト化または、Web ホスト プロバイダーに直接発行 ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image21.jpg))


後続の画面では、さまざまなスクリプト作成オプションを指定する要求されます。 スクリプトがこれらの既存のオブジェクトを削除する drop ステートメントを含めるかどうかを指定することができます。 既定値は true の場合、最初にデータベースを展開するときにこれは問題ありません。 ターゲット データベースが SQL Server 2000、SQL Server 2005 または SQL Server 2008 であるかどうかを指定することもできます。 最後に、スキーマとデータ、スクリプトを作成するかどうかを指定できますのデータまたはスキーマだけです。 スキーマは、データベース オブジェクト、テーブル、ストアド プロシージャ、ビュー、およびなどのコレクションです。 データは、テーブル内に存在する情報です。

図 8 に示すようには、SQL Server 2008 データベース用スクリプトの生成、スキーマとデータの両方を公開するために既存のデータベース オブジェクトを削除するように構成ウィザードを設けています。


[![発行を指定するオプション](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**図 8**: 発行オプションを指定します ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image24.jpg))


最後の 2 つの画面を実行して、スクリプト作成のステータスを表示しています、操作をまとめたものです。 ウィザードの実行の最終的な結果は、実稼働環境でデータベースを作成し、開発のように、同じデータを設定するために必要な SQL コマンドを含むスクリプト ファイルがあることです。

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>実稼働環境のデータベースで SQL コマンドを実行します。

これでが残っているデータベースとそのデータを作成する SQL コマンドが含まれるスクリプトでは、実稼働データベースでスクリプトを実行します。 一部の web ホスト プロバイダーは、データベースで実行する SQL コマンドを入力することができます、コントロール パネルの テキスト ボックスを提供します。 非常に大きいスクリプト ファイルがあるかどうかは、このオプションは動作しない可能性があります (、`REVIEWS.MDF.sql`スクリプト ファイルは、サイズ、425 KB を超えるのインスタンス)。

SQL Server Management Studio (SSMS) を使用して実稼働データベース サーバーに直接接続することをお勧めします。 非 Express エディションの SQL Server コンピューターにインストールされている必要がある場合、可能性がありますが既にある SSMS をインストールします。 それ以外の場合を実行できます[をダウンロードしてインストール](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)SQL Server Management Studio Express Edition の無償コピーします。

SSMS を起動し、web ホスト プロバイダーによって提供される情報を使用して web ホストのデータベース サーバーに接続します。


[![Web ホスト プロバイダーのデータベース サーバーへの接続します。](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**図 9**: Your Web ホスト プロバイダーのデータベース サーバーへの接続 ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image27.jpg))


[データベース] タブを展開し、データベースを探します。 ツールバーの左上隅の 新しいクエリ ボタン、データベースの公開ウィザードによって作成されたスクリプト ファイルから SQL コマンドに貼り付けますを実稼働データベース サーバーでこれらのコマンドを実行する実行 ボタンをクリックします。 スクリプト ファイルが非常に大きくなる場合、コマンドを実行するまでに数分かかる場合があります。


[![Web ホスト プロバイダーのデータベース サーバーへの接続します。](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**図 10**: Your Web ホスト プロバイダーのデータベース サーバーへの接続 ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image30.jpg))


すべてが s です! この時点で、開発用データベースが実稼働環境に複製されました。 SSMS でデータベースを更新する場合は、新しいデータベース オブジェクトが表示されます。 図 11 は、実稼働データベースのテーブル、ストアド プロシージャ、および、開発用データベースのミラー化ユーザー定義関数を示します。 いて、データをパブリッシュするデータベース パブリッシュ ウィザードを指示しているため、実稼働データベースの s テーブル開発データベースのテーブルと同じデータの作成ウィザードの実行時にします。 図 12 内のデータを示しています、`Books`実稼働データベースのテーブルです。


[![実稼働データベースでデータベース オブジェクトが重複しています](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**図 11**: データベース オブジェクトされてが重複している実稼働データベースで ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image33.jpg))


[![実稼働データベースには、開発用データベースのと同じデータが含まれています。](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**図 12**: 実稼働データベースには、開発用データベースに同じデータが含まれています ([フルサイズのイメージを表示するをクリックして](deploying-a-database-cs/_static/image36.jpg))


この時点で実稼働環境にのみ、開発用データベースを配置おがします。 おされていない自体、web アプリケーションの展開を調べることも、実稼働データベースを使用して実稼働環境でアプリケーションに必要な構成変更内容を確認します。 次のチュートリアルでは、これらの問題を説明します。

## <a name="summary"></a>概要

データ ドリブン web アプリケーションを配置するには、開発時に、実稼働環境に使用するデータベースをコピーする必要があります。 多くの web ホスト プロバイダーは、データベースの配置のプロセスを簡略化するためのツールを提供します。 たとえば、DiscountASP.NET データベース FTP できます`.mdf`ファイル (またはバックアップ) とデータベース サーバーに、コントロール パネルからデータベースをアタッチします。 どのような機能に関係なく動作する別のオプション、web ホスト プロバイダーの提供は、開発データベース %s のスキーマとデータを作成するための SQL コマンドのスクリプトを生成する Microsoft の Database Publishing Wizard ツールです。 このスクリプトが生成されたら、実稼働データベースで実行することはできます。

書評 web アプリケーションのデータベースが実稼働環境ではこれでアプリケーションを展開できます。 ただし、web アプリケーションの構成情報が、データベースへの接続文字列を指定し、その接続文字列は、開発用データベースを参照します。 実稼働環境にサイトを展開するときに、この接続文字列情報を更新する必要があります。 次のチュートリアルを使用して、これらの構成の相違点を調べ、実稼働環境に、データ ドリブン書評サイトを発行するために必要な手順について説明します。

満足プログラミング!

#### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Microsoft SQL Server Database Publishing Wizard 1.1 のダウンロードします。](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Microsoft SQL Server Management Studio の Express Edition をダウンロードします。](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[前へ](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
[次へ](configuring-the-production-web-application-to-use-the-production-database-cs.md)
