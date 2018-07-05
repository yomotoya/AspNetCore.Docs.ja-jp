---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: メンバーシップ |Microsoft Docs
author: microsoft
description: ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルのビルド 1.x します。 ASP.NET フォーム認証では、incorp する便利な手段を提供しています.
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: f776ed628e206c06543589767ba364af3c76ae16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818218"
---
<a name="membership"></a>メンバーシップ
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルのビルド 1.x します。 ASP.NET フォーム認証では、ログイン フォームを ASP.NET アプリケーションに組み込むし、データベースまたは他のデータ ストアに対してユーザーを検証する便利な手段を提供します。


ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルのビルド 1.x します。 ASP.NET フォーム認証では、ログイン フォームを ASP.NET アプリケーションに組み込むし、データベースまたは他のデータ ストアに対してユーザーを検証する便利な手段を提供します。 FormsAuthentication クラスのメンバーでは、認証 cookie の処理、有効なログインを確認、ユーザーをサインアウトなどのログできるようにします。ただし、ASP.NET 1.x アプリケーションでフォーム認証を実装すると、大量のコードを要求できます。

ASP.NET 2.0 メンバーシップは、だけでフォーム認証を使用する場合より大きく進歩しています。 (メンバーシップは、フォーム認証と組み合わせると、最も堅牢ながフォーム認証の使用は必須ではありません)。すぐにわかるよう、多くのコードをまったく記述しなくても、強力なメンバーシップ システムを実装するために ASP.NET 2.0 の ASP.NET メンバーシップと login コントロールを使用できます。

## <a name="implementing-membership-in-aspnet-20"></a>ASP.NET 2.0 のメンバーシップを実装します。

メンバーシップは、次の 4 つの手順によって実装されます。 同様に実装できるオプションの構成と関連する多くのサブ手順があることに留意してください。 次の手順は、メンバーシップの構成の全体像を説明するものです。

1. メンバーシップ データベースを作成 (SQL Server として使用する場合、メンバーシップ ストアです。)
2. アプリケーション構成ファイルでのメンバーシップ オプションを指定します。 (メンバーシップは既定で有効です)。
3. 使用するメンバーシップ ストアの種類を決定します。 オプションがあります。 

    - Microsoft SQL Server (バージョン 7.0 以降)
    - Active Directory ストア
    - カスタム メンバーシップ プロバイダー
4. ASP.NET フォーム認証用のアプリケーションを構成します。 もう一度、メンバーシップは、フォーム認証を活用するために設計されていますが、フォーム認証の使用は必須ではありません。
5. メンバーシップのユーザー アカウントを定義し、必要な場合は、ロールを構成します。

## <a name="creating-the-membership-database"></a>メンバーシップ データベースを作成します。

場合は SQL Server 7.0 を使用しているか、後で、メンバーシップ ストアとして aspnet を使用することができます\_regsql ユーティリティ (使用可能な最も簡単に、Visual Studio .NET 2005 コマンド プロンプトから)、データベースを構成します。 Aspnet\_としてコマンド プロンプト ツールまたは GUI ウィザードを使用して、regsql ユーティリティを使用できます。 ウィザード メソッドは、データベースを構成する最も簡単な方法です。 ウィザードにアクセスするには、次のコマンドを実行だけです。

`aspnet_regsql W`

そのコマンドを実行すると次に示す ASP.NET SQL Server セットアップ ウィザードに表示されます。


![](membership/_static/image1.jpg)

**図 1**


ASP.NET SQL Server セットアップ ウィザードは、ウィザードで指定したインスタンスで、Web サイトを作成します。 ただし、ASP.NET は、データベースに接続する、machine.config ファイルで接続文字列が使用されます。 既定では、この接続文字列は、SQL Server 2005 のインスタンスをポイントは、SQL Server 2000 または SQL Server 7.0 のインスタンスを使用している、machine.config ファイルで接続文字列を変更する必要があります。 その接続文字列は、ここに配置できます。

[!code-xml[Main](membership/samples/sample1.xml)]

残念ながら、接続文字列を変更しない場合は、ASP.NET は与えない記述的なエラー。 データベースを作成するいないと答えるから不満があがるは引き続きだけです。 上記の例で、ローカルの SQL Server 2000 インスタンスを指す接続文字列を変更しました。

## <a name="specifying-configuration-and-adding-users-and-roles"></a>構成および追加のユーザーとロールを指定します。

メンバーシップの構成では、次の手順では、アプリケーションの web.config ファイルに必要な情報を追加します。 Asp.net 1.x では、web.config ファイルを変更することがありますは困難でした lowerCamelCase の使用と Intellisense の欠如のため。 Visual Studio .NET 2005 により、タスクが構成ファイルでは、Intellisense を備えた非常に簡単ですが、ASP.NET 2.0 は、構成ファイルを編集するための Web インターフェイスを提供することでさらに、1 つのステップを移動します。

Web インターフェイスを起動するには、次に示すように、ソリューション エクスプ ローラー ツールバーで [ASP.NET 構成] ボタンをクリックします。 ログイン コントロールを挿入するときに表示されるポップアップを使用して、Web インターフェイスを起動することもできます。


![](membership/_static/image2.jpg)

**図 2**


これは、次に示す ASP.NET Web サイト管理ツールを起動します。 ASP.NET Web サイトの管理は、アプリケーション設定を管理しやすく 4 タブ インターフェイスです。 次のタブを使用できます。

- **Home**
- **セキュリティ**ユーザー、ロール、およびアクセスを構成します。
- **アプリケーション**アプリケーション設定を構成します。
- **プロバイダー**構成して、アプリケーションのメンバーシップ プロバイダーをテストします。

Web サイトの管理ツールでは、簡単に新しいユーザーを作成、新しいロールを作成して、ユーザーとロールを管理できます。 Windows インターフェイスでは、この機能を使用できません。 Windows インターフェイスでは承認の設定を簡単に定義と追加、削除、およびプロバイダー、Web サイト管理ツールではない機能の管理を行うことができます。

Windows インターフェイスを起動するには、インターネット インフォメーション サービス スナップインを開く、アプリケーションを右クリックし、プロパティを選択します。 ASP.NET タブをクリックし、構成の編集 ボタンをクリックします。 (有効にする構成の編集 ボタンの ASP.NET 2.0 では、アプリケーションを実行する必要があります。 構成できます ASP.NET のバージョン ASP.NET ダイアログ。)次に示すように、ASP.NET 構成の設定 ダイアログが表示されます。


![](membership/_static/image3.jpg)

**図 3**


[全般] タブで、接続文字列とアプリケーションの設定が一覧表示されます。 斜体の設定は親構成ファイル (machine.config ファイルまたは高いレベルの web.config) で定義され、斜体ではなく設定は、アプリケーション構成ファイルから。 場合は、設定を追加すると、削除、またはアプリケーション レベルでは、編集 ASP.NET は追加、削除、または継承元となる構成ファイルから設定を削除する代わりに、アプリケーション レベルの web.config で設定を変更します。

[認証] タブは、以下に示します。 これは、メンバーシップの設定を構成します。 メンバーシップ プロバイダーでの認証設定を作成し、ロール プロバイダーをここで構成できます。


![](membership/_static/image4.jpg)

**図 4**


## <a name="implementing-membership-in-your-application"></a>アプリケーションでのメンバーシップの実装

指定されたログオン コントロールを使用すること、アプリケーションで ASP.NET 2.0 メンバーシップを実装する最も簡単な方法です。 このメソッド コードをまったく記述せずに ASP.NET 2.0 メンバーシップの基本を実装することができます。

次のログオン コントロールは ASP.NET 2.0 です。

## <a name="login-control"></a>Login コントロール

Login コントロールは、他のユーザーがメンバーシップ システムにログインするためのインターフェイスを提供します。 ユーザー名とパスワード textboxt とログイン ボタンに提供します。 他の一般的ななど多くの機能をまだ行っていないため、アクセスする際に自動的にログインにユーザーにより、チェック ボックスをユーザーに登録リンクをパスワード関連語句などへのリンク。Login コントロールのすべての機能は、コントロールのプロパティを使用してカスタマイズできます。

Asp.net 1.x では、開発者は、かなりのフォーム認証を使用する場合は、ルックアップを実行するためのコードを記述する必要があります。 ASP.NET 2.0 のメンバーシップを持つ任意のコードをまったく記述しなくてもユーザーを検証できます。 ASP.NET は自動的にユーザーの検索処理を実行します。 (ASP.NET メンバーシップを使用せず、Login コントロールを使用する場合は、使用、 **OnAuthenticate**ユーザーを検証するメソッド)。

## <a name="loginview-control"></a>LoginView コントロール

LoginView コントロールが既定では 2 つのテンプレートを提供するテンプレート コントロールです。AnonymousTemplate し、LoggedInTemplate します。 表示されるテンプレートは、ユーザーがメンバーシップ システムにログインするかどうかによって決まります。 このコントロールは通常、ユーザー ログインしているときに、ユーザーがまだログインしていないときのログイン コントロール、LoginStatus コントロールやその他のログイン コントロールを表示する使用されます。 ASP.NET アプリケーションでロールの管理を使用する場合、LoginView コントロールは、ユーザー ロールに基づいて、特定のテンプレートを表示できます。 (より ASP.NET のロール管理については、後ほど。)

## <a name="passwordrecovery-control"></a>PasswordRecovery コントロール

PasswordRecovery コントロールでは、現在のパスワードを自分で電子メールを受信するか、自分のパスワードをリセットできます。 クリア テキストと暗号化されたパスワードを回復およびユーザーに電子メールで送信します。 パスワードがハッシュされる場合は回復できません。 代わりに、ユーザーは、パスワード リセットを実行する必要があります。

## <a name="loginstatus-control"></a>LoginStatus コントロール

現在ログオンしているユーザーにログインしていないユーザーにログイン インジケーターとログアウト インジケーターを表示する、LoginStatus コントロールが使用されます。 Request.IsAuthenticated プロパティは、表示するインジケーターの決定に使用されます。 LoginStatus コントロールによって表示されるインジケーターは、テキストを指定できます (を使用して実装、 **LoginText**と**LogoutText**プロパティ) やイメージ (を使用して実装、 **LoginImageUrl**と**LogoutImageUrl**プロパティです)。

によって指定された URL にそのユーザーをリダイレクトする、LoginStatus コントロールを使用して、ユーザーがログアウト、 **LogoutPageUrl**プロパティ。 そのプロパティが設定されていない場合は、現在のページが更新されます。 サイトはフォーム認証で保護されている可能性があります、ために、現在のページの更新は、ユーザーをサイトのログイン ページにリダイレクトされます。

## <a name="loginname-control"></a>LoginName コントロール

LoginName コントロールは、現在、サイトにログインしてユーザーのユーザー名を表示します。

## <a name="createuserwizard-control"></a>CreateUserWizard コントロール

CreateUserWizard コントロールは、メンバーシップ システムに登録する便利な方法でユーザーを提供します。 次に示すインターフェイスを使用して、(WizardSteps のコレクションとして実装) ステップを追加することができます。


![](membership/_static/image5.jpg)

**図 5**


CreateUserWizard は、ウィザード クラスから派生し、次のテンプレートを提供します、template 宣言されたコントロールを示します。

- **Header テンプレート**このテンプレートは、ウィザードのヘッダーの外観を制御します。
- **SidebarTemplate**このテンプレートは、ウィザードのサイド バーの外観を制御します。
- **StartNavigationTemplate**スタートの手順で、ウィザードのナビゲーションの外観が、このテンプレートを制御します。
- **StepNavigationTemplate**このテンプレートは、開始日または終了の手順ではないときのナビゲーション領域の外観を制御します。
- **FinishNavigationTemplate**このテンプレートは、[完了] 手順でのナビゲーション領域の外観を制御します。

さらに、ウィザードに追加する各手順では、ASP.NET では、ContentTemplate とそのステップの CustomNavigationTemplate の両方を含むカスタム テンプレート作成されます。 CreateUserWizard をカスタマイズする方法の詳細については、VS.NET 2005 のドキュメントを参照してください。

## <a name="changepassword-control"></a>ChangePassword コントロール

ChangePassword コントロールでは、自分のパスワードを変更することができます。 DisplayUserName プロパティが true (既定では false です) ログインしていないときに、ユーザーは自分のパスワードを変更できます。 場合、ユーザー*は*既にログインして DisplayUserName プロパティが true をユーザーがそのユーザーのユーザー ID を知っているが提供するためにログインしていない別のユーザーのパスワードを変更できるようなります。

ユーザーにログインすることがなくパスワードを変更できるようにする場合が必要で、ChangePassword コントロールが表示されるページは匿名アクセスを許可することに留意してください。 当然ながら、ユーザーが自分のパスワードを変更するには、古いパスワードを提供する必要があります。

## <a name="role-management"></a>ロール管理

ロール管理を使用すると、特定のロールにユーザーを割り当てるし、特定のファイルまたはそのロールに基づいたフォルダーへのアクセスを制限できます。 ロール管理では、プログラムによって使いのロールを確認または特定のロールのすべてのユーザーを確認して適宜応答できるように、API も提供します。

ロール管理は、ASP.NET メンバーシップの要件ではありませんもはメンバーシップ ロール管理を使用するために必要になります。 ただし、2 つは、互いを適切に補完し、場合、開発者は相互に組み合わせてそれらに使用します。

アプリケーションでロールの管理を有効にするには、web.config ファイルで、次の変更を行います。

[!code-xml[Main](membership/samples/sample2.xml)]

ときに、 **cacheRolesInCookie**属性に設定されて true の場合、ASP.NET キャッシュ クライアントの cookie にユーザー ロールのメンバーシップ。 これにより、ロール プロバイダーへの呼び出しなしに行われるロールの参照です。 いることを確認する開発者はこの属性を使用する場合、 **cookieProtection**属性がすべてに設定します。 (これは、既定の設定です)。これにより、cookie のデータが暗号化され、cookie の内容が変更されていないことを確認するのに役立ちます。 Web サイトの管理ツールを使用して、ロールを追加できます。 簡単にロールを定義し、これらの役割に基づいた、サイトの部分へのアクセスを構成し、ユーザー ロールを割り当てることができます。


![](membership/_static/image6.jpg)

**図 6**


上記のように、ロールの名前を入力するだけ、[役割の追加] をクリックして新しいロールを追加できます。 既存のロールを管理または既存のロールの一覧で、適切なリンクをクリックして削除できます。

ロールを管理する場合は、追加または次に示すようにユーザーを削除できます。


![](membership/_static/image7.jpg)

**図 7**


ロールのユーザーは、チェック ボックスをチェックして、特定のロールにユーザーを簡単に追加できます。 ASP.NET は、適切なエントリで、メンバーシップ データベースを自動的に更新されます。 アプリケーションのアクセス規則を構成するされます。 ASP.NET 1.x の開発者が使用してこれを行う使い慣れて、&lt;承認&gt;内の要素、web.config ファイルおよびそのオプションは、ASP.NET 2.0 で引き続き使用できます。 ただし、ツールを使用して、Web サイトの管理に示すように以下をアクセスを管理する方が簡単にルールします。


![](membership/_static/image8.jpg)

**図 8**


この場合は、管理フォルダーが強調表示されます (そのがわかりにくく、ツールが淡い灰色でそれを強調するため) と、管理者ロールにアクセスが許可されています。 その他のすべてのユーザーが拒否されました。 ルールを選択し、上へ移動し、下へ移動 ボタンを使用してルールを並べ替えますヘッドのアイコンをクリックすることができます。 ASP.NET と同様&lt;承認&gt;要素、規則が表示される順序で処理されます。 つまり、ショット上記の規則の順序を反転させた場合だれする必要がありますアクセス管理フォルダー ASP.NET が発生する最初の規則がフォルダーにすべてのユーザーを拒否する規則になるためです。

ASP.NET 2.0 では、アクセス規則を指定するフォルダーに web.config ファイルを追加します。 構成ファイルを使用して、または Web サイトの管理ツールを使用して、アクセス規則を編集できます。 つまり、Web サイトの管理ツールでは、使いやすい環境で使用される構成ファイルを編集できますインターフェイスだけです。

## <a name="using-roles-in-code"></a>コードでロールの使用

ロール管理用の API がバージョン以降変更されていない 1.x します。 **IsInRole**メソッドを使用して、ユーザーが特定のロールがあるか判断します。

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET では、現在のコンテキストのメンバーとして RolePrincipal インスタンスも作成します。 すべてのユーザーが次のように所属するロールを取得する RolePrincipal オブジェクトを使用できます。

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>LoginView コントロールで Rolegroup の使用

ロールの管理とメンバーシップについて理解したら、LoginView コントロールが ASP.NET 2.0 でこの機能を活用を受け取る方法について簡単に説明することができます。 既に説明したように、LoginView コントロールには既定では 2 つのテンプレートを含むテンプレート コントロールです。AnonymousTemplate し、LoggedInTemplate します。 内の LoginView タスク ダイアログ (下図参照) のリンクは、RoleGroups を編集することができます。


![](membership/_static/image9.jpg)

**図 9**


各 RoleGroup オブジェクトには、RoleGroup が適用されるロールを定義する文字列の配列が含まれています。 新しい RoleGroup を LoginView コントロールを追加するには、Rolegroup の編集リンクをクリックします。 上記の図では、管理者用の新しい RoleGroup を追加することがわかります。 その RoleGroup を選択して (RoleGroup[0]) ビュー ドロップダウン リストから、構成できます管理者ロールのメンバーにのみ表示されるテンプレート。 次の図に、Sales ロールおよび配布ロールのメンバーに適用される新しい RoleGroup を追加しました。 2 番目の RoleGroup LoginView タスク ダイアログ ボックスで、ビューのドロップダウン リストに追加され、そのテンプレートに追加されたものは、販売またはディストリビューション内のユーザーが表示されますロール。


![](membership/_static/image10.jpg)

**図 10**


## <a name="overriding-the-existing-membership-provider"></a>既存のメンバーシップ プロバイダーをオーバーライドします。

いくつかの ASP.NET メンバーシップの機能を拡張する方法があります。 まず、それを継承し、そのメソッドをオーバーライドして SqlMembershipProvider クラスの既存の機能を明らかに変更できます。 たとえば、ユーザーの作成時に、独自の機能を実装する場合は、SqlMembershipProvider から次のように継承するクラスを作成することができます。

[!code-csharp[Main](membership/samples/sample5.cs)]

その一方で、(たとえば、Access データベースにメンバーシップ情報の保存) を独自のプロバイダーを作成する場合、独自のプロバイダーを作成できます。

## <a name="creating-your-own-membership-provider"></a>独自のメンバーシップ プロバイダーを作成します。

独自のメンバーシップ プロバイダーを作成するには、MembershipProvider クラスから継承するクラスを作成する必要があります。 VB.NET を使用している場合、Visual Studio 2005 はすべてのメソッドをオーバーライドする必要のあるスタブを追加します。 C#、スタブを追加することに使用している場合します。

次をオーバーライドする必要があります。

- ApplicationName プロパティ
- ChangePassword 関数
- ChangePasswordQuestionAndAnswer 関数
- CreateUser 関数
- DeleteUser 関数
- EnablePasswordReset プロパティ
- EnablePasswordRetrieval プロパティ
- FindUsersByEmail 関数
- FindUsersByName 関数
- GetAllUsers 関数
- GetNumberOfUsersOnline 関数
- GetPassword 関数
- GetUser 関数
- GetUserNameByEmail 関数
- MaxInvalidPasswordAttempts プロパティ
- MinRequiredNonAlphanumericCharacters プロパティ
- MinRequiredPasswordLength プロパティ
- PasswordAttemptWindow プロパティ
- PasswordFormat プロパティ
- PasswordStrengthRegularExpression プロパティ
- RequiresQuestionAndAnswer プロパティ
- RequiresUniqueEmail プロパティ
- ResetPassword 関数
- ユーザー関数のロックを解除します。
- UpdateUser 関数
- ValidateUser 関数

これを c# 開発者として実装する非常にリストします。 簡単に実装は一切 VB.NET でクラスを作成して、コードを c# に変換する .NET Reflector などのツールを使用することもあります。

接続文字列とその他のプロパティを Initialize メソッドの既定値に設定する必要があります。 (Initialize メソッドは、プロバイダーが実行時に読み込まれたときに発生した)。Initialize メソッドの 2 番目のパラメーターは、型 System.Collections.Specialized.NameValueCollection がありへの参照、&lt;追加&gt;web.config ファイルで、カスタム プロバイダーに関連付けられている要素。 そのエントリは、次のようになります。

[!code-xml[Main](membership/samples/sample6.xml)]

Initialize メソッドの例を次に示します。

[!code-csharp[Main](membership/samples/sample7.cs)]

ログイン フォームを提出するとき、ユーザーを検証するためには、ValidateUser メソッドを使用する必要があります。 このメソッドは、ログイン コントロールでのログイン ボタンをクリックすると発生します。 このメソッドでユーザーの検索を行うコードが配置されます。

ご覧のように、独自のメンバーシップ プロバイダーの作成は難しくなく、ASP.NET 2.0 のこの強力な機能を拡張することができます。
