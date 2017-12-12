---
title: "ASP.NET Core MVC でのモデルの検証"
author: rachelappel
description: "ASP.NET Core MVC でのモデルの検証について説明します。"
keywords: "ASP.NET Core、MVC での検証"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a3f3f7010d7744d59ce2dd88b323418423b3ae08
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>ASP.NET Core MVC でのモデル検証の概要

によって[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>モデルの検証の概要

アプリは、データベースのデータを保存する前に、アプリは、データを検証する必要があります。 潜在的なセキュリティ上の脅威は、種類とサイズで適切に書式設定し、ルールに従ってくださいことを確認のためには、データをチェックする必要があります。 冗長と実装が煩雑でできますが、検証する必要があります。 MVC では、検証は、クライアントとサーバーの両方で発生します。

さいわい、.NET には、検証属性に検証が抽象化します。 これらの属性には、検証コード記述するコードの量を減らすにはが含まれています。

[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)です。

## <a name="validation-attributes"></a>検証属性

検証属性は、データベース テーブル内のフィールドの検証に概念的に似ていますので、モデルの検証を構成する方法です。 これには、データ型または必要なフィールドの割り当てなどの制約が含まれます。 その他の種類検証にはでは、電子メール アドレスまたは電話番号、クレジット カードなど、ビジネス ルールを適用するデータにパターンを適用することが含まれます。 検証属性は、大幅に簡素化され、使いやすく、これらの要件を適用することを確認します。

以下は、注釈付き`Movie`ムービーやテレビ番組に関する情報を格納するアプリケーションからモデル。 ほとんどのプロパティが必要ないくつかの文字列プロパティの長さの要件があります。 さらのために数値の範囲の制限がある、`Price`プロパティを 0 から $999.99、カスタム検証属性と共ににします。

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

モデル全体を読むだけで、コードを保守しやすいように、このアプリのデータについてのルールが表示されます。 いくつかの一般的な組み込みの検証属性を次に示します。

* `[CreditCard]`: 検証プロパティは、クレジット カード形式です。

* `[Compare]`: モデルの検索一致で 2 つのプロパティを検証します。

* `[EmailAddress]`: 検証電子メール形式を、プロパティがあります。

* `[Phone]`: 検証プロパティは、電話形式です。

* `[Range]`: 特定の範囲内でプロパティの値が検証されます。

* `[RegularExpression]`: データが指定された正規表現と一致することを検証します。

* `[Required]`: 必要なプロパティを使用します。

* `[StringLength]`: 文字列プロパティに指定された最大長でが多くてことを検証します。

* `[Url]`: を検証、プロパティには、URL の形式です。

MVC から派生した任意の属性をサポートしている`ValidationAttribute`検証のためです。 多くの便利な検証属性は含まれて、 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)名前空間。

組み込みの属性が用意されてより多くの機能が必要になる可能性があります。 派生することによってカスタム検証属性を作成する場合、それらの時間`ValidationAttribute`を実装する、モデルの変更または`IValidatableObject`です。

## <a name="notes-on-the-use-of-the-required-attribute"></a>必須の属性の使用に関する注意事項

null 非許容の[値の型](/dotnet/csharp/language-reference/keywords/value-types) (`decimal`、`int`、`float`、`DateTime` など) は本質的に必須ではなく、`Required` 属性を必要としません。 アプリ マークされている null 非許容の型のサーバー側の検証チェックを実行しない`Required`です。

MVC モデル バインディングは、検証と検証の属性を持つ関係はありませんは、不足値または null 非許容の型の空白文字を含むフォーム フィールドの送信を却下します。 ない場合、`BindRequired`属性モデル バインディング ターゲット プロパティに null 非許容の型の場合、フォーム フィールドが存在しないデータの欠落が無視受信フォーム データからです。

[BindRequired 属性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute)(も参照してください[属性を持つモデル バインディング動作をカスタマイズする](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) フォームのデータが完全なことを確認すると便利です。 プロパティに適用する場合、モデル バインド システムには、そのプロパティの値が必要です。 型に適用する場合、モデル バインド システムのすべての型のプロパティの値が必要です。

使用すると、 [Nullable\<T > 型](/dotnet/csharp/programming-guide/nullable-types/)(たとえば、`decimal?`または`System.Nullable<decimal>`) とマークして`Required`プロパティが、標準の null 許容型 (の場合と同様に、サーバー側の検証チェックが実行されます。例では、 `string`)。

クライアント側の検証がマークされているモデル プロパティに対応するフォーム フィールドの値が必要です`Required`とマークしていない型の null 非許容のプロパティの`Required`します。 `Required`クライアント側の検証エラー メッセージを制御するために使用します。

## <a name="model-state"></a>モデルの状態

モデルの状態では、送信された HTML フォームの値で検証エラーを表します。

MVC は引き続きに達するまでフィールドの検証エラー (既定で 200) の最大数。 この数を構成するには次のコードを挿入することで、`ConfigureServices`メソッドで、 *Startup.cs*ファイル。

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>処理のモデル状態エラー

モデルの検証が呼び出される、各コント ローラー アクションの前に発生しを検査するアクション メソッドの責任である`ModelState.IsValid`して適切に対処します。 多くの場合は、適切な反応をなんらかの種類のモデルの検証が失敗した理由の詳細を示す理想的には、エラー応答を返します。

ケース フィルターがこのようなポリシーを実装する適切な場所をする可能性があります、モデルの検証エラーを処理するための標準的な規則に準拠するアプリの一部を選択します。 有効および無効なモデルの状態を持つユーザーの操作の動作をテストする必要があります。

## <a name="manual-validation"></a>手動検証

モデル バインディングと検証の完了後にその一部を繰り返すこともできます。 たとえば、ユーザーが正しくありませんテキスト、整数を指定してください フィールドにまたはモデルのプロパティの値を計算する必要があります。

検証を手動で実行する必要があります。 これを行うには、呼び出し、`TryValidateModel`メソッドを次のように。

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>カスタム検証

検証属性の機能のほとんどの検証要件に対応します。 ただし、いくつかの検証規則とは違うだけのフィールドを確保するなどの汎用的なデータ検証が必要なまたは値の範囲に準拠するいると、ビジネスに固有です。 これらのシナリオでは、カスタム検証属性は、優れたソリューションです。 MVC での独自のカスタム検証属性の作成は簡単です。 継承した、 `ValidationAttribute`、オーバーライド、`IsValid`メソッドです。 `IsValid`メソッドは、次の 2 つのパラメーターを受け取り、1 つは、という名前のオブジェクトを*値*、2 番目の`ValidationContext`という名前のオブジェクト*validationContext*です。 *値*カスタム検証を検証するフィールドの実際の値を参照します。

次の例で、ビジネス ルールで制限ユーザーが設定されていないこと、ジャンル*クラシック*1960年以降後にリリースされたムービーにします。 `[ClassicMovie]`属性はまず、ジャンルをチェックし、従来型である場合、チェックをリリース日を 1960年よりも後であることを確認します。 これは、1960年後にリリースされた、検証が失敗します。 この属性は、データの検証に使用できる暦を表す整数パラメーターを受け取ります。 次のように、属性のコンス トラクターのパラメーターの値をキャプチャすることができます。

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

`movie`上を表す変数、`Movie`を検証するフォームの送信からのデータを格納しているオブジェクト。 検証コードが日とでジャンルをチェックするこの例では、`IsValid`のメソッド、`ClassicMovieAttribute`規則に従ってクラスです。 検証成功時に`IsValid`を返します、`ValidationResult.Success`コード、および検証に失敗したときに、`ValidationResult`エラー メッセージを使用します。 ユーザーが変更すると、`Genre`フィールドに、フォームを送信して、`IsValid`のメソッド、`ClassicMovieAttribute`ムービーが、従来型であるかどうかを確認してください。 すべての組み込みの属性と同様に適用、`ClassicMovieAttribute`などのプロパティに`ReleaseDate`前のコード例で示すように、検証が発生したことを確認します。 のみ行われますので`Movie`を使用する型の場合よりよい方法は`IValidatableObject`次の段落に示すようにします。

また、この同じコードでしたに配置するモデルを実装して、`Validate`メソッドを`IValidatableObject`インターフェイスです。 実装するカスタム検証属性は、個々 のプロパティを検証するため、動作、`IValidatableObject`を次に示すようにクラス レベルの検証を実装するために使用できます。

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>クライアント側の検証

クライアント側の検証は、ユーザーにとって非常に便利です。 保存されますがそれ以外の場合に費やす時間のラウンド トリップを待機しているサーバー。 ビジネス用語として、何百もの時間を乗算した値の秒の小数部をもいくつか追加最大多数の時間、費用やフラストレーションは解消します。 簡単かつ迅速検証より効率的に作業しより優れた品質の入力し、出力を生成することができます。

次のように動作するクライアント側の検証のために、適切な JavaScript のスクリプト参照では、ビューが必要です。

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

[JQuery 控えめな検証](https://github.com/aspnet/jquery-validation-unobtrusive)スクリプトは、人気の高い上に構築されるカスタムの Microsoft フロント エンド ライブラリ[jQuery 検証](https://jqueryvalidation.org/)プラグインします。 2 つの場所に同じ検証ロジックをコードが jQuery 控えめな検証なし: サーバー側の検証属性、モデルのプロパティの中に 2 回、クライアント側スクリプトでもう (jQuery 検証のための例は、 [ `validate()`](https://jqueryvalidation.org/validate/)メソッドではどの程度複雑なこのなる可能性があります)。 代わりに、MVC の[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と[HTML ヘルパー](xref:mvc/views/overview)検証属性を使用して HTML 5 を表示するためにモデルのプロパティのメタデータを入力できるようには[データ属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)で検証が必要な形式の要素。 MVC の生成、`data-`組み込みとカスタムの両方の属性の属性です。 jQuery 控えめな検証がアプリケーションを解析し、`data-`属性し、jQuery、変数をクライアントにサーバー側の検証ロジックを効率的に「コピー」を検証するロジックを渡します。 次に示すように、関連するタグ ヘルパーを使用して、クライアントで検証エラーを表示できます。

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上記のタグ ヘルパーは、以下の HTML を表示します。 注意して、 `data-` html 属性の出力の検証属性に対応している、`ReleaseDate`プロパティです。 `data-val-required`下の属性には、リリース日 フィールドにユーザーを満たさない場合に表示するエラー メッセージが含まれています。 jQuery 検証の控えめな jQuery 検証をこの値を渡します[ `required()` ](https://jqueryvalidation.org/required-method/)付随するでそのメッセージを表示するメソッド **\<span >**要素。

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

したがって、フォームが有効になるまで、クライアント側の検証は、送信しないようにします。 [送信] ボタンは、フォームを送信するか、エラー メッセージを表示する JavaScript を実行します。

MVC は、.NET データ型に基づく可能性のあるオーバーライドを使用して、プロパティの型の属性値を決定`[DataType]`属性。 基本`[DataType]`属性では、実際のサーバー側の検証は行われません。 ブラウザーでは、独自のエラー メッセージを選択し、jQuery 検証控えめなパッケージが、メッセージを上書きし、それらを他のユーザーと一貫して表示ただし、希望するが、それらのエラーを表示します。 ユーザーを適用すると最も明らかにこのような`[DataType]`などサブクラス`[EmailAddress]`です。

### <a name="adding-validation-to-dynamic-forms"></a>検証は、動的なフォームを追加します。

JQuery 控えめな検証に渡すための検証ロジックとパラメーター jQuery 検証ページが初めて読み込まれるときに、動的に生成されたフォームの検証が自動的に発生しません。 代わりに、jQuery 作成直後に動的形式の解析を控えめな検証を指定する必要があります。 たとえば、次のコードは、AJAX を使用して追加、フォーム上のクライアント側の検証を設定する方法を示しています。

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

`$.validator.unobtrusive.parse()`メソッドは 1 つの引数には、jQuery セレクターを受け取ります。 このメソッドは、jQuery の解析を控えめな検証を指示、`data-`そのセレクター内のフォームの属性です。 これらの属性の値は、フォーム、目的のクライアント側の検証規則の欠落を表すように jQuery 検証プラグインに渡されます。

### <a name="adding-validation-to-dynamic-controls"></a>検証は、ダイナミック コントロールを追加します。

などの個人コントロールときに、フォームに検証規則を更新することも`<input/>`s および`<select/>`s、動的に生成されます。 これらの要素のセレクターを渡すことはできません、`parse()`メソッドを直接周囲のフォームが既に解析されているし、更新されないためです。  代わりに、まず、既存の検証データを削除して次に示すように、フォーム全体は再解析。

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

カスタム属性では、クライアント側のロジックを作成することがありますと[控えめな検証](http://jqueryvalidation.org/documentation/)に対するクライアントの検証の一部として自動的に実行されます。 どのようなデータの属性を実装することによって追加を制御するには、まず、`IClientModelValidator`インターフェイスの次のようにします。

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

このインターフェイスを実装する属性は、生成されたフィールドに HTML 属性を追加できます。 出力を調べて、`ReleaseDate`要素は、ここでは、前の例では、次のような HTML を表示、`data-val-classicmovie`属性で定義されている、`AddValidation`メソッドの`IClientModelValidator`です。

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

控えめな検証でデータを使用して、`data-`エラー メッセージを表示する属性。 ただし、jQuery のルールが把握して、jQuery を追加するまでメッセージ`validator`オブジェクト。 これは、という名前のメソッドを追加する次の例に示されて`classicmovie`jQuery にカスタムのクライアント検証コードを含む`validator`オブジェクト。

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery なりました検証コードが false を返す場合に表示するエラー メッセージと同様に、カスタムの JavaScript の検証を実行するには、情報。

## <a name="remote-validation"></a>リモート検証

リモート検証は、サーバー上のデータを使用してクライアント上のデータを検証する必要があるときに使用する優れた機能です。 たとえば、アプリは、電子メールまたはユーザー名は既に使用中でと、そのためにはデータの大量のクエリを実行する必要があるかどうかを確認する必要があります。 ダウンロード大きなデータ セットのいずれかを検証するためや、いくつかのフィールドの多くのリソースを消費します。 機密情報を公開する場合もします。 代わりにラウンドト リップを要求すると、フィールドの検証を開始します。

2 つの手順では、リモート検証を実装できます。 最初を使用したモデルを付ける必要があります、`[Remote]`属性。 `[Remote]`属性は複数のオーバー ロードを呼び出して、適切なコードをクライアント側 JavaScript を直接行うこともできますを受け取ります。 次の例が指す、`VerifyEmail`のアクション メソッド、`Users`コント ローラー。

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

定義されている、2 番目の手順が、対応するアクション メソッドに検証コードを配置する、`[Remote]`属性。 JQuery 検証に従って[ `remote()` ](https://jqueryvalidation.org/remote-method/)メソッドのドキュメント。

> Serverside 応答する必要がある JSON 文字列である必要があります`"true"`の有効な要素は、可能であり`"false"`、 `undefined`、または`null`無効な要素は、既定のエラー メッセージを使用します。 文字列の場合、serverside 応答、リフレッシュ レート。 `"That name is already taken, try peter123 instead"`、この文字列は、既定値の代わりにカスタム エラー メッセージとして表示されます。

定義、`VerifyEmail()`に従ってこれらの規則では、次のようにします。 検証エラーを返すメッセージの電子メールが行われた場合、または`true`かどうか、電子メールには、無料で結果をラップ、`JsonResult`オブジェクト。 クライアント側では、続行するか、必要な場合は、エラーを表示、返される値を使用できます。

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

ユーザーは、電子メール アドレスを入力、ときに、ビューでの JavaScript がについての電子メールが取得されている場合は、エラー メッセージが表示されますへのリモート呼び出しできます。 それ以外の場合、ユーザーは通常どおり、フォームを送信できます。

`AdditionalFields`のプロパティ、`[Remote]`属性は、サーバー上のデータに対するフィールドの組み合わせを検証するために役立ちます。  たとえば場合、`User`上記のモデルが 2 つの追加のプロパティと呼ばれる`FirstName`と`LastName`、ないことを確認の既存のユーザー既に名のペアリングすることができます。  次のコードに示すように、新しいプロパティを定義します。

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`でしたに設定されている明示的に文字列`"FirstName"`と`"LastName"`を使用して、 [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof)リファクタリング後でこのような演算子が簡略化します。  検証を実行するアクション メソッドは、2 つの引数の値のいずれかを同意し、必要があります`FirstName`の値のいずれかと`LastName`です。


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

ここでユーザー入力と姓と名前で、JavaScript:

* 使うリモート呼び出しをそのペアの名前が取得されているかどうかを参照してください。
* ペアを取得した場合、エラー メッセージが表示されます。 
* ない場合は、ユーザーは、フォームを送信できます。

2 つ以上のフィールドを検証する必要があるかどうか、`[Remote]`属性として指定することにコンマ区切りの一覧です。  例については、追加する、`MiddleName`プロパティ、モデルを設定、`[Remote]`属性の次のコードに示すようにします。

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`、すべての属性の引数と同様に、定数式がある必要があります。  そのため、使用しないでください、[補間文字列](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings)呼び出したり[ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx)初期化するために`AdditionalFields`です。 追加するその他のフィールドごとに、`[Remote]`属性に、対応するコント ローラー アクション メソッドに別の引数を追加する必要があります。
