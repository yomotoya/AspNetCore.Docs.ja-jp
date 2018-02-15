---
title: "ASP.NET Core MVC でのモデルの検証"
author: rachelappel
description: "ASP.NET Core MVC でのモデルの検証について説明します。"
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: dfb24a4c72b15737295b7aea406be24160fc6674
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC でのモデルの検証の概要

作成者: [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>モデルの検証の概要

アプリでは、データベースにデータを格納する前に、データを検証する必要があります。 データについて、セキュリティ脅威の可能性をチェックし、種類とサイズが適切に書式設定されていることを検証し、ルールに準拠していることを確認する必要があります。 検証は必要なことですが、実装するのは冗長で面倒な場合があります。 MVC では、検証はクライアントとサーバーの両方で発生します。

さいわい、.NET では検証が検証属性に抽象化されています。 これらの属性には検証コードが含まれているため、開発者が記述しなければならないコードの量は少なくて済みます。

[GitHub のサンプルを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。

## <a name="validation-attributes"></a>検証属性

検証属性はモデルの検証を構成する方法であり、概念的にはデータベース テーブルでのフィールドの検証に似ています。 これには、データ型の割り当てや必須フィールドなどの制約が含まれます。 その他の種類の検証としては、クレジット カード、電話番号、メール アドレスなど、ビジネス ルールを強制するためのデータへのパターンの適用があります。 検証属性を使うと、これらの要件の適用がはるかに単純で簡単になります。

次に示すのは、映画やテレビ番組に関する情報を格納するアプリの注釈付き `Movie` モデルです。 ほとんどのプロパティは必須であり、一部の文字列プロパティには長さの要件があります。 さらに、`Price` プロパティには 0 から $999.99 までの数値範囲制限が、カスタム検証属性と共に適用されています。

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

モデルに目を通すだけでこのアプリのデータについてのルールがわかり、コードの保守が簡単になります。 いくつかの一般的な組み込み検証属性を次に示します。

* `[CreditCard]`: プロパティがクレジット カード形式であることを検証します。

* `[Compare]`: モデルの 2 つのプロパティが一致することを検証します。

* `[EmailAddress]`: プロパティが電子メール形式であることを検証します。

* `[Phone]`: プロパティが電話番号形式であることを検証します。

* `[Range]`: プロパティの値が特定の範囲内であることを検証します。

* `[RegularExpression]`: データが指定した正規表現と一致することを検証します。

* `[Required]`: プロパティを必須にします。

* `[StringLength]`: 文字列プロパティが指定した最大長以下であることを検証します。

* `[Url]`: プロパティが URL 形式であることを検証します。

MVC は、検証のために `ValidationAttribute` から派生した任意の属性をサポートします。 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) 名前空間には多くの便利な検証属性が含まれています。

組み込みの属性で提供されるものより多くの機能が必要になる場合があります。 そのようなときは、`ValidationAttribute` から派生するか、モデルを変更して `IValidatableObject` を実装することにより、カスタム検証属性を作成できます。

## <a name="notes-on-the-use-of-the-required-attribute"></a>必須属性の使用に関する注意事項

null 非許容の[値の型](/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須ではなく、`Required` 属性を必要としません。 アプリは、`Required` とマークされている null 非許容型に対して、サーバー側の検証チェックを実行しません。

検証および検証属性では考慮されていない MVC モデル バインドは、null 非許容型に対して欠落値または空白文字を含むフォーム フィールドの送信を却下します。 ターゲット プロパティに `BindRequired` 属性がない場合、モデル バインドは null 非許容型の欠落値を無視し、そのフォーム フィールドは受信フォーム データからなくなります。

[BindRequired 属性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (「[属性を使用してモデル バインドの動作をカスタマイズする](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)」も参照) は、フォーム データが完全であることを保証するのに便利です。 プロパティに適用すると、モデル バインド システムはそのプロパティの値を必須にします。 型に適用すると、モデル バインド システムはその型のすべてのプロパティに対して値を必須にします。

[Nullable\<T> 型](/dotnet/csharp/programming-guide/nullable-types/) (例: `decimal?`、`System.Nullable<decimal>`) を使い、それを `Required` とマークすると、プロパティが標準の null 許容型 (例: `string`) の場合と同様に、サーバー側の検証チェックが実行されます。

クライアント側検証では、`Required` とマークされているモデル プロパティに対応するフォーム フィールド、および `Required` とマークされていない null 非許容型のプロパティに対しては、値が必要です。 `Required` を使って、クライアント側検証のエラー メッセージを制御できます。

## <a name="model-state"></a>モデルの状態

モデルの状態は、送信された HTML フォーム値での検証エラーを表します。

MVC は、エラー数の最大数 (既定値は 200) に達するまで、フィールドの検証を続けます。 *Startup.cs* ファイルの `ConfigureServices` メソッドに次のコードを挿入することで、この値を構成できます。

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>モデル状態エラーの処理

モデル検証は各コントローラー アクションが呼び出される前に行われます。`ModelState.IsValid` を検査し、適切に対処するのはアクション メソッドの仕事です。 多くの場合において適切な対応は、エラー応答を返すことであり、モデルの検証が失敗した理由を詳しく示すのが理想的です。

一部のアプリでは、モデル検証エラーを処理するとき、標準的な規則に従います。その場合、そのようなポリシーを実装する場所としてフィルターが適していることがあります。 有効および無効なモデル状態でのアクションの動作をテストする必要があります。

## <a name="manual-validation"></a>手動検証

モデル バインドと検証が完了した後、その一部を繰り返すことが必要になる場合があります。 たとえば、整数が想定されているフィールドにユーザーがテキストを入力したかもしれない場合や、モデルのプロパティの値を計算する必要がある場合などです。

検証の手動実行が必要になることがあります。 その場合は、次に示すように `TryValidateModel` メソッドを呼び出します。

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>カスタム検証

検証属性の機能は、ほとんどの検証ニーズに対応します。 ただし、ビジネスに固有の検証規則もあります。 規則が、必須フィールドや値範囲の確認などの一般的なデータ検証方法ではない場合があります。 そのような場合は、カスタム検証属性が優れた解決策です。 MVC では独自のカスタム検証属性を簡単に作成できます。 `ValidationAttribute` から継承し、`IsValid` メソッドをオーバーライドするだけです。 `IsValid` メソッドは 2 つのパラメーターを受け取ります。1 番目は *value* という名前のオブジェクトで、2 番目は *validationContext* という名前の `ValidationContext` オブジェクトです。 *Value* は、カスタム検証メソッドで検証するフィールドの実際の値を示します。

次の例のビジネス規則では、ユーザーは 1960 年より後にリリースされた映画のジャンルを *Classic* に設定できないことになっています。 `[ClassicMovie]` 属性は、最初にジャンルをチェックし、それが Classic である場合は、リリース日をチェックして 1960 年以前であることを確認します。 1960 年より後にリリースされている場合、検証は失敗します。 この属性は、データの検証に使うことができる年を表す整数パラメーターを受け取ります。 次のように、属性のコンストラクターでパラメーターの値をキャプチャすることができます。

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

上の `movie` 変数は、検証のためのフォーム送信からのデータを格納している `Movie` オブジェクトを表します。 この例では、検証コードは規則に従って `ClassicMovieAttribute` クラスの `IsValid` メソッドで日付とジャンルをチェックします。 検証が成功すると `IsValid` は `ValidationResult.Success` コードを返し、検証が失敗したときは `ValidationResult` とエラー メッセージを返します。 ユーザーが `Genre` フィールドを変更してフォームを送信すると、`ClassicMovieAttribute` の `IsValid` メソッドは映画がクラシックかどうかを検証します。 他の組み込み属性と同じように、検証が行われるようにするには、`ClassicMovieAttribute` を `ReleaseDate` などのプロパティに適用します (前のコード例を参照)。 この例は `Movie` 型でのみ機能するので、次の段落で示すように、`IValidatableObject` を使うのがさらによい方法です。

または、`IValidatableObject` インターフェイスに `Validate` メソッドを実装することで、これと同じコードをモデルに配置することもできます。 カスタム検証属性は個別のプロパティを検証する場合に使えるのに対し、`IValidatableObject` の実装は、ここで示すようにクラス レベルの検証を実装するために使うことができます。

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>クライアント側の検証

クライアント側の検証は、ユーザーにとって非常に便利です。 他の方法ではサーバーへのラウンド トリップを待つために必要な時間を節約できます。 ビジネス的に表現すると、1 回だけ見ればわずかな時間でも、それが毎日何百回も積み重なると、膨大な時間、費用、フラストレーションになります。 簡単かつ迅速な検証は、ユーザーの作業をいっそう効率的にし、入力と出力の品質が向上します。

クライアント側検証が動作するには、ここで示すように、適切な JavaScript スクリプト参照をビューに設定する必要があります。

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) スクリプトは、人気のある [jQuery Validate](https://jqueryvalidation.org/) プラグインを基に作成された Microsoft のカスタム フロントエンド ライブラリです。 jQuery Unobtrusive Validation を使わないと、同じ検証ロジックを 2 か所でコーディングする必要があります。1 つはモデル プロパティでのサーバー側検証属性で、もう 1 つはクライアント側スクリプトです (jQuery Validate の [`validate()`](https://jqueryvalidation.org/validate/) メソッドの例を見ると、これがどれほど複雑になるかがわかります)。 代わりに、MVC の[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)および [HTML ヘルパー](xref:mvc/views/overview)では、モデル プロパティの検証属性と型メタデータを使って、検証の必要なフォーム要素に HTML 5 の [data- 属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)をレンダリングできます。 MVC は、組み込み属性とカスタム属性の両方に対して、`data-` 属性を生成します。 その後、jQuery Unobtrusive Validation は `data-` 属性を解析し、ロジックを jQuery Validate に渡して、実質的にサーバー側検証ロジックをクライアントに "コピー" します。 次に示すように、関連するタグ ヘルパーを使って、クライアントで検証エラーを表示できます。

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上記のタグ ヘルパーは、以下の HTML をレンダリングします。 HTML 出力の `data-` 属性が、`ReleaseDate` プロパティの検証属性に対応していることに注意してください。 下の `data-val-required` 属性には、ユーザーがリリース日フィールドを入力していない場合に表示するエラー メッセージが含まれています。 jQuery Unobtrusive Validation はこの値を jQuery Validate の [`required()`](https://jqueryvalidation.org/required-method/) メソッドに渡し、このメソッドは付随する **\<span>** 要素にそのメッセージを表示します。

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

クライアント側検証は、フォームが有効になるまで送信を許可しません。 送信ボタンをクリックすると、フォームの送信またはエラー メッセージの表示を行う JavaScript が実行されます。

MVC は、プロパティの .NET データ型に基づいて型属性の値を特定し、場合によっては `[DataType]` 属性を使って上書きします。 基本 `[DataType]` 属性では、実際のサーバー側検証は行われません。 ブラウザーは独自のエラー メッセージを選んで自由にエラーを表示しますが、jQuery Validation Unobtrusive パッケージはメッセージを上書きし、他と整合性のあるメッセージを表示できます。 これが最もよくわかるのは、ユーザーが `[EmailAddress]` などの `[DataType]` サブクラスを適用した場合です。

### <a name="add-validation-to-dynamic-forms"></a>動的なフォームに検証を追加する

jQuery Unobtrusive Validation はページが初めて読み込まれるときに検証ロジックとパラメーターを jQuery Validate に渡すので、動的に生成されるフォームでは、検証は自動的には行われません。 代わりに、作成直後に動的フォームを解析するよう、jQuery Unobtrusive Validation に指示する必要があります。 たとえば、次のコードは、AJAX によって追加されるフォームでクライアント側検証を設定する方法を示しています。

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

`$.validator.unobtrusive.parse()` メソッドは、1 つの引数で jQuery セレクターを受け取ります。 このメソッドは、そのセレクター内のフォームの `data-` 属性を解析するよう jQuery Unobtrusive Validation に指示します。 その後、フォームで必要なクライアント側検証規則が適用されるように、これらの属性の値は jQuery Validate プラグインに渡されます。

### <a name="add-validation-to-dynamic-controls"></a>動的なコントロールに検証を追加する

`<input/>` や `<select/>` などの個別のコントロールが動的に生成されるときに、フォームの検証規則を更新することもできます。 格納しているフォームが既に解析されていて更新されないため、これらの要素のセレクターを `parse()` メソッドに直接渡すことはできません。 代わりに、まず既存の検証データを削除した後、次に示すようにフォーム全体を再解析します。

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

カスタム属性のクライアント側ロジックを作成することができ、[Unobtrusive Validation](http://jqueryvalidation.org/documentation/) はそれを検証の一部としてクライアントで自動的に実行します。 最初に、次に示すように `IClientModelValidator` インターフェイスを実装することで、追加される data- 属性を制御します。

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

このインターフェイスを実装する属性は、生成されるフィールドに HTML 属性を追加できます。 `ReleaseDate` 要素の出力を調べると、前の例と似た HTML であることがわかりますが、ここでは、`IClientModelValidator` の `AddValidation` メソッドで定義された `data-val-classicmovie` 属性があります。

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

Unobtrusive Validation は、`data-` 属性のデータを使ってエラー メッセージを表示します。 ただし、jQuery には、ユーザーが jQuery の `validator` オブジェクトを追加するまで、規則やメッセージのことはわかりません。 下の例では、これは、カスタム クライアント検証コードを含む `classicmovie` という名前のメソッドを jQuery の `validator` オブジェクトに追加することで示されています。

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

これで、jQuery には、カスタム JavaScript 検証を実行するための情報、およびその検証コードが false を返した場合に表示するエラー メッセージが存在するようになります。

## <a name="remote-validation"></a>リモート検証

リモート検証は、クライアント上のデータをサーバー上のデータに対して検証する必要があるときに使う優れた機能です。 たとえば、アプリでメール アドレスやユーザー名が既に使われているかどうかを検証する必要があり、そのために大量のデータをクエリする必要があるような場合です。 1 つまたは少数のフィールドを検証するために大きなデータ セットをダウンロードするのでは、消費するリソースが多すぎます。 機密情報が漏えいする可能性もあります。 代わりの方法として、ラウンド トリップ要求を行って、フィールドを検証します。

リモート検証は、2 ステップのプロセスで実装できます。 最初に、`[Remote]` 属性でモデルに注釈を付ける必要があります。 `[Remote]` 属性は複数のオーバーロードを受け付けるので、それを使って、クライアント側の JavaScript が適切なコードを呼び出すようにします。 次の例では、`Users` コントローラーの `VerifyEmail` アクション メソッドを指し示しています。

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

2 番目のステップでは、`[Remote]` 属性で定義されている対応するアクション メソッドに、検証コードを配置します。 jQuery Validate の [`remote()`](https://jqueryvalidation.org/remote-method/) メソッドのドキュメントには次のように記載されています。

> サーバー側の応答は、JSON 文字列でなければなりません。そして、有効な要素の場合は `"true"` でなければならず、無効な要素の場合は `"false"`、`undefined`、または `null` と既定のエラー メッセージを使うことができます。 サーバー側の応答が文字列の場合 (例:  `"That name is already taken, try peter123 instead"`)、この文字列が既定値の代わりにカスタム エラー メッセージとして表示されます。

次に示すように、`VerifyEmail()` メソッドの定義はこれらの規則に従います。 このメソッドは、メール アドレスが使われている場合は検証エラー メッセージを返し、メール アドレスが空いている場合は `true` を返します。また、結果を `JsonResult` オブジェクトにラップします。 クライアント側では、返された値を使って、続行するか、必要な場合はエラーを表示できます。

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

ユーザーがメール アドレスを入力すると、ビューの JavaScript はリモート呼び出しを行って、そのメール アドレスが使われているかどうかを確認し、使われている場合はエラー メッセージを表示します。 使われていない場合は、ユーザーは普通にフォームを送信できます。

`[Remote]` 属性の `AdditionalFields` プロパティは、フィールドの組み合わせをサーバー上のデータに対して検証するときに役立ちます。 たとえば、上の `User` モデルに `FirstName` および `LastName` という名前の 2 つの追加プロパティがある場合、その名前のペアを使う既存ユーザーがいないことを確認できます。 次のコードで示すように、新しいプロパティを定義します。

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields` を文字列 `"FirstName"` および `"LastName"` に明示的に設定することもできますが、[`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) 演算子をこのように使うと、後のリファクタリングが容易になります。 検証を実行するアクション メソッドでは、`FirstName` の値と `LastName` の値の 2 つの引数を受け付ける必要があります。

[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

ここで、ユーザーが姓と名を入力すると、JavaScript は以下のことを行います。

* リモート呼び出しを行って、その名前のペアが使われているかどうかを確認します。
* ペアが使われている場合は、エラー メッセージが表示されます。 
* 使われていない場合は、ユーザーはフォームを送信できます。

複数の追加フィールドを `[Remote]` 属性で検証する必要がある場合は、コンマ区切りのリストとして提供します。 たとえば、`MiddleName` プロパティをモデルに追加するには、`[Remote]` 属性を次のコードのように設定します。

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

他の属性引数と同じように、`AdditionalFields` も定数式である必要があります。 したがって、[補間文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings)を使ったり、[`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) を呼び出して `AdditionalFields` を初期化したりすることはできません。 `[Remote]` 属性に新しいフィールドを追加するごとに、対応するコントローラー アクション メソッドに別の引数を追加する必要があります。
