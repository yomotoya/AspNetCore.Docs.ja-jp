---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: ユーザー アカウント (VB) の作成 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、フレームワークを使用して、メンバーシップ (SqlMembershipProvider) を使用して新しいユーザー アカウントを作成するをについて説明します。 新しい us を作成する方法が表示されます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: d665e7ba43401da76a88a904c10a587aa4576d4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892070"
---
<a name="creating-user-accounts-vb"></a>ユーザー アカウント (VB) の作成
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> このチュートリアルでは、フレームワークを使用して、メンバーシップ (SqlMembershipProvider) を使用して新しいユーザー アカウントを作成するをについて説明します。 新しいユーザーを作成し、ASP プログラムと方法が表示されます。NET の組み込みの CreateUserWizard コントロール。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a>[前のチュートリアル](creating-the-membership-schema-in-sql-server-vb.md)アプリケーション サービスのスキーマのデータベースで、追加のテーブル、ビュー、およびストアド プロシージャで必要なインストール、`SqlMembershipProvider`と`SqlRoleProvider`です。 これにより、このシリーズのチュートリアルの残りの必要がありますが、インフラストラクチャが作成されます。 このチュートリアルではについて学びますメンバーシップ フレームワークを使用して (を使用して、 `SqlMembershipProvider`) 新しいユーザー アカウントを作成します。 新しいユーザーを作成し、ASP プログラムと方法が表示されます。NET の組み込みの CreateUserWizard コントロール。

新しいユーザー アカウントを作成する方法を学習するには、だけでなくも必要になりますで最初に作成したデモの web サイトを更新、  *<a id="_msoanchor_2"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)* チュートリアルおよび強化し、  *<a id="_msoanchor_3"> </a>[フォーム認証の構成と高度なトピック](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* チュートリアルです。 デモ web アプリケーションには、ハード コーディングされたユーザー名とパスワードの組み合わせに対してユーザーの資格情報を検証するログイン ページがあります。 さらに、`Global.asax`カスタムを作成するコードが含まれています`IPrincipal`と`IIdentity`認証済みユーザーのオブジェクト。 メンバーシップ framework に対するユーザーの資格情報を検証し、プリンシパルと id のカスタム ロジックを削除するログイン ページを更新します。

開始しましょう!

## <a name="the-forms-authentication-and-membership-checklist"></a>フォーム認証とメンバーシップのチェックリスト

メンバーシップ framework での作業を始める前に、もう一度このポイントに達するに移動しました、重要な手順を確認してみましょう。 使用してメンバーシップ framework を使用する場合、`SqlMembershipProvider`フォーム ベース認証のシナリオで、次の手順は web アプリケーションにメンバーシップ機能を実装する前に実行する必要があります。

1. **フォーム ベース認証を有効にします。** 説明したよう *<a id="_msoanchor_4"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)*、編集してフォーム認証が有効になっている`Web.config`と設定、 `<authentication>`要素の`mode`属性を`Forms`です。 フォーム認証が有効になっている、受信要求ごとについて、*フォーム認証チケット*、存在する場合、識別、リクエスターです。
2. **アプリケーション サービスのスキーマを適切なデータベースに追加します。** 使用する場合、`SqlMembershipProvider`アプリケーション サービスのスキーマをデータベースにインストールする必要があります。 通常このスキーマは、アプリケーションのデータ モデルを保持するのと同じデータベースに追加されます。  *<a id="_msoanchor_5"> </a>[メンバーシップ スキーマを作成する SQL Server で](creating-the-membership-schema-in-sql-server-vb.md)* チュートリアルを使用して調べる、`aspnet_regsql.exe`これを実現するツールです。
3. **手順 2 からデータベースを参照する Web アプリケーションの設定をカスタマイズします。** *メンバーシップ スキーマを作成する SQL Server で*チュートリアルでは、web アプリケーションを構成する 2 つの方法を示しましたできるように、`SqlMembershipProvider`手順 2. で選択したデータベースを使用: 変更することによって、`LocalSqlServer`接続文字列名です。または手順 2 をフレームワークのメンバーシップ プロバイダーの一覧に新しい登録済みのプロバイダーを追加してからデータベースを使用して新しいプロバイダーをカスタマイズします。

Web アプリケーションの構築を使用する場合、`SqlMembershipProvider`フォーム ベース認証する必要がありますを使用する前にこれら 3 つの手順を実行して、`Membership`クラスや ASP.NET ログイン Web コントロールです。 前のチュートリアルで既にこれらの手順を実行してから、メンバーシップ framework の使用を開始する準備ができました!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新規の ASP.NET ページの追加

このチュートリアルで、次の 3 お検査各種のメンバーシップに関連する関数と機能します。 一連のトピックでは、これらのチュートリアル全体で検証を実装する ASP.NET ページが必要ですが。 これらのページと、サイト マップ ファイルを作成しましょう`(Web.sitemap)`です。

という名前のプロジェクトに新しいフォルダーを作成して開始`Membership`です。 次に 5 つの新しい ASP.NET ページを追加、`Membership`フォルダーで、各ページのリンク、`Site.master`マスター ページ。 ページの名前を付けます。

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

この時点で、プロジェクトのソリューション エクスプ ローラーはのスクリーン ショット、図 1 に示すようになります。


[![メンバーシップ フォルダーに 5 つの新しいページが追加されました](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**図 1**: 5 新しいページに追加されている、`Membership`フォルダー ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image3.png))


各ページ、この時点では 2 つのコンテンツ コントロールは、マスター ページの contentplaceholders にごとに 1 つ:`MainContent`と`LoginContent`です。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

注意してください、 `LoginContent` ContentPlaceHolder の既定のマークアップにログオンするか、いったんログオフして、ユーザーが認証されるかどうかに応じて、サイトからへのリンクが表示されます。 存在、`Content2`コンテンツ コントロール、ただし、マスター ページの既定のマークアップをオーバーライドします。 説明したよう *<a id="_msoanchor_6"> </a>[フォーム認証の概要を](../introduction/an-overview-of-forms-authentication-vb.md)* チュートリアルでは、これはページの左の列にログインに関連するオプションを表示しない場所おで役に立ちます。

これらの 5 ページでは、ただし、表示することのマスター ページの既定のマークアップ、 `LoginContent` ContentPlaceHolder です。 このため、削除の宣言型マークアップ、`Content2`コンテンツ コントロールです。 その後、1 つだけのコンテンツ コントロールを含めるそれぞれ 5 つのページのマークアップの必要があります。

## <a name="step-2-creating-the-site-map"></a>手順 2: サイト マップを作成します。

最も単純な web サイトを除くすべては、何らかの形式の移動ユーザー インターフェイスを実装する必要があります。 移動ユーザー インターフェイスは、サイトのさまざまなセクションへのリンクの単純なリストにあります。 また、これらのリンクは、メニューやツリー ビューに配置可能性があります。 開発者はページ、ナビゲーションのユーザー インターフェイスの作成は、ストーリーの半分だけです。 保守し、更新可能な方法で、サイトの論理構造を定義するなんらかの手段も必要です。 新しいページを追加または既存のページが削除される、ソースを更新する 1 つのサイト マップ - したいし、が、サイトのナビゲーションのユーザー インターフェイス間で、これらの変更が反映されます。

これら 2 つのタスク - サイト マップを定義して、サイト マップに基づくナビゲーションのユーザー インターフェイスを実装するは簡単に、サイト マップ framework 感謝実行し、ナビゲーション Web が追加されたで ASP.NET version 2.0 を制御します。 サイト マップを定義する開発者向けサイト マップ フレームワークを利用して、そのプログラムの API を使用してアクセスするために (、 [ `SiteMap`クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx))。 組み込みナビゲーション Web コントロールにはが含まれて、[メニュー コントロール](https://msdn.microsoft.com/library/bz09dy46.aspx)、 [TreeView コントロール](https://msdn.microsoft.com/library/3eafky27.aspx)、および[サイト マップ パス コントロール](https://msdn.microsoft.com/library/3eafky27.aspx)です。

メンバーシップとロールのフレームワークと同様に、サイト マップ framework は上に構築される、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)です。 サイト マップ プロバイダー クラスのジョブがによって使用されるメモリ内構造を作成するには、 `SiteMap` XML ファイルまたはデータベース テーブルなどの永続的なデータ ストアからのクラスです。 .NET Framework がデータを読み取り、サイト マップ XML ファイルから既定のサイト マップ プロバイダーが付属しています ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx))、これは、プロバイダーは、このチュートリアルで使用します。 代替サイト マップ プロバイダー実装によって、このチュートリアルの最後に、さらに測定値のセクションを参照してください。

既定のサイト マップ プロバイダーには、適切にフォーマットされた XML という名前のファイルが必要ですが`Web.sitemap`ルート ディレクトリが存在します。 この既定のプロバイダーを使用していることからは、このようなファイルを追加し、適切な XML 形式で、サイト マップの構造を定義する必要があります。 ファイルを追加するには、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 サイト マップという名前の種類のファイルを追加することを選択 ダイアログ ボックスから`Web.sitemap`です。


[![プロジェクトのルート ディレクトリに Web.sitemap をという名前のファイルを追加します。](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**図 2**: ファイルの名前を追加`Web.sitemap`プロジェクトのルート ディレクトリに ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image6.png))


XML サイト マップ ファイルは、階層構造として、web サイトの構造を定義します。 この階層関係がの先祖を使用して XML ファイルでモデル化されて、`<siteMapNode>`要素。 `Web.sitemap`始める必要があります、`<siteMap>`を正確に 1 つの親ノード`<siteMapNode>`子。 この最上位`<siteMapNode>`要素が、階層のルートを表し、子孫のノードの任意の数を含めることがあります。 各`<siteMapNode>`要素を含める必要があります、`title`属性を含めることができます必要に応じて`url`と`description`; 他の属性ごと、空でない`url`属性は一意である必要があります。

次の XML を入力、`Web.sitemap`ファイル。

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

上記のサイト マップ マークアップでは、図 3 に示すように階層を定義します。


[![サイト マップが階層ナビゲーション構造体を表します](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**図 3**: サイト マップが階層ナビゲーション構造体を表します ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>手順 3: 移動ユーザー インターフェイスを挿入するマスター ページを更新します。

ASP.NET には、ユーザー インターフェイスを設計するためのナビゲーション関連の Web コントロールの数が含まれています。 これらには、メニューの TreeView、およびサイト マップ パス コントロールが含まれます。 メニューを TreeView のコントロールは、サイト マップ パスには、その先祖と参照される現在のノードを示す階層リンクが表示されますが、それぞれメニューや、ツリー内のサイト マップ構造をレンダリングします。 サイト マップ データが Web コントロール、SiteMapDataSource を使用してその他のデータにバインドでき、経由でプログラムからアクセスできる、`SiteMap`クラスです。

サイト マップ フレームワークとナビゲーション コントロールの完全な詳細については、このチュートリアル シリーズの範囲外ですが、以降ではなくみましょう独自移動ユーザー インターフェイスの作成時間をかけて代わりに借用で使用される my  *[ASP.NET 2.0 のデータを扱う](../../data-access/index.md)* チュートリアル シリーズは、図 4 に示すように、ナビゲーション リンクの 2 つが深い箇条書きリストを表示する Repeater コントロールを使用します。

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>左の列に 2 つのレベルのリンクの一覧を追加します。

このインターフェイスを作成するには、次に示す宣言型マークアップを追加、`Site.master`マスター ページの左の列をテキスト TODO:] メニューの [ここに表示されます。 現在あります。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

上記のマークアップがという名前のリピータ コントロールをバインド`menu`で定義されているサイト マップの階層が返されます、SiteMapDataSource に`Web.sitemap`です。 以降、SiteMapDataSource コントロールの[`ShowStartingNode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)が開始以降、[ホーム] ノードの子孫で、サイト マップの階層を返す False に設定します。 リピータのそれぞれ表示これらのノード (現在だけメンバーシップ)、`<li>`要素。 Repeater を別の内部では、入れ子になった順序なしリストで現在のノードの子が表示されます。

図 4 は、手順 2. で作成したサイト マップ構造を持つ、上記のマークアップの表示される出力を示します。 リピータ マークアップ バニラ順序なしリストです。定義されているカスケード スタイル シート規則`Styles.css`見た目心地よいレイアウトを担当します。 上記のマークアップのしくみの詳細についてを参照してください、[マスター ページとサイト ナビゲーション](https://asp.net/learn/data-access/tutorial-03-vb.aspx)チュートリアルです。


[![ナビゲーションのユーザー インターフェイスが表示されるリストを使用して入れ子になった順序なし](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**図 4**: ナビゲーションのユーザー インターフェイスが表示されるリストを使用して入れ子になった順序付けられていない ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>階層リンク バー ナビゲーションを追加します。

左の列内のリンクの一覧、に加えてみましょうもがある各ページの 表示、[階層リンク](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)です。 階層リンクは、すぐに、サイト階層内の現在位置をユーザーに表示されるナビゲーションのユーザー インターフェイス要素です。 サイト マップ パス コントロールは、サイト マップ内の現在のページの場所を特定する、サイト マップ フレームワークを使用し、し、この情報に基づいて階層リンク バーを表示します。

具体的には、追加、`<span>`要素、マスター ページのヘッダーを`<div>`要素、し、新しい設定`<span>`要素の`class`属性階層にします。 (、`Styles.css`クラスには、階層リンク クラスのルールが含まれています)。次に、この新規作成 をサイト マップ パスを追加`<span>`要素。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

図 5 にアクセスしたときに、サイト マップ パスの出力を示しています`~/Membership/CreatingUserAccounts.aspx`です。


[![階層リンクには、現在のページが表示されます。 および、サイトでは、その先祖のマップ](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**図 5**: 階層リンク、サイト マップでの現在のページとその先祖の表示 ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>手順 4: カスタム プリンシパルと Id ロジックの削除

 *<a id="_msoanchor_7"> </a>[フォーム認証の構成と高度なトピック](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* チュートリアルに認証されたユーザーにカスタム プリンシパルと id オブジェクトを関連付ける方法を説明しました。 内のイベント ハンドラーを作成することでこれを実現お`Global.asax`のアプリケーションの`PostAuthenticateRequest`後に発生するイベント、`FormsAuthenticationModule`がユーザーを認証します。 このイベント ハンドラーで置き換えました、`GenericPrincipal`と`FormsIdentity`によって追加されたオブジェクト、`FormsAuthenticationModule`で、`CustomPrincipal`と`CustomIdentity`そのチュートリアルで作成したオブジェクトします。

カスタム プリンシパル オブジェクトと id オブジェクトはほとんどの場合、特定のシナリオで役に立ちますが、`GenericPrincipal`と`FormsIdentity`オブジェクトで十分です。 したがって、既定の動作に戻るには価値のあることと思います。 削除するか、コメント アウトすることによってこの変更を行う、`PostAuthenticateRequest`イベント ハンドラーまたは削除することによって、`Global.asax`完全ファイルします。

> [!NOTE]
> コメントまたは内のコードを削除した後`Global.asax`でコードをコメント アウトする必要があります`Default.aspx's`キャストする分離コード クラス、`User.Identity`プロパティを`CustomIdentity`インスタンス。


## <a name="step-5-programmatically-creating-a-new-user"></a>手順 5: 新しいユーザーをプログラムで作成します。

メンバーシップ フレームワークを使用して新しいユーザー アカウントを作成する、`Membership`クラスの[`CreateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)です。 このメソッドには、ユーザー名、パスワード、およびその他のユーザーに関連するフィールドのパラメーターに入力します。 呼び出しでは、上の 構成済みのメンバーシップ プロバイダーを新しいユーザー アカウントの作成を委任し、返しますが、、 [ `MembershipUser`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)だけで作成されたユーザー アカウントを表すです。

`CreateUser`メソッドが 4 つのオーバー ロードが、異なる数の入力パラメーターを入力します。

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

これら 4 つのオーバー ロードは、収集される情報の量によって異なります。 最初のオーバー ロードでは、たとえばが必要がユーザー名とパスワードのみ、新しいユーザー アカウントの 2 つ目では、ユーザーの電子メール アドレスも必要です。

新しいユーザー アカウントの作成に必要な情報が、メンバーシップ プロバイダーの構成設定に依存しているために、これらのオーバー ロードが存在します。  *<a id="_msoanchor_8"> </a>[メンバーシップ スキーマを作成する SQL Server で](creating-the-membership-schema-in-sql-server-vb.md)* でメンバーシップ プロバイダーの構成設定の指定を調べるおチュートリアル`Web.config`です。 表 2 には、構成設定の完全な一覧が含まれています。

1 つそのようなメンバーシップ プロバイダーの構成設定にどのような影響を与えます`CreateUser`オーバー ロードを使用することがありますが、`requiresQuestionAndAnswer`設定します。 場合`requiresQuestionAndAnswer`に設定されている`true`(既定)、し、新しいユーザー アカウントを作成するときに、セキュリティの質問と回答を指定お必要があります。 この情報は後で、ユーザーをリセットまたはパスワードを変更する必要がある場合に使用します。 具体的には、その時点でのセキュリティの質問を表示されているとリセットまたはパスワードを変更するために正しい解答を入力する必要があります。 したがって場合、`requiresQuestionAndAnswer`に設定されている`true`最初の 2 つのいずれかを呼び出して、`CreateUser`のセキュリティの質問と回答が見つからないため、例外の結果をオーバー ロードします。 アプリケーションがセキュリティの質問と解答を要求するように現在構成されているためにはユーザーをプログラムで作成するときに、後者の 2 つのオーバー ロードのいずれかを使用する必要があります。

使用して説明するために、`CreateUser`メソッド、ユーザーに、名前、パスワード、電子メール、および定義済みのセキュリティの質問に対する回答を求めるおユーザー インターフェイスを作成してみましょう。 開く、 `CreatingUserAccounts.aspx`  ページで、`Membership`フォルダー コンテンツ コントロールに次の Web コントロールを追加。

- という名前のテキスト ボックス `Username`
- という名前のテキスト ボックス`Password`が`TextMode`プロパティに設定 `Password`
- という名前のテキスト ボックス `Email`
- という名前のラベル`SecurityQuestion`でその`Text`プロパティをクリア
- という名前のテキスト ボックス `SecurityAnswer`
- という名前のボタン`CreateAccountButton`が`Text`プロパティに設定を作成するユーザー アカウント
- という名前のラベル コントロール`CreateAccountResults`でその`Text`プロパティをクリア

この時点で、画面は、スクリーン ショット図 6 に示すようになります。


[![CreatingUserAccounts.aspx ページにさまざまな Web コントロールを追加します。](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**図 6**: さまざまな Web コントロールを追加、 `CreatingUserAccounts.aspx Page` ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion`ラベルと`SecurityAnswer` ボックスには定義済みのセキュリティの質問を表示し、ユーザーの応答を収集するためのものです。 独自のセキュリティの質問を定義するには、各ユーザーを許可することはユーザー単位のユーザーごとにセキュリティの質問と回答の両方が格納されていることに注意してください。 ただし、つまり、ユニバーサル セキュリティ質問を使用することにしましたが、この例の場合: 好きな色は何ですか。

この定義済みのセキュリティの質問を実装するのには、という名前のページの分離コード クラスに定数を追加`passwordQuestion`セキュリティの質問を割り当てることです。 次に、`Page_Load`イベント ハンドラー、割り当てにこの定数、`SecurityQuestion`ラベルの`Text`プロパティ。

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

イベント ハンドラーを次に、作成、 `CreateAccountButton'` s`Click`イベントし、次のコードを追加します。

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click`という名前の変数を定義してイベント ハンドラーを起動`createStatus`型の[ `MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx)です。 `MembershipCreateStatus` 状態を示す列挙体には、`CreateUser`操作します。 たとえば、ユーザー アカウントが正常に作成、表示された`MembershipCreateStatus`インスタンスの値に設定されます`Success;`一方で、同じユーザー名を持つユーザーが既に存在するために、操作が失敗した場合に設定されますの値`DuplicateUserName`. `CreateUser`使用のオーバー ロードに渡す必要があります、`MembershipCreateStatus`インスタンス メソッドにします。 このパラメーターは内で適切な値に設定されて、`CreateUser`メソッド、およびおは、ユーザー アカウントが正常に作成されたかどうかを決定するメソッドの呼び出し後の値を調べることができます。

呼び出した後`CreateUser`を渡して、 `createStatus`、`Select Case`に割り当てられている値に応じて、適切なメッセージを出力するステートメントを使用`createStatus`です。 図 7 は、新しいユーザーが正常に作成されたときに出力を示します。 図 8 と 9 は、ユーザー アカウントを作成していない場合、出力を表示します。 図 8 には、訪問者は、メンバーシップ プロバイダーの構成設定に記述されたパスワードの強度の要件を満たしていない、5 文字のパスワードを入力します。 図 9 に訪問者は既存のユーザー名 (図 7 に作成されたもの) でユーザー アカウントを作成しようとしています。


[![新しいユーザー アカウントが正常に作成されました](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**図 7**:、新しいユーザー アカウントが正常に作成されました ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image21.png))


[![指定のパスワードは脆弱すぎるために、ユーザー アカウントは作成されません。](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**図 8**: 指定されたパスワードが脆弱すぎますため、ユーザー アカウントは作成されません ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image24.png))


[![ユーザー アカウントが作成されたため、ユーザー名は既に使用中](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**図 9**: ユーザー アカウントが作成されたため、ユーザー名は既に使用中 ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> 最初の 2 つのいずれかを使用するときに、成功または失敗を判断する方法の思う`CreateUser`もメソッドがオーバー ロードの型のパラメーターを持つ`MembershipCreateStatus`です。 これらの最初の 2 つのオーバー ロードをスロー、 [ `MembershipCreateUserException`例外](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx)を含む、障害発生した場合に、 [ `StatusCode`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx)型の`MembershipCreateStatus`します。


少数のユーザー アカウントを作成した後の内容を一覧表示して、アカウントが作成されたことを確認してください、`aspnet_Users`と`aspnet_Membership`内のテーブル、`SecurityTutorials.mdf`データベース。 図 10 に示す経由で 2 つのユーザーを追加した、`CreatingUserAccounts.aspx`ページ: Tito としました。


[![メンバーシップ ユーザーのストアに 2 人のユーザーがある: Tito とブルース](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**図 10**: メンバーシップ ユーザーのストアに 2 人のユーザーがある: Tito とブルース ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image30.png))


メンバーシップ ユーザーのストアは今すぐ、ブルースと Tito のアカウントの情報が含まれていますが、まだを実装しなければならなくブルースまたは Tito サイトにログオンできるようにする機能。 現時点では、`Login.aspx`ユーザーの資格情報を検証は 1 組のユーザー名/パスワードのハードコーディング セットに対して*いない*メンバーシップ フレームワークに対して指定された資格情報を検証します。 今すぐに新しいユーザー アカウントを表示するため、`aspnet_Users`と`aspnet_Membership`で十分にテーブルになります。 チュートリアルでは、[次へ]、  *<a id="_msoanchor_9"> </a>[を検証するユーザー資格情報に対して、メンバーシップ ユーザー ストア](validating-user-credentials-against-the-membership-user-store-vb.md)*、メンバーシップ ストアに対して検証へのログイン ページを更新します。

> [!NOTE]
> すべてのユーザーが表示されない場合、`SecurityTutorials.mdf`データベースである可能性があります、web アプリケーションが既定のメンバーシップ プロバイダーを使用しているため`AspNetSqlMembershipProvider`が使用される、`ASPNETDB.mdf`ユーザー ストアとしてのデータベースです。 この問題を特定するのには、ソリューション エクスプ ローラーで [更新] ボタンをクリックします。 という名前のデータベース`ASPNETDB.mdf`に追加された、`App_Data`フォルダーで、これが問題です。 手順 4 に戻り、  *<a id="_msoanchor_10"> </a>[メンバーシップ スキーマを作成する SQL Server で](creating-the-membership-schema-in-sql-server-vb.md)* 手順については、メンバーシップ プロバイダーを正しく構成する方法のチュートリアルです。


ほとんどのユーザー アカウントのシナリオを作成、ユーザー名、パスワード、電子メール、および新しいアカウントを作成する時点で、必要なその他の情報を入力するいくつかのインターフェイスの訪問者が表示されます。 このステップでおを手動でこのようなインターフェイスの構築に検査しを使用する方法を説明し、`Membership.CreateUser`をプログラムで新しいユーザー アカウントを追加する方法、ユーザーの入力を基にします。 このコードは、新しいユーザー アカウントを作成しました。 補足情報だけが作成したユーザー アカウントでは、サイトにユーザーのログインまたはユーザーに確認の電子メールを送信するなどの操作は実行しませんでした。 追加の手順で、ボタンのコードを追加が必要になります`Click`イベント ハンドラー。

ASP.NET は CreateUserWizard コントロールでは、メンバーシップ フレームワークで、アカウントを作成し、後のアカウントを実行すること、新しいユーザー アカウントを作成するためのユーザー インターフェイスの表示から、ユーザーのアカウントの作成プロセスを処理するものでは付属しています作成などのタスク、確認の電子メールを送信するだけで作成されたユーザーをサイトにログインします。 CreateUserWizard コントロールを使用するは、ページには、ツールボックスから CreateUserWizard コントロールをドラッグして、いくつかのプロパティを設定と同じくらい簡単です。 ほとんどの場合、1 行のコードを記述する必要はありません。 手順 6 で詳細に高度なこのコントロールについて学びます。

新しいユーザー アカウントは、一般的なアカウントを作成する web ページを介してのみ作成される場合、可能性は高くありませんを使用するコードを記述する必要がこれまで、`CreateUser`メソッド、CreateUserWizard コントロールとして可能性がありますニーズを満たします。 ただし、`CreateUser`詳細にカスタマイズされたアカウントを作成するユーザー エクスペリエンスをする必要があるか、別のインターフェイスから新しいユーザー アカウントをプログラムで作成する必要がある場合は、メソッドはシナリオに便利です。 たとえば、ユーザーが他のアプリケーションからユーザー情報を含む XML ファイルをアップロードできるページがあるとします。 ページが解析してアップロードされた XML の内容をファイルして呼び出すことによって、XML で表されるユーザーごとに新しいアカウントを作成、`CreateUser`メソッドです。

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>手順 6: CreateUserWizard コントロールで、新しいユーザーを作成します。

ASP.NET は、ログイン Web コントロールの数が付属しています。 これらのコントロールは、一般的なユーザー アカウントおよびログインに関連する様々 な構成を支援します。 [CreateUserWizard コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)メンバーシップ フレームワークに新しいユーザー アカウントを追加するためのユーザー インターフェイスを提供するものでは、このような 1 つのコントロールがします。

その他のログインに関連する Web コントロールの多くのように 1 行のコードを記述することがなく、CreateUserWizard を使用できます。 メンバーシップ プロバイダーの構成設定と、内部での呼び出しに基づくユーザー インターフェイスを直感的に提供、`Membership`クラスの`CreateUser`後に、ユーザーが必要な情報を入力し、ユーザーの作成 ボタンをクリックします。 CreateUserWizard コントロールでは、非常にカスタマイズできます。 アカウントの作成プロセスのさまざまなステージ中に発生するイベントのホストがあります。 アカウントの作成ワークフローにカスタム ロジックを挿入する、必要に応じて、イベント ハンドラーを作成できます。 さらに、CreateUserWizard の外観は、非常に柔軟性があります。 既定のインターフェイスの外観を定義するプロパティの数があります。必要に応じて、コントロールをテンプレートに変換することができますか、追加のユーザーの登録ステップを追加することがあります。

CreateUserWizard コントロールの既定のインターフェイスと動作を使用して見てから始めましょう。 コントロールのプロパティおよびイベントを使用して外観をカスタマイズする方法から見ていきましょう。

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>CreateUserWizard の既定のインターフェイスと動作の確認

戻り、 `CreatingUserAccounts.aspx`  ページで、`Membership`フォルダーで、デザインまたは分割モードに切り替えるし、ページの上部に CreateUserWizard コントロールを追加します。 ツールボックスのログイン コントロール セクションをファイリング CreateUserWizard コントロール。 コントロールを追加すると、次のように設定します。 その`ID`プロパティを`RegisterUser`です。 画面は、図 11 と、CreateUserWizard は、新しいユーザーのユーザー名、パスワード、電子メール アドレス、およびセキュリティの質問および回答のテキスト ボックスを持つインターフェイスをレンダリングします。


[![CreateUserWizard コントロール レンダリング汎用的なユーザー インターフェイスを作成します。](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**図 11**: CreateUserWizard コントロールは、汎用的な作成ユーザー インターフェイスをレンダリング ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image33.png))


手順 5 で作成したインターフェイスを持つ CreateUserWizard コントロールによって生成される既定のユーザー インターフェイスを比較する時点を見てみましょう。 まず、CreateUserWizard コントロールによりセキュリティの質問と回答の両方を指定するビジター、一方、手動で作成されたインターフェイスは、定義済みのセキュリティの質問を使用します。 インターフェイスのフォームのフィールドに検証を実装するまだがありましたが、CreateUserWizard コントロールのインターフェイスにはからも検証コントロールが含まれます。 および CreateUserWizard コントロール インターフェイスには (と共にテキスト パスワードを入力して、パスワードの比較のテキスト ボックスが等しいことを確認 CompareValidator) パスワードの確認入力 ボックスが含まれています。

興味深いは、ユーザー インターフェイスを表示するときは CreateUserWizard コントロールがメンバーシップ プロバイダーの構成設定を参照しているです。 たとえば、セキュリティの質問と回答のテキスト ボックスが表示される場合のみ`requiresQuestionAndAnswer`が True に設定します。 CreateUserWizard が自動的に設定パスワードの強度の要件が満たされていることを確認する RegularExpressionValidator コントロールを追加同様に、その`ErrorMessage`と`ValidationExpression`プロパティに基づいて、 `minRequiredPasswordLength`、 `minRequiredNonalphanumericCharacters`、および`passwordStrengthRegularExpression`構成設定。

CreateUserWizard コントロールでは、その名前からわかるようにはから派生した、[ウィザード コントロール](https://msdn.microsoft.com/library/s2etd1ek.aspx)です。 ウィザードのコントロールは、インターフェイスを提供する、マルチ ステップ タスクを完了するために設計されています。 ウィザード コントロールでの任意の数を含めることがあります`WizardSteps`それぞれが、HTML を定義するテンプレート、および Web を制御する手順。 ウィザード コントロールは、最初の初期状態が表示されます。 `WizardStep`、ナビゲーション コントロールを次に、1 つのステップから続行するか、前の手順に戻るには、ユーザーを許可するとします。

図 11 では宣言型マークアップを示しています、CreateUserWizard コントロールの既定のインターフェイスが用意されています 2 `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? 新しいユーザー アカウントを作成するための情報を収集するためのインターフェイスを表示します。 これは、図 11 に示されたステップです。
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? アカウントが正常に作成されたことを示すメッセージを表示します。

これらの手順のいずれかのテンプレートを変換することで、または独自の追加によって、CreateUserWizard の外観と動作を変更できます`WizardStep`s。 追加することを見ていきましょう、`WizardStep`で登録インターフェイスを*追加のユーザー情報を格納する*チュートリアルです。

CreateUserWizard コントロールが動作を確認してみましょう。 参照してください、`CreatingUserAccounts.aspx`ページがブラウザーを使用します。 CreateUserWizard のインターフェイスにいくつかの無効な値を入力して起動します。 再試行パスワードの強度要件に準拠していないパスワードを入力して、またはユーザー名 ボックスを空のままです。 CreateUserWizard 適切なエラー メッセージが表示されます。 図 12 は、条件が不十分な強力なパスワードを使用してユーザーを作成しようとするときに出力を示します。


[![CreateUserWizard 検証コントロールを自動的に挿入します。](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**図 12**:「CreateUserWizard 自動的挿入の検証コントロール ([フルサイズ イメージを表示するに、をクリックして](creating-user-accounts-vb/_static/image36.png))


次は CreateUserWizard に適切な値を入力し、ユーザーの作成 ボタンをクリックします。 必要なフィールドが入力されているし、パスワードの強度が十分でと仮定した場合は CreateUserWizard されメンバーシップ フレームワークを通じて、新しいユーザー アカウントを作成、表示されます、`CompleteWizardStep`のインターフェイスを (図 13 を参照してください)。 バック グラウンドで、CreateUserWizard を呼び出す、`Membership.CreateUser`メソッド、手順 5. で行ったのと同じようにします。


[![新しいユーザー アカウントが正常に作成されました](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**図 13**:、新しいユーザー アカウントが正常に作成された ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> 図 13 に示す、`CompleteWizardStep`のインターフェイスには、[続行] ボタンが含まれています。 ただし、この時点でクリックすると、単にすると、同じページに訪問者を残したまま、ポストバックを実行します。 カスタマイズ CreateUserWizard の外観と動作をそのプロパティ セクションでは、訪問者の送信は、このボタンのある方法について見て`Default.aspx`(またはその他のいくつかのページ)。


新しいユーザー アカウントを作成すると、Visual Studio に戻り、確認、`aspnet_Users`と`aspnet_Membership`テーブルをアカウントが正常に作成されたことを確認する図 10 で行ったようにします。

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>CreateUserWizard の動作とそのプロパティを使用した外観をカスタマイズします。

さまざまなプロパティを使用の方法でカスタマイズする、CreateUserWizard`WizardStep`秒、およびイベント ハンドラー。 このセクションの内容を見てみましょうのプロパティを使用したコントロールの外観をカスタマイズする方法次のセクションでは、イベント ハンドラーからコントロールの動作を拡張する、します。

CreateUserWizard コントロールの既定のユーザー インターフェイスに表示されるテキストのほぼすべて、多種多様なプロパティをカスタマイズできます。 テキスト ボックスの左側に表示されるユーザー名、パスワード、パスワードの確認、電子メール、セキュリティの質問、およびセキュリティの回答のラベルをカスタマイズするなど、 [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx)、 [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx)、 [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx)、 [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx)、 [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)、および[ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)プロパティは、それぞれします。 同様に、ユーザーを作成し、続行のボタンのテキストを指定するためのプロパティがある、`CreateUserWizardStep`と`CompleteWizardStep`にもこれらのボタンは、ボタン、ある、または ImageButtons としてレンダリングされます。

色、罫線、フォント、および他のビジュアル要素は、スタイル プロパティのホストを構成できます。 CreateUserWizard コントロール自体が、一般的な Web コントロール スタイルのプロパティ - `BackColor`、 `BorderStyle`、 `CssClass`、`Font`というようの特定のセクションの外観を定義するスタイル プロパティの数があると、CreateUserWizard のインターフェイスです。 [ `TextBoxStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)のインスタンスのテキスト ボックスのスタイルを定義、`CreateUserWizardStep`中、 [ `TitleTextStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx)(へのサインアップ、新しいタイトルのスタイルを定義しますアカウント)。

外観に関連するプロパティに加えてには、さまざまな CreateUserWizard コントロールの動作に影響するプロパティがあります。 [ `DisplayCancelButton`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)場合、true の場合、表示する (既定値は False)、ユーザーの作成ボタンの横の [キャンセル] ボタンを設定します。 [キャンセル] ボタンを表示する場合にも設定して、 [ `CancelDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)ユーザーは [キャンセル] をクリックした後に送信されるページを指定します。 [続行] ボタン、前のセクションで説明したとおり、`CompleteWizardStep`のインターフェイスは、ポストバックを発生させるが、同じページに訪問者のままにします。 [続行] ボタンをクリックした後、その他のいくつかのページに訪問者を送信するで URL を指定するだけ、 [ `ContinueDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)です。

説明を更新、 `RegisterUser` CreateUserWizard コントロールを [キャンセル] ボタンを表示して、訪問者を送信する`Default.aspx`[キャンセル] または [続行] ボタンがクリックされたとき。 これを実現する、次のように設定します。、`DisplayCancelButton`プロパティを True にあり、両方は、`CancelDestinationPageUrl`と`ContinueDestinationPageUrl`プロパティ ~/Default.aspx です。 図 14 では、ブラウザーで表示したときに、更新された CreateUserWizard を示します。


[![CreateUserWizardStep には、[キャンセル] ボタンが含まれています。](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**図 14**: `CreateUserWizardStep` [キャンセル] ボタンが含まれています ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image42.png))


訪問者は、ユーザー名、パスワード、電子メール アドレス、およびセキュリティの質問と解答を入力し、ユーザーの作成をクリックすると、新しいユーザー アカウントが作成され、訪問者が新しく作成されたユーザーとしてログインしています。 ページにアクセスするユーザーが自分の新しいアカウントを作成すると仮定すると、これが目的の動作である可能性がありますです。 ただし、新しいユーザー アカウントを追加する管理者を許可することがあります。 これにより、ユーザー アカウントが作成されますが、管理者はまま残り、ログインしている、管理者として (新しく作成したアカウントではなく)。 この動作は、ブール値を変更できます[`LoginCreatedUser`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)です。

メンバーシップ フレームワーク内のユーザー アカウントを含む承認済みのフラグです。承認されていないユーザーが、サイトにログインできません。 既定では、新しく作成したアカウントは承認されると、ユーザーがすぐに、サイトにログインできるようにマークされます。 ただし、承認されていないとしてマークされている新しいユーザー アカウントを持っていることです。 管理者は; でログインすることが、新しいユーザーを手動で承認する必要があります。または、ユーザーがログオンを許可する前に、サインアップ時に入力した電子メール アドレスが有効であることを確認する可能性があります。 これは、場合でも、作成できる、新しく作成したユーザー アカウントは CreateUserWizard コントロールの設定によって、承認されていないとマーク[`DisableCreatedUser`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)True (既定値は False) にします。

ノートの他の動作に関連するプロパティには、`AutoGeneratePassword`と`MailDefinition`です。 場合、 [ `AutoGeneratePassword`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx)を True に設定されている、`CreateUserWizardStep`パスワードとパスワードの確認テキスト ボックスで; には表示されません代わりに、新しく作成したユーザーのパスワードが自動的に生成を使用して、 `Membership`クラスの[`GeneratePassword`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)です。 `GeneratePassword`メソッドは、構成されたパスワードの強度の要件を満たすために英数字以外の文字のための十分な数と指定された長さのパスワードを構築します。

[ `MailDefinition`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)アカウントの作成プロセス中に指定した電子メール アドレスに電子メールを送信する場合に便利です。 `MailDefinition`プロパティには、構築された電子メール メッセージに関する情報を定義するためのサブプロパティのシリーズが含まれています。 これらのサブプロパティがなどのオプションを含める`Subject`、 `Priority`、 `IsBodyHtml`、 `From`、 `CC`、および`BodyFileName`です。 [ `BodyFileName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)テキスト ファイルまたは電子メール メッセージの本文を含む HTML ファイルをポイントします。 本文は、事前に定義された 2 つのプレース ホルダーをサポートしています:`<%UserName%>`と`<%Password%>`です。 これらのプレース ホルダーに存在する場合、`BodyFileName`ファイルでは、だけが作成したユーザーの名前とパスワードに置き換えられます。

> [!NOTE]
> `CreateUserWizard`コントロールの`MailDefinition`プロパティには、新しいアカウントの作成時に送信される電子メール メッセージの詳細だけを指定します。 電子メール メッセージを実際に送信する方法については含まれません (つまり、SMTP サーバーまたはメール ドロップ ディレクトリが使用されているか、すべて認証情報、およびなど)。 これらの低レベルの詳細で定義されている必要があります、 `<system.net>` 」の「`Web.config`です。 これらの構成設定と ASP.NET 2.0 から電子メールを送信すると、一般の詳細についてを参照してください、 [SystemNetMail.com でよく寄せられる質問](http://www.systemnetmail.com/)と私の記事[ASP.NET 2.0 で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)です。


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>イベント ハンドラーを使用して CreateUserWizard の動作を拡張します。

CreateUserWizard コントロールでは、そのワークフロー内で複数のイベントを発生させます。 たとえば、訪問者は、ユーザー名、パスワード、および他の関連情報を入力したユーザーの作成 ボタンをクリックしたら、CreateUserWizard コントロールが発生させます。 その[`CreatingUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)です。 作成プロセス中に問題がある場合、 [ `CreateUserError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)が発生します。 ただし、ユーザーが正常に作成される場合は、次に、 [ `CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)が発生します。 生成されるその他の CreateUserWizard コントロール イベントが、これらは、次の 3 つの最も密接もの。

特定のシナリオで、適切なイベントのイベント ハンドラーを作成することで行うには CreateUserWizard のワークフローをタップすることもできます。 これを示すためを拡張してみましょう、 `RegisterUser` CreateUserWizard コントロール ユーザー名とパスワードをいくつかのカスタム検証を追加します。 具体的には、みましょう、CreateUserWizard するように増やしますユーザー名は、先頭または末尾のスペースを含めることはできませんし、ユーザー名に含めることが任意の場所のパスワード。 つまり、他のユーザーが"Scott"のようなユーザー名を作成または Scott や Scott.1234 のようなユーザー名/パスワードの組み合わせを阻止します。

イベント ハンドラーを作成、これを実現する、`CreatingUser`余分な検証チェックを実行するイベントです。 提供されたデータが有効でない場合、作成プロセスをキャンセルする必要があります。 また、ラベルの Web コントロールにユーザー名またはパスワードが無効であるかを説明するメッセージを表示するページを追加する必要があります。 設定、CreateUserWizard コントロールの下にラベル コントロールを追加して、開始、`ID`プロパティを`InvalidUserNameOrPasswordMessage`とその`ForeColor`プロパティを`Red`です。 クリアしますその`Text`プロパティとその`EnableViewState`と`Visible`プロパティを False にします。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

CreateUserWizard コントロールのイベント ハンドラーを次に、作成`CreatingUser`イベント。 作成し、イベント ハンドラー、デザイナーでコントロールを選択し、[プロパティ] ウィンドウに移動します。 そこから、稲妻のアイコンをクリックし、イベント ハンドラーを作成する適切なイベントをダブルクリックします。

`CreatingUser` イベント ハンドラーに次のコードを追加します。

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

ユーザー名と CreateUserWizard コントロールに入力したパスワードがを通じて利用できることに注意してください。 その[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)と[`Password`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)、それぞれします。 おプロパティを使用して、上記のイベント ハンドラーで指定されたユーザー名に先頭または末尾のスペースが含まれるかどうかと、ユーザー名をパスワードで見つかったかどうかを決定します。 エラー メッセージが表示されます両方の条件が満たされた場合、`InvalidUserNameOrPasswordMessage`ラベルと、イベント ハンドラーの`e.Cancel`プロパティに設定されている`True`です。 場合`e.Cancel`に設定されている`True`、CreateUserWizard short-circuits、ワークフローでは、効果的にユーザー アカウントの作成プロセスをキャンセルします。

図 15 のスクリーン ショットを示しています`CreatingUserAccounts.aspx`ユーザーが先頭のスペースを持つユーザー名を入力します。


[![先頭または末尾のスペースでのユーザー名は許可されていません](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**図 15**: 先頭または末尾のスペースでのユーザー名は許可されていません ([フルサイズのイメージを表示するをクリックして](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> CreateUserWizard コントロールの使用例を見ていきます`CreatedUser`内のイベント、  *<a id="_msoanchor_11"> </a>[追加のユーザー情報を格納する](storing-additional-user-information-vb.md)* チュートリアルです。


## <a name="summary"></a>まとめ

`Membership`クラスの`CreateUser`メソッドはメンバーシップ フレームワークで、新しいユーザー アカウントを作成します。 それは、構成済みのメンバーシップ プロバイダーへの呼び出しを代行させることで。 場合、 `SqlMembershipProvider`、`CreateUser`メソッドを追加するレコード、`aspnet_Users`と`aspnet_Membership`データベース テーブルです。

プログラムで (で示したように手順 5)、新しいユーザー アカウントを作成できる、中に迅速かつ容易にアプローチでは、CreateUserWizard コントロールを使用します。 このコントロールは、ユーザー情報を収集し、メンバーシップ フレームワークで、新しいユーザーを作成するための複数のステップ ユーザー インターフェイスを表示します。 背後は、このコントロールで使用して同じ`Membership.CreateUser`手順 5 は、コントロールで調べるとメソッドが、ユーザー インターフェイスで、検証コントロールを作成し、コードがまったくを記述することのないユーザー アカウント作成エラーに応答します。

この時点でに新しいユーザー アカウントを作成する機能を備えています。 ただし、まだにログイン ページが 2 番目のチュートリアルでは指定したハード コーディングされた資格情報を検証します。 <a id="_msoanchor_12"> </a>[次のチュートリアル](validating-user-credentials-against-the-membership-user-store-vb.md)を更新して`Login.aspx`ユーザーを検証するメンバーシップ フレームワークに対して資格情報を提供します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [`CreateUser` テクニカル ドキュメント](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard コントロールの概要](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [ファイル システム ベースのサイト マップ プロバイダーを作成します。](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [ASP.NET 2.0 のウィザード コントロールでステップ バイ ステップのユーザー インターフェイスの作成](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [サイト ナビゲーションの ASP.NET 2.0 を確認します。](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [マスター ページとサイトのナビゲーション](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [SQL のサイト マップ プロバイダーを待っています。](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Teresa マーフィーしました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)です。

> [!div class="step-by-step"]
> [前へ](creating-the-membership-schema-in-sql-server-vb.md)
> [次へ](validating-user-credentials-against-the-membership-user-store-vb.md)
