---
uid: web-pages/overview/security/16-adding-security-and-membership
title: "ページ (Razor) サイトの ASP.NET Web へのセキュリティとメンバーシップの追加 |Microsoft ドキュメント"
author: tfitzmac
description: "この章では、一部のページがログインするユーザーにのみ使用できるように、web サイトをセキュリティで保護する方法を示します。 (もわかりますページ tha... を作成する方法"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトへのセキュリティとメンバーシップの追加
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトをセキュリティで保護するページの一部がログインするユーザーにのみ使用できるようにする方法について説明します。 (もわかりますだれでもアクセスできるページを作成する方法。)
> 
> **学習する内容。** 
> 
> - できるように、一部のページのみのメンバーへのアクセスを制限することができます、登録ページ、およびログイン ページを持つ web サイトを作成する方法。
> - パブリックおよびメンバー専用のページを作成する方法。
> - サイトでさまざまなセキュリティ アクセス許可を持つグループのロールを定義する方法と、ロールにユーザーを割り当てる方法です。
> - CAPTCHA を使用して自動プログラム (bot) がメンバーのアカウントを作成することを防止する方法です。
>   
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - WebMatrix**スターター サイト**テンプレート。
> - `WebSecurity`ヘルパーと`Roles`クラスです。
> - `ReCaptcha`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


ユーザーがその &#8212; にログインできるように、web サイトを設定することができます。つまり、サイトをサポートするよう*メンバーシップ*です。 これができます、さまざまな理由です。 たとえば、サイトには、メンバーにのみ利用できるようにページがあります。 場合によっては、ユーザーにフィードバックを送信するか、コメントのままにするためにログインが求められます。

ユーザーが web サイトにメンバーシップをサポートしている場合でも、サイトの一部のページを使用する前にログインする必要があります。 ログインしていないユーザーと呼ばれる*匿名ユーザー*です。

ユーザーは、web サイトに登録できますをことができますし、サイトにログインします。 Web サイトでは、ユーザー名 (電子メール アドレス) とユーザーがユーザー本人であることを確認するためのパスワードが必要です。 ログインおよびユーザーの身元を確認するには、このプロセスと呼ばれる*認証*です。

さまざまな方法でセキュリティとメンバーシップを設定することができます。

- 基に新しいサイトを作成する簡単な方法は、WebMatrix を使用している場合、**スターター サイト**テンプレート。 このテンプレートは、セキュリティおよびメンバーシップは既に構成されており、既に登録ページ、ログイン ページとなど。

    テンプレートによって作成されたサイトでは、ユーザーが Twitter または Facebook、Google などの外部のサイトを使用してログインできるようにするオプションもあります。
- 既存のサイトにセキュリティを追加する場合、または使用しない場合、**スターター サイト**テンプレート、独自の登録 ページ、ログイン ページおよびなどを作成できます。

この記事は、最初のオプションの焦点を当てています&mdash;を使用してセキュリティを追加する方法、**スターター サイト**テンプレート。 独自のセキュリティを実装する方法に関する基本的な情報を提供しを実行する方法に関する詳細情報へのリンクを提供します。 別の記事で詳しく説明されている外部のログインを有効にする方法に関する情報もあります。

## <a name="creating-website-security-using-the-starter-site-template"></a>スターター サイト テンプレートを使用して web サイトのセキュリティを作成します。

WebMatrix で使用することができます、**スターター サイト**以下を含む web サイトを作成するテンプレート。

- メンバー用のユーザー名とパスワードの格納に使用されるデータベースです。
- 登録ページ (新しい) の匿名ユーザーを登録できます。
- ログインとログアウト ページです。
- パスワードの回復とリセット ページです。

次の手順では、サイトを作成し、構成する方法について説明します。

1. WebMatrix を開始し、、**クイック スタート** ページで、**サイトからテンプレート**です。
2. 選択、**スターター サイト**テンプレートをクリックして**OK**です。 WebMatrix では、新しいサイトを作成します。
3. 左側のウィンドウでをクリックして、**ファイル**ワークスペース セレクター。
4. Web サイトのルート フォルダーで開く、  *\_AppStart.cshtml*ファイル。 これはグローバル設定を格納するために使用される特別なファイルです。 コメント アウトを使用していくつかのステートメントが含まれている、`//`文字。

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    これらのステートメントを構成、`WebMail`ヘルパーに渡し、電子メールを送信するために使用できます。 メンバーシップ システムは、ユーザーが登録時に、またはパスワードを変更するときに確認メッセージを送信するのに電子メールを使用できます。 (たとえば、ユーザーが登録した後、取得、登録プロセスを完了するためをクリックしてリンクを含む電子メールです。)

    」の説明に従って、SMTP サーバーへのアクセスを要求する電子メールを送信する[ASP.NET Web Pages サイトを追加する電子メール](https://go.microsoft.com/fwlink/?LinkId=202899)です。 電子メールの設定この部に保存する *\_AppStart.cshtml*ファイルの電子メールを送信する各ページに繰り返しにコーディングする必要があるないようにします。 (登録データベースを設定する SMTP 設定を構成する必要はありません。 電子メールのエイリアスからユーザーを検証し、忘れたパスワードをリセットできるようにする場合にのみ、SMTP の設定が必要)
5. 削除することで、ステートメントのコメントを解除`//`前にある 1 つずつです。

    確認の電子メールを設定しない場合は、この手順と次の手順を省略できます。 SMTP の値が設定されていない場合、新しいアカウントが確認の電子メールなしですぐに利用できます。
6. コードの次の電子メールに関連する設定を変更します。

    - 設定`WebMail.SmtpServer`へのアクセスが SMTP サーバーの名前にします。
    - ままにして`WebMail.EnableSsl`'éý'`true`です。 この設定は、暗号化して、SMTP サーバーに送信される資格情報を保護します。
    - 設定`WebMail.UserName`SMTP サーバーのアカウントのユーザー名にします。
    - 設定`WebMail.Password`SMTP サーバーのアカウントのパスワードにします。
    - 設定`WebMail.From`の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。

    > [!NOTE] 
    > 
    > **ヒント:**これらのプロパティの値についての詳細については、次を参照してください。[電子メール設定を構成する](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings)で[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkID=202906)します。
7. 保存して閉じる *\_AppStart.cshtml*です。
8. 実行、 *Default.cshtml*ブラウザーのページです。

    ![security-membership-2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > プロパティのインスタンスである必要があることを示すエラーが発生する場合`ExtendedMembershipProvider`サイトが ASP.NET Web Pages のメンバーシップ システム (SimpleMembership) を使用して構成されていない可能性があります。 ホスティング プロバイダーのサーバーの構成は、ローカル サーバーとは異なる場合があることができます。 この問題を解決するをサイトの次の要素を追加*Web.config*ファイル。
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > この要素の子として追加、`<configuration>`要素とのピアとして、`<system.web>`要素。
9. ページの右上隅で、クリックして、**登録**リンクします。 *Register.cshtml*ページが表示されます。
10. ユーザー名とパスワードを入力し、クリックして**登録**です。

    ![security-membership-3](16-adding-security-and-membership/_static/image2.png)

    Web サイトを作成したときに、**スターター サイト**テンプレート、という名前のデータベース*StarterSite.sdf* 、サイトで作成された*アプリ\_データ*フォルダーです。 登録の際に、ユーザー情報がデータベースに追加されます。 SMTP 値を設定すると、メッセージは、登録を完了するために使用する電子メール アドレスに送信します。

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. 電子メール プログラムに移動し、サイトに、確認コードとハイパーリンクを持つメッセージを検索します。
12. 自分のアカウントをアクティブ化するハイパーリンクをクリックします。 確認のハイパーリンクは、登録の確認 ページを開きます。

    ![security-membership-5](16-adding-security-and-membership/_static/image4.png)
- クリックして、**ログイン**リンク、および登録したアカウントを使用してサインインします。

    ログインした後、**ログイン**と**登録**はリンクに置き換え、**ログアウト**リンク。 ユーザーのログイン名は、リンクとして表示されます。 (リンクできますページに移動するパスワードを変更することができます。)

    ![security-membership-6](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > 既定では、ASP.NET web ページの資格情報をサーバーにクリア テキストで (として送信人間が判読できるテキスト)。 運用サイトは、セキュリティで保護された HTTP を使用する必要があります (https://とも呼ばれる、 *secure socket layer*または SSL) サーバーと交換される機密情報を暗号化します。 必要な電子メールを作成することができます送信されるメッセージを設定して SSL を使用して`WebMail.EnableSsl=true`前の例に示すようにします。 SSL の詳細については、次を参照してください。 [Web 通信のセキュリティ保護: 証明書、SSL、および https://](https://go.microsoft.com/fwlink/?LinkId=208660)です。

## <a name="additional-membership-functionality-in-the-site"></a>サイトの追加のメンバーシップ機能

サイトには、ユーザーがそのアカウントを管理できるその他の機能が含まれています。 ユーザーは、次の操作を行うことができます。

- 自分のパスワードを変更します。 ログイン後、ユーザー名 (リンク) をクリックしています。 これは、ページに、新しいパスワードを作成することができます (*Account/ChangePassword.cshtml*)。
- パスワードを忘れても回復します。 [ログイン] ページでは、リンク (**するパスワードを忘れましたしますか?**) ページにユーザーを受け取る (*Account/ForgotPassword.cshtml*)、電子メール アドレスを入力することができます。 サイトには新しいパスワードを設定するためをクリックしてリンクを含む電子メール メッセージを送信して (*Account/PasswordReset.cshtml*)。

後で説明するよう、外部のサイトを使用してログインできますもユーザーを指定することもできます。

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>メンバー専用のページを作成します。

当面の web サイト内の任意のページに誰でも参照できます。 ログインしている人にのみ使用するページがあることができますが、(つまり、メンバーに)。 ASP.NET では、ログインのメンバーでのみアクセスできるページを作成することができます。 通常、匿名ユーザーは、メンバーだけがページにアクセスすると、リダイレクトすることにログイン ページに。

この手順では、ログインしているユーザーにのみ使用可能なページを格納するフォルダーを作成します。

1. サイトのルート ディレクトリにある新しいフォルダーを作成します。 (、リボンの下にある矢印をクリックして**新規**を選択し**新しいフォルダー**)。
2. 新しいフォルダーの名前を付けます*メンバー*です。
3. 内部、*メンバー*フォルダーで、新しいページを作成し、という名前が*MembersInformation.cshtml*です。
4. 既存のコンテンツを次のコードとマークアップに置き換えます。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    このコードはテスト、`IsAuthenticated`のプロパティ、`WebSecurity`オブジェクトを返します`true`ユーザー ログインしている場合。 コードの呼び出しで、ユーザーがログオンしていない場合`Response.Redirect`をユーザーに送信する、 *Login.cshtml*  ページで、*アカウント*フォルダーです。

    リダイレクトの URL が含まれています、`returnUrl`クエリ文字列の値を使用する`Request.Url.LocalPath`を現在のページのパスを設定します。 設定した場合、`returnUrl`次のようにクエリ文字列内の値 (およびかどうか、戻り先 URL はローカル パス)、ログイン ページはユーザーに戻りますこのページにログインした後にします。

    コードにも設定 *\_SiteLayout.cshtml*ページとしてそのページをレイアウトします。 (詳細については、レイアウト ページは、次を参照してください[ASP.NET Web Pages サイトで一貫したレイアウトを作成する](https://go.microsoft.com/fwlink/?LinkId=202891)。)。
5. サイトを実行します。 ログインしていますが、クリックして、**ログアウト**ページの上部にあるボタンをクリックします。
6. ブラウザーで、ページの要求*メンバー/MembersInformation*です。 たとえば、URL は、次のようになります。

    `http://localhost:38366/Members/MembersInformation`

    (ポート番号 (38366) は、URL の異なる.)

    リダイレクトしている、 *Login.cshtml*ページで、ログインしていないためです。
- 以前に作成したアカウントを使用してをログインします。 リダイレクトしている、 *MembersInformation*ページ。 ログに記録しているため現時点表示ページの内容。

保護するには複数のページへのアクセスは、これを行うことができます。

- 各ページに、セキュリティ チェックを追加します。
- 作成、  *\_PageStart.cshtml*  ページで、フォルダー、保護対象のページを保存し、追加セキュリティ チェックがあります。 *\_PageStart.cshtml*ページはフォルダー内のすべてのページの [グローバル] ページの種類として機能します。 この手法がで詳しく説明されている[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)します。

## <a name="creating-security-for-groups-of-users-roles"></a>ユーザー (ロール) のグループのセキュリティを作成します。

サイトに多数のメンバーがある場合は、ページを参照してください。 それらを許可する前に各ユーザーのアクセス許可を個別にチェックする効率的ではありません。 グループを作成する代わりに、何ができるまたは*ロール*、個々 のメンバーに属していること。 役割に基づいたのアクセス許可を確認し、できます。 このセクションで作成、&quot;管理者&quot;ロールであるユーザーにアクセス可能なページを作成し、(に属している) でそのロール。

ASP.NET メンバーシップ システムは、役割をサポートするを設定します。 ただしとは異なりとログインのメンバーシップの登録、**スターター サイト**テンプレートが含まれていないできるページは、ロールを管理します。 (ロールの管理は、ユーザーのタスクではなく、管理タスクです。)ただし、WebMatrix で、メンバーシップ データベースに直接、グループを追加することができます。

1. WebMatrix でをクリックして、**データベース**ワークスペース セレクター。
2. 左側のウィンドウで開く、 *StarterSite.sdf*ノード、開いている、**テーブル**ノードを展開し、ダブルクリック、 *web ページ\_ロール*テーブル。

    ![security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. という名前のロールを追加&quot;admin&quot;です。 *RoleId*フィールドが自動的に入力します。 (主キーであるし、で説明したように、識別フィールドに設定されている[ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893))。
4. 新機能の値を書き留めて、 *RoleId*フィールドです。 (最初のロールを定義している場合は、その 1 なります。)

    ![security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. 閉じる、 *web ページ\_ロール*テーブル。
6. 開く、 *UserProfile*テーブル。
7. メモ、 *UserId*閉じ、テーブル、テーブル内のユーザーの 1 つ以上の値。
8. 開く、 *web ページ\_UserInRoles*テーブルし、入力、 *UserID*と*RoleID*テーブルに値。 たとえば、ユーザー 2 にするため、 &quot;admin&quot;ロール、これらの値を入力します。

    ![security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. 閉じる、 *web ページ\_UsersInRoles*テーブル。

    これで定義されたロールがある場合は、そのロールにユーザーにアクセス可能なページを構成できます。
10. という名前の新しいページを作成、web サイトのルート フォルダーで*AdminError.cshtml*し、既存のコンテンツを次のコードに置き換えます。 これらのページへのアクセスが許可されていない場合にユーザーがリダイレクトされるページになります。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. という名前の新しいページを作成、web サイトのルート フォルダーで*AdminOnly.cshtml*し、既存のコードを次のコードに置き換えます。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole`メソッドを返します。`true`場合、現在のユーザーは (この場合は、"admin"ロール) を指定したロールのメンバーです。
12. 実行*Default.cshtml*ブラウザーでログインしないが、します。 (既にログオンしている場合をログアウトします。)
13. ブラウザーのアドレス バーに追加*AdminOnly* URL にします。 (つまり、要求、 *AdminOnly.cshtml*ファイルです)。リダイレクトしている、 *AdminError.cshtml*ページ、ログインしていない現在のユーザーとして、 &quot;admin&quot;ロール。
14. 戻る*Default.cshtml*に追加したユーザーとしてログインし、 &quot;admin&quot;ロール。
15. 参照*AdminOnly.cshtml*ページ。 この時間ページが表示されます。

## <a name="preventing-automated-programs-from-joining-your-website"></a>Web サイトに参加したり、自動プログラムの防止

[ログイン] ページでは自動プログラムは停止しません (とも呼ば*ロボットを web*または*bot*) から web サイトに登録することです。 この手順では、[登録] ページの ReCaptcha テストを有効にする方法について説明します。

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. ReCaptcha.Net で web サイトを登録 ([http://recaptcha.net](http://recaptcha.net))。 登録が完了したら、公開キーと秘密キーが表示されます。
2. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
3. *アカウント*、開いているフォルダー、ファイルの名前*Register.cshtml*です。
4. コードでは、ページの上部にある、次の行を見つけて削除することでそれらのコメントを解除、`//`コメント文字。

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. 置き換える`PRIVATE_KEY`ReCaptcha 秘密キーにします。
6. ページのマークアップでは、削除、`@*`と`*@`ページ マークアップの次の行の周りからコメント文字。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. 置き換える`PUBLIC_KEY`キーを使用します。
8. 削除まだしていない場合は、削除、 `<div>` 「には、CAPTCHA の確認 を有効にする」で始まるテキストが含まれる要素です。 (全体を削除`<div>`要素とその内容です)。

1. 実行*Default.cshtml*ブラウザーでします。 クリックして、サイトにログインする場合、**ログアウト**リンクします。
2. クリックして、**登録**リンクし、CAPTCHA テストを使用して登録をテストします。

    ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

詳細については、`ReCaptcha`ヘルパーに渡しを参照してください[自動プログラムを防ぐ (Bot) を使用して、ASP.NET Web サイトから、CATPCHA を使用して](https://go.microsoft.com/fwlink/?LinkId=251967)です。

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>ユーザが外部のサイトを使用してログイン

**スターター サイト**テンプレートには、コードとユーザーができるマークアップが含まれています。 これには、Facebook、Windows Live、Twitter、Google、または yahoo を使用してログインします。 既定では、この機能は無効です。 一般的な手順ことユーザーを使用してこれらの外部プロバイダーを使用してログはこの。

- サポートする外部サイトのどれを決定します。
- 必要に応じて、そのサイトに移動し、ログインのアプリをセットアップします。 (などを行う必要があるこの Facebook ログインを許可するためにします。)
- サイトでは、プロバイダーを構成します。 ほとんどの場合だけがあるの一部のコードのコメントを解除する、  *\_AppStart.cshtml*ファイル。
- ユーザー登録ページ マークアップに追加のログインに外部サイトへのリンク。 通常、テキストがわずかに変化し、必要とするマークアップをコピーできます。

トピックのステップ バイ ステップの説明を参照して[ASP.NET Web Pages サイト内の外部のサイトからのログインを有効にすると](https://go.microsoft.com/fwlink/?LinkId=251969)です。

ユーザーが、サイトに戻った後、別のサイトからユーザーがログイン、および*関連付けます*で、サイトにログインします。 実際には、ユーザーの外部ログインについて、サイト メンバーシップ エントリが作成されます。 外部ログインを持つメンバーシップ (ロール) などの標準機能を使用できます。

## <a name="adding-security-to-an-existing-website"></a>既存の web サイトへのセキュリティの追加

この記事の前の手順を使用してに依存しています、**スターター サイト**web サイトのセキュリティの基礎としてテンプレート。 起動するための実際的でない場合、**スターター サイト**テンプレートか、そのテンプレートに基づくサイトから、関連ページをコピーを実装できます、同じ種類のセキュリティ、自分のサイトで、自分でコーディングします。 同じ種類のページを作成する — 登録、ログイン、およびなど — およびメンバーシップを設定するヘルパー クラスとクラスを使用します。

基本的なプロセスは、ブログの投稿に記載されて[ASP.NET Razor のセキュリティを実装する最も簡単な方法](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)です。 ほとんどの作業を行う、次のメソッドとプロパティを使用して、`WebSecurity`ヘルパー。

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). これらのメソッドを使用して、他のユーザーは既に登録されているかどうかを判別できると、それらを登録します。
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). このプロパティでは、現在のユーザーがログインしているかどうかを確認できます。 これは、ユーザーをリダイレクトするログイン ページへログインがない場合に便利です。
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). これらのメソッドは、in または out にユーザーをログインします。
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). このプロパティは、(この場合、ユーザーがログインして)、現在のユーザーのログイン名を表示するため便利です。
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). このメソッドは、登録の確認の電子メールを設定する場合に便利です。 (詳細については、ブログの投稿で説明[ASP.NET Web Pages のセキュリティの確認機能を使用して](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267))。

ロールを管理するには、使用することができます、[ロール](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx)と[メンバーシップ](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx)クラス、ブログ記事で説明されているようです。

## <a name="additional-resources"></a>その他のリソース

- [サイト全体の動作をカスタマイズする](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Web 通信の保護: 証明書、SSL、および https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [ASP.NET Razor のセキュリティを実装する最も簡単な方法](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)と[ASP.NET Web Pages のセキュリティの確認機能を使用して](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)です。 これらを使用せずに ASP.NET メンバーシップ機能を実装する方法を説明するブログの投稿、**スターター サイト**テンプレート。
- [ASP.NET Web ページ サイトで外部サイトからのログインを有効にする](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity クラスの API リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99))(MSDN)
- [SimpleRoleProvider クラスの API リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99))(MSDN)
- [に関する SimpleMembershipProvider クラスの API リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99))(MSDN)
