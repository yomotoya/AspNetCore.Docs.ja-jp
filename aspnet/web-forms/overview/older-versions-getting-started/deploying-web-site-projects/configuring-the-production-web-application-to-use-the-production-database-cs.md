---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: "運用データベース (c#) を使用する実稼働 Web アプリケーションの構成 |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルで既に説明した、構成については、開発環境と運用環境間で異なるは珍しいことではありません。 これは、es しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: e99b74030104bf17d4d79cd7670cf903270fa51f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>運用データベース (c#) を使用する実稼働 Web アプリケーションを構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> 前のチュートリアルで既に説明した、構成については、開発環境と運用環境間で異なるは珍しいことではありません。 これは、データベースの接続文字列は、開発環境と運用環境間で異なるデータ ドリブン web アプリケーション、特に当てはまります。 このチュートリアルより詳細に適切な接続文字列を含める実稼働環境を構成する方法について説明します。


## <a name="introduction"></a>はじめに

通常、データ ドリブン web アプリケーションは、実稼働環境で場合より開発で別のデータベースを使用します。 Web ホスト プロバイダーによってホストされ、ローカルで開発されたアプリケーションの場合、開発用データベース通常が存在する開発者のコンピューター上、実稼働データベースが web ホスティング企業の施設のデータベース サーバーでホストされているときにします。 データ ドリブン web アプリケーションを配置するには、開発用データベースを実稼働データベース サーバーにコピーする必要があります。 前のチュートリアルでは、この手順を実行する方法について説明しました。

Web アプリケーションの情報を使用して、*接続文字列*データベースとの接続を確立します。 接続文字列は、通常は`Web.config`、データベース サーバー名、データベース、セキュリティ コンテキスト、およびその他の情報の名前を指定します。 Web アプリケーションによって使用されるデータベースは、web アプリケーションは、開発環境または運用環境で実行されているかどうかに依存しているために、接続文字列は 2 つの環境間で異なります必要があります。

構成については、開発環境と運用環境間で異なるは珍しいことではありません。 *一般的な構成の相違点の間で開発および運用*チュートリアルでこれら 2 つの環境だけでなく、簡単に説明の間で独立した構成情報を維持するための手法を説明します。データベース接続文字列です。 このチュートリアルより詳細に適切な接続文字列を含める実稼働環境を構成する方法について説明します。

## <a name="examining-the-connection-string-information"></a>接続文字列情報を確認します。

書評の web アプリケーションで使用する接続文字列が、アプリケーションの構成ファイルに格納されている`Web.config`です。 `Web.config`適切なネーミングの接続文字列を格納するための特別なセクションが含まれる[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx)です。 `Web.config`ファイルの書評の web サイトがある 1 つの接続文字列をという名前のこのセクションで定義されている`ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

接続文字列のデータ ソース = \SQLEXPRESS;。AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated セキュリティ = Trueユーザー インスタンスは True を = - は、セミコロンと各オプションで区切られたオプションと値のペアと等号で区切られた値を持つオプションと値の数で構成されます。 この接続文字列で使用される 4 つのオプションは次のとおりです。

- `Data Source`-(指定されている場合)、データベース サーバーとデータベース サーバー インスタンス名の場所を指定します。 値、`.\SQLEXPRESS`例を示しますが、データベース サーバーとインスタンス名。 期間は、データベース サーバーが、アプリケーションと同じコンピューター上であるを指定しますインスタンス名が`SQLEXPRESS`です。
- `AttachDbFilename`-データベース ファイルの場所を指定します。 値には、プレース ホルダーが含まれています。 `|DataDirectory|`、アプリケーション s の完全なパスに解決される`App_Data`実行時にフォルダーです。
- `Integrated Security`-(false)、データベースまたは現在の Windows アカウントの資格情報 (true) に接続するときに、指定されたユーザー名とパスワードを使用するかどうかを示すブール値。
- `User Instance`構成オプションが、SQL Server Express エディションを示す、ローカル コンピューター上の管理者以外のユーザーがアタッチし、SQL Server Express Edition のデータベースへの接続を許可するかどうかを特定します。 参照してください[SQL Server Express ユーザー インスタンス](https://msdn.microsoft.com/en-us/library/ms254504.aspx)詳細については、この設定します。
  

使用可能な接続文字列オプションは、データベースに接続して使用している ADO.NET データベース プロバイダーによって異なります。 たとえば、データベースとは異なります、Oracle データベースに接続するために使用する Microsoft SQL Server への接続の接続文字列。 同様に、SqlClient プロバイダーを使用して Microsoft SQL Server データベースに接続するときに、OLE DB プロバイダーを使用するよりも別の接続文字列が使用します。

ようにサイトを使用して手動でデータベース接続文字列を構築できます[ConnectionStrings.com](http://www.connectionstrings.com/)としてリソースをどのようなオプションを使用できます。 ただしより簡単な方法は、Visual Studio でサーバー エクスプ ローラーに、データベースを追加するはし、[プロパティ] ウィンドウから接続文字列を取得します。 %S この後者の手法を使用して、実稼働データベース サーバーへの接続文字列を構築するために使用できます。

Visual Studio を開き、サーバー エクスプ ローラー ウィンドウに移動し (Visual Web Developer で、このウィンドウと呼ばれるデータベース エクスプ ローラー)。 データ接続 オプションを右クリックし、コンテキスト メニューから 接続の追加オプションを選択します。 図 1 に示すようにウィザードが表示されます。 適切なデータ ソースを選択し、[続行] をクリックします。


[![サーバー エクスプ ローラーに新しいデータベースを追加します。](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**図 1**: サーバー エクスプ ローラーに新しいデータベースを追加する ([フルサイズのイメージを表示するをクリックして](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


次に、さまざまなデータベース接続情報を指定 (図 2 を参照してください)。 Web ホスティング企業にサインアップするときに、データベース サーバー名、データベース名、ユーザー名と、データベースへの接続に使用するパスワードのデータベースに接続する方法に関する情報を指定した必要があります。 この情報を入力したら、このウィザードを完了して、データベース、サーバー エクスプ ローラーを追加する [ok] をクリックします。


[![データベース接続情報を指定します](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**図 2**: データベース接続情報を指定します ([フルサイズのイメージを表示するをクリックして](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


サーバー エクスプ ローラーで、実稼働環境のデータベースが表示されます。 サーバー エクスプ ローラーからデータベースを選択し、[プロパティ] ウィンドウに移動します。 データベースの接続文字列で接続文字列をという名前のプロパティがあります。 運用環境と、SqlClient プロバイダーでは、Microsoft SQL Server データベースを使用するいると仮定した場合、接続文字列は、次のようになります。

**データ ソース =*serverName*です。Initial Catalog =*databaseName*です。Persist Security Info = Trueユーザー ID =*username*です。パスワード =*パスワード***

ここで*serverName*、 *databaseName*、 *username*、および*パスワード*データベース サーバー名、データベースの値では、名前、およびユーザー名と、web ホスト会社から提供されたパスワード。

## <a name="deploying-the-book-reviews-web-application"></a>Book レビュー Web アプリケーションを展開します。

前のチュートリアルでは、ステップ スルー、開発用データベースを実稼働環境にコピーが、データ ドリブン アプリケーションを配置するを探索できませんでした。 この時点で、実稼働環境は、データベースが含まれていますが、静的レビューで、アプリケーションのバージョン、書籍のレビューを使用しています。 更新された構成情報と実稼働サーバーに、新しいデータ ドリブン アプリケーションを配置する必要があります。

すぐをデータ ドリブン アプリケーションを開発環境から実稼働環境に展開します。 このプロセスが、前のチュートリアルで詳しく説明します。 リフレッシャー場合を参照してください、 *FTP クライアントを使用して、web サイトを展開する*または*を展開する、web サイトを使用して Visual Studio*チュートリアルです。 実稼働データベースの接続文字列がいずれかの代替されることを意味する実稼働環境で使用されることを確認する必要があります`Web.config`ファイルを配置する必要があります。 具体的には、この設定変更`Web.config`ファイルの`<connectionStrings>`要素は、実稼働データベースの接続文字列を格納する必要があるし、次のようになります。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

内の接続文字列を`<connectionStrings>`要素の名前は同じ (`ReviewsConnectionString`)、開発データベース接続文字列ではなく、実稼働データベースの接続文字列が含まれていますが、します。

形式化された展開のワークフローがない限り、変更するか手動で、 `Web.config` (開発データベース接続文字列を使用してに戻すに記憶を展開する前に、実稼働データベースの接続文字列を使用するファイルその後) または別の管理`Web.config`展開プロセスの一部として、実稼働環境にアップロードを取得している実稼働環境の構成情報を持つファイルです。

> [!NOTE]
> 誤ってを展開する場合、`Web.config`を実稼働環境でアプリケーションがデータベースに接続しようとすると、エラーになりますが、開発データベース接続文字列を含むファイルです。 明らかにこのエラーは`SqlException`レポート サーバーが見つからないか、アクセスできませんでしたメッセージを使用します。


サイトは実稼働環境に展開した後は、お使いのブラウザーを使用して実稼働サイトを参照してください。 表示して、データ ドリブン アプリケーションをローカルで実行するときと同じユーザー エクスペリエンスをご利用いただけますが必要です。 もちろん実稼働環境で、web サイトにアクセスすると、サイトが電源に実稼働データベース サーバーが、開発中、データベースでは、開発環境で web サイトにアクセスします。 図 3 に表示される、*学べる自分で ASP.NET 3.5 24 時間以内に*(ブラウザーのアドレス バーの URL に注意してください) の実稼働環境で web サイトからのページを確認します。


[![データ ドリブン アプリケーションは、ここで使用可能な 実稼働です!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**図 3**: The Data-Driven アプリケーションは現在使用可能なで運用します。 ([フルサイズのイメージを表示するをクリックして](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>別の構成ファイルで接続文字列を保存します。

開発および運用環境での個別の構成情報を保持するための一般的な手法は 2 つのバージョンを`Web.config`: 開発環境と運用環境のいずれかのいずれか。 展開に適切な`Web.config`バージョンを実稼働環境にコピーできます。 理想的には、展開のワークフローの一部としてこのプロセスを自動化するとします。

2 つの異なるを維持するのではなく`Web.config`できます必要に応じて、ファイルより細かい違いを提供します。 構成する要素、`Web.config`ファイルで参照される、外部構成ファイルで定義できる、`Web.config`ファイル。 いずれかが簡単に言えば`Web.config`ファイルの両方の環境との接続文字列を含むよう databaseConnectionStrings.config ファイルを参照するアプリケーションで使用され、環境ごとに一意になります。 個別のファイルに異なる構成情報を分離することが提供する、読みやすいように検索し、単純な`Web.config`概要、開発環境と運用環境の構成の違いを明確にし、ファイルです。

この手法を使用するのには、という名前の web アプリケーションに新しいフォルダーを作成して開始`ConfigSections`です。 次に、名前付き databaseConnectionStrings.dev.config および databaseConnectionStrings.production.config この新しいフォルダーに 2 つのファイルを追加します。次に、コピー、`<connectionStrings>`要素から`Web.config`databaseConnectionStrings.dev.config と databaseConnectionStrings.production.config のファイル内の接続文字列を変更し、databaseConnectionStrings.production.config ファイルに、実稼働データベースの接続文字列を指定します。 たとえば、databaseConnectionStrings.dev.config ファイルを含める必要があります、`<connectionStrings>`開発用データベースを参照する接続文字列を持つ要素。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

同様に、databaseConnectionStrings.production.config ファイルを含めることはだけ、`<connectionStrings>`要素が、実稼働データベースの接続文字列であります。

DatabaseConnectionStrings.dev.config ファイルのコピーを作成し、databaseConnectionStrings.config という名前を付けます。

> [!NOTE]
> ファイルの名前を構成 databaseConnectionStrings.config、以外のものな場合は d など`connectionStrings.config`または`dbInfo.config`です。 ただし、名前を使用して、ファイルを確認して、`.config`拡張機能として`.config`ファイルは、既定で処理されない ASP.NET エンジンです。 などの名前を付ける場合、ファイル以外、 `connectionStrings.txt`、ユーザーは、ブラウザーをポイントでした[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)ファイルの内容を表示および!


この時点で、`ConfigSections`フォルダーは、次の 3 つのファイル (図 4 を参照) を含める必要があります。 DatabaseConnectionStrings.dev.config と databaseConnectionStrings.production.config ファイルには、開発および運用環境では、接続文字列がそれぞれ含まれます。 DatabaseConnectionStrings.config ファイルには、実行時に web アプリケーションで使用される接続文字列情報が含まれています。 その結果、databaseConnectionStrings.config ファイルと同じになります、開発環境で databaseConnectionStrings.dev.config ファイル一方、実稼働環境では、databaseConnectionStrings.config ファイルと同じにする必要があります。databaseConnectionStrings.production.config です。


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**図 4**: ConfigSections ([フルサイズのイメージを表示するをクリックして](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


今すぐに指示する必要があります`Web.config`接続文字列ストアに対して databaseConnectionStrings.config ファイルを使用します。 開いている`Web.config`と既存の置換`<connectionStrings>`を次の要素。

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource`属性を基準とする物理パスを指定する、`Web.config`ファイル。 場合、外部`.config`と同じディレクトリ内のファイルが`Web.config`のファイル名にこの属性を設定、`.config`ファイル。 場合に、サブディレクトリ内に databaseConnectionStrings.config を持つケースとしては、ConfigSections\databaseConnectionStrings.config と同様に、フォルダーとファイル名を区切るためにバック スラッシュを使用するサブフォルダーを指定します。

この変更により、開発環境と運用環境を含む同じ`Web.config`ファイル。 ここで唯一の違いは、databaseConnectionStrings.config ファイルです。 実稼働環境に databaseConnectionStrings.production.config ファイルをコピーし、databaseConnectionStrings.config に名前を変更します。今後、実稼働データベースの接続文字列への変更がある場合は、必要がありますに databaseConnectionStrings.production.config ファイルに行い、実稼働環境にそのファイルをアップロード databaseConnectionStrings.config 名前を変更します。

> [!NOTE]
> いずれかの情報を指定することがあります`Web.config`要素では、別のファイルを使用して、`configSource`内からそのファイルを参照する属性`Web.config`です。


## <a name="summary"></a>概要

データ ドリブン アプリケーションは、通常、開発環境と運用環境で異なるデータベースを使用します。 その結果、web アプリケーションの構成に格納されているデータベースの接続文字列は、環境ごとに一意でなければなりません。 このチュートリアルでは、実稼働データベースの接続文字列と 2 つの環境での一意の接続文字列の情報を維持する方法を決定する方法を説明しました。

満足プログラミング!

#### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [接続文字列と構成ファイル](https://msdn.microsoft.com/en-us/library/ms254494.aspx)
- [データベースの構成情報 @ ConnectionStrings.com を文字列します。](http://www.connectionstrings.com/)
- [Web.config ファイルから設定を移動します。](http://www.asp101.com/tips/index.asp?id=154)
- [技術ドキュメント、 &lt;connectionStrings&gt;要素](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[前へ](deploying-a-database-cs.md)
[次へ](configuring-a-website-that-uses-application-services-cs.md)
