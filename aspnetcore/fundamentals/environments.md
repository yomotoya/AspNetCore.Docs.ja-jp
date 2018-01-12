---
title: "ASP.NET Core での複数の環境での作業"
author: ardalis
description: "ASP.NET Core が複数の環境間でのアプリの動作を制御するためのサポートを提供する方法について説明します。"
keywords: "ASP.NET Core、環境の設定、ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a>複数の環境での作業

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core は、開発、ステージング、運用環境など、複数の環境間でのアプリの動作を制御するためのサポートを提供します。 環境変数は、その環境用に構成するアプリを許可する、ランタイム環境を示すために使用されます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="development-staging-production"></a>開発、ステージング、実稼働環境

ASP.NET Core は、特定の環境変数を参照`ASPNETCORE_ENVIRONMENT`にで、アプリケーションが実行されている環境について説明します。 この変数を設定する任意の値が、通常使用される 3 つの値: `Development`、 `Staging`、および`Production`です。 これらのサンプルで使用される値と ASP.NET Core で提供されるテンプレートが表示されます。

現在の環境設定を検出できますプログラムから、アプリケーション内で。 環境を使用してさらに、[タグ ヘルパー](../mvc/views/tag-helpers/index.md)に含める特定のセクションで、[ビュー](../mvc/views/index.md)現在アプリケーション環境に基づきます。

注: Windows および macOS、環境名を指定は大文字小文字を区別します。 変数を設定するかどうか`Development`または`development`または`DEVELOPMENT`結果は同じになります。 ただしは、Linux、**大文字小文字を区別**既定では OS。 環境変数、ファイル名と設定は、大文字小文字の区別を必要とします。

### <a name="development"></a>開発

これは、アプリケーションを開発するときに使用する環境でなければなりません。 通常はありませんにするなど、実稼働環境でアプリの実行時に使用できる機能を有効に使用、[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)です。

Visual Studio を使用している場合は、プロジェクトのデバッグ · プロファイルで、環境を構成できます。 デバッグ プロファイルを指定して、[サーバー](xref:fundamentals/servers/index)を設定するアプリケーションやその環境変数を起動するときに使用します。 プロジェクトには、複数のデバッグ プロファイル環境変数を異なる方法で設定することができます。 使用してこれらのプロファイルを管理する、**デバッグ**web アプリケーション プロジェクトのタブ**プロパティ**メニュー。 プロジェクトのプロパティで設定する値が永続化、 *launchSettings.json*ファイル、およびすることができますもプロファイルを構成ファイルを直接編集することによってです。

IIS Express のプロファイルを次に示します。

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

ここでは、`launchSettings.json`用のプロファイルを含むファイル`Development`と`Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

プロジェクトのプロファイルに加えられた変更は反映されません使用される web サーバーが再起動されるまで (具体的には、Kestrel 再起動する必要がその環境に加えられた変更が検出する前に)。

>[!WARNING]
> 環境変数に格納*launchSettings.json*任意の方法でセキュリティ保護されていないと、1 つを使用する場合に、プロジェクトのソース コード リポジトリの一部になります。 **このファイルに資格情報またはその他の機密データを保存しないでください。** このようなデータを格納する場所を必要がある場合、*シークレット Manager*ツール」に記載[アプリ シークレットは、開発中の安全な保管](xref:security/app-secrets)です。

### <a name="staging"></a>ステージング

慣例により、`Staging`環境とは、実稼働前環境の実稼働環境に展開する前に最終的なテストに使用します。 理想的には、物理的な特性をミラー化する実稼働環境でのユーザーに影響を与えずに対処できますが、ステージング環境で最初に実稼働環境で発生する可能性のある問題が発生するようにします。

### <a name="production"></a>実稼働

`Production`環境はライブであるときに、アプリケーションを実行する環境エンドユーザーによって使用されているとします。 この環境は、セキュリティ、パフォーマンス、およびアプリケーションの堅牢性を最大化するように構成する必要があります。 一般的な運用環境が存在する可能性のある設定の開発とは異なりますは次のとおりです。

* キャッシュを有効にします。

* クライアント側のすべてのリソースがバンドルされている、縮小、および CDN から供給される可能性があることを確認します。

* 診断 ErrorPages をオフにします。

* わかりやすいエラー ページで有効にします。

* 運用ログおよび監視を有効にする (たとえば、 [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

これは、操作は、完全な一覧を示すものではではありません。 なら、アプリケーションの多くの部分で環境のチェックを回避することをお勧めします。 推奨される方法は、アプリケーション内でそのようなチェックを実行する代わりに、`Startup`クラス可能な限り

## <a name="setting-the-environment"></a>環境の設定

環境を設定するためのメソッドは、オペレーティング システムによって異なります。

### <a name="windows"></a>Windows
設定する、`ASPNETCORE_ENVIRONMENT`を使用して、アプリが開始された場合、現在のセッションの`dotnet run`、次のコマンドを使用

**コマンド ライン**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

これらのコマンドは、現在のウィンドウに対してのみ有効になります。 ウィンドウが閉じられたときに、ASPNETCORE_ENVIRONMENT 設定は、既定の設定またはコンピューターの値に戻ります。 Windows を開いたとき、値をグローバルに設定するために、**コントロール パネルの**  > **システム** > **システムの詳細設定**を追加または編集、 `ASPNETCORE_ENVIRONMENT`値。

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET コア環境変数](environments/_static/windows_aspnetcore_environment.png) 

**web.config**

参照してください、*環境変数の設定*のセクションで、 [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)トピックです。

**IIS アプリケーション プール単位**

分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。

### <a name="macos"></a>macOS
MacOS の現在の環境を設定する場合に実行できます行で、アプリケーションを実行しています。

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
使用してまたは`export`をアプリを実行する前に設定します。

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
コンピューターのレベルの環境変数が設定されて、*なる*または*.bash_profile*ファイル。 任意のテキスト エディターを使用して、ファイルを編集し、次のステートメントを追加します。

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
Linux ディストリビューションの場合を使用して、`export`セッション ベースの変数の設定のコマンドラインでコマンドと*bash_profile*マシン レベルの環境設定のファイルです。

## <a name="determining-the-environment-at-runtime"></a>実行時に環境を決定します。

`IHostingEnvironment`環境を操作するための中核となる抽象型を提供します。 このサービスは、提供、ASP.NET によってホスト レイヤーとを使用して、スタートアップ ロジックに挿入できます[依存性の注入](dependency-injection.md)です。 Visual Studio での ASP.NET Core web サイト テンプレートは、(存在する場合)、環境固有の構成ファイルをロードして、アプリのエラー処理設定をカスタマイズする、このアプローチを使用します。 どちらの場合も、この動作には、呼び出すことによって、現在指定されている環境を参照する`EnvironmentName`または`IsEnvironment`のインスタンスで`IHostingEnvironment`適切なメソッドに渡されます。

> [!NOTE]
> 使用して、特定の環境で、アプリケーションが実行されているかどうかを確認する必要がある場合`env.IsEnvironment("environmentname")`正しくの場合は無視されますので (確認する場合ではなく`env.EnvironmentName == "Development"`など)。

たとえば、環境固有のエラー処理をセットアップするのに、構成メソッドに次のコードを使用することができます。

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

アプリが実行されている場合、`Development`その環境では、Visual Studio、開発に固有のエラー ページ (通常は実行できません実稼働環境で) および特別なデータベース エラーに"BrowserLink"機能を使用するために必要なランタイム サポートを有効にページ (移行を適用する方法を提供し、開発でのみ使用するため)。 それ以外の場合、アプリは開発環境で実行されていない、ハンドルされない例外への応答に表示される標準的なエラー処理 ページが構成されます。

現在の環境に応じて、実行時にクライアントに送信するコンテンツを決定する必要があります。 たとえば、開発環境で一般に提供する最小化されていないスクリプトとスタイル シートは、デバッグが簡単には。 運用環境とテスト環境が縮小されたバージョンを提供する必要がありますと、CDN から一般にします。 これを行う環境を使用して[タグ ヘルパー](../mvc/views/tag-helpers/intro.md)です。 現在の環境を使用して指定された環境のいずれかと一致する場合は、環境タグ ヘルパーにその内容は表示のみ、`names`属性。

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

アプリケーションを参照してくださいタグ ヘルパーの使用を開始する[タグ ヘルパーの概要](../mvc/views/tag-helpers/intro.md)です。

## <a name="startup-conventions"></a>スタートアップの表記規則

ASP.NET Core では、現在の環境に基づくアプリケーションのスタートアップの構成、規則ベースのアプローチをサポートします。 アプリケーションの動作に基づいてどの環境には、作成し、独自の規則を管理することができますもプログラムで制御できます。

ASP.NET Core アプリケーションの起動時、`Startup`ブートス トラップ アプリケーション、その構成設定などをロードするクラスを使用 ([の詳細については、ASP.NET スタートアップ](startup.md))。 ただし、クラスが存在する場合が名前付き`Startup{EnvironmentName}`(たとえば`StartupDevelopment`)、および`ASPNETCORE_ENVIRONMENT`環境変数は、その名前をそのと一致する`Startup`クラスは、代わりに使用します。 したがって、構成することも`Startup`開発では、独立した`StartupProduction`を実稼働環境でアプリの実行時に使用されます。 またはその逆です。

> [!NOTE]
> 呼び出す`WebHostBuilder.UseStartup<TStartup>()`構成セクションをオーバーライドします。

まったく別の使用に加えて`Startup`クラスの現在の環境に基づく内でアプリケーションを構成する方法の調整を行うことも、`Startup`クラスです。 `Configure()`と`ConfigureServices()`メソッドのような環境固有のバージョンのサポート、`Startup`クラス、フォームの自体`Configure{EnvironmentName}()`と`Configure{EnvironmentName}Services()`です。 メソッドを定義する場合`ConfigureDevelopment()`の代わりに呼び出されます`Configure()`開発環境を設定するとします。 同様に、`ConfigureDevelopmentServices()`はの代わりに呼び出されます`ConfigureServices()`同じ環境内でします。

## <a name="summary"></a>まとめ

ASP.NET Core では、さまざまな機能と開発者はさまざまな環境で、アプリケーションの動作を簡単に制御を許可する規則を提供します。 実稼働環境にステージングするには、開発環境からアプリケーションを発行するときに環境変数を設定適切に環境は、必要に応じて、デバッグ、テスト、または実稼働環境で使用するアプリケーションの最適化できます。

## <a name="additional-resources"></a>その他のリソース

* [構成](xref:fundamentals/configuration/index)

* [Tag Helpers の概要](../mvc/views/tag-helpers/intro.md)
