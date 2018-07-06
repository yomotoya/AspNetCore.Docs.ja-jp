---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: モデル検証の追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 50911920ed80f09033cfc53a137a6bbba3fc62f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834492"
---
<a name="adding-validation-to-the-model"></a>モデル検証の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションでは検証ロジックを追加します、`Movie`モデル ユーザーが作成またはアプリケーションを使用してムービーを編集するときに検証規則が適用されているを確認します。

## <a name="keeping-things-dry"></a>DRY に維持すること

ASP.NET MVC の設計原則の 1 つは、DRY (&quot;Don't Repeat Yourself&quot;)。 ASP.NET MVC には、機能や動作を 1 回だけ指定して、それをアプリケーションですべての場所で反映することがお勧めします。 これを記述する必要があるコードの量を削減なり、エラーが発生しやすく保守が簡単に作成するコード。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションを使用している DRY 原則の好例です。 検証規則は、1 つの場所 (モデル クラス) 内で宣言によって指定でき、アプリケーションで、規則をすべての場所で適用します。

どのムービー アプリケーションでこの検証のサポートの利用を行うを見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルへの検証規則の追加

いくつかの検証ロジックを追加することから始めます、`Movie`クラス。

*Movie.cs* ファイルを開きます。 追加、`using`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

名前空間を含まないに注意してください。`System.Web`します。 DataAnnotations には、組み込みの宣言によって、クラスまたはプロパティに適用できる検証属性セットが提供します。

今すぐ更新、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性. 属性を適用する場所の例として、次のコードを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

アプリケーションを実行し、次の実行時エラーがもう一度表示されます。

***データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。データベースを更新する Code First Migrations を使用する ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***

スキーマを更新するのに移行を使用します。 ソリューションをビルドし、開き、**パッケージ マネージャー コンソール**ウィンドウし、次のコマンドを入力します。

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`指定された名前の派生クラス (*AddDataAnnotationsMig*)、および、`Up`メソッドを更新するコードを表示できますスキーマの制約。 `Title`と`Genre`フィールドが null 許容では不要になった (つまり、値を入力する必要があります)、`Rating`フィールドが 5 の最大長。

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required`属性では、プロパティ値がある必要がありますを示します。 このサンプルで、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、および`Price`プロパティを有効にするためにします。 `Range` 属性は、指定した範囲内に値を制限します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。 組み込みの型 (など`decimal, int, float, DateTime`) は、既定で要求され、必要はありません、`Required`属性。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスで指定した検証規則が適用されている最初でいます。 たとえば、次のコードは例外をスロー時に、`SaveChanges`メソッドが呼び出されると、いくつか必要なため、`Movie`プロパティの値は不足していると、価格が (0 では有効な範囲外です)。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

検証規則が、.NET Framework によって自動的に適用することで、アプリケーションより堅牢です。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

ここで、更新された一覧を表示する完全なコードは、 *Movie.cs*ファイル。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC の検証エラー UI

アプリケーションを再実行しに移動し、 */Movies* URL。

をクリックして、**新規作成**のリンクを新しいムービーを追加します。 いくつかの無効な値をフォームに入力し、**作成**ボタンをクリックします。

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (&quot;、&quot;) と小数点を含める必要があります*globalize.js*と特定*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`します。 次のコードは操作する Views\Movies\Edit.cshtml ファイルへの変更、 &quot;FR-FR&quot;カルチャ。


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

どのフォームが自動的に赤い境界線の色を強調表示に使用無効なデータが含まれ、それぞれの横にある該当する検証エラー メッセージが生成するテキスト ボックスに注意してください。 エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。

実際のメリットは、1 行のコードを変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml*この検証 UI を有効にするために表示します。 このチュートリアルで前に作成したコントローラーとビューにより、`Movie` モデル クラスのプロパティで検証属性を使って指定した検証規則が自動的に取得されます。

プロパティにお気付き`Title`と`Genre`、フォームを送信するまでに必要な属性が適用されません (ヒット、**作成**ボタン)、または入力フィールドにテキストを入力し、それを削除します。 フィールドは最初は空 (など、作成ビューにフィールド) と必須の属性のみと、その他の検証属性を持つ検証をトリガーするには、次を実行できます。

1. フィールドにタブします。
2. いくつかのテキストを入力します。
3. タブを終了します。
4. フィールドにタブします。
5. テキストを削除します。
6. タブを終了します。

上の順序は、送信ボタンを押すことがなく、必要な検証をトリガーします。 単に、フィールドのいずれかを入力しなくても、送信ボタンを押すと、クライアント側の検証がトリガーされます。 クライアント側の検証エラーがなくなるまで、フォーム データはサーバーに送信されません。 これをテストするには、HTTP Post メソッドにブレークポイントを配置するかを使用して、 [fiddler ツール](http://fiddler2.com/fiddler2/)または IE 9 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)します。

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧表示、`Create`メソッド、`MovieController`クラスのようになります。 このチュートリアルで先ほどの作成方法を変更していません。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

最初の (HTTP GET の) `Create` アクション メソッドは、初期の作成フォームを表示します。 2 番目の (`[HttpPost]`) バージョンは、フォームの送信を処理します。 2 番目の `Create` メソッド (`HttpPost` バージョン) は、`ModelState.IsValid` を呼び出してムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create` メソッドはフォームを再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。 このムービーの例を使用していることで**サーバーには、クライアント側で検出された検証エラーがある場合に、フォームは送信されません、2 つ目** `Create`**メソッドが呼び出されない**します。 クライアントの検証が無効になっている場合は、ブラウザーで JavaScript を無効にして、HTTP POST`Create`メソッドの呼び出し`ModelState.IsValid`ムービーに検証エラーがあるかどうかを確認します。

`HttpPost Create` メソッドにブレークポイントを設定し、メソッドが呼び出されないことを確認できます。検証エラーが検出された場合、クライアント側の検証はフォームのデータを送信しません。 ブラウザーで JavaScript を無効にすると、エラーのあるフォームが送信され、ブレークポイントがヒットします。 JavaScript がなくても完全な検証が行われます。 次の図は、Internet Explorer で JavaScript を無効にする方法を示します。

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

次の図では、FireFox ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation-to-the-model/_static/image5.png)

次の図は、Chrome ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation-to-the-model/_static/image6.png)

以下は、 *Create.cshtml*チュートリアルの前半でスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

コードの使用に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティ。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッド。 これら 2 つのヘルパー メソッドがコント ローラーで、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 適切なモデルと表示のエラー メッセージに指定された検証属性に自動的に検索します。

コント ローラーもビュー テンプレートの作成を知っているもの、または特定のエラー メッセージが表示されますが適用されている実際の検証ルールに関する、このアプローチのすばらしい点です。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。 これらの同じ検証規則は、編集ビューとその他のビュー テンプレートを作成するモデルを編集するに自動的に適用されます。

後で検証ロジックを変更する場合は、これを行うだけで、モデルに検証属性を追加することで (この例で、`movie`クラス)。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 また、これは DRY 原則に完全に従うことを意味します。

## <a name="adding-formatting-to-the-movie-model"></a>ムービー モデルへの書式設定を追加します。

*Movie.cs* ファイルを開き、`Movie` クラスを調べます。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、組み込みの検証属性セットだけでなく、書式設定属性を提供します。 既に適用された、 [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドに列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性は検証属性ではありません、HTML をレンダリングする方法をビュー エンジンに通知に使用されます。 上記の例では、`DataType.Date`属性は、時刻のない日にのみ、としてムービー日付を表示します。 たとえば、次[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性は、データの形式を検証しません。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

上記の属性は、データの書式設定、ビュー エンジンに対してヒントのみを提供 (などの属性の指定と&lt;、&gt; url のおよび&lt;a =&quot;mailto:EmailAddress.com&quot; &gt;電子メール。 使用することができます、 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)データの形式を検証する属性。

別のアプローチを使用する、`DataType`属性が明示的に設定する、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。 次のコードは日付の書式設定文字列とリリースの日付プロパティ (つまり、 &quot;d&quot;)。 リリース日の一部として、時間にするないを指定するのにには、これを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

完全な`Movie`クラスを次に示します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

アプリケーションを実行しを参照、`Movies`コント ローラー。 リリース日と価格が適切に書式設定されます。 次の図は、リリース日と価格を使用して&quot;FR-FR&quot;カルチャとします。

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

次の図は、同じデータが既定のカルチャ (英語) で表示されます。

![](adding-validation-to-the-model/_static/image8.png)

このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。

> [!div class="step-by-step"]
> [前へ](adding-a-new-field-to-the-movie-model-and-table.md)
> [次へ](examining-the-details-and-delete-methods.md)
