---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: ASP.NET Web API 2 の構成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 37df04487404a9eb0753b5f2ae48bffb51d1c363
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840366"
---
<a name="configuring-aspnet-web-api-2"></a>ASP.NET Web API 2 の構成
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、ASP.NET Web API を構成する方法について説明します。

- [構成設定](#settings)
- [ASP.NET ホストによる Web API の構成](#webhost)
- [OWIN 自己ホストによる Web API の構成](#selfhost)
- [グローバルな Web API サービス](#services)
- [コント ローラーごとの構成](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>構成設定

Web API の構成設定が定義されている、 [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)クラス。

| メンバー | 説明 |
| --- | --- |
| **DependencyResolver** | コント ローラーの依存関係の挿入を有効にします。 参照してください[、Web API の依存関係競合回避モジュールを使用して](dependency-injection.md)します。 |
| **Filters** | アクション フィルター。 |
| **Formatters** | [メディア タイプ フォーマッタ](../formats-and-model-binding/media-formatters.md)です。 |
| **IncludeErrorDetailPolicy** | サーバーが HTTP 応答メッセージで例外メッセージやスタック トレースなどのエラーの詳細を含めるかどうかを指定します。 参照してください[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))します。 |
| **Initializer** | 最終初期化を実行する関数、**HttpConfiguration**です。 |
| **MessageHandlers** | [HTTP メッセージ ハンドラー](http-message-handlers.md)します。 |
| **ParameterBindingRules** | コント ローラー アクションのパラメーターをバインドするための規則のコレクション。 |
| **Properties** | 汎用プロパティ バッグ。 |
| **Routes** | ルートのコレクション。 参照してください[ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。 |
| **Services** | サービスのコレクション。 参照してください[Services](#services)です。 |


## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition。

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>ASP.NET ホストによる Web API の構成

ASP.NET アプリケーションで、**Application\_Start** メソッドから [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) を呼び出して Web API を設定します。 **構成**メソッドは型の 1 つのパラメーターを持つデリゲートを受け取ります**HttpConfiguration**します。 デリゲート内で、構成のすべてを実行します。

匿名デリゲートの使用例を次に示します。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

Visual Studio 2017 では、「ASP.NET Web アプリケーション」プロジェクト テンプレートを自動的に設定、構成コードの"Web API"を選択した場合、**新しい ASP.NET プロジェクト**ダイアログ。

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

プロジェクト テンプレートは、アプリ内で WebApiConfig.cs という名前のファイルを作成します。\_開始フォルダー。 このコード ファイルは、Web API の構成コードを配置する必要があります、デリゲートを定義します。

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

プロジェクト テンプレートもからデリゲートを呼び出すコードを追加**Application\_Start**です。


[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>OWIN 自己ホストによる Web API の構成

OWIN 自己ホストする場合は、作成、新しい**HttpConfiguration**インスタンス。 このインスタンスで、任意の構成を実行し、インスタンスを渡す、 **Owin.UseWebApi**拡張メソッド。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

このチュートリアル[Self-Host ASP.NET Web API 2 を使用して OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)は完全な手順について説明します。

<a id="services"></a>
## <a name="global-web-api-services"></a>グローバルな Web API サービス

**HttpConfiguration.Services**コレクションには、Web API を使用してコント ローラーの選択とコンテンツ ネゴシエーションなどのさまざまなタスクを実行するグローバル サービスのセットが含まれています。

> [!NOTE]
> **サービス**コレクションは、サービスの検出または依存関係の挿入の汎用メカニズムではありません。 Web API フレームワークに認識されているサービスの種類のみ格納されます。


**サービス**コレクションは、サービスの既定のセットで初期化されており、独自のカスタム実装を行うことができます。 一部のサービスは、インスタンス 1 つだけを持つ他のユーザーに複数のインスタンスをサポートします。 (ただし、コント ローラー レベルでサービスを提供することもできます。 を参照してください[コント ローラーごとの構成](#percontrollerconfig)します。

単一インスタンス サービス


| サービス | 説明 |
| --- | --- |
| **IActionValueBinder** | パラメーターのバインドを取得します。 |
| **IApiExplorer** | アプリケーションによって公開される Api の説明を取得します。 参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)します。 |
| **IAssembliesResolver** | アプリケーションのアセンブリの一覧を取得します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **IBodyModelValidator** | 要求本文からメディア タイプ フォーマッタによって読み取られるモデルを検証します。 |
| **IContentNegotiator** | コンテンツ ネゴシエーションを実行します。 |
| **IDocumentationProvider** | Api のドキュメントを提供します。 既定値は**null**します。 参照してください[Web API のヘルプ ページを作成する](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)します。 |
| **IHostBufferPolicySelector** | ホストで HTTP メッセージのエンティティ ボディをバッファーする必要があるかどうかを示します。 |
| **IHttpActionInvoker** | コント ローラー アクションを呼び出します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **IHttpActionSelector** | コント ローラーのアクションを選択します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **IHttpControllerActivator** | コント ローラーがアクティブにします。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **IHttpControllerSelector** | コント ローラーを選択します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **IHttpControllerTypeResolver** | アプリケーションで Web API コント ローラーの種類の一覧を示します。 参照してください[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。 |
| **ITraceManager** | このトレース フレームワークを初期化します。 参照してください[ASP.NET Web API でのトレース](../testing-and-debugging/tracing-in-aspnet-web-api.md)します。 |
| **ITraceWriter** | トレース ライターを提供します。 既定では"no op"トレース ライターです。 参照してください[ASP.NET Web API でのトレース](../testing-and-debugging/tracing-in-aspnet-web-api.md)します。 |
| **IModelValidatorCache** | モデル検証コントロールのキャッシュを提供します。 |

複数インスタンスのサービス


|                 サービス                 |                                                                                                              説明                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           コント ローラー アクションのフィルターの一覧を返します。                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                指定された型のモデル バインダーを返します。                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     モデルのメタデータを提供します。                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   モデルの検証コントロールを提供します。                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | 値プロバイダーを作成します。 詳細については、Mike Stall のブログの投稿を参照してください[WebAPI でカスタム値プロバイダーを作成する方法。](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

マルチ インスタンスのサービスには、カスタム実装を追加するには、呼び出す**追加**または**挿入**上、**サービス**コレクション。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

単一インスタンスのサービスをカスタム実装で置き換えるには、呼び出す**置換**上、**サービス**コレクション。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>コント ローラーごとの構成

コント ローラーごとに、次の設定をオーバーライドすることができます。

- メディア タイプ フォーマッタ
- パラメーター バインディング規則
- Services

これを行うには、実装するカスタム属性を定義、 **IControllerConfiguration**インターフェイス。 コント ローラーに、属性を適用します。

次の例では、カスタム フォーマッタで既定のメディア タイプ フォーマッタを置き換えます。

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize**メソッドは 2 つのパラメーターを受け取ります。

- **HttpControllerSettings**オブジェクト
- **HttpControllerDescriptor**オブジェクト

**HttpControllerDescriptor**調べることができます (たとえば、2 つのコント ローラー間で区別するために) 情報提供を目的のコント ローラーの説明が含まれています。

使用して、 **HttpControllerSettings**コント ローラーを構成するオブジェクト。 このオブジェクトには、コント ローラーごとにオーバーライドできる構成パラメーターのサブセットが含まれています。 すべての設定は変更しないことを既定のグローバル**HttpConfiguration**オブジェクト。
