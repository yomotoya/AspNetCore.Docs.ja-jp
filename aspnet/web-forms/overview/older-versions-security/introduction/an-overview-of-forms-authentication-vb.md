---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: フォーム認証 (VB) の概要 |Microsoft Docs
author: rick-anderson
description: 実装します。 このチュートリアルでは単なる説明から変換されます。具体的には、フォーム認証の実装を紹介します。 Web アプリケーションの w.
ms.author: aspnetcontent
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 757cdebc436a4cb799f92374744ee9cb69eb0e0b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809460"
---
<a name="an-overview-of-forms-authentication-vb"></a>フォーム認証 (VB) の概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> 実装します。 このチュートリアルでは単なる説明から変換されます。具体的には、フォーム認証の実装を紹介します。 メンバーシップとロールを簡単なフォーム認証から移行する際、まず、このチュートリアルで作成する web アプリケーションは後のチュートリアルで構築続けます。
> 
> このトピックの詳細については、このビデオを参照してください: [ASP.NET での基本的なフォーム認証の使用](# "using-basic-forms-authentication-in-aspnet")します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](security-basics-and-asp-net-support-vb.md)ASP.NET によって提供される各種の認証、承認、およびユーザー アカウントのオプションについて説明しました。 実装します。 このチュートリアルでは単なる説明から変換されます。具体的には、フォーム認証の実装を紹介します。 メンバーシップとロールを簡単なフォーム認証から移行する際、まず、このチュートリアルで作成する web アプリケーションは後のチュートリアルで構築続けます。

このチュートリアルでは、前のチュートリアルで触れましたトピック、フォーム認証ワークフローの詳細で始まります。 次に、フォーム認証の概念をデモするための ASP.NET web サイトを作成します。 次に、私たちは、サイトを構成フォーム認証を使用して、単純なログイン ページを作成および確認する方法、コードでは、ユーザーが認証されると、そうである場合、ユーザー名、ログインしているかどうかを参照してください。

フォームの認証ワークフローを web アプリケーションで有効にすると、ログインおよびログオフのページを作成して、ユーザー アカウントをサポートし、web ページを使用してユーザーを認証する ASP.NET アプリケーションのビルドのすべての重要な手順について説明します。 こうした理由を過去のプロジェクトでのフォーム認証の場合でも、エクスペリエンスの構成が既にに次に進む前に、完全には、このチュートリアルを行うには推奨は、別の時にこれらのチュートリアルが構築されるためです。

## <a name="understanding-the-forms-authentication-workflow"></a>フォーム認証のワークフローを理解します。

ASP.NET ランタイムは、ASP.NET ページや ASP.NET Web サービスなど、ASP.NET リソースの要求を処理するときに、要求は、ライフ サイクル中にイベントの数を生成します。 要求が認証されると、未処理の例外となどの場合に発生するイベントが承認されたときに発生したものと、要求の非常に開始、終了時に発生するイベントがあります。 イベントの完全な一覧を表示するを参照してください、 [HttpApplication オブジェクトのイベント](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx)します。

*HTTP モジュール*マネージ クラスの要求のライフ サイクルの特定のイベントに応答であるコードが実行されます。 ASP.NET では、バック グラウンドで重要なタスクを実行する HTTP モジュールの数が付属しています。 この記事に特に関連する 2 つの組み込み HTTP モジュールは次のとおりです。

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -は、通常、ユーザーのクッキー コレクションに含まれる、フォーム認証チケットを検査して、ユーザーを認証します。 フォーム認証チケットが存在しない場合、ユーザーは匿名です。
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -現在のユーザーの権限が要求された URL にアクセスするかどうかを決定します。 このモジュールは、アプリケーションの構成ファイルで指定された承認規則を参照して、権限を決定します。 ASP.NET にも含まれています、[持ちます](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)機関要求されたファイルの Acl を参照して決定します。

FormsAuthenticationModule は、UrlAuthorizationModule (および持ちます) を実行する前にユーザーを認証しようとします。 承認モジュールが、要求を終了し、返します、要求を行ったユーザーが要求されたリソースにアクセスする権限がない場合、 [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html)状態。 Windows 認証のシナリオでは、ブラウザーに HTTP 401 ステータスが返されます。 この状態コードの場合、ブラウザーにモーダル ダイアログ ボックスを使用して、資格情報の入力を求めます。 フォーム認証では、ただし、HTTP 401 Unauthorized ステータスが送信されないブラウザーのため、FormsAuthenticationModule は、この状態を検出し、代わりに、ユーザーをログイン ページにリダイレクトするそれを変更します (を使用して、 [HTTP 302 リダイレクト](http://www.checkupdown.com/status/E302.html)状態)。

ログイン ページの責任では、ユーザーの資格情報が有効と、フォーム認証チケットを作成し、ユーザーをページにリダイレクトを参照してくださいを試行した場合はかどうかを決定します。 FormsAuthenticationModule は、ユーザーを識別するために使用する web サイト上のページへの後続の要求に認証チケットが含まれます。


[![フォーム認証のワークフロー](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**図 01**:、フォーム認証のワークフロー ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image3.png))。


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>ページの訪問者の間での認証チケットの記憶

ログインしたらは、サイトを参照する場合に、ユーザーがログインしているが保持されるように、フォーム認証チケット要求のたびに web サーバーに送信する必要があります。 これは通常、ユーザーのクッキー コレクションに認証チケットを配置することによって実現します。 [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie)はユーザーのコンピューター上に存在し、cookie を作成した web サイトへの各要求の HTTP ヘッダーで送信される小さなテキスト ファイルです。 そのため、フォーム認証チケットが作成され、ブラウザーの cookie に格納されて、そのサイトへの後続アクセスは、ユーザーを識別するため、要求と共に認証チケットを送信します。

> [!NOTE]
> 各チュートリアルで使用されるデモ web アプリケーションは、ダウンロードとして入手できます。 このダウンロード可能なアプリケーションは、.NET Framework version 3.5 を対象となる Visual Web Developer 2008 で作成されました。 .NET 3.5 アプリケーションの対象は後の Web.config ファイルには、3.5 に固有の追加の構成要素が含まれます。 要約すると、ダウンロード可能な web アプリケーションでは、コンピューターに .NET 3.5 をインストールするがまだある場合は、web.config ファイルから 3.5 固有のマークアップを削除せずは機能しません。


Cookie の 1 つの側面は、日付と時刻が、ブラウザーが cookie を破棄するには、有効期限です。 フォーム認証 cookie の期限が切れると、ユーザーされなく認証およびため匿名のようになります。 ユーザーは、パブリックのターミナルからアクセスすること、ときに期限切れのブラウザーを閉じるときに認証チケットに仕込むする可能性があります。 自宅からすると、しかし、その同じユーザーすることがあるないように、ブラウザーの再起動前後で覚えられないよう認証チケットをそのサイトにアクセスするたびに再記録します。 この決定は、アカウントを記憶 チェック ボックスのログイン ページの形式でユーザーによる多くの場合。 手順 3 では、ログイン ページで、アカウントを記憶 チェック ボックスを実装する方法を説明します。 次のチュートリアルでは、認証チケットのタイムアウト設定の詳細を説明します。

> [!NOTE]
> Web サイトにログオンするために使用するユーザー エージェントが cookie をサポートしていないことができます。 このような場合は、ASP.NET が cookieless フォーム認証チケットを使用できます。 このモードでは、認証チケットが URL にエンコードされます。 クッキーなしの認証チケットを使用する場合と、作成され、次のチュートリアルで管理の方法を紹介します。


### <a name="the-scope-of-forms-authentication"></a>フォーム認証のスコープ

FormsAuthenticationModule は管理されている ASP.NET ランタイムの一部のコードします。 前のバージョン 7 のマイクロソフトの[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) web サーバー、IIS の HTTP パイプラインと、ASP.NET ランタイムのパイプラインの個別障害が発生しました。 簡単に言えば、IIS 6 以前のバージョンで、FormsAuthenticationModule は、ASP.NET ランタイムに要求が IIS から委任されるときにのみ実行します。 既定では、.aspx、.asmx、または .ashx の拡張子を持つページが要求されたときに、IIS はなどの静的コンテンツ自体の HTML ページと CSS とイメージ ファイルにのみ要求を渡しますは ASP.NET ランタイムを処理します。

IIS 7 では、ただしでは、統合された IIS と ASP.NET パイプライン。 呼び出すため、FormsAuthenticationModule IIS 7 をセットアップするいくつかの構成設定で*すべて*要求。 さらに、IIS 7 では、任意の種類のファイルの URL 承認規則を定義できます。 詳細については、次を参照してください。 [IIS6 との間の変更と IIS7 セキュリティ](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above)、 [Your Web プラットフォームのセキュリティ](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)、および[理解 IIS7 URL 承認](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)します。

かいつまんで言う、IIS 7 よりも前のバージョンでは、ASP.NET ランタイムによって処理されるリソースを保護するのにフォーム認証を使用することができますのみです。 同様に、URL 承認規則は、ASP.NET ランタイムによって処理されるリソースにのみ適用されます。 IIS 7 では、FormsAuthenticationModule と UrlAuthorizationModule をすべての要求には、この機能を拡張するため、IIS の HTTP パイプラインに統合できます。

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>手順 1: このチュートリアル シリーズの ASP.NET web サイトを作成します。

Visual Studio 2008 では、Microsoft の無料版で、このシリーズ全体で構築することに ASP.NET web サイトを作成できるだけ多くを達成するために[Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)します。 SqlMembershipProvider のユーザー ストアには実装、 [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)データベース。 Visual Studio 2005 または Visual Studio 2008 または SQL Server の別のエディションを使用している場合は、心配しないでくださいし、手順とほぼ同じになります - 重要な相違点を指摘します。

フォーム認証を構成することができます、前に最初の ASP.NET web サイトが必要。 まず新しいファイル システムに基づく ASP.NET web サイトを作成します。 これを実現するには、Visual Web Developer を起動ファイル メニューに移動し、新しい Web サイト ダイアログ ボックスを表示する新しい Web サイトを選択します。 ASP.NET Web サイト テンプレートを選択し、場所ドロップダウン リストをファイル システムに設定、web サイトを配置するフォルダーを選択して VB. に言語を設定 これには、Default.aspx ASP.NET ページで、アプリで新しい web サイトを作成は\_データ フォルダー、および Web.config ファイル。

> [!NOTE]
> Visual Studio プロジェクト管理の 2 つのモードをサポートしています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 Web サイト プロジェクトでは、Web アプリケーション プロジェクトで Visual Studio .NET 2002年/2003 プロジェクト アーキテクチャを模倣する-プロジェクト ファイルが含まれていて、プロジェクトのソース コードを/bin フォルダーに配置されている 1 つのアセンブリにコンパイルは、プロジェクト ファイルが不足しています。 Visual Studio 2005 最初に唯一サポートされている Web サイト プロジェクトは、Service Pack 1。 Web アプリケーション プロジェクト モデルが再入がVisual Studio 2008 には、両方のプロジェクト モデルが用意されています。 Visual Web Developer 2005 および 2008 のエディション、ただし、のみがサポート Web サイト プロジェクト。 Web サイト プロジェクト モデルは使用されます。 Express 以外のエディションを使用しているし、使用するかどうか、 [Web アプリケーション プロジェクト モデル](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)代わりを自由に行う可能性があるいくつかの相違点、画面とではなく実行する必要があります手順に表示されるものとの間に注意してください、スクリーン ショットが示すように、これらのチュートリアルで説明する手順。


[![新しいファイル システムに基づく Web サイトを作成します。](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**図 02**: New File System-Based Web サイトの作成 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image6.png))。


### <a name="adding-a-master-page"></a>マスター ページを追加します。

次に、Site.master をという名前のルート ディレクトリ内のサイトに新しいマスター ページを追加します。 [マスター ページ](https://msdn.microsoft.com/library/wtxbf3hh.aspx)ASP.NET ページに適用できるサイト全体のテンプレートを定義するページの開発者を有効にします。 マスター ページの主な利点は、サイトの全体的な外観定義できます 1 つの場所でこれにより簡単に更新したり、サイトのレイアウトを調整します。


[![マスター ページを追加するという名前の web サイトに Site.master](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**図 03**: web サイトにマスター ページという Site.master を追加 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image9.png))。


マスター ページで、ここで、サイト全体のページ レイアウトを定義します。 デザイン ビューを使用して、必要なレイアウトまたは Web コントロールを追加またはソース ビューで手動でマークアップを手動で追加することができます。 構造で使用されるレイアウトを模倣するために、マスター ページのレイアウト、  *[ASP.NET 2.0 でデータを扱う](../../data-access/index.md)* チュートリアルのシリーズ (図 4 参照)。 マスター ページを使用して[カスケード スタイル シート](http://www.w3schools.com/css/default.asp)配置と (このチュートリアルの関連するダウンロードに含まれています) を Style.css ファイルで定義された CSS 設定でスタイル。 CSS 規則が定義されているときに、次に示すマークアップからわかることはできません、ようにナビゲーション&lt;div&gt;のようにし、左側に表示が固定幅 200 ピクセルに絶対にコンテンツが配置されています。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

マスター ページは、静的なページ レイアウトやマスター ページを使用する ASP.NET ページが編集可能なリージョンの両方を定義します。 これらのコンテンツの編集可能なリージョンは、コンテンツ内で示しているようにプレース ホルダー コントロールで示されます&lt;div&gt;します。 このマスター ページが 1 つのプレース ホルダー (MainContent) が、マスター ページの複数の ContentPlaceHolders 場合があります。

上記で入力したマークアップをデザイン ビューに切り替えると、マスター ページのレイアウトを示します。 このマスター ページを使用する任意の ASP.NET ページには、MainContent リージョンのマークアップを指定する機能によりこの均一なレイアウトがあります。


[![デザイン ビューで表示した場合、マスター ページ](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**図 04**:、マスター ページ、ときに表示をデザイン ビュー ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image12.png))。


### <a name="creating-content-pages"></a>コンテンツ ページを作成します。

この時点で、web サイトに Default.aspx ページがあるが、先ほど作成したマスター ページは使用されません。 ページには、コンテンツが含まれていない場合、マスター ページを使用する web ページの宣言型マークアップを操作することはできますが、簡単ですだけのページを削除および再使用するマスター ページを指定する、プロジェクトに追加します。 したがって、Default.aspx をプロジェクトから削除することによって開始します。

次に、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、Default.aspx という名前の新しい Web フォームを追加することもできます。 今回は、チェック ボックスをマスター ページを選択し、一覧から Site.master マスター ページを選択します。


[![マスター ページの選択を選択する新しい Default.aspx ページを追加します。](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**図 05**: 追加、新しい Default.aspx ページを選択するマスター ページの選択 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image15.png))。


[![Site.master マスター ページを使用してください。](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**図 06**: Site.master マスター ページを使用して ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image18.png))。


> [!NOTE]
> Web アプリケーション プロジェクト モデルを使用している場合、新しい項目の追加 ダイアログ ボックスでは、このマスター ページの選択 チェック ボックスは含まれません。 代わりに、Web コンテンツ フォームの種類の項目を追加する必要があります。 Web コンテンツ フォームのオプションを選択し、追加をクリックすると、Visual Studio は、同じ Select マスターを表示 ダイアログ ボックスを図 6 に示すようにします。


だけ、新しい Default.aspx ページの宣言型マークアップが含まれています、@Pageマスターへのパスを指定するディレクティブ、マスター ページの MainContent ContentPlaceHolder のファイルとコンテンツ コントロールをページします。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

ここでは、Default.aspx を空のままにします。 コンテンツを追加するには、このチュートリアルの後半に返されます。

> [!NOTE]
> このマスター ページには、メニューまたはその他のいくつかのナビゲーション インターフェイスのセクションが含まれています。 今後のチュートリアルでこのようなインターフェイスを作成します。


## <a name="step-2-enabling-forms-authentication"></a>手順 2: は、フォーム認証を有効にします。

ASP.NET web サイトを作成して、フォーム認証を有効にするのには、次のタスクです。 使用して、アプリケーションの認証構成を指定、 [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)Web.config にします。&lt;認証&gt;要素には、アプリケーションによって使用される認証モデルを指定するモードをという名前の 1 つの属性が含まれています。 この属性は、次の 4 つの値の 1 つがあります。

- **Windows** - 訪問者を認証する web サーバーの役割は Windows 認証を使用するアプリケーションでは、前のチュートリアルで説明したように、基本、ダイジェスト、または統合 Windows を通じてこれは、通常認証します。
- **フォーム**-ユーザーが web ページ上のフォームを使用して認証されます。
- **Passport**-ユーザーは、Microsoft の Passport Network を使用して認証されます。
- **None**-認証モデルは使用されません。 すべての訪問者は匿名です。

既定では、ASP.NET アプリケーションは、Windows 認証を使用します。 フォーム認証を認証の種類を変更し、必要がありますを変更する、&lt;認証&gt;をフォームに要素の mode 属性。

プロジェクトで Web.config ファイルがまだ含まれない場合は、1 つ今すぐを右クリックして、ソリューション エクスプ ローラーでプロジェクト名を新しい項目の追加 を選択して Web 構成ファイルを追加しを追加します。


[![プロジェクトがまだ Web.config を含めない場合、今すぐ追加します。](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**図 07**: 場合、プロジェクトはいないまだ含める Web.config、今すぐ追加 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image21.png))。


次に、検索、&lt;認証&gt;要素とフォーム認証で使用するように更新します。 この変更を行う Web.config ファイルのマークアップは、次のようなになります。

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> XML ファイルを Web.config には、大文字小文字の区別が重要です。 大文字の F.、フォームにモード属性を設定することを確認します。フォームなどの異なる大文字小文字を使用する場合、ブラウザーを使用してサイトにアクセスしたときに、構成エラーを受け取ります。


&lt;認証&gt;要素には、必要に応じて、&lt;フォーム&gt;フォーム認証に固有の設定を含む子要素。 ここを使ってみましょう既定のフォーム認証設定。 見て、&lt;フォーム&gt;子要素で、次のチュートリアルで詳しく説明します。

## <a name="step-3-building-the-login-page"></a>手順 3: ログイン ページを作成します。

フォーム認証をサポートするためには、web サイトには、ログイン ページが必要があります。 フォーム認証ワークフローのセクション、FormsAuthenticationModule は自動的にリダイレクトについて説明したようにログイン ページにユーザーがページへのアクセスを試みる場合表示する権限。 匿名ユーザーにログイン ページへのリンクを表示する ASP.NET Web コントロールも。 ここで問題、ログイン ページの URL とは何ですか。

既定では、フォーム認証システムは Login.aspx という名前のログイン ページが必要ですし、web アプリケーションのルート ディレクトリに配置します。 を別のログイン ページの URL を使用する場合は、Web.config で指定することによって行うようにことができます。次のチュートリアルでこれを行う方法が表示されます。

ログイン ページには、3 つの役割があります。

1. 自分の資格情報を入力するビジターをできるようにするインターフェイスを提供します。
2. 送信された資格情報が有効なかどうかを決定します。
3. フォーム認証チケットを作成して、ユーザーにログインします。

### <a name="creating-the-login-pages-user-interface"></a>ログイン ページのユーザー インターフェイスを作成します。

最初のタスクを開始しましょう。 Login.aspx という名前のサイトのルート ディレクトリに新しい ASP.NET ページを追加し、Site.master マスター ページと関連付けます。


[![新しい ASP.NET ページの追加 Login.aspx という名前](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**図 08**: 追加、新しい ASP.NET ページという Login.aspx ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image24.png))。


2 つのテキスト ボックス、ユーザーの名前の 1 つは、自分のパスワードとフォームを送信するボタン用に 1 つの一般的なログイン ページのインターフェイスで構成されます。 Web サイトには、アカウントを記憶 チェック ボックス オンにした場合にブラウザーの再起動後、結果として得られる認証チケットが引き続き発生するにはこともよくありますが含まれます。

Login.aspx に 2 つの Textbox を追加し、ユーザー名とパスワードをそれぞれ、ID プロパティを設定します。 また、パスワードの TextMode プロパティをパスワードに設定します。 次に、RememberMe、Text プロパティに保存する ID プロパティを設定、チェック ボックス コントロールを追加します。 次に、ログイン ボタンのテキスト プロパティを持つがログインに設定をという名前のボタンを追加します。 最後に、Label Web コントロールを追加し、ID プロパティを設定する InvalidCredentialsMessage、その Text プロパティをユーザー名またはパスワードが無効です。 もう一度やり直してください。、赤、ForeColor プロパティおよびその Visible プロパティを False にします。

この時点で、画面を図 9 でスクリーン ショットのようになり、ページの宣言の構文は次のようにします。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![ログイン ページには、2 つのテキスト ボックス、チェック ボックス、ボタン、およびラベルが含まれています。](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**図 09**: [ログイン ページを含む 2 つのテキスト ボックス、チェック ボックス、ボタン、およびラベル ([フルサイズの画像を表示する] をクリックします](an-overview-of-forms-authentication-vb/_static/image27.png))。


最後に、ログイン ボタンの click イベント ハンドラーを作成イベントです。 デザイナーでは、単にこのイベント ハンドラーを作成するボタン コントロールをダブルクリックします。

### <a name="determining-if-the-supplied-credentials-are-valid"></a>かどうか、指定された資格情報は有効期間を決定します。

これで、ボタンのクリックで 2 のタスクを実装する必要がありますイベント ハンドラーが指定された資格情報が有効かどうかを決定します。 ためには指定された資格情報を一致させる任意の既知の資格情報を持つことを確認できるように、すべてのユーザーの資格情報を保持するユーザー ストアを使用する必要があります。

ASP.NET 2.0 では、前に開発者が独自のユーザー ストアの両方を実装して、ストアに対して指定された資格情報を検証するコードの記述を担当します。 ほとんどの開発者はストアを実装、ユーザー データベースでは、ユーザー名、パスワード、電子メール、lastlogindate というなどのように列を持つユーザーをという名前のテーブルを作成します。 次の表では、次に、ユーザー アカウントごとに 1 つのレコードになります。 ユーザーの指定された資格情報の確認は、一致するユーザー名、データベースのクエリを実行して、データベース内のパスワードが指定されたパスワードに前もっていることを確認します。

ASP.NET 2.0 で開発者する必要がありますを使用してメンバーシップ プロバイダーのいずれかのユーザー ストアを管理します。 このチュートリアル シリーズで使用するユーザー ストアの SQL Server データベースを使用すると、SqlMembershipProvider します。 使用する場合、テーブル、ビュー、およびプロバイダーで想定されているストアド プロシージャを含む特定のデータベース スキーマを実装する必要があります。 このスキーマを実装する方法について、  *[SQL Server でメンバーシップ スキーマを作成する](../membership/creating-the-membership-schema-in-sql-server-vb.md)* チュートリアル。 インプレース メンバーシップ プロバイダーは呼び出すだけです、ユーザーの資格情報の検証、[メンバーシップ クラス](https://msdn.microsoft.com/library/system.web.security.membership.aspx)の[ValidateUser (*username*、*パスワード*)メソッド](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)を示すブール値を返すかどうかの有効性、 *username*と*パスワード*の組み合わせ。 ように見えて SqlMembershipProvider のユーザー ストアをまだ実装していない、この時点でのメンバーシップ クラスは、ValidateUser メソッドを使用することはできません。

時間をかけて独自カスタム ユーザー データベースのテーブル (これは、SqlMembershipProvider を実装した後は、不使用になります) をビルドしましょう代わりにハード コードではなく、ログインの有効な資格情報はページ自体になります。 ログイン ボタンのクリック イベント ハンドラーを次のコードを追加します。

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

ご覧のように、Scott、Jisun、および Sam の 3 つの有効なユーザー アカウントがあるし、3 つすべてが同じパスワード (パスワード) にあります。 コードは、ユーザー名とパスワードの有効な一致を検索するユーザーとパスワードの配列をループ処理します。 ユーザー名とパスワードの両方が有効な場合は、ユーザーのログインし、それらを適切なページにリダイレクトする必要があります。 資格情報が有効でない場合 InvalidCredentialsMessage ラベルを表示します。

有効な資格情報を入力すると、ときに、適切なページがリダイレクトされますし説明しました。 適切なページで、ただしはでしょうか。 表示する権限がないページにアクセスするユーザー、FormsAuthenticationModule 自動的にページにリダイレクト、ログインを思い出してください。 そのため、ReturnUrl パラメーターを使用して、クエリ文字列で、要求された URL が含まれます。 つまり、ProtectedPage.aspx を参照してください。 しようとしているユーザーと、そのためには、権限のない、FormsAuthenticationModule がリダイレクトするとに。

Login.aspx?ReturnUrl=ProtectedPage.aspx

ログインすると、ユーザーは ProtectedPage.aspx にリダイレクトする必要があります。 または、ユーザーはログイン ページを参照して、独自 volition で可能性があります。 その場合は、ユーザーにログインした後、送信先となるルート フォルダーの Default.aspx ページ。

### <a name="logging-in-the-user"></a>ユーザーのログイン

指定された資格情報が有効であると仮定すると必要があります、フォーム認証チケットを作成するため、サイトにユーザーにログインします。 [FormsAuthentication クラス](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx)で、 [System.Web.Security 名前空間](https://msdn.microsoft.com/library/system.web.security.aspx)認証システム、フォームを使用してユーザーをログアウトし、ログ記録のためのさまざまなメソッドを提供します。 FormsAuthentication クラスでは、いくつかの方法がありますが、ここではこの時点で、3 つには。

- [GetAuthCookie(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) - creates a forms authentication ticket for the supplied name *username*. 次に、このメソッドは、作成し、認証チケットの内容を保持する HttpCookie オブジェクトを返します。 場合*persistCookie*が true の場合、永続的なクッキーが作成されます。
- [SetAuthCookie (*username*、 *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -、GetAuthCookie を呼び出します (*username*、 *persistCookie*)フォーム認証 cookie を生成します。 このメソッドは、(それ以外に使用される cookie ベースのフォーム認証されている、このメソッド呼び出しの cookie なしのチケットのロジックを処理する内部クラスを想定) クッキー コレクションに GetAuthCookie によって返されるクッキーを追加します。
- [RedirectFromLoginPage(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) - this method calls SetAuthCookie(*username*, *persistCookie*), and then redirects the user to the appropriate page.

GetAuthCookie はクッキーをクッキー コレクションに書き込む前に認証チケットを変更する必要がある場合に便利です。 SetAuthCookie は、フォーム認証チケットを作成し、Cookie のコレクションに追加するが、適切なページにユーザーをリダイレクトしたくない場合に便利です。 おそらく、ログイン ページのいくつか別のページに送信したりします。

ユーザーにログインし、適切なページにリダイレクトするので、RedirectFromLoginPage を使用してみましょう。 更新ログイン ボタンのクリック イベント ハンドラー、2 つのコメントの TODO 行を次のコード行に置き換えます。

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

フォーム認証チケットのユーザー名テキスト ボックスのテキストのプロパティを使用して、フォーム認証チケットを作成するときに*username*パラメーター、および RememberMe のチェック ボックスのチェック状態、 *persistCookie*パラメーター。

ログイン ページをテストするには、ブラウザーでアクセスします。 まず言ったりのユーザー名とパスワードの問題など、無効な資格情報を入力します。 [ログイン] ボタンをクリックすると、ポストバックが発生し、InvalidCredentialsMessage ラベルが表示されます。


[![InvalidCredentialsMessage ラベルが表示されるときに入力する資格情報が無効](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**図 10**: The InvalidCredentialsMessage ラベルが表示されるときに入力する資格情報が無効 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image30.png))。


次に、有効な資格情報を入力し、[ログイン] ボタンをクリックします。 ポストバックがフォーム認証チケットが発生したときにこの時間が作成され、自動的に Default.aspx にリダイレクトされます。 この時点でログイン web サイト ログインして現在を示す視覚的な手掛かりはありませんが。 プログラムで、ユーザーかどうかを確認する方法を説明します。 手順 4. では、またはページにアクセスするユーザーを識別する方法についてもいないで記録されます。

手順 5 では、ユーザーが、web サイトからのログを記録するための手法について説明します。

### <a name="securing-the-login-page"></a>ログイン ページのセキュリティ保護

(自分のパスワードを含む) の資格情報がで web サーバーにインターネット経由で転送されるときに、ユーザーは自分の資格情報を入力し、ログイン ページのフォームを送信する、*プレーン テキスト*します。 つまり、ネットワーク トラフィックをスニッフィング ハッカーは、ユーザー名とパスワードを確認できます。 これを防ぐにを使用して、ネットワーク トラフィックを暗号化するために不可欠[セキュリティで保護されたソケット レイヤー (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)します。 これにより、web サーバーによって受信されるまで、ブラウザーを離れる時点から、資格情報 (およびその全体のページの HTML マークアップ) を暗号化することが保証されます。

Web サイトには、機密情報が含まれています、しない限り、ユーザーのパスワード、プレーン テキストでネットワーク経由で送信されますそれ以外の場合のログイン ページと他のページで SSL を使用する必要はだけです。 既定では、これは両方暗号化およびデジタル署名 (改ざんを防ぐため) するためには、フォーム認証チケットのセキュリティ保護について心配する必要はありません。 フォーム認証チケットのセキュリティの詳細な説明については、次のチュートリアルでは表示されます。

> [!NOTE]
> 多くの財務、医療の web サイトで SSL を使用するように構成*すべて*ページにアクセスできるユーザーを認証します。 このような web サイトを構築している場合は、フォーム認証チケットがセキュリティで保護された接続経由でのみ送信されるように、フォーム認証システムを構成できます。 さまざまなフォーム認証の構成オプションを次のチュートリアルで紹介*[フォーム認証の構成と高度なトピック](../membership/creating-the-membership-schema-in-sql-server-vb.md)* します。


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>手順 4: 認証済みユーザーを検出して、自身の Id を決定します。

この時点でフォーム認証を有効にし、初歩的なログイン ページを作成しましたが、あるユーザーが認証済みまたは匿名であるかどうかを判断方法を確認します。 特定のシナリオで異なるデータや、認証済みまたは匿名ユーザーがページにアクセスしているかどうかに応じて、情報を表示することがあります。 認証されたユーザーの id を認識しておくことがよく必要さらに、します。

これらの手法を説明するために既存の Default.aspx ページを強化しましょう。 Default.aspx では、1 つの名前付き AuthenticatedMessagePanel と別の名前付き AnonymousMessagePanel、2 つのパネル コントロールを追加します。 最初のパネルで WelcomeBackMessage をという名前のラベル コントロールを追加します。 2 番目のパネルのハイパーリンク コントロールを追加、ログインし、NavigateUrl プロパティをその Text プロパティを設定 ~/login.aspx です。 この時点で Default.aspx の宣言型マークアップは、次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

ここまででおそらく想像のとおり、考え方は、認証された訪問者や匿名の訪問者に AnonymousMessagePanel だけに AuthenticatedMessagePanel だけを表示します。 これを実現するには、かどうかにでユーザーがログインしているかどうかに応じて、これらのパネルの表示プロパティを設定する必要があります。

[Request.IsAuthenticated プロパティ](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx)要求が認証されているかどうかを示すブール値を返します。 ページに次のコードを入力\_イベント ハンドラーのコードを読み込みます。

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

このコードでは、ブラウザーで Default.aspx を参照してください。 ログインにまだある場合は、ログイン ページへのリンクが表示されます (図 11 を参照してください)。 このリンクをクリックし、サイトにログインします。 手順 3. で説明したように資格情報を入力後する返されますが、Default.aspx に、この時間、ページが表示されます、ようこそ! メッセージ (図 12 を参照してください)。


[![アクセスして、匿名でログのリンクが表示される場合](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**図 11**: ときにアクセスして匿名で、ログのリンクが表示されます ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image33.png))。


[![認証されたユーザーは、ようこそで表示されます。メッセージ](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**図 12**: 認証されたユーザーは、ようこそで表示されます。 メッセージ ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image36.png))。


使用して、現在ログオンしているユーザーの id を決定できます、 [HttpContext オブジェクト](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)の[ユーザー プロパティ](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)します。 HttpContext オブジェクトは、現在の要求に関する情報を表し、他のユーザーの間での応答、要求、およびセッションでは、このような共通 ASP.NET オブジェクトのホームのです。 ユーザー プロパティは、現在の HTTP 要求と実装のセキュリティ コンテキストを表す、 [IPrincipal インターフェイス](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx)します。

ユーザー プロパティは、FormsAuthenticationModule で設定されます。 具体的には、FormsAuthenticationModule では、受信要求でフォーム認証チケットが検出されると、その新しい GenericPrincipal オブジェクトを作成し、ユーザーのプロパティに割り当てられます。

(GenericPrincipal) などのプリンシパル オブジェクトでは、ユーザーの id および所属するロールについて説明します。 IPrincipal インターフェイスには、2 つのメンバーを定義します。

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -プリンシパルが、指定されたロールに属しているかを示すブール値を返すメソッド。
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -を実装するオブジェクトを返すプロパティ、 [IIdentity インターフェイス](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)します。 IIdentity インターフェイスには、3 つのプロパティが定義されています: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx)、 [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)、および[名前](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx)します。

次のコードを使用して現在のユーザーの名前を判断できます。

Dim currentUsersName As String = User.Identity.Name

フォーム認証の使用時に、 [FormsIdentity オブジェクト](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx)GenericPrincipal の Identity プロパティが作成されます。 FormsIdentity クラスは、常にその IsAuthenticated プロパティの AuthenticationType プロパティに、True の文字列形式を返します。 Name プロパティは、フォーム認証チケットを作成するときに指定されたユーザー名を返します。 FormsIdentity にはこれら 3 つのプロパティに加えて、基になる認証チケットを使用してへのアクセスが含まれています、[プロパティのチケット](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)します。 Ticket プロパティは、型のオブジェクトを返します[FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)、有効期限、IsPersistent、IssueDate、名などのプロパティを持ちます。

重要な点を次に示しますを取り去る、 *username* 、FormsAuthentication.GetAuthCookie で指定されたパラメーター (*ユーザー名*、 *persistCookie*)、FormsAuthentication.SetAuthCookie (*username*、 *persistCookie*)、および FormsAuthentication.RedirectFromLoginPage (*username*、 *persistCookie*) メソッドは User.Identity.Name によって返されるのと同じ値です。 さらに、これらのメソッドによって作成された認証チケットは//新しい一般プリンシパルを FormsIdentity オブジェクトにキャストしてチケット プロパティにアクセスして利用できます。

Dim ident As FormsIdentity = CType(User.Identity, FormsIdentity)

認証チケットとして FormsAuthenticationTicket を dim ident を = です。チケット

Default.aspx でより個人的なメッセージを説明しましょう。 ページを更新する\_WelcomeBackMessage ラベルの Text プロパティには、文字列の開始が割り当てられているように、イベント ハンドラーを読み込む*username*!

WelcomeBackMessage.Text「帰りなさい、」= &amp; User.Identity.Name &amp; "!"

図 13 は、(Scott のユーザーとしてログイン) の場合、この変更の効果を示します。


[![ウェルカム メッセージを含むユーザーの名前で現在ログオンしています。](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**図 13**: ようこそメッセージには、現在ログインしてユーザーの名前が含まれています ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image39.png))。


### <a name="using-the-loginview-and-loginname-controls"></a>LoginView および LoginName コントロールの使用

共通の要件は、認証済みおよび匿名ユーザーに別のコンテンツを表示します。そのため、現在ログオンしているユーザーの名前を表示されています。 そのため、ASP.NET には、1 行のコードを記述する必要はありませんが、図 13 に示すように同じ機能を提供する 2 つの Web コントロールが含まれます。

[LoginView コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)テンプレートに基づく Web コントロールを簡単に認証と匿名ユーザーに別のデータを表示することができます。 LoginView には、2 つの定義済みテンプレートが含まれています。

- AnonymousTemplate - このテンプレートに追加されたすべてのマークアップは、匿名の訪問者にのみ表示されます。
- LoggedInTemplate - このテンプレートのマークアップは、認証されたユーザーにのみ表示されます。

Site.master、私たちのサイトのマスター ページを LoginView コントロールを追加してみましょう。 LoginView コントロールだけを追加するのではなく、両方を新しい ContentPlaceHolder コントロールを追加してみましょうとし、その新しい ContentPlaceHolder 内 LoginView コントロールを配置します。 この判断に"the rationale"は、間もなく明白になります。

> [!NOTE]
> LoginView コントロールは、AnonymousTemplate と LoggedInTemplate だけでなく、ロール固有のテンプレートを含めることができます。 ロール固有のテンプレートは、指定されたロールに属しているユーザーにのみ、マークアップを表示します。 今後のチュートリアルでは、LoginView コントロールのロール ベースの機能を見ていきます。


内のナビゲーションで、マスター ページに LoginContent を名前付きプレース ホルダーを追加することで開始&lt;div&gt;要素。 単に、TODO のすぐ上の結果として得られるマークアップを配置することに、ソース ビューのツールボックスからプレース ホルダー コントロールをドラッグできます。 メニューがここにテキスト。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

次に、LoginContent ContentPlaceHolder 内 LoginView コントロールを追加します。 マスター ページの ContentPlaceHolder のコントロールに配置するコンテンツと見なされます*既定のコンテンツ*プレース ホルダーの。 このマスター ページを使用する ASP.NET ページは、各プレース ホルダーを独自のコンテンツを指定またはマスター ページの既定のコンテンツを使用します。

ツールボックスの [ログイン] タブは、LoginView、およびその他のログインに関連するコントロールに配置されます。


[![ツールボックスで LoginView コントロール](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**図 14**: ツールボックスで、LoginView コントロール ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image42.png))。


次に、2 つ追加&lt;br/&gt; LoginView コントロールでの直後後がまだ、プレース ホルダー内にある要素。 この時点では、ナビゲーション&lt;div&gt;要素のマークアップは次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

デザイナーまたは宣言型マークアップから LoginView のテンプレートを定義できます。 Visual Studio のデザイナーでは、ドロップダウン リストで構成されているテンプレートを一覧表示、LoginView のスマート タグを展開します。 入力テキストこんにちは, AnonymousTemplate; に新戦力次に、ハイパーリンク コントロールを追加し、ログ内に、テキストと NavigateUrl プロパティを設定し、~/Login.aspx、それぞれします。

AnonymousTemplate を構成した後、LoggedInTemplate に切り替えるし、テキスト、「ようこそ」を入力します。 LoginName コントロールをツールボックスから、「ようこそ、」テキストの直後に配置すること、LoggedInTemplate にドラッグします。 [LoginName コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)、意味の名前として、現在ログインしているユーザーの名前を表示します。 内部的には、LoginName コントロールは、User.Identity.Name プロパティだけを出力します。

LoginView のテンプレートにこれらの追加を行った後マークアップは次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

これにより、Site.master マスター ページに、web サイト内の各ページに、ユーザーが認証されているかどうかに応じて、別のメッセージが表示されます。 図 15 は、ユーザー Jisun ブラウザーからアクセスすると、Default.aspx ページを示しています。 ようこそメッセージが 2 回繰り返されます Jisun: 左側 (LoginView コントロールを追加しました) を使用して、マスター ページのナビゲーション セクションでは 1 回と Default.aspx の中に 2 回コンテンツ (パネル コントロールとプログラム ロジック) 経由での領域。


[![LoginView コントロール表示の [ようこそ] のバックアップでは、Jisun します。](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**図 15**:、LoginView コントロールが表示されますようこそ、Jisun します。 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image45.png))。


マスター ページを追加、LoginView は、弊社サイト上のすべてのページに表示できます。 ただし、ある可能性があります web ページにこのメッセージを表示するはありません。 このような 1 つのページでは、ログイン ページへのリンクように見える場所にあるため、ログイン ページです。 マスター ページで ContentPlaceHolder の LoginView コントロールに配置しましたので、コンテンツ ページで、この既定のマークアップを無効にできます。 Login.aspx を開き、デザイナーに移動します。 コンテンツ コントロールを明示的に定義されていますされませんのででマスター ページで LoginContent ContentPlaceHolder の Login.aspx、ログイン ページは、このプレース ホルダーのマスター ページの既定のマークアップが表示されます。 透けてこのデザイナー - LoginContent ContentPlaceHolder (LoginView コントロール) の既定のマークアップを示しています。


[![マスター ページの LoginContent ContentPlaceHolder のログイン ページがコンテンツにある既定値を示します](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**図 16**: ログイン ページは、マスター ページの LoginContent ContentPlaceHolder のコンテンツの既定の表示 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image48.png))。


LoginContent ContentPlaceHolder の既定のマークアップをオーバーライドするには、デザイナー内の領域を右クリックし、コンテキスト メニューからカスタム コンテンツの作成のオプションを選択します。 (Visual Studio 2008、プレース ホルダーを使用してが含まれる場合、スマート タグを選択すると、同じオプションを提供します)。ページのマークアップにされ、新しいコンテンツ コントロールが追加されますをこのページのカスタム コンテンツを定義できます。 など、ログインしてくださいここでは、カスタム メッセージを追加することも、みましょうだけ空白のままにすることができます。

> [!NOTE]
> Visual Studio 2005 では、空を作成カスタム コンテンツを作成する ASP.NET ページにコントロールのコンテンツします。 Visual Studio 2008 では、ただし、カスタム コンテンツを作成するコピー、マスター ページの既定のコンテンツを新しく作成されたコンテンツ コントロールにします。 Visual Studio 2008 を使用している場合、新しいコンテンツ コントロールを作成した後を確認してくださいマスター ページからコピーされたコンテンツをクリアするには


図 17 では、この変更を行った後、ブラウザーからアクセスしたときの Login.aspx ページを示します。 こんにちは, 新戦力またはへようこそ のバックアップがないことに注意してください。*ユーザー名*左側のナビゲーションでメッセージ&lt;div&gt;は Default.aspx にアクセスするとします。


[![ログイン ページには、既定の LoginContent プレース ホルダーのマークアップが非表示になります](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**図 17**: ログイン ページには、既定の LoginContent ContentPlaceHolder のマークアップが非表示になります ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image51.png))。


## <a name="step-5-logging-out"></a>手順 5: ログアウト

手順 3 では、サイトへのユーザーのログインのログイン ページを構築しましたが、あるユーザーがログアウトする方法について。内のユーザーをログインするためのメソッドに加え、FormsAuthentication クラスも用意されています、 [SignOut メソッド](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)します。 SignOut メソッドでは、それによってサイトからユーザーをログイン フォーム認証チケットは、単に破棄します。

ログアウト リンクは、このような一般的な機能を提供する ASP.NET には、ユーザーがログアウトする設計にコントロールが含まれています。[LoginStatus コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx)ログイン linkbutton コントロールまたはユーザーの認証の状態に応じて、ログアウト LinkButton が表示されます。 ログアウト LinkButton が認証されたユーザーに表示されますが、匿名ユーザーのログイン LinkButton がレンダリングされます。 LoginStatus の経由でログインおよびログアウト Linkbutton のテキストを構成することができます LoginText および LogoutText プロパティ。

ログイン ページにリダイレクトを発行元となる、ポストバックを発生させるログイン LinkButton をクリックします。 ログアウト LinkButton をクリックし、LoginStatus コントロール FormsAuthentication.SignOff メソッドの呼び出しをページにユーザーをリダイレクトします。 ログオンしているページは LogoutAction プロパティは、次の 3 つの値のいずれかに割り当てることができますに依存する無効なユーザーがリダイレクトされます。

- 更新 - 既定では、だけ訪問したページにユーザーをリダイレクトします。 だけ訪問したページが、FormsAuthenticationModule は自動的に匿名ユーザーは、許可しない場合は、ログイン ページに、ユーザーをリダイレクトします。

ここで、リダイレクトが実行される理由が気ができます。 場合は、ユーザーが、同じページに保持する理由の明示的なリダイレクトが必要でしょうか。 理由は、ログオフ LinkButton がクリックされたときに、ユーザーがあるフォーム認証チケットのクッキーのコレクションであるためにです。 そのため、認証された要求のポストバック要求です。 LoginStatus コントロールが、サインアウト メソッドを呼び出しますが、FormsAuthenticationModule がユーザーを認証した後に発生します。 そのため、明示的なリダイレクトは、ページを再要求するブラウザーをによりします。 ブラウザー ページを再要求するまでは、フォーム認証チケットが削除され、そのため、受信要求は匿名です。

- リダイレクト - ユーザーは、LoginStatus の LogoutPageUrl プロパティで指定された URL にリダイレクトされます。
- RedirectToLoginPage - ユーザーは、ログイン ページにリダイレクトされます。

みましょうマスター ページに、LoginStatus コントロールを追加し署名したことを確認するメッセージを表示するページにユーザーに送信するリダイレクト オプションを使用するように構成します。まず Logout.aspx をという名前のルート ディレクトリで、ページを作成します。 このページを関連付ける Site.master マスター ページを忘れないでください。 次に、出力が記録されたことをユーザーに説明するページのマークアップにメッセージを入力します。

次に、Site.master マスター ページに戻るし、LoginContent プレース ホルダーで、LoginView の下に、LoginStatus コントロールを追加します。 ~/Logout.aspx にリダイレクトする、LoginStatus コントロールの LogoutAction プロパティとその LogoutPageUrl プロパティを設定します。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

LoginStatus が LoginView コントロールの外側にあるため、匿名、認証済みの両方のユーザーに表示されますが、[ok] ため、ログインまたはログアウト LinkButton、LoginStatus が正しく表示します。 LoginStatus コントロールの追加により、AnonymousTemplate 内のハイパーリンク ログでは不要、ので削除します。

図 18 は、Jisun にアクセスしたときに、Default.aspx を示します。 左側の列がメッセージを表示することに注意してください、こそ、ログアウトのリンクと共に Jisun します。ログアウト LinkButton をクリックするとポストバックが発生する、Jisun が、システムでは、サインアウトおよび Logout.aspx にリダイレクトさせます。 図 19 に示すように達した Logout.aspx の Jisun 時間で彼女署名済みであるため匿名。 その結果、左側の列には、ストレンジャー、ログイン ページへのリンクの開始、テキストが表示されます。


[![Default.aspx ようこそ、ログアウト LinkButton と共に Jisun を示しています。](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**図 18**: Default.aspx 示しますようこそ戻る]、[Jisun に沿ってログアウト LinkButton で ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image54.png))。


[![Logout.aspx 示しますへようこそ、ログイン LinkButton と共に新戦力](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**図 19**: Logout.aspx 示しますへようこそ、ログイン LinkButton と共に新戦力 ([フルサイズの画像を表示する をクリックします](an-overview-of-forms-authentication-vb/_static/image57.png))。


> [!NOTE]
> (手順 4. で Login.aspx に対して実行した) など、マスター ページの LoginContent ContentPlaceHolder を非表示にする Logout.aspx ページをカスタマイズすることをお勧めします。 LoginStatus コントロールによってログイン LinkButton が表示されるためです (こんにちは, の下に 1 つ新戦力) ReturnUrl クエリ文字列パラメーターで、現在の URL を渡して、ログイン ページにユーザーを送信します。 つまりがログアウトしたユーザーは、この LoginStatus のログインの LinkButton をクリックしてログオンしの場合は、Logout.aspx で、ユーザーが混乱することが簡単に戻るがリダイレクトされます。


## <a name="summary"></a>まとめ

このチュートリアルではを調べる場合は、フォーム認証ワークフローの使用を開始し、ASP.NET アプリケーションでフォーム認証の実装にします。 フォーム認証の 2 つの役割を持つ、FormsAuthenticationModule で電源が: フォーム認証チケットに基づくユーザーを識別して、承認されていないユーザーをログイン ページにリダイレクトします。

.NET Framework の FormsAuthentication クラスには、作成、検査、およびフォーム認証チケットを削除するためのメソッドが含まれています。 Request.IsAuthenticated プロパティとユーザー オブジェクト、要求が認証されたかどうかと、ユーザーの id に関する情報を決定するため追加のプログラムによるサポートを提供します。 ログインに関連する多くの一般的なタスクを実行するため、コーディングは不要のクイック方法を開発者に提供し、LoginView、LoginStatus、LoginName Web コントロールもあります。 今後のチュートリアルでは、さらに詳しく説明内のログインに関連する Web コントロールやこれらを究明します。

このチュートリアルでは、フォーム認証の大まかな概要を提供します。 さまざまな構成オプションを確認をどの cookieless フォーム認証チケットの動作を見る、か ASP.NET でフォーム認証チケットの内容を保護する方法の詳細されませんでした。 これらのトピックと詳細を説明します、[次のチュートリアル](forms-authentication-configuration-and-advanced-topics-vb.md)します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [IIS6 と iis 7 のセキュリティの変更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [ログインの ASP.NET コントロール](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 のセキュリティ、メンバーシップ、およびロール管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;フォーム&gt;要素&lt;認証&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックのビデオ トレーニング

- [ASP.NET で基本フォーム認証を使用する](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者には、Alicja Maziarz、John の Suru Teresa Murphy などがあります。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com)します。

> [!div class="step-by-step"]
> [前へ](security-basics-and-asp-net-support-vb.md)
> [次へ](forms-authentication-configuration-and-advanced-topics-vb.md)
