---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: アカウントの確認とパスワードの回復では、ASP.NET Identity (c#) |Microsoft Docs
author: HaoK
description: このチュートリアルを完了する必要がありますを実行する前に、ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成します。 このチュートリアルには.
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 47dc2c1044a5964624ba2f8af4f174a2fd99d3e8
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236407"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>ASP.NET Identity での確認とパスワードの回復をアカウント (C#)

> このチュートリアルを実行する前に行う必要がありますまず[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成する](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)します。 このチュートリアルでは、詳細が含まれていて、ローカル アカウントの確認の電子メールを設定し、ASP.NET Identity で忘れたパスワードをリセットできるようにする方法が表示されます。

ローカル ユーザー アカウントが、ユーザー、アカウントのパスワードを作成する必要があり、そのパスワードが、web アプリで (安全に) 格納されています。 ASP.NET Identity では、ソーシャル アカウントは、アプリのパスワードを作成するユーザーを必要としないこともできます。 [ソーシャル アカウント](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)(Google、Twitter、Facebook、または Microsoft) などのサード パーティを使用してユーザーを認証します。 このトピックでは、次の項目について説明します。

- [ASP.NET MVC アプリの作成](#createMvc)と ASP.NET Identity 機能を検証します。
- [ビルド Id サンプル](#build)
- [確認の電子メール設定します。](#email)

新しいユーザーは、ローカル アカウントを作成します。 電子メールのエイリアスを登録します。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

登録 ボタンを選択するには、メール アドレスの検証トークンを含む確認メールが送信されます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

ユーザーは自分のアカウントの確認トークンを使用して電子メールで送信されます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

リンクを選択すると、アカウントを確認します。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>パスワード回復/リセット

自分のパスワードを忘れた場合、ローカル ユーザーがパスワードのリセットを有効にするセキュリティ トークンが、電子メール アカウントに送信されることができます。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
ユーザーは、自分のパスワードをリセットできるようにリンクを含む電子メールがすぐされます。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
リンクを選択するは、リセット ページに移動されます。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
選択すると、**リセット**ボタンは、パスワードがリセットされたことを確認します。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>ASP.NET Web アプリを作成する

インストールと実行によって開始[Visual Studio 2017](https://visualstudio.microsoft.com/)します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームは ASP.NET Identity をもサポートするため、web フォーム アプリで同じ手順を実行できます。
2. 認証の変更**個々 のユーザー アカウント**します。
3. アプリを実行し、選択、**登録**リンクし、ユーザーを登録します。 この時点では、電子メールの検証のみ、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
4. サーバー エクスプ ローラーに移動します。**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし**テーブル定義を開く**します。

    次の図は、`AspNetUsers`スキーマ。

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. 右クリックし、 **AspNetUsers**テーブルを選択**テーブル データの表示**します。  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   この時点で、電子メールが確認されていません。

ASP.NET Identity の既定のデータ ストアは、Entity Framework が、他のデータ ストアを使用して、フィールドを追加することを構成することができます。 参照してください[資料](#addRes)このチュートリアルの最後のセクション。

[OWIN startup クラス](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)( *Startup.cs* )、アプリが開始し、呼び出すときに呼び出される、`ConfigureAuth`メソッド*アプリ\_Start\Startup.Auth.cs*、これにより、OWIN パイプラインを構成して、ASP.NET Identity を初期化します。 `ConfigureAuth` メソッドを調べます。 各`CreatePerOwinContext`呼び出しコールバックを登録します (に保存されている、 `OwinContext`) 指定した型のインスタンスを作成する要求あたり 1 回呼び出すされます。 コンス トラクターで、ブレークポイントを設定して`Create`各型のメソッド (`ApplicationDbContext, ApplicationUserManager`) 要求のたびに呼び出されることを確認します。 インスタンスを`ApplicationDbContext`と`ApplicationUserManager`アプリケーション全体でアクセスできます。 OWIN コンテキストに格納されます。 ASP.NET Identity では、cookie ミドルウェアを OWIN パイプラインにフックします。 詳細については、次を参照してください。 [ASP.NET Identity で UserManager クラスの要求の有効期間管理あたり](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)します。

新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更すると、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。 メモ、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。 セキュリティ クッキーに格納されていない、`AspNetUsers`テーブル (または任意の場所で Identity DB)。 使用して、セキュリティ cookie トークンが自己署名[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。

Cookie ミドルウェアは、各要求の cookie を確認します。 `SecurityStampValidator`メソッドで、`Startup`クラスが、DB をヒットして、定期的にセキュリティ スタンプをチェックに指定された、`validateInterval`します。 これは、セキュリティ プロファイルを変更しない限り (サンプル) では 30 分のみ発生します。 データベースへのトリップを最小限に抑える、30 分間隔が選択されました。 参照してください、 [2 要素認証のチュートリアル](index.md)の詳細。

コードでは、コメント、`UseCookieAuthentication`メソッドは、cookie 認証をサポートしています。 `SecurityStamp`フィールドと関連付けられているコードは、強固なセキュリティをアプリは、のパスワードを変更するときにログインする必要がログインに使用したブラウザーから。 `SecurityStampValidator.OnValidateIdentity`メソッドは、パスワードを変更または外部ログインを使用するときに使用される、ユーザーがログインすると、セキュリティ トークンを検証するアプリを使用できます。 これは、古いパスワードで生成されたトークン (cookie) が無効にすることを確認する必要です。 サンプル プロジェクトで変更した場合のユーザーに対して新しいトークンでは、ユーザーのパスワードが生成された、前のすべてのトークンが無効にし、`SecurityStamp`フィールドが更新されます。

Id システムを許可するアプリを構成するため、ユーザーのセキュリティ プロファイルが変更されたとき (たとえば、ログインが関連付けられたユーザーが自分のパスワードまたは変更を変更する場合 (などで、Facebook、Google、Microsoft アカウントなど)、すべてから、ユーザーがログインブラウザーのインスタンス。 次の画像など、[シングル サインアウトのサンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)アプリ、ユーザーが 1 つのボタンを選択して (この場合、IE、Firefox、Chrome は) 内のすべてのブラウザー インスタンスからサインアウトします。 または、サンプルでは、特定のブラウザー インスタンスからログアウトのみできます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[シングル サインアウトのサンプル](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)アプリは、ASP.NET Identity を使用すると、セキュリティ トークンの再生成する方法を示しています。 これは、古いパスワードで生成されたトークン (cookie) が無効にすることを確認する必要です。 この機能は、アプリケーションにセキュリティの追加のレイヤーを提供します。パスワードを変更するときに、このアプリケーションにログインした場所にログインされます。

*アプリ\_Start\IdentityConfig.cs*ファイルが含まれています、 `ApplicationUserManager`、`EmailService`と`SmsService`クラス。 `EmailService`と`SmsService`各実装クラス、`IIdentityMessageService`インターフェイス、電子メールと SMS を構成するには、各クラスに共通のメソッドがあるようにします。 このチュートリアルを使用して電子メール通知を追加する方法だけでは[SendGrid](http://sendgrid.com/)SMTP とその他のメカニズムを使用して電子メールを送信することができます。

`Startup`クラスは、ソーシャル ログイン (Facebook、Twitter など) は、拙著のチュートリアルを参照してください。 追加するボイラー プレートも含まれています。 [Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)の詳細。

確認、`ApplicationUserManager`クラスは、ユーザーの id 情報を含むし、次の機能を構成します。

- パスワードの強度の要件。
- ユーザーのロックアウト (試行と時刻)。
- 2 要素認証 (2 fa)。 別のチュートリアルでは、2 fa と SMS を取り上げる予定です。
- 電子メールと SMS サービスをフックします。 (取り上げます SMS 別のチュートリアルでは)。

`ApplicationUserManager`ジェネリック クラス`UserManager<ApplicationUser>`クラス。 `ApplicationUser` 派生した[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)します。 `IdentityUser` ジェネリックから派生した`IdentityUser`クラス。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

上記のプロパティのプロパティと一致し、`AspNetUsers`前に示したテーブル。

ジェネリック引数`IUser`主キーの種類を使用してクラスを派生できます。 参照してください、 [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)サンプル文字列から int または GUID に主キーを変更する方法を示します。

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) で定義されている*Models\IdentityModels.cs*として。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

上記の強調表示されたコードを生成、 [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)します。 ASP.NET Identity と OWIN クッキー認証は、クレーム ベース、したがって、framework が、アプリを生成する必要があります、`ClaimsIdentity`ユーザー。 `ClaimsIdentity` 情報を持つユーザーの名など、ユーザーのすべての要求についての時代し、ユーザーが所属するロール。 この段階でユーザーの複数のクレームを追加することもできます。

OWIN`AuthenticationManager.SignIn`メソッドで、`ClaimsIdentity`とし、ユーザー サインインします。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)する追加のプロパティを追加する方法を示しています、`ApplicationUser`クラス。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装していないことを確認する新しいユーザーに登録メールを確認することをお勧め (つまりに登録していない他のユーザーの電子メールで)。 ようにしたい、ディスカッション フォーラムが`"bob@example.com"`としての登録から`"joe@contoso.com"`します。 電子メールの確認を求めず`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定`"bib@example.com"`気付いていなかったとパスワードの回復を使用して、アプリは、正しいメール アドレスがある見つからないため、彼はできません。 確認の電子メールがボットから制限の保護のみを提供し、決定スパムから保護を提供しません、登録に使用できる多くの作業用電子メールの別名が。次の例では、ユーザーは (それらによりに登録した電子メール アカウントで受信した確認リンクを選択します。) 自分のアカウントが確認されているまで、自分のパスワードを変更することができません。この作業の流れは、例を確認し、これに自分のプロファイルを変更したときに、ユーザーに電子メールを送信、管理者によって作成された新しいアカウントのパスワードをリセット リンクを送信する他のシナリオに適用できます。 新しいユーザーが電子メール、SMS テキスト メッセージまたは別のメカニズムによって確認されて前に、web サイトにデータを投稿するを防ぐために一般的にします。 <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>完全なサンプルをビルドします。

このセクションでは、皆様より完全なサンプルをダウンロードする NuGet を使用します。

1. 新規作成***空***ASP.NET Web プロジェクト。
2. パッケージ マネージャー コンソールで、次のコマンドを入力します。 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   このチュートリアルで使用します[SendGrid](http://sendgrid.com/)電子メールを送信します。 `Identity.Samples`パッケージで使用されるコードをインストールします。
3. 設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。
4. ローカル アカウントの作成をアプリを実行してテストを選択すると、**登録**リンク、および登録フォームを投稿します。
5. 確認の電子メールをシミュレートするデモ電子メール リンクを選択します。
6. サンプルからデモ電子メール リンク確認コードを削除 (、 `ViewBag.Link` account コント ローラー コード。 参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。

> [!WARNING]
> このサンプルでは、セキュリティ設定を変更する場合は、運用アプリを加えられた変更を明示的に呼び出すのセキュリティ監査を受ける必要があります。


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>アプリでコードを調べる\_Start\IdentityConfig.cs

アカウントを作成しに追加する方法を示します、*管理者*ロール。 サンプルの電子メールを管理者アカウントを使用する電子メールと置き換える必要があります。 管理者アカウントを作成するには、現在最も簡単な方法は、プログラム的に、`Seed`メソッド。 作成し、ユーザーとロールを管理することがツールを将来があることと思います。 サンプル コードには、ユーザーとロールを作成および管理することは、ロールとユーザー管理ページを実行する管理者アカウントがあります。 このサンプルでは、DB のシード処理時に、管理者アカウントが作成されます。

パスワードを変更し、電子メール通知を受信できるアカウントに名前を変更します。

> [!WARNING]
> セキュリティ - ソース コード内の機密データは store ことはありません。

以前は、説明したように、 `app.CreatePerOwinContext` startup クラス内の呼び出しにコールバックを追加する、`Create`アプリ DB コンテンツ、ユーザー マネージャーと役割マネージャー クラスのメソッド。 OWIN パイプラインの呼び出し、`Create`これらのメソッドは要求ごとにクラスし、コンテキストの各クラスを格納します。 アカウント コント ローラーには、HTTP コンテキスト (を OWIN コンテキストを含む) からユーザーのマネージャーが公開しています。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

ユーザーがローカルのアカウントを登録すると、`HTTP Post Register`メソッドが呼び出されます。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

上記のコードでは、モデル データを使用して電子メールとパスワードの入力を使用して、新しいユーザー アカウントを作成します。 電子メール エイリアスが、データ ストア内にある場合は、アカウントの作成が失敗して、フォームが再び表示されます。 `GenerateEmailConfirmationTokenAsync`メソッドは、セキュリティで保護された確認トークンを作成し、ASP.NET Identity データ ストアに格納されます。 [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx)メソッドは、リンクを含む、作成、`UserId`および確認トークン。 このリンクは、ユーザーに電子メールで送信し、ユーザーが自分のアカウントを確認する、電子メール アプリのリンクを選択できます。

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>確認の電子メール設定します。

移動して、 [Azure SendGrid へのサインアップ ページ](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)無料アカウントで登録するとします。 SendGrid を構成するために、次のようなコードを追加します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> 電子メール クライアントには、頻繁にテキスト メッセージ (HTML なし) のみがそのまま使用します。 テキストおよび HTML でメッセージを指定する必要があります。 上記の SendGrid サンプルでは、これは、`myMessage.Text`と`myMessage.Html`上記のコード。


次のコードは、電子メールを使用して送信する方法を示します、 [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)クラス`message.Body`リンクのみを返します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> セキュリティ - ソース コード内の機密データは store ことはありません。 アカウントと資格情報は、appSetting で格納されます。 Azure では安全に保管するこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portal でのタブ。 参照してください[ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。


アプリを実行するには、電子メール エイリアス登録リンクを選択して確認の電子メール、SendGrid の資格情報を入力します。 これを行う方法について、 [Outlook.com](http://outlook.com)電子メール アカウントは、John Atten の[ C# Outlook.Com SMTP ホスト用の SMTP 構成](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx)彼と[ASP.NET Identity 2.0。アカウントの検証を設定し、2 要素認証](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)投稿します。

ユーザーが選択されると、**登録**のメール アドレスの検証トークンを含む確認の電子メールが送信されるボタンをクリックします。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

ユーザーは自分のアカウントの確認トークンを使用して電子メールで送信されます。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>コードを確認します

次のコードは `POST ForgotPassword` メソッドを示します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

ユーザーの電子メール アドレスが確認されていない場合、メソッドはサイレント モードで失敗します。 エラーは、無効な電子メール アドレスにポストされた、悪意のあるユーザーは、攻撃するのに有効なユーザー Id (電子メールのエイリアス) を検索するのに情報を使用できます。

次のコードは、`ConfirmEmail`ユーザーに電子メールを送信するために確認リンクを選択するときに呼び出される、account コント ローラー メソッド。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

忘れたパスワード トークンが使用されるは無効になります。 次のコードの変更、`Create`メソッド (で、*アプリ\_Start\IdentityConfig.cs*ファイル) を 3 時間で期限切れのトークンを設定します。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

上記のコードでは、パスワードを忘れてしまったと、電子メール確認トークンは 3 時間で期限切れ。 既定の`TokenLifespan`1 日になっています。

次のコードは、電子メールの確認方法を示しています。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 アプリのセキュリティを強化するには、ASP.NET Identity では 2 要素認証 (2 fa) をサポートしています。 参照してください[ASP.NET Identity 2.0。アカウントの検証と 2 要素認証を設定する](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)John Atten でします。 ログイン パスワードの試行の失敗では、アカウントのロックアウトを設定できますが、そのアプローチにより、ログインを受けやすい[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトします。 2 fa でのみアカウントのロックアウトを使用することをお勧めします。  
<a id="addRes"></a>

## <a name="additional-resources"></a>その他の技術情報

- [ASP.NET Identity のカスタム ストレージ プロバイダーの概要](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。
- [ASP.NET MVC と Id 2.0:基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。
- [ASP.NET Identity 入門](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET Identity 2.0.0 の RTM の発表](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)Pranav Rastogi が。
