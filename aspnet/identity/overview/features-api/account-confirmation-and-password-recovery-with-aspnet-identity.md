---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "アカウントの確認と ASP.NET Identity (c#) とパスワードの回復 |Microsoft ドキュメント"
author: HaoK
description: "このチュートリアルを実行する前に、電子メールの確認とパスワードのリセットのログで web アプリの作成をセキュリティで保護された ASP.NET MVC 5 を完了する必要があります。 このチュートリアルには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 5fa7b6227eb88aa6766ab8776bc8a3cc1111b942
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>アカウントの確認と ASP.NET Identity (c#) とパスワードの回復
====================
によって[ハオ最後にトリム](https://github.com/HaoK)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルを実行する前に完了してください[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)です。 このチュートリアルでは、詳細が含まれています、ローカル アカウントの確認の電子メールを設定し、ASP.NET Identity の中で、パスワードをリセットできるようにする方法を示します。 この記事は、Rick Anderson によって書き込まれました ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、最後に、ハオ トリムと Suhas Joshi です。 NuGet サンプルは、最後にハオ トリムによって、主に書き込まれました。


ローカル ユーザー アカウントには、アカウントのパスワードを作成するユーザーが必要とされ、そのパスワードは、web app で (安全) 格納されます。 ASP.NET の Id には、ソーシャル アカウントは、アプリのパスワードを作成するユーザーを必要としないもサポートしています。 [ソーシャル アカウント](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)(Google、Twitter、Facebook、または Microsoft) などのサード パーティを使用してユーザーを認証します。 このトピックは、次について説明します。

- [ASP.NET MVC アプリの作成](#createMvc)と ASP.NET Identity の機能を調査します。
- [Id サンプルのビルド](#build)
- [確認の電子メールを設定します。](#email)

新しいユーザーは、その電子メールのエイリアスを作成するローカル アカウントを登録します。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

登録 ボタンをクリックすると、メール アドレスの検証トークンを含む確認の電子メールを送信します。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

ユーザーには、自分のアカウントの確認トークンを使用して電子メールが送信されます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

リンクをクリックすると、アカウントを確認します。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>パスワードの回復またはリセット

ローカルのユーザーが自分のパスワードを忘れたパスワードのリセットを有効にすると、電子メール アカウントに送信されるセキュリティ トークンことができます。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
ユーザーはすぐに電子メールをリンクに、パスワードをリセットすることです。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
リンクをクリックに移動し、リセットされたページ。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
クリックすると、**リセット**ボタンは、パスワードがリセットされたことを確認します。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>ASP.NET Web アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 Visual Studio のインストール[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。

> [!NOTE]
> 警告: Visual Studio をインストールする必要があります[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)このチュートリアルを完了します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームは、web フォーム アプリケーションで同じ手順を実行できなかったために、ASP.NET Identity もサポートします。
2. 既定の認証としてのままにして**個々 のユーザー アカウント**です。
3. アプリを実行する をクリックして、**登録**リンクし、ユーザーを登録します。 この時点では、電子メールにのみ、検証、 [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
4. サーバー エクスプ ローラーに移動**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開き**です。

    次の図は、`AspNetUsers`スキーマ。

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. 右クリックして、 **AspNetUsers**テーブルを選択して**テーブル データの表示**です。  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 この時点で、電子メールが確認されていません。

ASP.NET Id の既定のデータ ストアは、Entity Framework が、他のデータ ストアを使用して、フィールドを追加することを構成することができます。 参照してください[その他のリソース](#addRes)このチュートリアルの最後のセクションでします。

[OWIN スタートアップ クラス](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)( *Startup.cs* )、アプリが起動しを呼び出すときに呼び出される、`ConfigureAuth`メソッド*アプリ\_Start\Startup.Auth.cs*、これにより、OWIN パイプラインを構成して、ASP.NET Identity を初期化します。 確認、`ConfigureAuth`メソッドです。 各`CreatePerOwinContext`呼び出し、コールバックの登録 (に保存されている、 `OwinContext`) 指定した型のインスタンスを作成する要求あたり 1 回呼び出すされます。 コンス トラクターにブレークポイントを設定することができ、`Create`各型のメソッド (`ApplicationDbContext, ApplicationUserManager`) 要求のたびに呼び出されることを確認します。 インスタンスを`ApplicationDbContext`と`ApplicationUserManager`は、アプリケーション全体でアクセス可能な OWIN コンテキストに格納します。 ASP.NET Identity は、cookie ミドルウェアを OWIN パイプラインにフックします。 詳細については、次を参照してください。 [UserManager クラスは、ASP.NET Identity の要求の有効期間管理あたり](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)します。

新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更するときに、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。 注意してください、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。 セキュリティ クッキーに保存されていない、`AspNetUsers`テーブル (または Identity db では、他の場所から)。 Cookie のセキュリティ トークンは、自己署名を使用して[DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。

Cookie ミドルウェアでは、各要求の cookie を確認します。 `SecurityStampValidator`メソッドで、`Startup`クラスの DB のヒット数およびセキュリティ スタンプを定期的にチェックで指定されたとおり、`validateInterval`です。 これはセキュリティ プロファイルを変更する場合を除きます (この例では) で 30 分おきのみ発生します。 データベースへのトリップを最小限に抑える、30 分間隔が選択されました。 参照してください  [2 要素認証チュートリアル](index.md)詳細についてはします。

コードでは、コメント、`UseCookieAuthentication`メソッドは、cookie 認証をサポートしています。 `SecurityStamp`フィールドと関連付けられているコードは、層を追加、アプリケーションにセキュリティのパスワードを変更するときにログインする必要がログインで使用するブラウザー外です。 `SecurityStampValidator.OnValidateIdentity`メソッドは、パスワードを変更または外部ログインを使用する際に使用される、ユーザーがログインすると、セキュリティ トークンを検証するアプリを使用します。 古いパスワードを使用して生成されたトークン (クッキー) を無効にしたことを確認する必要です。 サンプル プロジェクトで変更した場合のユーザーのパスワード、新しいトークンでは、ユーザーが生成、以前のすべてのトークンが無効にし、`SecurityStamp`フィールドを更新します。

Id システムを許可するアプリを構成するため、ユーザーのセキュリティ プロファイルが変更されたときに (たとえば、ユーザーが、パスワードまたは変更を変更するときに関連付けられているログイン (Facebook、Google、2016/2/14 などの Microsoft アカウントなど)、外のすべてのユーザーがログインしています。ブラウザーのインスタンス。 たとえば、下の例のイメージ、[シングル サインアウトのサンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)アプリで、1 つのボタンをクリックしてすべてのブラウザー インスタンス (ここでは、IE、Firefox および Chrome) からサインアウトすることができます。 または、サンプルには、のみ、特定のブラウザー インスタンスからログアウトすることができます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[シングル サインアウトのサンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)アプリは、ASP.NET Identity を使用すると、セキュリティ トークンを再生成する方法を示しています。 古いパスワードを使用して生成されたトークン (クッキー) を無効にしたことを確認する必要です。 この機能を提供すると、アプリケーションのセキュリティ層を追加パスワードを変更するときに、このアプリケーションにログインした場所に記録されます。

*アプリ\_Start\IdentityConfig.cs*ファイルが含まれています、 `ApplicationUserManager`、`EmailService`と`SmsService`クラスです。 `EmailService`と`SmsService`各実装するクラス、`IIdentityMessageService`インターフェイス、電子メールと SMS を構成するには、各クラスに共通のメソッドがあるようにします。 このチュートリアルはのみを使用して電子メール通知を追加する方法を示しますが[SendGrid](http://sendgrid.com/)SMTP、およびその他のメカニズムを使用して電子メールを送信することができます。

`Startup`クラスは、ソーシャル ログイン (Facebook、Twitter など) が自分のチュートリアルを参照して追加するボイラー プレートも含まれています。 [Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)詳細についてはします。

確認、`ApplicationUserManager`クラスでは、ユーザー id 情報が含まれており、次の機能を構成します。

- パスワードの強度要件です。
- ユーザーのロックアウト (試行と時刻)。
- 2 要素認証 (2 fa)。 別のチュートリアルでは 2 fa と SMS を説明します。
- 電子メールと SMS サービスをフックします。 (取り上げ、SMS 別のチュートリアルでは)。

`ApplicationUserManager`ジェネリック クラス`UserManager<ApplicationUser>`クラスです。 `ApplicationUser`派生した[IdentityUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)です。 `IdentityUser`ジェネリックから派生した`IdentityUser`クラス。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

上記のプロパティで指定したプロパティと一致し、`AspNetUsers`上に示す表。

ジェネリック引数`IUser`を使用する主キーの種類を使用してクラスを派生できます。 参照してください、 [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)サンプル int または GUID を文字列から主キーを変更する方法を示します。

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) で定義された*Models\IdentityModels.cs*として。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

上記の強調表示されたコードの生成、 [ClaimsIdentity](https://msdn.microsoft.com/en-us/library/system.security.claims.claimsidentity.aspx)です。 ASP.NET Identity と OWIN の Cookie 認証クレームに基づく、ため、フレームワークが、アプリを生成する必要があります、`ClaimsIdentity`ユーザー。 `ClaimsIdentity`情報を持つユーザーの名前など、ユーザーのすべての要求について age し、ユーザーが所属するロール。 この段階で、ユーザーのクレームを追加することもできます。

OWIN`AuthenticationManager.SignIn`メソッドでは、渡します、`ClaimsIdentity`し、ユーザーがサインインします。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)する他のプロパティを追加する方法を示しています、`ApplicationUser`クラスです。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装するしていないことを確認すると、新しいユーザーが登録電子メールを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。 しないようにする、ディスカッション フォーラムするいたと仮定`"bob@example.com"`としての登録から`"joe@contoso.com"`です。 電子メールの確認なし`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定します`"bib@example.com"`いなかったが検出されました。 これは、と彼は、アプリは、正しいメール アドレスが見つからないため、パスワードの回復を使用できなくします。 確認の電子メールが bot から制限の保護のみを提供し、決定スパム攻撃者から保護を提供しません、登録に使用できる多くの作業電子メール エイリアスが。次のサンプルでは、ユーザーことはできませんのアカウントが確認されるまで、パスワードを変更する (それらによりに登録されている電子メール アカウントで受信確認のリンクをクリックするとします)。このワークフローは、例を確認し、自分のプロファイルに変更されたときに、ユーザー、電子メールを送信する、管理者によって作成された新しいアカウントのパスワードをリセット リンクを送信するその他のシナリオに適用できます。 一般にする新しいユーザーが電子メール、SMS テキスト メッセージ、または別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐ。 <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>完全なサンプルの構築

このセクションでは、させるより完全なサンプルをダウンロードする NuGet を使用します。

1. 新しい***空***ASP.NET Web プロジェクトです。
2. パッケージ マネージャー コンソールで、次を入力して、次のコマンド。 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 このチュートリアルで使用されます[SendGrid](http://sendgrid.com/)電子メールを送信します。 `Identity.Samples`パッケージは操作するコードをインストールします。
3. 設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。
4. クリックすると、アプリを実行して、テストの作成にローカル アカウント、**登録**をリンクして、登録フォームを投稿します。
5. 確認の電子メールをシミュレートするデモ電子メールのリンクをクリックします。
6. このサンプルからデモ電子メール リンク確認コードを削除 (、`ViewBag.Link`アカウント コント ローラー内のコード。 参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。

> [!NOTE]
> 警告: このサンプルでは、セキュリティ設定のいずれかを変更すると、運用アプリが行われた変更を明示的に呼び出すのセキュリティ監査を受ける必要があります。


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>アプリでコードを調べる\_Start\IdentityConfig.cs

サンプルは、アカウントを作成し、それを追加する方法を示します、 *Admin*ロール。 サンプルの電子メールは、管理者アカウントに使用する電子メールを置き換えてください。 管理者アカウントを作成するには、今すぐ最も簡単な方法は、プログラム的に、`Seed`メソッドです。 ツールがあると、将来の作成し、ユーザーとロールを管理することを許可する予定です。 サンプル コードには、ユーザーおよびロールを作成および管理することが、ロールとユーザー管理ページを実行する管理者アカウントがあります。 このサンプルでは、データベースがシード処理時に、管理者アカウントが作成されます。

パスワードを変更し、電子メール通知を受信できるアカウントに名前を変更します。

> [!WARNING]
> セキュリティ - ソース コード内の機密データはストアことはありません。

前に述べたよう、`app.CreatePerOwinContext`スタートアップ クラス内の呼び出しへのコールバックの追加、`Create`アプリ DB コンテンツ、ユーザー マネージャーと役割マネージャー クラスのメソッドです。 OWIN パイプラインの呼び出し、`Create`これらのメソッドは、要求ごとにクラスし、各クラスのコンテキストを格納します。 アカウント コント ローラーは、(を OWIN コンテキストを含む)、HTTP コンテキストからユーザー マネージャーを公開します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

ユーザーがローカル アカウントを登録すると、`HTTP Post Register`メソッドが呼び出されます。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

上記のコードでは、モデル データを使用して、電子メールおよび入力したパスワードを使用して、新しいユーザー アカウントを作成します。 電子メール エイリアスがデータ ストア内にある場合は、アカウントの作成に失敗して、フォームが再度表示されます。 `GenerateEmailConfirmationTokenAsync`メソッドは、セキュリティで保護された確認トークンを作成し、ASP.NET Identity のデータ ストアに保存します。 [Url.Action](https://msdn.microsoft.com/en-us/library/dd505232(v=vs.118).aspx)メソッドは、リンクを含む、作成、`UserId`および確認トークン。 このリンクは、ユーザーに電子メールで送信し、ユーザーが自分のアカウントを確認するには、その電子メール アプリでリンクをクリックできます。

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>確認の電子メールを設定します。

移動して、 [Azure SendGrid のサインアップ ページ](https://azure.microsoft.com/en-us/gallery/store/sendgrid/sendgrid-azure/)無料アカウントを登録するとします。 SendGrid を構成するのには、次のようなコードを追加します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> 電子メール クライアントには、頻繁にテキスト メッセージ (HTML なし) のみがそのまま使用します。 メッセージ テキストと HTML を指定する必要があります。 サンプルでは、SendGrid 上、これは、`myMessage.Text`と`myMessage.Html`上に示したコードです。


次のコードは、電子メールを使用して送信する方法を示します、 [MailMessage](https://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx)クラス`message.Body`リンクのみを返します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> セキュリティ - ソース コード内の機密データはストアことはありません。 アカウントと資格情報は、appSetting に格納されます。 Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。 参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。


アプリを実行するには、登録電子メール エイリアスを持つリンクをクリックすることを確認して、電子メールで、SendGrid の資格情報を入力します。 これを行う方法について、 [Outlook.com](http://outlook.com)電子メール アカウントは、「John Atten [c# SMTP ホスト構成に Outlook.Com SMTP](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx)彼と[ASP.NET Identity 2.0: アカウントの検証を設定2 要素認証と](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)ポストします。

ユーザーがクリックした後、**登録**検証トークンを含む確認電子メールがユーザーの電子メール アドレスに送信ボタンをクリックします。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

ユーザーには、自分のアカウントの確認トークンを使用して電子メールが送信されます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>コードを調べます

次のコードは `POST ForgotPassword` メソッドを示します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

ユーザーの電子メール アドレスが確認されていない場合、メソッドはサイレント モードで失敗します。 エラーが無効な電子メール アドレス、ポストされた場合、悪意のあるユーザーを攻撃する有効なユーザー Id (電子メールの別名) を検索するその情報を使用できます。

次のコードは、`ConfirmEmail`でアカウント コント ローラー、ユーザーに電子メールを送信するために確認のリンクをクリックしたときに呼び出されるメソッド。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

パスワードを忘れたトークンが使用されるは無効になります。 次のコードの変更、`Create`メソッド (で、*アプリ\_Start\IdentityConfig.cs*ファイル) 3 時間に期限切れにトークンを設定します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

上記のコードでは、パスワードを忘れてしまったと電子メール確認トークン有効期限が切れる 3 時間にします。 既定値`TokenLifespan`は 1 日です。

次のコードは、電子メールの確認方法を示しています。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 アプリをより安全にするには、ASP.NET Identity には、2 要素認証 (2 fa) がサポートしています。 参照してください[ASP.NET Identity 2.0: アカウントの検証と 2 要素認証を設定する](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)John Atten でします。 ログイン パスワード試行の失敗では、アカウントのロックアウトを設定できますが、その手法では、現在のログインの影響を受け[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトされます。 アカウントのロックアウトを 2 fa でのみ使用することをお勧めします。  
<a id="addRes"></a>

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Id のカスタムの記憶域プロバイダーの概要](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。
- [ASP.NET MVC と Id 2.0: 基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。
- [ASP.NET Id の概要](../getting-started/introduction-to-aspnet-identity.md)
- [Announcing ASP.NET Identity 2.0.0 の RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi でします。
