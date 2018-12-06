---
title: Visual Studio for ASP.NET Core の開発時 IIS サポート
author: shirhatti
description: ASP.NET Core アプリが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 65dbe690a33d82a4edddf315803dc4c656db27a0
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549102"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core の開発時 IIS サポート

著者: [Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Luke Latham](https://github.com/guardrex)

この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。 このトピックでは、手順に従ってこの機能を有効にし、プロジェクトを設定します。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio (Windows 版)](https://www.microsoft.com/net/download/windows)。
* **ASP.NET および Web の開発**ワークロード
* **.NET Core クロスプラットフォームの開発**ワークロード
* X.509 セキュリティ証明書

## <a name="enable-iis"></a>IIS を有効にする

1. **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。
1. **[インターネット インフォメーション サービス]** チェック ボックスをオンにします。

![[インターネット インフォメーション サービス] チェック ボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

IIS のインストールには、システムの再起動が必要になる場合があります。

## <a name="configure-iis"></a>IIS の構成

IIS には、次のように構成された Web サイトが含まれている必要があります。

* アプリの起動プロファイルの URL ホスト名と一致するホスト名。
* 割り当てられた証明書でのポート 443 のバインド。

たとえば、追加された Web サイトの **[ホスト名]** は "localhost" に設定されます (このトピックの後半では、起動プロファイルも "localhost" を使用します)。 ポートは "443" (HTTPS) に設定されます。 **IIS Express Development Certificate** が Web サイトに割り当てられていますが、すべての有効な証明書を使用できます。

![証明書が割り当てられた、ポート 443 での localhost に対するバインド セットを表示する、IIS の [Web サイトの追加] ウィンドウ](development-time-iis-support/_static/add-website-window.png)

ホスト名がアプリの起動プロファイルの URL ホスト名と一致する **[既定の Web サイト]** が、IIS のインストールに既に含まれている場合は、次の操作を行います。

* ポート 443 (HTTPS) に対するポートのバインドを追加する。
* Web サイトに有効な証明書を割り当てる。

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio の開発時 IIS サポートを有効にする

1. Visual Studio インストーラーを起動します。
1. **[開発時 IIS サポート]** コンポーネントを選択します。 **[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントがオプションとして表示されます。 このコンポーネントにより、リバース プロキシ構成の IIS の背後で ASP.NET Core アプリを実行するのに必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。 [Web & Cloud]\(Web とクラウド\) のセクションでは、[ASP.NET と Web 開発] パネルが選択されています。 右側の [概要] パネルの [省略可能] 領域には、[開発時 IIS サポート] のチェックボックスがあります。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>プロジェクトを構成する

### <a name="https-redirection"></a>HTTPS リダイレクト

新しいプロジェクトの場合、**[新しい ASP.NET Core Web アプリケーション]** ウィンドウで、**[Configure for HTTPS]\(HTTPS 用に構成する\)** のチェック ボックスをオンにします。

![HTTPS 用に構成するチェック ボックスが選択された新しい ASP.NET Core Web アプリケーション ウィンドウ](development-time-iis-support/_static/new-app.png)

既存のプロジェクトで、[UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) 拡張メソッドを呼び出すことで、`Startup.Configure` の HTTPS リダイレクト ミドルウェアを使用します。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>IIS 起動プロファイル

新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。

1. **[プロファイル]** で、**[新規]** ボタンを選択します。 ポップアップ ウィンドウで、プロファイルに "IIS" という名前を付けます。 **[OK]** をクリックして、プロファイルを作成します。
1. **[起動]** の設定で、一覧から **[IIS]** を選択します。
1. **[ブラウザーの起動]** チェック ボックスをオンにして、エンドポイント URL を指定します。 HTTPS プロトコルを使用します。 この例では `https://localhost/WebApplication1` を使用します。
1. **[環境変数]** セクションで、**[追加]** ボタンを選択します。 キーを `ASPNETCORE_ENVIRONMENT`、値を `Development` として環境変数を指定します。
1. **[Web サーバー設定]** で **[アプリ URL]** を設定します。 この例では `https://localhost/WebApplication1` を使用します。
1. プロファイルを保存します。

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。 [プロファイル] と [起動] の設定は [IIS] に設定されます。 [ブラウザーの起動] 機能は https://localhost/WebApplication1 のアドレスで有効になっています。 同じアドレスが、[Web サーバー設定] の [アプリ URL] フィールドにも入力されます。](development-time-iis-support/_static/project_properties.png)

または、起動プロファイルをアプリの [launchSettings.json](http://json.schemastore.org/launchsettings) ファイルに手動で追加します。

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>プロジェクトを実行する

Visual Studio:

* ビルド構成のドロップダウン リストが **[デバッグ]** に設定されていることを確認します。
* [実行] ボタンを **[IIS]** プロファイルに設定し、ボタンを選択してアプリを起動します。

![VS ツールバーの [実行] ボタンは [IIS] プロファイルに設定され、ビルド構成のドロップダウン リストは [リリース] に構成されます。](development-time-iis-support/_static/toolbar.png)

管理者として実行していない場合、Visual Studio によって再起動を求められる場合があります。 その場合は、Visual Studio を再起動します。

信頼されていない開発証明書を使用すると、信頼されていない証明書に対する例外を作成するように、ブラウザーによって求められる場合があります。

> [!NOTE]
> [マイ コードのみ](/visualstudio/debugger/just-my-code)とコンパイラの最適化を使用してリリースのビルド構成をデバッグすると、エクスペリエンスの品質が低下します。 たとえば、ブレーク ポイントがヒットしません。

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [HTTPS の適用](xref:security/enforcing-ssl)
