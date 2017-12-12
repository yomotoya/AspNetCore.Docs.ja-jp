---
title: "アプリケーション モデルの操作"
author: ardalis
description: 
keywords: "ASP.NET Core,ASP.NET コア MVC、アプリケーション モデル"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a>アプリケーション モデルの操作

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 定義、*アプリケーション モデル*MVC アプリのコンポーネントを表すです。 読み取りおよび MVC 要素の動作を変更するには、このモデルを操作できます。 既定では、MVC はクラスは、コント ローラーであると見なされます、これらのクラスにする方法は、アクション、およびパラメーターとルーティングの動作方法を決定する特定の規則に従います。 ニーズに合わせて、アプリケーションの独自の規則を作成し、グローバル、または属性として適用するには、この動作をカスタマイズすることができます。

## <a name="models-and-providers"></a>モデルおよびプロバイダー

ASP.NET Core MVC アプリケーションのモデルには、抽象インターフェイスと、MVC アプリケーションを記述する具体的な実装クラスの両方が含まれます。 このモデルでは、MVC アプリケーションのコント ローラー、アクション、アクション パラメーターのルート、および既定の規則に従ってフィルターの検出の結果を示します。 アプリケーション モデルを使用して、既定の MVC 動作から別の規則に従って、アプリを変更できます。 パラメーター、名前、ルート、およびフィルターはすべてデータとして使用構成アクションおよびコント ローラー。

ASP.NET Core MVC アプリケーションのモデルには、次のような構造があります。

* ApplicationModel
    * コント ローラー (ControllerModel)
        * アクション (ActionModel)
            * パラメーター (ParameterModel)

モデルの各レベルが、共通のアクセス`Properties`コレクション、および下位レベルにアクセスして、階層の上位レベルで設定されたプロパティ値を上書きします。 プロパティに保存される、`ActionDescriptor.Properties`アクションが作成されます。 要求を処理しているときに任意のプロパティを追加または変更、規則からアクセスできます`ActionContext.ActionDescriptor.Properties`です。 -アクションごとに、フィルターやモデル バインダーなどを構成する優れた方法としては、プロパティを使用します。

> [!NOTE]
> `ActionDescriptor.Properties`コレクションはスレッド セーフはな (書き込み) のアプリのスタートアップが完了します。 規則は、このコレクションにデータを安全に追加する最善の方法です。

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC によって定義された、プロバイダーのパターンを使用して、アプリケーション モデルの読み込み、 [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider)インターフェイスです。 ここでいくつかの方法の内部実装の詳細は、このプロバイダーが機能します。 高度なトピックでは、-、アプリケーション モデルを活用するほとんどのアプリは規則で作業してこれを行う必要があります。

実装、`IApplicationModelProvider`インターフェイス「ラップ」、各実装通話で`OnProvidersExecuting`に基づいて昇順でその`Order`プロパティです。 `OnProvidersExecuted`メソッドが逆の順序で呼び出されます。 フレームワークでは、いくつかのプロバイダーを定義します。

最初 (`Order=-1000`)。

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

(`Order=-990`)。

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> どの 2 つのプロバイダーの値が同じ順序`Order`と呼ばれますが、定義されていませんし、したがってが依存していません。

> [!NOTE]
> `IApplicationModelProvider`フレームワークの作成者を拡張するための高度な概念がします。 一般に、アプリは、規則を使用する必要があり、フレームワークは、プロバイダーを使用する必要があります。 重要な違いは、プロバイダーが規則の前に常に実行することです。

`DefaultApplicationModelProvider` ASP.NET Core MVC で使用される既定の動作の多くを確立します。 その役割は次のとおりです。

* グローバル フィルターをコンテキストに追加します。
* コント ローラーのコンテキストに追加
* アクション、コント ローラーのパブリック メソッドの追加
* コンテキストにアクション メソッドのパラメーターを追加します。
* ルート アクションとその他の属性を適用します。

いくつかの組み込みのビヘイビアーはによって実装される、`DefaultApplicationModelProvider`です。 このプロバイダーは、構築するため、 [ `ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)を参照する順番[ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)、 [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)、および[`ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel)インスタンス。 `DefaultApplicationModelProvider`クラスは、内部のフレームワーク実装の詳細は、今後変更したりできます。 

`AuthorizationApplicationModelProvider`に関連付けられている動作を適用する役割が、`AuthorizeFilter`と`AllowAnonymousFilter`属性。 [これらの属性の詳細について](xref:security/authorization/simple)です。

`CorsApplicationModelProvider`に関連付けられている動作を実装、`IEnableCorsAttribute`と`IDisableCorsAttribute`、および`DisableCorsAuthorizationFilter`です。 [詳細については、CORS](xref:security/cors)です。

## <a name="conventions"></a>規則

アプリケーション モデルでは、モデル全体またはプロバイダーをオーバーライドするよりもモデルの動作をカスタマイズする簡単な方法を提供する規則の抽象化を定義します。 これらの抽象化は、アプリの動作を変更することをお勧めします。 規則のカスタマイズを動的に適用するコードを記述するための手段です。 中に[フィルター](xref:mvc/controllers/filters)フレームワークの動作を変更する手段を提供のカスタマイズでは、アプリ全体が一緒にワイヤード方法を制御できます。

次の規則を使用できます。

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

MVC のオプションを追加することによって、または実装することで、規則が適用される`Attribute`s とコント ローラー、アクション、またはアクションのパラメーターに適用する (に似て[ `Filters` ](xref:mvc/controllers/filters))。 フィルターとは異なり規則は各要求の一部ではなく、アプリの起動時にのみ実行されます。

### <a name="sample-modifying-the-applicationmodel"></a>例: ApplicationModel を変更します。

次の規則を使用すると、アプリケーション モデルにプロパティを追加します。 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

MVC が追加されたときに、オプションとしてアプリケーション モデルの規則が適用される`ConfigureServices`で`Startup`です。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

プロパティからアクセス可能な`ActionDescriptor`コント ローラー アクション内でプロパティのコレクション。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>サンプル: ControllerModel 説明を変更します。

前の例のようにに、カスタム プロパティを含める、コント ローラー モデルを変更することもできます。 これらは、アプリケーション モデルで指定された同じ名前の既存のプロパティを上書きします。 次の規則の属性は、コント ローラー レベルの説明を追加します。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

この規則は、コント ローラーに属性として適用されます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

同様に、前の例のように「説明」プロパティにアクセスします。

### <a name="sample-modifying-the-actionmodel-description"></a>サンプル: ActionModel 説明を変更します。

別の属性を規則は、既にアプリケーションまたはコント ローラーのレベルで適用される動作をオーバーライドする個々 のアクションに適用できます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

前の例のコント ローラー内のアクションにこれを適用するには、コント ローラー レベルの規則を上書きする方法を示しています。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>例: ParameterModel を変更します。

アクション パラメーターを変更するには、次の規則を適用できます、`BindingInfo`です。 次の規則は、パラメーターは、ルート パラメーター; である必要があります。その他の潜在的なバインドなどのソース (クエリ文字列の値) は無視されます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

属性は、任意のアクション パラメーターを適用できます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>サンプル: ActionModel 名の変更

次の規則を変更、`ActionModel`を更新する、*名前*が適用されるアクションのです。 新しい名前は、属性のパラメーターとして提供されます。 この新しい名前は、このアクション メソッドに到達するのに使用されるルートに影響するため、ルーティング、によって使用されます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

この属性は、アクション メソッドに適用、 `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

メソッドの名前になっても`SomeName`、属性がメソッド名を使用する MVC 規則を上書きおよびとアクション名を置換`MyCoolAction`です。 したがって、この操作に到達するのに使用されるルートは`/Home/MyCoolAction`します。

> [!NOTE]
> この例には基本的には、組み込みの使用と同じ[ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute)属性。

### <a name="sample-custom-routing-convention"></a>サンプル: カスタム ルーティング規約

使用することができます、`IApplicationModelConvention`をどのようにルーティング動作をカスタマイズします。 次の規則を置き換えて、そのルート コント ローラーの名前空間を組み込む例、`.`を持つ名前空間で`/`ルート上で。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

スタートアップのオプションとして、規則が追加されます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> 規則を追加することができます、[ミドルウェア](xref:fundamentals/middleware)にアクセスして`MvcOptions`を使用します。`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

このサンプルでは、コント ローラーが、名前に"Namespace"を持つ属性のルーティングを使用していないルートにこの規則が適用されます。 次のコント ローラーは、この規則を示しています。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>WebApiCompatShim でアプリケーション モデルの使用法

ASP.NET Core MVC は、ASP.NET Web API 2 から異なる一連の規則を使用します。 カスタム規則を使用して、Web API アプリのものと一致するように、ASP.NET Core MVC アプリの動作を変更できます。 Microsoft が付属しています、 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)特にこの目的のためです。

> [!NOTE]
> 詳細については[ASP.NET Web API からの移行](xref:migration/webapi)です。

Web API の互換性 Shim を使用するのには、パッケージをプロジェクトに追加して、MVC を呼び出すことによって、規則を追加する必要があります`AddWebApiConventions`で`Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Shim によって提供される規則は、それらに適用された特定の属性を持っているアプリの一部にのみ適用されます。 次の 4 つの属性は、コント ローラーで、規則、shim の規則によって変更する必要がありますコントロールに使用されます。

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>アクションの表記規則

`UseWebApiActionConventionsAttribute`名に基づいてアクションに HTTP メソッドをマップするために使用 (たとえば、`Get`にマップ`HttpGet`)。 属性のルーティングを使用しないアクションにのみ適用されます。

### <a name="overloading"></a>オーバーロード

`UseWebApiOverloadingAttribute`を適用するために使用、`WebApiOverloadingApplicationModelConvention`規則。 この規則の追加、`OverloadActionConstraint`アクションの選択のプロセスに制限されることは候補アクション要求がすべて非-省略可能なパラメーターを満たすものにします。

### <a name="parameter-conventions"></a>パラメーターの表記規則

`UseWebApiParameterConventionsAttribute`を適用するために使用、`WebApiParameterConventionsApplicationModelConvention`アクション規則。 この規則では、アクションのパラメーターとして使用される単純な型がバインドされている URI から既定では、複合型は、要求本文からバインドを指定します。

### <a name="routes"></a>ルート

`UseWebApiRoutesAttribute`コントロールかどうか、`WebApiApplicationModelConvention`コント ローラーの規則を適用します。 サポートを追加するこの規則が使用される有効な場合、[領域](xref:mvc/controllers/areas)ルートにします。

互換パッケージが含まれています、規則のセットだけでなく、`System.Web.Http.ApiController`基底クラスを Web API によって提供されるものを置き換えます。 これにより、Web API および継承用に記述された、コント ローラーからその`ApiController`ASP.NET Core MVC での実行中に、設計どおりに動作します。 この基本コント ローラー クラスはすべての装飾、`UseWebApi*`上記の属性です。 `ApiController`プロパティ、メソッド、および Web API で検出されると互換性のある結果の型を公開します。

## <a name="using-apiexplorer-to-document-your-app"></a>ApiExplorer を使用してアプリを文書化するには

アプリケーション モデルは、公開、 [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)プロパティ レベルに応じたアプリの構造をスキャンするために使用できます。 これに使用できる[Swagger などのツールを使用して、Web Api のヘルプ ページを生成する](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger)です。 `ApiExplorer`プロパティが公開する`IsVisible`アプリのモデルのどの部分を公開するかを指定する設定できるプロパティです。 規則を使用してこの設定を構成することができます。

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

このアプローチ (およびその他の規則が必要な場合) を使用して、有効にするにまたは、アプリ内で任意のレベルで API の可視性を無効にします。 
