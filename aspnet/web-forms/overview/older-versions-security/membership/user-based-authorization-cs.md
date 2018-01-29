---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: "ユーザー ベースの承認 (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限するのになります。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bee98878b5191a096b851c65aaea19ad989f608
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="user-based-authorization-c"></a>ユーザー ベースの承認 (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限するのになります。


## <a name="introduction"></a>はじめに

ユーザー アカウントを提供するほとんどの web アプリケーションでは、サイト内の特定のページへのアクセスを特定のユーザーを制限する一部のようにします。 大半のオンライン messageboard サイト、たとえば、- 匿名と認証済みのすべてのユーザーが messageboard の投稿を表示することいますが、認証されたユーザーだけが新しい投稿を作成する web ページを参照してください。 特定のユーザー (または、特定の一連のユーザー) にアクセスできる、のみの管理ページがある可能性があります。 さらに、ページ レベルの機能は、ユーザー単位のユーザーごとに異なることができます。 投稿の一覧を表示すると、認証されたユーザーはこのインターフェイスは、匿名の訪問者に対し、その個々 の投稿を評価するためのインターフェイスに表示されます。

ASP.NET では、簡単にユーザー ベースの承認規則を定義します。 内のマークアップのほんの少しだけ`Web.config`、特定の web ページまたはディレクトリ全体をロックできますダウンは、のみ、指定したユーザーのサブセットにアクセスできるようにします。 ページ レベルの機能にできます、オンまたはオフ プログラムと宣言の手段で現在ログインしているユーザーに基づいて。

このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限するのになります。 開始しましょう!

## <a name="a-look-at-the-url-authorization-workflow"></a>URL 承認ワークフローを見る

説明したように、 [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、ASP.NET ランタイムが ASP.NET リソース要求のライフ サイクル中にイベントの数を発生させるため、要求を処理するときにします。 *HTTP モジュール*マネージ クラスのコードを持つは要求のライフ サイクルの特定のイベントに応答して実行します。 ASP.NET は、バック グラウンドでの必須のタスクを実行する HTTP モジュールの数が付属しています。

このような 1 つの HTTP モジュールが[ `FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)です。 主な機能、前のチュートリアルで説明したように、`FormsAuthenticationModule`は現在の要求の id を確認します。 これは、cookie であるか、URL 内に埋め込まれているフォーム認証チケットを調べることによって行います。 この識別中に行われる、 [ `AuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)です。

別の重要な HTTP モジュールが、 [ `UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)への応答で発生する、 [ `AuthorizeRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)(後の動作を`AuthenticateRequest`イベント)。 `UrlAuthorizationModule`で構成のマークアップを調べて`Web.config`を現在の id が指定したページにアクセスする権限を持つかどうかを判断します。 このプロセスと呼びます*URL 承認*です。

について確認手順 1 で、URL 承認規則の構文がまずみましょう見てどのような`UrlAuthorizationModule`は、要求が承認されているかどうかどうかによって異なります。 場合、`UrlAuthorizationModule`と判断した要求が承認されると、し、それは何も、ライフ サイクルを通じて要求が続行されます。 ただし、要求がある場合*いない*、承認、`UrlAuthorizationModule`ライフ サイクルを中止し、指示、`Response`返されるオブジェクト、 [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html)状態。 この HTTP 401 ステータスは、クライアントに返されませんフォーム認証を使用するときに場合、`FormsAuthenticationModule`ステータスは、HTTP 401 に変更を検出した、 [HTTP 302 リダイレクト](http://www.checkupdown.com/status/E302.html)ログイン ページにします。

図 1 は、ASP.NET パイプラインのワークフローを示す、 `FormsAuthenticationModule`、および`UrlAuthorizationModule`未承認の要求の受信時にします。 具体的には、図 1 はの匿名の訪問者によって要求を示しています。 `ProtectedPage.aspx`、これは匿名ユーザーにアクセスを拒否するページ。 訪問者が、匿名であるため、`UrlAuthorizationModule`は要求を中止し、HTTP 401 Unauthorized ステータスを返します。 `FormsAuthenticationModule`ログイン ページへの 302 リダイレクトに 401 ステータスを変換します。 ログイン ページを使用して、ユーザーが認証された後にリダイレクトされます`ProtectedPage.aspx`です。 この時間、`FormsAuthenticationModule`彼認証チケットに基づく、ユーザーを識別します。 訪問者が認証されると、これで、`UrlAuthorizationModule`ページへのアクセスを許可します。


[![フォーム認証と承認ワークフローの URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**図 1**: フォーム認証と承認ワークフローの URL ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image3.png))


図 1 は、匿名ユーザーが匿名ユーザーに利用可能なないリソースにアクセスしようとしたときに発生する相互作用を示しています。 このような場合は、匿名ユーザーは、クエリ文字列の指定を参照してください彼女しようとしました ページで、ログイン ページにリダイレクトされます。 ユーザーが正常にログオン後、彼女は自動的にリダイレクトされます彼女が最初に表示しようとするリソースに。

未承認の要求が行われると、匿名ユーザーによって、このワークフローは簡単です、簡単に何が起こったかを理解するビジターおよびその理由です。 ことに注意に維持しながら、`FormsAuthenticationModule`がリダイレクト*任意*要求が認証済みユーザーによって行われた場合でも、ログイン ページにユーザー権限がありません。 機関を彼女がないページにアクセスしようとすると、認証されたユーザーは、ユーザー エクスペリエンスに混乱をこれがあります。

当社の web サイトの URL 承認規則の構成であることを想像してくださいになるよう、ASP.NET ページ`OnlyTito.aspx`Tito にのみアクセスできます。 でした。 Sam にサイトを訪問し、ログオンして、アクセスを試むことになっているでしょう`OnlyTito.aspx`です。 `UrlAuthorizationModule`は要求のライフ サイクルを中断し、HTTP 401 Unauthorized ステータスを返します`FormsAuthenticationModule`が検出され、Sam をログイン ページにリダイレクトします。 Sam のログインが既に、以降は、彼女は可能性があります不思議に思えるかもしれません理由彼女が送信された、ログイン ページに戻ります。 彼女は、自分のログイン資格情報が何らかの理由で、失われたまたは彼女に無効な資格情報が入力されている理由可能性があります。 Sam は、ログイン ページから自分の資格情報を再入力する場合彼女 (再) ログに記録されにリダイレクト`OnlyTito.aspx`です。 `UrlAuthorizationModule`ことが検出 Sam は、このページを開いたことはできませんし、ログイン ページに彼女が返されます。

図 2 は、この混乱を招くワークフローを示しています。


[![混乱を招くサイクルにつながる可能性が既定のワークフロー](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**図 2**: の既定のワークフローにつながる可能性が混乱を招くサイクル ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image6.png))


図 2 に示すワークフローには、でも、ほとんどのコンピューター詳しければビジター befuddle すばやくことができます。 手順 2. でサイクルが混乱これを防止する方法について取り上げます。

> [!NOTE]
> ASP.NET では、2 つのメカニズムを使用して、現在のユーザーが特定の web ページにアクセスできるかどうかを判断します。 URL 承認およびファイルの承認。 ファイルの承認がによって実装される、 [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)、要求されたファイルの Acl を参照して権限を決定します。 ファイルの承認が最もよく使用 Windows 認証されている Acl を Windows アカウントに適用されるアクセス許可があるためです。 フォーム認証を使用する場合は、オペレーティング システムとファイル システム レベルのすべての要求がサイトにアクセスするユーザーに関係なく、同じ Windows アカウントで実行されます。 このチュートリアルの系列では、フォーム認証に焦点を当てています、のでおはについては説明しませんファイル承認します。


### <a name="the-scope-of-url-authorization"></a>URL 承認のスコープ

`UrlAuthorizationModule`はマネージ コードは、ASP.NET ランタイムの一部であります。 前の Microsoft のバージョン 7[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) web サーバー IIS の HTTP パイプラインと、ASP.NET ランタイムのパイプラインの間で異なるバリアが発生しました。 要約すると、IIS 6 と前に、ASP です。NET の`UrlAuthorizationModule`ASP.NET ランタイムを IIS からの要求が委任されて場合にのみ実行します。 既定では、IIS が HTML ページ、CSS、JavaScript、およびイメージ ファイルのように静的なコンテンツそのものを処理し、ASP.NET ランタイムときの拡張機能のページへの要求を渡しのみ`.aspx`、 `.asmx`、または`.ashx`を要求します。

IIS 7 では、ただしでは、統合された IIS と ASP.NET パイプライン。 呼び出すための IIS 7 をセットアップするいくつかの構成設定で、`UrlAuthorizationModule`の*すべて*要求、任意の種類のファイルに対する URL 承認規則を定義できることを意味します。 さらに、IIS 7 には、独自の URL 承認エンジンが含まれています。 ASP.NET 統合と IIS 7 のネイティブの URL 承認機能の詳細については、次を参照してください。 [Understanding IIS7 URL 承認](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)です。 ASP.NET と IIS 7 の統合の詳細については、Shahram Khosravi のブックのコピーを取得します。 *Professional IIS 7 と ASP.NET 統合プログラミング*(ISBN: 978 0470152539)。

簡単に言うと、IIS 7 より前のバージョン、URL 承認規則だけに適用されます、ASP.NET ランタイムによって処理されるリソース。 IIS 7 と IIS のネイティブの URL 承認機能を使用したり、ASP を統合することができます。NET の`UrlAuthorizationModule`に IIS の HTTP パイプラインでは、すべての要求にこの機能を拡張することです。

> [!NOTE]
> 方法にいくつかの微妙ながら、重要な違いが ASP です。NET の`UrlAuthorizationModule`と IIS 7 の URL 承認の機能は、承認規則を処理します。 このチュートリアルは、IIS 7 の URL の承認機能やと比較すると、承認規則を解析する方法の違いを検査しない、`UrlAuthorizationModule`です。 これらのトピックの詳細については、msdn または IIS 7 のドキュメントを参照してください[www.iis.net](https://www.iis.net/)です。


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>手順 1: での URL 承認規則を定義します。`Web.config`

`UrlAuthorizationModule`アプリケーションの構成で定義されている URL 承認規則に基づいて特定の id の要求されたリソースにアクセス許可または拒否するかどうかを決定します。 承認規則が記述された、 [ `<authorization>`要素](https://msdn.microsoft.com/library/8d82143t.aspx)の形式で`<allow>`と`<deny>`子要素です。 各`<allow>`と`<deny>`子要素を指定できます。

- 特定のユーザー
- ユーザーのコンマ区切りの一覧
- 疑問符 (?) で表される、すべての匿名ユーザー
- アスタリスクで示される、すべてのユーザー (\*)

次のマークアップは、ユーザー Tito および Scott を許可およびその他のすべてを拒否する URL 承認規則を使用する方法を示しています。

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` - Tito と Scott - ユーザーは、要素を定義中に、`<deny>`要素に指示する*すべて*ユーザーが拒否されました。

> [!NOTE]
> `<allow>`と`<deny>`要素は、ロールの承認規則も指定できます。 今後のチュートリアルでのロールベースの承認を見ていきます。


次の設定は、Sam (匿名の訪問者を含む) 以外のユーザーにアクセスを付与します。

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

認証されたユーザーのみを許可するのには、次の構成は、すべての匿名ユーザーに対してアクセスを拒否を使用します。

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

承認規則が内で定義された、`<system.web>`内の要素`Web.config`し、すべての web アプリケーションで ASP.NET リソースに適用します。 多くの場合、アプリケーションでは、さまざまなセクションのさまざまな承認規則があります。 など、電子商取引サイトですべてのビジター可能性があります、製品を熟読、製品レビューを参照してください、カタログを検索およびなどです。 ただし、認証されたユーザーだけは、チェック アウト、または 1 つの配布の履歴を管理するページに到達可能性があります。 さらに、サイト管理者など、ユーザーの選択 でアクセスできるサイトの部分があります。

ASP.NET では、簡単に、サイトの別のファイルとフォルダーのさまざまな承認規則を定義します。 ルート フォルダーの指定された承認規則`Web.config`ファイルは、サイト内のすべての ASP.NET リソースに適用します。 ただし、これら既定承認上書きできる特定のフォルダーを追加することによって、`Web.config`で、`<authorization>`セクションです。

認証されたユーザーのみが ASP.NET のページにアクセスできるようにしましょう当社の web サイトを更新する、`Membership`フォルダーです。 追加する必要があります。 これを実現する、`Web.config`ファイルの名前を、`Membership`フォルダーを匿名ユーザーを拒否するには、その承認設定を設定します。 右クリックし、`Membership`ソリューション エクスプ ローラーでフォルダーが、コンテキスト メニューから、[新しい項目の追加] メニューを選択し、という名前の新しい Web 構成ファイルを追加`Web.config`です。


[![メンバーシップ フォルダーに Web.config ファイルを追加します。](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**図 3**: 追加、`Web.config`ファイルの名前を`Membership`フォルダー ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image9.png))


この時点で、プロジェクトを含めることは 2 つ`Web.config`ファイル: 1 つで 1 つのルート ディレクトリで、`Membership`フォルダーです。


[![アプリケーションは、2 つの Web.config ファイルを含める必要がありますようになりました](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**図 4**: Your アプリケーションする必要がありますを含む 2 つ`Web.config`ファイル ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image12.png))


構成ファイルを更新、`Membership`フォルダーが匿名ユーザーにアクセスを禁止するようにします。

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

すべてであることには!

この変更をテストするには、ブラウザーでのホーム ページを参照してくださいし、ログに記録されるかどうかを確認します。ASP.NET アプリケーションの既定の動作は、すべてのビジターを許可するため、ルート ディレクトリの承認変更を加えることがありませんでしたので`Web.config`ファイル、匿名ユーザーとしてのルート ディレクトリ内のファイルにアクセスすることができます。

左の列にあるユーザー アカウントを作成するリンクをクリックします。 これは、操作により、`~/Membership/CreatingUserAccounts.aspx`です。 以降、`Web.config`ファイルで、`Membership`フォルダーは、匿名アクセスを禁止する承認規則を定義、`UrlAuthorizationModule`は要求を中止し、HTTP 401 Unauthorized ステータスを返します。 `FormsAuthenticationModule`ログイン ページに送信する、302 リダイレクト ステータスにこれを変更します。 あるページおしようとしたアクセスに注意してください (`CreatingUserAccounts.aspx`) 経由でログイン ページに渡される、 `ReturnUrl` querystring パラメーター。


[![URL 承認規則匿名アクセスを禁止する、以降は、ログイン ページにリダイレクトしておされます。](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**図 5**: おはログイン ページにリダイレクト URL 承認規則の匿名アクセスを禁止する場合、ので ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image15.png))


リダイレクトしてログインすると、時に、`CreatingUserAccounts.aspx`ページ。 この時間、`UrlAuthorizationModule`匿名不要になったために、ページへのアクセスを許可します。

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>特定の場所に URL 承認規則を適用します。

定義されている承認の設定、`<system.web>`のセクション`Web.config`そのディレクトリとそのサブディレクトリ内の ASP.NET リソースのすべてに適用 (それ以外の場合、別の作業でオーバーライドされるまで`Web.config`ファイル)。 場合によっては、ただしは必要があります、1 つまたは 2 つの特定のページを除く特定の承認設定に指定されたディレクトリ内のすべての ASP.NET リソース。 追加することでこれを行う、`<location>`内の要素`Web.config`が承認規則とは異なる、ファイルを指す、およびその中の一意の承認規則を定義します。

使用して説明するために、`<location>`みましょう Tito のみがアクセスできるように承認の設定をカスタマイズ、特定のリソースの構成設定をオーバーライドする要素`CreatingUserAccounts.aspx`です。 これを実現するには追加、`<location>`要素を`Membership`フォルダーの`Web.config`ファイルし、次のように見えるように、そのマークアップを更新します。

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>`内の要素`<system.web>`、ASP.NET のリソースの既定の URL 承認規則を定義、`Membership`フォルダーとそのサブフォルダーです。 `<location>`要素により、特定のリソースに対してこれらの規則を上書きします。 上記のマークアップで、`<location>`要素の参照、`CreatingUserAccounts.aspx`ページおよび Tito を許可するなどの承認規則を指定しますが、拒否する他のすべてです。

この承認の変更をテストするには、匿名ユーザーとして、web サイトを訪問して起動します。 任意のページにアクセスしようとする場合、`Membership`フォルダーなど`UserBasedAuthorization.aspx`、`UrlAuthorizationModule`要求を拒否し、ログイン ページにリダイレクトされます。 言い、Scott、としてログインした後の任意のページにアクセスすることができます、`Membership`フォルダー*を除く*の`CreatingUserAccounts.aspx`します。 アクセスしようとしています。`CreatingUserAccounts.aspx`すべてのユーザーとしてログオンして起こしません Tito が承認されていないアクセス試行、ログイン ページにリダイレクトします。

> [!NOTE]
> `<location>`要素は、構成の外側に記述する必要があります`<system.web>`要素。 個別を使用する必要があります`<location>`各リソースの承認設定をオーバーライドするための要素。


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>見てどの`UrlAuthorizationModule`アクセスを許可または拒否の承認規則の使用

`UrlAuthorizationModule`かどうかを特定の URL の URL 承認を分析して特定の id を承認する規則を最初から開始し、ダウンさかのぼり、一度に 1 つを決定します。 場合によっては、一致が見つかったで、一致が見つかるとすぐに、ユーザーがアクセス許可または拒否する`<allow>`または`<deny>`要素。 **一致するものが見つからない場合、ユーザーがアクセスを許可します。** そのため、アクセスを制限する場合を使用すること、 `<deny>` URL 承認の構成の最後の要素としての要素。 **省略した場合、* * *`<deny>`* * *、要素のすべてのユーザー アクセスが許可されます。**

使用されるプロセスを理解するのには、`UrlAuthorizationModule`機関を確認するのに例を考えてみます URL 承認規則がこの手順について説明しました。 最初のルールを`<allow>`Tito および Scott へのアクセスを可能な要素です。 2 番目のルールは、`<deny>`すべてのユーザーにアクセスを拒否する要素。 匿名ユーザーがアクセスする場合、`UrlAuthorizationModule`調べれば、開始が匿名 Scott または Tito のいずれかですか? 答えは、当然ながらはなし、2 番目のルールに進みます。 すべてのユーザーのセットに匿名ですか。 以降、応答は、[はい]、ここで、`<deny>`とルールが有効で格納されます訪問者がログイン ページにリダイレクトします。 同様に Jisun を訪問している場合は、`UrlAuthorizationModule`質問は Jisun 開始 Scott または Tito しますか? 彼女はではないため、 `UrlAuthorizationModule` 2 番目の質問では、すべてのユーザーのセットには、Jisun に進みますか? 彼女は、彼女は、アクセスが拒否されるようにします。 最後に、Tito 場合、最初の質問はによって引き起こされる、`UrlAuthorizationModule`肯定の応答は Tito はアクセスが許可されます。

以降、`UrlAuthorizationModule`プロセスが、承認規則の上、すべての一致で停止するから特定の規則を汎用性の前に設定することが重要です。 つまり、Jisun と、匿名ユーザーが禁止されているが、他のすべての認証されたユーザーを許可する承認規則を定義するのにはの先頭に最も固有のルールの 1 つに影響を与えず Jisun - してより限定規則の他のすべてを許可するものに進みます認証されたユーザーがすべての匿名ユーザーを拒否します。 次の URL 承認規則では、まず Jisun、拒否して、匿名ユーザーを拒否して、このポリシーを実装します。 Jisun 以外のユーザーは認証済みアクセスが許可されるためこれらのどちらも`<deny>`ステートメントと一致することです。

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>手順 2: 承認されていない、認証されたユーザーに対して、ワークフローを修正します。

URL 承認のワークフロー セクションには、A の場所 では、このチュートリアルで既に説明したよう anytime 未承認の要求には、`UrlAuthorizationModule`は要求を中止し、HTTP 401 Unauthorized ステータスを返します。 この 401 ステータスの変更では、 `FormsAuthenticationModule` 302 にユーザーをログイン ページに送信する状態をリダイレクトします。 このワークフローは、ユーザーが認証された場合でも、未承認の要求で発生します。

ログイン ページに、認証されたユーザーを返すことは、システムに既にログインしている、以降は混乱する可能性があります。 ほんの少しの作業が、制限されているページにアクセスしようとしたことを説明するページに対して承認されていない要求を行う認証されたユーザーをリダイレクトすることで、このワークフローを向上します。

という名前の web アプリケーションのルート フォルダーに新しい ASP.NET ページを作成して開始`UnauthorizedAccess.aspx`; 忘れずにこのページに関連付ける、`Site.master`マスター ページ。 このページを作成した後にを参照するコンテンツ コントロールを削除、 `LoginContent` ContentPlaceHolder のため、マスター ページの既定のコンテンツを表示するようにします。 つまり、状況を説明するメッセージを次に、追加、ユーザーが保護されたリソースにアクセスしようとしたことです。 このようなメッセージを追加した後、`UnauthorizedAccess.aspx`ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

今すぐに送信される場合は、承認されていない要求が認証済みユーザーによって実行されるように、ワークフローを変更する必要があります、`UnauthorizedAccess.aspx`ログイン ページではなくページ。 プライベート メソッド内でログイン ページに不正な要求をリダイレクトするロジックが埋め込まれています、`FormsAuthenticationModule`クラス、おこの動作をカスタマイズすることはできません。 ユーザーをリダイレクトするログイン ページに独自のロジックを追加、何ができるか、ただし、 `UnauthorizedAccess.aspx`、必要に応じて。

ときに、`FormsAuthenticationModule`未承認のユーザーをリダイレクトするログイン ページに、名前を持つクエリ文字列の要求、承認されていない URL の追加`ReturnUrl`です。 たとえば、未認証のユーザーにアクセスしようとしました。 `OnlyTito.aspx`、`FormsAuthenticationModule`がリダイレクトするに`Login.aspx?ReturnUrl=OnlyTito.aspx`です。 そのため、認証されたユーザーを含むクエリ文字列でログイン ページに達した場合、`ReturnUrl`パラメーターは、次のトピックこの未認証のユーザーだけ試行されたことを表示するユーザー権限がないページを参照してください。 このような場合は、彼女にリダイレクトする`UnauthorizedAccess.aspx`です。

これを実現するには、ログイン ページの次のコードを追加`Page_Load`イベントのハンドラー。

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

上記のコード、承認されていない認証されたユーザーをリダイレクトする、`UnauthorizedAccess.aspx`ページ。 このロジックの動作を表示するには、匿名ユーザーとしてサイトにアクセスし、左の列にユーザー アカウントを作成するリンクをクリックします。 これは、操作により、`~/Membership/CreatingUserAccounts.aspx`ページのみを構成しました手順 1. で Tito へのアクセス許可。 匿名ユーザーは禁止されていますので、 `FormsAuthenticationModule` us をログイン ページにリダイレクトします。

この時点では匿名ため、`Request.IsAuthenticated`返します`false`にリダイレクトされないことと`UnauthorizedAccess.aspx`です。 代わりに、ログイン ページが表示されます。 Tito、ブルースなど別のユーザーとしてログインします。 適切な資格情報を入力すると、ログイン ページに設定されて us リダイレクト`~/Membership/CreatingUserAccounts.aspx`です。 ただし、このページが Tito にアクセスできるのみ、ためおを表示する権限し、ログイン ページに迅速に返されます。 ただし、この時間`Request.IsAuthenticated`返します`true`(および`ReturnUrl`querystring パラメーターが存在する) にリダイレクトしましたので、`UnauthorizedAccess.aspx`ページ。


[![認証、承認されていないユーザーは UnauthorizedAccess.aspx にリダイレクトされます。](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**図 6**: 認証、承認されていないユーザーがリダイレクトされるを`UnauthorizedAccess.aspx`([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image18.png))


このカスタマイズされたワークフローは、短いサーキット図 2 にサイクルよりわかりやすい、単純なユーザー エクスペリエンスを表示します。

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>手順 3: 現在のログインのユーザーに基づく機能を制限します。

URL 承認簡単粗い承認規則を指定できます。 手順 1. で説明したとおり、URL の承認おできます簡潔状態どの id が許可されており、どれがフォルダー内の特定のページまたはすべてのページの表示を拒否します。 特定のシナリオでただし、することも、ページにアクセスして、訪問するユーザーに基づいて、ページの機能を制限するためのすべてのユーザーを許可します。

独自の製品を確認する認証済みの訪問者をできるようにする電子商取引 web サイトの場合を考えます。 匿名ユーザーが製品のページを訪問したとき、製品情報だけが表示されない与えられます、レビューのままにすることです。 ただし、同じページにアクセス、認証されたユーザーには 変更履歴のインターフェイス参照してください。 認証されたユーザーは、この製品をまだ確認されませんが場合、インターフェイスが有効にする; レビューを送信するにはそれ以外の場合に表示して、以前から送信されたレビュー。 このシナリオの手順を実行するには、さらに、製品ページ可能性があります追加情報を表示および e コマース ポータルで使用できるこれらのユーザーの拡張機能を提供します。 やなどの製品ページ可能性があります stock では、インベントリの一覧を製品の価格と従業員がアクセスしたときの説明を編集するオプションが含まれます。

宣言またはプログラムによって (または、2 つの組み合わせを通じて)、このような細かい粒度の承認規則を実装することができます。 次のセクションで LoginView コントロールを使用して細かい粒度の承認を実装する方法が表示されます。 次は、プログラムによる手法について説明します。 きめ細かく承認規則を適用することで見ることができます、前にただし、まずいただくために機能を持つは、それにアクセスしたユーザーによって異なります。 ページを作成します。

GridView 内の特定のディレクトリ内のファイルを一覧するページを作成してみましょう。 GridView がある 2 つの列を含める各ファイルの名前、サイズ、およびその他の情報を一覧表示する、と共に: いずれかのビュー「and」というタイトルが付けられた 1 つの削除。 ビューの LinkButton がクリックされた場合、選択したファイルの内容が表示されます。削除の LinkButton がクリックされた場合は、ファイルが削除されます。 そのビューおよび delete の機能をすべてのユーザーに使用できるように、このページを最初に作成してみましょう。 使用方法で LoginView コントロールおよび機能をプログラムで制限するセクションでは、有効にするにまたはページにアクセスするユーザーに基づくこれらの機能を無効にする方法が表示されます。

> [!NOTE]
> ビルドには ASP.NET ページは、ファイルの一覧を表示するのに GridView コントロールを使用します。 このチュートリアルでは、系列は、フォーム認証、承認、ユーザー アカウント、およびロールに焦点を当てています、以降しない GridView コントロールの内部動作をについて説明する時間をかけるにします。 このチュートリアルでは、このページを設定するための特定の詳細な手順についてはの特定の選択肢が行われた理由または影響の特定のプロパティが表示される出力が詳細には掘り下げてされません。 GridView コントロールの詳細についてを参照してください   *[ASP.NET 2.0 のデータを扱う](../../data-access/index.md)*一連のチュートリアルです。


開いて開始、`UserBasedAuthorization.aspx`ファイルで、`Membership`フォルダーとという名前のページへの GridView コントロールの追加`FilesGrid`です。 GridView のスマート タグで、[フィールド] ダイアログ ボックスを起動する列の編集リンクをクリックします。 ここでは、左下隅で自動生成のフィールドのチェック ボックスをオフにします。 次に、左上隅 (Select および削除のボタンで見つかります CommandField 型) から選択 ボタン、削除 ボタン、および 2 つの BoundFields を追加します。 [選択] ボタンを設定`SelectText`プロパティを表示し、最初の BoundField の`HeaderText`と`DataField`プロパティ名にします。 2 番目の BoundField の設定`HeaderText`プロパティ サイズ (バイト単位) をその`DataField`プロパティの長さをその`DataFormatString`{0: n0} プロパティとその`HtmlEncode`プロパティを False にします。

GridView の列を構成した後、[フィールド] ダイアログ ボックスを閉じるには、[ok] をクリックします。 [プロパティ] ウィンドウからの設定、GridView の`DataKeyNames`プロパティを`FullName`です。 この時点で、GridView の宣言型マークアップは、次のようになります。

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

GridView のマークアップを作成、特定のディレクトリ内のファイルを取得し、それらを GridView にバインドするコードを記述する準備ができました。 ページの次のコードを追加`Page_Load`イベントのハンドラー。

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

上記のコードを使用して、 [ `DirectoryInfo`クラス](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx)アプリケーションのルート フォルダー内のファイルの一覧を取得します。 [ `GetFiles()`メソッド](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx)ディレクトリ内のすべてのファイルの配列として返します[`FileInfo`オブジェクト](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)、これが GridView にバインドし、します。 `FileInfo`オブジェクトなどさまざまなプロパティがあります`Name`、 `Length`、および`IsReadOnly`、その他。 GridView がだけ表示されます、宣言型マークアップからわかるように、`Name`と`Length`プロパティです。

> [!NOTE]
> `DirectoryInfo`と`FileInfo`クラスは、 [ `System.IO`名前空間](https://msdn.microsoft.com/library/system.io.aspx)です。 では、これらのクラス名、名前空間の名前を付けますまたはクラス ファイルにインポートされた名前空間があるかが必要 (を介して`using System.IO`)。


すぐをブラウザーからこのページを参照してください。 アプリケーションのルート ディレクトリに存在するファイルの一覧が表示されます。 クリックすると、ビューまたはあるの削除のいずれかによって、ポストバックがアクションが発生しないきたところ、必要なイベント ハンドラーを作成します。


[![GridView は、Web アプリケーションのルート ディレクトリ内のファイルを一覧表示します。](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**図 7**: GridView は、Web アプリケーションのルート ディレクトリにファイルを示します ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image21.png))


選択したファイルの内容を表示するための手段が必要です。 Visual Studio に戻り、という名前のテキスト ボックスを追加して`FileContents`GridView 上。 設定の`TextMode`プロパティを`MultiLine`とその`Columns`と`Rows`プロパティ 95%、10、それぞれします。

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Gridview のイベント ハンドラーを次に、作成[`SelectedIndexChanged`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx)し、次のコードを追加します。

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

この例では、GridView の`SelectedValue`プロパティを選択したファイルの完全なファイル名を確認します。 内部的には、`DataKeys`コレクションが取得するために参照されている、 `SelectedValue`、GridView を設定することが不可欠であるため、`DataKeyNames`プロパティを名前、この手順で既に説明したようにします。 [ `File`クラス](https://msdn.microsoft.com/library/system.io.file.aspx)に代入している文字列に、選択したファイルの内容の読み取りに使用、`FileContents`テキスト ボックスの`Text`プロパティ、それによって ページで選択したファイルの内容を表示します。


[![テキスト ボックスに、選択したファイルの内容が表示されます。](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**図 8**: テキスト ボックスに、選択したファイルの内容が表示されます ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> HTML マークアップを含むファイルの内容を表示して表示またはファイルの削除を試みますが表示されます、`HttpRequestValidationException`エラーです。 これは、ポストバック時に、テキスト ボックスの内容は web サーバーに送信されるために発生します。 既定では、ASP.NET を発生させます、`HttpRequestValidationException`エラーは、HTML マークアップなどの危険性のあるポストバック コンテンツを検出します。 このエラーの発生を無効にするには、してオフに、ページの要求の検証を追加する`ValidateRequest="false"`を`@Page`ディレクティブです。 どのような予防措置としても行う必要がありますタイミングとして要求の検証の利点の詳細について無効にすることをお読み[要求の検証 - スクリプト攻撃の防止](https://asp.net/learn/whitepapers/request-validation/)です。


Gridview の最後に、次のコードでイベント ハンドラーを追加[`RowDeleting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

コードで、削除するファイルの完全名を表示するだけ、 `FileContents` TextBox*せず*ファイルを実際に削除します。


[![[削除] ボタンをクリックするとは実際にファイルを削除できません。](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**図 9**: ファイルをクリックする、削除ボタンは実際に削除されません ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image27.png))


手順 1 で匿名ユーザーが内のページを表示することを禁止する URL 承認規則を構成しました、`Membership`フォルダーです。 細かい粒度の認証を示す、するためにできるようにして、匿名ユーザーのアクセス、 `UserBasedAuthorization.aspx`  ページで、限られた機能がします。 このページに開放にすべてのユーザーがアクセスして、次のコードを追加`<location>`要素を`Web.config`ファイルで、`Membership`フォルダー。

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

これを追加した後`<location>`要素、サイトからのログ記録で新しい URL 承認規則をテストします。 匿名ユーザーとしてする必要がありますを許可するを参照してください、`UserBasedAuthorization.aspx`ページ。

現時点では、任意の認証済みまたは匿名ユーザーがアクセスできる、`UserBasedAuthorization.aspx`ページを表示し、またはファイルを削除します。 させて認証されたユーザーのみがファイルの内容を表示し、Tito のみが、ファイルを削除できるようにします。 宣言によって、プログラム、または両方のメソッドの組み合わせによって、このような細かい粒度の承認規則を適用できます。 みましょう宣言型の方法を使用して、ファイルの内容を表示できるを制限するにはプログラムによる、ファイルを削除できるユーザーを制限する方法を使用します。

### <a name="using-the-loginview-control"></a>LoginView コントロールを使用します。

これまで見てきた過去のチュートリアルでと LoginView コントロールは、認証と匿名ユーザー用の別のインターフェイスを表示するのに便利ですが匿名ユーザーにアクセスできない機能を非表示にする簡単な方法を提供します。 のみを表示する必要があります匿名ユーザーは、表示するか、またはファイルを削除することはできませんから、`FileContents`認証済みユーザーによってアクセスすると、このページのテキスト ボックス。 これを実現する、LoginView コントロール、ページを追加、名前を付けます`LoginViewForFileContentsTextBox`に移動し、 `FileContents` LoginView コントロールのテキスト ボックスの宣言型マークアップ`LoggedInTemplate`です。

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

LoginView のテンプレートでの Web コントロールは、分離コード クラスから直接アクセスではなくなりました。 たとえば、 `FilesGrid` GridView の`SelectedIndexChanged`と`RowDeleting`イベント ハンドラーを現在参照、`FileContents`などのコードのある TextBox コントロール。

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

ただし、このコードは有効ではなくなりました。 移動することによって、 `FileContents`  テキスト ボックスに、`LoggedInTemplate`テキスト ボックスに直接アクセスできません。 代わりに、使用する必要があります、`FindControl("controlId")`コントロールをプログラムによって参照されるメソッド。 更新プログラム、`FilesGrid`イベント ハンドラー ボックスを参照する次のようにします。

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

LoginView をテキスト ボックスを移動した後`LoggedInTemplate`ページのコードを使用して、テキスト ボックスの参照を更新し、`FindControl("controlId")`パターン、匿名ユーザーとしてそのページを参照してください。 図 10 に示す、 `FileContents`  テキスト ボックスは表示されません。 ただし、ビューの LinkButton は引き続き表示されます。


[![LoginView コントロールを表示する、FileContents テキスト ボックスに認証されたユーザーのみ](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**図 10**:「LoginView コントロールのみがレンダリング、 `FileContents` Authenticated Users をテキスト ボックス ([フルサイズ イメージを表示するに、をクリックして](user-based-authorization-cs/_static/image30.png))


匿名ユーザーの表示 ボタンを非表示にする方法の 1 つは GridView フィールドを TemplateField に変換することです。 これにより、ビューの LinkButton の宣言型マークアップを含むテンプレートが生成されます。 TemplateField に LoginView コントロールを追加したり LoginView の内の LinkButton を配置できます`LoggedInTemplate`これにより、匿名の訪問者から [表示] ボタンを非表示にします。 これを実現するには、[フィールド] ダイアログ ボックスを起動する GridView のスマート タグからの列の編集リンクをクリックします。 次に、左下隅にある一覧から選択 ボタンを選択し、このフィールドを TemplateField リンクに変換 をクリックします。 これによりから、フィールドの宣言型マークアップを変更します。

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 移動先: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

この時点で、LoginView、TemplateField に追加できます。 次のマークアップでは、認証されたユーザーに対してのみ表示 LinkButton が表示されます。

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

図 11 に示す、最終的な結果はかなりのビューとして列がまだ表示されている場合でも、非表示には、列内でビューのあります。 見てみましょう全体の GridView 列 (と LinkButton だけでなく) を非表示にする方法、次のセクションでします。


[![LoginView コントロールは、匿名の訪問者のビューのあるを非表示に](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**図 11**: LoginView コントロールは、匿名の訪問者のビューのあるを非表示に ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>プログラムで機能を制限します。

状況によっては、宣言的な手法では、ページに機能を制限するための十分なです。 たとえば、特定のページの機能の可用性は、ページにアクセスしたユーザーは、匿名または認証されたかどうかを超える条件に依存する可能性があります。 このような場合は、さまざまなユーザー インターフェイス要素表示したり、プログラムによる方法で非表示になります。

機能をプログラムで制限するためには、2 つのタスクを実行する必要があります。

1. ページにアクセスしたユーザーが、機能にアクセスできるかどうかを判断し、
2. プログラムによってユーザーが対象の機能へのアクセスを持っているかどうかに基づくユーザー インターフェイスを変更します。

これら 2 つのタスクのアプリケーションを示すためには、説明のみを許可 Tito GridView からファイルを削除します。 最初のタスクは、次は Tito ページにアクセスであるかどうかを決定します。 決定されています (表示または非表示)、GridView の列を削除する必要があります。 GridView の列が経由でアクセスできる、`Columns`プロパティです。 列だけ表示する場合、`Visible`プロパティがに設定されている`true`(既定)。

次のコードを追加、 `Page_Load` GridView にデータをバインドする前にイベント ハンドラー。

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

説明したよう、 [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、 `User.Identity.Name` id の名前を返します。 これは、ログイン コントロールに入力されたユーザー名に対応します。 Tito ページにアクセス、GridView の 2 番目の列のかどうかは`Visible`プロパティに設定されている`true`、それ以外に設定されている`false`です。 最終的には、Tito 以外にアクセス ページで、別の認証されたユーザーまたは匿名ユーザーのいずれかと、列の削除はレンダリングされません (図 12 を参照)。ただし、ページにアクセスする Tito、列の削除が存在 (図 13 を参照してください)。


[![列の削除はされませんレンダリングされるときに訪問によって他のユーザー以外の Tito (ブルース) など](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**図 12**: 列の削除はされませんレンダリングされるときに訪問によって他のユーザー以外の Tito (ブルース) など ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image36.png))


[![Tito 用に描画される列の削除](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**図 13**: Tito に対して列の削除がレンダリング ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>手順 4: クラスとメソッドに承認規則を適用します。

手順 3 は、匿名ユーザーによるファイルの内容の表示を許可されていませんし、ファイルを削除してから、すべてのユーザーも Tito を禁止します。 宣言とプログラムの手法を承認されていない訪問者に関連するユーザー インターフェイス要素を非表示にすることで実現しました。 この単純な例では、については、単純なユーザー インターフェイス要素を適切に非表示がより複雑なサイトで同じ機能を実行するさまざまな方法である可能性がありますか。 未承認のユーザーにその機能を制限したり、非表示またはすべての該当するユーザー インターフェイス要素を無効にするを忘れる場合の対処方法

機能の特定の部分を未認証のユーザーがアクセスできないことを確認する簡単な方法は、そのクラスまたはメソッドを装飾するが、 [ `PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)です。 .NET ランタイム クラスを使用して、そのメソッドの 1 つを実行時に現在のセキュリティ コンテキストが、クラスを使用するか、メソッドを実行する権限を持っていることを確認することを確認します。 `PrincipalPermission`属性は、これらの規則を定義できる機構を提供します。

使用する例を見てみましょう、 `PrincipalPermission` GridView の属性`SelectedIndexChanged`と`RowDeleting`それぞれ匿名ユーザーと Tito、以外のユーザーが実行を禁止するイベント ハンドラー。 必要なことを行うには、各関数の定義の上に適切な属性を追加です。

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

属性、`SelectedIndexChanged`の属性として、イベントのハンドラーが決定され、認証されたユーザーだけが、イベント ハンドラーを実行できる、`RowDeleting`イベント ハンドラーが Tito に実行を制限します。

場合、Tito は別のユーザーが実行しようとした何らかの理由で、`RowDeleting`イベント ハンドラーまたは認証されていないユーザーが実行を試みる、 `SelectedIndexChanged` .NET ランタイムで発生するイベント ハンドラー、`SecurityException`です。


[![SecurityException がスローされるセキュリティ コンテキストが、メソッドを実行する権限がない場合](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**図 14**: セキュリティ コンテキストが、メソッドを実行する権限がない場合、`SecurityException`がスローされます ([フルサイズのイメージを表示するをクリックして](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> 複数のセキュリティ コンテキストがクラスまたはメソッドにアクセスできるように、クラスまたはメソッドを装飾できるは`PrincipalPermission`の各セキュリティ コンテキストの属性です。 つまり、Tito およびブルースの両方が実行を許可する、 `RowDeleting` 、イベント ハンドラーを追加*2* `PrincipalPermission`属性。


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

ASP.NET ページだけでなく多くのアプリケーションでは、ビジネス ロジックとデータ アクセス レイヤーなど、さまざまな層が含まれています、アーキテクチャもが存在します。 これらのレイヤーでは、一般に、クラス ライブラリとして実装し、クラスとビジネス ロジックとデータに関連する機能を実行するためのメソッドを提供します。 `PrincipalPermission`属性はこれらのレイヤーに承認規則を適用するために役立ちます。

使用する方法について、`PrincipalPermission`属性をクラスやメソッドで承認規則を定義しを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ[ビジネス レイヤーを使用してデータして承認規則を追加します。`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>まとめ

このチュートリアルでは、ユーザー ベースの承認規則を適用する方法を説明しました。 ASP を見てから始まります。NET の URL の承認フレームワークです。 要求ごとに、ASP.NET エンジンの`UrlAuthorizationModule`id が要求されたリソースにアクセスを承認されているかどうかを決定する、アプリケーションの構成で定義されている URL 承認規則を検査します。 つまり、URL 承認簡単に特定のディレクトリ内のすべてのページまたは特定のページの承認規則を指定できます。

URL の承認フレームワークでは、ページ単位ごとに承認規則が適用されます。 URL 承認では、要求元のいずれかの id はか特定のリソースにアクセスする権限します。 多くのシナリオでは、ただし、さらにきめ細かく承認ルールの呼び出します。 ページへのアクセスが許可されるユーザーを定義するのではなく、everyone のアクセス、ページが別のデータを表示するか、ページにアクセスしたユーザーによって異なる機能を提供することがあります。 ページ レベルの承認には、通常、承認されていないユーザーが禁止されている機能にアクセスすることを防ぐために特定のユーザー インターフェイス要素を非表示が含まれます。 さらに、クラスと特定のユーザーに対してそのメソッドの実行にアクセスを制限する属性を使用することができます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ビジネスおよびデータ層を使用する承認規則の追加`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET の承認](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Iis 6 と iis 7 のセキュリティの変更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [特定のファイルおよびサブディレクトリを構成します。](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [ユーザーに基づくデータ変更機能を制限します。](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [LoginView コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [IIS7 URL 承認を理解します。](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`テクニカル ドキュメント](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [ASP.NET 2.0 のデータの操作](../../data-access/index.md)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

>[!div class="step-by-step"]
[前へ](validating-user-credentials-against-the-membership-user-store-cs.md)
[次へ](storing-additional-user-information-cs.md)
