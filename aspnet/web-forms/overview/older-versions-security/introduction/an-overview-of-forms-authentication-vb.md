---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "フォーム認証 (VB) の概要 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは単なるディスカッションからに移る実装です。具体的には、フォーム認証の実装で見ていきます。 Web アプリケーション w."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 90bcff91d0642e6af66f43fd807b253cc516d277
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-vb"></a>フォーム認証 (VB) の概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> このチュートリアルでは単なるディスカッションからに移る実装です。具体的には、フォーム認証の実装で見ていきます。 まず、このチュートリアルで構築する web アプリケーションは引き続き、以降のチュートリアルでは、構築するのには単純なフォーム認証からのメンバーシップおよびロールに移動します。
> 
> このトピックの詳細については、このビデオを参照してください: [ASP.NET での基本的なフォーム認証の使用](# "using-basic-forms-authentication-in-aspnet")です。


## <a name="introduction"></a>はじめに

[前のチュートリアル](security-basics-and-asp-net-support-vb.md)ASP.NET によって提供される各種の認証、承認、およびユーザー アカウントのオプションについて説明しました。 このチュートリアルでは単なるディスカッションからに移る実装です。具体的には、フォーム認証の実装で見ていきます。 まず、このチュートリアルで構築する web アプリケーションは引き続き、以降のチュートリアルでは、構築するのには単純なフォーム認証からのメンバーシップおよびロールに移動します。

このチュートリアルは、フォーム認証のワークフロー、前のチュートリアルで時に説明したトピックを掘り下げて見てから始まります。 次に、フォーム認証の概念をデモするための ASP.NET web サイトを作成します。 次に、フォーム認証を使用して、簡単なログイン ページを作成およびを決定する方法、コードでは、ユーザーが認証され場合は、ユーザー名、ログオンに使用するかどうかを参照してください。 サイトは構成します。

認証ワークフロー、web アプリケーションで有効にして、ログイン時とログオフ時のページを作成するユーザー アカウントをサポートし、web ページを使用してユーザーを認証する ASP.NET アプリケーションのビルドのすべての重要な手順は、フォームを理解できます。 -これによりこれらのチュートリアル - 別の時に作成されているためお勧めの過去のプロジェクトのフォーム認証を既にお持ちのエクスペリエンスを構成する場合でも、次のいずれかに進む前に、完全には、このチュートリアルを実行します。

## <a name="understanding-the-forms-authentication-workflow"></a>フォーム認証のワークフローを理解します。

ASP.NET ランタイムが ASP.NET ページや ASP.NET Web サービスなど、ASP.NET リソースの要求を処理するときに、要求のライフ サイクル中に多数のイベントを発生させます。 要求が認証されると、承認されると、未処理の例外となどの場合に発生するイベントのときに発生するものと、要求の非常に開始し、非常に最後に発生するイベントがあります。 イベントの完全な一覧を表示するを参照してください、 [HttpApplication オブジェクトのイベント](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx)です。

*HTTP モジュール*マネージ クラスのコードを持つは要求のライフ サイクルの特定のイベントに応答して実行します。 ASP.NET は、バック グラウンドでの必須のタスクを実行する HTTP モジュールの数が付属しています。 ここに特に関連する 2 つの組み込み HTTP モジュールは次のとおりです。

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -は、通常、ユーザーのクッキー コレクションに含まれる、フォーム認証チケットを調べることによって、ユーザーを認証します。 フォーム認証チケットが存在しない場合、ユーザーは匿名です。
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -現在のユーザーが要求された URL にアクセスする承認されたかどうかを決定します。 このモジュールは、アプリケーションの構成ファイルで指定された承認規則を参照して、権限を決定します。 ASP.NET も含まれています、[持ちます](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)要求されたファイルの Acl を参照して機関を指定します。

FormsAuthenticationModule が、UrlAuthorizationModule (および持ちます) を実行する前にユーザーを認証しようとするとします。 承認モジュールが、要求を終了しを返します、要求を行うユーザーが要求されたリソースにアクセスする権限がない場合、 [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html)状態です。 Windows 認証のシナリオでは、ブラウザーに HTTP 401 ステータスが返されます。 この状態コードは、モーダル ダイアログ ボックスを使用して資格情報をユーザー入力を求めるブラウザーをによりします。 フォーム認証では、ただし、HTTP 401 Unauthorized ステータスは送信されません、ブラウザーに、FormsAuthenticationModule がこの状態が検出され、変更する代わりに、ユーザーをログイン ページにリダイレクトするため (を使用して、 [HTTP 302 リダイレクト](http://www.checkupdown.com/status/E302.html)状態)。

ログイン ページの責任では、ユーザーの資格情報が有効と場合は、フォーム認証チケットを作成し、ユーザーをページにリダイレクトされたしようとしているビジットするかどうかを決定します。 認証チケットは、ユーザーを識別する、FormsAuthenticationModule を使用して web サイト上のページへの後続の要求に含まれます。


[![フォーム認証のワークフロー](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**図 01**: フォーム認証のワークフロー ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>ページのビジットの間で認証チケットを記憶

ログに記録して、フォーム認証チケット必要がありますに返される要求のたびに、web サーバーが、サイトを閲覧する際、ユーザーがログインしている残るようです。 これは通常、ユーザーのクッキー コレクションに認証チケットを配置することによって行います。 [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie)されるユーザーのコンピューター上に存在し、cookie を作成した web サイトへの各要求で HTTP ヘッダーで送信される小さなテキスト ファイルです。 そのため、フォーム認証チケットが作成され、ブラウザーの cookie に格納されて、そのサイトへの後アクセスは、ユーザーを識別するため、要求と共に認証チケットを送信します。

> [!NOTE]
> それぞれのチュートリアルで使用するデモ web アプリケーションはダウンロードです。 このダウンロード可能なアプリケーションは、.NET Framework version 3.5 を対象と Visual Web Developer 2008 で作成されました。 アプリケーションが .NET 3.5 を対象となるので、Web.config ファイルには、追加の 3.5 に固有の構成要素が含まれます。 手短、.NET 3.5 をインストール、ダウンロード可能な web アプリケーションでは、コンピューターにまだがある場合は、web.config ファイルから 3.5 固有のマークアップを削除せずは機能しません。


Cookie の 1 つは、有効期限が切れる日付と時刻のブラウザーが cookie を破棄するには、します。 フォーム認証 cookie の期限が切れると、ユーザー不要になった認証およびため匿名のようになります。 公共の端末からユーザーを訪問しているときに、認証チケットのブラウザーを閉じるときに期限切れにする可能性があります。 自宅からオフィスを訪れた、ただし、その同じユーザー場合、認証チケットがあるないように、ブラウザーの再起動の間で記憶される、サイトにアクセスするたびに再ログインします。 この決定が多くの場合、次回のチェック ボックスのログイン ページで、フォーム内のユーザーによって行われます。 手順 3 では、ログイン ページで、アカウントを記憶 チェック ボックスを実装する方法を検討します。 次のチュートリアルでは、認証チケットのタイムアウト設定の詳細を説明します。

> [!NOTE]
> Web サイトにログオンするために使用するユーザー エージェントが cookie をサポートしていないことができます。 このような場合は、ASP.NET が cookie なしのフォーム認証チケットを使用できます。 このモードでは、認証チケットを URL にエンコードされます。 Cookie なしの認証チケットを使用する場合と、作成され、次のチュートリアルでは管理の方法を紹介します。


### <a name="the-scope-of-forms-authentication"></a>フォーム認証のスコープ

FormsAuthenticationModule がマネージ コードは、ASP.NET ランタイムの一部です。 前の Microsoft のバージョン 7[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) web サーバー IIS の HTTP パイプラインと、ASP.NET ランタイムのパイプラインの間で異なるバリアが発生しました。 要するに、IIS 6 以前のバージョンで、FormsAuthenticationModule は、ASP.NET ランタイムを IIS からの要求が委任されている場合にのみ実行します。 既定では、拡張子が .aspx、.asmx、または .ashx のページが要求されたときに、IIS は静的なコンテンツ自体の HTML ページと CSS およびイメージ ファイルの要求を渡すだけのように、ASP.NET ランタイムを処理します。

IIS 7 では、ただしでは、統合された IIS と ASP.NET パイプライン。 IIS 7 の FormsAuthenticationModule を呼び出すをセットアップするいくつかの構成設定で*すべて*要求します。 さらに、IIS 7 では、任意の種類のファイルに対する URL 承認規則を定義できます。 詳細については、次を参照してください。 [IIS6 間での変更と IIS7 セキュリティ](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above)、 [Web プラットフォーム セキュリティ](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)、および[Understanding IIS7 URL 承認](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)です。

手短、IIS 7 以前のバージョンで、ASP.NET ランタイムによって処理されるリソースを保護するのにフォーム認証を使用することができますのみです。 同様に、URL 承認規則は、ASP.NET ランタイムによって処理されるリソースにのみ適用されます。 しかし、IIS 7 FormsAuthenticationModule と UrlAuthorizationModule をすべての要求にこの機能を拡張するため、IIS の HTTP パイプラインに統合することがします。

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>手順 1: このチュートリアルの一連の ASP.NET web サイトを作成します。

Visual Studio 2008 の Microsoft の無料版の最も長い可能な対象ユーザーを達成するためにこの系列全体で構築することに ASP.NET web サイトが作成されます[Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)です。 SqlMembershipProvider ユーザー ストアを実装します。、 [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)データベース。 Visual Studio 2005 または Visual Studio 2008 または SQL Server の別のエディションを使用している場合心配 - 手順はほぼ同一であると、重要な相違点で指摘されます。

フォーム認証を構成して、前に、ASP.NET web サイトが最初に必要です。 まず、新しいファイル システムに基づく ASP.NET web サイトを作成します。 これを実現するには、Visual Web Developer を起動し、ファイル メニューに移動し、選択新しい Web サイトを新しい Web サイト ダイアログ ボックスを表示します。 ASP.NET Web サイト テンプレートの選択、ファイル システムに場所ドロップダウン リストを設定、web サイトを配置するフォルダーを選択および vb です。 言語に設定 これには、Default.aspx ASP.NET ページで、アプリで新しい web サイトを作成は\_データ フォルダーと Web.config ファイルです。

> [!NOTE]
> Visual Studio には、プロジェクト管理の 2 つのモードがサポートされています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 Web サイト プロジェクトでは、Web アプリケーション プロジェクトを模倣プロジェクト アーキテクチャ Visual Studio .NET 2002年/2003 - プロジェクト ファイルをインクルードし、プロジェクトのソース コードを/bin フォルダーに配置されている単一のアセンブリにコンパイルされ、プロジェクト ファイルが不足しています。 Visual Studio 2005 最初に唯一のサポートされている Web サイト プロジェクト、Web アプリケーション プロジェクト モデルは、Service Pack 1; 再導入されましたが、Visual Studio 2008 には、両方のプロジェクト モデルが用意されています。 Visual Web Developer 2005 および 2008 のエディションはサポート Web サイト プロジェクト。 Web サイト プロジェクトのモデルを使用するされます。 Express 以外のエディションを使用しているし、使用するかどうか、 [Web アプリケーション プロジェクト モデル](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)代わりに、自由にこれがあることをいくつかの相違点、画面とではなく実行する必要があります手順で表示されるものの間に注意してください、スクリーン ショットに表示し、これらのチュートリアルで説明する手順。


[![新しいファイル システムに基づいた Web サイトを作成します。](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**図 02**: New File System-Based Web サイトの作成 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>マスター ページを追加します。

次に、Site.master をという名前のルート ディレクトリ内のサイトに新しいマスター ページを追加します。 [マスター ページ](https://msdn.microsoft.com/library/wtxbf3hh.aspx)ASP.NET ページに適用できるサイトのテンプレートを定義するページの開発者を有効にします。 マスター ページの主な利点、サイトの全体的な外観できますは定義されている 1 つの場所でしやすくための更新や、サイトのレイアウトを調整します。


[![マスター ページを追加するという名前の web サイトに Site.master](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**図 03**: マスター ページという Site.master、web サイトに追加 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image9.png))


マスター ページで、ここで、サイト全体のページ レイアウトを定義します。 デザイン ビューを使用して、必要なレイアウトまたは Web コントロールを追加または、ソース ビューで手動でマークアップを手動で追加することができます。 使用されるレイアウトを模倣するために、マスター ページのレイアウトを構造化した my  *[ASP.NET 2.0 のデータを扱う](../../data-access/index.md)*チュートリアル シリーズの (図 4 を参照してください)。 マスター ページを使用して[カスケード スタイル シート](http://www.w3schools.com/css/default.asp)の配置とスタイル (このチュートリアルの関連するダウンロードに含まれています) を Style.css ファイルで定義された CSS 設定を使用します。 CSS 規則が定義されているマークアップを次に示すのかを判断できないときにするよう、ナビゲーション&lt;div&gt;の左側に表示し、200 ピクセルの固定幅ができるように、コンテンツが絶対位置に配置します。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

マスター ページは、静的ページ レイアウトやマスター ページを使用する ASP.NET ページが編集可能な領域の両方を定義します。 これらのコンテンツの編集可能な領域は、プレース ホルダー コントロール、コンテンツを確認することができますで示されます&lt;div&gt;です。 マスター ページが 1 つ ContentPlaceHolder (メイン) がマスター ページの可能性があります複数 contentplaceholders にします。

上記で入力したマークアップを含む、デザイン ビューに切り替えると、マスター ページのレイアウトを示します。 このマスター ページを使用するすべての ASP.NET ページには、メイン地域のマークアップを指定することでこの統一されたレイアウトがあります。


[![デザイン ビューで表示したときに、マスター ページ](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**図 04**:、マスター ページ、ときに表示をデザイン ビュー ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>コンテンツ ページを作成します。

この時点で Default.aspx ページでマイクロソフト web サイトが作成したばかりのマスター ページを使用しません。 ページには、コンテンツが含まれていない場合は、マスター ページを使用する web ページの宣言型マークアップを操作することができますがまだは簡単にだけ、ページを削除して再使用するマスター ページを指定する、プロジェクトに追加します。 そのため、Default.aspx をプロジェクトから削除することによって開始します。

次に、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、Default.aspx という名前の新しい Web フォームの追加を選択します。 現時点は、チェック ボックスをマスター ページを選択し、一覧から Site.master マスター ページを選択します。


[![マスター ページの選択 を選択する新しい Default.aspx ページを追加します。](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**図 05**: 追加、新しい Default.aspx ページを選択するマスター ページの選択 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Site.master マスター ページを使用してください。](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**図 06**: Site.master マスター ページを使用して ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Web アプリケーション プロジェクトのモデルを使用している場合、新しい項目の追加 ダイアログ ボックスではマスター ページを選択 チェック ボックスが含まれません。 代わりに、Web コンテンツ形式の種類のアイテムを追加する必要があります。 Web コンテンツ形式オプションを選択すると、追加 をクリックして、Visual Studio がマスターの同じ選択を表示 ダイアログ ボックスの図 6 に示すようにします。


新しい Default.aspx ページの宣言型マークアップにだけが含まれています、@Pageマスターへのパスを指定するディレクティブ、マスター ページのメイン ContentPlaceHolder のファイルとコンテンツ コントロールをページします。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

ここでは、Default.aspx を空のままにします。 おについては、コンテンツを追加するには、このチュートリアルの後半でそれに戻ります。

> [!NOTE]
> このマスター ページには、メニューやその他のナビゲーション インターフェイスのセクションが含まれています。 今後のチュートリアルでこのようなインターフェイスを作成します。


## <a name="step-2-enabling-forms-authentication"></a>手順 2:、フォーム認証が有効にします。

ASP.NET web サイトを作成して、[次へ] タスクは、フォーム認証を有効にするのには。 アプリケーションの認証構成を指定、 [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)Web.config でします。&lt;認証&gt;要素には、アプリケーションによって使用される認証モデルを指定する mode という名前の 1 つの属性が含まれています。 この属性は、次の 4 つの値のいずれかを持つことができます。

- **Windows** - 訪問者の認証に、web サーバーの役割は Windows 認証を使用するアプリケーションで、前のチュートリアルで説明したように、通常これには Basic、Digest または統合 Windows認証します。
- **フォーム**-ユーザーが web ページ上のフォームを使用して認証されます。
- **Passport**-ユーザーは、Microsoft の Passport Network を使用して認証されます。
- **None**-認証モデルが使用されていない、すべての訪問者は匿名です。

既定では、ASP.NET アプリケーションは、Windows 認証を使用します。 フォーム認証を認証の種類を変更し、必要がありますを変更する、&lt;認証&gt;フォームに要素の mode 属性。

プロジェクトに Web.config ファイルがまだ含まれない場合は、1 つ今すぐを右クリックして、ソリューション エクスプ ローラーでプロジェクト名を新しい項目の追加 を選択して Web 構成ファイルを追加しを追加します。


[![プロジェクトがまだ Web.config を行わない場合は、今すぐ追加します。](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**図 07**: 場合、プロジェクトはありませんまだ含める Web.config、今すぐ追加 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image21.png))


次に、検索、&lt;認証&gt;要素およびフォーム認証で使用するように更新します。 この変更後に、Web.config ファイルのマークアップを次のようになります。

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Web.config は、XML ファイルには、大文字小文字の区別が重要です。 大文字の F.、フォームにモード属性を設定することを確認してください。大文字小文字が異なる、フォームなどを使用する場合、ブラウザーを使用してサイトにアクセスしたときに構成エラーを受け取ります。


&lt;認証&gt;要素には、必要に応じて、&lt;フォーム&gt;フォーム認証に固有の設定が含まれる子要素です。 ここでは、みましょうだけを使用して既定のフォーム認証設定。 について学びます、&lt;フォーム&gt;子要素を次のチュートリアルで詳しく説明します。

## <a name="step-3-building-the-login-page"></a>手順 3: ログイン ページの作成

フォーム認証をサポートするためには、当社の web サイトには、ログイン ページが必要があります。 フォーム認証ワークフローのセクションを FormsAuthenticationModule は自動的にリダイレクトについて説明したようにユーザーをログイン ページされていないページへのアクセスを試みた場合表示する権限。 匿名ユーザーにログイン ページへのリンクを表示する ASP.NET Web コントロールがあります。 もっとも、ログイン ページの URL は何ですか。

既定では、フォーム認証システムは Login.aspx 名前を指定する、ログイン ページが必要ですし、web アプリケーションのルート ディレクトリに配置されます。 別のログイン ページの URL を使用する場合は、これを行う Web.config で指定しています。これ以降のチュートリアルを実行する方法が表示されます。

ログイン ページには、3 つの役割があります。

1. 自分の資格情報を入力するビジターをできるようにするインターフェイスを提供します。
2. 送信された資格情報が有効なかどうかを判断します。
3. フォーム認証チケットを作成することで、ユーザーにログインします。

### <a name="creating-the-login-pages-user-interface"></a>ログイン ページのユーザー インターフェイスを作成します。

最初のタスクを開始しましょう。 Login.aspx をという名前のサイトのルート ディレクトリに新しい ASP.NET ページを追加し、Site.master マスター ページと関連付けます。


[![新しい ASP.NET ページの追加 Login.aspx という名前](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**図 08**: 新しい ASP.NET ページという Login.aspx に追加 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image24.png))


典型的なログイン ページのインターフェイスは、次の 2 つのテキスト ボックス、ユーザーの名前のいずれか、そのパスワードとフォームを送信するためのボタンに 1 つで構成されます。 Web サイトには、アカウントを記憶するチェック ボックス オンの場合、ブラウザーの再起動前後で、結果として得られる認証チケットが引き続き発生するにはこともよくありますが含まれます。

Login.aspx に 2 つのテキスト ボックスを追加し、ユーザー名とパスワードをそれぞれの ID プロパティを設定します。 また、パスワードの TextMode プロパティをパスワードに設定します。 次に、RememberMe し記憶 me. するには、そのテキスト プロパティに、ID プロパティを設定、CheckBox コントロールを追加します。 次に、ログイン ボタンのテキスト プロパティが設定されるログインをという名前のボタンを追加します。 最後に、ラベルの Web コントロールを追加し、ID プロパティを設定 InvalidCredentialsMessage、そのテキスト プロパティにユーザー名またはパスワードが無効です。 もう一度やり直してください。、赤、前景色プロパティおよびその表示プロパティを False にします。

この時点で、画面を図 9 でスクリーン ショットのようになり、ページの宣言の構文は、次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![ログイン ページには、次の 2 つのテキスト ボックス、チェック ボックス、ボタン、およびラベルが含まれています。](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**図 09**:、ログイン ページを含む 2 つのテキスト ボックス、チェック ボックス、ボタン、およびラベル ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image27.png))


最後に、ログイン ボタンのクリック イベント ハンドラーを作成イベントです。 デザイナーで、このイベント ハンドラーを作成するボタン コントロール をダブルクリックします。

### <a name="determining-if-the-supplied-credentials-are-valid"></a>かどうか、指定された資格情報が有効期間を決定します。

ボタンのクリックで 2 のタスクを実装する必要がありますイベント ハンドラーが指定された資格情報が有効かどうかを決定します。 このために指定された資格情報を一致させる既知の資格情報を持つことを確認できるように、すべてのユーザーの資格情報を保持するユーザー ストアを指定する必要があります。

ASP.NET 2.0 では、前に開発者が独自のユーザー ストアの両方を実装して、ストアに対して指定された資格情報を検証するコードの記述を担当します。 ほとんどの開発者ストアを実装ユーザー データベースでは、ユーザー名、パスワード、電子メール、LastLoginDate などのように列を持つユーザーをという名前のテーブルを作成します。 次に、次の表は、ユーザー アカウントごとに 1 つのレコードがあります。 ユーザーの指定された資格情報の検証が行われます。 一致するユーザー名のデータベースのクエリを実行して、データベース内のパスワードが指定されたパスワードに当てはめて考えることを確認します。

ASP.NET 2.0 では、開発者必要がありますを使用してメンバーシップ プロバイダーのいずれかのユーザー ストアを管理します。 このチュートリアルの系列に使用するユーザー ストアの SQL Server データベースを使用すると、SqlMembershipProvider です。 SqlMembershipProvider を使用する場合、テーブル、ビュー、およびプロバイダーで想定されているストアド プロシージャを含む特定のデータベース スキーマを実装する必要があります。 このスキーマを実装する方法については、 *[メンバーシップ スキーマを作成する SQL Server で](../membership/creating-the-membership-schema-in-sql-server-vb.md)*チュートリアルです。 インプレース メンバーシップ プロバイダーは、ユーザーの資格情報を検証するほど単純では呼び出すことと、[メンバーシップ クラス](https://msdn.microsoft.com/library/system.web.security.membership.aspx)の[ValidateUser (*username*、*パスワード*)メソッド](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)を示すブール値を返すことをするかどうかの有効性、 *username*と*パスワード*の組み合わせ。 おを実装していない SqlMembershipProvider のユーザーのストア、表示されるようにこの時点でのメンバーシップ クラスは、ValidateUser メソッドを使用することはできません。

時間がかかる場合、テーブルの構築に自社カスタム ユーザー データベース (お、SqlMembershipProvider が実装された後に古いなります) をみましょう代わりにハード コードではなくログイン内で有効な資格情報ページします。 ログイン ボタンのでは、イベント ハンドラーをクリックして、次のコードを追加します。

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

わかります - Scott、Jisun、および Sam - 次の 3 つの有効なユーザー アカウントがあるし、3 つすべてが同じパスワード (パスワード) にあります。 コードは、有効なユーザー名とパスワードと一致するユーザー名およびパスワードの配列をループします。 ユーザー名とパスワードの両方が有効な場合は、ユーザーをログインし、それらを適切なページにリダイレクトする必要があります。 資格情報が有効でない場合 InvalidCredentialsMessage ラベルを表示します。

有効な資格情報を入力すると、ときに説明したように、適切なページにはリダイレクトされます。 ただし、適切なページとは? すると、ユーザーは、表示、承認されていないページにアクセスを FormsAuthenticationModule 自動的にリダイレクトされログイン ページに注意してください。 これにより、ReturnUrl パラメーターを使用して、クエリ文字列の要求された URL が含まれます。 つまり、ユーザーが、ProtectedPage.aspx にアクセスしようとしています。 そのためには、承認されていない場合は、FormsAuthenticationModule がリダイレクトするとします。

Login.aspx?ReturnUrl=ProtectedPage.aspx

ログインすると、ProtectedPage.aspx にユーザーをリダイレクトする必要があります。 または、ユーザーには、独自 volition でログイン ページが参照してください可能性があります。 その場合は、ユーザーにログインした後を送信するルート フォルダーの Default.aspx ページに。

### <a name="logging-in-the-user"></a>ユーザーのログイン

指定された資格情報は、有効なは、想定される必要があります、フォーム認証チケットを作成するこれによりユーザーが、サイトでのログ記録します。 [FormsAuthentication クラス](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx)で、 [System.Web.Security 名前空間](https://msdn.microsoft.com/library/system.web.security.aspx)認証システムとでは、フォームを使用してユーザーをログにログ記録用のさまざまなメソッドを提供します。 FormsAuthentication クラスでは、いくつかの方法がありますが、ここではこの時点で、3 つには。

- [GetAuthCookie(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) - creates a forms authentication ticket for the supplied name *username*. 次に、このメソッドは、作成し、認証チケットの内容を保持する HttpCookie オブジェクトを返します。 場合*persistCookie*が true の場合、永続的な cookie が作成されます。
- [SetAuthCookie(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) - calls the GetAuthCookie(*username*, *persistCookie*) method to generate the forms authentication cookie. このメソッドは、(使用されます。 それ以外の場合、cookie ベースのフォーム認証されている、このメソッドが cookie なしのチケットのロジックを処理する内部クラスを呼び出すと仮定した場合) の Cookie のコレクションに GetAuthCookie から返されるクッキーを追加します。
- [RedirectFromLoginPage(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) - this method calls SetAuthCookie(*username*, *persistCookie*), and then redirects the user to the appropriate page.

GetAuthCookie は、Cookie のコレクションに、cookie の書き込み前に認証チケットを変更する必要がある場合に便利です。 SetAuthCookie は、フォーム認証チケットを作成し、Cookie のコレクションに追加するが、適切なページにユーザーをリダイレクトしたくない場合に便利です。 おそらくするいくつかの代替のページに送信したり、ログイン ページに保存します。

ユーザーにログインし、適切なページにリダイレクトするので、RedirectFromLoginPage を利用してみましょう。 更新ログイン ボタンのクリック イベント ハンドラー、2 つのコメントが付けられた TODO 行を次のコード行に置き換えます。

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

フォーム認証チケットのユーザー名 テキスト ボックスのテキストのプロパティを使用して、フォーム認証チケットを作成するときに*username*パラメーター、および RememberMe チェックのチェック状態、 *persistCookie*パラメーター。

ログイン ページをテストするには、ブラウザーで、参照してください。 まずいいえのユーザー名とパスワードは正しくないなどの無効な資格情報を入力します。 [ログイン] ボタンをクリックするとポストバックが発生し、InvalidCredentialsMessage ラベルが表示されます。


[![InvalidCredentialsMessage ラベルが表示されるときに入力する資格情報が無効](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**図 10**:「InvalidCredentialsMessage ラベルが表示されるときに入力する資格情報が無効 ([フルサイズ イメージを表示するに、をクリックして](an-overview-of-forms-authentication-vb/_static/image30.png))


次に、有効な資格情報を入力し、[ログイン] ボタンをクリックします。 ポストバックのフォーム認証チケットが発生すると、この時間が作成され、Default.aspx に自動的にリダイレクトされます。 この時点でにログインが、web サイト現在ログオン中を示すために視覚的な手掛かりはありません。 プログラムによって、ユーザーかどうかを確認する方法がわかりますが、手順 4. でまたはないだけでなく、ページにアクセスしたユーザーを識別する方法が記録されます。

手順 5 では、web サイトからユーザーをログインするための手法を調べます。

### <a name="securing-the-login-page"></a>ログイン ページのセキュリティ保護

Web サーバーにインターネット経由で (自分のパスワードを含む) の資格情報が転送されるときに、ユーザーは自分の資格情報を入力し、ログイン ページのフォームを送信する、*プレーン テキスト*です。 つまり、ハッカーによるネットワーク トラフィックを調べるには、ユーザー名とパスワードを確認できます。 これを回避するのにを使用して、ネットワーク トラフィックを暗号化するために不可欠[セキュリティで保護されたソケット レイヤー (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)です。 これにより、web サーバーによって受信されるまでは、ブラウザーを離脱時から資格情報 (だけでなく、全体のページの HTML マークアップ) が暗号化されています。

Web サイトに機密情報が含まれている場合を除き、ここで、ユーザーのパスワードはそれ以外の場合、送信されますプレーン テキストでネットワーク経由でのログイン ページで、他のページで SSL を使用する必要はだけです。 以降、既定では、これは両方暗号化およびデジタル署名 (改ざんを防ぐため)、フォーム認証チケットのセキュリティ保護について心配する必要はありません。 フォーム認証チケットのセキュリティの詳細については、以下のチュートリアルに表示されます。

> [!NOTE]
> SSL を使用する財務や医療の多くの web サイトが構成されている*すべて*ページにアクセスできるユーザーを認証します。 このような web サイトを構築する場合は、フォーム認証チケットがセキュリティで保護された接続を介して転送のみをフォーム認証システムを構成できます。 次のチュートリアルでは、さまざまなフォーム認証の構成オプションで注目は*[フォーム認証の構成と高度なトピック](../membership/creating-the-membership-schema-in-sql-server-vb.md)*です。


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>手順 4: 認証されたユーザーを検出して、自身の Id を決定します。

この時点で、フォーム認証を有効になり、基本的なログイン ページを作成おが、おまだおできますを特定する方法、ユーザーが認証済みまたは匿名かどうかを確認します。 特定のシナリオでは、さまざまなデータや、認証済みまたは匿名ユーザーがページにアクセスしているかどうかに応じて、情報を表示お必要があります。 さらに、いわゆるいただくために、認証されたユーザーの id を把握します。

これらの手法を説明するために既存の Default.aspx ページを拡張してみましょう。 Default.aspx で、1 つの名前付き AuthenticatedMessagePanel と別の名前付き AnonymousMessagePanel の 2 つのパネル コントロールを追加します。 最初のパネルで WelcomeBackMessage をという名前のラベル コントロールを追加します。 2 番目のパネルのハイパーリンク コントロールを追加、ログでおよびその NavigateUrl プロパティにそのテキスト プロパティを設定 ~/Login.aspx です。 この時点で Default.aspx の宣言型マークアップは、次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

ここまでで推察が、認証された利用者と匿名の訪問者を AnonymousMessagePanel だけに AuthenticatedMessagePanel だけを表示するという考え方です。 これを実現するには、ユーザーがログインしているかどうかどうかに応じて、これらのパネルの表示プロパティを設定する必要があります。

[Request.IsAuthenticated プロパティ](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx)要求が認証されているかどうかを示すブール値を返します。 ページに次のコードを入力\_イベント ハンドラーのコードを読み込みます。

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

配置でこのコードでは、ブラウザーを使って Default.aspx を参照してください。 ログインがまだある、想定されるので、ログイン ページへのリンクが表示されます (図 11 を参照してください)。 このリンクをクリックし、サイトにログインします。 手順 3 で説明したとおり、資格情報を入力した後に表示されます、Default.aspx にではこの時点ページへようこそ の背面! メッセージ (図 12 を参照してください)。


[![オフィスを訪れた匿名で、ログのリンクが表示される場合](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**図 11**: ときにオフィスを訪れた匿名で、ログのリンクが表示されます ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image33.png))


[![認証されたユーザーへようこそ の背面に表示されます。メッセージ](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**図 12**: 認証されたユーザーがへようこそ の背面に表示されます。 メッセージ ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image36.png))


使用して、現在ログオンしているユーザーの id を決定できます、 [HttpContext オブジェクト](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)の[ユーザー プロパティ](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)です。 HttpContext オブジェクトは、現在の要求に関する情報を表し、他のユーザー間では、応答、要求、およびセッションとしてこのような一般的な ASP.NET オブジェクトのホームです。 ユーザー定義プロパティは、現在の HTTP 要求と実装のセキュリティ コンテキストを表す、 [IPrincipal インターフェイス](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx)です。

ユーザー定義プロパティは、FormsAuthenticationModule で設定されます。 具体的には、FormsAuthenticationModule には、受信要求のフォーム認証チケットが検出されると、その新しい GenericPrincipal オブジェクトを作成し、ユーザーのプロパティに割り当てます。

(GenericPrincipal) などのプリンシパルのオブジェクトは、ユーザーの id とが所属するロールに関する情報を提供します。 IPrincipal インターフェイスでは、2 つのメンバーを定義します。

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -プリンシパルが、指定したロールに属しているかを示すブール値を返すメソッド。
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -を実装するオブジェクトを返すプロパティ、 [IIdentity インターフェイス](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)です。 IIdentity インターフェイス定義の 3 つのプロパティ: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx)、 [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)、および[名前](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx)です。

次のコードを使用して現在のユーザーの名前お判断できます。

CurrentUsersName として文字列を dim User.Identity.Name を =

フォーム認証の使用時に、 [FormsIdentity オブジェクト](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx)GenericPrincipal の Identity プロパティを作成します。 FormsIdentity クラスは、常にその IsAuthenticated プロパティの文字列およびフォームの AuthenticationType プロパティは True を返します。 Name プロパティでは、フォーム認証チケットを作成するときに指定されたユーザー名を返します。 FormsIdentity にはだけでなく、これら 3 つのプロパティには、基になる認証チケットを使用してへのアクセスが含まれています、[プロパティのチケット](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)です。 Ticket プロパティが型のオブジェクトを返します[所属](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)、これは有効期限、IsPersistent、IssueDate、名などのプロパティがあります。

対処離れた設定されている重要な点、 *username* 、FormsAuthentication.GetAuthCookie で指定されたパラメーター (*username*、 *persistCookie*)、FormsAuthentication.SetAuthCookie (*username*、 *persistCookie*)、および FormsAuthentication.RedirectFromLoginPage (*username*、 *persistCookie*) メソッドが User.Identity.Name によって返されるのと同じ値です。 さらに、これらのメソッドによって作成された認証チケットは//新しい一般プリンシパル オブジェクトにキャストする、FormsIdentity し Ticket プロパティにアクセスして使用します。

Dim ident As FormsIdentity = CType(User.Identity, FormsIdentity)

認証チケットとして所属を dim ident を = です。チケット

Default.aspx で個人用に設定された複数のメッセージを提供してみましょう。 ページを更新\_WelcomeBackMessage ラベルのテキスト プロパティには、文字列の開始を元に戻すが割り当てられているように、イベント ハンドラーを読み込む*username*!

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

図 13 は、(Scott のユーザーとしてログイン) の場合、この変更の効果を示しています。


[![ウェルカム メッセージが含まれています、現在ログオンしているユーザーの名前で](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**図 13**: ようこそメッセージには、現在ログインにユーザーの名前が含まれています ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>LoginView および LoginName コントロールの使用

一般的な要件は、認証および匿名ユーザーに異なるコンテンツを表示します。そのため、現在ログオンしているユーザーの名前を表示されています。 そのため、ASP.NET には、1 行のコードを記述する必要はありませんが、図 13 に示すように同じ機能を提供する 2 つの Web コントロールが含まれます。

[LoginView コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)テンプレートに基づく Web コントロールを容易に認証および匿名ユーザーに別のデータを表示するものです。 LoginView には、2 つの定義済みテンプレートが含まれています。

- AnonymousTemplate - このテンプレートに追加されたすべてのマークアップは匿名ユーザーにのみ表示されます。
- LoggedInTemplate - このテンプレートのマークアップが認証されたユーザーにのみ表示されます。

サイトのマスター ページ、Site.master LoginView コントロールを追加してみましょう。 LoginView コントロールだけを追加するではなく両方新しい ContentPlaceHolder コントロールを追加してみましょう。 ただし、し、その新しい ContentPlaceHolder 内にある LoginView コントロールを配置します。 この判断に論理的根拠は、間もなく明白になります。

> [!NOTE]
> LoginView コントロールは、AnonymousTemplate と LoggedInTemplate だけでなく、ロール固有のテンプレートを含めることができます。 ロール固有のテンプレートは、指定されたロールに属しているユーザーにのみ、マークアップを表示します。 今後のチュートリアルで LoginView コントロールの役割に基づいた機能を見ていきます。


ナビゲーション内で、マスター ページに LoginContent を名前付きプレース ホルダーを追加することによって開始&lt;div&gt;要素。 ツールボックス、ソース ビューから、TODO の右上の結果として得られるマークアップを配置する ContentPlaceHolder コントロールをドラッグすることだけことができます。 メニューがここにテキスト。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

次に、LoginContent ContentPlaceHolder 内 LoginView コントロールを追加します。 マスター ページの ContentPlaceHolder のコントロールに配置しているコンテンツと見なされます*既定のコンテンツ*ContentPlaceHolder のです。 つまり、このマスター ページを使用する ASP.NET ページは各 ContentPlaceHolder を各自のコンテンツを指定するか、マスター ページの既定のコンテンツを使用することができます。

LoginView、およびその他のログインに関連するコントロールはツールボックスの [ログイン] タブにあります。


[![LoginView コントロールがツールボックスで](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**図 14**: ツールボックスの LoginView コントロール ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image42.png))


次に、2 つ追加&lt;ブラジル/&gt; LoginView コントロールの直後後、ただしもまだ、ContentPlaceHolder 内にある要素。 この時点では、ナビゲーション&lt;div&gt;要素のマークアップは、次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

デザイナーまたは宣言型マークアップから LoginView のテンプレートを定義することができます。 Visual Studio のデザイナーからは、ドロップダウン リストで構成されているテンプレートの一覧を表示する LoginView のスマート タグを展開します。 テキスト、こんにちは AnonymousTemplate; に知らない人を入力します。次に、ハイパーリンク コントロールを追加し、ログ内に、テキストと NavigateUrl プロパティを設定し、~/Login.aspx、それぞれします。

AnonymousTemplate を構成した後、LoggedInTemplate に切り替えるし、テキスト、「ようこそ戻る」を入力します。 LoginName コントロールをツールボックスから「の開始 元に戻す」テキストの直後に配置すること、LoggedInTemplate をドラッグします。 [LoginName コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)、名前として意味、現在ログインしているユーザーの名前を表示します。 内部的には、ログイン名コントロールだけでは出力 User.Identity.Name プロパティ

LoginView のテンプレートへの追加を行った後、マークアップを次のようになります。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Site.master マスター ページにこの追加当社の web サイト内の各ページに、ユーザーが認証されるかどうかに応じて異なるメッセージが表示されます。 図 15 は、ユーザー Jisun ブラウザーからアクセスしたときに、Default.aspx ページを示しています。 メッセージが 2 回繰り返されて Jisun、背面へようこそ: コンテンツ (パネル コントロールとプログラム ロジック) 経由で領域 (経由で追加したばかり LoginView コントロール) の左側に、マスター ページのナビゲーション セクションに 2 回、Default.aspx の中に 2 回です。


[![LoginView コントロールの表示の開始、背面 Jisun です。](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**図 15**: LoginView コントロールが表示されますようこそ戻る、Jisun です。 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image45.png))


マスター ページに、LoginView を追加したのでは、すべてのページについては、サイトに表示できます。 ただし、ある可能性があります web ページをしたくありませんこのメッセージを表示します。 このような 1 つのページは、ログイン ページ、ログイン ページへのリンクがある場所ように見えるためです。 マスター ページ プレース ホルダーに LoginView コントロールを配置してから、コンテンツ ページでこの既定のマークアップをオーバーライドおできます。 Login.aspx を開き、デザイナーに移動します。 コンテンツ コントロールを明示的に定義されてはいないためマスター ページで LoginContent ContentPlaceHolder の Login.aspx でログイン ページは、この ContentPlaceHolder のマスター ページの既定のマークアップが表示されます。 を介して表示できるこのデザイナー - LoginContent ContentPlaceHolder は、既定のマークアップ (LoginView コントロール) を示しています。


[![マスター ページの LoginContent ContentPlaceHolder のログイン ページがコンテンツにある既定値を表示します。](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**図 16**: マスター ページの LoginContent ContentPlaceHolder のコンテンツの既定のログイン ページに表示 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image48.png))


LoginContent ContentPlaceHolder の既定のマークアップをオーバーライドするには、デザイナー内の領域でを右クリックし、コンテキスト メニューからカスタム コンテンツの作成オプションを選択します。 (Visual Studio 2008、ContentPlaceHolder の使用が含まれる場合、スマート タグを選択した場合、同じオプションを提供します)。新しいコンテンツ コントロールをページのマークアップとそれによって追加のカスタム コンテンツをこのページを定義することができます。 など、ログインしてください、ここでは、カスタム メッセージを追加することも、ここだけ空白のままにすることもできます。

> [!NOTE]
> Visual Studio 2005 で、カスタム コンテンツを作成する、空、ASP.NET ページ内のコントロールのコンテンツします。 Visual Studio 2008 でただし、カスタム コンテンツを作成するコピー マスター ページの既定のコンテンツを新しく作成されたコンテンツ コントロールにします。 Visual Studio 2008 を使用している場合、新しいコンテンツ コントロールを作成した後を確認してください、マスター ページから経由でコピーされた内容をクリアするには


図 17 では、この変更を行った後、ブラウザーからアクセスしたときの Login.aspx ページを示します。 こんにちは, 知らない人またはへようこそ のバックアップがないことに注意してください*username*左側のナビゲーションでメッセージ&lt;div&gt; Default.aspx にアクセスする場合は、します。


[![ログイン ページには、既定 LoginContent ContentPlaceHolder のマークアップが非表示になります](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**図 17**: ログイン ページには、既定の LoginContent ContentPlaceHolder のマークアップが非表示になります ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>手順 5: ログアウトしています

手順 3 でについて説明しました、サイトへのユーザーのログインのログイン ページの作成、おまだユーザーがログアウトする方法を参照してください。内のユーザーがログインするためのメソッドに加えて、FormsAuthentication クラスも用意されています、 [SignOut メソッド](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)です。 SignOut メソッドには、フォーム認証チケットは、これにより、サイト外のユーザーのログ記録だけを破棄します。

ログアウト リンクは、このような一般的な機能を提供するその ASP.NET には、ユーザーがログアウトに特化したコントロールが含まれています。[ログイン状態コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx)ログイン LinkButton またはユーザーの認証の状態に応じて、ログアウト LinkButton のいずれかが表示されます。 ログアウト LinkButton が認証されたユーザーに表示されますが、匿名のユーザーに対してログイン LinkButton が表示されます。 ログイン状態の経由でログインおよびログアウトあるテキストを構成できます LoginText および LogoutText プロパティです。

ログインの LinkButton をクリックすると、ログイン ページにリダイレクトが発行元となる、ポストバックが発生します。 ログアウト LinkButton をクリックし、FormsAuthentication.SignOff メソッドを呼び出すため、ログイン状態コントロール、ページに、ユーザーをリダイレクトします。 ログオンしているページは LogoutAction プロパティは、次の 3 つの値のいずれかに割り当てることができますに依存する無効なユーザーをリダイレクトします。

- 更新 - 既定値です。訪問しただけのページに、ユーザーをリダイレクトします。 訪問しただけのページが、FormsAuthenticationModule が自動的には、匿名ユーザーは、許可しない場合は、ログイン ページに、ユーザーをリダイレクトします。

ここで、リダイレクトが実行される理由について調べる必要があります。 場合は、ユーザーが、同じページ上に残る理由の明示的なリダイレクトに必要ですか? ログオフ LinkButton がクリックされたときに、ユーザーがあるフォーム認証チケットのクッキー コレクションのためです。 そのため、認証された要求のポストバック要求です。 ログイン状態コントロールが SignOut メソッドを呼び出しています。 しかし、FormsAuthenticationModule がユーザーを認証した後の動作をします。 そのため、明示的なリダイレクトは、ページを再要求するブラウザーをによりします。 ブラウザー ページを再要求するまでに、フォーム認証チケットが削除され、そのため、受信要求は匿名です。

- リダイレクト - ユーザーは、ログイン状態の LogoutPageUrl プロパティで指定された URL にリダイレクトされます。
- RedirectToLoginPage - ユーザーは、ログイン ページにリダイレクトされます。

みましょうマスター ページにログイン状態コントロールを追加し署名したことを確認するメッセージを表示するページにユーザーに送信するリダイレクト オプションを使用するように構成します。まず Logout.aspx をという名前のルート ディレクトリでページを作成します。 このページを関連付ける Site.master マスター ページを忘れないでください。 次に、出力が記録されたことをユーザーに説明するページのマークアップにメッセージを入力します。

次に、Site.master マスター ページに戻り、LoginContent ContentPlaceHolder で、LoginView 下にあるログイン状態コントロールを追加します。 ~/Logout.aspx にリダイレクトするログイン状態コントロールの LogoutAction プロパティとその LogoutPageUrl プロパティを設定します。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

LoginView コントロールできないところで、ログイン状態があるため、匿名と認証の両方のユーザーに表示されますが、[ok] ため、ログインまたはログアウト LinkButton、ログイン状態が正しく表示します。 ログイン状態コントロールの追加により、ログ内のハイパーリンクをクリック、AnonymousTemplate は余分なので、これを削除します。

図 18 Jisun を訪問したとき、Default.aspx を示しています。 左の列がメッセージを表示することに注意してください、お帰りをログアウト リンクと共に Jisun です。ログアウト LinkButton をクリックするとポストバックを発生させるをシステムから Jisun を署名し、自分を Logout.aspx にリダイレクトします。 図 19 に示す Jisun Logout.aspx に達した時点で彼女署名済みおよび匿名であるためです。 その結果、左の列は、テキストようこそ で、え、ログイン ページへのリンクを示します。


[![Default.aspx、ログアウト LinkButton と共に Jisun を新規に作成を示しています。](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**図 18**: Default.aspx 示しますようこそ戻る、Jisun に沿ってログアウト LinkButton で ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx 示しますへようこそ はログイン LinkButton と共に知らない人](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**図 19**: Logout.aspx 示しますへようこそ はログイン LinkButton と共に知らない人 ([フルサイズのイメージを表示するをクリックして](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> I は、(同じように手順 4. で Login.aspx 用)、マスター ページの LoginContent ContentPlaceHolder を非表示にする Logout.aspx ページをカスタマイズすることを勧めします。 ログインの LinkButton がログイン状態コントロールによって表示されるためです (こんにちは、下にある 1 つ知らない人) ReturnUrl querystring パラメーターの現在の URL を渡してログイン ページに、ユーザーに送信します。 つまりログオンしがログアウトしたユーザーがこのログイン状態のログインの LinkButton がクリックした場合は、Logout.aspx で、ユーザーが混乱することが簡単に戻るがリダイレクトされます。


## <a name="summary"></a>まとめ

このチュートリアルでは、フォーム認証ワークフローの検証の使用を開始をし、ASP.NET アプリケーションでフォーム認証を実装するためになっています。 フォーム認証の 2 つの役割を持つ、FormsAuthenticationModule で電源が入って: フォーム認証チケットに基づくユーザーを識別し、承認されていないユーザーをログイン ページにリダイレクトします。

.NET Framework の FormsAuthentication クラスには、作成、検査、およびフォーム認証チケットを削除するためのメソッドが含まれています。 Request.IsAuthenticated プロパティとユーザー オブジェクトを要求が認証されたかどうかと、ユーザーの身元に関する情報を決定するその他のプログラムによるサポートを提供します。 ログインに関連する多くの一般的なタスクを実行するため、コードのない方法を開発者に提供 LoginView、ログイン状態、および LoginName Web コントロールがあります。 今後のチュートリアルでは、これらおよび他のログインに関連する Web コントロールで詳しくを考察はします。

このチュートリアルでは、フォーム認証の大まかな概要を提供します。 おまたはしていないさまざまな構成オプションを調べ、どの cookie なしフォーム認証チケットの動作を見て ASP.NET がフォーム認証チケットの内容を保護する方法を調査します。 これらのトピックと詳細を説明します、[次のチュートリアル](forms-authentication-configuration-and-advanced-topics-vb.md)です。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Iis 6 と iis 7 のセキュリティの変更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [ログインの ASP.NET コントロール](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [プロフェッショナル向けの ASP.NET 2.0 セキュリティ、メンバーシップ、およびロール管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [&lt;認証&gt;要素](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;フォーム&gt;要素&lt;認証&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックに関するビデオ トレーニング

- [ASP.NET で基本フォーム認証を使用する](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Alicja Maziarz、John Suru Teresa マーフィーなどがあります。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com)です。

>[!div class="step-by-step"]
[前へ](security-basics-and-asp-net-support-vb.md)
[次へ](forms-authentication-configuration-and-advanced-topics-vb.md)
