---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "モデルの検証の追加 |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 93b4df5fcbde8d87866d00dffda8a241d0dd596b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model"></a>モデルの検証の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


これでこのセクションで追加検証ロジックを`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。

## <a name="keeping-things-dry"></a>ドライに保つこと

ASP.NET MVC の中核となる設計の基本思想の 1 つはドライ (&quot;しないことを繰り返さない&quot;)。 ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。 これを記述する必要のあるコードの量を削減と低いエラーが発生しやすいと保守が簡単に記述して、コード。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。 (モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、どこからでも、アプリケーションで、規則を適用します。

ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルに検証規則を追加します。

いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。

*Movie.cs* ファイルを開きます。 追加、`using`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

名前空間が含まれていない`System.Web`です。 DataAnnotations は、すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。

更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性. 属性を適用する場所の例として、次のコードを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

アプリケーションを実行し、次の実行時エラーがもう一度表示されます。

***データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。Code First Migrations を使用して、データベースを更新するを検討してください ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***

スキーマを更新するのに移行を使用します。 ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`指定された名前のクラスを派生 (*AddDataAnnotationsMig*)、および、`Up`メソッドを更新するコードを確認できますスキーマの制約。 `Title`と`Genre`フィールドが null 許容では不要になった (つまり、値を入力する必要があります) および`Rating`フィールドが 5 の最大長。

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required`属性は、プロパティは値である必要がありますを示します。 このサンプルでは、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、と`Price`有効にするにはプロパティです。 `Range` 属性は、指定した範囲内に値を制限します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。 組み込みの型 (など`decimal, int, float, DateTime`) が既定値を必要し、しない必要があります、`Required`属性。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。 次のコードが例外をスローするなど、ときに、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティ値が不足していると、価格が (0 では、有効な範囲外です)。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

ここでは、完全なコードが、更新されたリスト*Movie.cs*ファイル。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC では、UI の検証エラー

アプリケーションを再実行しに移動し、 */Movies* URL。

をクリックして、**新規作成**リンクを新しいムービーを追加します。 いくつかの無効な値にフォームに記入し、**作成**ボタンをクリックします。

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (&quot;、&quot;) する必要があります、小数点の*globalize.js*と特定の*cultures/globalize.cultures.js*ファイル (から[https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`です。 次のコードを操作する Views\Movies\Edit.cshtml ファイルへの変更を示しています、 &quot;FR-FR&quot;カルチャ。


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

フォームが自動的に使用方法赤い枠線の色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。 エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。

実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml* UI この検証を有効にするために表示します。 このチュートリアルで前に作成したコントローラーとビューにより、`Movie` モデル クラスのプロパティで検証属性を使って指定した検証規則が自動的に取得されます。

プロパティ用お気付き`Title`と`Genre`、フォームを送信するまでに必要な属性が適用されません (ヒット、**作成**ボタンをクリック)、または入力フィールドにテキストを入力し、それを削除します。 フィールドを最初に空 (作成ビューのフィールド) などで必須の属性のみと、他の検証属性を持つ検証をトリガーするには、次を実行できます。

1. タブのフィールドにします。
2. いくつかのテキストを入力します。
3. タブです。
4. フィールドに戻す タブします。
5. テキストを削除します。
6. タブです。

上の順序は、[送信] ボタンを押すことがなく必要な検証をトリガーします。 単に任意のフィールドを入力することがなく送信 ボタンを押すと、クライアント側の検証がトリガーされます。 クライアント側の検証エラーがなくなるまで、フォーム データはサーバーに送信されません。 これをテストするには、HTTP Post メソッドにブレークポイントを配置するかを使用して、 [fiddler ツール](http://fiddler2.com/fiddler2/)または IE 9 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)です。

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。 このチュートリアルで先ほどの作成から変更されていません。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

最初の (HTTP GET の) `Create` アクション メソッドは、初期の作成フォームを表示します。 2 番目の (`[HttpPost]`) バージョンは、フォームの送信を処理します。 2 番目の `Create` メソッド (`HttpPost` バージョン) は、`ModelState.IsValid` を呼び出してムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create` メソッドはフォームを再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。 使用して、このムービーの例では**サーバーには、クライアント側で検出された検証エラーがある場合に、フォームは通知されません、2 つ目** `Create`**メソッドが呼び出されない**です。 クライアントの検証が無効になっている場合は、ブラウザーで JavaScript が無効にして HTTP POST`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。

`HttpPost Create` メソッドにブレークポイントを設定し、メソッドが呼び出されないことを確認できます。検証エラーが検出された場合、クライアント側の検証はフォームのデータを送信しません。 ブラウザーで JavaScript を無効にすると、エラーのあるフォームが送信され、ブレークポイントがヒットします。 JavaScript がなくても完全な検証が行われます。 次の図は、Internet Explorer で JavaScript を無効にする方法を示しています。

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

次の図では、FireFox ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation-to-the-model/_static/image5.png)

次の図は、Chrome ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation-to-the-model/_static/image6.png)

以下は、 *Create.cshtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。 これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。

このアプローチのすばらしい点は、コント ローラーもビュー テンプレートの作成を知っているものが適用されている実際の検証規則の情報や、特定のエラー メッセージが表示されてです。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。 これらの同じ検証規則は、編集ビュー、およびその他のビュー テンプレートを作成、モデルを編集するに自動的に適用されます。

検証ロジックを後で変更する場合は、これを行う正確に 1 か所でモデルの検証属性を追加することで (この例では、`movie`クラス)。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 すると、いることを意味して完全性を記念するためです。

## <a name="adding-formatting-to-the-movie-model"></a>書式設定、ムービーのモデルを追加します。

*Movie.cs* ファイルを開き、`Movie` クラスを調べます。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。 既に適用された、 [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性が検証属性ではありません、HTML をレンダリングする方法をビュー エンジンに通知するために使用します。 上記の例では、`DataType.Date`属性は、時刻のない日にのみ、としてムービー日付を表示します。 たとえば、次[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性は、データの形式を検証しません。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

上に示した属性は、データを書式設定、ビュー エンジンのヒントを提供するだけ (などの属性を提供および&lt;、&gt;の URL をおよび&lt;、href =&quot;mailto:EmailAddress.com&quot; &gt;電子メール アドレス。 使用することができます、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)データの形式を検証する属性。

その他の方法を使用する、`DataType`属性が明示的に設定する、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。 次のコードは、日付の書式設定文字列のリリース日付プロパティを示しています (つまり、 &quot;d&quot;)。 リリース日の一部として、時間にするないを指定するのにには、これを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

完全な`Movie`クラスを次に示します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

アプリケーションを実行しを参照、`Movies`コント ローラー。 リリース日と価格の形式が適切にします。 次の図は、リリース日とを使用して価格を示しています。 &quot;FR-FR&quot; 、カルチャとします。

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

次の図は、既定のカルチャ (英語) に表示される同じデータを示します。

![](adding-validation-to-the-model/_static/image8.png)

このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。

>[!div class="step-by-step"]
[前へ](adding-a-new-field-to-the-movie-model-and-table.md)
[次へ](examining-the-details-and-delete-methods.md)
