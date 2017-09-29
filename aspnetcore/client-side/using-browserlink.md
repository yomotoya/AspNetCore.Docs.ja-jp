---
title: "ASP.NET Core のブラウザー リンク"
author: ncarandini
description: "Browser Link は、Visual Studio の機能に 1 つまたは複数の web ブラウザーで、開発環境をリンクする方法について説明します。"
keywords: "ASP.NET Core、browser link を CSS 同期"
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 67ddc58e38962bd876050739a2a1447be4f589bb
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET Core のブラウザー リンク 

によって[Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)、および[Tom Dykstra](https://github.com/tdykstra)

ブラウザー リンクは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio の機能です。 これはクロス ブラウザー テスト用に便利にいくつかのブラウザーで web のアプリケーションは、一度に更新 Browser Link を使用できます。

## <a name="browser-link-setup"></a>ブラウザー リンクのセットアップ

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x **Web アプリケーション**、**空**、および**Web API**テンプレート プロジェクトを使用して、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)パッケージ リファレンスを含むメタパッケージ[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)です。 したがってを使用して、`Microsoft.AspNetCore.All`メタパッケージ Browser Link を使用できるようにする追加の操作は必要ありません。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x **Web アプリケーション**プロジェクト テンプレートは、のパッケージの参照を持つ、 [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)パッケージです。 **空**または**Web API**テンプレート プロジェクトでは、パッケージの参照を追加する必要があります`Microsoft.VisualStudio.Web.BrowserLink`です。

これは、Visual Studio の機能では、最も簡単な方法にパッケージを追加するため、**空**または**Web API**プロジェクト テンプレートを開くには、 **Package Manager Console** (**ビュー** > **その他のウィンドウ** > **Package Manager Console**) し、次のコマンドを実行します。

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

また、使用することができます**NuGet Package Manager**です。 プロジェクト名を右クリックして**ソリューション エクスプ ローラー**選択**NuGet パッケージの管理**:

![開いている NuGet パッケージ マネージャー](using-browserlink/_static/open-nuget-package-manager.png)

検索して、パッケージをインストールします。

![パッケージで NuGet パッケージ マネージャーを追加します。](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>構成

`Configure`のメソッド、 *Startup.cs*ファイル。

```csharp
app.UseBrowserLink();
```

内のコードは、通常、`if`ブロックを次に示すように、開発環境で Browser Link をのみ有効にします。

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。

## <a name="how-to-use-browser-link"></a>Browser Link を使用する方法

Visual Studio の横にブラウザー リンク ツール バー コントロールの表示を開く ASP.NET Core プロジェクトがある場合は、ときに、**デバッグ ターゲット**ツール バー コントロール。

![ブラウザー リンク ドロップダウン メニュー](using-browserlink/_static/browserLink-dropdown-menu.png)

ブラウザー リンク ツール バー コントロールで、次の操作を実行できます。

* 複数のブラウザーで web アプリケーションを一度にを更新します。
* 開く、**ブラウザー リンク ダッシュ ボード**です。
* 有効または無効に**Browser Link**です。 注: ブラウザー リンクは、Visual Studio 2017 (15.3) で既定では無効です。
* 有効にするにまたは、CSS 自動同期を無効にします。

> [!NOTE]
> Visual Studio プラグインによって、最も顕著な*Web 拡張機能パック 2015*と*Web 拡張機能パック 2017*ブラウザー リンク拡張機能を提供、ASP でいくつかの追加の機能は機能しません。NET Core プロジェクト。

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>複数のブラウザーで web アプリケーションを一度に更新します。

プロジェクトを開始するときに起動する 1 つの web ブラウザーを選択するには、ドロップダウン メニューを使用して、**デバッグ ターゲット**ツール バー コントロール。

![F5 キーを押してドロップダウン メニュー](using-browserlink/_static/debug-target-dropdown-menu.png)

一度に複数のブラウザーを開き、選択**を参照しています.**同一のドロップダウン リストからです。 ブラウザーを選択して CTRL キーを押しながらクリックして**参照**:

![一度に多くのブラウザーを開く](using-browserlink/_static/open-many-browsers-at-once.png)

Visual Studio を開いて、インデックスが表示された表示スクリーン ショットと 2 つの開いているブラウザーを次に示します。

![例を次の 2 つのブラウザーとの同期します。](using-browserlink/_static/sync-with-two-browsers-example.png)

ブラウザー リンク ツール バー コントロールをプロジェクトに接続されているブラウザーを参照してくださいにポインターを合わせます。

![ホバー時のヒント](using-browserlink/_static/hoover-tip.png)

インデックス ビューを変更し、ブラウザー リンクは、更新ボタンをクリックすると、接続されているすべてのブラウザーは更新されます。

![ブラウザー変更の同期](using-browserlink/_static/browsers-sync-to-changes.png)

ブラウザー リンクは、Visual Studio の外部からを起動して、アプリケーションの URL に移動したブラウザーでも動作します。

### <a name="the-browser-link-dashboard"></a>ブラウザー リンク ダッシュ ボード

Browser Link のドロップダウン メニューを開いているブラウザーとの接続を管理からブラウザー リンク ダッシュ ボードを開きます。

![ダッシュ ボード browserslink 開く](using-browserlink/_static/open-browserlink-dashboard.png)

ブラウザーが接続されていない場合を選択して非デバッグ セッションを開始することができます、*ブラウザーで表示*リンク。

![browserlink ダッシュ ボードいいえ接続](using-browserlink/_static/browserlink-dashboard-no-connections.png)

それ以外の場合、接続されているブラウザーは、各ブラウザーで表示されているページへのパスで示されています。

![browserlink-ダッシュ ボードの 2 つの接続](using-browserlink/_static/browserlink-dashboard-two-connections.png)

必要に応じて、その 1 つのブラウザーを更新するリストされているブラウザーの名前をクリックすることができます。

### <a name="enable-or-disable-browser-link"></a>有効にするにまたは Browser Link を無効にします。

これを無効にした後 Browser Link を再度有効に再接続するようにブラウザーを更新する必要があります。

### <a name="enable-or-disable-css-auto-sync"></a>有効にするにまたは、CSS 自動同期を無効にします。

CSS 自動同期を有効にすると、CSS ファイルにすべての変更を加えるときに接続されているブラウザーが自動的に更新します。

## <a name="how-does-it-work"></a>Application Insights のしくみ

Browser Link では、SignalR を使用して、Visual Studio とブラウザー間の通信チャネルを作成します。 Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) に接続できる SignalR サーバーとして機能します。 Browser Link では、ASP.NET の要求パイプラインにミドルウェア コンポーネントも登録します。 このコンポーネントは特殊な挿入`<script>`ページ要求ごとに、サーバーからの参照。 選択すると、スクリプト参照を表示する**ソースの表示**の最後までスクロールし、ブラウザーで、`<body>`タグの内容。

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

ソース ファイルは変更されません。 ミドルウェア コンポーネントは、スクリプト参照を動的に挿入します。 

ブラウザー側のコードは、すべての JavaScript であるために、ブラウザーのプラグインを必要とせず、SignalR がサポートされているすべてのブラウザーで機能します。
