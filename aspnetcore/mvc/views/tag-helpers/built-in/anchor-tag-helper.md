---
title: "アンカー タグ ヘルパー |Microsoft ドキュメント"
author: pkellner
description: "アンカー タグ ヘルパーを使用する方法を示しています。"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 503ad7c4ce8c4f08b2a06dbe9f985566f54d3ca2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/21/2017
---
# <a name="anchor-tag-helper"></a>アンカー タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 

アンカー タグ ヘルパーの強化 HTML アンカー (`<a ... ></a>`) 新しい属性を追加してタグ。 生成されたリンク (上、`href`タグ)、新しい属性を使用して作成します。 その URL には、https などの省略可能なプロトコルを含めることができます。

下の スピーカー コント ローラーは、このドキュメントのサンプルで使用されます。

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>アンカー タグ ヘルパー属性

### <a name="asp-controller"></a>asp コント ローラー

`asp-controller`URL の生成に使用するコント ローラーを関連付けるために使用します。 指定されたコント ローラーは、現在のプロジェクトに存在する必要があります。 次のコードには、すべてのスピーカーが一覧表示します。 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

生成されたマークアップになります。

```html
<a href="/Speaker">All Speakers</a>
```

場合、`asp-controller`が指定されていると`asp-action`既定ではない、`asp-action`現在実行中のビューの既定のコント ローラーのメソッドになります。 上記の例では場合、 `asp-action` 、左からこのアンカー タグ ヘルパーが生成されると*HomeController*の`Index`ビュー (**ホーム/**)、生成されたマークアップになります。

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp アクション

`asp-action`含まれているコント ローラー アクション メソッドの名前を生成された`href`です。 たとえば、次のコードは、生成された設定`href`スピーカー詳細ページを指します。

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

生成されたマークアップになります。

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

ない場合は`asp-controller`属性が指定されて、現在のビューを実行するビューを呼び出す既定のコント ローラーが使用されます。  
 
場合、属性`asp-action`は`Index`、url、既定値に先行する操作は追加されず、`Index`呼び出されるメソッド。 アクションを指定 (または既定値) で参照されているコント ローラーに存在する必要があります`asp-controller`です。

### <a name="asp-page"></a>asp ページ

使用して、`asp-page`特定ページを指す URL を設定するアンカー タグ内の属性です。 スラッシュでページ名の前に付ける「/」の URL を作成します。 以下のサンプル内の URL は、現在のディレクトリに「スピーカー」ページを指します。

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

`asp-page`上記のコード サンプルでの属性が次のスニペットは、次のようなビューでの HTML 出力を表示します。

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```
cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">ビュー スピーカー</a>
```

The `asp-route-id` produces the following output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```


### <a name="asp-route-value"></a>asp - ルート - {value}

`asp-route-`次のワイルドカード ルート プレフィックス。 任意の値は、潜在的なルートのパラメーターとして解釈されます末尾ダッシュ後に配置します。 既定のルートが見つからない場合は、要求のパラメーターと値として生成された href にこのルート プレフィックスが追加されます。 それ以外の場合、ルート テンプレートで置き換えられますされます。

前提とするとがコント ローラー メソッドように定義します。

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

定義されている既定のルート テンプレートがあると、 *Startup.cs*次のようにします。

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

**Cshtml** 、アンカー タグ ヘルパーを使用する必要のあるファイル、**スピーカー**ビューに、コント ローラーから渡されたモデル パラメーターを次に示します。

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

生成される HTML れます次のように**id**で既定のルートが見つかりました。

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

次の場合はそのルート プレフィックスがあるルーティング テンプレートの一部でない場合は、 **cshtml**ファイル。

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

生成される HTML れます次のように**speakerid**が一致するルート上で見つかりませんでした。

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

いずれか`asp-controller`または`asp-action`が指定されていないがで同じ既定の処理の後に、`asp-route`属性。

### <a name="asp-route"></a>asp ルート

`asp-route`名前付きのルートに直接リンクする URL を作成する方法を提供します。 ルーティング属性を使用して、ように、ルートをということができます、`SpeakerController`で使用されると、`Evaluations`メソッドです。

`Name = "speakerevals"`アンカー タグ ヘルパーのルート URL を使用してそのコント ローラーのメソッドを直接生成するように指示`/Speaker/Evaluations`です。 場合`asp-controller`または`asp-action`が他に指定されている`asp-route`、生成されるルートできない可能性がありますが予期したものとします。 `asp-route`属性のいずれかで使用しないで`asp-controller`または`asp-action`ルート競合を回避します。

### <a name="asp-all-route-data"></a>asp すべてルート データ

`asp-all-route-data`ここで、キーは、パラメーター名と値は、そのキーに関連付けられている値のキー値のペアのディクショナリを作成できます。

次の例に、としてインライン ディクショナリが作成され、razor ビューにデータが渡されます。 代わりに、データも、モデルを使用して渡される可能性があります。

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

上記のコードには、次の URL が生成されます: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

ときに、リンクをクリックして、コント ローラー メソッド`EvaluationsCurrent`と呼びます。 これは、そのコント ローラーがある 2 つの文字列パラメーターから作成したものと一致するために呼び出されますが、`asp-all-route-data`ディクショナリ。

ディクショナリ一致しているすべてのキーは、パラメーターをルーティングする場合として適切なルート上でそれらの値が置き換えられるし、要求パラメーターと一致しない値が生成されます。

### <a name="asp-fragment"></a>asp フラグメント

`asp-fragment`URL に追加する URL フラグメントを定義します。 アンカー タグ ヘルパー追加ハッシュ文字 (#)。 場合は、タグを作成します。

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

生成された URL になります: http://localhost/Speaker/Evaluations#SpeakerEvaluations

ハッシュのタグは、クライアント側アプリケーションを構築するときに便利です。 簡単なマークを付け、たとえば、JavaScript では、検索、使用できます。

### <a name="asp-area"></a>asp 領域

`asp-area`ASP.NET Core は適切なルートの設定を使用して領域の名前を設定します。 どの区分属性によって、ルートの再マップの例を以下に示します。 設定`asp-area`ブログに、ディレクトリをプレフィックス`Areas/Blogs`関連付けられているコント ローラーとこのアンカー タグのビューのルートにします。

* プロジェクト名
  * wwwroot
  * 区分
    * ブログ
      * コント ローラー
        * HomeController.cs
      * ビュー
        * Home
          * Index.cshtml
          * AboutBlog.cshtml
  * コント ローラー

などの無効である area タグを指定する```area="Blogs"```参照するときに、```AboutBlog.cshtml```ファイルは、次のようになりますアンカー タグ ヘルパーの使用します。

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

生成される HTML では、領域セグメントが含まれますされ、次のようになります。

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Web アプリケーションで作業する MVC 区分のルート テンプレートは存在する場合領域への参照を含める必要があります。 そのテンプレートでは、2 番目のパラメーターの`routes.MapRoute`メソッド呼び出しとして表示されます。`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp プロトコル

`asp-protocol` 、プロトコルを指定するのには (など`https`)、URL にします。 プロトコルを含むアンカー タグ ヘルパーの使用例は、次のようになります。

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

次のように HTML を生成します。

```<a href="https://localhost/Home/About">About</a>```

ドメインの例では、localhost がアンカー タグ ヘルパーは、URL を生成するときに、web サイトのパブリック ドメインを使用します。

## <a name="additional-resources"></a>その他の技術情報

* [領域](xref:mvc/controllers/areas)
