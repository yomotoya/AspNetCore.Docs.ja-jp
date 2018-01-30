---
title: "ASP.NET Core での Id の概要"
author: rick-anderson
description: "ASP.NET Core アプリケーションの Id を使用します。 含まれている (RequireDigit、RequiredLength、RequiredUniqueChars など) にパスワードの要件を設定します。"
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: 52b78922da54a2d4e66120552c2ffc9f585f406b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core での Id の概要

によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、Jon Galloway [Erik Reitan](https://github.com/Erikre)、および[Steve Smith](https://ardalis.com/)

ASP.NET Core Id は、アプリケーションにログイン機能を追加することができるメンバーシップ システムがします。 ユーザーは、ユーザー名でアカウントやログインを作成できます、パスワード、またはこれらには、Facebook、Google、Microsoft アカウント、Twitter またはなど、外部ログイン プロバイダーを使用できます。

ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用する ASP.NET Core Id を構成することができます。 代わりに、独自の永続的なストア、たとえば、Azure テーブル ストレージを使用することができます。 このドキュメントでは、Visual Studio と CLI を使用するための手順を説明します。

[表示または、サンプル コードをダウンロードします。](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(ダウンロードする方法)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Id の概要

このトピックでを ASP.NET Core Id を使用して、ログインを登録する機能を追加する方法について説明し、ユーザーをログアウトします。 ASP.NET Core の Id を使用してアプリの作成に関する詳細な手順については、この記事の最後に次の手順を参照してください。

1.  個々 のユーザー アカウントを使って ASP.NET Core Web アプリケーション プロジェクトを作成します。

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Visual Studio で、次のように選択します。**ファイル** > **新規** > **プロジェクト**です。 選択**ASP.NET Core Web アプリケーション** をクリック**OK**です。

    ![[新しいプロジェクト] ダイアログ](identity/_static/01-new-project.png)

    ASP.NET Core の選択**Web アプリケーション (モデル-ビュー-コント ローラー)** for ASP.NET 2.x、コアし、選択**認証の変更**です。

    ![[新しいプロジェクト] ダイアログ](identity/_static/02-new-project.png)

    オファー、ダイアログを表示認証オプション。 選択**個々 のユーザー アカウント** をクリック**OK**前のダイアログに戻ります。

    ![[新しいプロジェクト] ダイアログ](identity/_static/03-new-project-auth.png)

    選択すると**個々 のユーザー アカウント**モデル、ViewModels、ビュー、コント ローラー、およびプロジェクト テンプレートの一部として、認証に必要なその他のアセットを作成する Visual Studio に指示します。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    .NET Core CLI を使用する場合を使用して新しいプロジェクトを作成``dotnet new mvc --auth Individual``です。 このコマンドは、Visual Studio によって作成された同じ Identity テンプレート コードを新しいプロジェクトを作成します。

    作成されたプロジェクトが含まれています、 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` Id データおよび SQL Server を使用するスキーマが引き続き発生する、パッケージ[Entity Framework Core](https://docs.microsoft.com/ef/)です。

    ---

2.  Id サービスを構成しのミドルウェアを追加`Startup`です。

    Id サービスが、アプリケーションに追加されます、`ConfigureServices`メソッドで、`Startup`クラス。

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。
    
    呼び出して、アプリケーションの識別情報が有効になっている`UseAuthentication`で、`Configure`メソッドです。 `UseAuthentication`認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    これらのサービスを使用して、アプリケーションで利用できる[依存性の注入](xref:fundamentals/dependency-injection)です。
    
    呼び出して、アプリケーションの識別情報が有効になっている`UseIdentity`で、`Configure`メソッドです。 `UseIdentity`cookie ベースの認証を追加[ミドルウェア](xref:fundamentals/middleware)要求パイプラインにします。
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    アプリケーション起動プロセスの詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)です。

3.  ユーザーを作成します。

    アプリケーションを起動し、をクリックして、**登録**リンクします。

    初めてこの操作を実行している場合は、移行を実行する必要はあります。 アプリケーションでは、するように求められます**適用移行**です。 必要な場合は、ページを更新します。
    
    ![移行の Web ページを適用します。](identity/_static/apply-migrations.png)
    
    代わりに、メモリ内データベースを使用してアプリを永続的なデータベースを使用しない ASP.NET Core の Id を使用してテストできます。 メモリ内のデータベースを使用するのには、追加、``Microsoft.EntityFrameworkCore.InMemory``アプリをパッケージ化し、アプリの呼び出しを変更``AddDbContext``で``ConfigureServices``次のようにします。

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    ユーザーがクリックしたとき、**登録**、リンク、``Register``でアクションが呼び出される``AccountController``です。 ``Register``アクションが呼び出すことによって、ユーザーを作成`CreateAsync`上、`_userManager`オブジェクト (に提供される``AccountController``依存関係の挿入によって)。
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    ユーザーが正常に作成されている場合、ユーザーがログインへの呼び出しによって``_signInManager.SignInAsync``です。

    **注:**を参照してください[アカウント確認](xref:security/authentication/accconfirm#prevent-login-at-registration)手順については、登録時に即時にログインできません。
 
4.  ログイン。
 
    をクリックしてユーザーがサインインできるよう、**ログイン**サイトの上部にあるリンクの承認を必要とするサイトの一部にアクセスする場合、ログイン ページに移動可能性がありますか。 ユーザーがログイン ページのフォームを送信するときに、 ``AccountController`` ``Login``アクションが呼び出されます。

    ``Login``アクション呼び出し``PasswordSignInAsync``上、``_signInManager``オブジェクト (に提供される``AccountController``依存関係の挿入によって)。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    基本``Controller``クラスが公開する``User``コント ローラーのメソッドからアクセスできるプロパティです。 インスタンスを列挙できます`User.Claims`承認決定を行うとします。 詳細については、次を参照してください。[承認](xref:security/authorization/index)です。
 
5.  ログアウトします。
 
    クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    上記のコードの呼び出しの上、`_signInManager.SignOutAsync`メソッドです。 `SignOutAsync`メソッドは、cookie に格納されているユーザーの信頼性情報をクリアします。
 
<a name="pw"></a>
6.  構成します。

    Id は、アプリのスタートアップ クラスでオーバーライドできるいくつかの既定の動作を持っています。 `IdentityOptions`既定の動作を使用する場合に構成する必要はありません。 次のコードは、パスワードの強度のいくつかのオプションを設定します。

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Id を構成する方法の詳細については、次を参照してください。[構成 Identity](xref:security/authentication/identity-configuration)です。
    
    構成することもできます、主キーのデータ型を参照してください[Id の構成の主キーのデータ型](xref:security/authentication/identity-primary-key-configuration)です。
 
7.  データベースを表示します。

    アプリは、SQL Server データベース (既定の windows と Visual Studio ユーザー用) を使用している場合は、データベース作成されたアプリを表示できます。 使用することができます**SQL Server Management Studio**です。 または、Visual Studio から、次のように選択します。**ビュー** > **SQL Server オブジェクト エクスプ ローラー**です。 接続**(localdb) \MSSQLLocalDB**です。 一致する名前を持つデータベース**aspnet - <*、プロジェクトの名前*>-<*日付文字列*>** が表示されます。

    ![AspNetUsers データベース テーブルのコンテキスト メニュー](identity/_static/04-db.png)
    
    データベースを展開し、その**テーブル**を右クリックし、 **dbo します。AspNetUsers**テーブルを選択して**ビュー データ**です。

8. Identity 動作を確認します。

    既定値*ASP.NET Core Web アプリケーション*プロジェクト テンプレートを使用してしなくても、アプリケーションでのアクションにアクセスするユーザーにログインします。 ASP.NET Identity が動作することを確認するには追加、`[Authorize]`属性を`About`のアクション、`Home`コント ローラー。
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    使用してプロジェクトを実行**Ctrl** + **f5 キーを押して**に移動し、**に関する**ページ。 認証されたユーザーのみがアクセス、**に関する** ページが表示、ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトします。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    コマンド ウィンドウを開き、プロジェクトのルートに移動ディレクトリを含む、`.csproj`ファイル。 実行、`dotnet run`アプリを実行するコマンド。

    ```cs
    dotnet run 
    ```

    出力で指定された URL を参照、`dotnet run`コマンド。 URL を指す必要があります`localhost`生成されたポート番号。 移動し、**に関する**ページ。 認証されたユーザーのみがアクセス、**に関する** ページが表示、ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトします。

    ---

## <a name="identity-components"></a>Identity コンポーネント

Id システムのプライマリ参照アセンブリが`Microsoft.AspNetCore.Identity`です。 ASP.NET Core Id のインターフェイスのコア セットを含む、このパッケージをによって含まれる`Microsoft.AspNetCore.Identity.EntityFrameworkCore`です。

ASP.NET Core アプリケーションで Id システムを使用するには、これらの依存関係が必要です。

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-エンティティ フレームワークのコアで Id を使用する、必要な型が含まれています。

* `Microsoft.EntityFrameworkCore.SqlServer`Entity Framework Core は、SQL Server などのリレーショナル データベースの Microsoft の推奨されるデータ アクセス テクノロジです。 使用することができます、テストは`Microsoft.EntityFrameworkCore.InMemory`します。

* `Microsoft.AspNetCore.Authentication.Cookies`-を cookie ベースの認証を使用するアプリを有効にするミドルウェア。

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core の Id への移行

ストアを参照の追加情報と、既存の Id の移行に関するガイダンスの[移行認証と Id](xref:migration/identity)です。

## <a name="setting-password-strength"></a>パスワードの強度を設定

参照してください[構成](#pw)のパスワードの最小要件を設定するサンプルです。

## <a name="next-steps"></a>次の手順

* [認証と ID の移行](xref:migration/identity)
* [アカウントの確認とパスワードの回復](xref:security/authentication/accconfirm)
* [SMS での 2 要素認証](xref:security/authentication/2fa)
* [Facebook、Google、および他の外部プロバイダーを使用する認証の有効化](xref:security/authentication/social/index)
