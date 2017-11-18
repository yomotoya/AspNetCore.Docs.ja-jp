---
title: "Nginx 搭載の Linux で ASP.NET Core をホストする"
description: "Ubuntu 16.04 でリバース プロキシとして Nginx をセットアップし、Kestrel で実行している ASP.NET Core Web アプリケーションに HTTP トラフィックを転送する方法について説明します。"
keywords: "ASP.NET Core、Linux、nginx、Ubuntu、リバース プロキシ"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 01768263fe82dc75a7da0e113b1850c8d788bfd3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="56ba2-104">Nginx 搭載の Linux で ASP.NET Core をホストするための環境をセットアップし、その環境に展開する</span><span class="sxs-lookup"><span data-stu-id="56ba2-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="56ba2-105">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="56ba2-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="56ba2-106">このガイドでは、Ubuntu 16.04 サーバーで本稼働対応の ASP.NET Core 環境をセットアップする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="56ba2-107">**注:** Ubuntu 14.04 の場合、Kestrel プロセスを監視するためのソリューションとして supervisord が推奨されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="56ba2-108">systemd は Ubuntu 14.04 ではご利用いただけません。</span><span class="sxs-lookup"><span data-stu-id="56ba2-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="56ba2-109">この文書の前のバージョンをご覧ください</span><span class="sxs-lookup"><span data-stu-id="56ba2-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="56ba2-110">このガイドでは:</span><span class="sxs-lookup"><span data-stu-id="56ba2-110">This guide:</span></span>

* <span data-ttu-id="56ba2-111">リバース プロキシ サーバーの後ろに既存の ASP.NET Core アプリケーションを配置します</span><span class="sxs-lookup"><span data-stu-id="56ba2-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="56ba2-112">Kestrel Web サーバーに要求を転送するようにリバース プロキシ サーバーを設定します</span><span class="sxs-lookup"><span data-stu-id="56ba2-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="56ba2-113">Web アプリケーションを起動時にデーモンとして実行します</span><span class="sxs-lookup"><span data-stu-id="56ba2-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="56ba2-114">Web アプリケーションの再起動を支援するようにプロセス管理ツールを構成します</span><span class="sxs-lookup"><span data-stu-id="56ba2-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56ba2-115">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="56ba2-115">Prerequisites</span></span>

1. <span data-ttu-id="56ba2-116">Ubuntu 16.04 サーバーへのアクセスと sudo 特権が与えられた標準ユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="56ba2-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="56ba2-117">既存の ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="56ba2-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="56ba2-118">アプリをコピーする</span><span class="sxs-lookup"><span data-stu-id="56ba2-118">Copy over your app</span></span>

<span data-ttu-id="56ba2-119">開発環境から `dotnet publish` を実行し、サーバーで実行可能な自己完結型ディレクトリにアプリをパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="56ba2-120">ワークフローに統合されているツール (SCP や FTP など) を利用し、サーバーに ASP.NET Core アプリをコピーします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="56ba2-121">次のようにアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-121">Test the app, for example:</span></span>
 - <span data-ttu-id="56ba2-122">コマンド ラインから `dotnet yourapp.dll` を実行します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="56ba2-123">ブラウザーで、`http://<serveraddress>:<port>` に移動し、アプリが Linux で動作することを検証します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
<span data-ttu-id="56ba2-124">**注:** [Yeoman](xref:client-side/yeoman) を利用し、新しいプロジェクトのために新しい ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-124">**Note:** Use [Yeoman](xref:client-side/yeoman) to create a new ASP.NET Core app for a new project.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="56ba2-125">リバース プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="56ba2-125">Configure a reverse proxy server</span></span>

<span data-ttu-id="56ba2-126">リバース プロキシは、動的 Web アプリケーションにサービスを提供するための一般的なしくみです。</span><span class="sxs-lookup"><span data-stu-id="56ba2-126">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="56ba2-127">リバース プロキシは HTTP 要求を終了させ、ASP.NET Core アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-127">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="56ba2-128">リバース プロキシ サーバーを利用する理由</span><span class="sxs-lookup"><span data-stu-id="56ba2-128">Why use a reverse proxy server?</span></span>

<span data-ttu-id="56ba2-129">ASP.NET Core から動的コンテンツにサービスを提供することに関して、Kestrel は優れた Web ツールです。しかしながら、IIS、Apache、Nginx などのサーバーのように機能が豊富ではありません。</span><span class="sxs-lookup"><span data-stu-id="56ba2-129">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="56ba2-130">リバース プロキシ サーバーは、静的コンテンツ サービス、要求のキャッシュ、要求の圧縮、HTTP サーバーからの SSL 終了などの作業の負荷を軽減します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-130">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="56ba2-131">リバース プロキシ サーバーは専用コンピューター上に置かれることもあれば、HTTP サーバーと並んで展開されることもあります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-131">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="56ba2-132">このガイドの目的のために、単一インスタンスの Nginx が使用されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-132">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="56ba2-133">HTTP サーバーと並んで、同じサーバー上で実行されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-133">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="56ba2-134">要件によっては、別のセットアップを選択することになります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-134">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="56ba2-135">要求はリバース プロキシによって転送されるため、`Microsoft.AspNetCore.HttpOverrides` パッケージの `ForwardedHeaders` ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-135">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="56ba2-136">このミドルウェアは `X-Forwarded-Proto` ヘッダーを利用して `Request.Scheme` を更新します。リダイレクト URI とその他のセキュリティ ポリシーが正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-136">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="56ba2-137">リバース プロキシ サーバーを設定するとき、認証ミドルウェアで `UseForwardedHeaders` を先に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-137">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="56ba2-138">この順序により、認証ミドルウェアは影響を受けた値を利用し、正しいリダイレクト URI を生成できます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-138">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56ba2-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56ba2-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="56ba2-140">`UseAuthentication` や同様の認証スキーム ミドルウェアを呼び出す前に、`UseForwardedHeaders` メソッドを呼び出します (*Startup.cs* の `Configure` メソッド)。</span><span class="sxs-lookup"><span data-stu-id="56ba2-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56ba2-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56ba2-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="56ba2-142">`UseIdentity`、`UseFacebookAuthentication` や同様の認証スキーム ミドルウェアを呼び出す前に、`UseForwardedHeaders` メソッドを呼び出します (*Startup.cs* の `Configure` メソッド)。</span><span class="sxs-lookup"><span data-stu-id="56ba2-142">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a><span data-ttu-id="56ba2-143">Nginx をインストールする</span><span class="sxs-lookup"><span data-stu-id="56ba2-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="56ba2-144">オプションの Nginx モジュールをインストールする場合、ソースから Nginx をビルドしなければならないことがあります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-144">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="56ba2-145">`apt-get` を利用し、Nginx をインストールします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="56ba2-146">インストーラーにより System V init スクリプトが作成されます。このスクリプトがシステム起動時に Nginx をデーモンとして実行します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="56ba2-147">Nginx は初めてのインストールとなるので、次を実行して明示的に起動します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="56ba2-148">ブラウザーで Nginx の既定のランディング ページが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="56ba2-149">Nginx を構成する</span><span class="sxs-lookup"><span data-stu-id="56ba2-149">Configure Nginx</span></span>

<span data-ttu-id="56ba2-150">Nginx をリバース プロキシとして構成し、ASP.NET Core アプリケーションに要求を転送するには、`/etc/nginx/sites-available/default` を変更します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="56ba2-151">テキスト エディターで開き、中身を次のものに変更します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="56ba2-152">この Nginx 構成ファイルは、ポート `80` から入ってくるパブリック トラフィックをポート `5000` に転送します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="56ba2-153">Nginx 構成の変更が終わったら、`sudo nginx -t` を実行し、構成ファイルの構文を検証します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-153">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="56ba2-154">構成ファイル テストが合格の場合、`sudo nginx -s reload` を実行し、変更を反映するように Nginx に要求できます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-154">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="56ba2-155">アプリケーションの監視</span><span class="sxs-lookup"><span data-stu-id="56ba2-155">Monitoring our application</span></span>

<span data-ttu-id="56ba2-156">これで、`http://yourhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリケーション (`http://127.0.0.1:5000`) に転送するように Nginx が設定されました。</span><span class="sxs-lookup"><span data-stu-id="56ba2-156">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="56ba2-157">ただし、Nginx は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="56ba2-157">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="56ba2-158">*systemd* を利用し、基礎 Web アプリを起動し、監視するサービス ファイルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-158">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="56ba2-159">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="56ba2-160">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="56ba2-160">Create the service file</span></span>

<span data-ttu-id="56ba2-161">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="56ba2-162">次はアプリケーションのサービス ファイルの例です。</span><span class="sxs-lookup"><span data-stu-id="56ba2-162">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="56ba2-163">**注:** ユーザー *www-data* が構成で利用されない場合、ここで定義するユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-163">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="56ba2-164">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-164">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="56ba2-165">サービスを起動し、動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-165">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="56ba2-166">リバース プロキシを構成し、Kestrel を systemd で管理している状態で、Web アプリケーションは完全に構成されたことになり、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-166">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="56ba2-167">妨げとなるファイアウォールを禁止すれば、リモート コンピューターからもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-167">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="56ba2-168">応答ヘッダーを調べるとき、Kestrel がサービスを提供している ASP.NET Core アプリケーションが `Server` ヘッダーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="56ba2-169">ログの表示する</span><span class="sxs-lookup"><span data-stu-id="56ba2-169">Viewing logs</span></span>

<span data-ttu-id="56ba2-170">Kestrel を利用する Web アプリケーションは `systemd` で管理されるため、すべてのイベントとプロセスが記録され、中心的ジャーナルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-170">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="56ba2-171">ただし、このジャーナルには、`systemd` が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="56ba2-172">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="56ba2-173">さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="56ba2-174">アプリケーションのセキュリティ強化</span><span class="sxs-lookup"><span data-stu-id="56ba2-174">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="56ba2-175">AppArmor を有効にする</span><span class="sxs-lookup"><span data-stu-id="56ba2-175">Enable AppArmor</span></span>

<span data-ttu-id="56ba2-176">Linux Security Modules (LSM) は、Linux 2.6 以降の Linux カーネルに含まれるフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="56ba2-176">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="56ba2-177">LSM は、セキュリティ モジュールのさまざまな実装に対応しています。</span><span class="sxs-lookup"><span data-stu-id="56ba2-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="56ba2-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) は Mandatory Access Control システムを実装する LSM です。このシステムは、プログラムのリソース範囲を限定できます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="56ba2-179">AppArmor が有効であり、正しく構成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="56ba2-180">ファイアウォールの構成</span><span class="sxs-lookup"><span data-stu-id="56ba2-180">Configuring our firewall</span></span>

<span data-ttu-id="56ba2-181">使用されていないすべての外部ポートを閉じます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="56ba2-182">ufw (uncomplicated firewall/複雑ではないファイアウォール) は `iptables` のフロント エンドとなり、ファイアウォールを構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="56ba2-183">必要なあらゆるポートでトラフィックを許可するように `ufw` が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-183">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="56ba2-184">Nginx のセキュリティを強化する</span><span class="sxs-lookup"><span data-stu-id="56ba2-184">Securing Nginx</span></span>

<span data-ttu-id="56ba2-185">Nginx の既定のディストリビューションでは SSL が有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="56ba2-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="56ba2-186">追加のセキュリティ機能を有効にするには、ソースからビルドします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="56ba2-187">ソースをダウンロードし、ビルド依存関係をインストールする</span><span class="sxs-lookup"><span data-stu-id="56ba2-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="56ba2-188">Nginx 応答名を変更する</span><span class="sxs-lookup"><span data-stu-id="56ba2-188">Change the Nginx response name</span></span>

<span data-ttu-id="56ba2-189">*src/http/ngx_http_header_filter_module.c* を編集します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="56ba2-190">オプションを構成し、ビルドする</span><span class="sxs-lookup"><span data-stu-id="56ba2-190">Configure the options and build</span></span>

<span data-ttu-id="56ba2-191">正規表現には PCRE ライブラリが必要です。</span><span class="sxs-lookup"><span data-stu-id="56ba2-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="56ba2-192">正規表現は、ngx_http_rewrite_module の場所ディレクティブで使用されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="56ba2-193">http_ssl_module により HTTPS プロトコル サポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="56ba2-194">*ModSecurity* など、Web アプリケーション ファイアウォールを利用してアプリケーションの防護を検討してください。</span><span class="sxs-lookup"><span data-stu-id="56ba2-194">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="56ba2-195">SSL を構成する</span><span class="sxs-lookup"><span data-stu-id="56ba2-195">Configure SSL</span></span>

* <span data-ttu-id="56ba2-196">ポート `443` で HTTPS トラフィックを待ち受けるようにサーバーを構成します。信頼されている証明書機関 (CA) が発行した有効な証明書を指定します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-196">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="56ba2-197">次の */etc/nginx/nginx.conf* ファイルにある方法のいくつかを利用し、セキュリティを強化します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-197">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="56ba2-198">たとえば、強力な暗号を選択したり、HTTP 経由のすべてのトラフィックを HTTPS にリダイレクトしたりします。</span><span class="sxs-lookup"><span data-stu-id="56ba2-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="56ba2-199">`HTTP Strict-Transport-Security` (HSTS) ヘッダーを追加すると、クライアントが行う後続のすべての要求が HTTPS 経由のみになります。</span><span class="sxs-lookup"><span data-stu-id="56ba2-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="56ba2-200">Strict-Transport-Security ヘッダーは追加しないでください。あるいは、今後、SSL を無効にすることがあれば、適切な `max-age` を選択してください。</span><span class="sxs-lookup"><span data-stu-id="56ba2-200">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="56ba2-201">*/etc/nginx/proxy.conf* 構成ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="56ba2-202">*/etc/nginx/nginx.conf* 構成ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-202">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="56ba2-203">この例では、1 つの構成ファイルに `http` セクションと `server` セクションの両方が含まれています。</span><span class="sxs-lookup"><span data-stu-id="56ba2-203">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="56ba2-204">Nginx をクリックジャッキングから守る</span><span class="sxs-lookup"><span data-stu-id="56ba2-204">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="56ba2-205">クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。</span><span class="sxs-lookup"><span data-stu-id="56ba2-205">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="56ba2-206">クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-206">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="56ba2-207">X-FRAME-OPTIONS を利用し、サイトのセキュリティを強化します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-207">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="56ba2-208">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-208">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="56ba2-209">行 `add_header X-Frame-Options "SAMEORIGIN";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-209">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="56ba2-210">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="56ba2-210">MIME-type sniffing</span></span>

<span data-ttu-id="56ba2-211">このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、ほとんどのブラウザーで MIME スニッフィングが阻止されます。</span><span class="sxs-lookup"><span data-stu-id="56ba2-211">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="56ba2-212">`nosniff` オプションを指定すると、サーバーがコンテンツを "text/html" と読むとき、ブラウザーはそれを "text/html" として表示します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-212">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="56ba2-213">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-213">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="56ba2-214">行 `add_header X-Content-Type-Options "nosniff";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="56ba2-214">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
