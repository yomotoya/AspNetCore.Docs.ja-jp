---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: IIS と ASP.NET 開発サーバー (VB) の違いをコア |Microsoft ドキュメント
author: rick-anderson
description: ローカルで ASP.NET アプリケーションをテストする場合は、ASP.NET 開発 Web サーバーを使用している可能性があります。 ただし、実稼働 web サイトは、最も可能性の高い pow しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887549"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>IIS と ASP.NET 開発サーバー (VB) のコア違い
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> ローカルで ASP.NET アプリケーションをテストする場合は、ASP.NET 開発 Web サーバーを使用している可能性があります。 ただし、実稼働 web サイトでは、最も可能性の高い電源 IIS です。 これらの web サーバーが、要求を処理する方法のいくつか違いがあるし、これらの違いは重要な結果を持つことができます。 このチュートリアルより密接の違いの一部について説明します。


## <a name="introduction"></a>はじめに

ユーザーが、ASP.NET アプリケーションにアクセスするたびに、ブラウザーは、web サイトに要求を送信します。 その要求が生成し、要求されたリソースの内容を取得するには、ASP.NET ランタイムと連携して、web サーバー ソフトウェアで取得します。 [**すれば**インターネット**すれば**情報**S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services)を一般的なインターネット ベースの機能を提供するサービスのスイートにはWindows サーバー。 IIS は実稼働環境での ASP.NET アプリケーションの最も一般的に使用される web サーバーです。ASP.NET アプリケーションが機能するように、web ホスト プロバイダーによって使用されている web サーバー ソフトウェアではほとんどの場合です。 これは IIS をインストールする必要がありますが、IIS を開発環境で web サーバー ソフトウェアとして使用もことができ、それを正しく構成します。


ASP.NET 開発サーバーは、代替 web サーバー オプションに、開発環境です。付属していて、Visual Studio に統合します。 Web アプリケーションは、IIS を使用するように構成されましたが、しない限り、ASP.NET 開発サーバーが自動的に開始および初めて Visual Studio 内から web ページにアクセスする web サーバーとして使用します。 バックアップで作成したデモ web アプリケーション、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-vb.md)チュートリアルが IIS を使用して構成されていない両方のファイル システム ベースの web アプリケーションです。 そのため、Visual Studio 内からこれらの web サイトのいずれかへのアクセス、ASP.NET 開発サーバーが使用されます。

理想の世界で、開発環境と運用環境は同じです。 ただし、前のチュートリアルで説明したよう珍しくありませんの環境が異なる構成設定にします。 環境で別の web サーバーのソフトウェアを使用してアプリケーションを展開するときに考慮対象としてに考慮しなければならない別の変数を追加します。 このチュートリアルでは、IIS と ASP.NET 開発サーバーの主な違いについて説明します。 これらの相違点があるため、開発環境で問題なく実行されるコード、例外をスローまたは実稼働環境で実行されるときの動作が異なる場合。

## <a name="security-context-differences"></a>セキュリティ コンテキストの相違点

Web サーバー ソフトウェアは、受信要求を処理するたびに、その要求を特定のセキュリティ コンテキストを関連付けます。 このセキュリティ コンテキスト情報は使用、オペレーティング システムによって、要求によってどのような操作は許容を決定します。 たとえば、ASP.NET ページには、ディスク上のファイルにいくつかのメッセージをログに記録するコードがあります。 エラーなしで実行するには、この ASP.NET ページで、セキュリティ コンテキストは、適切なファイル システム レベルの権限、つまりに対する書き込みアクセス許可ファイルが必要です。

ASP.NET 開発サーバーは、受信要求を現在ログオンしているユーザーのセキュリティ コンテキストに関連付けます。 管理者として、デスクトップにログオンしている場合、ASP.NET 開発サーバーによって処理される ASP.NET ページは、管理者と同じアクセス権があります。 ただし、IIS によって処理される ASP.NET 要求は、特定のマシン アカウントに関連付けられます。 既定では、web ホスト プロバイダーは、各顧客の一意のアカウント構成可能性がありますが、ネットワーク サービスのマシン アカウントが IIS バージョン 6 および 7 で使用します。 さらに、web ホスト プロバイダーがこのコンピューターのアカウントにアクセス許可が制限を指定する可能性があります。 最終的な結果は、開発環境ではエラーなく実行されますが、実稼働環境でホストされている場合、承認に関連する例外が生成される web ページがあります。

ページのユーザーが最新の日付と時刻を格納するディスク上のファイルを作成する書評 web サイトに作成しましたアクションでこの種類のエラーを表示するには、表示、*学べる自分で ASP.NET 3.5 24 時間以内に*を確認します。 どうしたら、開く、`~/Tech/TYASP35.aspx`ページし、次のコードを追加、`Page_Load`イベントのハンドラー。

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [ `File.WriteAllText`メソッド](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx)存在しませんし、指定された内容を書き込む場合は、新しいファイルを作成します。 ファイルが既に存在する場合は、既存のコンテンツが上書きされます。


次を参照してください、*学べる自分で ASP.NET 3.5 24 時間以内に*book ASP.NET 開発サーバーを使用して、開発環境での確認 ページ。 ログインするいると仮定するとを作成し、web でテキスト ファイルを変更するには、適切なアクセス権を持つアカウントを使用してコンピューターにログオン アプリケーションのルート ディレクトリ、book レビューが表示されますが、以前と同じ日付と時刻、ユーザーのたびに、ページが閲覧 IP アドレスが格納されている、`LastTYASP35Access.txt`ファイル。 このファイルに、ブラウザーをポイントします。図 1 に示すいずれかのようなメッセージが表示されます。


[![テキスト ファイルには、最後の日付と時刻を Book レビューがアクセスされたが含まれています。&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**図 1**: テキスト ファイルには、最後の日付と時刻を Book レビューがアクセスされたが含まれています ([フルサイズのイメージを表示するをクリックして](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


実稼働環境に web アプリケーションを展開し、ホストにアクセス*学べる自分で ASP.NET 3.5 24 時間以内に*書籍の確認 ページ。 この時点で書籍の確認 ページには、normal または図 2 に示すように、エラー メッセージとしてか表示されます。 一部の web ホスト プロバイダーは、エラーが発生せず、ページのケースが使用される、匿名 ASP.NET コンピューター アカウントに対する書き込みアクセス許可を付与します。 ただし、web ホスト プロバイダーが匿名アカウントへの書き込みアクセスを禁止する場合は、 [ `UnauthorizedAccessException`例外](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)場合に発生するが、`TYASP35.aspx`ページが、現在の日付と時刻を作成しようとしています。、`LastTYASP35Access.txt`ファイル。


[![IIS によって使用される既定マシン アカウントには、ファイル システムに書き込むアクセス許可がありません。](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**図 2**: の既定のマシン アカウントで使用される IIS はある許可ではなく、ファイル システムへの書き込みを ([フルサイズのイメージを表示するをクリックして](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


良いニュースは、ほとんどの web ホスト プロバイダーで、ある種の web サイトでファイル システム アクセス許可を指定できるようにするアクセス許可ツールです。 ルート ディレクトリに匿名の ASP.NET アカウント書き込みのアクセス権を付与し、書籍の確認 ページを再確認します。 (必要な場合は、web ホスト プロバイダーに問い合わせてくださいで既定の ASP.NET アカウントに対する書き込みアクセス許可を付与する方法です。)この時間がエラーなくページを読み込む必要があります、`LastTYASP35Access.txt`ファイルを正常に作成する必要があります。

Take 離れたここでは、ASP.NET 開発サーバーは、IIS と異なるセキュリティ コンテキストで動作するためであること、ファイル システムに対する読み取りまたは書き込みする ASP.NET ページからの読み取りまたは Windows イベント ログに書き込むまたは読み取りや書き込みを Windows に考えられる レジストリでは、開発で期待どおりに機能が、実稼働環境で例外が生成されます。 Web アプリケーションを構築、共有 web ホスティング環境に、展開時にない読み取りまたは書き込み、イベント ログまたは Windows レジストリに。 また、ASP.NET ページからの読み取りまたは読み取りを許可し、実稼働環境で、適切なフォルダーに対する権限を記述する必要があります、ファイル システムに書き込むことを書き留めます。

## <a name="differences-on-serving-static-content"></a>静的なコンテンツの違い

IIS と ASP.NET 開発サーバーの間のもう 1 つの主要な違いは、静的なコンテンツの要求を処理する方法です。 ASP.NET ページ、画像、または JavaScript ファイルでは、ASP.NET 開発サーバーに付属しているすべての要求は、ASP.NET ランタイムによって処理されます。 既定では、IIS などが ASP.NET web ページ、Web サービスなど、ASP.NET リソースに対する要求を受信すると、ASP.NET ランタイムがのみ呼び出します。 -イメージ、CSS ファイル、JavaScript ファイル、PDF ファイル、ZIP ファイル、および、like - 静的なコンテンツの要求は、ASP.NET ランタイムの関与なしに IIS によって取得されます。 (、IIS が静的なコンテンツを提供するときに、ASP.NET のランタイムで動作するように指示することは。 詳細については、このチュートリアルでは、「実行フォーム ベース認証と IIS 7 での静的ファイルの URL の認証」セクションを参照してください)。

ASP.NET ランタイムでは、さまざまな (要求元を識別する) 認証と承認 (要求元に、要求されたコンテンツを表示するアクセス許可がある場合を決定する) など、要求されたコンテンツを生成する手順を実行します。 認証の一般的な形式は*フォーム ベース認証*、テキスト ボックスに、web ページ上の - 通常はユーザー名とパスワードの資格情報を入力することで識別される場合、ユーザーにします。 その資格情報の検証時に、web サイトを格納、*認証チケット*が後続の要求ごとに、web サイトに送信され、ユーザーを認証するために使用されるは、ユーザーのブラウザーの cookie。 さらを指定することは*URL 承認*のルールをどのようなユーザーまたはできますが、特定のフォルダーにアクセスできません。 多くの ASP.NET web サイトは、フォーム ベース認証をユーザー アカウントをサポートし、認証されたユーザーまたは特定のロールに属しているユーザーにアクセスできるサイトの部分を定義するには URL 承認を使用します。

> [!NOTE]
> ASP の完全な検査します。NET のフォーム ベースの認証、URL の承認、およびその他のユーザー アカウントに関連する機能を必ずチェック アウト my [web サイトのセキュリティ チュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。


フォーム ベースの承認を使用してユーザー アカウントをサポートし、使用している URL の承認、認証されたユーザーのみを許可するように構成フォルダーが含まれる web サイトを検討してください。 このフォルダーには、ASP.NET ページが含まれています。 と PDF ファイルを表示できるの PDF ファイルと目的では、認証されたユーザーのみがあることを想像してください。

訪問者が自分のブラウザーのアドレス バーに直接 URL を入力してこれらの PDF ファイルのいずれかを表示しようとすると、どうなりますか。 を調べるにみましょう書評サイトに新しいフォルダーを作成、PDF ファイルをいくつかの追加および匿名ユーザーをこのフォルダーへのアクセスを禁止する URL 承認を使用するサイトを構成します。 デモ アプリケーションをダウンロードする場合と呼ばれるフォルダーを作成しましたが表示されます`PrivateDocs`から PDF を追加し、 [web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)(適切にどのように!)。 `PrivateDocs`フォルダーも含まれて、`Web.config`ファイルを匿名ユーザーを拒否する URL 承認規則を指定します。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

ルート ディレクトリにある Web.config ファイルを更新することで、フォーム ベース認証を使用する web アプリケーションを構成した最終的に、置換します。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

代入:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

ASP.NET 開発サーバーを使用して、サイトにアクセスし、ブラウザーのアドレス バーで PDF ファイルのいずれかに直接の URL を入力します。 : URL ようになりますが、このチュートリアルに関連付けられている web サイトをダウンロードしている場合 `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

ファイルの ASP.NET 開発サーバーに要求を送信するブラウザーは、アドレス バーにこの URL を入力します。 ASP.NET 開発サーバーは要求を処理するため、ASP.NET ランタイムに渡します。 そのため、まだログインしていないこと、`Web.config`で、`PrivateDocs`フォルダーが匿名アクセスを拒否するように構成は、ASP.NET ランタイムに自動的にリダイレクト us ログイン ページに`Login.aspx`(図 3 を参照してください)。 ASP.NET には、ログイン ページにユーザーをリダイレクトするときに、 `ReturnUrl` querystring パラメーター ページを示す、ユーザーがしようとして表示します。 ユーザーが正常にログインした後は、このページに戻ってことができます。


[![承認されていないユーザーがログイン ページに自動的にリダイレクト](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**図 3**: 承認されていないユーザーがログイン ページに自動的にリダイレクト ([フルサイズのイメージを表示するをクリックして](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


これで実稼働環境でこの動作を確認してみましょう。 アプリケーションを配置しで Pdf のいずれかに直接の URL を入力、`PrivateDocs`実稼働環境でのフォルダーです。 これには、ブラウザー ファイルの IIS 要求を送信するように求められます。 静的ファイルが要求したため、IIS は取得し、ASP.NET ランタイムを呼び出すことがなく、ファイルを返します。 その結果、なかった URL 承認チェックを実行します。想定しているプライベート PDF の内容は、ファイルへの直接の URL を知っている人にアクセスできます。


[![匿名ユーザーがファイルに直接 URL を入力してプライベート PDF ファイルをダウンロードできます。](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**図 4**: 匿名ユーザーにダウンロードできるプライベート PDF ファイルで入力するダイレクト URL、ファイル ([フルサイズのイメージを表示するをクリックして](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>IIS 7 での静的ファイルにフォーム ベースの認証と URL の認証を実行します。

承認されていないユーザーからの静的なコンテンツを保護に使用できる手法のいくつかあります。 IIS 7 が導入された、*統合パイプライン*IIS のワークフローと、ASP.NET ランタイムのワークフローを結婚します。 簡単に言うと、IIS、ASP.NET ランタイムの認証および承認モジュール (PDF ファイルと同様に、静的なコンテンツを含む) すべての着信要求を呼び出すために指示できます。 統合パイプラインを使用する web サイトを構成する方法を見つけて、web ホスト プロバイダーに問い合わせてください。

使用する IIS が構成されている統合パイプラインを追加する、次のマークアップ、`Web.config`ルート ディレクトリにファイル。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

このマークアップでは、IIS 7 ASP.NET ベースの認証と承認モジュールを使用するように指示します。 アプリケーションを再配置し、PDF ファイルを再アクセスしてください。 IIS が要求を処理するときに、この時間になります、ASP.NET ランタイムの認証と承認ロジックを要求を検査する機会です。 認証されたユーザーのみがで内容を表示する権限があるため、`PrivateDocs`フォルダー、匿名ユーザーは自動的にログイン ページにリダイレクト (戻るは図 3 を参照してください)。

> [!NOTE]
> Web ホスト プロバイダーは、IIS 6 を使用している場合は、統合パイプラインの機能を使用することはできません。 1 つの回避策は、プライベート ドキュメントを HTTP アクセスを禁止するフォルダーに配置する (など`App_Data`)、これらのドキュメントを提供するページを作成します。 このページが呼び出された可能性がある`GetPDF.aspx`、され、クエリ文字列パラメーターを通じて PDF の名前が渡されます。 `GetPDF.aspx`ページが最初にいることを確認ユーザー ファイルを表示するアクセス許可を持つ場合は、使用、 [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx)要求されている PDF ファイルの内容を要求するクライアントに送信するメソッド。 この手法のように動作 IIS 7 の統合パイプラインを有効にするがない場合。


## <a name="summary"></a>まとめ

運用環境で web アプリケーションでは、Microsoft の IIS web サーバー ソフトウェアを使用してホストされています。 開発環境でただし、アプリケーションがホストする IIS または ASP.NET 開発サーバーを使用しています。 理想的には、ミックスで変数を追加する別のソフトウェアを使用しているために、両方の環境で、同じ web サーバー ソフトウェアを使用してください。 ただし、ASP.NET 開発サーバーの使いやすさは、魅力的な選択肢、開発環境でします。 良いニュースはことのみ IIS と ASP.NET 開発サーバーの間で、いくつかの基本的な違いがある場合、これらの違いを認識している手順を実行できますにより、アプリケーションの動作と同じ機能の方法に関係なく、環境。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [IIS 7.0 で ASP.NET 統合](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [IIS 7 でのコンテンツのすべての種類で ASP.NET フォーラムの認証を使用して](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx)(ビデオ)
- [Visual Web Developer で web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [前へ](common-configuration-differences-between-development-and-production-vb.md)
> [次へ](deploying-a-database-vb.md)
