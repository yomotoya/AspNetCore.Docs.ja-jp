---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "ASP.NET Web API 2 の構成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1c007c4c327b7cde6ff52c6b0022acdff3c9b137
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-aspnet-web-api-2"></a>ASP.NET Web API 2 の構成
====================
によって[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、ASP.NET Web API を構成する方法について説明します。

- [構成設定](#settings)
- [ホスティングで ASP.NET Web API の構成](#webhost)
- [OWIN の自己ホスト Web API の構成](#selfhost)
- [グローバルの Web API サービス](#services)
- [コント ローラーごとの構成](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>構成設定

Web API の構成設定が定義されている、 [HttpConfiguration](https://msdn.microsoft.com/en-us/library/system.web.http.httpconfiguration.aspx)クラスです。

| メンバー | 説明 |
| --- | --- |
| **DependencyResolver** | コント ローラーの依存関係の挿入を有効にします。 参照してください[、Web API の依存関係競合回避モジュールを使用して](dependency-injection.md)です。 |
| **フィルター** | アクション フィルター。 |
| **フォーマッタ** | [メディア タイプ フォーマッタ](../formats-and-model-binding/media-formatters.md)です。 |
| **IncludeErrorDetailPolicy** | サーバーが HTTP 応答メッセージで例外メッセージやスタック トレースなどのエラーの詳細を含めるかどうかを指定します。 参照してください[IncludeErrorDetailPolicy](https://msdn.microsoft.com/en-us/library/system.web.http.includeerrordetailpolicy(v=vs.108))です。 |
| **初期化子** | 最終初期化を実行する関数、 **HttpConfiguration**です。 |
| **MessageHandlers** | [HTTP メッセージ ハンドラー](http-message-handlers.md)です。 |
| **ParameterBindingRules** | コント ローラー アクションのパラメーターをバインドするための規則のコレクション。 |
| **プロパティ** | 汎用プロパティ バッグ。 |
| **ルート** | ルートのコレクション。 参照してください[ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。 |
| **サービス** | サービスのコレクション。 参照してください[Services](#services)です。 |


## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition。

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>ホスティングで ASP.NET Web API の構成

ASP.NET アプリケーションで Web API を呼び出して構成[GlobalConfiguration.Configure](https://msdn.microsoft.com/en-us/library/system.web.http.globalconfiguration.configure.aspx)で、**アプリケーション\_開始**メソッドです。 **構成**メソッドが型の 1 つのパラメーターを持つデリゲートを受け取る**HttpConfiguration**です。 デリゲート内の構成のすべてを実行します。

匿名デリゲートの使用例を次に示します。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

Visual Studio 2017 で「ASP.NET Web アプリケーション」のプロジェクト テンプレートを自動的に設定、構成コードの"Web API"を選択した場合、**新しい ASP.NET プロジェクト**ダイアログ。

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

プロジェクト テンプレートは、アプリ内で WebApiConfig.cs をという名前のファイルを作成\_開始フォルダーです。 このコード ファイルでは、Web API 構成コードを配置する必要があります、デリゲートを定義します。

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

プロジェクト テンプレートもからデリゲートを呼び出すコードを追加**アプリケーション\_開始**です。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>OWIN の自己ホスト Web API の構成

OWIN の自己ホストの場合は、作成、新しい**HttpConfiguration**インスタンス。 このインスタンスでの構成を行うし、インスタンスを渡す、 **Owin.UseWebApi**拡張メソッド。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

チュートリアル[Self-Host ASP.NET Web API 2 を使用する OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)完了手順を示しています。

<a id="services"></a>
## <a name="global-web-api-services"></a>グローバルの Web API サービス

**HttpConfiguration.Services**コレクションには、コント ローラーの選択とコンテンツ ネゴシエーションなど、さまざまなタスクを実行する Web API を使用するグローバル サービスのセットが含まれています。

> [!NOTE]
> **Services**コレクションは、サービスの検出または依存関係の挿入の汎用の機構ではありません。 Web API フレームワークに認識されているサービスの種類のみ格納されます。


**Services**コレクションは、サービスの既定のセットを初期化し、独自のカスタム実装を提供することができます。 いくつかのサービスは、インスタンス 1 つだけが他のユーザーに複数のインスタンスをサポートします。 (ただし、コント ローラー レベルでサービスを提供することもできます。 を参照してください[コント ローラーごとの構成](#percontrollerconfig)です。

単一インスタンス サービス


| サービス | 説明 |
| --- | --- |
| **IActionValueBinder** | パラメーターのバインドを取得します。 |
| **IApiExplorer** | アプリケーションによって公開される Api の説明を取得します。 参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)です。 |
| **IAssembliesResolver** | アプリケーションのアセンブリの一覧を取得します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **IBodyModelValidator** | メディアの種類のフォーマッタによって要求の本体から読み取られるモデルを検証します。 |
| **IContentNegotiator** | コンテンツ ネゴシエーションを実行します。 |
| **IDocumentationProvider** | Api のドキュメントを提供します。 既定値は**null**です。 参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)です。 |
| **IHostBufferPolicySelector** | ホストが HTTP メッセージのエンティティ ボディをバッファーするかどうかを示します。 |
| **IHttpActionInvoker** | コント ローラーのアクションを呼び出します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **IHttpActionSelector** | コント ローラーのアクションを選択します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **IHttpControllerActivator** | コント ローラーがアクティブにします。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **IHttpControllerSelector** | コント ローラーを選択します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **IHttpControllerTypeResolver** | アプリケーションで Web API コント ローラーの種類の一覧を示します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。 |
| **ITraceManager** | このトレース フレームワークを初期化します。 参照してください[ASP.NET web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md)です。 |
| **ITraceWriter** | トレース ライターを提供します。 既定では、"no-op"トレース ライターです。 参照してください[ASP.NET web API Tracing](../testing-and-debugging/tracing-in-aspnet-web-api.md)です。 |
| **IModelValidatorCache** | モデル検証コントロールのキャッシュを提供します。 |

複数のインスタンスのサービス


| サービス | 説明 |
| --- | --- |
| **IFilterProvider** | コント ローラーのアクションのフィルターの一覧を返します。 |
| **ModelBinderProvider** | 特定の種類のモデル バインダーを返します。 |
| **ModelMetadataProvider** | モデルのメタデータを提供します。 |
| **ModelValidatorProvider** | モデルの検証コントロールを提供します。 |
| **ValueProviderFactory** | 値プロバイダーを作成します。 詳細については、Mike Stall のブログの投稿を参照してください[WebAPI でカスタム値プロバイダーを作成する方法。](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |。

マルチ インスタンスのサービスに、カスタム実装を追加するには、呼び出す**追加**または**挿入**上、 **Services**コレクション。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

呼び出しに単一インスタンスのサービスをカスタム実装に置き換えて、**置換**上、 **Services**コレクション。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>コント ローラーごとの構成

コント ローラーあたりごとに、次の設定をオーバーライドすることができます。

- メディア タイプ フォーマッタ
- パラメーター バインディング規則
- サービス

これを行うを実装するカスタム属性を定義する、 **IControllerConfiguration**インターフェイスです。 コント ローラーに属性が適用されます。

次の例は、既定のメディア タイプ フォーマッタをカスタム フォーマッタに置き換えます。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize**メソッドは、2 つのパラメーターを受け取ります。

- **HttpControllerSettings**オブジェクト
- **HttpControllerDescriptor**オブジェクト

**HttpControllerDescriptor**情報目的 (つまり、2 つのコント ローラー間で区別するために) を確認することができますが、コント ローラーの説明が含まれています。

使用して、 **HttpControllerSettings**コント ローラーを構成するオブジェクト。 このオブジェクトには、コント ローラーあたりごとにオーバーライド可能な構成パラメーターのサブセットが含まれています。 すべての設定は変更しないことを既定でグローバルに**HttpConfiguration**オブジェクト。
