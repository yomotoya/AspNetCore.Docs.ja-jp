---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: "ASP.NET Identity 空であるか既存の web プロジェクトのフォームを追加する |Microsoft ドキュメント"
author: raquelsa
description: "このチュートリアルでは、ASP.NET アプリケーションを ASP.NET Identity (ASP.NET の新しいメンバーシップ システム) を追加する方法を示します。 作成する場合、新しい Web フォームまたは MVC しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: f5783287a26174ddf65bb0eae34c347831d02c47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>追加の ASP.NET Identity 空であるか既存の web プロジェクトをフォームします。
====================
によって[Raquel Soares De Almeida](https://github.com/raquelsa)

> このチュートリアルは、追加する方法を示します[ASP.NET Identity](introduction-to-aspnet-identity.md) (ASP.NET の新しいメンバーシップ システム) を ASP.NET アプリケーションにします。
> 
> Visual Studio 2013 の RTM で個々 のアカウントで新しい Web フォームまたは MVC プロジェクトを作成する場合、Visual Studio は必要なすべてのパッケージをインストールし、必要なすべてのクラスを追加します。 このチュートリアルでは、既存の Web フォーム プロジェクト、または新しい空のプロジェクトに ASP.NET Id のサポートを追加する手順を示します。 すれば説明は、すべての NuGet パッケージをインストールする必要があり、クラスを追加する必要があります。 新しいユーザーを登録して、ユーザーの管理と認証用のすべてのメイン エントリ ポイント Api を強調表示しながらログ記録のサンプル Web フォームか検討されます。 このサンプルでは、ASP.NET Identity の既定の実装を Entity Framework で構築された SQL データ記憶域の使用します。 このチュートリアルでは、SQL データベースの LocalDB を使用します。
> 
> このチュートリアルは Raquel Soares De Almeida と Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="getting-started-aspnet-identity"></a>ASP.NET Identity を開始します。

1. インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。
2. をクリックして**新しいプロジェクト**先頭からページ、または、メニューを使用して選択、**ファイル**、し**新しいプロジェクト**です。
3. 選択**Visual c# i** n し、左側のウィンドウ**Web**し、 **ASP.NET Web アプリケーション**です。 プロジェクト"WebFormsIdentity"の名前を指定し、をクリックして**OK**です。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. **新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 通知、**認証の変更**ボタンが無効になり、このテンプレートでの認証のサポートは提供されません。 Web フォーム、MVC、Web API テンプレートを使用すると、認証方法を選択できます。 詳細については、次を参照してください。[認証の概要](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)です。

## <a name="adding-identity-packages-to-your-app"></a>アプリへの Id のパッケージの追加

ソリューション エクスプ ローラーで、プロジェクトを右クリックし、 **NuGet パッケージの管理**です。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。"*Identity.E*"です。 このパッケージのインストール をクリックします。   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
このパッケージの依存関係パッケージがインストールされる注: EntityFramework と Microsoft ASP.NET Identity Core です。

## <a name="adding-web-forms-to-register-users"></a>ユーザーの登録を行う Web フォームを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加**、し**Web フォーム**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. **項目の名前を指定** ダイアログ ボックスに、名前の新しい web フォーム**登録**、クリックして**ok**
3. 生成されたマークアップを置き換える*Register.aspx*次のコードでのファイルです。 コードの変更が強調表示されます。   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > これは簡略化されたバージョンののみ、 *Register.aspx*新しい ASP.NET Web フォーム プロジェクトを作成するときに作成されるファイルです。 上記のマークアップでは、フォーム フィールドと、新しいユーザーを登録するためのボタンを追加します。
4. 開く、 *Register.aspx.cs*ファイルおよびファイルの内容を次のコードに置き換えます。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. 上記のコードが簡略化、 *Register.aspx.cs*新しい ASP.NET Web フォーム プロジェクトを作成するときに作成されるファイルです。
    > 2. *IdentityUser*クラスは、既定の EntityFramework 実装、 *IUser*インターフェイスです。 *IUser*インターフェイスは、ASP.NET Identity Core 上のユーザーの最小限のインターフェイスです。
    > 3. *UserStore*クラスは、ユーザー ストアの既定の EntityFramework 実装します。 このクラスは ASP.NET Identity Core の最小限のインターフェイスを実装します*IUserStore*、 *IUserLoginStore*、 *IUserClaimStore*と*IUserRoleStore。*.
    > 4. *UserManager*への変更が自動的に保存するクラスが公開ユーザー関連 Api、 *UserStore*です。
    > 5. *IdentityResult*クラス id 操作の結果を表します。
5. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加**、 **ASP.NET フォルダーの追加**し**アプリ\_データ**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. 開く、 *Web.config*ファイルし、を使用してユーザー情報を格納するデータベースの接続文字列のエントリを追加します。 データベースは、その EntityFramework で Identity エンティティの実行時に作成されます。 接続文字列は、いずれかの新しい Web フォーム プロジェクトを作成すると作成に似ています。 強調表示されたコードは、追加する必要があります、マークアップを示しています。

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Visual Studio 2015 以降では、または置換`(localdb)\v11.0`で`(localdb)\MSSQLLocalDB`接続文字列にします。
    
7. ファイルを右クリックして*Register.aspx*を選択してプロジェクト**スタート ページとして設定**です。 ビルドおよび web アプリケーションを実行するには、ctrl キーを押しながら F5 キーを押します。 新しいユーザー名とパスワードを入力し、をクリックして**登録**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET の Id が検証をサポートし、このサンプルでは、ユーザー名とパスワードの既定の動作を確認することができます、Identity Core パッケージからの検証コントロール。 ユーザーの既定の検証コントロール (`UserValidator`) プロパティが含まれる`AllowOnlyAlphanumericUserNames`設定された既定値を持つ`true`します。 既定のパスワードの検証コントロール (`MinimumLengthValidator`) によりそのパスワードは 6 文字以上です。 これらの検証コントロールのプロパティは、`UserManager`カスタムの検証する場合をオーバーライドすることができます

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>LocalDb のユーザー データベースと Entity Framework によって生成されたテーブルを確認します。

1. **ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. 展開**DefaultConnection (WebFormsIdentity)**、展開**テーブル**を右クリックして**AspNetUsers**  をクリック**テーブル データの表示**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>OWIN 認証用のアプリケーションを構成します。

この時点でユーザーを作成するためのサポートのみ追加されました。 ここで、認証ユーザーのログインを追加できる方法を示すためになっています。 ASP.NET Identity は、Microsoft OWIN 認証ミドルウェアをフォーム認証を使用します。 OWIN の Cookie 認証は要求ベースの認証メカニズムでホストされている任意のフレームワークで使用できる、cookie [OWIN](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)または IIS です。 このモデルでは、ASP.NET MVC、Web フォームを含む複数のフレームワークで同じ認証パッケージを使用できます。 プロジェクト Katana とホストの柔軟な参照でミドルウェアを実行する方法の詳細について[Katana プロジェクトの使用を開始する](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx)です。

## <a name="installing-authentication-packages-to-your-application"></a>アプリケーションに認証パッケージをインストールします。

1. ソリューション エクスプ ローラーで、プロジェクトを右クリックし、 **NuGet パッケージの管理**です。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。"*Identity.Owin*"です。 このパッケージのインストール をクリックします。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. パッケージを検索***Microsoft.Owin.Host.SystemWeb***し、インストールします。   

    > [!NOTE]
    > **Microsoft.Aspnet.Identity.Owin**パッケージには、一連管理および ASP.NET Identity Core パッケージでの使用を OWIN 認証ミドルウェアを構成する OWIN 拡張機能のクラスにはが含まれています。  
    > **Microsoft.Owin.Host.SystemWeb**を ASP.NET の要求パイプラインを使用して IIS 上で実行する OWIN ベースのアプリケーションを有効にする OWIN サーバーがパッケージに含まれています。 詳細については、次を参照してください。 [、IIS での OWIN ミドルウェア パイプラインを統合する](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md)です。

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>OWIN 起動し、認証の構成クラスを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックしをクリックして**追加**、し**新しい項目の追加**です。 検索テキスト ボックス ダイアログ ボックスで次のように入力します。"*owin*"です。 クラスに名前を"*スタートアップ*"をクリックして**追加**です。   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Startup.cs ファイルでは、OWIN cookie 認証を構成する以下の強調表示されたコードを追加します。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > このクラスに含まれる、 `OwinStartup` OWIN スタートアップ クラスを指定するための属性です。 すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定したスタートアップ クラスがあります。 参照してください[OWIN スタートアップ クラス検出](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)このモデルについての詳細。

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>登録して、ユーザーのログ記録用の Web フォームの追加

1. 開く、 *Register.cs*ファイルし、登録が成功したときに、ユーザーがログインする次のコードを追加します。 以下は、変更が強調表示されます。

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - フレームワークに生成するアプリの開発者が必要ですし、ASP.NET Identity OWIN の Cookie 認証は要求ベースのシステムであるため、 [ClaimsIdentity](https://msdn.microsoft.com/en-us/library/microsoft.identitymodel.claims.claimsidentity.aspx)ユーザー。 ClaimsIdentity には、ユーザーが属するロールなど、ユーザーのすべての要求に関する情報があります。 この段階で、ユーザーのクレームを追加することもできます。
    > - ユーザーをサインインするには、OWIN と呼び出し元からの AuthenticationManager を使用して、`SignIn`上記のように、ClaimsIdentity を渡しています。 このコードは、ユーザーにサインインしも cookie を生成します。 この呼び出しはに似て[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx)によって使用される、 [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
2. **ソリューション エクスプ ローラー**、プロジェクトをクリックしてを右クリックして**追加**、し**Web フォーム**です。 Web フォームの名前を付けます**ログイン**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. 内容を置き換える、 *Login.aspx*を次のコード ファイル。  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. 内容を置き換える、 *Login.aspx.cs*を次のファイル。  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - `Page_Load`の現在のユーザーの状態を確認し、に基づいてアクションを今すぐその`Context.User.Identity.IsAuthenticated`状態です。  
    >     **ユーザー名のログインを表示**: 拡張メソッドを追加で、Microsoft ASP.NET Identity Framework[どのオブジェクト タイプも](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.aspx)を取得することができます、`UserName`と`UserId`用、ユーザーに記録されます。 これらの拡張メソッドが定義されている、`Microsoft.AspNet.Identity.Core`アセンブリ。 これらの拡張メソッドは、置換の[HttpContext.User.Identity.Name](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.user.aspx)です。
    > - SignIn メソッド:   
    >     `This`メソッドは、以前に置き換えて`CreateUser_Click`メソッドでこのサンプルと、ユーザーが正常に作成した後、ユーザー サインインようになりました。   
    >  拡張メソッドを追加で、Microsoft OWIN Framework`System.Web.HttpContext`への参照を取得することができます、`IOwinContext`です。 これらの拡張メソッドが定義されている`Microsoft.Owin.Host.SystemWeb`アセンブリ。 `OwinContext`クラスが公開する`IAuthenticationManager`を現在の要求で使用可能な認証ミドルウェアの機能を表すプロパティ。  
    >  ユーザーをサインインするを使用して、 `AuthenticationManager` OWIN と呼び出し元から`SignIn`およびを渡して、`ClaimsIdentity`上記のようにします。   
    >  フレームワークを生成するアプリを必要とし、ASP.NET Identity OWIN の Cookie 認証は要求ベースのシステムであるため、`ClaimsIdentity`ユーザー。   
    >  `ClaimsIdentity`ユーザーが属するロールなどのユーザーのすべての要求に関する情報が含まれています。 この段階で、ユーザーのクレームを追加することもできます。  
    >  このコードは、ユーザーにサインインしも cookie を生成します。 この呼び出しはに似て[FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx)によって使用される、 [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
    > - `SignOut`方法:   
    >  参照を取得、 `AuthenticationManager` OWIN と呼び出しから`SignOut`です。 これは、メソッドは[FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx)で使用する方法、 [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)モジュール。
5. キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。 新しいユーザー名とパスワードを入力し、をクリックして**登録**です。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 注: この時点では、新しいユーザーが作成されに記録します。
6. をクリックして**ログアウト**ボタンをクリックします。フォームでログにリダイレクトされます。
7. 無効なユーザー名またはパスワードとクリックを入力**ログイン**ボタンをクリックします。   
 `UserManager.Find`メソッドは null が返され、エラー メッセージ:"*無効なユーザー名またはパスワード*"が表示されます。  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
