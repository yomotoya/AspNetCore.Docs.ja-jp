---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: ASP.NET Identity を空または既存の web フォーム プロジェクトの追加 |Microsoft Docs
author: raquelsa
description: このチュートリアルでは、ASP.NET アプリケーションを ASP.NET Identity (ASP.NET の新しいメンバーシップ システム) を追加する方法を示します。 ときに、新しい Web フォームまたは MVC を作成しています.
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236511"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>追加の ASP.NET Identity を空または既存の web フォーム プロジェクト


> このチュートリアルは、追加する方法を示します[ASP.NET Identity](introduction-to-aspnet-identity.md) (ASP.NET の新しいメンバーシップ システム) を ASP.NET アプリケーション。
> 
> Visual Studio 2017 の RTM が個々 のアカウントで新しい Web フォームまたは MVC プロジェクトを作成するときに、Visual Studio は、必要なすべてのパッケージをインストールし、必要なすべてのクラスを追加します。 このチュートリアルでは、ASP.NET Identity のサポートを既存の Web フォーム プロジェクトまたは新しい空のプロジェクトに追加する手順について説明します。 すべての NuGet のパッケージをインストールする必要があるとを追加する必要があるクラスについて説明されます。 新しいユーザーを登録すると、ログイン ユーザーの管理と認証のためのすべてのメイン エントリ ポイントの Api を強調表示し、サンプルの Web フォームは経由されます。 このサンプルは Entity Framework に組み込まれている SQL データ ストレージの既定の ASP.NET Identity の実装を使用します。 このチュートリアルでは、SQL database の LocalDB を使用します。
> 

## <a name="get-started-with-aspnet-identity"></a>ASP.NET Identity を概要します。

1. インストールと実行によって開始[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)します。
2. 選択**新しいプロジェクト**開始から ページで、またはメニューを使用して選択します**ファイル**、し**新しいプロジェクト**します。
3. 左側のウィンドウで展開**Visual C#** を選択し、 **Web**、し**ASP.NET Web アプリケーション (.Net Framework)** します。 プロジェクトに名前を"WebFormsIdentity"選び**OK**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   通知、**認証の変更**ボタンが無効になり、このテンプレートでの認証のサポートは提供されません。 Web フォーム、MVC、Web API テンプレートを使用すると、認証方法を選択することができます。

## <a name="add-identity-packages-to-your-app"></a>アプリへの Identity のパッケージを追加します。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**NuGet パッケージの管理**します。 検索し、インストール、 **Microsoft.AspNet.Identity.EntityFramework**パッケージ。 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
このパッケージが依存関係パッケージをインストールすることに注意してください。**EntityFramework**と**Microsoft ASP.NET Identity に Core**します。

## <a name="add-a-web-form-to-register-users"></a>ユーザーを登録する web フォームを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、し**Web フォーム**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. **項目の名前を指定**] ダイアログ ボックスで、名前の新しい web フォーム**登録**、し、[ **[ok]**
3. 生成されたマークアップを置き換えます*Register.aspx*次のコードでのファイル。 コードの変更が強調表示されています。 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > これは簡略化されたバージョンののみ、 *Register.aspx*新しい ASP.NET Web フォーム プロジェクトを作成するときに作成されるファイル。 上記のマークアップは、フォーム フィールドと新しいユーザーを登録するためのボタンを追加します。
4. 開く、 *Register.aspx.cs*ファイルを開き、ファイルの内容を次のコードに置き換えます。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. 上記のコードが簡略化されたバージョンの*Register.aspx.cs*新しい ASP.NET Web フォーム プロジェクトを作成するときに作成されるファイル。
    > 2. *IdentityUser*クラスは、の既定の EntityFramework 実装、 *IUser*インターフェイス。 *IUser*インターフェイスは Asp.net Identity のユーザーの最小限のインターフェイスです。
    > 3. *UserStore*クラスは、ユーザー ストアの既定の EntityFramework 実装します。 このクラスは、Asp.net Identity の最小限のインターフェイスを実装します。*IUserStore*、 *IUserLoginStore*、 *IUserClaimStore*と*IUserRoleStore*します。
    > 4. *UserManager*への変更が自動的に保存するクラスが公開ユーザー関連 Api、 *UserStore*します。
    > 5. *IdentityResult*クラス id 操作の結果を表します。
5. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、 **ASP.NET フォルダーの追加**し**アプリ\_データ**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. 開く、 *Web.config*ファイルし、ユーザー情報を使用して、データベースの接続文字列のエントリを追加します。 データベースは、その EntityFramework で Identity エンティティのランタイムで作成されます。 接続文字列は、1 つの新しい Web フォーム プロジェクトを作成するときに作成に似ています。 強調表示されたコードは、追加する必要があります、マークアップを示しています。

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 以降では、または置換`(localdb)\v11.0`で`(localdb)\MSSQLLocalDB`の接続文字列。
    
7. ファイルを右クリックして*Register.aspx*プロジェクトを選び、**スタート ページとして設定**します。 ビルドして、web アプリケーションを実行するには、ctrl キーを押しながら F5 キーを押します。 新しいユーザー名とパスワードを入力し、選択**登録**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET の Id が検証をサポートし、このサンプルでは、ユーザー名とパスワードの既定の動作を確認することができます、Identity コア パッケージに由来する検証コントロール。 ユーザーの既定の検証コントロール (`UserValidator`) プロパティを持つ`AllowOnlyAlphanumericUserNames`に設定する既定値を持つ`true`します。 既定のパスワードの検証コントロール (`MinimumLengthValidator`) により、そのパスワードが 6 文字以上にします。 これらの検証コントロールのプロパティを`UserManager`カスタムの検証を用意する場合をオーバーライドすることができます

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>LocalDb の Id データベースと Entity Framework によって生成されたテーブルを確認します。

1. **ビュー**メニューの **サーバー エクスプ ローラー**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. 展開**DefaultConnection (WebFormsIdentity)**、展開**テーブル**を右クリックして**AspNetUsers**選び**テーブル データの表示**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>OWIN 認証のアプリケーションを構成します。

この時点でユーザーを作成するためのサポートのみが追加されました。 ここで、ユーザーのログインの認証を追加する方法を示すためにここ。 ASP.NET Identity では、Microsoft の OWIN 認証ミドルウェアを使用してフォーム認証。 OWIN クッキー認証 cookie、クレーム ベースの認証メカニズムでホストされている任意のフレームワークで使用できる[OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx)または IIS です。 このモデルでは、同じ認証パッケージは、ASP.NET MVC と Web フォームを含む複数のフレームワーク全体で使用できます。 プロジェクト Katana とホストに依存しない参照でミドルウェアを実行する方法の詳細については[Katana プロジェクトの概要](https://msdn.microsoft.com/magazine/dn451439.aspx)します。

## <a name="install-authentication-packages-to-your-application"></a>アプリケーションに認証パッケージをインストールします。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**NuGet パッケージの管理**します。 検索し、インストール、 ***Microsoft.AspNet.Identity.Owin***パッケージ。 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. 検索し、インストール、 ***Microsoft.Owin.Host.SystemWeb***パッケージ。

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin**パッケージには、一連管理および ASP.NET Identity のコア パッケージで使用する OWIN 認証ミドルウェアを構成する OWIN 拡張機能のクラスにはが含まれています。
    > **Microsoft.Owin.Host.SystemWeb**パッケージには、OWIN ベース アプリケーションを ASP.NET の要求パイプラインを使用して、IIS で実行できるようにする OWIN サーバーが含まれています。 詳細については、次を参照してください。 [、IIS での OWIN ミドルウェア パイプラインを統合する](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md)します。

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>OWIN の起動と認証の構成クラスを追加します。

1. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加**、し**新しい項目の追加**します。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。"*owin*"。 クラスの名前"*スタートアップ*"を選択および**追加**します。 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Startup.cs ファイルでは、OWIN クッキー認証の構成を次に示す強調表示されたコードを追加します。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > このクラスが含まれています、 `OwinStartup` OWIN スタートアップ クラスを指定するための属性。 すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定するスタートアップ クラスがあります。 参照してください[OWIN スタートアップ クラス検出](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)このモデルの詳細について。

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Web フォームを登録して、ユーザーのサインインを追加します。

1. 開く、 *Register.aspx.cs*ファイルし、登録が成功したときに、次のコードをサインインするユーザーを追加します。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - フレームワークに生成するアプリの開発者が必要です ASP.NET Identity と OWIN クッキー認証は要求ベースのシステムであるため、 [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx)ユーザー。 ClaimsIdentity には、ユーザーが属するロールなど、ユーザーのすべての要求についての情報があります。 この段階でユーザーの複数のクレームを追加することもできます。
    > - ユーザーをサインインするには、OWIN と呼び出し元から AuthenticationManager を使用して`SignIn`ClaimsIdentity 上記のように渡すとします。 このコードは、ユーザーがサインインし、cookie も生成します。 この呼び出しに似ています[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)で使用される、 [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
2. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加**、し**Web フォーム**します。 Web フォーム名前**ログイン**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. 内容を置き換える、 *Login.aspx*を次のコード ファイル。

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. 内容を置き換える、 *Login.aspx.cs*を次のファイル。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load`現在のユーザーの状態をチェックし、に基づいてアクションを実行するようになりましたその`Context.User.Identity.IsAuthenticated`状態。
    >     **ユーザー名でログインして表示**:Microsoft ASP.NET Identity フレームワークでの拡張メソッドが追加[どのオブジェクト タイプも](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)を取得することができます、`UserName`と`UserId`ユーザーでログオンします。 これらの拡張メソッドが定義されている、`Microsoft.AspNet.Identity.Core`アセンブリ。 これらの拡張メソッドは、置換の[HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)します。
    > - SignIn メソッド:`This`メソッドは、置換前`CreateUser_Click`メソッドでは、このサンプルと、ユーザーが正常に作成した後、ユーザー サインインようになりました。   
    >   拡張メソッドを追加で Microsoft の OWIN Framework`System.Web.HttpContext`への参照を取得することができます、`IOwinContext`します。 これらの拡張メソッドが定義されている`Microsoft.Owin.Host.SystemWeb`アセンブリ。 `OwinContext`クラスでは、`IAuthenticationManager`現在の要求で使用可能な認証ミドルウェアの機能を表すプロパティ。 使用して、ユーザーがサインインすることができます、 `AuthenticationManager` OWIN と呼び出し元から`SignIn`を渡して、`ClaimsIdentity`上記のようです。 フレームワークが、アプリを生成する必要があります ASP.NET Identity と OWIN クッキー認証は要求ベースのシステムであるため、`ClaimsIdentity`ユーザー。 `ClaimsIdentity`ユーザーが属するロールなど、ユーザーのすべての要求に関する情報があります。 このコードは、ユーザーがサインインし、cookie も生成は、この段階でユーザーの複数のクレームを追加することもできます。 この呼び出しに似ています[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx)で使用される、 [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
    > - `SignOut` 方法:参照を取得、 `AuthenticationManager` OWIN と呼び出しから`SignOut`します。 これは、メソッドは[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)メソッドで使用される、 [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
5. キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。 新しいユーザー名とパスワードを入力し、選択**登録**します。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   メモ:この時点では、新しいユーザーが作成されログに記録します。
6. 選択、**ログアウト**ボタンをクリックします。 フォームのログにリダイレクトされます。
7. 無効なユーザー名またはパスワードを入力し、選択、**ログイン**ボタンをクリックします。 
   `UserManager.Find`メソッドは null と、エラー メッセージを返します。"*無効なユーザー名またはパスワード*"が表示されます。
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
