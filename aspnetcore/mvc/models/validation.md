---
title: "ASP.NET Core MVC でのモデルの検証"
author: rick-anderson
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
ms.openlocfilehash: 0874d3b677cee2859da9eb85b0573811abbed12a
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
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

[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]

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

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

`movie`上を表す変数、`Movie`を検証するフォームの送信からのデータを格納しているオブジェクト。 検証コードが日とでジャンルをチェックするこの例では、`IsValid`のメソッド、`ClassicMovieAttribute`規則に従ってクラスです。 検証成功時に`IsValid`を返します、`ValidationResult.Success`コード、および検証に失敗したときに、`ValidationResult`エラー メッセージを使用します。 ユーザーが変更すると、`Genre`フィールドに、フォームを送信して、`IsValid`のメソッド、`ClassicMovieAttribute`ムービーが、従来型であるかどうかを確認してください。 すべての組み込みの属性と同様に適用、`ClassicMovieAttribute`などのプロパティに`ReleaseDate`前のコード例で示すように、検証が発生したことを確認します。 のみ行われますので`Movie`を使用する型の場合よりよい方法は`IValidatableObject`次の段落に示すようにします。

また、この同じコードでしたに配置するモデルを実装して、`Validate`メソッドを`IValidatableObject`インターフェイスです。 実装するカスタム検証属性は、個々 のプロパティを検証するため、動作、`IValidatableObject`を次に示すようにクラス レベルの検証を実装するために使用できます。

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a>クライアント側の検証

クライアント側の検証は、ユーザーにとって非常に便利です。 保存されますがそれ以外の場合に費やす時間のラウンド トリップを待機しているサーバー。 ビジネス用語として、何百もの時間を乗算した値の秒の小数部をもいくつか追加最大多数の時間、費用やフラストレーションは解消します。 簡単かつ迅速検証より効率的に作業しより優れた品質の入力し、出力を生成することができます。

次のように動作するクライアント側の検証のために、適切な JavaScript のスクリプト参照では、ビューが必要です。

[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

MVC では、モデルのプロパティからの型のメタデータの他の検証属性を使用してデータを検証し、JavaScript を使用してエラー メッセージを表示します。 MVC を使用してモデルからのフォーム要素を表示するために使用すると[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)または[HTML ヘルパー](xref:mvc/views/overview)は HTML 5、追加[データ属性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)として、検証を必要のあるフォーム要素で次に示します。 MVC の生成、`data-`組み込みとカスタムの両方の属性の属性です。 次に示すように、関連するタグ ヘルパーを使用して、クライアントで検証エラーを表示できます。

[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

上記のタグ ヘルパーは、以下の HTML を表示します。 注意して、 `data-` html 属性の出力の検証属性に対応している、`ReleaseDate`プロパティです。 `data-val-required`下の属性には、[リリース日] フィールドに、ユーザーを満たさないし、同時にそのメッセージが表示する場合に表示するエラー メッセージが含まれています。`<span>`要素。

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

クライアント側の検証では、形式が有効になるまで、送信ができなくなります。 [送信] ボタンは、フォームを送信するか、エラー メッセージを表示する JavaScript を実行します。

MVC は、.NET データ型に基づく可能性のあるオーバーライドを使用して、プロパティの型の属性値を決定`[DataType]`属性。 基本`[DataType]`属性では、実際のサーバー側の検証は行われません。 ブラウザーでは、独自のエラー メッセージを選択し、jQuery 検証控えめなパッケージが、メッセージを上書きし、それらを他のユーザーと一貫して表示ただし、希望するが、それらのエラーを表示します。 ユーザーを適用すると最も明らかにこのような`[DataType]`などサブクラス`[EmailAddress]`です。

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

2 つの手順では、リモート検証を実装できます。 最初を使用したモデルを付ける必要があります、`[Remote]`属性。 `[Remote]`属性は複数のオーバー ロードを呼び出して、適切なコードをクライアント側 JavaScript を直接行うこともできますを受け取ります。 例が指す、`VerifyEmail`のアクション メソッド、`Users`コント ローラー。

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

定義されている、2 番目の手順が、対応するアクション メソッドに検証コードを配置する、`[Remote]`属性。 返します、`JsonResult`クライアント側が続行または一時停止、および必要な場合にエラーが表示するために使用できます。

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

これで、ビューでの JavaScript が確認の電子メールを取得したかどうかにリモート呼び出しを行うユーザーは、電子メール アドレスを入力、ときに、その場合は、エラー メッセージを表示します。 それ以外の場合、ユーザーは通常どおり、フォームを送信できます。
