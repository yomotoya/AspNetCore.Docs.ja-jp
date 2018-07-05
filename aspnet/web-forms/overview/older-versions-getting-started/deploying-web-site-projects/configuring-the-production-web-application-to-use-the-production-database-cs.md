---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: 運用データベース (c#) を使用する実稼働 Web アプリケーションの構成 |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルで既に説明した、これは開発および運用環境間で異なる構成情報珍しくありません。 これは、es.
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 559697a08200e43e955697a7ad8613f1a495c073
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803017"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>運用データベース (c#) を使用する実稼働 Web アプリケーションを構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> 前のチュートリアルで既に説明した、これは開発および運用環境間で異なる構成情報珍しくありません。 これは、データベースの接続文字列は、開発および運用環境の間で異なるとデータ ドリブン web アプリケーション、特に当てはまります。 このチュートリアルより詳細に適切な接続文字列を含める、運用環境を構成する方法について説明します。


## <a name="introduction"></a>はじめに

データ ドリブン web アプリケーションは通常、運用環境で場合より開発で別のデータベースを使用します。 Web ホスト プロバイダーによってホストされ、ローカルで開発をアプリケーションでは、開発用データベース通常存在、開発者のコンピューター上、実稼働データベースが、web ホスティング企業の施設でのデータベース サーバーでホストされているときにします。 データ ドリブン web アプリケーションを展開するには、開発用データベースを実稼働データベース サーバーにコピーする必要があります。 前のチュートリアルでは、この手順を実行する方法について説明しました。

Web アプリケーションは、情報を使用して、*接続文字列*データベースとの接続を確立します。 通常格納されている接続文字列`Web.config`、データベース サーバー名、データベース、セキュリティ コンテキスト、およびその他の情報の名前を指定します。 Web アプリケーションによって使用されるデータベースは、開発または実稼働環境で web アプリケーションが実行されているかどうかに依存しているため、接続文字列が 2 つの環境間とは異なる必要があります。

開発および運用環境間で異なる構成については珍しくありません。 *一般的な構成の相違点の間で開発および運用*チュートリアルには、上の簡単な説明だけでなく、これら 2 つの環境間で個別の構成情報を維持するための手法がについて説明しましたデータベース接続文字列。 このチュートリアルより詳細に適切な接続文字列を含める、運用環境を構成する方法について説明します。

## <a name="examining-the-connection-string-information"></a>接続文字列情報を調べる

書籍レビューの web アプリケーションで使用される接続文字列が、アプリケーションの構成ファイルに格納されている`Web.config`します。 `Web.config` といううまい名前の接続文字列を格納するための特別なセクションが含まれる[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)します。 `Web.config`ファイルの書籍レビューの web サイトがある 1 つの接続文字列をという名前のこのセクションで定義されている`ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

接続文字列 – データ ソース = \SQLEXPRESS;。AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated Security = True;ユーザー インスタンスは True を = - は、セミコロンと各オプションで区切られたオプションと値のペアと等号で区切られた値を持つさまざまなオプションと値で構成されます。 この接続文字列で使用される 4 つのオプションがあります。

- `Data Source` -(ある場合)、データベース サーバーとデータベース サーバーのインスタンス名の場所を指定します。 値、`.\SQLEXPRESS`例を示しますが、データベース サーバーとインスタンス名。 期間は、データベース サーバーは、アプリケーションと同じコンピューターを指定しますインスタンス名が`SQLEXPRESS`します。
- `AttachDbFilename` -データベース ファイルの場所を指定します。 値には、プレース ホルダーが含まれています。 `|DataDirectory|`、アプリケーションの秒の完全なパスに解決される`App_Data`実行時にフォルダー。
- `Integrated Security` -(false)、データベースまたは現在の Windows にアカウントの資格情報 (true) に接続するときに指定されたユーザー名/パスワードを使用するかどうかを示すブール値。
- `User Instance` -SQL Server Express エディションにローカル コンピューターの管理者以外のユーザー接続し、SQL Server Express Edition のデータベースへの接続を許可するかどうかを示す特定の構成オプション。 参照してください[SQL Server Express ユーザー インスタンス](https://msdn.microsoft.com/library/ms254504.aspx)この設定の詳細についてはします。
  

使用可能な接続文字列オプションは、データベースに接続して使用されている ADO.NET データベース プロバイダーによって異なります。 たとえば、データベースとは異なります、Oracle データベースに接続するために使用する Microsoft SQL Server に接続するため接続文字列。 同様に、SqlClient プロバイダーを使用して Microsoft SQL Server データベースに接続するときに、OLE DB プロバイダーを使用するよりも、別の接続文字列を使用します。

ようにサイトを使用して手動でデータベース接続文字列を構築できます[ConnectionStrings.com](http://www.connectionstrings.com/)としてリソースをどのようなオプションを使用できます。 Visual Studio でサーバー エクスプ ローラーに、データベースを追加するより簡単な方法は、ただし、し、[プロパティ] ウィンドウから接続文字列を取得します。 この後者の手法を使用して、実稼働データベース サーバーへの接続文字列を構築するためのことができます。

Visual Studio を開き、サーバー エクスプ ローラー ウィンドウに移動します (Visual Web developer では、このウィンドウと呼ばれるデータベース エクスプ ローラー)。 データ接続オプションを右クリックし、コンテキスト メニューから 接続の追加オプションを選択します。 図 1 に示すようにウィザードが表示されます。 適切なデータ ソースを選択し、[続行] をクリックします。


[![サーバー エクスプ ローラーに新しいデータベースを追加します。](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**図 1**: サーバー エクスプ ローラーに新しいデータベースを追加する ([フルサイズの画像を表示する をクリックします](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))。


次に、さまざまなデータベース接続情報を指定 (図 2 参照)。 Web ホスティング会社にサインアップしたときに、データベース サーバー名、データベース名、ユーザー名とパスワードを使用して、データベースに接続して、具合のデータベースに接続する方法の情報を指定した必要があります。 この情報を入力したら、このウィザードを完了して、データベースをサーバー エクスプ ローラーに追加する [ok] をクリックします。


[![データベースの接続情報を指定します](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**図 2**: データベースの接続情報を指定します ([フルサイズの画像を表示する をクリックします](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))。


サーバー エクスプ ローラーで、実稼働環境のデータベースが表示されます。 サーバー エクスプ ローラーからデータベースを選択し、[プロパティ] ウィンドウに移動します。 あるデータベースの接続文字列で接続文字列をという名前のプロパティが表示されます。 運用環境と、SqlClient プロバイダーを Microsoft SQL Server データベースを使用するいると仮定すると、接続文字列は、次のようになります。

<strong>データ ソース =<em>serverName</em>;Initial Catalog =<em>databaseName</em>;Persist Security Info = True;ユーザー ID =<em>username</em>;パスワード =*パスワード</strong>*

場所*serverName*、 *databaseName*、*ユーザー名*、および*パスワード*データベース サーバー名、データベースの値には名、およびユーザー名、および web ホスト会社から提供されたパスワード。

## <a name="deploying-the-book-reviews-web-application"></a>書籍レビューの Web アプリケーションを展開します。

前のチュートリアルでは、開発用データベースを運用環境にコピー スルーしますが、でした、データ駆動型アプリケーションの展開を検討していません。 この時点で、実稼働環境では、データベースが含まれていますが、静的レビューで、書籍レビューのアプリケーションのバージョンを使用しています。 最新の構成情報と、実稼働サーバーに、新しいデータ ドリブン アプリケーションをデプロイする必要があります。

データ ドリブン アプリケーションを開発環境から運用環境に展開する時間がかかります。 このプロセスは、前のチュートリアルの詳細について説明しました。 リフレッシャーが必要な場合を参照してください、 *FTP クライアントを使用して、web サイトを展開する*または*展開 web サイトを使用して Visual Studio*チュートリアル。 実稼働データベースの接続文字列は、1 つの代替。 つまり、運用環境で使用することを確認する必要があります`Web.config`ファイルを配置する必要があります。 具体的には、これは変更`Web.config`ファイルの`<connectionStrings>`要素は、実稼働データベースの接続文字列を含める必要があり、次のようになります。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

内の接続文字列に注意してください、`<connectionStrings>`要素の名前は同じ (`ReviewsConnectionString`)、開発データベースの接続文字列ではなく、実稼働データベースの接続文字列が含まれていますが、します。

形式化されたデプロイのワークフローがない限り、手動で変更するか、 `Web.config` (開発データベースの接続文字列を使用してに戻すことを記憶を展開する前に、実稼働データベースの接続文字列を使用するファイルその後)、別の管理または`Web.config`ファイルを実稼働環境の構成情報、実稼働環境に展開プロセスの一部としてアップロードを取得します。

> [!NOTE]
> 誤ってデプロイする場合、`Web.config`運用上のアプリケーションがデータベースに接続しようとすると、エラーになりますが、開発データベースの接続文字列を含むファイル。 としてマニフェストにこのエラーを`SqlException`で、サーバーが見つからなかったか、アクセスできなかったことを報告するメッセージ。


サイトを運用環境にデプロイすると、ブラウザーを使用して実稼働サイトを参照してください。 データ ドリブン アプリケーションをローカルに実行する場合と同じユーザー エクスペリエンスをお楽しみくださいし、表示されます。 もちろん実稼働環境で、web サイトにアクセスすると、サイトが搭載されています、実稼働データベース サーバーが、開発、データベースでは、開発環境で web サイトにアクセスします。 図 3 は、*教える自分で ASP.NET 3.5 in 24 時間*(ブラウザーのアドレス バーに URL に注意してください) の実稼働環境で web サイトからのページを確認します。


[![データ ドリブン アプリケーションでは、ここで使用可能なに運用環境です。](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**図 3**: The Data-Driven アプリケーションは今すぐ使用できるで運用します。 ([フルサイズの画像を表示する をクリックします](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))。


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>別の構成ファイルで接続文字列を保存します。

開発および運用環境に個別の構成情報を維持するための一般的な手法は 2 つのバージョンの`Web.config`: 開発環境と運用環境のいずれかのいずれか。 展開に適切な`Web.config`バージョンを実稼働環境にコピーできます。 理想的には、展開ワークフローの一部としてこのプロセスを自動化するとします。

2 つの個別のメンテナンスではなく`Web.config`ファイルは、必要に応じてより詳細な相違点を提供します。 構成する要素、`Web.config`ファイルで参照される、外部構成ファイルで定義できる、`Web.config`ファイル。 いずれかが簡単に言えば`Web.config`ファイルの両方の環境は、接続文字列を含む databaseConnectionStrings.config ファイルを参照するアプリケーションで使用され、環境ごとに一意になります。 個別のファイルに異なる構成情報を分離することを読みやすいように提供する検索し、単純な`Web.config`ファイルと明確にで開発および運用環境の構成の違いについて説明します。

この手法を使用するという名前の web アプリケーションで新しいフォルダーを作成して開始`ConfigSections`します。 次に、databaseConnectionStrings.dev.config databaseConnectionStrings.production.config というこの新しいフォルダーに 2 つのファイルを追加します。次に、コピー、`<connectionStrings>`要素から`Web.config`databaseConnectionStrings.dev.config と databaseConnectionStrings.production.config ファイルにし、内の接続文字列を変更しますdatabaseConnectionStrings.production.config ファイルの実稼働データベースの接続文字列を指定できるようにします。 たとえば、databaseConnectionStrings.dev.config ファイルを含める必要があります、`<connectionStrings>`開発用データベースを参照する接続文字列を持つ要素。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

同様に、databaseConnectionStrings.production.config ファイルがだけ指定する必要があります、`<connectionStrings>`要素が、実稼働データベースの接続文字列であります。

DatabaseConnectionStrings.dev.config ファイルのコピーを作成し、databaseConnectionStrings.config という名前を付けます。

> [!NOTE]
> できるファイルの名前を構成 databaseConnectionStrings.config、以外のものを場合は d など`connectionStrings.config`または`dbInfo.config`します。 ただし、ファイルに名前を必ず、`.config`拡張機能として`.config`ファイルは、既定で処理されない、ASP.NET エンジン。 などの場合、別の名前、ファイル、 `connectionStrings.txt`、ユーザーは、ブラウザーをポイントでした[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)し、ファイルの内容を表示。


この時点で、`ConfigSections`フォルダーは、3 つのファイル (図 4 参照) を含める必要があります。 DatabaseConnectionStrings.dev.config と databaseConnectionStrings.production.config ファイルには、開発および運用環境では、接続文字列がそれぞれ含まれます。 DatabaseConnectionStrings.config ファイルには、実行時に web アプリケーションによって使用される接続文字列情報が含まれています。 その結果、databaseConnectionStrings.config ファイルが運用環境で databaseConnectionStrings.config ファイルと同じになりますが、開発環境で databaseConnectionStrings.dev.config ファイルに同じであります。databaseConnectionStrings.production.config します。


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**図 4**: ConfigSections ([フルサイズの画像を表示する をクリックします](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))。


今すぐに指示する必要があります`Web.config`databaseConnectionStrings.config ファイルの接続文字列のストアを使用します。 `Web.config`を開いて、既存の `<connectionStrings>` 要素を次の XAML に置き換えます。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource`属性に関連する物理パスを指定します、`Web.config`ファイル。 場合、外部`.config`と同じディレクトリ内のファイルが`Web.config`のファイル名にこの属性を設定、`.config`ファイル。 場合に、サブディレクトリ内 databaseConnectionStrings.config は、指定を ConfigSections\databaseConnectionStrings.config のように、フォルダーとファイル名を区切るためにバック スラッシュを使用してサブフォルダーです。

この変更により、開発および運用環境を含む同じ`Web.config`ファイル。 ここで唯一の違いは、databaseConnectionStrings.config ファイルです。 運用環境に databaseConnectionStrings.production.config ファイルをコピーし、databaseConnectionStrings.config に変更します。今後、実稼働データベースの接続文字列への変更がある場合は、必要があります databaseConnectionStrings.production.config ファイルにすることし、運用環境にそのファイルをアップロードする databaseConnectionStrings.config 名前を変更します。

> [!NOTE]
> いずれかの情報を指定することがあります`Web.config`内の要素別のファイルおよび使用して、`configSource`内からそのファイルを参照する属性`Web.config`します。


## <a name="summary"></a>まとめ

通常、データ駆動型アプリケーションは、開発および運用環境で異なるデータベースを使用します。 その結果、web アプリケーションの構成に格納されているデータベースの接続文字列は、環境ごとに一意である必要があります。 このチュートリアルでは、実稼働データベースの接続文字列と 2 つの環境での一意の接続文字列情報を管理する方法を決定する方法を説明しました。

満足のプログラミングです。

#### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [接続文字列と構成ファイル](https://msdn.microsoft.com/library/ms254494.aspx)
- [データベースの構成文字列は @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Web.config ファイルから設定を移動します。](http://www.asp101.com/tips/index.asp?id=154)
- [技術ドキュメント、 &lt;connectionStrings&gt;要素](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [前へ](deploying-a-database-cs.md)
> [次へ](configuring-a-website-that-uses-application-services-cs.md)
