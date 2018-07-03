---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: ユーザー ベースの承認 (VB) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限することに注目します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: c23e67d1a32ffb5115c76bee25ed8b82f44ae8d3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376363"
---
<a name="user-based-authorization-vb"></a>ユーザー ベースの承認 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限することに注目します。


## <a name="introduction"></a>はじめに

ユーザー アカウントを提供するほとんどの web アプリケーションでこの特定の訪問者から、サイト内の特定のページへのアクセスを制限するようにします。 たとえば、大半のオンライン messageboard サイト、- 匿名、認証済みのすべてのユーザーは、messageboard の投稿を表示することが認証されたユーザーだけが新しい投稿を作成する web ページを参照してください。 特定のユーザー (または特定のユーザーのセット) にアクセスできる管理ページがある可能性があります。 さらに、ページ レベルの機能は、ユーザー単位のユーザーごとに異なることができます。 投稿のリストを表示するときに認証されたユーザーはこのインターフェイスは匿名の訪問者を利用できませんが、その個々 の投稿を評価するためのインターフェイスに表示されます。

ASP.NET では、簡単にユーザー ベースの承認規則を定義できます。 内のマークアップのわずか`Web.config`、指定したユーザーのサブセットにアクセスできるだけように、特定の web ページまたはディレクトリ全体をダウン ロックできなかったことができます。 ページ レベルの機能にできますオンまたはオフ プログラムおよび宣言型の方法で、現在ログインしているユーザーに基づいて。

このチュートリアルでは、ページへのアクセスを制限して、さまざまな手法によってページ レベルの機能を制限することに注目します。 それでは、始めましょう!

## <a name="a-look-at-the-url-authorization-workflow"></a>URL の承認ワークフローを参照してください。

説明したように、 [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)チュートリアルでは、ASP.NET リソース要求ライフ サイクル中に多数のイベントを発生させますの ASP.NET ランタイムが要求を処理するときにします。 *HTTP モジュール*マネージ クラスの要求のライフ サイクルの特定のイベントに応答であるコードが実行されます。 ASP.NET では、バック グラウンドで重要なタスクを実行する HTTP モジュールの数が付属しています。

このような 1 つの HTTP モジュールが[ `FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)します。 主な機能の前のチュートリアルで説明したように、`FormsAuthenticationModule`は現在の要求の id を確認します。 これは、フォーム認証チケットがクッキーにまたは、URL 内に埋め込まれているを調べることによって実現されます。 この id は、実行中、 [ `AuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)します。

もう 1 つの重要な HTTP モジュールは、 [ `UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)への応答で発生する、 [ `AuthorizeRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)(後の動作を`AuthenticateRequest`イベント)。 `UrlAuthorizationModule`内の構成マークアップを調べて`Web.config`を現在の id が指定ページにアクセスする権限を持つかどうかを判断します。 このプロセスと呼ばれます*URL 承認*します。

について説明します、手順 1. で URL 承認規則の構文がまずみましょう見てどのような`UrlAuthorizationModule`は、要求が承認されているかどうかどうかによって異なります。 場合、`UrlAuthorizationModule`こと要求が承認されるは、何もとライフ サイクル全体は引き続き、要求を決定します。 ただし、要求の場合*いない*、承認、`UrlAuthorizationModule`ライフ サイクルを中止し、指示、`Response`オブジェクトを返す、 [HTTP 401](http://www.checkupdown.com/status/E401.html)状態。 この HTTP 401 ステータスは、クライアントに返されませんフォーム認証を使用するときに場合、`FormsAuthenticationModule`状態は、HTTP 401 に変更を検出、 [HTTP 302 リダイレクト](http://www.checkupdown.com/status/E302.html)ログイン ページにします。

図 1 は、ASP.NET パイプラインのワークフローを示しています、 `FormsAuthenticationModule`、および`UrlAuthorizationModule`未承認の要求が到着したとき。 具体的には、図 1 はの匿名の訪問者によって要求を示しています。 `ProtectedPage.aspx`、匿名ユーザーにアクセスを拒否する ページであります。 訪問者は、匿名であるため、`UrlAuthorizationModule`要求を中止し、HTTP 401 Unauthorized ステータスを返します。 `FormsAuthenticationModule`ログイン ページへの 302 リダイレクトに 401 ステータスを変換します。 [ログイン] ページで、ユーザーが認証された後、彼にリダイレクトされます。`ProtectedPage.aspx`します。 この時間、`FormsAuthenticationModule`彼認証チケットに基づくユーザーを識別します。 ゲスト ユーザーが認証されると、これで、`UrlAuthorizationModule`ページへのアクセスを許可します。


[![フォーム認証と承認ワークフローの URL](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**図 1**: [フォーム認証と承認ワークフローの URL ([フルサイズの画像を表示する] をクリックします](user-based-authorization-vb/_static/image3.png))。


図 1 は、匿名の訪問者がいない匿名ユーザーに提供されるリソースにアクセスしようとしたときに発生する相互作用を示しています。 このような場合は、匿名の訪問者は、クエリ文字列で指定されているアクセスしようと彼女のページで、ログイン ページにリダイレクトされます。 ログオンすると、ユーザーが正常に彼女は自動的にリダイレクトされます彼女が最初に表示する試行しているリソースに戻ります。

このワークフローが単純で、訪問者が変更点を理解するが簡単に匿名ユーザーによって承認されていない要求が行われる理由です。 注意で維持しながら、`FormsAuthenticationModule`はリダイレクト*任意*要求が認証されたユーザーによって行われた場合でも、ログイン ページにユーザー権限がありません。 これは、結果、機関を彼女がないページにアクセスしようとすると、認証されたユーザーに混乱を招くユーザー エクスペリエンス。

Imagine の web サイトが構成された、URL 承認規則を持っているように、ASP.NET ページ`OnlyTito.aspx`Tito にのみアクセスできます。 が。 ここで、Sam にサイトを訪問し、ログオンして、アクセスしようとし、ことを想像`OnlyTito.aspx`します。 `UrlAuthorizationModule`は、要求のライフ サイクルを停止し、HTTP 401 Unauthorized ステータスを返します`FormsAuthenticationModule`を検出し、Sam をログイン ページにリダイレクトします。 Sam がまだログインしているため、彼女疑問に思う理由彼女が送信されたログイン ページに戻ります。 彼女は、自分のログイン資格情報が何らかの形で失われたか、無効な資格情報を入力したことを理由可能性があります。 Sam は、ログイン ページから自分の資格情報を再入力する場合は (再度) ログオンされにリダイレクト`OnlyTito.aspx`します。 `UrlAuthorizationModule` Sam がこのページにアクセスできないし、彼女がログイン ページに返されますことを検出します。

図 2 は、この混乱を招くのワークフローを示しています。


[![既定のワークフローは混乱を招くサイクルにつながる](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**図 2**: の既定のワークフローにつながる混乱を招くサイクル ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image6.png))。


図 2 に示すワークフローは、でも、ほとんどのコンピューター経験豊富な訪問者をすばやく befuddle ことができます。 手順 2. でサイクルが混乱これを回避する方法を紹介します。

> [!NOTE]
> ASP.NET では、2 つのメカニズムを使用して、現在のユーザーが特定の web ページにアクセスできるかどうかを決定する: URL 承認とファイルの承認。 ファイルの承認はによって実装されて、 [ `FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)機関が要求されたファイルの Acl を参照して決定します。 ファイルの承認は、Acl が Windows アカウントに適用されるアクセス許可のために最もよく Windows 認証で使用されます。 フォーム認証を使用する場合は、すべてのオペレーティング システムとファイル システム レベルの要求がサイトにアクセスするユーザーに関係なく、同じ Windows アカウントによって実行されます。 このチュートリアル シリーズでは、フォーム認証に重点を置いていますため、私たちはについては説明しませんファイル承認します。


### <a name="the-scope-of-url-authorization"></a>URL 承認の範囲

`UrlAuthorizationModule`はマネージ コードは、ASP.NET ランタイムの一部であります。 前のバージョン 7 のマイクロソフトの[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) web サーバー、IIS の HTTP パイプラインと、ASP.NET ランタイムのパイプラインの個別障害が発生しました。 簡単に言えば、IIS 6 以降では、ASP します。NET の`UrlAuthorizationModule`ASP.NET ランタイムに要求が IIS から委任されるときにのみ実行されます。 既定では、IIS などの静的コンテンツ自体の HTML ページと CSS、JavaScript、およびイメージ ファイルの処理し、ASP.NET ランタイムの拡張子を持つページへの要求のみを渡します`.aspx`、 `.asmx`、または`.ashx`を要求します。

IIS 7 では、ただしでは、統合された IIS と ASP.NET パイプライン。 呼び出す、IIS 7 を設定するいくつかの構成設定で、`UrlAuthorizationModule`の*すべて*要求、任意の種類のファイルの URL 承認規則を定義できることを意味します。 さらに、IIS 7 には、独自の URL 承認エンジンが含まれています。 ASP.NET の統合と IIS 7 のネイティブ URL 承認の機能の詳細については、次を参照してください。[理解 IIS7 URL 承認](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)します。 ASP.NET と IIS 7 の統合の詳細については、集荷 Shahram Khosravi のマニュアルのコピーを*Professional IIS 7 と ASP.NET の統合プログラミング*(ISBN: 978 0470152539)。

簡単に言うと、IIS 7 よりも前のバージョンでは、URL 承認規則のみに適用されます、ASP.NET ランタイムによって処理されるリソース。 IIS 7 と IIS のネイティブ URL 認証機能を使用する、または ASP を統合することはできます。NET の`UrlAuthorizationModule`IIS の HTTP パイプラインにすべての要求には、この機能の拡張によって実現します。

> [!NOTE]
> 方法のいくつかの微妙ながらも重要な違いがある ASP します。NET の`UrlAuthorizationModule`と IIS 7 の URL 承認の機能が、承認規則を処理します。 このチュートリアルでは、IIS 7 の URL 承認の機能またはと比較すると、承認規則を解析する方法の違いはチェックしません、`UrlAuthorizationModule`します。 これらのトピックの詳細については、IIS 7 のマニュアルまたは msdn を参照してください[www.iis.net](https://www.iis.net/)します。


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>手順 1: URL 承認規則を定義します。`Web.config`

`UrlAuthorizationModule`アプリケーションの構成で定義されている URL 承認規則に基づいて特定の id の要求のリソースへのアクセス許可または拒否するかどうかを決定します。 承認規則が記述された、 [ `<authorization>`要素](https://msdn.microsoft.com/library/8d82143t.aspx)の形式で`<allow>`と`<deny>`子要素。 各`<allow>`と`<deny>`子要素を指定できます。

- 特定のユーザー
- ユーザーのコンマ区切りの一覧
- 疑問符 (?) で表される、すべての匿名ユーザー
- アスタリスクで示される、すべてのユーザー (\*)

次のマークアップは、URL の承認規則を使用してユーザー Tito と Scott に許可して、他のすべてを拒否する方法を示しています。

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

`<allow>` - Tito と Scott - どのようなユーザーが許可されている要素を定義中に、`<deny>`要素に指示する*すべて*ユーザーが拒否されます。

> [!NOTE]
> `<allow>`と`<deny>`要素は、ロールの承認規則も指定できます。 今後のチュートリアルでのロールベースの承認を見ていきます。


次の設定は、Sam (匿名の訪問者を含む) 以外のユーザーにアクセスを許可します。

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

認証されたユーザーのみを許可するのには、すべての匿名ユーザーにアクセスを拒否します。 次の構成を使用します。

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

内で承認規則が定義されている、`<system.web>`要素`Web.config`し、すべての web アプリケーションで ASP.NET のリソースに適用します。 多くの場合、アプリケーションでは、さまざまなセクションのさまざまな承認規則があります。 たとえば、e コマース サイトでは、すべての訪問者可能性があります製品をご参照、製品のレビューを表示、カタログを検索および具合します。 ただし、認証されたユーザーのみは、チェック アウト、または 1 つの配布の履歴を管理するページにアクセスできます。 さらに、サイト管理者など、ユーザーの選択 でアクセスできるサイトの部分があります。

ASP.NET では、簡単に、サイト内の別のファイルとフォルダーのさまざまな承認規則を定義できます。 ルート フォルダーの指定された承認規則`Web.config`ファイルは、サイト内のすべての ASP.NET リソースに適用されます。 ただし、これらの既定の承認設定の上書きできます特定のフォルダーを追加して、`Web.config`で、`<authorization>`セクション。

認証されたユーザーのみが内の ASP.NET ページにアクセスできるように、当社の web サイトを更新しましょう、`Membership`フォルダー。 追加する必要があります。 これを実現する、`Web.config`ファイルを、`Membership`フォルダーを匿名ユーザーを拒否するには、その承認設定を設定します。 右クリックし、`Membership`ソリューション エクスプ ローラーでフォルダーが、コンテキスト メニューから、[新しい項目の追加] メニューを選択し、という名前の新しい Web 構成ファイルを追加`Web.config`します。


[![メンバーシップのフォルダーに Web.config ファイルを追加します。](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**図 3**: 追加、`Web.config`ファイルを`Membership`フォルダー ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image9.png))。


この時点で、プロジェクトを含む必要があります 2`Web.config`ファイル: 1 つで 1 つのルート ディレクトリで、`Membership`フォルダー。


[![アプリケーションは、2 つの Web.config ファイルを含める必要がありますようになりました](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**図 4**: Your アプリケーションする必要がありますが含まれてという 2 つ`Web.config`ファイル ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image12.png))。


構成ファイルを更新、`Membership`フォルダーを匿名ユーザーへのアクセスを禁止するようにします。

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

GridView を ObjectDataSource にバインドされているユーザーのブラウザーでページ上の空白の領域でどのマークアップもレンダリングしません。

この変更をテストするにブラウザーでホーム ページを参照してください。 し、ログアウトするかどうかを確認します。ASP.NET アプリケーションの既定の動作は、すべての訪問者を許可するため、承認を変更したり、ルート ディレクトリの作成しなかったため`Web.config`ファイル、匿名の訪問者としてのルート ディレクトリ内のファイルにアクセスすることができます。

左側の列にあるユーザー アカウントを作成するリンクをクリックします。 これは、移動、`~/Membership/CreatingUserAccounts.aspx`します。 以降、`Web.config`ファイル、`Membership`フォルダーへの匿名アクセスを禁止するための承認規則を定義する、`UrlAuthorizationModule`要求を中止し、HTTP 401 Unauthorized ステータスを返します。 `FormsAuthenticationModule`ログイン ページへの送信、302 リダイレクト ステータスにこれを変更します。 ページ試行していたへのアクセスに注意してください (`CreatingUserAccounts.aspx`) 経由でログイン ページに渡される、`ReturnUrl`クエリ文字列パラメーター。


[![以降、URL 承認規則匿名アクセスを禁止する、私たちがログイン ページにリダイレクトされます。](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**図 5**: URL 承認規則匿名アクセスを禁止するため、私たちは、ログイン ページにリダイレクトされます ([フルサイズの画像を表示する をクリックします。](user-based-authorization-vb/_static/image15.png))。


ログインするにリダイレクトしましたが、`CreatingUserAccounts.aspx`ページ。 この時間、`UrlAuthorizationModule`匿名は不要になったために、ページへのアクセスを許可します。

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>特定の場所に URL 承認規則を適用します。

承認の設定で定義されている、`<system.web>`の`Web.config`そのディレクトリとそのサブディレクトリの ASP.NET リソースのすべてに適用されます (それ以外の場合別でオーバーライドされるまで`Web.config`ファイル)。 場合によっては、ただし、必要があります、1 つまたは 2 つの特定のページを除く特定の承認設定に指定したディレクトリ内のすべての ASP.NET リソース。 これは、追加することで実現できます、`<location>`要素`Web.config`の承認規則が異なる場合、ファイルを指す、および、その中に、一意の承認規則を定義します。

使用して説明するために、`<location>`要素を特定のリソースの構成設定を上書き、Tito のみがアクセスできるように承認設定をカスタマイズしましょう`CreatingUserAccounts.aspx`します。 これを行うには、追加、`<location>`要素を`Membership`フォルダーの`Web.config`ファイルを開き、次のように見えるように、そのマークアップを更新します。

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

`<authorization>`要素`<system.web>`内の ASP.NET リソースの既定の URL 承認規則を定義、`Membership`フォルダーとそのサブフォルダーです。 `<location>`要素では、特定のリソースに対してこれらのルールを上書きできます。 上記のマークアップで、`<location>`要素の参照、`CreatingUserAccounts.aspx`ページし、その承認規則の許可 Tito などを指定します。 拒否がその他のユーザー。

この承認の変更をテストするには、匿名ユーザーとして web サイトにアクセスして開始します。 任意のページにアクセスしようとすると、`Membership`フォルダーなど`UserBasedAuthorization.aspx`、`UrlAuthorizationModule`要求は拒否し、ログイン ページにリダイレクトされます。 答えると、Scott、としてログインした後に任意のページにアクセスできる、`Membership`フォルダー*を除く*の`CreatingUserAccounts.aspx`します。 アクセスしようとしています。`CreatingUserAccounts.aspx`すべてのユーザーとしてログオンが Tito れます、承認されていないアクセス試行を、ログイン ページにリダイレクトします。

> [!NOTE]
> `<location>`要素は、構成の外側に記述する必要があります`<system.web>`要素。 個別を使用する必要がある`<location>`各リソースの承認設定をオーバーライドする要素。


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>確認する方法、`UrlAuthorizationModule`承認規則を使用してアクセス許可または拒否

`UrlAuthorizationModule`かどうか、特定の URL の URL 承認を分析することで、特定のユーザーを承認するためにルールを 1 つ目からダウンの方法を操作して、一度に 1 つずつを決定します。 場合によっては、一致が見つかったで、一致が見つかるとすぐに、ユーザーがアクセス許可または拒否する`<allow>`または`<deny>`要素。 <strong>一致が検出されない場合、ユーザーがアクセスを許可します。</strong> その結果、アクセスを制限する場合を使用することが不可欠な`<deny>`URL 承認の構成の最後の要素としての要素。 <strong>省略した場合、</strong><strong>`<deny>`</strong><strong>要素では、すべてのユーザー アクセスが許可されます。</strong>

使用されるプロセスを理解する、`UrlAuthorizationModule`機関を確認する例を考えてみます URL 承認規則がこの手順の前半で説明しました。 最初のルールは、 `<allow>` Tito と Scott へのアクセスを許可する要素。 2 番目のルールは、`<deny>`要素をすべてのユーザーがアクセスを拒否します。 匿名ユーザーがアクセスする場合、`UrlAuthorizationModule`か、開始が匿名 Scott または Tito でしょうか。 いいえ、そのため、2 番目のルールに進みますが答え、当然ながら、です。 すべてのユーザーのセットでは匿名ですか。 以降、答えはい、ここでは、`<deny>`ルールが有効な配置し、訪問者は、ログイン ページにリダイレクトされます。 同様に、Jisun を訪問すると場合、 `UrlAuthorizationModule` Jisun が要求することによって開始 Scott または Tito のいずれかでしょうか。 そうでないので、 `UrlAuthorizationModule` Jisun ですべてのユーザーのセットには、2 番目の質問に進みますか? 彼女は、アクセスが拒否されます。 最後に、Tito 場合、最初の質問はによってもたらされる、`UrlAuthorizationModule`肯定の応答は Tito アクセスが許可されます。

以降、`UrlAuthorizationModule`プロセスの承認規則のダウン、停止してから、すべての一致に上からより特定の規則を特定性の低いものより前に設定することが重要です。 つまり、Jisun と、匿名ユーザーが禁止されているが、他のすべての認証されたユーザーを許可する承認規則を定義するにはの先頭に最も固有のルールの 1 つに影響を与える Jisun - してから他のすべての許可 - 特定性の低いルールに進みます認証されたユーザーがすべての匿名ユーザーを拒否します。 次の URL 承認規則は、まず Jisun、拒否して、すべての匿名ユーザーを拒否して、このポリシーを実装します。 Jisun 以外のユーザーは認証済みアクセスが許可されますので、どちらも`<deny>`ステートメントは一致します。

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>手順 2: 承認されていない、認証されたユーザーのワークフローの修正

URL 承認ワークフローのセクションで、検索では、このチュートリアルで先ほど説明したようにいつでも未承認の要求には、`UrlAuthorizationModule`要求を中止し、HTTP 401 Unauthorized ステータスを返します。 によってこの 401 ステータスが変更された、 `FormsAuthenticationModule` 302 にリダイレクトするユーザーをログイン ページに送信する状態。 このワークフローは、ユーザーが認証される場合でも、未承認の要求で発生します。

認証されたユーザーをログイン ページに返すことが既にシステムにログインしているために、それらを混同する可能性があります。 少し作業が、認証済み要求するユーザーを承認されていない制限付きのページにアクセスしようとしたことを説明するページにリダイレクトすることによって、このワークフローを向上します。

という名前の web アプリケーションのルート フォルダーで新しい ASP.NET ページを作成して開始`UnauthorizedAccess.aspx`; が、このページを関連付けることを忘れないでください、`Site.master`マスター ページ。 このページを作成した後を参照するコンテンツ コントロールを削除、`LoginContent`プレース ホルダーをマスター ページの既定のコンテンツが表示されます。 つまり、状況を説明するメッセージを次に、追加、ユーザーが保護されたリソースにアクセスしようとしたことです。 このようなメッセージを追加した後、`UnauthorizedAccess.aspx`ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

これで、認証されたユーザーが未承認の要求を実行する場合に送信されるように、ワークフローを変更する必要があります、`UnauthorizedAccess.aspx`ログイン ページではなくページ。 プライベート メソッド内でログイン ページに承認されていない要求をリダイレクトするロジックが埋め込まれています、`FormsAuthenticationModule`クラスのため、この動作をカスタマイズすることはできません。 ユーザーをリダイレクトするログイン ページに独自のロジックを追加するには何を行える、ただし、`UnauthorizedAccess.aspx`必要な場合、します。

ときに、`FormsAuthenticationModule`未承認のユーザーをリダイレクトするログイン ページに追加、名前を持つクエリ文字列の要求、承認されていない URL `ReturnUrl`。 たとえば、未認証のユーザーにアクセスしようとすると`OnlyTito.aspx`、`FormsAuthenticationModule`がリダイレクトするに`Login.aspx?ReturnUrl=OnlyTito.aspx`します。 そのため、認証されたユーザーを含むクエリ文字列でログイン ページに達した場合、`ReturnUrl`パラメーターを紹介しているだけこの認証されていないユーザーを表示する権限がありません彼女のページにアクセスしようとします。 このような場合は、彼女にリダイレクトする`UnauthorizedAccess.aspx`します。

これを実現するには、ログイン ページの次のコードを追加`Page_Load`イベント ハンドラー。

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

上記のコードに認証、承認されていないユーザーをリダイレクトする、`UnauthorizedAccess.aspx`ページ。 このロジックの動作を表示するには、匿名の訪問者としてサイトにアクセスし、左側にあるユーザー アカウントを作成するリンクをクリックします。 これは、移動、 `~/Membership/CreatingUserAccounts.aspx`  ページで、これだけで構成した手順 1. で Tito へのアクセス許可。 匿名ユーザーは禁止されていますので、`FormsAuthenticationModule`私たちは、ログイン ページにリダイレクトします。

この時点では匿名ため、`Request.IsAuthenticated`返します`False`にリダイレクトされないことと`UnauthorizedAccess.aspx`します。 代わりに、ログイン ページが表示されます。 Tito、Bruce など別のユーザーとしてログインします。 ログイン ページに戻りリダイレクトを適切な資格情報を入力すると、`~/Membership/CreatingUserAccounts.aspx`します。 ただし、このページは Tito にアクセスできるだけであるためそれを表示する権限がありませんが、ログイン ページにすぐに返されます。 ただし、この時間`Request.IsAuthenticated`返します`True`(および`ReturnUrl`クエリ文字列パラメーターが存在する) にリダイレクトしましたので、`UnauthorizedAccess.aspx`ページ。


[![認証、承認されていないユーザーは UnauthorizedAccess.aspx にリダイレクトされます。](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**図 6**: 承認されていないユーザーは、認証にリダイレクトされます`UnauthorizedAccess.aspx`([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image18.png))。


このカスタマイズされたワークフローでは、図 2 に示すように、サイクルのショート サーキットより賢明かつ簡単なユーザー エクスペリエンスを表示します。

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>手順 3: 現在ログインしているユーザーに基づく機能を制限します。

URL 承認を簡単に粗い承認規則を指定します。 手順 1. で説明したように URL 承認簡潔に記述できましたどの id が許可されており、どれがフォルダー内の特定のページまたはすべてのページの表示が拒否されます。 特定のシナリオでただし、することも、ページを参照してください。、訪問するユーザーを基に、ページの機能を制限するためのすべてのユーザーを許可します。

自社製品を確認する認証済みの訪問者は、電子商取引 web サイトの場合を考えます。 匿名ユーザーが製品のページを訪問したときは、製品情報だけを参照してレビューを残すことがない付与されます。 ただし、同じページにアクセスして、認証されたユーザーには確認インターフェイス参照してください。 認証されたユーザーは、この製品をまだ確認されませんが場合、インターフェイスが有効にする; レビューを送信するにはそれ以外の場合は表示することに送信したレビュー。 このシナリオの手順を実行するには、さらに、製品ページ可能性があります追加情報を表示し、e コマース企業に最適なそれらのユーザーの拡張機能を提供します。 たとえば、製品ページは、在庫のインベントリを一覧表示し、製品の価格と従業員がアクセスしたときの説明を編集するオプションが含まれます。

宣言的またはプログラム (または、2 つの組み合わせを通じて)、このような細かい粒度の承認規則を実装できます。 次のセクションは、LoginView コントロールを使用して細かく承認を実装する方法が表示されます。 次は、プログラムによる手法について説明します。 細かく承認規則を適用することについて考えることができます、前に、まず必要があります機能を持つは、それにアクセスしたユーザーによって異なります。 ページを作成します。

GridView を内の特定のディレクトリ内のファイルを一覧表示されたページを作成しましょう。 GridView と共に一覧表示するには、各ファイルの名前、サイズ、およびその他の情報、Linkbutton の 2 つの列が含まれます。 1 つは、ビューと 1 つのタイトルの削除」というタイトル。 ビューの LinkButton をクリックすると、選択したファイルの内容が表示されます。削除 LinkButton をクリックすると、ファイルは削除されます。 そのビューと削除機能がすべてのユーザーが使用できるように、このページを最初に作成してみましょう。 Using LoginView コントロールと機能をプログラムで制限するセクションでは、有効または、ページにアクセスしているユーザーに基づくこれらの機能を無効にする方法を説明します。

> [!NOTE]
> 構築するためには ASP.NET ページでは、ファイルの一覧を表示するのに GridView コントロールを使用します。 このチュートリアル シリーズは、フォーム認証、承認、ユーザー アカウント、およびロールに重点を置いています、以降しない GridView コントロールの内部動作を説明する時間をかけるにします。 このチュートリアルでは、このページをセットアップするための手順では、中の特定の選択を行った理由または影響の特定のプロパティが表示される出力が詳細には掘り下げてされません。 GridView コントロールのていねいに解説を参照してください、  *[ASP.NET 2.0 でデータを扱う](../../data-access/index.md)* チュートリアル シリーズです。


開いて開始、`UserBasedAuthorization.aspx`ファイル、`Membership`フォルダーとという名前のページに GridView コントロールを追加する`FilesGrid`します。 GridView のスマート タグから、[フィールド] ダイアログ ボックスを起動する列の編集リンクをクリックします。 ここでは、左下隅の 自動生成フィールド チェック ボックスをオフにします。 次に、(Select および Delete ボタン [commandfield] の種類ができます) 左上隅から [選択] ボタン、[削除] ボタン、および 2 つの BoundFields を追加します。 [選択] ボタンを設定`SelectText`プロパティを表示し、最初の BoundField の`HeaderText`と`DataField`プロパティ名にします。 設定の 2 つ目の BoundField の`HeaderText`プロパティ サイズ (バイト単位) をその`DataField`プロパティの長さをその`DataFormatString`プロパティを{0:N0}とその`HtmlEncode`プロパティを False にします。

GridView の列を構成した後、[フィールド] ダイアログ ボックスを閉じるには、[ok] をクリックします。 プロパティ ウィンドウで、設定、GridView の`DataKeyNames`プロパティを`FullName`します。 この時点で、GridView の宣言型マークアップは、次のようになります。

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

GridView のマークアップを作成、特定のディレクトリ内のファイルを取得し、それらを GridView にバインドするコードを記述する準備ができました。 ページの次のコードを追加`Page_Load`イベント ハンドラー。

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

上記のコードを使用して、 [ `DirectoryInfo`クラス](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx)アプリケーションのルート フォルダー内のファイルの一覧を取得します。 [ `GetFiles()`メソッド](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx)のすべてのファイルをディレクトリ内の配列として返します[`FileInfo`オブジェクト](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)、GridView にバインドされます。 `FileInfo`オブジェクトには、プロパティのさまざまななど`Name`、 `Length`、および`IsReadOnly`、他のユーザーの間で。 GridView がだけ表示されます、宣言型マークアップからわかるように、`Name`と`Length`プロパティ。

> [!NOTE]
> `DirectoryInfo`と`FileInfo`クラスは、 [ `System.IO`名前空間](https://msdn.microsoft.com/library/system.io.aspx)します。 そのため、するには、名前空間のクラス ファイルにインポートするこれらのクラス名、名前空間の名前を付ける (を使用して`Imports System.IO`)。


ご協力をブラウザーからこのページを参照してください。 アプリケーションのルート ディレクトリに存在するファイルの一覧が表示されます。 表示または削除の Linkbutton をクリックすると、ポストバックを発生させるが、まだため、アクションを実行せず、必要なイベント ハンドラーを作成します。


[![GridView には、Web アプリケーションのルート ディレクトリ内のファイルが一覧表示されます。](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**図 7**: GridView には、Web アプリケーションのルート ディレクトリ内のファイルが一覧表示されます ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image21.png))。


選択したファイルの内容を表示するための手段が必要です。 Visual Studio に戻り、という名前のテキスト ボックスを追加して`FileContents`GridView 上。 設定の`TextMode`プロパティを`MultiLine`とその`Columns`と`Rows`プロパティを 95% や 10 では、それぞれします。

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

GridView のイベント ハンドラーを次に、作成[`SelectedIndexChanged`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx)し、次のコードを追加します。

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

このコードは、GridView の`SelectedValue`プロパティを選択したファイルの完全なファイル名を確認します。 内部的には、`DataKeys`コレクションが取得するために参照されている、 `SelectedValue`GridView のように設定することが重要ですので、`DataKeyNames`プロパティを名前、この手順で前述したようにします。 [ `File`クラス](https://msdn.microsoft.com/library/system.io.file.aspx)に割り当てられるし、文字列に、選択したファイルの内容の読み取りに使用、`FileContents`テキスト ボックスの`Text`プロパティ ページで選択したファイルの内容が表示されます。


[![選択されているファイルの内容は、テキスト ボックスに表示されます。](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**図 8**: 選択したファイルの内容がテキスト ボックスに表示されます ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image24.png))。


> [!NOTE]
> HTML マークアップを含むファイルの内容を表示し、表示またはファイルを削除しようとしていてが表示されます、`HttpRequestValidationException`エラー。 これは、web サーバーにポストバック時に、テキスト ボックスの内容が送信されるために発生します。 既定では、ASP.NET は、 `HttpRequestValidationException` HTML マークアップなどの危険性のあるポストバック コンテンツが検出されるたびにエラー。 追加することで、ページの要求の検証をオフにするこのエラーの発生を無効にする`ValidateRequest="false"`を`@Page`ディレクティブ。 要求の検証としてどのような予防措置としてもときに実行する必要がありますの利点の詳細について無効にすることをお読みください[要求の検証 - スクリプト攻撃の防止](https://asp.net/learn/whitepapers/request-validation/)します。


最後に、次のコードでイベント ハンドラーを追加の GridView の[`RowDeleting`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

コードだけで削除するファイルの完全名を表示、 `FileContents` TextBox*せず*ファイルを実際に削除します。


[![削除ボタンをクリックしても、そのファイルが実際に削除はされません。](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**図 9**: ファイルをクリックする、削除ボタンも実際には削除されません ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image27.png))。


手順 1. で匿名ユーザーが内のページを表示することを禁止する URL 承認規則を構成した、`Membership`フォルダー。 細かく認証が発生する、するには匿名ユーザーのアクセスを許可、 `UserBasedAuthorization.aspx`  ページで、機能が制限されたが、します。 次のコードを追加するすべてのユーザーがアクセスするこのページを開き、`<location>`要素を`Web.config`ファイル、`Membership`フォルダー。

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

これを追加した後`<location>`要素、サイトからログアウトして、新しい URL 承認規則をテストします。 匿名ユーザーとしてアクセスする許可必要があります、`UserBasedAuthorization.aspx`ページ。

現時点では、任意の認証済みまたは匿名ユーザーがアクセスできる、`UserBasedAuthorization.aspx`ページを表示し、またはファイルを削除します。 しましょう認証されたユーザーのみがファイルの内容を表示して Tito のみがファイルを削除できるようにします。 宣言によって、プログラムから、または両方のメソッドの組み合わせによって、このような細かい粒度の承認規則を適用できます。 ファイルの内容を表示することができますを制限するのに宣言型のアプローチを使用しましょうファイルを削除できるユーザーを制限するプログラムによるアプローチを使用します。

### <a name="using-the-loginview-control"></a>LoginView コントロールを使用します。

過去のチュートリアルでご覧いただいたし、LoginView コントロールは、認証と匿名ユーザーの異なるインターフェイスを表示するために便利ですが匿名ユーザーにアクセスできない機能を非表示にする簡単な方法を提供します。 のみを表示する必要があります匿名ユーザーは、表示するか、またはファイルを削除することはできませんから、`FileContents`テキスト ボックスに、ページ、認証されたユーザーがアクセスするとします。 これを実現する、ページを LoginView コントロールを追加、名前を付けます`LoginViewForFileContentsTextBox`に移動し、`FileContents`に LoginView コントロールのテキスト ボックスの宣言型マークアップ`LoggedInTemplate`します。

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

LoginView のテンプレートでの Web コントロールは、分離コード クラスから直接アクセスではありません。 たとえば、 `FilesGrid` GridView の`SelectedIndexChanged`と`RowDeleting`イベント ハンドラーを現在参照、`FileContents`などのコードのある TextBox コントロール。

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

ただし、このコードは有効ではなくなりました。 移動することによって、`FileContents`にテキスト ボックス、`LoggedInTemplate`テキスト ボックスに直接アクセスすることはできません。 代わりに、使用する必要があります、`FindControl("controlId")`コントロールをプログラムで参照されるメソッド。 更新プログラム、`FilesGrid`イベント ハンドラー、テキスト ボックスを参照するようになります。

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

LoginView をテキスト ボックスに移動したら`LoggedInTemplate`参照を使用して、テキスト ボックスに、ページのコードを更新し、`FindControl("controlId")`パターン、匿名ユーザーとしてページを参照してください。 図 10 に示すよう、`FileContents`テキスト ボックスは表示されません。 ただし、ビューの LinkButton が引き続き表示されます。


[![LoginView コントロールでは、FileContents テキスト ボックスに認証されたユーザーに対してのみ表示します](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**図 10**:、LoginView コントロールのみを表示、 `FileContents` Authenticated Users のテキスト ボックス ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image30.png))。


匿名ユーザーの表示 ボタンを非表示にする方法の 1 つでは、GridView フィールドを TemplateField に変換します。 これにより、ビューの LinkButton の宣言型マークアップを格納しているテンプレートが生成されます。 できます、TemplateField に LoginView コントロールを追加し、LoginView の内で LinkButton を配置`LoggedInTemplate`のため、匿名の訪問者からの表示 ボタンを非表示にします。 これを実現するには、フィールド ダイアログ ボックスを起動する GridView のスマート タグからの列の編集リンクをクリックします。 次に、左下隅で、一覧から 選択 ボタンを選択し、このフィールドを TemplateField リンクに変換 をクリックします。 これによりから、フィールドの宣言型マークアップを変更します。

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 移動先: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 この時点で、LoginView、TemplateField に追加できます。 次のマークアップでは、認証されたユーザーに対してのみ表示 LinkButton が表示されます。 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

図 11 に示す最終的な結果がかなりのビューとして列が引き続き表示されること場合でも、列内のビューの Linkbutton が非表示 次のセクションで全体の GridView 列 (およびだけでなく LinkButton) を非表示にする方法に注目するは。


[![LoginView コントロールでは、匿名の訪問者のビューの Linkbutton が非表示に](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**図 11**: LoginView コントロールでは、匿名の訪問者のビューの Linkbutton が非表示に ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image33.png))。


### <a name="programmatically-limiting-functionality"></a>プログラムで機能を制限します。

状況によっては、宣言型の手法では、ページに機能を制限する不十分です。 たとえば、特定ページの機能の可用性は、ページにアクセスして、ユーザーが匿名または認証済みかどうか外の条件に依存する可能性があります。 このような場合は、さまざまなユーザー インターフェイス要素表示したり、プログラムによる方法で非表示になります。

機能をプログラムで制限するためには、2 つのタスクを実行する必要があります。

1. ページにアクセスするユーザーが、機能にアクセスできるかどうかを判断し、
2. ユーザーが目的の機能へのアクセスを持っているかどうかに基づいて、ユーザー インターフェイスをプログラムで変更します。

これら 2 つのタスクのアプリケーションを示すためには、のみを許可 Tito GridView からファイルを削除します。 最初のタスクは、次に、Tito ページにアクセスがあるかどうかを判断するは。 決定を GridView の列の削除を非表示にする (または表示する) 必要があります。 GridView の列はからアクセスできるその`Columns`プロパティ; 場合にのみ列が表示されるその`Visible`プロパティに設定されて`True`(既定値)。

次のコードを追加、 `Page_Load` GridView にデータをバインドする前にイベント ハンドラー。

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

説明したように、 [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)チュートリアルでは、 `User.Identity.Name` id の名前を返します。 これは、ログイン コントロールに入力されたユーザー名に対応します。 Tito ページにアクセス、GridView の 2 番目の列のかどうかは`Visible`プロパティに設定されて`True`。 それ以外に設定されている`False`します。 最終的には、Tito 以外にアクセス ページで、別の認証されたユーザーまたは匿名ユーザーのいずれかと、列の削除は表示されません (図 12 を参照)。しかし、Tito、ページにアクセスし、削除列が存在する (図 13 を参照してください)。


[![列の削除はしないレンダリングされるときにアクセスでだれかが以外の Tito (Bruce) など](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**図 12**: 列の削除はしないレンダリングされるときにアクセスでだれかが以外の Tito (Bruce) など ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image36.png))。


[![列の削除は Tito をレンダリングします。](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**図 13**: Tito は表示列の削除 ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image39.png))。


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>手順 4: クラスとメソッドに承認規則を適用します。

手順 3 は、匿名ユーザーによるファイルの内容の表示を禁止し、ファイルを削除してから、すべてのユーザーが Tito を禁止されています。 これは、宣言型とプログラムによる方法で承認されていない訪問者が、関連付けられているユーザー インターフェイス要素を非表示にしていました。 この単純な例では、わかりやすく、については正しくユーザー インターフェイス要素を非表示がもっと複雑なサイトと同じ機能を実行するさまざまな方法があるでしょうか。 未承認のユーザーにその機能を制限するでは、非表示にする、またはすべての該当するユーザー インターフェイス要素を無効にするを忘れる場合どのようなどうなりますか。

機能の特定の部分を未認証のユーザーがアクセスできないことを確認する簡単な方法は、そのクラスまたはメソッドを修飾するため、 [ `PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)します。 .NET ランタイム クラスを使用してまたはそのメソッドの 1 つ実行と、現在のセキュリティ コンテキストがクラスを使用して、またはメソッドを実行する権限を持っていることを確認するを確認します。 `PrincipalPermission`属性は、これらの規則を定義できますメカニズムを提供します。

使用する例を見てみましょう、 `PrincipalPermission` GridView の属性`SelectedIndexChanged`と`RowDeleting`を匿名ユーザーと Tito、以外のユーザーによる実行を禁止するそれぞれのイベント ハンドラー。 実行する必要がありますすべては、各関数の定義上、適切な属性を追加します。

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

属性、`SelectedIndexChanged`で属性として、イベントのハンドラーが決定され、認証されたユーザーのみが、イベント ハンドラーを実行できる、`RowDeleting`イベント ハンドラーが Tito に実行を制限します。

> [!NOTE]
> 属性は、クラス、メソッド、プロパティ、またはイベントに適用できます。 属性を追加する場合は、クラス、メソッド、プロパティ、またはイベント宣言ステートメントの一部にする必要があります。 Visual Basic では、ステートメント区切り文字として改行を使用するため属性は必要がありますか、またはすぐ上に行連結文字 (アンダー スコア) で宣言と同じ行に表示されます。 上記のコード スニペットでは、行連結文字は、1 つの行で、別のメソッドの宣言属性を配置に使用されます。


Tito 以外のユーザーが実行しようとした何らかの形で場合、`RowDeleting`イベント ハンドラーまたは認証されていないユーザーを実行しようと、 `SelectedIndexChanged` .NET ランタイムで発生するイベント ハンドラー、 `SecurityException`。


[![セキュリティ コンテキストがメソッドを実行する権限がない場合、SecurityException がスローされます。](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**図 14**: セキュリティ コンテキストが、メソッドを実行する権限がない場合、`SecurityException`がスローされます ([フルサイズの画像を表示する をクリックします](user-based-authorization-vb/_static/image42.png))。


> [!NOTE]
> クラスまたはメソッドにアクセスする複数のセキュリティ コンテキストは、クラスまたはメソッドを装飾する`PrincipalPermission`の各セキュリティ コンテキストの属性。 つまり、Tito と Bruce の両方が実行できるようにする、`RowDeleting`イベント ハンドラーを追加*2 つ*`PrincipalPermission`属性。


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

多くのアプリケーションは、ASP.NET ページだけでなく、ビジネス ロジックとデータ アクセス レイヤーなど、さまざまな層を含むアーキテクチャもあります。 これらの層は、クラス ライブラリとして実装され、クラスとビジネス ロジックとデータに関連する機能を実行するためのメソッドを提供します。 `PrincipalPermission`属性はこれらの層に承認規則を適用するために便利です。

使用する方法について、`PrincipalPermission`属性をクラスやメソッドで承認規則を定義しを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ[ビジネス レイヤーを使用してデータして承認規則を追加します`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)。

## <a name="summary"></a>まとめ

このチュートリアルでは、ユーザー ベースの承認規則を適用する方法を説明しました。 ASP を参照してくださいで開始されています。NET の URL の承認フレームワーク。 要求ごとに、ASP.NET エンジンの`UrlAuthorizationModule`id が要求されたリソースへのアクセスを承認されているかどうか判断する、アプリケーションの構成で定義されている URL 承認規則を検査します。 つまり、URL 承認簡単に特定のディレクトリ内のすべてのページまたは特定のページの承認規則を指定します。

URL の承認フレームワークのでは、ページごとに承認規則が適用されます。 URL 承認の要求元のいずれかの id はか特定のリソースにアクセスする権限します。 ただし、多くのシナリオをさらに細かく承認ルールの呼び出します。 ページへのアクセスが許可されるユーザーを定義するのではなく、everyone のアクセス ページが、別のデータを表示するか、ページにアクセスして、ユーザーによってさまざまな機能を提供することがあります。 ページ レベルの承認には、通常は、承認されていないユーザーが禁止されている機能にアクセスすることを防ぐために特定のユーザー インターフェイス要素を非表示が含まれます。 さらに、クラスと特定のユーザーに対しては、そのメソッドの実行にアクセスを制限する属性を使用することができます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ビジネス層とを使用してデータ層への承認規則の追加 `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET の承認](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [IIS6 と iis 7 のセキュリティの変更](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [特定のファイルとサブディレクトリを構成します。](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [ユーザーに基づくデータ変更機能を制限します。](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [LoginView コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [IIS7 URL 承認を理解します。](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` 技術ドキュメント](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [ASP.NET 2.0 のデータの操作](../../data-access/index.md)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](validating-user-credentials-against-the-membership-user-store-vb.md)
> [次へ](storing-additional-user-information-vb.md)
