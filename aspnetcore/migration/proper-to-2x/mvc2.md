---
title: "ASP.NET から ASP.NET をコア 2.0 の移行"
author: isaac2004
description: "このリファレンス ドキュメントでは、既存の ASP.NET MVC または Web API アプリケーションを ASP.NET Core 2.0 に移行するときのガイダンスを示します。"
keywords: "ASP.NET Core,MVC,移行"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: 68188072da5a857d730a1bc8a57df0ef6d10b922
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>ASP.NET から ASP.NET をコア 2.0 の移行

著者: [Isaac Levin](https://isaaclevin.com)

この記事は、ASP.NET アプリケーションを ASP.NET Core 2.0 に移行するための参考ガイドです。

## <a name="prerequisites"></a>必須コンポーネント

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降。

## <a name="target-frameworks"></a>ターゲット フレームワーク
ASP.NET Core 2.0 プロジェクトを使うと、開発者は、.NET Core と .NET Framework のどちらか一方または両方を対象にして柔軟に開発できます。 最も適切なターゲット フレームワークの決定については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)」をご覧ください。

.NET Framework を対象にする場合は、プロジェクトで個々の NuGet パッケージを参照する必要があります。

.NET Core を対象にすると、ASP.NET Core 2.0 [メタパッケージ](xref:fundamentals/metapackage)のおかげで、さまざまな明示的パッケージ参照をしなくて済みます。 `Microsoft.AspNetCore.All` メタパッケージをプロジェクトにインストールします。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

メタパッケージを使うと、メタパッケージ内で参照されているパッケージはアプリでは展開されません。 .NET Core ランタイム ストアにはこれらのアセットが含まれており、パフォーマンス向上のためにプリコンパイルされています。 詳しくは、「[Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage)」(ASP.NET Core 2.x 用 Microsoft.AspNetCore.All メタパッケージ) をご覧ください。

## <a name="project-structure-differences"></a>プロジェクトの構造の違い
*.csproj* ファイルの形式は、ASP.NET Core では簡素化されています。 いくつかの重要な変更は次のとおりです。
- ファイルがプロジェクトの一部と見なされるためにファイルを明示的に含める必要はありません。 これにより、大規模なチームで作業する場合に XML のマージが競合するリスクが軽減されます。
- 他のプロジェクトを GUID で参照することはなくなり、ファイルの読みやすさが向上します。
- Visual Studio でアンロードせずにファイルを編集することができます。

    ![Visual Studio 2017 の CSPROJ の編集コンテキスト メニュー オプション](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Global.asax ファイルの置換
ASP.NET Core では、アプリをブートストラップする新しいメカニズムが導入されました。 ASP.NET アプリケーションのエントリ ポイントは、*Global.asax* ファイルです。 ルート構成、フィルター、領域の登録などのタスクは、*Global.asax* ファイルで処理されます。

[!code-csharp[Main](samples/globalasax-sample.cs)]

このアプローチでは、アプリケーションとその展開先のサーバーが、実装を妨げるような方法で結合されます。 結合を切り離すため、複数のフレームワークを一緒に使うさらにクリーンな方法を提供する [OWIN](http://owin.org/) が導入されました。 OWIN は、必要なモジュールのみを追加するためのパイプラインを提供します。 ホスティング環境は、[Startup](xref:fundamentals/startup) 関数を取得して、サービスとアプリの要求パイプラインを構成します。 `Startup` は、ミドルウェアのセットをアプリケーションに登録します。 アプリケーションは、要求ごとに、既存のハンドラーのセットに対するリンク リストのヘッド ポインターを指定して、各ミドルウェア コンポーネントを呼び出します。 各ミドルウェア コンポーネントは、要求処理パイプラインに 1 つ以上のハンドラーを追加できます。 これは、新しいリストのヘッドであるハンドラーへの参照を返すことによって行われます。 各ハンドラーは、リスト内の次のハンドラーを記憶して呼び出します。 ASP.NET Core では、アプリケーションへのエントリ ポイントは `Startup` であり、*Global.asax* に依存する必要はなくなりました。 .NET Framework で OWIN を使うときは、パイプラインとして次のようなものを使います。

[!code-csharp[Main](samples/webapi-owin.cs)]

これにより既定のルートが構成され、既定では Json 経由の XmlSerialization です。 必要に応じて、このパイプラインに他のミドルウェアを追加します (サービスの読み込み、構成設定、静的ファイルなど)。

ASP.NET Core は同様のアプローチを使いますが、エントリを処理するために OWIN には依存しません。 代わりに、(コンソール アプリケーションと同じように) *Program.cs* の `Main` メソッドを通して行われ、そこから `Startup` が読み込まれます。

[!code-csharp[Main](samples/program.cs)]

`Startup` は、`Configure` メソッドを含む必要があります。 `Configure` では、必要なミドルウェアをパイプラインに追加します。 (既定の Web サイト テンプレートからの) 次の例では、複数の拡張メソッドを使って、以下をサポートするパイプラインが構成されています。

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* エラー ページ
* 静的ファイル
* ASP.NET Core MVC
* 同一。

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

ホストとアプリケーションは切り離されており、将来別のプラットフォームに柔軟に移動できます。

**注:** ASP.NET Core のスタートアップとミドルウェアについて詳しくは、「[Application Startup in ASP.NET Core](xref:fundamentals/startup)」(ASP.NET Core でのアプリケーションのスタートアップ) をご覧ください。

## <a name="storing-configurations"></a>保存の構成
ASP.NET では保存の設定がサポートされています。 これらの設定は、たとえば、アプリケーションが展開された環境のサポートに使われます。 一般的な方法は、すべてのカスタム キー/値ペアを、*Web.config* ファイルの `<appSettings>` セクションに保存するというものでした。

[!code-xml[Main](samples/webconfig-sample.xml)]

アプリケーションでは、`System.Configuration` 名前空間内の `ConfigurationManager.AppSettings` コレクションを使ってこれらの設定を読み取ります。

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core では、アプリケーションの構成データを任意のファイルに保存し、ミドルウェアのブートストラップの一部として読み込むことができます。 プロジェクト テンプレートで使われる既定のファイルは、*appsettings.json* です。

[!code-json[Main](samples/appsettings-sample.json)]

アプリケーション内部の `IConfiguration` のインスタンスにこのファイルを読み込むには、*Startup.cs* が使われます。

[!code-csharp[Main](samples/startup-builder.cs)]

アプリでは、`Configuration` から読み取って設定を取得します。

[!code-csharp[Main](samples/read-appsettings.cs)]

このアプローチにはプロセスをより堅牢にする拡張機能があります。たとえば、[依存性の注入](xref:fundamentals/dependency-injection) (DI) を使ってサービスとこれらの値を読み込むことができます。 DI アプローチは、厳密に型指定された構成オブジェクトのセットを提供します。

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**注:** ASP.NET Core の構成について詳しくは、「[Configuration in ASP.NET Core](xref:fundamentals/configuration/index)」(ASP.NET Core の構成) をご覧ください。

## <a name="native-dependency-injection"></a>ネイティブな依存性の注入
大規模で拡張性の高いアプリケーションを構築するときの重要な目標は、コンポーネントとサービスの疎な結合です。 [依存性の注入](xref:fundamentals/dependency-injection)はこれを実現するための一般的な手法であり、ASP.NET Core のネイティブなコンポーネントです。

ASP.NET アプリケーションでは、開発者はサードパーティのライブラリに依存して依存性の注入を実装します。 [Unity](https://github.com/unitycontainer/unity) はそのようなライブラリの 1 つであり、Microsoft Patterns & Practices によって提供されます。 

Unity で依存性の注入を設定する例は、`UnityContainer` をラップする `IDependencyResolver` の実装です。

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

`UnityContainer` のインスタンスを作成し、サービスを登録して、`HttpConfiguration` の依存関係リゾルバーをコンテナー用の `UnityResolver` の新しいインスタンスに設定します。

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

必要な場所に `IProductRepository` を挿入します。

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

依存性の注入は ASP.NET Core の一部であるため、*Startup.cs* の `ConfigureServices` メソッドに独自のサービスを追加できます。

[!code-csharp[Main](samples/configure-services.cs)]

Unity でそうであったように、リポジトリは任意の場所に挿入できます。

**注:** ASP.NET Core での依存性の注入について詳しくは、「[Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)」(ASP.NET Core での依存性の注入) をご覧ください。

## <a name="serving-static-files"></a>静的ファイルの提供
Web 開発の重要な部分は、静的なクライアント側アセットを提供する機能です。 静的なファイルの最も一般的な例は、HTML、CSS、Javascript、およびイメージです。 これらのファイルは、アプリ (または CDN) の公開された場所に保存され、要求によって読み込めるように参照される必要があります。 このプロセスは、ASP.NET Core で変更されました。

ASP.NET では、静的ファイルはさまざまなディレクトリに保存され、ビューで参照されます。

ASP.NET Core では、構成が変更されていない限り、静的ファイルは "Web ルート" (*&lt;コンテンツ ルート&gt;/wwwroot*) に保存されます。 ファイルは、`Startup.Configure` から `UseStaticFiles` 拡張メソッドを呼び出すことによって、要求パイプラインに読み込まれます。

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**注:** .NET Framework を対象にする場合は、NuGet パッケージ `Microsoft.AspNetCore.StaticFiles` をインストールします。

たとえば、*wwwroot/images* フォルダー内のイメージ アセットには、ブラウザーから `http://<app>/images/<imageFileName>` などの場所でアクセスできます。

**注:** ASP.NET Core での静的ファイルの提供について詳しくは、「[Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files)」(ASP.NET Core での静的ファイルの使用の概要) をご覧ください。

## <a name="additional-resources"></a>その他のリソース
* [.NET Core にライブラリを移植する](https://docs.microsoft.com/dotnet/core/porting/libraries)
