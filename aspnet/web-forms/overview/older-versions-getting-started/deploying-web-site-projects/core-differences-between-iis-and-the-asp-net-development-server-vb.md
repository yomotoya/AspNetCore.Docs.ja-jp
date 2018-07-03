---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: IIS と ASP.NET 開発サーバー (VB) の違いをコア |Microsoft Docs
author: rick-anderson
description: ローカルでの ASP.NET アプリケーションをテストする場合は、ASP.NET 開発 Web サーバーを使用している可能性があります。 ただし、運用 web サイトは、最も可能性の高い pow です.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: d16e4d20f2e1fe85b6844ea714546fbae8c95b4e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377033"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>IIS と ASP.NET 開発サーバー (VB) の間の中心的違い
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> ローカルでの ASP.NET アプリケーションをテストする場合は、ASP.NET 開発 Web サーバーを使用している可能性があります。 ただし、運用 web サイトは、最も可能性の高い電源 IIS です。 これらの web サーバーが、要求を処理する方法のいくつか違いがあるし、これらの相違点が重要な影響を及ぼすことができます。 このチュートリアルより密接な相違点について説明します。


## <a name="introduction"></a>はじめに

ユーザーが、ASP.NET アプリケーションにアクセスするたびに、ブラウザーは、web サイトに要求を送信します。 その要求を生成し、要求されたリソースのコンテンツを返す、ASP.NET ランタイムと連携して、web サーバー ソフトウェアによって取得されます。 [**は**インターネット**は**情報**S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services)の一般的なインターネット ベースの機能を提供するサービスのスイートにはWindows サーバー。 IIS は運用環境以外での ASP.NET アプリケーションの最もよく使用される web サーバーです。ほとんどの場合は、ASP.NET アプリケーションをホストする web ホスト プロバイダーによって使用されている web サーバーのソフトウェアです。 IIS をインストールする必要がありますが、IIS を開発環境で web サーバーのソフトウェアとして使用もでき、適切に構成します。


ASP.NET 開発サーバーは、開発環境用の代替 web サーバー オプションです。同梱され、Visual Studio に統合します。 IIS を使用する web アプリケーションが構成されている場合を除き、ASP.NET 開発サーバーが自動的に開始および web サーバーとして使用する初めての Visual Studio 内から web ページを参照してください。 バックアップで作成したデモ web アプリケーション、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-vb.md)チュートリアルが IIS を使用して構成されていない両方のファイル システム ベースの web アプリケーション。 そのため、Visual Studio 内からこれらの web サイトのいずれかにアクセスすると、ASP.NET 開発サーバーが使用されます。

理想の世界では、開発および運用環境を同一になります。 ただし、前のチュートリアルで説明したように異なる構成設定が環境にとって珍しいことではありません。 別の web サーバー ソフトウェアを使用して、環境でアプリケーションを展開する際の考慮事項に考慮すべきもう 1 つの変数を追加します。 このチュートリアルでは、IIS と ASP.NET 開発サーバーの間の主な違いについて説明します。 これらの違いにより、開発環境で問題なく実行されるコードが例外をスローまたは実稼働環境で実行されるときの動作が異なるシナリオがあります。

## <a name="security-context-differences"></a>セキュリティ コンテキストの相違点

Web サーバー ソフトウェアは、受信要求を処理するたびに、特定のセキュリティ コンテキストをその要求を関連付けます。 オペレーティング システムによっては、このセキュリティ コンテキスト情報を使用して要求によってどのようなアクションは許容されます。 を決定します。 たとえば、ASP.NET ページには、ディスク上のファイルをいくつかのメッセージを記録するコードがあります。 エラーなしで実行するには、この ASP.NET ページで、セキュリティ コンテキストが適切なファイル システム レベルのアクセス許可、つまり書き込みでそのファイルのアクセス許可が必要です。

ASP.NET 開発サーバーは、受信要求を現在ログオンしているユーザーのセキュリティ コンテキストに関連付けます。 管理者として、デスクトップにログオンしている場合、ASP.NET 開発サーバーによって処理される ASP.NET ページは、管理者と同じアクセス権があります。 ただし、IIS によって処理される ASP.NET 要求は、特定のマシン アカウントに関連付けられます。 既定では、web ホスト プロバイダーは、顧客ごとに一意のアカウント構成可能性がありますが IIS バージョン 6 および 7、によってネットワーク サービスのコンピューター アカウントが使用します。 さらに、web ホスト プロバイダーがこのコンピューター アカウントにアクセス許可が制限を指定する可能性があります。 最終的な結果は、開発環境でエラーなしで実行が、運用環境でホストされている場合の承認に関連する例外を生成する web ページがあります。

ユーザーが最新の日付と時刻を格納するディスク上のファイルを作成する書籍レビューの web サイトでページを作成しましたアクションでこの種類のエラーを表示するには、表示、*教える自分で ASP.NET 3.5 in 24 時間*を確認します。 作業を進めるには、開く、`~/Tech/TYASP35.aspx`ページし、次のコードを追加、`Page_Load`イベント ハンドラー。

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [ `File.WriteAllText`メソッド](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx)に指定されたコンテンツを書き込みますが存在しない場合は、新しいファイルを作成します。 ファイルが既に存在する場合は、既存のコンテンツが上書きされます。


次を参照してください、*教える自分で ASP.NET 3.5 in 24 時間*ASP.NET 開発サーバーを使用して開発環境で書籍の確認 ページ。 作成し、web でのテキスト ファイルを変更するには、適切なアクセス権を持つアカウントを使用してコンピューターにアプリケーションのルート ディレクトリ、書籍レビュー、以前と同じ表示されますが、ページが毎回アクセス日付と時刻、およびユーザーのログインするいると仮定 IP アドレスが格納されている、`LastTYASP35Access.txt`ファイル。 このファイルをお使いのブラウザーをポイントします。図 1 のようなメッセージが表示されます。


[![テキスト ファイルには、最後の日付と時刻の書籍レビューのアクセスが含まれています。&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**図 1**: テキスト ファイルには、最後の日付と時刻の書籍レビューのアクセスが含まれています ([フルサイズの画像を表示する をクリックします](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))。


Web アプリケーションを運用環境にデプロイして、ホストされているアクセス*教える自分で ASP.NET 3.5 in 24 時間*書籍の確認 ページ。 この時点で書籍の確認 ページには、normal または図 2 に表示されるエラー メッセージとしても表示されます。 一部の web ホスト プロバイダーは、ASP.NET による匿名のコンピューター アカウントの場合、ページはエラーなしに書き込みアクセス許可を付与します。 ただし、web ホスト プロバイダーは匿名アカウントへの書き込みアクセスを禁止する場合は、 [ `UnauthorizedAccessException`例外](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)発生する状況、`TYASP35.aspx`ページが、現在の日付と時刻を記述しようとしています。、`LastTYASP35Access.txt`ファイル。


[![IIS によって使用される既定のマシン アカウントには、ファイル システムに書き込むアクセス許可がありません。](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**図 2**: [既定のマシン アカウントによって使用 IIS がある許可ではなくファイル システムへの書き込みを ([フルサイズの画像を表示する] をクリックします](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))。


良い知らせは、ほとんどの web ホスト プロバイダーが、web サイトのファイル システム権限を指定することを許可するアクセス許可ツールのいくつかの並べ替えがあることです。 ルート ディレクトリに匿名の ASP.NET アカウント書き込みのアクセス権を付与し、書籍の確認 ページを再検討します。 (必要な場合は、web ホスト プロバイダーに問い合わせてください既定の ASP.NET アカウントに対する書き込みアクセス許可を付与する方法です。)この時間のエラーが発生せず、ページを読み込む必要があります、`LastTYASP35Access.txt`ファイルを正常に作成する必要があります。

とると、すぐここでは、ASP.NET 開発サーバーは、IIS 以外の別のセキュリティ コンテキストで動作するため、ファイル システムに対する読み取りまたは書き込みする ASP.NET ページからの読み取りまたは Windows イベント ログに書き込むまたは読み取るか、Windows への書き込み レジストリは、開発で期待どおりに動作が、運用環境での例外を生成します。 共有 web ホスト環境で web アプリケーションの構築がデプロイされるときに実行しない読み取りまたは書き込み、イベント ログまたは Windows レジストリに。 任意の ASP.NET ページからの読み取りまたは読み取りを許可し、運用環境で適切なフォルダーに対する権限を記述する必要があります、ファイル システムへの書き込みも書き留めます。

## <a name="differences-on-serving-static-content"></a>静的なコンテンツの違い

IIS と ASP.NET 開発サーバーの間のもう 1 つの主要な違いは、静的コンテンツの要求を処理する方法です。 ASP.NET ページ、画像、または JavaScript ファイルを ASP.NET 開発サーバーに付属しているすべての要求は、ASP.NET ランタイムによって処理されます。 既定では、IIS では、ASP.NET web ページや Web サービスなどなど、ASP.NET リソースの要求を受信すると、ASP.NET ランタイムがのみ呼び出します。 イメージ、CSS ファイル、JavaScript ファイル、PDF ファイル、ZIP ファイル、およびなどの-静的コンテンツの要求は、IIS で ASP.NET ランタイムが関与しない取得されます。 (、IIS の静的コンテンツを提供するときに、ASP.NET ランタイムで動作するよう指示することができます。 詳細については、このチュートリアルでは、「フォーム ベース認証を実行して IIS 7 でファイルに静的 URL 認証」セクションを参照してください)。

ASP.NET ランタイムは、さまざまな認証 (要求元を識別する)、承認 (要求元に要求されたコンテンツを表示するアクセス許可がある場合を決定する) など、要求されたコンテンツを生成する手順を実行します。 認証の一般的な形式は*フォーム ベース認証*、web ページ上のテキスト ボックスに - 通常はユーザー名とパスワードの資格情報を入力して、ユーザーが識別されるのです。 自分の資格情報の検証時に、web サイトを格納、*認証チケット*は、後続の要求ごとに、web サイトに送信されはユーザーの認証に使われますユーザーのブラウザーの cookie。 指定することはさらに、 *URL 承認*のルールをどのようなユーザーまたはできますが、特定のフォルダーにアクセスできません。 多くの ASP.NET web サイトは、ユーザー アカウントをサポートするために、認証されたユーザーまたは特定のロールに属しているユーザーにアクセスできるサイトの部分を定義したり、フォーム ベースの認証と承認の URL を使用します。

> [!NOTE]
> ASP の徹底的な調査できるようにします。NET のフォーム ベース認証、URL 承認、およびその他のユーザー アカウントに関連する機能は、チェック アウトすることを確認するマイ[web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。


フォーム ベースの承認を使用してユーザー アカウントをサポートし、フォルダー、URL の承認を使用して認証されたユーザーのみを許可するように構成する web サイトを検討してください。 このフォルダーには、ASP.NET ページが含まれています。 PDF ファイルを表示できるの PDF ファイルと目的は、認証されたユーザーのみが、想像してください。

訪問者が自分のブラウザーのアドレス バーに直接 URL を入力してこれらの PDF ファイルのいずれかを表示しようとすると、どうなりますか。 確認するには、みましょう、書籍レビューのサイトに新しいフォルダーを作成、いくつかの PDF ファイルを追加および URL 承認を使用して、匿名ユーザーがこのフォルダーにアクセスすることを禁止するサイトを構成します。 デモ アプリケーションをダウンロードする場合という名前のフォルダーを作成することが表示されます`PrivateDocs`から PDF を追加し、 [web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)(フィットする方法!)。 `PrivateDocs`フォルダーも含まれています、`Web.config`ファイルを匿名ユーザーを拒否する URL 承認規則を指定します。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

最後に、ルート ディレクトリに Web.config ファイルを更新することで、フォーム ベース認証を使用する web アプリケーションを構成しました置き換えます。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

代入:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

ASP.NET 開発サーバーを使用して、サイトにアクセスし、ブラウザーのアドレス バーで PDF ファイルのいずれかに直接 URL を入力します。 : URL はようになりますが、このチュートリアルに関連付けられている web サイトをダウンロードしている場合 `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

ファイルの ASP.NET 開発サーバーに要求を送信するブラウザーは、アドレス バーにこの URL を入力します。 ASP.NET 開発サーバーは要求を処理するため、ASP.NET ランタイムに渡します。 では、まだログインしていないことがあるため、`Web.config`で、`PrivateDocs`フォルダーへの匿名アクセスを拒否するように構成を ASP.NET ランタイムに自動的にリダイレクトします私たち、ログイン ページに`Login.aspx`(図 3 を参照してください)。 ASP.NET には、ログイン ページにユーザーをリダイレクトしたときに、`ReturnUrl`ページを示すクエリ文字列パラメーター、ユーザーが表示しようとします。 ユーザーに正常にログインした後は、このページに戻ってことができます。


[![承認されていないユーザーがログイン ページに自動的にリダイレクトされます。](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**図 3**: 承認されていないユーザーがログイン ページに自動的にリダイレクト ([フルサイズの画像を表示する をクリックします](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))。


これで運用環境でこの動作を見てみましょう。 アプリケーションを配置しで Pdf のいずれかに直接 URL を入力、`PrivateDocs`実稼働環境でのフォルダー。 これは、ファイルの IIS 要求を送信するブラウザーを求めます。 静的ファイルが要求されるため、IIS は取得し、ASP.NET ランタイムを呼び出さずに、ファイルを返します。 その結果、URL 承認チェックが実行されます。 がありません。おそらくプライベート PDF の内容では、ファイルへの直接の URL を知っている人にアクセスできます。


[![匿名ユーザーがファイルに直接 URL を入力して、プライベートの PDF ファイルをダウンロードできます。](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**図 4**: 匿名ユーザーにダウンロードできる、プライベート PDF ファイルによって入力ダイレクト URL、ファイル ([フルサイズの画像を表示する をクリックします](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))。


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>IIS 7 で静的ファイルにフォーム ベースの認証と URL の認証を実行します。

承認されていないユーザーから静的コンテンツを保護するために使用できる手法のいくつかがあります。 IIS 7 の導入、*統合パイプライン*、ASP.NET ランタイムのワークフローで IIS のワークフローを結婚します。 簡単に言うと、IIS、ASP.NET ランタイムの認証と承認モジュール (PDF ファイルなどの静的コンテンツを含む) すべての着信要求を起動するように指示できます。 統合パイプラインを使用する web サイトを構成する方法については、web ホスト プロバイダーにお問い合わせください。

統合パイプラインが次のマークアップを追加後、IIS が使用するよう構成されている、`Web.config`ルート ディレクトリ内のファイル。

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

このマークアップでは、IIS 7 ASP.NET ベースの認証と承認モジュールを使用するように指示します。 アプリケーションを再デプロイして、PDF ファイルを再度参照してください。 IIS が要求を処理するときに、この時点では、ASP.NET ランタイムの認証と承認ロジックうなります要求を検査します。 認証されたユーザーのみがコンテンツを表示する権限があるため、`PrivateDocs`フォルダー、匿名の訪問者がログイン ページに自動的にリダイレクト (戻るは図 3 を参照してください)。

> [!NOTE]
> 場合は、web ホスト プロバイダーには、IIS 6 がまだ使用して、統合パイプライン機能を使用することはできません。 1 つの回避策は、プライベート ドキュメントを HTTP アクセスを禁止するフォルダーに配置する (など`App_Data`) し、これらのドキュメントを処理するためにページを作成します。 このページを呼び出すことがあります`GetPDF.aspx`、され、クエリ文字列パラメーターで PDF の名前が渡されます。 `GetPDF.aspx`ページは、ユーザーがファイルを表示するアクセス許可を持つし、そうである場合は使用しているに最初に確認しますが、 [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx)要求された PDF ファイルの内容を要求元のクライアントに送信するメソッド。 この手法は、統合パイプラインを有効にするたくはない場合も IIS 7 の機能。


## <a name="summary"></a>まとめ

運用環境で web アプリケーションは、マイクロソフトの IIS web サーバー ソフトウェアを使用してホストされます。 開発環境でただし、アプリケーション ホストできます IIS または ASP.NET 開発サーバーを使用します。 理想的には、ミックスで変数を追加する別のソフトウェアを使用しているために、両方の環境で同じ web サーバーのソフトウェアを使用してください。 ただし、ASP.NET 開発サーバーの使いやすさは、有用な選択肢、開発環境でします。 良い知らせはことのみの IIS と ASP.NET 開発サーバーで、いくつかの基本的な違いがあるし、のこれらの違いに注意する場合は手順を実行できますアプリケーションが動作し、同じ機能を確保する方法に関係なく、環境。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [IIS 7.0 と ASP.NET の統合](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [ASP.NET フォーラムの認証を使用して、IIS 7 のコンテンツのすべての種類で](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx)(ビデオ)
- [Visual Web Developer で web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [前へ](common-configuration-differences-between-development-and-production-vb.md)
> [次へ](deploying-a-database-vb.md)
