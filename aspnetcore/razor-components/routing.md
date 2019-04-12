---
title: ルーティング razor コンポーネント
author: guardrex
description: アプリや NavLink コンポーネントの詳細については、要求をルーティングする方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515675"
---
# <a name="razor-components-routing"></a>ルーティング razor コンポーネント

作成者: [Luke Latham](https://github.com/guardrex)

アプリや NavLink コンポーネントの詳細については、要求をルーティングする方法について説明します。

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core エンドポイントのルーティングの統合

Razor のコンポーネントが統合[ASP.NET Core ルーティング](xref:fundamentals/routing)します。 ASP.NET Core アプリが対話型の Razor コンポーネントでの着信接続を受け入れるように構成されている`MapComponentHub<TComponent>`で`Startup.Configure`します。 `MapComponentHub` 指定するルート コンポーネント`App`セレクターに一致する DOM 要素内にレンダリングする必要があります`app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>ルート テンプレート

`<Router>`ルーティング、コンポーネントを使用して、ルート テンプレートは、アクセス可能な各コンポーネントに提供されます。 `<Router>`コンポーネントに表示されます、 *Components/App.razor*ファイル。

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

ときに、 *.razor*または *.cshtml*ファイルと、`@page`ディレクティブをコンパイルすると、生成されたクラスが指定された、<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>ルート テンプレートを指定します。 ルーターが持つクラスでコンポーネントが、実行時に、`RouteAttribute`どのコンポーネントが要求された URL に一致するルート テンプレートを表示します。

コンポーネントには、複数のルート テンプレートを適用できます。 次のコンポーネントがの要求に応答`/BlazorRoute`と`/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` では、レンダリング時に要求されたルートのフォールバック コンポーネントの設定は、解決はありません。 設定をこのオプトイン シナリオを有効にする、`FallbackComponent`フォールバック コンポーネント クラスの型のパラメーター。

次の例で定義されているコンポーネントを設定する*Pages/MyFallbackRazorComponent.cshtml*フォールバック コンポーネントとして、 `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> ルートを正しく生成するアプリを含める必要があります、`<base>`タグにその*wwwroot/index.html*で指定したアプリ ベース パスとファイル、`href`属性 (`<base href="/">`)。 詳細については、「 <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path> 」を参照してください。

## <a name="route-parameters"></a>ルート パラメーター

ルーターは、同じ名前 (大文字と小文字は区別されません) の対応するコンポーネントのパラメーターを設定するのにルート パラメーターを使用します。

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

省略可能なパラメーターは、まだサポートされていませんので、2 つ`@page`ディレクティブは、上記の例に適用されます。 最初のパラメーターを指定せず、コンポーネントへの移動を許可します。 2 番目の`@page`ディレクティブは、`{text}`ルート パラメーターと値を割り当てます、`Text`プロパティ。

## <a name="route-constraints"></a>ルート制約

ルート制約では、コンポーネントへのルート セグメントで一致する型を強制します。

次の例では、ルート ユーザー コンポーネントにのみ一致する場合。

* `Id`ルート セグメントでは、要求 URL に存在します。
* `Id`セグメントは、整数 (`int`)。

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

次の表に示すようにルート制約は使用できます。 インバリアント カルチャに一致するルート制約では、詳細については、表の下には、警告を参照してください。

| 制約 | 例           | 一致の例                                                                  | インバリアント<br>カルチャ<br>一致 |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | いいえ                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | [はい]                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | [はい]                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | [はい]                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | はい                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | いいえ                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | はい                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | [はい]                              |

> [!WARNING]
> URL の妥当性を検証し、CLR 型 (`int` や `DateTime` など) に変換されるルート制約では、常にインバリアント カルチャが使用されます。 これらの制約では、URL がローカライズ不可であることが前提となります。

## <a name="navlink-component"></a>NavLink コンポーネント

HTML の代わりに NavLink コンポーネントを使用して`<a>`ナビゲーション リンクを作成するときの要素。 NavLink コンポーネントと同様に動作を`<a>`切り替えます点を除いて、要素、 `active` CSS クラスかどうかに基づいて、`href`現在の URL と一致します。 `active`クラスは、どのページが表示されるナビゲーション リンクの間でのアクティブなページを理解します。

次の NavMenu コンポーネントを作成、[ブートス トラップ](https://getbootstrap.com/docs/)NavLink コンポーネントを使用する方法については、ナビゲーション バー。

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

2 つ`NavLinkMatch`オプション。

* `NavLinkMatch.All` &ndash; 全体の現在の URL と一致するときに、NavLink をアクティブながあることを指定します。
* `NavLinkMatch.Prefix` &ndash; 現在の URL のプレフィックスと一致するときに、NavLink をアクティブながあることを指定します。

前の例では、ホーム NavLink (`href=""`) すべての Url と一致して、常に受信、 `active` CSS クラス。 2 番目の NavLink のみを受信、 `active` Blazor ルート コンポーネントにアクセスするユーザー クラス (`href="BlazorRoute"`)。
