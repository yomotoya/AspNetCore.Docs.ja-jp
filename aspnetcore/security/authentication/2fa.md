---
title: "SMS と 2 要素認証"
author: rick-anderson
description: "ASP.NET Core での 2 要素認証 (2 fa) を設定する方法を示しています。"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 7bca1c6249bebe84b532b652ab736186f35c50ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="two-factor-authentication-with-sms"></a>SMS と 2 要素認証

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[スイス開発者](https://github.com/Swiss-Devs)

このチュートリアルの ASP.NET Core 対象 1.x のみです。 参照してください[ASP.NET Core でのアプリの認証子の QR コードを有効にすると生成](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 以降。

このチュートリアルでは、SMS を使用した 2 要素認証 (2 fa) を設定する方法を示します。 手順が示されて[twilio](https://www.twilio.com/)と[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)、他の SMS プロバイダーを使用することができます。 完了したことをお勧め[アカウントの確認とパスワードの回復](accconfirm.md)このチュートリアルを開始する前にします。

ビュー、[完成したサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)です。 [ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)です。

## <a name="create-a-new-aspnet-core-project"></a>新しい ASP.NET Core プロジェクトを作成します。

という名前の新しい ASP.NET Core web アプリを作成する`Web2FA`個々 のユーザー アカウントにします。 指示に従って、 [ASP.NET Core アプリケーションでは SSL を適用する](xref:security/enforcing-ssl)を設定し、SSL を要求します。

### <a name="create-an-sms-account"></a>SMS アカウントを作成します

例については、SMS アカウントの作成[twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)です。 認証資格情報を記録 (twilio の: accountSid と ASPSMS 用の authToken: ユーザー キーとパスワード)。

#### <a name="figuring-out-sms-provider-credentials"></a>SMS プロバイダーの資格情報を見つけ出し

**Twilio:**  
Twilio アカウントの [ダッシュ ボード] タブから、コピー、**アカウント SID**と**認証トークン**です。

**ASPSMS:**  
アカウントの設定に移動**ユーザー キー**コピーと共に使用して、**パスワード**です。

キー内のシークレット マネージャー ツールを使用してこれらの値は後で格納`SMSAccountIdentification`と`SMSAccountPassword`です。

#### <a name="specifying-senderid--originator"></a>SenderID を指定する/発信元

**Twilio:**  
数値 タブで、コピー、Twilio**電話番号**です。 

**ASPSMS:**  
ロックを解除発信者メニュー内で 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。 

キー内のシークレット マネージャー ツールを使用してこの値は後で格納`SMSAccountFrom`です。


### <a name="provide-credentials-for-the-sms-service"></a>SMS サービスの資格を情報します。

使用して、[オプション パターン](xref:fundamentals/configuration/options)ユーザー アカウントとキーの設定にアクセスします。 

   * セキュリティで保護された SMS キーを取得するクラスを作成します。 このサンプルで、`SMSoptions`でクラスを作成、 *Services/SMSoptions.cs*ファイル。

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

設定、 `SMSAccountIdentification`、`SMSAccountPassword`と`SMSAccountFrom`で、[シークレット マネージャー ツール](xref:security/app-secrets)です。 例:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* SMS プロバイダーの NuGet パッケージを追加します。 パッケージ マネージャー コンソール (PMC) を実行します。

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* コードを追加、 *Services/MessageServices.cs* SMS を有効にするファイル。 Twilio または ASPSMS セクションのいずれかを使用します。


**Twilio:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>使用するスタートアップを構成します。`SMSoptions`

追加`SMSoptions`のサービス コンテナーに、`ConfigureServices`メソッドで、 *Startup.cs*:

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>2 要素認証を有効にします。

開く、 *Views/Manage/Index.cshtml* Razor ファイルの表示と削除 で、コメント文字 (つまり、マークアップには、除外したりはありません)。

## <a name="log-in-with-two-factor-authentication"></a>2 要素認証を使用してログインします。

* アプリを実行して、新しいユーザーの登録

![Web アプリケーション登録の Microsoft Edge でビューを開く](2fa/_static/login2fa1.png)

* アクティブ化するユーザー名をタップして、`Index`管理コント ローラー アクション メソッド。 電話番号の順にタップ**追加**リンクします。

![ビューを管理します。](2fa/_static/login2fa2.png)

* 確認コードを受信してタップ、する電話番号を追加する**確認コードを送信**です。

![ページの 電話番号を追加します。](2fa/_static/login2fa3.png)

* 確認コードと共にテキスト メッセージが表示されます。 タップし、それを入力**送信**

![ページの 電話番号を確認してください。](2fa/_static/login2fa4.png)

テキスト メッセージが表示されない場合は、twilio ログ ページを参照してください。

* 電話番号が正常に追加された、管理ビューを示しています。

![ビューを管理します。](2fa/_static/login2fa5.png)

* タップ**を有効にする**2 要素認証を有効にします。

![ビューを管理します。](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>2 要素認証のテスト

* ログオフします。

* ログイン。

* 認証の 2 番目の要素を提供する必要があるために、ユーザー アカウントは、2 要素認証を有効になります。 このチュートリアルでは、電話番号の検証を有効にします。 組み込みのテンプレートを使用して、2 つ目の要素として電子メールで送信を設定することもします。 QR コードなど、認証の他の 2 番目の要素を設定することができます。 タップ**送信**です。

![確認コード ビューを送信します。](2fa/_static/login2fa7.png)

* SMS メッセージで取得するコードを入力します。

* クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、同じデバイスおよびブラウザーを使用するときにログオンする必要があるからです。 2 fa を有効にしてをクリックすると**このブラウザーに覚えて**が提供されます 2 fa の強力な保護で悪意のあるユーザーがデバイスへのアクセスがあるない限り、アカウントにアクセスしようとしているからです。 これは、定期的に使用する任意のプライベート デバイスで行うことができます。 設定して**このブラウザーに覚えて**を定期的に使用しないデバイスから 2 fa の追加のセキュリティを取得する、および独自のデバイスで 2 fa を通過する必要がないで利便性を取得します。

![ビューを確認してください。](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>ブルート フォース攻撃から保護するためのアカウントのロックアウト

2 fa でアカウントのロックアウトを使用することをお勧めします。 (ローカル アカウントまたはアカウントのソーシャル) 経由ユーザーがログオン後に 2 fa に失敗した場合はそれぞれが格納されている場合 (既定値は 5) の最大試行回数に達すると、ユーザーはロックアウトを 5 分間 (で時間のロックアウトを設定することができます`DefaultAccountLockoutTimeSpan`)。 次に、10 の試行が失敗した後 10 分間ロックアウトにアカウントを構成します。

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
