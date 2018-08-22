---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: FTP クライアント (c#) を使用して、サイトの展開 |Microsoft Docs
author: rick-anderson
description: ASP.NET アプリケーションをデプロイする最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。 足りない.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: cdecc85c056fc5153763d938c665b473117df9ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823886"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>FTP クライアント (c#) を使用してサイトを展開します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> ASP.NET アプリケーションをデプロイする最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。 このチュートリアルでは、FTP クライアントを使用して、web ホスト プロバイダーをデスクトップからファイルを取得する方法を示します。


## <a name="introduction"></a>はじめに

前のチュートリアルには、いくつかの ASP.NET ページ、マスター ページ、カスタム ベースで構成されますが、単純なの書籍レビューの ASP.NET web アプリケーションが導入された`Page`クラス、イメージの数と、次の 3 つの CSS スタイル シート。 このアプリケーションは、アプリケーションをインターネットへの接続を使用してアクセスできるすべてのユーザーがこの時点で、web ホスト プロバイダーをデプロイする準備ができました!


ディスカッション、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-cs.md)チュートリアルでは、web ホスト プロバイダーにコピーする必要があるファイルがわかっています。 (どのようなファイルをコピーすることを思い出してくださいは、アプリケーションが自動的にまたは明示的にコンパイルされたかに依存)。しかし、開発環境 (当社のデスクトップ) から運用環境 (web ホスト プロバイダーによって管理される web サーバー) まで、ファイルを得でしょうか。 [ **F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)はネットワーク経由で 1 台のコンピューターから別のファイルをコピーするため、一般的に使用されるプロトコルです。 もう 1 つのオプションは、FrontPage Server Extensions (FPSE) です。 このチュートリアルでは、スタンドアロン FTP クライアント ソフトウェアを使用して、開発環境から運用環境に必要なファイルを展開するについて説明します。

> [!NOTE]
> Visual Studio には、FTP を使用して web サイトを発行するためのツールが含まれています。次のチュートリアルでは、これらのツールと FPSE を使用するツールを見てがについて説明します。


FTP の必要がありますを使用してファイルをコピーする、 *FTP クライアント*開発環境にします。 FTP クライアントが実行しているコンピューターにインストールされているコンピューターからファイルをコピーするように設計されたアプリケーション、 *FTP サーバー*します。 (Web ホスト プロバイダーは、ほとんどと同様に、FTP 経由でファイル転送をサポートする場合は、web サーバーで実行されている FTP サーバー)。さまざまな FTP クライアント アプリケーションは使用できます。 Web ブラウザーは、FTP クライアントとしても倍精度浮動小数点ことができます。 私のお気に入りの FTP クライアントとこのチュートリアルで使用する 1 つは[FileZilla](http://filezilla-project.org/)Windows、Linux、および mac コンピューターで利用可能な無料のオープン ソースの FTP クライアント。 任意の FTP クライアントが機能、のでどうぞご自由に最も慣れている場合どのようなクライアントを使用します。

に沿ってをフォローしている場合は必要がある前に、web ホスト プロバイダーでアカウントを作成することができますこのチュートリアルまたは完了以降。 前述の前のチュートリアルでは、web ホスト プロバイダーの企業の一連の幅広い価格、機能、およびサービスの品質とがあります。 使用するこのチュートリアル シリーズの[割引 ASP.NET](http://discountasp.net) web ホストとして、プロバイダーがに従ってかまいません任意の web ホスト プロバイダーでは、サイトが開発した ASP.NET バージョンをサポートしている限り、します。 (これらのチュートリアル使用して作成された ASP.NET 3.5。)また、ため、ファイル、web ホスト プロバイダーにコピーします FTP を使用して、このチュートリアルと将来的にでは、web ホスト プロバイダーが、web サーバーへの FTP アクセスをサポートしています。 ほぼすべての web ホスト プロバイダーは、この機能を提供しますが、サインアップする前に再確認する必要があります。

## <a name="deploying-the-book-review-web-application-project"></a>書籍レビューの Web アプリケーション プロジェクトを配置します。

書籍レビューの web アプリケーションの 2 つのバージョンがあることを思い出してください。 1 つの Web アプリケーション プロジェクト モデル (BookReviewsWAP) およびその他の Web サイト プロジェクト モデル (BookReviewsWSP) を使用してを使用して実装します。 プロジェクトの種類は、サイトが自動的にまたは明示的にコンパイルされ、そのコンパイル モデルでは、展開する必要があるファイルによって決まるかどうかに影響します。 そのため、究明する BookReviewsWAP と BookReviewsWSP プロジェクトを個別に、展開する、BookReviewsWAP 以降します。 ご協力を行っていない既に場合は、これら 2 つの ASP.NET アプリケーションをダウンロードします。

移動して BookReviewsWAP プロジェクトを起動、`BookReviewsWAP`フォルダーをダブルクリックして、`BookReviewsWAP.sln`ファイル。 プロジェクトを展開する前に、ビルド、ソース コードに変更が、コンパイルされたアセンブリに含まれることを確保するために重要ですが。 プロジェクトをビルドするビルド メニューに移動し、ビルド BookReviewsWAP メニュー オプションを選択します。 プロジェクトのソース コードを 1 つのアセンブリにコンパイルこの`BookReviewsWAP.dll`に配置される`Bin`フォルダー。

必要なファイルを展開する準備ができました! FTP クライアントを起動し、web ホスト プロバイダーの web サーバーに接続します。 (Web ホスト会社にサインアップするときに、電子メールが送信されます、FTP サーバーに接続する方法については、FTP サーバーだけでなく、ユーザー名とパスワードのアドレスが含まれます。)

デスクトップから、次のファイルを web ホスト プロバイダーのルート web サイト フォルダーにコピーします。 プロバイダーをホストする web サーバーに FTP、web では web サイトのルート ディレクトリにあります。 ただし、一部の web ホスト プロバイダーがという名前のサブフォルダーをある`www`または`wwwroot`web サイトのファイルのルート フォルダーとして機能します。 最後に、ファイルを FTPing ときにする必要があります - 運用環境に対応するフォルダー構造を作成する、`Bin`フォルダー、`Fiction`フォルダー、`Images`フォルダー、という具合です。

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- すべての内容、`Styles`フォルダー
- すべての内容、`Images`フォルダー (とそのサブフォルダーでは、 `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

図 1 は、必要なファイルがコピーされた後に、FileZilla を示します。 FileZilla では、左側と右側のリモート コンピューター上のファイルをローカル コンピューター上のファイルが表示されます。 図 1 に示す、ASP.NET のソース コード ファイルなど、`About.aspx.cs`はローカル コンピューター (開発環境) に、コード ファイルを使用する場合に展開する必要がないために、web ホスト プロバイダー (運用環境) にコピーされていません。明示的なコンパイルします。

> [!NOTE]
> それらは無視されますと、実稼働サーバー上のソース コード ファイルがある場合も問題はありません。 ASP.NET は、web サイトの訪問者がアクセス可能なソース コード ファイルが、実稼働サーバー上に存在する場合でもがれないように、既定でソース コード ファイルへの HTTP 要求が禁止されます。 (ユーザーがアクセスしようしている場合は、`http://www.yoursite.com/Default.aspx.cs`のファイル - これらの型を説明するエラー ページが表示されます`.cs`ファイル - が禁止されています)。


[![FTP クライアントを使用して、デスクトップから Web ホスト プロバイダーの Web サーバーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**図 1**: FTP クライアントを使用して、自分のデスクトップから Web ホスト プロバイダーの Web サーバーに必要なファイルをコピーする ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))。


サイトの配置後に少しサイトをテストします。 かどうかドメイン名を購入し、DNS の設定を構成、適切にドメイン名を入力して、サイトにアクセスすることができます。 また、web ホスト プロバイダーする必要がありますが指定する、サイトへの URL にするようになります*accountname*.*webhostprovider*.com または*webhostprovider*.com/*accountname*します。 たとえば、ASP.NET の割引でアカウント URL は:`http://httpruntime.web703.discountasp.net`します。

図 2 は、デプロイの書籍レビューのサイトを示します。 表示されている割引 ASP に注意してください。NET のサーバーで`http://httpruntime.web703.discountasp.net`します。 この時点で自分の web サイトを表示、インターネットへの接続を持つすべてのユーザーでした。 私たちの予想どおり、サイトの外観や開発環境でテストしているときと同様に動作します。

> [!NOTE]
> アプリケーションの表示時間がかかることを確認するときにエラーが発生した場合は、ファイルの正しいセットをデプロイします。 次に、問題に関するすべての手がかりを表示するかどうかに表示するエラー メッセージを確認します。 次に、web ホスト会社のヘルプデスクを有効にするか適切なフォーラムに質問を投稿、 [ASP.NET フォーラム](https://forums.asp.net/)します。


[![インターネット接続があればだれでも、書籍レビューのサイトがアクセスできるようになりました](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**図 2**: インターネット接続があればだれでも、書籍レビューのサイトがアクセスできるようになりました ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))。


## <a name="deploying-the-book-review-web-site-project"></a>書籍レビューの Web サイト プロジェクトを配置します。

コンパイル済みアセンブリではありません、BookReviewsWSP Web サイト プロジェクトなどの自動コンパイルを使用する ASP.NET アプリケーションをデプロイするときに、`Bin`フォルダー。 結果として、運用環境に web アプリケーションのソース コード ファイルをデプロイする必要があります。 このプロセスを見てみましょう。

ように、Web アプリケーション プロジェクトでは、最初のビルド、アプリケーションを展開する前にすることをお勧めします。 Web サイト プロジェクトをビルドして、アセンブリが作成されることはできません、中には、ページのコンパイル時エラーのチェックします。 検出すること、サイト、サイトにアクセスするのではなく、これらのエラーの検索を開始する優れた!

プロジェクトを正常に作成した後は、FTP クライアントを使用して、web ホスト プロバイダーのルート web サイト フォルダーに次のファイルをコピーします。 運用環境に対応するフォルダー構造を作成する必要があります。

> [!NOTE]
> プロジェクトは BookReviewsWSP プロジェクトの配置を試したい BookReviewsWAP が既に展開されている場合は、まず BookReviewsWAP をデプロイするときにアップロードされたファイル、web サーバー上のすべてを削除して、BookReviewsWSP のファイルを展開します。


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- すべての内容、`Styles`フォルダー
- すべての内容、`Images`フォルダー (とそのサブフォルダーでは、 `BookCovers`)
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

図 3 は、必要なファイルをコピーした後、FileZilla を示します。 ご覧のように、ASP.NET ソース コード ファイルなど`About.aspx.cs`は、コード ファイルを自動を使用する場合に展開する必要があるため、ローカル コンピューター (開発環境) と web ホスト プロバイダー (運用環境) の両方に存在コンパイルします。


[![FTP クライアントを使用して、デスクトップから Web ホスト プロバイダーの Web サーバーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**図 3**: FTP クライアントを使用して、自分のデスクトップから Web ホスト プロバイダーの Web サーバーに必要なファイルをコピーする ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))。


ユーザー エクスペリエンスは、アプリケーションのコンパイル モデルの影響を受けません。 同じ ASP.NET ページにアクセスして、外観し動作は同じ Web アプリケーション プロジェクト モデルまたは Web サイト プロジェクト モデルを使用して、web サイトが作成されたかどうか。

### <a name="updating-a-web-application-on-production"></a>運用環境で Web アプリケーションを更新しています

Web アプリケーションの開発とデプロイは、1 回限りのプロセスではありません。 などの書籍レビューの web サイトを作成するときにさまざまなページを構築し、パーソナル コンピューター (開発環境) に付属のコードを記述しました。 特定の安定した状態に達するとを展開したアプリケーション、サイトにアクセスし、自分のレビューを読み取る他のユーザーができるようにします。 展開がこのサイトで自分の開発の末尾をマークしません。 書籍レビューをさらを追加またはレートの書籍、訪問者を許可するなどの新機能を実装または独自のコメントを残すことがあります。 このような拡張機能の開発環境で開発し、完了すると、展開する必要があります。 したがって、開発とデプロイ、cyclical します。 アプリケーションを開発し、それをデプロイします。 サイトがライブときに、運用環境では、新機能が追加され、アプリケーションを再デプロイが必要になります時間の経過と共にバグが修正されました。 これに、という具合です。

想像どおり、のみ追加または変更されたファイルをコピーする必要がある web アプリケーションを再デプロイするときにします。 変更されていないページを再デプロイする必要はありませんまたは (行っても問題はありませんが)、サーバーまたはクライアント側がファイルをサポートします。

> [!NOTE]
> プロジェクトに新しい ASP.NET ページを追加またはコードに関連する変更すると、いつでも、内のアセンブリを更新するプロジェクトをリビルドする必要がありますが明示的なコンパイルを使用する場合に注意する必要があります、`Bin`フォルダー。 その結果、(その他の新規および更新されたコンテンツと共に) の運用上の web アプリケーションを更新するときに、運用環境にこの更新されたアセンブリをコピーする必要があります。


変更されているいずれかを理解することも、`Web.config`または内のファイル、`Bin`ディレクトリが停止し、web サイトのアプリケーション プールを再起動します。 使用して、セッション状態に格納されている場合、`InProc`モード (既定)、サイトの訪問者は、これらのキー ファイルが変更されるたびに、そのセッションの状態に失われます。 この危険を避けるためには、検討を使用してセッションを格納する、`StateServer`または`SQLServer`モード。 このトピックの詳細については、読み取る[セッション状態モード](https://msdn.microsoft.com/library/ms178586.aspx)します。

最後に、アプリケーションを再デプロイが実行できる任意の場所数秒から運用環境にコピーする必要のあるファイルのサイズと数に応じて、数分に注意してください。 この期間中に、サイトにアクセスするユーザーは、エラーや不適切な動作を発生可能性があります。 「オフにできます」アプリケーション全体という名前のページを追加することで`App_Offline.htm`をユーザーに説明するアプリケーションのルート ディレクトリに、サイト メンテナンス (またはすべて) ですになることをバックアップする直後。 ときに、`App_Offline.htm`ファイルが存在する、ASP.NET ランタイムは、そのページにすべての着信要求をリダイレクトします。

## <a name="summary"></a>まとめ

Web アプリケーションを展開するには、開発環境から運用環境に必要なファイルをコピーする必要があります。 ファイルがネットワーク経由で転送する最も一般的な方法は、ファイル転送プロトコル (FTP)、およびほとんどの web ホスト プロバイダーは、web サーバーへの FTP アクセスをサポートします。 このチュートリアルでは、FTP クライアントを使用して、web サーバーに必要なファイルをデプロイする方法を説明しました。 デプロイすると、web サイトが参照できますすべてのユーザー接続でインターネットに!

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アプリ\_Offline.htm と「IE のわかりやすいエラー」機能を回避します。](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [セッション状態モード](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [前へ](determining-what-files-need-to-be-deployed-cs.md)
> [次へ](deploying-your-site-using-visual-studio-cs.md)
