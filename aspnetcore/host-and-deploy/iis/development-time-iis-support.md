---
title: Visual Studio for ASP.NET Core の開発時 IIS サポート
author: shirhatti
description: Windows Server 上の IIS の背後にある実行時に ASP.NET Core アプリケーションのデバッグのサポートを検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core の開発時 IIS サポート

によって[Sourabh Shirhatti](https://twitter.com/sshirhatti)と[Luke Latham](https://github.com/guardrex)

この記事の内容について説明します[Visual Studio](https://www.visualstudio.com/vs/) Windows サーバーで IIS 内側で実行される ASP.NET Core アプリケーションのデバッグをサポートします。 このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>IIS を有効にする

1. **[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。
1. 選択、**インターネット インフォメーション サービス**チェック ボックスをオンします。

![黒の四角形 (チェック マークされません) の一部の IIS 機能が有効になっていることを示すチェック Windows の機能を示すインターネット インフォメーション サービス チェック ボックス](development-time-iis-support/_static/enable_iis.png)

IIS のインストールには、システムの再起動を必要があります。

## <a name="configure-iis"></a>IIS の構成

IIS は、次のように構成されている web サイトである必要があります。

* アプリの起動プロファイルの URL ホスト名と一致するホスト名です。
* 割り当てられている証明書でポート 443 をバインドします。

たとえば、**ホスト名**"localhost"(起動プロファイルも使用します"localhost"このトピックで後述) に追加の web サイトを設定します。 ポートは、「443」(HTTPS) に設定されます。 **IIS Express 開発証明書**works、web サイトが有効な証明書に割り当てられます。

![割り当てられている証明書でポート 443 で localhost バインディング セットを表示する IIS の web サイトのウィンドウを追加します。](development-time-iis-support/_static/add-website-window.png)

既に IIS のインストールがある場合、 **Default Web Site**アプリの起動プロファイルの URL ホスト名と一致するホスト名を持つ。

* ポート 443 (HTTPS) ポートのバインドを追加します。
* Web サイトに有効な証明書を割り当てます。

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Visual Studio での開発時 IIS サポートを有効にします。

1. Visual Studio インストーラーを起動します。
1. 選択、 **IIS サポート開発時間**コンポーネントです。 コンポーネントが一覧表示でオプションとして、**概要**パネル、 **ASP.NET および web 開発**ワークロード。 コンポーネントのインストール、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)、ネイティブの IIS モジュールがリバース プロキシの構成で ASP.NET Core IIS の背後にあるアプリを実行する必要があります。

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。 [Web & Cloud]\(Web とクラウド\) のセクションでは、[ASP.NET と Web 開発] パネルが選択されています。 概要パネルの省略可能な領域の右側に、チェック ボックスが開発時間が IIS ではサポートします。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>プロジェクトを構成する

### <a name="https-redirection"></a>HTTPS のリダイレクト

新しいプロジェクトの場合、チェック ボックスをオン**HTTPS の構成**で、 **ASP.NET Core Web アプリケーションを新しい**ウィンドウ。

![新しい ASP.NET Core Web アプリケーション ウィンドウ HTTPS チェック ボックスをオンに設定します。](development-time-iis-support/_static/new-app.png)

既存のプロジェクトで内でリダイレクト ミドルウェアを HTTPS を使用して`Startup.Configure`を呼び出して、 [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)拡張メソッド。

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

### <a name="iis-launch-profile"></a>IIS は、プロファイルを起動します。

開発時 IIS サポートを追加する新しい起動プロファイルを作成します。

1. **プロファイル**、select、**新規**ボタンをクリックします。 ポップアップ ウィンドウで、プロファイル"IIS"という名前です。 選択**OK**プロファイルを作成します。
1. **起動**選択を設定する**IIS**一覧からです。
1. チェック ボックスをオン**起動ブラウザー**エンドポイント URL を提供しています。 HTTPS プロトコルを使用します。 この例では `https://localhost/WebApplication1` を使用します。
1. **環境変数** セクションで、select、**追加**ボタンをクリックします。 環境変数を指定のキーを持つ`ASPNETCORE_ENVIRONMENT`と値の`Development`します。
1. **Web サーバー設定**領域で、設定、**アプリの URL**です。 この例では `https://localhost/WebApplication1` を使用します。
1. プロファイルを保存します。

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。 [プロファイル] と [起動] の設定は [IIS] に設定されます。 アドレスの起動ブラウザーの機能が有効になっているhttps://localhost/WebApplication1です。 同じアドレスは、Web サーバーの設定 の アプリ URL フィールドにも表示されます。](development-time-iis-support/_static/project_properties.png)

また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。

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

## <a name="run-the-project"></a>プロジェクトを実行します。

VS UI で、実行ボタンに設定、 **IIS**をプロファイルし、アプリを起動するボタンを選択します。

!["IIS"プロファイルへの設定、VS ツールバーのボタンを実行します。](development-time-iis-support/_static/toolbar.png)

Visual Studio は、管理者として実行されていない場合、再起動を要求があります。 その場合は、Visual Studio を再起動します。

信頼されていない開発の証明書を使用すると、ブラウザーには、信頼されていない証明書の例外を作成する必要があります。

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index)
* [ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)
* [HTTPS の適用](xref:security/enforcing-ssl)
