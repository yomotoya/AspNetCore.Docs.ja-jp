---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: ユーザー アカウント (VB) の作成 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、(SqlMembershipProvider) を使用してメンバーシップ フレームワークを使用して、新しいユーザー アカウントを作成するをについて説明します。 新しい私たちを作成する方法に紹介しています.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: c7f5f4ec6ce27c1a52569e6414b8f96892f68574
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826503"
---
<a name="creating-user-accounts-vb"></a>ユーザー アカウントの作成 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> このチュートリアルでは、(SqlMembershipProvider) を使用してメンバーシップ フレームワークを使用して、新しいユーザー アカウントを作成するをについて説明します。 ASP、プログラムと、新しいユーザーを作成する方法が表示されます。NET のビルトインの CreateUserWizard コントロール。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a>[前のチュートリアル](creating-the-membership-schema-in-sql-server-vb.md)アプリケーションのサービス スキーマで、データベースにある追加のテーブル、ビュー、およびストアド プロシージャで必要なインストール、`SqlMembershipProvider`と`SqlRoleProvider`します。 これは、このシリーズのチュートリアルの残りの部分の必要があります。 インフラストラクチャを作成します。 このチュートリアルでは紹介メンバーシップ フレームワークを使用して (を使用して、 `SqlMembershipProvider`) 新しいユーザー アカウントを作成します。 ASP、プログラムと、新しいユーザーを作成する方法が表示されます。NET のビルトインの CreateUserWizard コントロール。

新しいユーザー アカウントを作成する方法を学習するには、に加えても必要になりますで最初に作成したデモ web サイトを更新、  *<a id="_msoanchor_2"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)* チュートリアルの機能が強化および、  *<a id="_msoanchor_3"> </a>[フォーム認証の構成と高度なトピック](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* チュートリアル。 このデモ web アプリケーションでは、ハード コーディングされたユーザー名とパスワードの組み合わせに対してユーザーの資格情報を検証するログイン ページがあります。 さらに、`Global.asax`カスタムを作成するコードが含まれています`IPrincipal`と`IIdentity`認証されたユーザーのオブジェクト。 メンバーシップ フレームワークに対してユーザーの資格情報を検証し、プリンシパルおよび id のカスタム ロジックを削除するログイン ページが更新されます。

それでは、始めましょう!

## <a name="the-forms-authentication-and-membership-checklist"></a>フォーム認証、メンバーシップのチェックリスト

メンバーシップ フレームワークの使用を始める前にこのポイントに到達する思い出させて重要な手順を確認してみましょう。 メンバーシップ フレームワークを使用する場合、`SqlMembershipProvider`フォーム ベースの認証シナリオで、次の手順は、web アプリケーションでメンバーシップ機能を実装する前に実行する必要があります。

1. **フォーム ベース認証を有効にします。** 説明したよう *<a id="_msoanchor_4"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)*、編集することによってフォーム認証が有効になっている`Web.config`と設定、 `<authentication>`要素の`mode`属性を`Forms`します。 フォーム認証が有効になっている、各受信要求について、*フォーム認証チケット*、存在する場合、識別する要求元。
2. **適切なデータベースへのアプリケーションのサービス スキーマを追加します。** 使用する場合、`SqlMembershipProvider`アプリケーション サービスのスキーマをデータベースにインストールする必要があります。 通常、このスキーマは、アプリケーションのデータ モデルを保持しているのと同じデータベースに追加されます。 *<a id="_msoanchor_5"> </a> [SQL Server でメンバーシップ スキーマを作成する](creating-the-membership-schema-in-sql-server-vb.md)* チュートリアルの検索を使用して、`aspnet_regsql.exe`これを実現するツール。
3. **手順 2 からデータベースを参照する Web アプリケーションの設定をカスタマイズします。** *SQL Server でメンバーシップ スキーマを作成*チュートリアルでは、web アプリケーションを構成する 2 つの方法を示しましたように、`SqlMembershipProvider`は手順 2. で選択したデータベースの使用: 変更することによって、`LocalSqlServer`接続文字列名。または、framework のメンバーシップ プロバイダーの一覧に新しい登録済みのプロバイダーを追加してからデータベースを使用して新しいプロバイダーのカスタマイズ手順 2。

使用する web アプリケーションを構築する場合、`SqlMembershipProvider`フォーム ベース認証を使用する前にこれら 3 つの手順を実行する必要が、`Membership`クラスまたは Asp.net ログイン コントロール。 既にこれらの手順を実行前のチュートリアルで、以降は、メンバーシップ フレームワークの使用を開始する準備ができました!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新しい ASP.NET ページの追加

このチュートリアルで、次の 3 検査メンバーシップ関連の機能と機能のさまざまな。 一連のトピックでは、このチュートリアルで調査を実装するために ASP.NET ページを必要になります。 これらのページと、サイト マップ ファイルを作成しましょう`(Web.sitemap)`します。

という名前のプロジェクトで新しいフォルダーを作成して開始`Membership`します。 次に 5 つの新しい ASP.NET ページを追加、`Membership`フォルダーを使用する各ページのリンク、`Site.master`マスター ページ。 ページの名前を付けます。

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

この時点で、プロジェクトのソリューション エクスプ ローラーのスクリーン ショット、図 1 に示すようなはずです。


[![メンバーシップのフォルダーに 5 つの新しいページが追加されました](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**図 1**: 5 新しいページに追加されている、`Membership`フォルダー ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image3.png))。


各ページが 2 つのコンテンツ コントロールは、マスター ページの ContentPlaceHolders ごとに 1 つがある必要があります、この時点では、:`MainContent`と`LoginContent`します。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

いることを思い出してください、 `LoginContent` ContentPlaceHolder の既定のマークアップがログオンまたはログオフ、ユーザーが認証されているかどうかによっては、サイトへのリンクが表示されます。 有無、`Content2`コンテンツ コントロールは、ただし、マスター ページの既定のマークアップをオーバーライドします。 説明したよう *<a id="_msoanchor_6"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)* チュートリアルでは、これは、ページの左側にあるログインに関連するオプションを表示しない場所おで役立ちます。

これら 5 つのページでは、ただし、表示することのマスター ページの既定のマークアップ、`LoginContent`プレース ホルダーです。 そのため、宣言型マークアップを削除する、`Content2`コンテンツ コントロール。 その後、1 つだけのコンテンツ コントロールを含めることが 5 つのページのマークアップの必要があります。

## <a name="step-2-creating-the-site-map"></a>手順 2: サイト マップを作成します。

最も単純な web サイトを除くすべては、何らかの形式のナビゲーション ユーザー インターフェイスを実装する必要があります。 ナビゲーション ユーザー インターフェイスは、サイトのさまざまなセクションへのリンクの簡単なリストにあります。 または、これらのリンクは、メニューまたはツリー ビューに配置された可能性があります。 ページの開発者としてナビゲーション ユーザー インターフェイスの作成は、ストーリーの半分だけです。 保守し、更新可能な方法で、サイトの論理構造を定義するなんらかの手段も必要です。 新しいページを追加または既存のページの削除のサイト マップの 1 つのソースを更新したいし、が、サイトのナビゲーション ユーザー インターフェイスの間で、これらの変更が反映されます。

サイト マップを定義およびベースのサイト マップ ナビゲーション ユーザー インターフェイスの実装ではこれら 2 つのタスクが簡単に、サイト マップ フレームワークに協力してくれた実現し、ナビゲーション Web が追加で ASP.NET version 2.0 を制御します。 サイト マップ フレームワークにより、開発者が、サイト マップを定義するためにプログラムによる API からアクセスし、(、 [ `SiteMap`クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx))。 Web のナビゲーション コントロールの組み込みが含まれて、[メニュー コントロール](https://msdn.microsoft.com/library/bz09dy46.aspx)、 [TreeView コントロール](https://msdn.microsoft.com/library/3eafky27.aspx)、および[SiteMapPath コントロール](https://msdn.microsoft.com/library/3eafky27.aspx)。

メンバーシップとロールのフレームワークのように、サイト マップ framework は基に作成された、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)します。 サイト マップ プロバイダー クラスのジョブは、によって使用されるメモリ内構造を生成する、 `SiteMap` XML ファイルまたはデータベース テーブルなどの永続的なデータ ストアからのクラス。 XML ファイルからサイト マップ データを読み取る、既定のサイト マップ プロバイダーが .NET Framework が付属しています ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx))、これは、このチュートリアルで使用するプロバイダー。 いくつか別のサイト マップ プロバイダーの実装このチュートリアルの最後に、それ以上の測定値のセクションを参照してください。

既定のサイト マップ プロバイダーという名前の適切にフォーマットされた XML ファイルが必要ですが`Web.sitemap`ルート ディレクトリが存在します。 この既定のプロバイダーを使用していることからこのようなファイルを追加し、適切な XML 形式で、サイト マップの構造を定義する必要があります。 ファイルを追加するには、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 という名前のサイト マップの種類のファイルを追加することを選択 ダイアログ ボックスで`Web.sitemap`します。


[![プロジェクトのルート ディレクトリに Web.sitemap という名前のファイルを追加します。](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**図 2**: ファイルの名前を追加`Web.sitemap`プロジェクトのルート ディレクトリに ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image6.png))。


XML のサイト マップ ファイルは、階層構造として、web サイトの構造を定義します。 この階層関係がの先祖を使用して XML ファイルでモデル化、`<siteMapNode>`要素。 `Web.sitemap`で開始する必要があります、`<siteMap>`を正確に 1 つを持つ親ノード`<siteMapNode>`子。 この最上位`<siteMapNode>`要素は、階層のルートを表すし、任意の数の子孫ノードの場合があります。 各`<siteMapNode>`要素を含める必要があります、`title`属性し、必要に応じて含めることができます`url`と`description`; 他のユーザーの間での属性は、各空でない`url`属性は一意である必要があります。

次の XML を入力、`Web.sitemap`ファイル。

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

上記のサイト マップのマークアップは、図 3 に示すように階層を定義します。


[![階層ナビゲーション構造を表すサイト マップ](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**図 3**: 階層ナビゲーション構造を表すサイト マップ ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image9.png))。


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>手順 3: マスター ページのナビゲーション ユーザー インターフェイスを更新しています

ASP.NET には、さまざまなユーザー インターフェイスを設計するためのナビゲーション関連の Web コントロールが含まれています。 これらには、メニューのツリー ビュー、および SiteMapPath コントロールが含まれます。 メニューを TreeView コントロールは、SiteMapPath、その先祖とアクセスされている現在のノードを示す階層リンクが表示されますが、それぞれ、メニューまたはツリーで、サイト マップ構造をレンダリングします。 サイト マップ データ Web コントロール、SiteMapDataSource を使用して他のデータにバインドすることができを使用してプログラムでアクセスできる、`SiteMap`クラス。

サイト マップのフレームワークとナビゲーション コントロールの完全な詳細については、このチュートリアル シリーズのスコープ外ですが後ではなく独自のナビゲーション ユーザー インターフェイスを作成してみましょう時間を費やす代わりに借用で使用される、  *[ASP.NET 2.0 でデータを扱う](../../data-access/index.md)* を図 4 に示すように、Repeater コントロールを使用して、ナビゲーション リンクの 2 つが深い箇条書きリストを表示するチュートリアル シリーズでします。

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>左の列に 2 つのレベルのリンクの一覧を追加します。

このインターフェイスを作成するには、次に示す宣言型マークアップを追加、`Site.master`マスター ページの左の列をテキスト TODO: メニューの移動は... ここで現在が存在します。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

上記のマークアップという名前の Repeater コントロールのバインド`menu`で定義されているサイト マップの階層が返されます、SiteMapDataSource に`Web.sitemap`します。 以降、SiteMapDataSource コントロールの[`ShowStartingNode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)以降、[ホーム] ノードの子孫では、サイト マップの階層が返され始めます False に設定されます。 これらのノード (だけ現在のメンバーシップ) の各 Repeater を表示、`<li>`要素。 次に、Repeater、別の内部は順序なしリストを入れ子になったで現在のノードの子を表示します。

図 4 は、手順 2. で作成したサイト マップ構造で表示される出力を上記のマークアップを示します。 Repeater は、バニラの順序なしリスト マークアップ; を出力します。定義されているカスケード スタイル シートのルール`Styles.css`美しくて心地よいレイアウトを担当します。 上記のマークアップのしくみの詳細についてを参照してください、[マスター ページとサイト ナビゲーション](https://asp.net/learn/data-access/tutorial-03-vb.aspx)チュートリアル。


[![ナビゲーション ユーザー インターフェイスが表示されるリストを使用して入れ子になった順序付けられていません。](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**図 4**: The ナビゲーション ユーザー インターフェイスが表示されるリストを使用して入れ子になった順序付けられていない ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image12.png))。


### <a name="adding-breadcrumb-navigation"></a>階層リンク ナビゲーションを追加します。

左側の列のリンクの一覧で、に加えてみましょうもがある各ページの表示、[階層リンク](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)します。 階層リンクは、サイト階層内の現在位置をそのユーザーをすばやく表示するナビゲーション ユーザー インターフェイス要素です。 SiteMapPath コントロールは、サイト マップ内の現在のページの場所を特定するサイト マップのフレームワークを使用し、し、この情報に基づいて階層リンクを表示します。

具体的には、追加、`<span>`要素、マスター ページのヘッダーを`<div>`要素、新しい設定と`<span>`要素の`class`属性階層リンクをします。 (、`Styles.css`クラスには、階層リンク クラスのルールが含まれています)。次に、この新規、SiteMapPath を追加`<span>`要素。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

図 5 にアクセスしたときに、SiteMapPath の出力を示しています`~/Membership/CreatingUserAccounts.aspx`します。


[![階層リンクには、現在のページが表示されます。 および、サイトでは、その先祖のマップ](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**図 5**: 階層リンクは、サイト マップの現在のページとその先祖を表示します ([フルサイズの画像を表示する をクリックします。](creating-user-accounts-vb/_static/image15.png))。


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>手順 4: カスタム プリンシパルおよび Id ロジックを削除します。

*<a id="_msoanchor_7"> </a>[フォーム認証の構成と高度なトピック](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* チュートリアルが認証済みユーザーに、カスタム プリンシパルと id オブジェクトに関連付ける方法を説明しました。 内のイベント ハンドラーを作成してこれを実現`Global.asax`のアプリケーションの`PostAuthenticateRequest`、後に発生するイベント、`FormsAuthenticationModule`がユーザーを認証します。 このイベント ハンドラーに置き換え、`GenericPrincipal`と`FormsIdentity`によって追加されたオブジェクト、`FormsAuthenticationModule`で、`CustomPrincipal`と`CustomIdentity`そのチュートリアルで作成したオブジェクトします。

カスタム プリンシパルと id オブジェクトは、ほとんどの場合、特定のシナリオで役に立ちます、`GenericPrincipal`と`FormsIdentity`オブジェクトで十分です。 その結果、既定の動作に戻るにことと思います。 この変更を削除するか、コメント アウトすることによって、`PostAuthenticateRequest`イベント ハンドラーまたは削除することによって、`Global.asax`完全ファイルします。

> [!NOTE]
> コメントまたは内のコードを削除した後`Global.asax`でコードをコメント アウトする必要があります`Default.aspx's`をキャストする分離コード クラス、`User.Identity`プロパティを`CustomIdentity`インスタンス。


## <a name="step-5-programmatically-creating-a-new-user"></a>手順 5: プログラムによって新しいユーザーの作成

メンバーシップ フレームワークを使用して新しいユーザー アカウントを作成する、`Membership`クラスの[`CreateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)します。 このメソッドに、ユーザー名、パスワード、およびその他のユーザーに関連するフィールドのパラメーターを入力する必要があります。 呼び出しでは、上の 新しいユーザー アカウントの作成を構成済みのメンバーシップ プロバイダーにデリゲートを返しますが、 [ `MembershipUser`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)作成したばかりのユーザー アカウントを表します。

`CreateUser`メソッドが 4 つのオーバー ロード、さまざまな入力パラメーター数を入力します。

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

これら 4 つのオーバー ロードは、収集される情報の量によって異なります。 最初のオーバー ロードなどが必要です、ユーザー名とパスワード、新しいユーザー アカウントの 2 つ目では、ユーザーの電子メール アドレスも必要ですが。

これらのオーバー ロードは、新しいユーザー アカウントを作成するための情報が、メンバーシップ プロバイダーの構成設定に依存するために存在します。 *<a id="_msoanchor_8"> </a> [SQL Server でメンバーシップ スキーマを作成する](creating-the-membership-schema-in-sql-server-vb.md)* でメンバーシップ プロバイダーの構成設定を指定する調べるチュートリアル`Web.config`します。 表 2 には、構成設定の完全な一覧が含まれています。

どのような影響を与える設定を 1 つこのようなメンバーシップ プロバイダー構成`CreateUser`オーバー ロードを使用することがありますが、`requiresQuestionAndAnswer`設定します。 場合`requiresQuestionAndAnswer`に設定されている`true`(既定)、セキュリティの質問と回答を指定新しいユーザー アカウントを作成するときにする必要があります。 ユーザーがリセットまたは自分のパスワードを変更する必要がある場合、この情報は後で使用します。 具体的には、その時点でのセキュリティの質問が表示し、リセットまたはパスワードを変更するには正しい解答を入力する必要があります。 そのため場合、`requiresQuestionAndAnswer`に設定されている`true`最初の 2 つのいずれかを呼び出してから`CreateUser`セキュリティの質問と回答が見つからないため、例外の結果をオーバー ロードします。 アプリケーションは現在のセキュリティの質問と回答を要求するように構成されている、ためプログラムでユーザーを作成するときに、後者の 2 つのオーバー ロードのいずれかを使用する必要になります。

使用して説明するために、`CreateUser`メソッドを名、パスワード、電子メール、および定義済みのセキュリティの質問の答えのユーザーを表示ユーザー インターフェイスを作成しましょう。 開く、`CreatingUserAccounts.aspx`ページで、`Membership`フォルダー コンテンツ コントロールに、次の Web コントロールを追加。

- という名前のテキスト ボックス `Username`
- という名前のテキスト ボックス`Password`が`TextMode`プロパティに設定 `Password`
- という名前のテキスト ボックス `Email`
- という名前のラベル`SecurityQuestion`でその`Text`プロパティが取り除か
- という名前のテキスト ボックス `SecurityAnswer`
- という名前のボタン`CreateAccountButton`が`Text`プロパティをユーザー アカウントの作成 を設定
- という名前のラベル コントロール`CreateAccountResults`でその`Text`プロパティが取り除か

この時点で、画面のスクリーン ショット、図 6 に示すようなはずです。


[![CreatingUserAccounts.aspx ページに、さまざまな Web コントロールを追加します。](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**図 6**: さまざまな Web コントロールを追加、 `CreatingUserAccounts.aspx Page` ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image18.png))。


`SecurityQuestion`ラベルと`SecurityAnswer`テキスト ボックスには、定義済みのセキュリティの質問を表示し、ユーザーの回答を収集するためのものです。 おり、独自のセキュリティの質問を定義するには、各ユーザーを許可することがユーザー単位のユーザーごとにセキュリティの質問と回答の両方が格納されていることに注意してください。 ただし、つまり、ユニバーサル セキュリティの質問を使用することにしましたが、この例の: 好きな色とは何ですか?

この定義済みのセキュリティの質問を実装するには、という名前のページの分離コード クラスに定数を追加`passwordQuestion`セキュリティの質問を割り当てることです。 次に、`Page_Load`イベント ハンドラー、割り当てにこの定数、`SecurityQuestion`ラベルの`Text`プロパティ。

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

イベント ハンドラーを次に、作成、 `CreateAccountButton'` s`Click`イベントと、次のコードを追加します。

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click`という名前の変数を定義することでイベント ハンドラーを起動`createStatus`型の[ `MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx)します。 `MembershipCreateStatus` 状態を示す列挙体は、`CreateUser`操作。 たとえば、ユーザー アカウントが正常に作成されると、結果の`MembershipCreateStatus`インスタンスの値に設定されます`Success;`その一方で、同じユーザー名を持つユーザーが既に存在するために、操作が失敗した場合は設定する必要がの値`DuplicateUserName`. `CreateUser`使用のオーバー ロードに渡す必要があります、`MembershipCreateStatus`インスタンス メソッドにします。 このパラメーターは内で適切な値に設定されて、`CreateUser`メソッド、およびユーザー アカウントが正常に作成されているかどうかを判断するメソッド呼び出しの後にその値を確認できます。

呼び出した後`CreateUser`で渡し`createStatus`、`Select Case`に割り当てられている値に応じて、適切なメッセージを出力するステートメントが使用される`createStatus`します。 図 7 は、新しいユーザーが正常に作成されたときに、出力を示します。 図 8 と 9 は、ユーザー アカウントが作成されていない場合、出力を表示します。 図 8 には、訪問者は、メンバーシップ プロバイダーの構成設定に記述されたパスワードの強度の要件を満たしていない、5 文字のパスワードを入力します。 図 9 で、訪問者が既存のユーザー名 (図 7 で作成されたもの) でユーザー アカウントを作成しようとするは。


[![新しいユーザー アカウントが正常に作成されました](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**図 7**:、[新しいユーザー アカウントが正常に作成されました ([フルサイズの画像を表示する] をクリックします](creating-user-accounts-vb/_static/image21.png))。


[![指定されたパスワードが脆弱すぎますために、ユーザー アカウントは作成されません。](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**図 8**: 指定されたパスワードが脆弱すぎますために、ユーザー アカウントは作成されません ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image24.png))。


[![ユーザー アカウントが作成されたため、ユーザー名は既に使用中](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**図 9**:「ユーザー アカウントが作成されたため、ユーザー名は既に使用中 ([フルサイズ画像を表示する をクリックします。](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> 最初の 2 つのいずれかを使用する場合は、成功または失敗を判断する方法と思うかもしれません`CreateUser`もメソッドがオーバー ロードを持つ型のパラメーターの`MembershipCreateStatus`します。 これらの最初の 2 つのオーバー ロードのスロー、 [ `MembershipCreateUserException`例外](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx)を含む、障害が発生した場合、 [ `StatusCode`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx)型の`MembershipCreateStatus`します。


いくつかのユーザー アカウントを作成した後の内容を一覧表示して、アカウントが作成されたことを確認、`aspnet_Users`と`aspnet_Membership`内のテーブル、`SecurityTutorials.mdf`データベース。 図 10 に示すようを使用して 2 つのユーザーを追加した、`CreatingUserAccounts.aspx`ページ: Tito と Bruce します。


[![メンバーシップ ユーザー ストアにある 2 人のユーザー: Tito と Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**図 10**: メンバーシップ ユーザー ストアにある 2 人のユーザー: Tito と Bruce ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image30.png))。


メンバーシップ ユーザー ストアはこれで、Bruce と Tito のアカウントの情報が含まれていますが、あるサイトにログオンするには、Bruce または Tito できる機能を実装します。 現時点では、`Login.aspx`ユーザーの資格情報を検証は 1 組のユーザー名/パスワードのハード コーディングされたセットに対して*いない*メンバーシップ フレームワークに対して指定された資格情報を検証します。 ここでの新しいユーザー アカウントを表示するため、`aspnet_Users`と`aspnet_Membership`テーブルが十分でする必要があります。 次のチュートリアルで *<a id="_msoanchor_9"> </a>[を検証するユーザー資格情報に対して、メンバーシップ ユーザー ストア](validating-user-credentials-against-the-membership-user-store-vb.md)*、メンバーシップ ストアの検証に使用するログイン ページを更新します。

> [!NOTE]
> すべてのユーザーに表示されない場合、`SecurityTutorials.mdf`データベース、可能性があります、web アプリケーションは既定のメンバーシップ プロバイダーを使用して`AspNetSqlMembershipProvider`、使用、`ASPNETDB.mdf`そのユーザーのストアとしてのデータベース。 この問題を確認するのには、ソリューション エクスプ ローラーで [更新] ボタンをクリックします。 という名前のデータベースの場合`ASPNETDB.mdf`に追加されて、`App_Data`フォルダー、問題になります。 手順 4. に戻り、  *<a id="_msoanchor_10"> </a> [SQL Server でメンバーシップ スキーマを作成する](creating-the-membership-schema-in-sql-server-vb.md)* メンバーシップ プロバイダーを適切に構成する方法についてのチュートリアル。


ほとんどのシナリオを作成ユーザー アカウント、訪問者は、ユーザー名、パスワード、電子メール、および新しいアカウントの作成時点で、必要なその他の情報を入力するいくつかのインターフェイスが表示されます。 この手順で手動でこのようなインターフェイスの構築について説明しましたし、使用する方法を説明し、、`Membership.CreateUser`プログラムで新しいユーザー アカウントを追加する方法、ユーザーの入力に基づいています。 このコードは、新しいユーザー アカウントを作成しました。 フォロー アップを作成したばかりのユーザー アカウントでは、サイトにユーザーのログインまたはユーザーに確認メールを送信するなどのアクションは実行しませんでした。 追加の手順で、ボタンの追加のコードが必要になります`Click`イベント ハンドラー。

ASP.NET は、メンバーシップ フレームワークでアカウントを作成し、後のアカウントを実行すること、新しいユーザー アカウントを作成するためのユーザー インターフェイスの表示から、ユーザーのアカウント作成プロセスを処理するために設計されていますが、CreateUserWizard コントロールに付属します。確認の電子メールを送信して、サイトに作成したばかりのユーザーをログインなどの作成作業します。 CreateUserWizard コントロールを使用するは CreateUserWizard コントロールをツールボックスから、ページにドラッグして、いくつかのプロパティを設定し、同じくらい簡単です。 ほとんどの場合、1 行のコードを記述する必要はありません。 手順 6 で詳細に気の利いたこのコントロールを紹介します。

場合のみ、新しいユーザー アカウントはアカウントの作成の一般的な web ページを作成、可能性がありますいないを使用するコードを記述する必要がこれまで、`CreateUser`メソッド、CreateUserWizard コントロールとして可能性がありますニーズを満たします。 ただし、`CreateUser`メソッドは、詳細にカスタマイズされたアカウントの作成のユーザー エクスペリエンスが必要がありますまたはプログラムで代替インターフェイスを通じて新しいユーザー アカウントを作成する必要がある場合のシナリオで便利です。 たとえば、ユーザーが他のアプリケーションからユーザー情報を含む XML ファイルをアップロードできるページがあります。 ページがアップロードされた XML の内容は、ファイルし、呼び出すことによって、XML で表されるユーザーごとに新しいアカウントを作成、解析、`CreateUser`メソッド。

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>手順 6: CreateUserWizard コントロールに新しいユーザーを作成します。

ASP.NET では、ログイン Web コントロールの数が付属しています。 これらのコントロールは、一般的なユーザー アカウントにログイン関連シナリオに役立ちます。 [CreateUserWizard コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)メンバーシップ フレームワークに新しいユーザー アカウントを追加するためのユーザー インターフェイスを提供するように設計されたこのような 1 つのコントロールです。

多くの他のログインに関連する Web コントロールと、1 行のコードを記述することがなく、CreateUserWizard を使用できます。 メンバーシップ プロバイダーの構成設定と内部的に呼び出しに基づくユーザー インターフェイスを提供する直感的に、`Membership`クラスの`CreateUser`後に、ユーザーが必要な情報を入力し、ユーザーの作成 ボタンをクリックします。 CreateUserWizard コントロールは、非常にカスタマイズできます。 アカウントの作成プロセスのさまざまな段階中に発生するイベントのホストがあります。 アカウントの作成のワークフローにカスタム ロジックを挿入する、必要に応じて、イベント ハンドラーを作成できます。 さらに、CreateUserWizard の外観は、非常に柔軟性があります。 既定のインターフェイスの外観を定義するプロパティがいくつか必要に応じて、コントロールをテンプレートに変換できるか、追加のユーザーの登録ステップを追加することがあります。

CreateUserWizard コントロールの既定のインターフェイスと動作の使用を見てから始めましょう。 コントロールのプロパティおよびイベントを使用して外観をカスタマイズする方法について説明します。

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>CreateUserWizard の既定のインターフェイスと動作の確認

戻り、`CreatingUserAccounts.aspx`ページで、`Membership`フォルダーをデザインまたは分割モードに切り替えるし、ページの上部に CreateUserWizard コントロールを追加します。 CreateUserWizard コントロールをツールボックスのログイン コントロール セクションをファイリングします。 コントロールを追加すると、次のように設定します。 その`ID`プロパティを`RegisterUser`します。 画面は、図 11 で、CreateUserWizard は、新しいユーザーのユーザー名、パスワード、電子メール アドレス、およびセキュリティの質問および答えのテキスト ボックスを持つインターフェイスをレンダリングします。


[![CreateUserWizard コントロール レンダリング汎用的なユーザー インターフェイスを作成します。](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**図 11**: CreateUserWizard コントロールは汎用的な作成ユーザー インターフェイスを表示します ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image33.png))。


手順 5 で作成したインターフェイスを持つ CreateUserWizard コントロールによって生成される既定のユーザー インターフェイスを比較する少し見てみましょう。 まず、CreateUserWizard コントロールは、手動で作成されたインターフェイスの定義済みのセキュリティの質問に対してセキュリティの質問と回答の両方を指定するビジターをできます。 CreateUserWizard コントロールのインターフェイスには、インターフェイスのフォーム フィールドの検証を実装するためにまだがありましたが、検証コントロールも含まれます。 および CreateUserWizard コントロール インターフェイスには (を十分にテキストが、パスワードを入力し、テキスト ボックスの パスワードの比較が等しい CompareValidator) とパスワードの確認テキスト ボックスが含まれています。

ユーザー インターフェイスを表示するときに、CreateUserWizard コントロールが、メンバーシップ プロバイダーの構成設定を参照している、興味深いです。 たとえば、セキュリティの質問と答えのテキスト ボックスが表示される場合のみ`requiresQuestionAndAnswer`が True に設定します。 CreateUserWizard が自動的に設定、パスワードの強度の要件が満たされていることを確認する RegularExpressionValidator コントロールを追加同様に、その`ErrorMessage`と`ValidationExpression`プロパティに基づいて、 `minRequiredPasswordLength`、 `minRequiredNonalphanumericCharacters`、および`passwordStrengthRegularExpression`構成設定。

CreateUserWizard コントロールはその名のとおりから派生は、[ウィザード コントロール](https://msdn.microsoft.com/library/s2etd1ek.aspx)します。 ウィザード コントロールは、複数手順のタスクを完了するためのインターフェイスを提供する設計されています。 ウィザード コントロールが任意の数のあります`WizardSteps`、それぞれが、HTML を定義するテンプレートおよび Web のコントロールのステップ。 ウィザード コントロールは、まず最初に表示されます`WizardStep`、と共にへ、1 つのステップから続行するか、前の手順に戻るには、ユーザーが使用できるナビゲーション コントロール。

図 11 では宣言型マークアップを示していますとは 2 つの CreateUserWizard コントロールの既定のインターフェイスが含まれます`WizardStep`: %s

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? 新しいユーザー アカウントを作成するための情報を収集するインターフェイスをレンダリングします。 これは、図 11 に示す手順です。
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? アカウントが正常に作成されたことを示すメッセージを表示します。

次の手順のいずれかのテンプレートに変換することで、または独自に追加することで、CreateUserWizard の外観と動作を変更できる`WizardStep`秒。 追加することに注目するは、`WizardStep`で登録インターフェイスに、*追加ユーザーの情報を格納する*チュートリアル。

CreateUserWizard コントロールの動作を見てみましょう。 参照してください、`CreatingUserAccounts.aspx`ページがブラウザーを使用します。 CreateUserWizard のインターフェイスにいくつかの無効な値を入力して起動します。 お試しくださいパスワード強度の要件に適合しないパスワードを入力するか、ユーザー名テキスト ボックスを空のままです。 CreateUserWizard、適切なエラー メッセージが表示されます。 図 12 は、十分に強力なパスワードを使用してユーザーを作成する際に出力を示します。


[![CreateUserWizard に自動的に検証コントロールを挿入します。](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**図 12**:、CreateUserWizard 自動的に挿入検証コントロール ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image36.png))。


次に、CreateUserWizard に適切な値を入力し、ユーザーの作成 ボタンをクリックします。 必須のフィールドが入力されているし、パスワードの強度が十分なと仮定すると、CreateUserWizard はメンバーシップ フレームワークを通じて新しいユーザー アカウントを作成し、表示、`CompleteWizardStep`のインターフェイスを (図 13 を参照してください)。 CreateUserWizard を呼び出し、バック グラウンドで、`Membership.CreateUser`メソッド、手順 5. で行ったのと同じようです。


[![新しいユーザー アカウントが正常に作成されました](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**図 13**:、[新しいユーザー アカウントが正常に作成された ([フルサイズの画像を表示する] をクリックします](creating-user-accounts-vb/_static/image39.png))。


> [!NOTE]
> 図 13 に示すよう、`CompleteWizardStep`のインターフェイスには、続行ボタンが含まれています。 ただし、この時点でのみクリックしては、同じページに訪問者を残したまま、ポストバックを実行します。 カスタマイズ CreateUserWizard の外観と動作からのプロパティ セクションでに訪問者の送信は、このボタンのある方法を紹介`Default.aspx`(またはその他のいくつかのページ)。


新しいユーザー アカウントを作成すると、Visual Studio に戻り、確認、`aspnet_Users`と`aspnet_Membership`アカウントが正常に作成されたことを確認する図 10 で行ったようにテーブルです。

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>CreateUserWizard の動作とそのプロパティを使用した外観をカスタマイズします。

さまざまなプロパティを使用の方法でカスタマイズできます。 CreateUserWizard`WizardStep`秒、およびイベント ハンドラー。 このセクションでのプロパティを使用したコントロールの外観をカスタマイズする方法に注目するは次のセクションがイベント ハンドラーを使用して、コントロールの動作を拡張します。

CreateUserWizard コントロールの既定のユーザー インターフェイスに表示されるテキストのほぼすべての大量のプロパティの使用してカスタマイズできます。 によって、テキスト ボックスの左側に表示されるユーザー名、パスワード、パスワードの確認、電子メール、セキュリティの質問、および秘密の答えのラベルのカスタマイズなど、 [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx)、 [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx)、 [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx)、 [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx)、 [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)、および[ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)プロパティは、それぞれします。 同様のユーザーの作成と続行ボタンにテキストを指定するにはプロパティは、`CreateUserWizardStep`と`CompleteWizardStep`これらのボタンは、ボタン、Linkbutton、または ImageButtons としてレンダリングされる場合と同様、します。

色、罫線、フォント、および他のビジュアル要素は、ホストのスタイル プロパティを使用して構成できます。 CreateUserWizard コントロール自体が、共通の Web コントロールのスタイル プロパティの`BackColor`、 `BorderStyle`、 `CssClass`、`Font`というように、スタイル プロパティの特定のセクションの外観を定義するためのいくつかと、CreateUserWizard のインターフェイスです。 [ `TextBoxStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)、たとえば、テキスト ボックスのスタイルを定義、`CreateUserWizardStep`中、 [ `TitleTextStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx)(サインアップ、新しいタイトルのスタイルを定義します。アカウント)。

外観に関連するプロパティだけでなく、多数の CreateUserWizard コントロールの動作に影響するプロパティがあります。 [ `DisplayCancelButton`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)場合、true の場合、表示する (既定値は False)、ユーザーの作成ボタンの横の [キャンセル] ボタンを設定します。 [キャンセル] ボタンを表示する場合にも設定することを確認して、 [ `CancelDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)ユーザーは [キャンセル] をクリックした後に送信されるページを指定します。 [続行] ボタン、前のセクションで説明したように、`CompleteWizardStep`のインターフェイスは、ポストバックを発生させるが、同じページに訪問者を残します。 [続行] ボタンをクリックした後に他のいくつかのページに訪問者を送信するで URL を指定だけ、 [ `ContinueDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)します。

更新、 `RegisterUser` CreateUserWizard コントロール [キャンセル] ボタンを表示して、訪問者に送信する`Default.aspx`[キャンセル] または [続行] ボタンがクリックされたとき。 これを行うには、設定、`DisplayCancelButton`とプロパティを True に、`CancelDestinationPageUrl`と`ContinueDestinationPageUrl`プロパティを ~/Default.aspx です。 図 14 は、ブラウザーで表示した場合は、更新された CreateUserWizard を示します。


[![CreateUserWizardStep には、[キャンセル] ボタンが含まれています。](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**図 14**: `CreateUserWizardStep` [キャンセル] ボタンが含まれています ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image42.png))。


訪問者は、ユーザー名、パスワード、電子メール アドレス、およびセキュリティの質問および回答の入力、ユーザーの作成 をクリックするは、新しいユーザー アカウントが作成され、訪問者を新しく作成したユーザーとして記録されます。 ページにアクセスするユーザーが自分で新しいアカウントを作成することと仮定すると、これが目的の動作である可能性がありますです。 ただし、新しいユーザー アカウントを追加する管理者を許可することがあります。 これにより、ユーザー アカウントが作成されますが (および新しく作成したアカウントではなく) 管理者として、管理者がログインしたままとします。 この動作は、ブール値を変更できます[`LoginCreatedUser`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)します。

メンバーシップ フレームワーク内のユーザー アカウントに含まれて、承認済みのフラグです。ユーザーが承認されていないユーザーは、サイトにログインすることです。 既定では、新しく作成したアカウントは、ユーザーがすぐに、サイトにログインできるように、承認済みとしてマークされます。 ただし、未承認としてマークされている新しいユーザー アカウントを持っていることです。 管理者は; にログインできるようにする前に、新しいユーザーを手動で承認する必要があります。または、ユーザーがログオンを許可する前に、サインアップ時に入力した電子メール アドレスが有効であることを確認したい場合があります。 これは、場合でも、CreateUserWizard コントロールの設定によって未承認としてマークされている新しく作成したユーザー アカウントがあることができます[`DisableCreatedUser`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)true (既定値は False です)。

注目すべきその他の動作に関連のプロパティが含まれます`AutoGeneratePassword`と`MailDefinition`します。 場合、 [ `AutoGeneratePassword`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx)を True に設定されている、`CreateUserWizardStep`パスワードとパスワードの確認テキスト ボックスで; には表示されません代わりに、新しく作成したユーザーのパスワードが自動的に生成を使用して、 `Membership`クラスの[`GeneratePassword`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)です。 `GeneratePassword`メソッドが構成されているパスワードの強度の要件を満たすために英数字以外の文字のための十分な数と指定した長さのパスワードを作成します。

[ `MailDefinition`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)アカウントの作成プロセス中に指定した電子メール アドレスに電子メールを送信する場合に便利です。 `MailDefinition`プロパティに一連構築された電子メール メッセージに関する情報を定義するためのサブプロパティにはが含まれています。 これらのサブプロパティがなどのオプションを含める`Subject`、 `Priority`、 `IsBodyHtml`、 `From`、 `CC`、および`BodyFileName`します。 [ `BodyFileName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)テキストまたは電子メール メッセージの本文を含む HTML ファイルをポイントします。 本文は、事前に定義された 2 つのプレース ホルダーをサポートしています:`<%UserName%>`と`<%Password%>`します。 これらのプレース ホルダーに存在する場合、`BodyFileName`ファイルでは、作成したばかりのユーザーの名とパスワードに置き換えられます。

> [!NOTE]
> `CreateUserWizard`コントロールの`MailDefinition`プロパティには、新しいアカウントが作成されるときに送信される電子メール メッセージの詳細だけを指定します。 電子メール メッセージを実際に送信する方法については含まれません (は、SMTP のサーバーまたはメール ドロップ ディレクトリを使用するかどうか、認証情報、およびなど)。 これらの低レベルの詳細で定義する必要があります、`<system.net>`セクション`Web.config`します。 これらの構成設定と ASP.NET 2.0 から電子メールを送信すると、一般の詳細についてを参照してください、 [SystemNetMail.com でよく寄せられる質問](http://www.systemnetmail.com/)および筆者の記事[ASP.NET 2.0 での電子メールの送信](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)します。


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>イベント ハンドラーを使用して、CreateUserWizard の動作を拡張します。

CreateUserWizard コントロールは、そのワークフローの中にイベントの数を生成します。 たとえば、訪問者は、ユーザー名、パスワード、および他の関連情報を入力し、ユーザーの作成 ボタンをクリックして、CreateUserWizard コントロールが発生させます。 その[`CreatingUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)します。 作成プロセス中に問題がある場合、 [ `CreateUserError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)が発生します。 ただし、ユーザーが正常に作成する場合は、次に、 [ `CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)が発生します。 生成される追加の CreateUserWizard コントロール イベントが、これらは、次の 3 つの最も密接な。

特定のシナリオで、CreateUserWizard のワークフロー、適切なイベントのイベント ハンドラーを作成するためにタップすることもできます。 これを示すためを強化しましょう、 `RegisterUser` CreateUserWizard コントロールをいくつかのカスタム検証では、ユーザー名とパスワードを含めます。 ユーザー名が先頭または末尾のスペースを含めることはできませんし、パスワードにユーザー名ことはできません任意の場所に表示されるように、CreateUserWizard をしましょう強化具体的には、します。 つまり、"Scott"のようなユーザー名を作成するか、Scott や Scott.1234 のようなユーザー名/パスワードの組み合わせからようにします。

イベント ハンドラーを作成しますがこれを実現する、`CreatingUser`追加の検証チェックを実行するイベントです。 指定されたデータが有効でない場合、作成プロセスをキャンセルする必要があります。 また、ユーザー名またはパスワードが無効であるかを説明するメッセージを表示するページに、ラベルの Web コントロールを追加する必要があります。 CreateUserWizard コントロールの設定の下にあるラベル コントロールを追加して、開始、`ID`プロパティを`InvalidUserNameOrPasswordMessage`とその`ForeColor`プロパティを`Red`します。 クリアしますその`Text`プロパティとその`EnableViewState`と`Visible`プロパティを False にします。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

CreateUserWizard コントロールのイベント ハンドラーを次に、作成`CreatingUser`イベント。 イベント ハンドラーを作成、デザイナーでコントロールを選択し、[プロパティ] ウィンドウに移動します。 そこから、稲妻のアイコンをクリックし、イベント ハンドラーを作成する適切なイベントをダブルクリックします。

`CreatingUser` イベント ハンドラーに次のコードを追加します。

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

ユーザー名と CreateUserWizard コントロールに入力したパスワードがを通じて使用可能なことに注意してください。 その[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)と[`Password`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)、それぞれします。 指定されたユーザー名が先頭または末尾のスペースを含むかどうかと、パスワードにユーザー名が見つかったかどうかを判断するのに上記のイベント ハンドラーでこれらのプロパティを使用します。 エラー メッセージが表示されるこれらの条件のいずれかが満たされる場合、`InvalidUserNameOrPasswordMessage`ラベルと、イベント ハンドラーの`e.Cancel`プロパティに設定されて`True`します。 場合`e.Cancel`に設定されている`True`CreateUserWizard を実行せずに、ワークフローでは、実質的に、ユーザー アカウントの作成プロセスをキャンセルします。

図 15 のスクリーン ショットを示しています。`CreatingUserAccounts.aspx`ユーザーが先頭のスペースを持つユーザー名を入力します。


[![先頭または末尾のスペースでのユーザー名は許可されていません](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**図 15**: 先頭または末尾のスペースでのユーザー名は許可されていません ([フルサイズの画像を表示する をクリックします](creating-user-accounts-vb/_static/image45.png))。


> [!NOTE]
> CreateUserWizard コントロールの使用例が表示されます`CreatedUser`内のイベント、  *<a id="_msoanchor_11"> </a>[追加ユーザーの情報を格納する](storing-additional-user-information-vb.md)* チュートリアル。


## <a name="summary"></a>まとめ

`Membership`クラスの`CreateUser`メソッドは、メンバーシップ フレームワークに新しいユーザー アカウントを作成します。 これは構成済みのメンバーシップ プロバイダーへの呼び出しを代行させることで。 場合、 `SqlMembershipProvider`、`CreateUser`メソッドは、レコードを追加、`aspnet_Users`と`aspnet_Membership`データベース テーブル。

プログラムで (に示した手順 5) は、新しいユーザーのアカウントを作成することができます、CreateUserWizard コントロールの使用が高速かつ容易の手法をします。 このコントロールは、ユーザー情報を収集し、メンバーシップ フレームワークで新しいユーザーを作成するための複数ステップのユーザー インターフェイスをレンダリングします。 このコントロールでは、実際には、下にある使用同じ`Membership.CreateUser`手順 5 では、コントロールに検査され、メソッドは、ユーザー インターフェイスの検証コントロールを作成し、まったくコードを記述することがなくユーザー アカウント作成エラーに応答します。

この時点で新しいユーザー アカウントを作成する機能があります。 ただし、引き続き、ログイン ページは 2 番目のチュートリアルに戻り指定されたハード コーディングされたその資格情報に対して検証です。 <a id="_msoanchor_12"> </a>[次のチュートリアル](validating-user-credentials-against-the-membership-user-store-vb.md)を更新して`Login.aspx`ユーザーを検証するメンバーシップ フレームワークに対して資格情報を提供します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [`CreateUser` 技術ドキュメント](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard コントロールの概要](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [ファイル システム ベースのサイト マップ プロバイダーを作成します。](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [ASP.NET 2.0 ウィザード コントロールのステップ バイ ステップのユーザー インターフェイスを作成します。](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [サイトのナビゲーションでの ASP.NET 2.0 の確認](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [マスター ページとサイト ナビゲーション](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [待っている SQL サイト マップ プロバイダー](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)します。

> [!div class="step-by-step"]
> [前へ](creating-the-membership-schema-in-sql-server-vb.md)
> [次へ](validating-user-credentials-against-the-membership-user-store-vb.md)
