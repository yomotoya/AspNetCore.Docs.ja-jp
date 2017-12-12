---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: "FTP クライアント (c#) を使用して、サイトを展開する |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET アプリケーションを配置する最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。 Thi しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e4af20fa1fecd1f363e979023b41203096d64ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>FTP クライアント (c#) を使用して、サイトを展開します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> ASP.NET アプリケーションを配置する最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。 このチュートリアルでは、FTP クライアントを使用して、web ホスト プロバイダーをデスクトップからファイルを取得する方法を示します。


## <a name="introduction"></a>はじめに

前のチュートリアルには、いくつかの ASP.NET ページ、マスター ページ、カスタム ベースで構成されますが、単純なの書籍レビュー ASP.NET web アプリケーションが導入された`Page`クラス、イメージの数と、次の 3 つの CSS スタイル シートです。 この時点で、アプリケーションがアクセスできるだれでも、インターネットへの接続に、web ホスト プロバイダーには、このアプリケーションを展開する準備が整いました。


話し合いから、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-cs.md)チュートリアルでは、web ホスト プロバイダーにコピーする必要があるファイルがわかります。 (再呼び出しがどのようなファイルがコピーされるによって異なるかどうか、アプリケーションが明示的にまたは自動的にコンパイルします。)しかし、どうやってファイル (デスクトップ、) の開発環境から運用環境 (web ホスト プロバイダーによって管理されている web サーバー) まででしょうか。 [ **F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)ネットワーク経由で 1 台のコンピューターから別のファイルをコピーするための一般的に使用されるプロトコルします。 FrontPage Server Extensions (FPSE) こともできます。 このチュートリアルは、スタンドアロンの FTP クライアント ソフトウェアを使用して、必要なファイル、開発環境から運用環境を展開するについて説明します。

> [!NOTE]
> Visual Studio には、FTP; 経由で web サイトを発行するためのツールが含まれています。次のチュートリアルを見て FPSE を使用するツールと同様に、これらのツールについて説明します。


必要な FTP を使用してファイルをコピーする、 *FTP クライアント*開発環境でします。 FTP クライアントが実行しているコンピューターにインストールされているコンピューターからファイルをコピーするように設計されたアプリケーション、 *FTP サーバー*です。 (Web ホスト プロバイダーはほとんどの FTP を介したファイル転送をサポートする場合は、FTP サーバー、web サーバーで実行されている。)FTP クライアント アプリケーションの数は使用できます。 Web ブラウザーは、FTP クライアントとしても倍精度浮動小数点ことができます。 お気に入りの FTP クライアントとは、このチュートリアルを使用した 1 つは[FileZilla](http://filezilla-project.org/)Windows、Linux、および mac コンピューターで利用可能な無料、オープン ソースの FTP クライアント。 任意の FTP クライアントは機能、ただし、ためお気軽に最も使いやすいはどのようなクライアントを使用します。

に沿ってしている場合は必要がありますにアカウントを作成する前に、web ホスト プロバイダーを使用できますこのチュートリアルまたは完了以降。 述べたように前のチュートリアルでは、さまざまな価格、機能、およびサービスの品質と web ホスト プロバイダー会社などがあります。 このチュートリアルの系列が使用する[Discount ASP.NET](http://discountasp.net) web ホストとして、プロバイダーができますに従って、web ホスト プロバイダーでは、サイトが開発された ASP.NET バージョンをサポートしている限り、します。 (これらのチュートリアル使用して作成された ASP.NET 3.5。)また、ファイル、web ホスト プロバイダーをコピーしているため FTP を使用して、このチュートリアルと将来のでは、web ホスト プロバイダーが、web サーバーに対する FTP アクセスをサポートしています。 ほぼすべての web ホスト プロバイダーは、この機能を提供しますが、サインアップする前に再確認してください。

## <a name="deploying-the-book-review-web-application-project"></a>Book レビュー Web アプリケーション プロジェクトを配置します。

Web アプリケーションの書籍の確認の 2 つのバージョンがあることに注意してください: 1 つの Web アプリケーション プロジェクト モデル (BookReviewsWAP) およびその他の Web サイト プロジェクト モデル (BookReviewsWSP) を使用してを使用して実装します。 プロジェクトの種類は、自動的にまたは明示的に、サイトがコンパイルされ、そのコンパイル モデルがどのようなファイルを配置する必要がありますを指定するかどうかに影響します。 その結果、見ていきます BookReviewsWAP と BookReviewsWSP プロジェクトを個別に、展開する、BookReviewsWAP で開始します。 行っていない既にこれら 2 つの ASP.NET アプリケーションをダウンロードしてみましょう。

移動して BookReviewsWAP プロジェクトを起動して、`BookReviewsWAP`フォルダーとをダブルクリックすると、`BookReviewsWAP.sln`ファイル。 プロジェクトを配置する前に、ビルドのソース コードを変更するが、コンパイルされたアセンブリに含まれていることを確認する必要があります。 プロジェクトをビルドするビルド メニューを開き、BookReviewsWAP のビルド メニュー オプションを選択します。 1 つのアセンブリに、プロジェクト内のソース コードをコンパイルこの`BookReviewsWAP.dll`に配置されている、`Bin`フォルダーです。

必要なファイルを展開する準備が整いました。 FTP クライアントを起動し、web ホスト プロバイダーの web サーバーに接続します。 (Web ホスト会社にサインアップするときに、FTP サーバーに接続する方法に関する情報を電子メールで送信されます。 これには、FTP サーバーだけでなく、ユーザー名とパスワードのアドレスが含まれます) です。

デスクトップから、次のファイルを web ホスト プロバイダーのルート web サイト フォルダーにコピーします。 ホストしている場合、web サーバーに FTP、web のプロバイダーは、web サイトのルート ディレクトリにあります。 ただし、一部の web ホスト プロバイダーがという名前のサブフォルダーをある`www`または`wwwroot`web サイトのファイルのルート フォルダーとして機能します。 最後に、ファイルを FTPing ときにする必要があります、実稼働環境に対応するフォルダー構造を作成する、`Bin`フォルダー、`Fiction`フォルダー、`Images`フォルダーというようにします。

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- 完全な内容、`Styles`フォルダー
- 完全な内容、`Images`フォルダー (とそのサブフォルダー `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

図 1 は、必要なファイルをコピーした後、FileZilla を示します。 FileZilla は、左と右上のリモート コンピューター上のファイルで、ローカル コンピューター上のファイルを表示します。 図 1 に示す ASP.NET ソース コード ファイルなど`About.aspx.cs`、ローカル コンピューター (開発環境) 上にあるが、コード ファイルを使用する場合に展開する必要がないために、web ホスト プロバイダー (運用環境) にコピーされていません。明示的なコンパイルします。

> [!NOTE]
> これらは無視されて、ソース コード ファイルの実稼働サーバー上で害はありません。 ASP.NET は、これらは、web サイトへの訪問者にアクセスできない場合でも、ソース コード ファイルは、実稼働サーバー上に存在できるように、既定ではソース コード ファイルへの HTTP 要求が禁止されます。 (を参照してくださいしようとしているユーザーの場合は、`http://www.yoursite.com/Default.aspx.cs`これらの種類のファイルを含むことを説明するエラー ページが表示されます`.cs`ファイルでは禁止されています)。


[![FTP クライアントを使用して、デスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**図 1**: FTP クライアントを使用して、自分のデスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーする ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


サイトを展開した後すぐサイトをテストします。 ドメイン名を購入して、DNS の設定を構成したかどうか、適切にドメイン名を入力して、サイトにアクセスすることができます。 代わりに、web ホスト プロバイダー必要がありますが付属する、サイトの URL を次のように*accountname*.*webhostprovider*.com または*webhostprovider*.com/*accountname*です。 たとえば、ASP.NET の割引のマイ アカウントの URL は:`http://httpruntime.web703.discountasp.net`です。

図 2 は、配置の書評サイトを示します。 いる表示されて Discount ASP に注意してください。NET のサーバーで`http://httpruntime.web703.discountasp.net`です。 この時点で自分の web サイトを表示、インターネットへの接続を持つすべてのユーザーです。 予想、サイトの外観や開発環境でテストする場合と同様に動作します。

> [!NOTE]
> 正しいファイルのセットを展開していることを確認するすぐ、アプリケーションを表示するときにエラーが発生する場合。 次に、エラー メッセージがわかるかどうか、問題についての手がかりを確認します。 次に、web ホスト会社のヘルプデスクを有効にしたり、適切なフォーラムに質問を投稿、 [ASP.NET フォーラム](https://forums.asp.net/)です。


[![本のレビュー サイトは、インターネット接続を持つユーザーにアクセスできるようになりました](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**図 2**: Book レビュー サイトは、インターネット接続を持つユーザーにアクセスできるようになりました ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Book レビューの Web サイト プロジェクトを配置します。

BookReviewsWSP Web サイト プロジェクトなど、自動コンパイルを使用する ASP.NET アプリケーションを展開するときに、コンパイルされたアセンブリではありません、`Bin`フォルダーです。 その結果、web アプリケーションのソース コード ファイルを実稼働環境に展開する必要があります。 このプロセスについて説明しましょう。

ように Web アプリケーション プロジェクトには、最初のビルド アプリケーションを展開する前にすることをお勧めします。 Web サイト プロジェクトのビルド、アセンブリが作成されることはできません、間は、コンパイル時のエラー ページの確認はします。 検出して、サイトへの訪問者を持つのではなく、これらのエラーの検索を開始するより強力な!

プロジェクトを正常に作成した後は、web ホスト プロバイダーのルート web サイト フォルダーに、次のファイルをコピーするのに、FTP クライアントを使用します。 実稼働環境に対応するフォルダー構造を作成する必要があります。

> [!NOTE]
> プロジェクトは、まだ BookReviewsWSP プロジェクトの配置を試したい BookReviewsWAP が既に展開されている場合は、まず BookReviewsWAP を展開するときにアップロードされた web サーバー上のファイルをすべて削除し、BookReviewsWSP のファイルを展開します。


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- 完全な内容、`Styles`フォルダー
- 完全な内容、`Images`フォルダー (とそのサブフォルダー `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

図 3 は、必要なファイルをコピーした後、FileZilla を示します。 ご覧のように、ASP.NET ソース コード ファイルなど`About.aspx.cs`は、コード ファイルが自動を使用するときに、展開する必要があるため、ローカル コンピューター (開発環境) と web ホスト プロバイダー (運用環境) の両方に存在コンパイルします。


[![FTP クライアントを使用して、デスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**図 3**: FTP クライアントを使用して、自分のデスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーする ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


ユーザー エクスペリエンスは、アプリケーションのコンパイル モデルの影響を受けません。 同じ ASP.NET ページにアクセスし、外観や Web アプリケーション プロジェクトのモデルまたは Web サイト プロジェクトのモデルを使用して、web サイトが作成されたかどうか、同じ動作です。

### <a name="updating-a-web-application-on-production"></a>実稼働環境で Web アプリケーションの更新

Web アプリケーションの開発と展開は、1 回限りのプロセスではありません。 たとえば、書籍レビュー web サイトを作成するときにさまざまなページに構築され、パーソナル コンピューター (開発環境) に付属のコードを記述しました。 特定の安定した状態に達するとを展開したアプリケーション サイトにアクセスし、レビューを読み他のユーザーができるようにします。 展開は、このサイトで自分の開発の最後をマークしません。 複数の書評を追加または my レート ブックへの訪問者を許可するなどの新機能を実装または独自のコメントを残す場合があります。 このような拡張機能の開発環境で開発し、完了すると、展開する必要があります。 開発と配置、そのためが循環的です。 アプリケーションを開発して、展開するとします。 サイトがライブときに、実稼働環境での新機能が追加され、アプリケーションを再デプロイする必要のある時間の経過と共にバグが修正されています。 同様に続きます。

期待どおり、のみを追加または変更されたファイルをコピーする必要がある web アプリケーションを再デプロイするときにします。 サーバーまたはクライアント側 (行っても問題はありませんが) ファイルをサポートしてか変更していないページを再配置する必要はありません。

> [!NOTE]
> または、新しい ASP.NET ページをプロジェクトに追加するコードに関連する変更を加えるたびにアセンブリを更新する、プロジェクトをリビルドする必要がありますを明示的なコンパイルを使用する場合に留意すること、`Bin`フォルダーです。 したがって、(、他の新規および更新内容と共に) の運用上の web アプリケーションを更新するときに、実稼働環境にこの更新されたアセンブリをコピーする必要があります。


またに変更されているいずれかを理解、`Web.config`または内のファイル、`Bin`ディレクトリが停止し、web サイトのアプリケーション プールを再起動します。 使用して、セッション状態に格納されている場合、`InProc`モード (既定)、サイトの訪問者は、これらのキー ファイルが変更されるたびに、セッション状態に失われます。 この危険を回避することを検討してセッションを使用して格納する、`StateServer`または`SQLServer`モード。 このトピックの詳細については読み取る[セッション状態モード](https://msdn.microsoft.com/en-us/library/ms178586.aspx)です。

最後に、アプリケーションの再配置する実行できる任意の場所は数秒から実稼働環境にコピーする必要のあるファイルのサイズと数によっては、数分に注意してください。 この期間中にサイトを訪問ユーザー可能性がありますが発生するエラーや不適切な動作をします。 「無効にできます」アプリケーション全体という名前のページを追加することによって`App_Offline.htm`をユーザーに説明する、アプリケーションのルート ディレクトリに、サイトのメンテナンス (など) がダウンしてがなることをバックアップ直後。 ときに、`App_Offline.htm`ファイルが存在、ASP.NET ランタイムがそのページにすべての着信要求をリダイレクトします。

## <a name="summary"></a>概要

Web アプリケーションを配置するには、開発環境から運用環境に必要なファイルをコピーする必要があります。 ファイルがネットワーク経由で転送する最も一般的な方法は、ファイル転送プロトコル (FTP)、およびほとんどの web ホスト プロバイダーは、web サーバーに対する FTP アクセスをサポートします。 このチュートリアルでは、FTP クライアントを使用して、web サーバーに必要なファイルを配置する方法を説明しました。 展開した後、web サイトが参照できますすべてのユーザー接続でインターネットに!

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アプリ\_Offline.htm と「IE フレンドリ エラー」機能を回避します。](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [セッション状態モード](https://msdn.microsoft.com/en-us/library/ms178586.aspx)

>[!div class="step-by-step"]
[前へ](determining-what-files-need-to-be-deployed-cs.md)
[次へ](deploying-your-site-using-visual-studio-cs.md)
