---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "メンバーシップ |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルの構築 1.x です。 ASP.NET フォーム認証では、incorp する便利な手段を提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="membership"></a>メンバーシップ
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルの構築 1.x です。 ASP.NET フォーム認証では、ログイン フォームを ASP.NET アプリケーションに組み込むし、データベースまたはその他のデータ ストアに対してユーザーを検証する便利な手段を提供します。


ASP.NET から ASP.NET メンバーシップが、成功すると、フォーム認証モデルの構築 1.x です。 ASP.NET フォーム認証では、ログイン フォームを ASP.NET アプリケーションに組み込むし、データベースまたはその他のデータ ストアに対してユーザーを検証する便利な手段を提供します。 FormsAuthentication クラスのメンバーでは、認証 cookie の処理、有効なログインを確認などをユーザーのログインにできます。ただし、ASP.NET 1.x アプリケーションでフォーム認証を実装すると、かなりの量のコードを要求できます。

ASP.NET 2.0 のメンバーシップは、単独でフォーム認証を使用する場合より大幅に進歩がします。 (メンバーシップは、フォーム認証と組み合わせると、最も堅牢ながフォーム認証の使用は必須ではありません)。すぐにわかるように、多くのコードをまったく記述しなくても、強力なメンバーシップ システムを実装するのに ASP.NET 2.0 の ASP.NET メンバーシップとログイン コントロールを使用できます。

## <a name="implementing-membership-in-aspnet-20"></a>ASP.NET 2.0 でのメンバーシップを実装します。

メンバーシップは、次の 4 つの手順によって実装されます。 同様に実装できるオプションの構成だけでなく、含まれている多くのサブ手順があることに注意してください。 メンバーシップの構成の全体像を説明するためには、次の手順が意図したものです。

1. メンバーシップ データベースの作成 (SQL Server として使用する場合、メンバーシップ ストアです。)
2. アプリケーション構成ファイルでは、メンバーシップ オプションを指定します。 (メンバーシップは既定で有効にします。)
3. 使用するメンバーシップ ストアの種類を決定します。 オプションは次のとおりです。 

    - Microsoft SQL Server (バージョン 7.0 以降)
    - Active Directory ストア
    - カスタム メンバーシップ プロバイダー
4. ASP.NET フォーム認証用のアプリケーションを構成します。 もう一度、メンバーシップは、フォーム認証に活用するために設計されていますが、フォーム認証の使用は必須ではありません。
5. メンバーシップのユーザー アカウントを定義し、必要な場合は、ロールを構成します。

## <a name="creating-the-membership-database"></a>メンバーシップ データベースを作成します。

場合は SQL Server 7.0 を使用しているか、後で、メンバーシップ ストアとして aspnet を使用することができます\_regsql ユーティリティ (使用可能な最も簡単に、Visual Studio .NET 2005 コマンド プロンプトから) データベースを構成します。 Aspnet\_regsql ユーティリティは、コマンド プロンプト ツールとして、またはフル インストール ウィザードを使用して、使用できます。 ウィザード メソッドは、データベースを構成する最も簡単な方法です。 ウィザードにアクセスするには、次のコマンドを実行するだけ。

`aspnet_regsql W`

そのコマンドを実行すると、次のようにするは、ASP.NET SQL Server のセットアップ ウィザードが表示されます。


![](membership/_static/image1.jpg)

**図 1**


ASP.NET SQL Server セットアップ ウィザードでは、ウィザードで指定したインスタンスで、Web サイトを作成します。 ただし、ASP.NET は、データベースへの接続に、machine.config ファイルに接続文字列が使用されます。 既定では、この接続文字列が指す、SQL Server 2005 インスタンスのため、SQL Server 2000 または SQL Server 7.0 のインスタンスを使用している場合は、machine.config ファイル内の接続文字列を変更する必要があります。 その接続文字列は、ここで配置できます。

[!code-xml[Main](membership/samples/sample1.xml)]

残念ながら、接続文字列を変更しない場合は、ASP.NET 知ることはできません、わかりやすいエラーです。 データベースを作成していないことを示す不満をだけ続行されます。 上記の例では、ローカルの SQL Server 2000 のインスタンスを指す接続文字列を変更すればいます。

## <a name="specifying-configuration-and-adding-users-and-roles"></a>構成および追加のユーザーとロールを指定します。

メンバーシップの構成の次の手順では、アプリケーションの web.config ファイルに必要な情報を追加します。 ASP.NET で 1.x では、web.config ファイルを変更することもあります困難でした lowerCamelCase を使用して、Intellisense がないのためです。 Visual Studio .NET 2005 では、構成ファイルの Intellisense を簡単のタスクがなりますが、ASP.NET 2.0 が構成ファイルを編集するための Web インターフェイスを提供することによってさらに、1 つのステップになります。

Web インターフェイスを起動するには、次のように、ソリューション エクスプ ローラーのツールバーで [ASP.NET 構成] ボタンをクリックします。 ログイン コントロールが挿入されたときに表示されるポップアップを使用して、Web インターフェイスを起動することもできます。


![](membership/_static/image2.jpg)

**図 2**


これは、次に示す ASP.NET Web サイト管理ツールを起動します。 ASP.NET Web サイトの管理は、アプリケーション設定を管理しやすく 4 タブ インターフェイスです。 次のタブを使用できます。

- **ホーム**
- **セキュリティ**ユーザー、ロール、およびアクセスを構成します。
- **アプリケーション**アプリケーション設定を構成します。
- **プロバイダー**構成と、アプリケーションのメンバーシップ プロバイダーをテストします。

Web サイト管理ツールに簡単に新しいユーザーを作成し、新しいロールは、およびユーザーとロールを管理できます。 Windows のインターフェイスでは、この機能を使用できません。 Windows のインターフェイスでは承認の設定を簡単に定義および追加、削除、およびプロバイダー、Web サイト管理ツールに含まれていない機能を管理することができます。

Windows のインターフェイスを起動するには、インターネット インフォメーション サービス スナップインを開くをアプリケーションを右クリックし、プロパティを選択します。 ASP.NET タブをクリックし、構成の編集 ボタンをクリックします。 (アプリケーションを有効にする構成の編集 ボタンの ASP.NET 2.0 で実行する必要があります。 構成できます ASP.NET のバージョン ASP.NET ダイアログ。)次に示すように、ASP.NET 構成の設定 ダイアログが表示されます。


![](membership/_static/image3.jpg)

**図 3**


[全般] タブで、接続文字列とアプリケーションの設定が一覧表示されます。 斜体ですべての設定が親の構成ファイル (machine.config または上位レベルにある web.config) で定義されている、設定斜体ではなく、アプリケーション構成ファイルから。 場合は、設定を追加すると、削除、またはアプリケーション レベルでは、編集 ASP.NET は追加、削除、または継承元となる構成ファイルから設定を削除する代わりに、アプリケーション レベルの web.config で設定を変更します。

[認証] タブは、以下に示します。 これは、メンバーシップの設定を構成します。 メンバーシップ プロバイダーの認証設定を作成し、ロール プロバイダーはここで構成することができます。


![](membership/_static/image4.jpg)

**図 4**


## <a name="implementing-membership-in-your-application"></a>アプリケーションでのメンバーシップの実装

アプリケーションに ASP.NET 2.0 のメンバーシップを実装する最も簡単な方法では、指定されたログオン制御を使用します。 このメソッドでは、すべてのコードを記述せずに ASP.NET 2.0 のメンバーシップの基本を実装することができます。

次のログオン コントロールは、ASP.NET 2.0 で使用できます。

## <a name="login-control"></a>ログイン コントロール

ログイン コントロールでは、他のユーザーが、メンバーシップ システムにログインするためのインターフェイスを提供します。 ユーザー名とパスワード textboxt とログイン ボタンに提供します。 その他の一般的ななど多くの機能がまだ行われないため、チェック ボックスを利用できるユーザーにアクセスする際に自動的にログインするユーザーの登録へのリンク、パスワード関連語句などへのリンク。ログイン コントロールのすべての機能は、コントロールのプロパティを使用してカスタマイズできます。

ASP.NET で 1.x では、開発者は、相当量のフォーム認証を使用しているときに、参照を実行するコードを記述する必要があります。 ASP.NET 2.0 のメンバーシップを持つすべてのコードをまったく記述しなくてもユーザーを検証できます。 ASP.NET は自動的に処理、ユーザーの検索をします。 (使用することができますを ASP.NET メンバーシップを使用せず、ログイン コントロールを使用している場合、**述べた**ユーザーを検証するメソッドです)。

## <a name="loginview-control"></a>LoginView コントロール

LoginView コントロールが既定では 2 つのテンプレートを提供するテンプレート コントロールです。AnonymousTemplate し、LoggedInTemplate します。 表示されているテンプレートは、ユーザーが、メンバーシップ システムにログインしているかどうかによって決まります。 通常、このコントロールは、ユーザーがログインした場合、ユーザーがまだログインしていない場合は、ログイン コントロールおよびログイン状態コントロールやその他のログイン コントロールを表示する使用されます。 ロール管理、ASP.NET アプリケーションを使用している場合、LoginView コントロールは、ユーザー ロールに基づいて、特定のテンプレートを表示できます。 (複数の ASP.NET ロール管理については、後ほど。)

## <a name="passwordrecovery-control"></a>PasswordRecovery コントロール

PasswordRecovery コントロールでは、現在のパスワードを自分で電子メールを受信または自分のパスワードをリセットすることができます。 クリア テキストや暗号化パスワードを回復およびユーザーに電子メールで送信します。 パスワードがハッシュされている場合は回復できません。 代わりに、ユーザーはパスワードのリセットを実行する必要があります。

## <a name="loginstatus-control"></a>ログイン状態コントロール

ログイン状態コントロールを使用して、現在にログオンしているユーザーをログインしていないユーザーにログイン インジケーターとログアウト インジケーターを表示します。 Request.IsAuthenticated プロパティを使用すると、表示するインジケーターを決定します。 ログイン状態コントロールによって表示されるインジケーターは、テキストを指定できます (を介して実装、 **LoginText**と**LogoutText**プロパティ) またはイメージ (を介して実装、 **LoginImageUrl**と**LogoutImageUrl**プロパティです)。

ユーザーがログイン状態コントロールを使用してログアウトすると、そのユーザーにリダイレクトされるで指定された URL、 **LogoutPageUrl**プロパティです。 そのプロパティが設定されていない場合は、現在のページが更新されます。 サイトがフォーム認証で保護されている可能性があります、ために、現在のページの更新は、ユーザーをサイトのログイン ページにリダイレクトされます。

## <a name="loginname-control"></a>LoginName コントロール

LoginName コントロールには、現在のサイトにログオンしたユーザーのユーザー名が表示されます。

## <a name="createuserwizard-control"></a>CreateUserWizard コントロール

CreateUserWizard コントロールでは、メンバーシップ システムに登録する便利な手段を持つユーザーを提供します。 次に示すインターフェイスを使用して (の WizardSteps コレクションとして実装) の手順を追加できます。


![](membership/_static/image5.jpg)

**図 5**


ウィザードのクラスから派生し、次のテンプレートを提供するテンプレートのコントロールを CreateUserWizard には。

- **HeaderTemplate**このテンプレートは、ウィザードのヘッダーの外観を制御します。
- **SidebarTemplate**このテンプレートは、ウィザードのサイド バーの外観を制御します。
- **StartNavigationTemplate**スタートの手順で、ウィザードのナビゲーションの外観は、このテンプレートを制御します。
- **StepNavigationTemplate**このテンプレートは、開始日または終了の手順ではないときのナビゲーション領域の外観を制御します。
- **FinishNavigationTemplate**このテンプレートは、[完了] 手順でのナビゲーション領域の外観を制御します。

さらに、ウィザードに追加する各ステップでは、ASP.NET は、ContentTemplate とその手順の CustomNavigationTemplate の両方を含むカスタム テンプレートが作成されます。 CreateUserWizard をカスタマイズする方法の詳細については、VS.NET 2005 のマニュアルを参照してください。

## <a name="changepassword-control"></a>ChangePassword コントロール

ChangePassword コントロールでは、自分のパスワードを変更することができます。 DisplayUserName プロパティが true (既定値は) の場合、ユーザー ログインしていないときに自分のパスワードを変更できます。 場合、ユーザー*は*に既にログインして、DisplayUserName プロパティが true、ユーザーは、そのユーザーのユーザー ID を知っているを提供することに記録されていない別のユーザーのパスワードを変更できるとします。

ユーザーにログインしなくてもパスワードを変更できる場合は、必要があります ChangePassword コントロールを表示するページへの匿名アクセスを許可することに注意してください。 当然ながら、ユーザーは自分のパスワードを変更するために、古いパスワードを提供する必要があります。

## <a name="role-management"></a>ロール管理

ロール管理を使用すると、特定のロールにユーザーを割り当てるし、特定のファイルまたはそのロールに基づいたフォルダーへのアクセスを制限できます。 ロール管理では、プログラムによって someones 役割を確認するか、特定のロールのすべてのユーザーを確認して適切に対応するように、API も提供します。

ロール管理ではなく、ASP.NET メンバーシップの要件もはメンバーシップ、ロール管理を使用するために必要です。 ただし、2 つが適切に相互補完、開発者が互いに連携してそれらに使用可能性があります。

アプリケーションでロールの管理を有効にするには、web.config ファイルに次の変更を行い。

[!code-xml[Main](membership/samples/sample2.xml)]

ときに、 **cacheRolesInCookie**属性に設定されている場合は true、ASP.NET によりキャッシュ クライアントの cookie にユーザー ロールのメンバーシップ。 これにより、RoleProvider への呼び出しを使用しない場合に参照をロールします。 開発者がいることを確認することをお勧めこの属性を使用する場合、 **cookieProtection**属性がすべてに設定します。 (これは、既定の設定です)。これにより、cookie のデータが暗号化され、cookie の内容が変更されていないことを確認するのに役立ちます。 ロールを追加するには、Web サイト管理ツールを使用します。 簡単にロールを定義し、これらの役割に基づくサイトの各部分へのアクセスを構成し、ユーザー ロールを割り当てることができます。


![](membership/_static/image6.jpg)

**図 6**


上記のように、単に、ロールの名前を入力し、役割の追加をクリックして新しいロールを追加できます。 既存のロールを管理または既存のロールの一覧で、適切なリンクをクリックして削除できます。

ロールを管理するときに追加するか次に示すようにユーザーを削除します。


![](membership/_static/image7.jpg)

**図 7**


ユーザー ロールにチェック ボックスをオンするには、特定のロールにユーザーを簡単に追加できます。 ASP.NET は、適切なエントリで、メンバーシップ データベースを自動的に更新されます。 することも、アプリケーションのアクセス ルールを構成します。 ASP.NET 1.x 開発者に慣れて経由でこれを行う、&lt;承認&gt;web.config ファイル、およびそのオプションの要素は ASP.NET 2.0 では引き続き使用できます。 ただしの管理が容易にアクセス ルールを Web サイト管理ツールとして表示される以下を使用します。


![](membership/_static/image8.jpg)

**図 8**


ここでは、管理フォルダーが強調表示されます (そのがわかりにくくなるため、ツールは明るい灰色でハイライト) 管理者ロールがアクセスを許可されているとします。 その他のすべてのユーザーが拒否されました。 ルールを選択し、上へ移動し、下へ移動 ボタンを使用してルールを並べ替えますヘッドのアイコンをクリックすることができます。 ASP.NET と同様に&lt;承認&gt;要素、ルールが表示される順序で処理されます。 つまり、ショットを上記の規則の順序を反転させた場合誰する必要がありますアクセス管理フォルダー ASP.NET は発生する最初のルールがフォルダーにすべてのユーザーを拒否する規則になるためです。

ASP.NET 2.0 では、アクセス ルールを指定するフォルダーに web.config ファイルを追加します。 構成ファイルを使用して、または Web サイト管理ツールを使用して、アクセス規則を編集できます。 つまり、Web サイト管理ツールは、ユーザー フレンドリな環境で使用される構成ファイルを編集できますインターフェイスだけです。

## <a name="using-roles-in-code"></a>コードでのロールの使用

ロール管理用の API がバージョン以降に変更されていない 1.x です。 **IsInRole**メソッドは、特定のロールのユーザーの判断に使用します。

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET は、現在のコンテキストのメンバーとしても RolePrincipal インスタンスを作成します。 RolePrincipal オブジェクトは、すべてのユーザーが所属する次のようにロールを取得するために使用できます。

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>LoginView コントロールでの RoleGroups の使用

ロール管理およびメンバーシップについて理解がある場合は、これで、LoginView コントロールが ASP.NET 2.0 でこの機能を利用を受け取る方法について簡単に説明することができます。 既に説明したよう LoginView コントロールは既定では 2 つのテンプレートを含むテンプレート コントロールです。AnonymousTemplate し、LoggedInTemplate します。 内の LoginView タスク ダイアログ (下図参照) のリンクは、RoleGroups を編集することができます。


![](membership/_static/image9.jpg)

**図 9**


各 RoleGroup オブジェクトには、RoleGroup が適用されるロールを定義する文字列の配列が含まれています。 LoginView コントロールに新しい RoleGroup を追加するには、RoleGroups の編集リンクをクリックします。 上記の図では、管理者用の新しい RoleGroup を追加したことがわかります。 [その RoleGroup] を選択して (ビュー ドロップダウン リストから RoleGroup[0])、できます構成管理者ロールのメンバーにのみ表示されるテンプレートです。 次の図で、Sales ロールおよび配布ロールのメンバーに適用される新しい RoleGroup を追加しました。 2 番目の RoleGroup LoginView タスク ダイアログ ボックスで、ビューのドロップダウン リストに追加し、そのテンプレートに追加されたものは、販売または配布ですべてのユーザーが表示されますロール。


![](membership/_static/image10.jpg)

**図 10**


## <a name="overriding-the-existing-membership-provider"></a>既存のメンバーシップ プロバイダーをオーバーライドします。

いくつかの方法は、ASP.NET メンバーシップの機能を拡張することがあります。 まず、それを継承するメソッドをオーバーライドして SqlMembershipProvider クラスの既存の機能を明らかに変更できます。 たとえば、ユーザーが作成されるときに、独自の機能を実装する場合は、SqlMembershipProvider から次のように継承するクラスを作成することができます。

[!code-csharp[Main](membership/samples/sample5.cs)]

場合は、その一方で、(情報を格納する、メンバーシップ、Access データベースなど)、独自のプロバイダーを作成する、独自のプロバイダーを作成することができます。

## <a name="creating-your-own-membership-provider"></a>独自のメンバーシップ プロバイダーを作成します。

独自のメンバーシップ プロバイダーを作成するには、まず、MembershipProvider クラスから継承するクラスを作成する必要があります。 VB.NET を使用している場合、Visual Studio 2005 はすべてのメソッドをオーバーライドする必要のあるスタブを追加します。 使用する場合、C# の場合、スタブを追加します。

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

これに、リスト c# 開発者として、実装します。 実装は一切 VB.NET にクラスを作成し、コードを c# に変換する .NET Reflector または同様のツールを使用して簡単にすることもあります。

Initialize メソッドが既定値には、接続文字列およびその他のプロパティを設定してください。 (Initialize メソッドは発生しませんプロバイダーは実行時に読み込まれます。)Initialize メソッドの 2 番目のパラメーター型 System.Collections.Specialized.NameValueCollection への参照、&lt;追加&gt;web.config ファイルで、カスタム プロバイダーに関連付けられている要素です。 そのエントリは、次のようになります。

[!code-xml[Main](membership/samples/sample6.xml)]

Initialize メソッドの例を次に示します。

[!code-csharp[Main](membership/samples/sample7.cs)]

ログイン フォームを送信するときにユーザーを検証するためには、ValidateUser メソッドを使用する必要があります。 このメソッドは、ユーザーがログイン コントロールに [ログイン] ボタンをクリックしたときに発生します。 このメソッド内のユーザーの参照を実行するコードを配置します。

ご覧のように、独自のメンバーシップ プロバイダーを記述は難しくなく、ASP.NET 2.0 のこの強力な機能を拡張することができます。
