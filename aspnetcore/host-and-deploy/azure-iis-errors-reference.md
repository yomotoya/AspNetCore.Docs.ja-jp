---
title: Azure App Service と ASP.NET Core を使用した IIS の一般的なエラーのリファレンス
author: guardrex
description: Azure アプリのサービスと IIS の ASP.NET Core アプリケーションをホストしているときに、一般的なエラーを識別します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 995890a5e6b0cc1d9cebc21486917a7a39587076
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Azure App Service と ASP.NET Core を使用した IIS の一般的なエラーのリファレンス

作成者: [Luke Latham](https://github.com/guardrex)

次の一覧に含まれていないエラーもあります。 ここでは、表示されていないエラーが発生した場合[新しい懸案事項を開く](https://github.com/aspnet/Docs/issues/new)エラーを再現する詳細な指示を伴うです。

次の情報を収集します。

* ブラウザーの動作
* アプリケーション イベント ログ エントリ
* ASP.NET Core モジュール stdout ログ エントリ

次の一般的なエラー情報を比較します。 一致が見つかった場合は、トラブルシューティングのアドバイスに従います。

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>インストーラーが VC++ 再頒布可能パッケージを取得できない

* **インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー

* **インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)

  &#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。

トラブルシューティング:

* システムがインターネットにアクセスをホストしているバンドルをインストールするときに、この例外が発生、インストーラーが取得できない場合に、 *Microsoft Visual C 2015 Redistributable*です。 インストーラーを取得、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)です。 インストーラーが失敗した場合、サーバーがフレームワークに依存する展開 (FDD) をホストするために、.NET Core ランタイムを受信しません。 場合は、FDD をホストするには、プログラムで、ランタイムがインストールされていることを確認&amp;機能します。 必要な場合からランタイム インストーラーを入手[.NET のすべてのダウンロード](https://www.microsoft.com/net/download/all)です。 ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された

* **アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。 このデータはエラーです。

トラブルシューティング:

* **C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。 前に、ASP.NET Core モジュールがインストールされている場合、OS のアップグレードと任意の AppPool し、実行 32 ビット モードで OS のアップグレード後に、この問題が発生しました。 OS アップグレード後に ASP.NET Core モジュールを修復してください。 参照してください[.NET Core のホストのバンドルをインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)です。 選択**修復**インストーラーを実行するとします。

## <a name="platform-conflicts-with-rid"></a>プラットフォームが RID と競合している

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーション ログ:** アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\{パス}\' commandline でプロセスを開始できませんでした '"c:\\{パス} {アセンブリ} です {。exe | dll}"'、エラー コード = ' 0x80004005: ff します。

* **ASP.NET Core モジュールのログ:** 未処理の例外: System.BadImageFormatException: を読み込めませんでしたファイルまたはアセンブリ '{アセンブリ} .dll' です。 正しくない形式のプログラムを読み込もうとしました。

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。

* いることを確認、`<PlatformTarget>`で、 *.csproj* RID には抵触しません。 などを指定しない、`<PlatformTarget>`の`x86`と、RID のパブリッシュ`win10-x64`、いずれかを使用して*dotnet 発行-c リリース-r win10 x64*かを設定して、`<RuntimeIdentifiers>`で、 *.csproj*に`win10-x64`です。 プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。

* この例外は、アプリをアップグレードするときに Azure のアプリ展開の場合に発生し、以前の展開からすべてのファイルを手動で削除、新しいアセンブリを展開する場合。 アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。

## <a name="uri-endpoint-wrong-or-stopped-website"></a>URI のエンドポイントが間違っているか、Web サイトが停止している

* **ブラウザー:** ERR_CONNECTION_REFUSED

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* アプリ用の正しい URI エンドポイントが使用されていることを確認します。 バインドを確認してください。

* IIS web サイトがないことを確認、 *Stopped*状態です。

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>CoreWebEngine または W3SVC サーバー機能が無効

* **OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)

トラブルシューティング:

* 適切な役割と機能が有効になっていることを確認します。 「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。

## <a name="incorrect-website-physical-path-or-app-missing"></a>不適切な web サイトの物理パス、またはアプリがありません。

* **ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* IIS web サイトを確認**基本設定**と物理アプリ フォルダーです。 アプリが、IIS web サイトにあるフォルダーにあることを確認**物理パス**です。

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可

* **ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング:

* 適切なロールが有効になっていることを確認します。 「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。

* **[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。 インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。 参照してください[バンドルをホストしている .NET Core のインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)です。

* 確認して、**アプリケーション プール** > **プロセス モデル** > **Identity**に設定されている**ApplicationPoolIdentity**またはカスタム id は、アプリの配置フォルダーにアクセスする適切なアクセス許可を持っています。

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>正しくない processPath、不足しているパス変数をホストしているバンドルがインストールされていない、システムと IIS の再起動されません、vc++ 再頒布可能パッケージがインストールされていないまたは dotnet.exe アクセス違反

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーションのログ:** アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '".\{アセンブリ} .exe"'、エラー コード = ' 0x80070002: 0。

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。

* チェック、 *processPath*属性を`<aspNetCore>`内の要素*web.config*であることを確認する*dotnet*フレームワークに依存する展開 (FDD) または *.\{アセンブリ} .exe*自己完結型の展開 (SCD)。

* FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。 *C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。

* FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。 AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。 上の AppPool ユーザー id に対して構成された拒否ルールがないことを確認、 *C:\Program Files\dotnet*とアプリのディレクトリ。

* FDD が配置されているし、IIS を再起動しなくても .NET Core がインストールされています。 サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* ホスト システムに、.NET Core ランタイムをインストールしなくても、FDD が展開された可能性があります。 .NET Core ランタイムがインストールされていない場合は、実行、 **.NET Core をホストしているバンドル インストーラー**システムにします。 参照してください[バンドルをホストしている .NET Core のインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)です。 場合は、インターネット接続のないシステムで、.NET Core ランタイムをインストールしようとすると、取得、ランタイムから[.NET のすべてのダウンロード](https://www.microsoft.com/net/download/all)ASP.NET Core モジュールをインストールするバンドルをホストしているインストーラーを実行しています。 インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。

* FDD が配置されていると、 *Microsoft Visual C 2015 Redistributable (x64)* システムがインストールされていません。 インストーラーを取得、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)です。

## <a name="incorrect-arguments-of-aspnetcore-element"></a>\<aspNetCore\> 要素の引数の誤り

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーションのログ:** アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '"dotnet".\{アセンブリ} .dll '、エラー コード = ' 0x80004005: 80008081 です。

* **ASP.NET Core モジュールのログ:** を実行するアプリケーションは存在しません: ' パス\{アセンブリ} .dll '

トラブルシューティング:

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。

* 確認、*引数*属性を`<aspNetCore>`内の要素*web.config*したことを確認するかを (a) *.\{アセンブリ} .dll*のフレームワークに依存する展開 (FDD); か、(b) ではない、空の文字列 (*引数 =""*)、またはアプリの引数の一覧 (*引数"arg1 arg2,..."を =*)自己完結型の展開 (SCD)。

## <a name="missing-net-framework-version"></a>.NET Framework バージョンの欠落

* **ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)

* **アプリケーションのログ:** ErrorCode アプリケーション = 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '"dotnet".\{アセンブリ} .dll '、エラー コード = ' 0x80004005: 80008081 です。

* **ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。 例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。

トラブルシューティング:

* システムにない .NET Framework のバージョンをインストールします。

* フレームワークに依存する展開 (FDD) は、適切なランタイムがシステムにインストールされていることを確認します。 2.0 では、ホストのシステムに配置する 1.1 からプロジェクトをアップグレードすると、この例外により、2.0 framework がホスト システム上にあることを確認します。

## <a name="stopped-application-pool"></a>アプリケーション プールの停止

* **ブラウザー:** 503 サービスを利用できません

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング

* アプリケーション プールされていないことを確認、 *Stopped*状態です。

## <a name="iis-integration-middleware-not-implemented"></a>IIS 統合ミドルウェアが未実装

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーションのログ:** アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを作成 '"c:\\{PATH}\{アセンブリ} です {。exe | dll}"' がクラッシュまたは応答がありませんでしたまたは指定されたポート {ポート}、ErrorCode でリッスンできませんでした '0x800705b4' を =

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。

トラブルシューティング

* Kestrel でアプリをローカルに実行できることを確認します。 プロセスのエラーは、アプリ内の問題の結果である可能性があります。 詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。

* か、いることを確認します。
  * IIS 統合ミドルウェアが参照する呼び出し、`UseIISIntegration`メソッドをアプリの`WebHostBuilder`(ASP.NET Core 1.x)
  * アプリの使用、`CreateDefaultBuilder`メソッド (ASP.NET Core 2.x)。
  
  参照してください[ASP.NET Core でのホスト](xref:fundamentals/host/index)詳細についてはします。

## <a name="sub-application-includes-a-handlers-section"></a>サブアプリケーションに \<handlers\> セクションが含まれている

* **ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー

* **アプリケーション ログ:** エントリはありません

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成され、ルート アプリケーションの通常の操作を示します。 ログ ファイルが sub アプリ用に作成されません。

トラブルシューティング

* サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。

## <a name="stdout-log-path-incorrect"></a>標準出力ログのパスが正しくありません。

* **ブラウザー:** アプリが通常どおりに応答します。

* **アプリケーションのログ:** 警告: stdoutLogFile を作成できませんでした\\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log、ErrorCode = -。2147024893 です。

* **ASP.NET Core モジュールのログ:** ログ ファイルは作成されません

トラブルシューティング

* `stdoutLogFile`で指定されたパス、`<aspNetCore>`要素の*web.config*が存在しません。 詳細については、次を参照してください。、[ログの作成とリダイレクト](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)ASP.NET Core モジュール構成の参照トピックの「します。

## <a name="application-configuration-general-issue"></a>アプリケーション構成の一般的な問題

* **ブラウザー:** HTTP エラー 502.5 - 処理エラー

* **アプリケーションのログ:** アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを作成 '"c:\\{PATH}\{アセンブリ} です {。exe | dll}"' がクラッシュまたは応答がありませんでしたまたは指定されたポート {ポート}、ErrorCode でリッスンできませんでした '0x800705b4' を =

* **ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です

トラブルシューティング

* この一般的な例外は、開始するほとんどの場合、アプリの構成の問題のために、処理が失敗したことを示します。 参照する[ディレクトリ構造](xref:host-and-deploy/directory-structure)アプリの構成ファイルが存在して、アプリの配置ファイルとフォルダーが適切であることを確認し、アプリおよび環境の正しい設定が含まれています。 詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。
