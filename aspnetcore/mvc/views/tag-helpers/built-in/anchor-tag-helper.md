---
title: "ASP.NET Core のアンカー タグ ヘルパー"
author: pkellner
description: "アンカー タグ ヘルパーを使用する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>アンカー タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 

アンカー タグ ヘルパーは、新しい属性を追加することで HTML アンカー (`<a ... ></a>`) タグを拡張します。 (`href` タグで) 生成されるリンクは、新しい属性を使用して作成されます。 その URL には、https などの省略可能なプロトコルを含めることができます。

このドキュメントのサンプルでは、以下のスピーカー コントローラーを使用します。

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>アンカー タグ ヘルパーの属性

### <a name="asp-controller"></a>asp-controller

`asp-controller` は URL の生成に使用されるコントローラーとの関連付けに使用されます。 指定されたコントローラーが、現在のプロジェクトに存在する必要があります。 次のコードでは、すべてのスピーカーが一覧表示されます。 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

次のようなマークアップが生成されます。

```html
<a href="/Speaker">All Speakers</a>
```

`asp-controller` が指定され、`asp-action` が指定されない場合は、既定の `asp-action` が現在実行されているビューの既定のコントローラーのメソッドになります。 つまり、上記の例で、`asp-action` が除外されていて、このアンカー タグ ヘルパーが *HomeController* の `Index` ビュー (**/Home**) から生成される場合、次のようなマークアップが生成されます。

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` は、生成される `href` に含まれるコントローラーのアクション メソッドの名前です。 たとえば、次のコードは、スピーカーの詳細ページを示すために生成される `href` を設定します。

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

次のようなマークアップが生成されます。

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

`asp-controller` 属性が指定されていない場合、現在のビューを実行しているビューを呼び出す既定のコントローラーが使用されます。  
 
属性 `asp-action` が `Index` で、URL に操作が何も追加されていない場合、既定の `Index` メソッドが呼び出されます。 指定された (または既定の) 操作が、`asp-controller` で参照されるコントローラーに存在する必要があります。

### <a name="asp-page"></a>asp-page

`asp-page` 属性をアンカー タグで使用すると、URL が特定のページを示すように設定されます。 ページ名の前にスラッシュ "/" を付けると URL が作成されます。 以下のサンプルの URL は、現在のディレクトリの "スピーカー" ページを示しています。

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

前のコード サンプルの `asp-page` 属性は、ビューに次のスニペットに似た HTML 出力を表示します。

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

`asp-page` 属性は `asp-route`、`asp-controller`、`asp-action` の各属性と相互に排他的です。 ただし、`asp-page` は、次のコード サンプルで示されているように、`asp-route-id` とともに使用してルーティングを制御できます。

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` で生成される出力は次のとおりです。

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Razor ページで `asp-page` 属性を使用するには、`"./Speaker"` のように、URL が相対パスである必要があります。 `asp-page` 属性の相対パスは MVC ビューでは使用できません。 MVC ビューでは、代わりに "/" の構文を使用します。

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` はワイルド カード ルート プレフィックスです。 末尾ダッシュの後ろにある値はすべて、潜在的なルート パラメーターと解釈されます。 既定のルートが見つからない場合は、このルート プレフィックスが生成された href に要求パラメーターおよび値として追加されます。 見つかった場合は、そのルートがルート テンプレートで置き換えられます。

次のようにコントローラー メソッドが定義されていることが前提です。

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

さらに、既定のルート テンプレートが次のように *Startup.cs* に定義されている必要もあります。

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

コントローラーからビューに渡される**スピーカー** モデル パラメーターを使用するために必要なアンカー タグ ヘルパーが含まれる **cshtml** ファイルは次のようになります。

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

**id** が既定のルートで見つかったため、次のような HTML が生成されます。

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

ルート プレフィックスが見つかったルーティング テンプレートに含まれていない場合、**cshtml** ファイルは次のようになります。

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

**speakerid** が一致するルートで見つからなかったため、次のような HTML が生成されます。

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

`asp-controller` または `asp-action` のどちらかが指定されていない場合は、`asp-route` 属性の場合と同じ既定の処理に従います。

### <a name="asp-route"></a>asp-route

`asp-route` により、名前付きルートに直接リンクする URL を作成することができます。 ルーティング属性を使用すると、`SpeakerController` に示されているようにルートに名前を付け、その `Evaluations` メソッドで使用することができます。

`Name = "speakerevals"` からアンカー タグ ヘルパーに、URL `/Speaker/Evaluations` を使用してそのコントローラー メソッドに直接ルートを生成するように指示されます。 `asp-controller` または `asp-action` が `asp-route` の他に指定されていると、想定と異なるルートが生成される場合があります。 ルートの競合を防ぐには、`asp-route` を属性 `asp-controller` または `asp-action` のどちらかとともに使用してはいけません。

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` では、キーがパラメーター名で、値がそのキーに関連付けられた値であるキー値ペアのディクショナリを生成できます。

以下の例に示すように、インライン ディクショナリが作成され、Razor ビューにデータが渡されます。 または、データがモデルによって渡される場合もあります。

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

上記のコードでは、次の URL が生成されます: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

リンクをクリックすると、コントローラー メソッド `EvaluationsCurrent` が呼び出されます。 これが呼び出されるのは、そのコントローラーに `asp-all-route-data` ディクショナリから作成されたものと一致する 2 つの文字列パラメーターが含まれているためです。

ディクショナリのいずれかのキーがルート パラメーターと一致している場合、その値はルートで適宜置き換えられ、一致しない他の値が要求パラメーターとして生成されます。

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` では URL に追加される URL フラグメントが定義されます。 アンカー タグ ヘルパーにより、ハッシュ文字 (#) が追加されます。 タグを作成する場合: 

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

次の URL が生成されます: http://localhost/Speaker/Evaluations#SpeakerEvaluations

ハッシュ タグはクライアント側アプリケーションを構築するときに便利です。 たとえば、JavaScript でのマーク付けや検索を簡単にするために使用できます。

### <a name="asp-area"></a>asp-area

`asp-area` は ASP.NET Core が適切なルートを設定するために使用する区分名を設定します。 区分の属性によって、どのようにルートの再マップが行われるかの例を以下に示します。 `asp-area` を [ブログ] に設定すると、このアンカー タグの関連付けられているコントローラーとビューのルートに、ディレクトリ `Areas/Blogs` のプレフィックスが付けられます。

* プロジェクト名
  * wwwroot
  * 区分
    * ブログ
      * コントローラー
        * HomeController.cs
      * ビュー
        * Home
          * Index.cshtml
          * AboutBlog.cshtml
  * コントローラー

```AboutBlog.cshtml``` ファイルを参照するときに ```area="Blogs"``` などの有効な区分タグを指定すると、アンカー タグ ヘルパーを使用した次のようなファイルになります。

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

生成される HTML には区分セグメントが含まれ、次のようになります。

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> MVC 区分が Web アプリケーションで動作するには、ルート テンプレートに区分への参照 (存在する場合) が含まれている必要があります。 その `routes.MapRoute` メソッド呼び出しの 2 番目のパラメーターであるテンプレートは次のように表示されます: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

`asp-protocol` は URL に (`https` などの) プロトコルを指定するためにあります。 プロトコルを含むアンカー タグ ヘルパーの例は、次のとおりです。

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

HTML は次のように生成されます。

```<a href="https://localhost/Home/About">About</a>```

例ではドメインが localhost ですが、アンカー タグ ヘルパーは URL を生成するときに Web サイトのパブリック ドメインを使用します。

## <a name="additional-resources"></a>その他の技術情報

* [領域](xref:mvc/controllers/areas)
