---
title: "ASP.NET Core でのフォームにタグ ヘルパー"
author: rick-anderson
description: "組み込みのフォームでタグ ヘルパーの使用について説明します。"
keywords: "ASP.NET Core、タグ ヘルパーに渡し、TagHelper、HTML フォームのフォームします。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3f7792d7458013f837a48ca2caa459f35658f02
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>ASP.NET Core でのフォームにタグ ヘルパーの使用の概要

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Dave Paquette](https://twitter.com/Dave_Paquette)、および[Jerrie Pelser](https://github.com/jerriep)

このドキュメントでは、フォームとフォームでよく使用される HTML 要素の操作を示します。 HTML[フォーム](https://www.w3.org/TR/html401/interact/forms.html)要素は、主要なメカニズム web アプリを使用してポストバックをサーバーにデータを提供します。 このドキュメントのほとんどについて説明します[タグ ヘルパー](tag-helpers/intro.md)とどのように役立つ生産的に堅牢な HTML フォームを作成します。 読むことをお勧め[タグ ヘルパーの概要](tag-helpers/intro.md)このドキュメントを読む前にします。

多くの場合、HTML ヘルパー、別の方法を特定のタグ ヘルパーに提供するが、タグ ヘルパーの HTML ヘルパーを置き換えないし、各 HTML ヘルパーのタグ ヘルパーがないことを認識することが重要です。 代わりに HTML ヘルパーが存在する場合が指定されています。

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a>フォーム タグ ヘルパー

[フォーム](https://www.w3.org/TR/html401/interact/forms.html)タグ ヘルパー。

* HTML を生成[\<フォーム >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC コント ローラーのアクションまたは名前付きのルートの属性の値

* 非表示を生成[要求の検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)クロスサイト リクエスト フォージェリを防ぐために (を使用すると、 `[ValidateAntiForgeryToken]` HTTP Post のアクション メソッドの属性)

* 提供、`asp-route-<Parameter Name>`属性に、ここで`<Parameter Name>`がルートの値に追加します。 `routeValues`パラメーター`Html.BeginForm`と`Html.BeginRouteForm`同様の機能を提供します。

* HTML ヘルパーの代替手段を持つ`Html.BeginForm`と`Html.BeginRouteForm`

例:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

フォーム タグ ヘルパーの上には、次の HTML が生成されます。

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

MVC ランタイムによって生成、`action`フォーム タグ ヘルパーの属性から値を属性`asp-controller`と`asp-action`です。 フォーム タグ ヘルパー生成も非表示[要求の検証トークン](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)クロスサイト リクエスト フォージェリを防ぐために (を使用すると、 `[ValidateAntiForgeryToken]` HTTP Post のアクション メソッドの属性)。 フォーム タグ ヘルパーがこのサービスを提供、クロスサイト リクエスト フォージェリから純粋な HTML フォームを保護するは困難です。

### <a name="using-a-named-route"></a>名前付きのルートを使用します。

`asp-route`タグ ヘルパー属性、html マークアップを生成できますも`action`属性。 使用したアプリ、[ルート](../../fundamentals/routing.md)という`register`登録ページで、次のマークアップを使用でした。

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

内のビューの多くは、*ビュー/アカウント*フォルダー (で新しい web アプリを作成するときに生成される*個々 のユーザー アカウント*) が含まれて、 [asp ルート-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms)属性。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [2]}} -->

```none
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
   ```

>[!NOTE]
>組み込みのテンプレートと`returnUrl`承認済みのリソースにアクセスしようとしていますが、ことがなく認証、承認されたときに、自動的に設定されるだけです。 未承認のアクセスを試みると、セキュリティ ミドルウェアにリダイレクトする、ログイン ページで、`returnUrl`を設定します。

## <a name="the-input-tag-helper"></a>入力タグ ヘルパー

入力タグ ヘルパー バインド HTML [\<入力 >](https://www.w3.org/wiki/HTML/Elements/input)要素で、razor ビューのモデルの式をします。

構文:

```HTML
<input asp-for="<Expression Name>" />
   ```

入力タグ ヘルパー。

* 生成、`id`と`name`で指定された式の名前の HTML 属性、`asp-for`属性。 `asp-for="Property1.Property2"` は `m => m.Property1.Property2` と同じです。 式の名前は、の使用目的は、`asp-for`属性の値。 参照してください、[式名](#expression-names)詳細についてはします。

* HTML を設定`type`モデルの種類に基づいて値の属性と[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)モデル プロパティに適用される属性

* HTML は上書きされません`type`属性値のいずれかを指定します。

* 生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)から属性を検証[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)モデルのプロパティに適用される属性

* 重複する HTML ヘルパー機能を持つ`Html.TextBoxFor`と`Html.EditorFor`です。 参照してください、**入力タグ ヘルパーの HTML ヘルパー代替**詳細セクションです。

* 厳密な型指定を提供します。 プロパティの変更の名前は、タグ ヘルパーを更新しない場合、次のようなエラーが表示されます。

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

`Input`タグ ヘルパーの設定、HTML`type`属性ベースの .NET 型にします。 次の表には、いくつかの一般的な .NET 型と (すべての .NET 型が一覧に) 生成された HTML の種類が一覧表示します。

|.NET 型|入力の型|
|---|---|
|Bool|型"checkbox"を =|
|文字列型|型"text"を =|
|DateTime|型"datetime"を =|
|Byte|種類 ="number"|
|Int|種類 ="number"|
|Single、Double|種類 ="number"|


次の表に、いくつかの一般的な[データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)を (すべての検証属性が表示されている) 特定の入力の種類にマップする入力タグ ヘルパー属性。


|属性|入力の型|
|---|---|
|[EmailAddress]|型"email"を =|
|[Url]|型"url"を =|
|[HiddenInput]|種類 ="hidden"|
|[Phone]|型「電話」を =|
|[DataType(DataType.Password)]| 型"password"を =|
|[DataType(DataType.Date)]| 型"date"を =|
|[DataType(DataType.Time)]| 型「時間」を =|


例:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

上記のコードは、次の HTML を生成します。

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

データの注釈に適用される、`Email`と`Password`プロパティは、モデルのメタデータを生成します。 入力タグ ヘルパーは、モデルのメタデータを消費し、生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`属性 (を参照してください[モデルの検証](../models/validation.md))。 これらの属性では、入力フィールドにアタッチする検証コントロールについて説明します。 これにより、控えめな HTML5 および[jQuery](https://jquery.com/)検証します。 控えめな属性の形式である`data-val-rule="Error Message"`ここで、ルールは、検証規則の名前 (など`data-val-required`、 `data-val-email`、 `data-val-maxlength`, などです)。エラー メッセージは、属性で提供されるの値として表示されます、`data-val-rule`属性。 また、フォームの属性がある`data-val-ruleName-argumentName="argumentValue"`など、そのルールに関する追加情報を提供する`data-val-maxlength-max="1024"`です。

### <a name="html-helper-alternatives-to-input-tag-helper"></a>入力タグ ヘルパーの代替 HTML ヘルパー

`Html.TextBox`、 `Html.TextBoxFor`、`Html.Editor`と`Html.EditorFor`入力タグ ヘルパーの機能が重複しています。 入力タグ ヘルパーは自動的に設定されている、`type`属性です。`Html.TextBox`と`Html.TextBoxFor`は表示されません。 `Html.Editor``Html.EditorFor`コレクション、複合オブジェクト、テンプレートの処理、入力タグ ヘルパーはありません。 入力タグ ヘルパー`Html.EditorFor`と`Html.TextBoxFor`厳密に型指定された (、ラムダ式を使用)。`Html.TextBox`と`Html.Editor`されない (式の名前を使用)。

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`および`@Html.EditorFor()`特殊なを使用して`ViewDataDictionary`という名前のエントリ`htmlAttributes`既定のテンプレートを実行するときにします。 この動作は、必要に応じてを補う`additionalViewData`パラメーター。 キー"htmlAttributes"は区別されません。 キー"htmlAttributes"と同じように処理される、`htmlAttributes`入力のようなヘルパーにオブジェクトが渡される`@Html.TextBox()`です。

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>式の名前

`asp-for`属性値は、`ModelExpression`ラムダ式の右側にあるとします。 したがって、`asp-for="Property1"`なります`m => m.Property1`は生成されたコード内でプレフィックスする必要はありません`Model`です。 使用することができます、"@"文字の前に移動して、インライン式、 `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

次が生成されます。

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

コレクションのプロパティを持つ`asp-for="CollectionProperty[23].Member"`と同じ名前が生成されます`asp-for="CollectionProperty[i].Member"`とき`i`プロパティ値を持つ`23`します。

### <a name="navigating-child-properties"></a>子のプロパティを移動します。

ビュー モデルのプロパティのパスを使用して子プロパティに移動することもできます。 子を含む複雑なモデル クラスを考えます`Address`プロパティです。

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

バインド ビューでは、 `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

次の HTML が生成`Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
   ```

### <a name="expression-names-and-collections"></a>式の名前とコレクション

サンプルは、配列を含むモデル`Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

操作方法:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
   ```

次の Razor は、特定のアクセス方法を示しています。`Color`要素。

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

*Views/Shared/EditorTemplates/String.cshtml*テンプレート。

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

サンプルを使用して`List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

次の Razor は、コレクションに対する反復処理する方法を示しています。

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

*Views/Shared/EditorTemplates/ToDoItem.cshtml*テンプレート。

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>常に使用する`for`(および*いない* `foreach`) リストを反復処理します。 LINQ でインデクサーを評価する式は負荷のかかるし、最小限に抑える必要があります。

&nbsp;

>[!NOTE]
>上記のコメント付きのサンプル コードでラムダ式を交換する方法を示しています、`@`各にアクセスする演算子`ToDoItem`一覧にします。

## <a name="the-textarea-tag-helper"></a>Textarea タグ ヘルパー

`Textarea Tag Helper`タグ ヘルパーは、入力タグ ヘルパーに似ています。

* 生成、`id`と`name`属性、およびデータの検証属性をモデルから、 [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea)要素。

* 厳密な型指定を提供します。

* HTML ヘルパーの代替方法:`Html.TextAreaFor`

例:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

次の HTML が生成されます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6, 7, 8]}} -->

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>ラベル タグ ヘルパー

* ラベルのキャプションを生成し、`for`属性を[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)式の名前の要素

* 代わりに HTML ヘルパー:`Html.LabelFor`です。

`Label Tag Helper`純粋な HTML label 要素は次の利点を提供します。

* 自動的に値を取得する、わかりやすいラベルから、`Display`属性。 時間との組み合わせを意図した表示名を変更する可能性があります`Display`属性とタグ ヘルパーのラベルが適用されます、`Display`どこからでも使用されます。

* ソース コードの少ないマークアップ

* 厳密なモデル プロパティを使用して型指定します。

例:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

に対して次の HTML を生成、`<label>`要素。

```HTML
<label for="Email">Email Address</label>
   ```

生成されたラベル タグ ヘルパーの`for`"Email"は、id の属性値に関連付けられている、`<input>`要素。 タグ ヘルパーの一貫性のある生成`id`と`for`要素が正しく関連付けられている可能性があるようにします。 このサンプルではキャプションを起源、`Display`属性。 モデルが含まれていなかった場合、`Display`属性、キャプションは、式のプロパティの名前になります。

## <a name="the-validation-tag-helpers"></a>検証タグ ヘルパー

検証タグ ヘルパーの 2 つがあります。 `Validation Message Tag Helper` (するメッセージが表示されます、検証プロパティを 1 つのモデルに)、および`Validation Summary Tag Helper`(検証エラーの概要が表示されます)。 `Input Tag Helper`入力要素に基づいてデータ モデルのクラスに注釈属性に HTML5 クライアント側の検証属性を追加します。 検証は、サーバーも実行します。 検証タグ ヘルパーでは、検証エラーが発生したときに、これらのエラー メッセージが表示されます。

### <a name="the-validation-message-tag-helper"></a>検証メッセージ タグ ヘルパー

* 追加、 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`属性を[span](https://developer.mozilla.org/docs/Web/HTML/Element/span)要素は、指定したモデルのプロパティの入力フィールドの検証エラー メッセージをアタッチします。 クライアント側の検証エラーが発生したときに[jQuery](https://jquery.com/)のエラー メッセージを表示、`<span>`要素。

* 検証では、サーバー上の場所も受け取ります。 クライアント上で JavaScript が無効になる場合があり、いくつかの検証は、サーバー側でのみ実行できます。

* HTML ヘルパーの代替方法:`Html.ValidationMessageFor`

`Validation Message Tag Helper`と共に使用される、`asp-validation-for`された HTML 属性[span](https://developer.mozilla.org/docs/Web/HTML/Element/span)要素。

```HTML
<span asp-validation-for="Email"></span>
   ```

検証メッセージ タグ ヘルパーは、次の HTML を生成します。

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

一般的に使用する、`Validation Message Tag Helper`後、`Input`同じプロパティ タグ ヘルパー。 これには、エラーが発生した入力近く検証エラー メッセージが表示されます。

> [!NOTE]
> 正しい JavaScript では、ビューが必要と[jQuery](https://jquery.com/)スクリプト クライアント側の検証のために参照します。 参照してください[モデルの検証](../models/validation.md)詳細についてはします。

本文として MVC がエラー メッセージが表示を配置する場合、サーバー側で検証エラー (たとえばとカスタム サーバー側の検証がある場合またはクライアント側の検証が無効になっています)、`<span>`要素。

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>検証概要タグ ヘルパー

* ターゲット`<div>`要素を`asp-validation-summary`属性

* HTML ヘルパーの代替方法:`@Html.ValidationSummary`

`Validation Summary Tag Helper`検証メッセージの概要を表示するために使用します。 `asp-validation-summary`属性値は、次のいずれかを指定できます。

|asp 検証サマリー|検証メッセージが表示されます。|
|--- |--- |
|ValidationSummary.All|プロパティとモデルのレベル|
|ValidationSummary.ModelOnly|モデル|
|ValidationSummary.None|なし|

### <a name="sample"></a>サンプル

次の例では、データ モデルで装飾されて`DataAnnotation`で検証エラー メッセージを生成する属性、`<input>`要素。  検証エラーが発生したときに検証タグ ヘルパーには、エラー メッセージが表示されます。

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

生成された HTML (モデルが有効な場合):

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 8, 9, 12, 13]}} -->

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>Select タグ ヘルパー

* 生成されます[選択](https://www.w3.org/wiki/HTML/Elements/select)と関連付けられた[オプション](https://www.w3.org/wiki/HTML/Elements/option)モデルのプロパティの要素。

* HTML ヘルパーの代替手段を持つ`Html.DropDownListFor`と`Html.ListBoxFor`

`Select Tag Helper` `asp-for` 、モデルのプロパティ名を指定します、[選択](https://www.w3.org/wiki/HTML/Elements/select)要素および`asp-items`を指定します、[オプション](https://www.w3.org/wiki/HTML/Elements/option)要素。  例:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

例:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

`Index`メソッドの初期化、 `CountryViewModel`、選択した国を設定およびに渡します、`Index`ビュー。

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST`Index`メソッドは、選択範囲を表示します。

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

`Index`ビュー。

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

これには、("CA"選択) で、次の HTML が生成されます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6]}} -->

```HTML
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

> [!NOTE]
> 使用しないで`ViewBag`または`ViewData`選択タグ ヘルパーとします。 ビュー モデルは、MVC のメタデータを提供することをより堅牢になり、通常それほど大きな問題です。

`asp-for`属性値は、特殊なケースを必要し、しない、`Model`プレフィックス、その他のタグ ヘルパー属性は (など`asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>列挙型のバインディング

使用すると便利なことがよくあります`<select>`で、`enum`プロパティを生成し、`SelectListItem`からの要素、`enum`値。

例:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

`GetEnumSelectList`メソッドを生成、`SelectList`列挙型のオブジェクト。

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

装飾できるは、列挙子リストが、`Display`豊富な UI を取得する属性。

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

次の HTML が生成されます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [4, 5]}} -->

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

### <a name="option-group"></a>オプション グループ

HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)ビュー モデルには、1 つまたは複数が含まれている場合、要素が生成される`SelectListGroup`オブジェクト。

`CountryViewModelGroup`グループ、 `SelectListItem` "North America"および"Europe"グループに要素。

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

2 つのグループを次に示します。

![オプション グループの例](working-with-forms/_static/grp.png)

生成される HTML。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]}} -->

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>複数の選択

タグの選択ヘルパーが自動的に生成、[複数 =「複数」](http://w3c.github.io/html-reference/select.html)属性でプロパティが指定された場合、`asp-for`属性は、`IEnumerable`です。 たとえば、次のようなモデルを指定します。

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

次のビュー。

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

次の HTML を生成します。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3]}} -->

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>選択されていません

複数のページで、「指定されていません」のオプションを使用して自分宛てを検索する場合は、HTML の繰り返しを排除するためのテンプレートを作成できます。

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

*Views/Shared/EditorTemplates/CountryViewModel.cshtml*テンプレート。

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

HTML を追加する[\<オプション >](https://www.w3.org/wiki/HTML/Elements/option)要素に限定されません、*選択されていない*ケースです。 たとえば、次のビューとアクション メソッド上のコードに似た HTML が生成されます。

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

正しい`<option>`要素が選択されます (含まれて、`selected="selected"`属性) によっては、現在`Country`値。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [5]}} -->

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>その他のリソース

* [タグ ヘルパー](tag-helpers/intro.md)

* [HTML フォーム要素](https://www.w3.org/TR/html401/interact/forms.html)

* [検証トークンの要求](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [モデル バインド](../models/model-binding.md)

* [モデルの検証](../models/validation.md)

* [データ注釈](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [このドキュメントのスニペットをコード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)です。
