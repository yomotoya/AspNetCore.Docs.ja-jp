---
uid: web-pages/overview/security/16-adding-security-and-membership
title: ページ (Razor) サイトを ASP.NET Web へのセキュリティとメンバーシップの追加 |Microsoft Docs
author: Rick-Anderson
description: この章では、ページの一部がログインするユーザーにのみ使用できるように、web サイトをセキュリティで保護する方法を示します。 (もわかりますページ tha… を作成する方法
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 1c36adf23f3b53e4fbf3dbdce7ca85664b32c975
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021600"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトへのセキュリティとメンバーシップの追加
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ページの一部がログインするユーザーにのみ使用できるように、ASP.NET Web Pages (Razor) の web サイトをセキュリティで保護する方法について説明します。 (もわかりますだれでもアクセスできるページを作成する方法。)
> 
> **学習内容。** 
> 
> - できるように、一部のページのみのメンバーへのアクセスを制限することができます、登録ページとログイン ページを持つ web サイトを作成する方法。
> - パブリックおよびメンバー専用ページを作成する方法。
> - サイトで異なるセキュリティ アクセス許可を持つグループのロールを定義する方法と、ロールにユーザーを割り当てる方法。
> - 自動化されたプログラム (ボット) がメンバーのアカウントを作成するを防ぐために CAPTCHA を使用する方法。
>   
> 
> この記事で導入された ASP.NET 機能を次に示します。
> 
> - WebMatrix**スターター サイト**テンプレート。
> - `WebSecurity`ヘルパーと`Roles`クラス。
> - `ReCaptcha`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


そこにユーザーがログオンするように、web サイトを設定できます&#8212;は、サイトをサポートするよう*メンバーシップ*します。 これはできる多くの理由で役立つあります。 たとえば、サイトには、メンバーにのみ使用可能な必要のあるページがあります。 場合によっては、ユーザーがフィードバックを送信するか、コメントのままにするためにログインする必要があります。

Web サイトにメンバーシップをサポートしている場合でもユーザーは、サイトの一部のページを使用する前にログインする必要はありません。 ログインしていないユーザーと呼ばれる*匿名ユーザー*します。

ユーザーは、web サイトで登録でき、サイトに、ログインできます。 Web サイトでは、ユーザー名 (電子メール アドレス) とパスワードをユーザーがユーザー本人であることを確認が必要です。 ログインとユーザーの id を確認するには、このプロセスと呼ばれる*認証*します。

さまざまな方法で、セキュリティとメンバーシップを設定することができます。

- に基づいて新しいサイトとして作成する簡単な方法は、WebMatrix を使用している場合、**スターター サイト**テンプレート。 このテンプレートは、セキュリティとメンバーシップは既に構成されており、既に登録ページやログイン ページでなど。

    テンプレートによって作成されたサイトでは、ユーザーが Facebook、Google などの外部のサイトを使用してログインするか、Twitter できるようにするためのオプションもあります。
- 既存のサイトにセキュリティを追加する場合、または使用しない場合、**スターター サイト**テンプレート、独自の登録 ページ、ログイン ページで、およびなどを作成できます。

この記事では、最初のオプションについて重点的に取り上げます。&mdash;を使用してセキュリティを追加する方法、**スターター サイト**テンプレート。 独自のセキュリティを実装する方法に関する基本的な情報を提供し、し、その方法に関する詳細情報へのリンクを提供します。 別の記事で詳しく説明されている外部のログインを有効にする方法に関する情報もあります。

## <a name="creating-website-security-using-the-starter-site-template"></a>スターター サイト テンプレートを使用して web サイトのセキュリティを作成します。

WebMatrix で、使用することができます、**スターター サイト**以下を含む web サイトを作成するテンプレート。

- メンバーのユーザー名とパスワードを格納するために使用するデータベース。
- 登録ページ (新しい) 匿名のユーザーを登録できます。
- ログインとログアウト ページです。
- パスワード回復とリセット ページ。

次の手順では、サイトを作成し、構成する方法について説明します。

1. WebMatrix を起動し、**クイック スタート**] ページで、[**テンプレートからサイト**します。
2. 選択、**スターター サイト**テンプレートとクリック**OK**。 WebMatrix は、新しいサイトを作成します。
3. 左側のウィンドウでをクリックして、**ファイル**ワークスペース セレクター。
4. Web サイトのルート フォルダーを開き、  *\_AppStart.cshtml*ファイル、グローバル設定を含めるために使用される特殊なファイルが生成されます。 コメント アウトを使用していくつかのステートメントが含まれている、`//`文字。

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    これらのステートメントの構成、`WebMail`ヘルパーで、電子メールの送信に使用することができます。 メンバーシップ システムでは、ユーザーは登録時に、またはパスワードを変更するときに、確認メッセージを送信するのに電子メールを使用できます。 (たとえば、ユーザーが登録した後、取得、登録プロセスを完了するには、クリックできるリンクを含む電子メールが)。

    」の説明に従って、SMTP サーバーへのアクセスを必要と電子メールを送信する[ASP.NET Web ページ サイトを追加する電子メール](https://go.microsoft.com/fwlink/?LinkId=202899)します。 このサーバーの全体に電子メールの設定を格納します *\_AppStart.cshtml*ファイルに各電子メールを送信できるページに繰り返しコードを作成する必要はありません。 (登録データベースを設定する SMTP 設定を構成する必要はありません。 電子メールのエイリアスからユーザーを検証し、ユーザーが忘れたパスワードをリセットできるようにする場合にのみ、SMTP の設定が必要)。
5. 削除することで、ステートメントをコメント解除します`//`の前に 1 つずつあります。

    電子メールの確認を設定しない場合は、この手順と次の手順を省略できます。 SMTP の値が設定されていない場合、新しいアカウントが確認の電子メールなしですぐに利用します。
6. 次のコードで電子メールに関連する設定を変更します。

   - 設定`WebMail.SmtpServer`へのアクセスが SMTP サーバーの名前にします。
   - まま`WebMail.EnableSsl`設定`true`します。 この設定は、それらを暗号化することで、SMTP サーバーに送信される資格情報を保護します。
   - 設定`WebMail.UserName`SMTP サーバー アカウントのユーザー名にします。
   - 設定`WebMail.Password`SMTP サーバー アカウントのパスワードにします。
   - 設定`WebMail.From`を自分の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。

     > [!NOTE] 
     > 
     > **ヒント:** これらのプロパティの値の詳細については、次を参照してください。[電子メール設定を構成する](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings)で[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkID=202906)します。
7. 保存して閉じます *\_AppStart.cshtml*します。
8. 実行、 *Default.cshtml*ブラウザーでページ。

    ![セキュリティ-メンバーシップ-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > プロパティのインスタンスである必要がありますを示すエラーが表示された場合`ExtendedMembershipProvider`サイトが ASP.NET Web Pages のメンバーシップ システム (SimpleMembership) を使用して構成されていない可能性があります。 ホスティング プロバイダーのサーバーが構成されているローカル サーバーとは異なる場合があることができます。 これを解決するをサイトの次の要素を追加*Web.config*ファイル。
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > 子としてこの要素を追加、`<configuration>`要素とのピアとして、`<system.web>`要素。
9. ページの右上隅にある クリックして、**登録**リンク。 *Register.cshtml*ページが表示されます。
10. ユーザー名とパスワードを入力し、クリックして**登録**します。

    ![セキュリティ-メンバーシップ-3](16-adding-security-and-membership/_static/image2.png)

    Web サイトを作成するときに、**スターター サイト**テンプレート、という名前のデータベース*StarterSite.sdf*でサイトの作成された*アプリ\_データ*フォルダー。 登録時に、ユーザー情報は、データベースに追加されます。 SMTP の値を設定すると、メッセージは、登録を完了するために使用した電子メール アドレスに送信します。

    ![セキュリティ-メンバーシップ-4](16-adding-security-and-membership/_static/image3.png)
11. 電子メール プログラムに移動し、サイトに、確認コードとハイパーリンクにすると、メッセージを検索します。
12. 自分のアカウントをアクティブ化するハイパーリンクをクリックします。 確認のハイパーリンクは、登録の確認 ページを開きます。

    ![セキュリティ-メンバーシップ-5](16-adding-security-and-membership/_static/image4.png)
13. をクリックして、**ログイン**リンク、し、登録したアカウントを使用してログインします。

      ログインした後、**ログイン**と**登録**はリンクに置き換え、**ログアウト**リンク。 ユーザーのログイン名がリンクとして表示されます。 (リンクできるページに移動するパスワードを変更することができます。)

      ![6-セキュリティ-メンバーシップ](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > 既定では、ASP.NET web ページの資格情報をサーバーにクリア テキストで送信 (人間が判読できるテキスト) として。 実稼働サイトがセキュリティで保護された HTTP を使用する必要があります (https:// とも呼ばれる、 *secure socket layer*または SSL) サーバーと交換される機密情報を暗号化します。 電子メールが必要なことができます送信されるメッセージを設定して SSL を使用して`WebMail.EnableSsl=true`前の例のようにします。 SSL の詳細については、次を参照してください。 [Web 通信をセキュリティで保護する: 証明書、SSL、および https://](https://go.microsoft.com/fwlink/?LinkId=208660)します。

## <a name="additional-membership-functionality-in-the-site"></a>サイトの追加のメンバーシップの機能

サイトには、その他のユーザーが自分のアカウントを管理できる機能が含まれています。 ユーザーは、次の操作を行うことができます。

- 自分のパスワードを変更します。 ログインしたら、ユーザー名 (つまり、リンク) をクリックすることができます。 これは、ページに新しいパスワードを作成することができます (*Account/ChangePassword.cshtml*)。
- 忘れたパスワードを回復します。 リンクがあるログイン ページで、(**パスワードを忘れたでしたか?**) ページにユーザーを受け取る (*Account/ForgotPassword.cshtml*) 電子メール アドレスを入力することができます。 サイトがそれらには、新しいパスワードを設定するをクリックできるリンクを含む電子メール メッセージを送信します (*Account/PasswordReset.cshtml*)。

後で説明するよう、外部のサイトを使用してログインできますもユーザーを作成することもできます。

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>メンバー専用ページを作成します。

当面、web サイト内のページに誰でも参照できます。 ログインしている人にのみ使用可能なページがありますしたい場合がありますが (つまり、メンバーに)。 ASP.NET では、ログインしているメンバーのみがアクセスできるページを作成することができます。 通常、匿名ユーザーは、メンバー専用ページにアクセスすると、リダイレクトすることに、ログイン ページに。

この手順では、ログインしているユーザーにのみ使用可能なページを格納するフォルダーを作成します。

1. サイトのルート ディレクトリにある新しいフォルダーを作成します。 (リボンの下にある矢印をクリックします**新規**選び、**新しいフォルダー**。)。
2. 新しいフォルダーの名前*メンバー*します。
3. 内で、*メンバー*フォルダー、新しいページを作成し、という名前*MembersInformation.cshtml*します。
4. 既存のコンテンツを次のコードとマークアップに置き換えます。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    このコードをテスト、`IsAuthenticated`のプロパティ、`WebSecurity`オブジェクトを返します`true`ユーザー ログインしている場合。 コードの呼び出しで、ユーザーがログオンしていない場合`Response.Redirect`をユーザーに送信する、 *Login.cshtml*ページで、*アカウント*フォルダー。

    リダイレクトの URL が含まれています、`returnUrl`クエリ文字列の値を使用する`Request.Url.LocalPath`の現在のページのパスを設定します。 設定した場合、`returnUrl`このようなクエリ文字列の値 (およびかどうか、戻り先 URL はローカル パス)、ログイン ページは、ログインした後に、このページのユーザーを返しますが。

    また、コード、設定 *\_SiteLayout.cshtml*ページ、レイアウト ページとして。 (詳細については、レイアウト ページは、次を参照してください[ASP.NET Web Pages サイトで一貫したレイアウトを作成する](https://go.microsoft.com/fwlink/?LinkId=202891)。)。
5. サイトを実行します。 まだログインしている場合にクリックして、**ログアウト**ページの上部にあるボタンをクリックします。
6. ブラウザーでページを要求 */メンバー/MembersInformation*します。 など、URL は次のようになります。

    `http://localhost:38366/Members/MembersInformation`

    (ポート番号 (38366) は、URL の異なる.)

    リダイレクトされます、 *Login.cshtml*  ページ ログインしていないためです。
7. 先ほど作成したアカウントを使用してをログインします。 リダイレクトして、 *MembersInformation*ページ。 ログインしているため、この時間表示ページの内容。

複数のページへのアクセスをセキュリティで保護は、これを行うことができます。

- 各ページには、セキュリティ チェックを追加します。
- 作成、  *\_PageStart.cshtml*保護対象のページを保存し、追加セキュリティ チェックがフォルダー内のページ。 *\_PageStart.cshtml*ページは、フォルダー内のすべてのページのグローバル ページの一種として機能します。 この手法はで詳しく説明[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)します。

## <a name="creating-security-for-groups-of-users-roles"></a>ユーザー (ロール) のグループのセキュリティを作成します。

サイトのメンバーの多くの場合、ページを参照してください。 それらを許可する前に、各ユーザーのアクセス許可を個別に確認するが効率的ですされません。 グループを作成する代わりに、何ができるまたは*ロール*に個々 のメンバーが属していること。 ロールに基づいてアクセス許可を確認できます。 このセクションで作成します、&quot;管理者&quot;ロールであるユーザーにアクセスできるページを作成し、(に属する) でそのロール。

ASP.NET メンバーシップ システムは、役割をサポートする設定します。 ただし、メンバーシップの登録と、ログインと異なり、**スターター サイト**テンプレートが含まれていないロールを管理する際に役立つページ。 (ロールの管理は、ユーザーのタスクではなく、管理タスク) です。ただし、WebMatrix でメンバーシップ データベースに直接グループを追加することができます。

1. WebMatrix で、をクリックして、**データベース**ワークスペース セレクター。
2. 左側のウィンドウで開く、 *StarterSite.sdf*ノード、**テーブル**ノードをクリックして、 *web ページ\_ロール*テーブル。

    ![7-セキュリティ-メンバーシップ](16-adding-security-and-membership/_static/image6.png)
3. という名前のロールを追加&quot;管理者&quot;します。 *RoleId*フィールドが自動的に入力します。 (主キーは、し、で説明したように、識別フィールドに設定されている[ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893))。
4. 値をメモしておきます、 *RoleId*フィールド。 (最初のロールを定義している場合は、それは 1 なります。)

    ![セキュリティ-メンバーシップ-8](16-adding-security-and-membership/_static/image7.png)
5. 閉じる、 *web ページ\_ロール*テーブル。
6. 開く、 *UserProfile*テーブル。
7. 書き留めて、 *UserId*テーブルを閉じますテーブル内のユーザーの 1 つ以上の値。
8. 開く、 *web ページ\_UserInRoles*テーブルし、入力、 *UserID*と*RoleID*テーブルに値。 たとえば、ユーザー 2 に配置するため、&quot;管理者&quot;ロールでは、これらの値を入力します。

    ![9-セキュリティ-メンバーシップ](16-adding-security-and-membership/_static/image8.png)
9. 閉じる、 *web ページ\_UsersInRoles*テーブル。

    定義されているロールがある場合はがそのロールのユーザーにアクセスできるページを構成できます。
10. という名前の新しいページを作成、web サイトのルート フォルダーで*AdminError.cshtml*既存のコンテンツを次のコードに置き換えます。 ページへのアクセスが許可されていない場合にユーザーがリダイレクトされるページになります。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. という名前の新しいページを作成、web サイトのルート フォルダーで*AdminOnly.cshtml*既存のコードを次のコードに置き換えます。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole`メソッドを返します。 `true` 、現在のユーザーが (この例では、"admin"ロール) で指定されたロールのメンバーであります。
12. 実行*Default.cshtml*ブラウザーで、ログインはありませんが。 (既にログインしている場合をログアウトします。)
13. ブラウザーのアドレス バーに追加*AdminOnly* URL にします。 (つまり、要求、 *AdminOnly.cshtml*ファイルです)。リダイレクトされます、 *AdminError.cshtml*ページのためのユーザーとしてログインは現在、&quot;管理者&quot;ロール。
14. 戻り*Default.cshtml*に追加したユーザーとしてログインし、&quot;管理者&quot;ロール。
15. 参照する*AdminOnly.cshtml*ページ。 この時間はのページが表示されます。

## <a name="preventing-automated-programs-from-joining-your-website"></a>Web サイトに参加したり、自動化されたプログラムの防止

[ログイン] ページでは自動化されたプログラムは停止しません (とも呼ば*web ロボット*または*ボット*) から web サイトに登録します。 この手順では、登録ページの ReCaptcha テストを有効にする方法について説明します。

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Web サイトには、recaptcha. Net の登録 ([http://recaptcha.net](http://recaptcha.net))。 登録が完了したら、公開キーと秘密キーが表示されます。
2. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
3. *アカウント*フォルダーで、という名前のファイルを開く*Register.cshtml*します。
4. ページの上部にあるコードで、次の行を検索し、削除することでコメントを解除します、`//`コメント文字。

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. 置換`PRIVATE_KEY`独自 ReCaptcha 秘密キーを使用します。
6. ページのマークアップでは、削除、`@*`と`*@`に関するページのマークアップでは、次の行からコメント文字。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. 置換`PUBLIC_KEY`キーを使用します。
8. これは既に削除していない、削除、 `<div>` 「には、CAPTCHA の確認... を有効にする」で始まるテキストを含む要素。 (全体を削除`<div>`要素とその内容)。

9. 実行*Default.cshtml*ブラウザーでします。 を、サイトにログインしている場合は、クリックして、**ログアウト**リンク。
10. をクリックして、**登録**リンクし、CAPTCHA テストを使用して登録をテストします。

     ![セキュリティ-メンバーシップ-10](16-adding-security-and-membership/_static/image9.png)

詳細については、`ReCaptcha`ヘルパーを参照してください[自動プログラムを防ぐ (ボット) を使用して ASP.NET Web サイトから、CATPCHA を使用して](https://go.microsoft.com/fwlink/?LinkId=251967)します。

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>ユーザーが外部のサイトを使用してログイン

**スターター サイト**テンプレートには、コードとユーザーができるマークアップが含まれます。 Facebook、Windows Live、Twitter、Google、Yahoo、を使用してログインします。 既定では、この機能は有効になっていません。 一般的な手順でこれらの外部プロバイダーを使用してログではできるようにするためのユーザーを使用するはこの。

- サポートする外部サイトのどれを決定します。
- 必要な場合、そのサイトに移動し、ログイン アプリを設定します。 (などを行う必要があるこの Facebook ログインを許可するためにします。)
- サイトで、プロバイダーを構成します。 ほとんどの場合だけがあるの一部のコードのコメントを解除する、  *\_AppStart.cshtml*ファイル。
- により、ユーザー登録ページにマークアップを追加でのログ記録用の外部サイトへのリンク。 通常、テキストを少し変更して、マークアップをコピーできます。

詳細な手順については、トピックで確認できます[ASP.NET Web ページ サイトで外部のサイトからのログインを有効にする](https://go.microsoft.com/fwlink/?LinkId=251969)します。

別のサイトから、ユーザーがログインした後、ユーザーが、サイトに戻ったと*関連付けます*サイトにログインします。 実際には、ユーザーの外部ログインのサイトでメンバーシップ エントリを作成します。 外部ログインを持つメンバーシップ (ロール) などの標準機能を使用できます。

## <a name="adding-security-to-an-existing-website"></a>既存の web サイトへのセキュリティの追加

この記事の前の手順を使用して依存、**スターター サイト**web サイトのセキュリティの基礎としてテンプレート。 開始するための実際的でない場合、**スターター サイト**テンプレートにそのテンプレートに基づくサイトから関連するページをコピーすることができますを実装する同じ種類のセキュリティの独自のサイトで自分でコーディングしています。 同じ種類のページを作成する-登録やログイン、— およびメンバーシップを設定するヘルパー クラスとクラスを使用します。

基本的なプロセスがブログの投稿で説明されている[ASP.NET Razor のセキュリティを実装する最も基本的な方法は、](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)します。 ほとんどの作業を行う、次のメソッドとプロパティを使用して、`WebSecurity`ヘルパー。

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx)、 [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx)します。 これらのメソッドのユーザーは既に登録されているかどうかを判断できるように登録するとします。
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). このプロパティを使用して、現在のユーザーがログインしているかどうかを確認できます。 ログインがない場合、ユーザーをログイン ページにリダイレクトするのには便利です。
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx)、 [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx)します。 これらのメソッドは、入力または出力に、ユーザーをログインします。
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx)します。 このプロパティは、ユーザーがログイン) であれば、現在のユーザーのログイン名を表示するために便利です。
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx)します。 このメソッドは、登録の確認の電子メールを設定する場合に便利です。 (詳細については、ブログの投稿で説明[ASP.NET Web Pages のセキュリティの確認機能を使用して](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267))。

ロールを管理するには、使用することができます、[ロール](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx)と[メンバーシップ](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx)クラスをブログ記事で説明されているとします。

## <a name="additional-resources"></a>その他のリソース

- [サイト全体の動作をカスタマイズする](https://go.microsoft.com/fwlink/?LinkId=202906)
- [セキュリティで保護する Web 通信: 証明書、SSL、および https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [ASP.NET Razor のセキュリティを実装する最も基本的な方法は、](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)と[ASP.NET Web Pages のセキュリティの確認機能を使用して](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)します。 これらを使用せずに ASP.NET メンバーシップ機能を実装する方法を説明するブログの投稿、**スターター サイト**テンプレート。
- [ASP.NET Web ページ サイトで外部サイトからのログインを有効にする](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity クラスの API リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99))(MSDN)
- [SimpleRoleProvider クラスの API リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99))(MSDN)
- [クラスの API をに関する SimpleMembershipProvider リファレンス](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99))(MSDN)
