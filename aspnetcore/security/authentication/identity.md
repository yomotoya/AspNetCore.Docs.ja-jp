---
title: ASP.NET core Identity の概要
author: rick-anderson
description: ASP.NET Core アプリでは、Id を使用します。 含まれている (RequireDigit、RequiredLength、RequiredUniqueChars など) にパスワードの要件を設定します。
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095640"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET core Identity の概要

によって[Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、Jon Galloway、 [Erik Reitan](https://github.com/Erikre)、および[Steve Smith](https://ardalis.com/)

ASP.NET Core Identity は、メンバーシップ システム、アプリケーションにログイン機能を追加することができます。 ユーザーはユーザー名を持つアカウントとログインを作成でき、パスワード、またはこれらには、Facebook、Google、Microsoft アカウント、Twitter またはなど、外部ログイン プロバイダーを使用できます。

ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用する ASP.NET Core Identity を構成することができます。 または、独自の永続的なストア、Azure Table Storage などを使用することができます。 このドキュメントでは、Visual Studio と CLI を使用するための手順を説明します。

[表示またはサンプル コードをダウンロードします。](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Id の概要

このトピックでは、ASP.NET Core Identity を使用して、ログインを登録する機能を追加する方法について説明し、ユーザーをログアウトします。 ASP.NET Core Identity を使用してアプリの作成について詳細な手順については、この記事の最後に次の手順を参照してください。

1. 個々 のユーザー アカウントを使って、ASP.NET Core Web アプリケーション プロジェクトを作成します。

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   Visual Studio で、次のように選択します。**ファイル** > **新規** > **プロジェクト**します。 選択**ASP.NET Core Web アプリケーション** をクリック**OK**します。

   ![[新しいプロジェクト] ダイアログ](identity/_static/01-new-project.png)

   ASP.NET Core を選択します。 **Web アプリケーション (モデル-ビュー-コント ローラー)** for ASP.NET Core 2.x、し、選択**認証の変更**します。

   ![[新しいプロジェクト] ダイアログ](identity/_static/02-new-project.png)

   ダイアログがサービス認証の選択肢です。 選択**個々 のユーザー アカウント** をクリック**OK**前のダイアログに戻ります。

   ![[新しいプロジェクト] ダイアログ](identity/_static/03-new-project-auth.png)

   選択**個々 のユーザー アカウント**モデル、ビューモデル、ビュー、コント ローラー、およびプロジェクト テンプレートの一部として、認証に必要なその他の資産を作成する Visual Studio に指示します。

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

   .NET Core CLI を使用する場合を使用して新しいプロジェクトを作成`dotnet new mvc --auth Individual`です。 このコマンドは、同じ Id テンプレート コードが Visual Studio によって作成、新しいプロジェクトを作成します。

   作成したプロジェクトが含まれています、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`パッケージで、Id データと SQL Server を使用するスキーマが解決しない[Entity Framework Core](https://docs.microsoft.com/ef/)します。

   ---

2. Id サービスを構成しでミドルウェアを追加`Startup`します。

   Identity サービスでアプリケーションに追加されます、`ConfigureServices`メソッドで、`Startup`クラス。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。

   呼び出すことによって、アプリケーションの id が有効に`UseAuthentication`で、`Configure`メソッド。 `UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。

   呼び出すことによって、アプリケーションの id が有効に`UseIdentity`で、`Configure`メソッド。 `UseIdentity` cookie ベースの認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   アプリケーション起動プロセスの詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)します。

3. ユーザーを作成します。

   アプリケーションを起動し、をクリックして、**登録**リンク。

   初めてこの操作を実行している場合は、移行を実行するために必要する必要があります。 アプリケーションでは、するように求められます**適用移行**します。 必要な場合は、ページを更新します。

   ![移行の Web ページを適用します。](identity/_static/apply-migrations.png)

   または、永続的なデータベースを使用せず、アプリでメモリ内データベースを使用して ASP.NET Core Identity を使用してテストできます。 メモリ内データベースを使用する追加、`Microsoft.EntityFrameworkCore.InMemory`をアプリにパッケージ化し、アプリの呼び出しを変更`AddDbContext`で`ConfigureServices`次のようにします。

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   ユーザーがクリックすると、**登録**、リンク、`Register`でアクションが呼び出された`AccountController`。 `Register`アクションを呼び出して、ユーザーを作成します`CreateAsync`上、`_userManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   ユーザーが正常に作成された場合、ユーザーがログインへの呼び出しによって`_signInManager.SignInAsync`します。

   **注:** を参照してください[アカウントの確認](xref:security/authentication/accconfirm#prevent-login-at-registration)登録時の即時のログインを防止する手順についてはします。

4. ログイン。

   ユーザーがクリックしてサインインできる、**ログイン**サイトの上部にあるリンクまたは承認を必要とするサイトの一部にアクセスする場合、ログイン ページにナビゲート可能性があります。 ユーザーがログイン ページで、フォームを送信するときに、 `AccountController` `Login`アクションが呼び出されます。

   `Login`アクション呼び出し`PasswordSignInAsync`上、`_signInManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   基本`Controller`クラスでは、`User`コント ローラー メソッドからアクセスできるプロパティです。 たとえば、列挙することができます`User.Claims`承認決定を行うとします。 詳細については、次を参照してください。[承認](xref:security/authorization/index)します。

5. ログアウトします。

   クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   上記のコードの呼び出しの上、`_signInManager.SignOutAsync`メソッド。 `SignOutAsync`メソッドは、cookie に格納されているユーザーの要求をクリアします。

<a name="pw"></a>
6. 構成します。

   Id は、アプリのスタートアップ クラスでオーバーライドすることがいくつかの既定の動作を持っています。 `IdentityOptions` 既定の動作を使用する場合に構成する必要はありません。 次のコードは、パスワードの強度のいくつかのオプションを設定します。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Id を構成する方法の詳細については、次を参照してください。[構成 Identity](xref:security/authentication/identity-configuration)します。

   構成することもできます、主キーのデータ型を参照してください[Id の構成の主キーのデータ型](xref:security/authentication/identity-primary-key-configuration)します。

7. データベースを表示します。

   アプリは、SQL Server データベース (既定では、Windows と Visual Studio のユーザー) を使用している場合は、データベースに作成したアプリを表示できます。 使用することができます**SQL Server Management Studio**します。 または、Visual studio で、次のように選択します。**ビュー** > **SQL Server オブジェクト エクスプ ローラー**します。 接続する **(localdb) \MSSQLLocalDB**します。 一致する名前を持つデータベース`aspnet-<name of your project>-<guid>`が表示されます。

   ![AspNetUsers データベース テーブルのコンテキスト メニュー](identity/_static/04-db.png)

   データベースを展開し、その**テーブル**、右クリックし、 **dbo します。AspNetUsers**テーブルを選択**ビュー データ**します。

8. Identity の動作を確認します

    既定の*ASP.NET Core Web アプリケーション*プロジェクト テンプレートでは必要なく、アプリケーションで任意のアクションへのアクセスにログインします。 ASP.NET Identity が動作することを確認するには追加、`[Authorize]`属性を`About`のアクション、`Home`コント ローラー。

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    使用してプロジェクトを実行**Ctrl** + **f5 キーを押して**に移動し、**について**ページ。 認証されたユーザーのみがアクセス可能性があります、**について**ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトするため、ここで、ページします。

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    コマンド ウィンドウを開き、プロジェクトのルートに移動しますディレクトリを含む、`.csproj`ファイル。 実行、 [「dotnet run」](/dotnet/core/tools/dotnet-run)アプリを実行するコマンド。

    ```csharp
    dotnet run 
    ```

    参照からの出力で指定された URL、 [「dotnet run」](/dotnet/core/tools/dotnet-run)コマンド。 URL を指す必要があります`localhost`生成されたポート番号。 移動し、**について**ページ。 認証されたユーザーのみがアクセス可能性があります、**について**ASP.NET ログインまたは登録するには、ログイン ページにリダイレクトするため、ここで、ページします。

    ---

## <a name="identity-components"></a>Id コンポーネント

Id システムの主な参照アセンブリが`Microsoft.AspNetCore.Identity`します。 このパッケージは、ASP.NET Core Identity のインターフェイスのコア セットが含まれていてに含まれている`Microsoft.AspNetCore.Identity.EntityFrameworkCore`します。

ASP.NET Core アプリケーションで、Id システムを使用するには、これらの依存関係が必要です。

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Entity Framework Core での Id を使用する必要な型が含まれています。

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core は、SQL Server のようなリレーショナル データベース用の Microsoft の推奨されるデータ アクセス テクノロジです。 テストは、使用することができます`Microsoft.EntityFrameworkCore.InMemory`します。

* `Microsoft.AspNetCore.Authentication.Cookies` -Cookie ベースの認証を使用するアプリができるミドルウェア。

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core Identity に移行します。

追加情報と、既存の Id の移行のガイダンスについては、「」を参照をストア[移行の認証と Id](xref:migration/identity)します。

## <a name="setting-password-strength"></a>パスワードの強度を設定

参照してください[構成](#pw)のパスワードの最小要件を設定するサンプルです。

## <a name="next-steps"></a>次の手順

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
