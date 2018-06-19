---
uid: mvc/overview/getting-started/introduction/adding-validation
title: 検証の追加 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 946d4d5e5a506fb437232f9f4440c98e33a1a9b3
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "33966560"
---
<a name="adding-validation"></a>検証の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

検証ロジックをここで追加、`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。

## <a name="keeping-things-dry"></a>ドライに保つこと

ASP.NET MVC の中核となる設計の基本思想の 1 つは[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)(&quot;しないことを繰り返さない&quot;)。 ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。 これを記述する必要のあるコードの量を削減と低いエラーが発生しやすいと保守が簡単に記述して、コード。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。 (モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、どこからでも、アプリケーションで、規則を適用します。

ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルに検証規則を追加します。

いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。

*Movie.cs* ファイルを開きます。 通知、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が含まれていない`System.Web`です。 DataAnnotations は、すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。 (などの書式設定属性も含まれています[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)を書式設定のヘルプおよびいずれかの検証が提供されない。)。

更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)、および[`Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性。 置換、`Movie`を次のクラス。

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、文字列の最大長を設定、データベースに対してこの制限を設定して、そのため、データベース スキーマを変更します。 右クリックして、**映画**テーブルに**サーバー エクスプ ローラー**  をクリック**テーブル定義を開く**:

![](adding-validation/_static/image1.png)

上記の図で、文字列フィールドに設定するすべてを表示できる[NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx)です。 スキーマを更新するのに移行を使用します。 ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。

[!code-console[Main](adding-validation/samples/sample2.cmd)]

このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`指定された名前のクラスを派生 (`DataAnnotations`)、し、、`Up`メソッドのスキーマの制約を更新するコードを確認できます。

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre`フィールドは null 許容では不要になった (つまり、値を入力する必要があります)。 `Rating`フィールドが 5 の最大長を持つと`Title`60 の最大長であります。 上の 3 の最小の長さ`Title`上の範囲と`Price`スキーマの変更を作成できませんでした。

ムービーのスキーマを確認してください。

![](adding-validation/_static/image2.png)

文字列フィールドは、新しい長さ制限を表示および`Genre`null 許容型としてチェックされていません。

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required` および `MinimumLength` 属性は、プロパティに値が必要であることを示します。ただし、この検証を満たすためにユーザーが空白を入力することは禁止されていません。 [正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)属性を使用できる文字の制限を入力します。 上記のコードで、`Genre` と `Rating` は、文字のみを使用する必要があります (空白、数字、特殊文字は使用できません)。 [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性に指定した範囲内の値を規定します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。 値の型 (など`decimal, int, float, DateTime`) が本質的に必須で、必要はありません、`Required`属性。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。 たとえば、次のコードがスローされます、 [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx)例外時に、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティの値が見つかりません。

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

上記のコードには、次の例外がスローされます。

*1 つまたは複数のエンティティの検証に失敗しました。詳細については 'EntityValidationErrors' プロパティを参照してください。*

検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC では、UI の検証エラー

アプリケーションを実行しに移動し、 */Movies* URL。

をクリックして、**新規作成**リンクを新しいムービーを追加します。 フォームに無効な値をいくつか入力します。 jQuery クライアント側の検証でエラーが検出されるとすぐに、エラー メッセージが表示されます。

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (「,」)、小数点、NuGet を含める必要がありますこのチュートリアルで前述のようにグローバル化します。


フォームが自動的に使用方法赤い枠線の色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。 エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。

実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml* UI この検証を有効にするために表示します。 このチュートリアルで前に作成したコントローラーとビューにより、`Movie` モデル クラスのプロパティで検証属性を使って指定した検証規則が自動的に取得されます。 `Edit` アクション メソッドを使って検証をテストします。同じ検証が適用されます。

クライアント側の検証エラーがなくなるまで、フォーム データはサーバーに送信されません。 これを確認するには HTTP Post メソッドを使用してブレークポイントを配置することにより、 [fiddler ツール](http://fiddler2.com/fiddler2/)、または、IE [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)です。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。 このチュートリアルで先ほどの作成から変更されていません。

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

最初の (HTTP GET の) `Create` アクション メソッドは、初期の作成フォームを表示します。 2 番目の (`[HttpPost]`) バージョンは、フォームの送信を処理します。 2 番目の `Create` メソッド (`HttpPost` バージョン) は、`ModelState.IsValid` を呼び出してムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create` メソッドはフォームを再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。 例では、ムービー、**クライアント側で検出された検証エラーがある場合に、サーバーにフォームが送信されません、2 つ目** `Create`**メソッドが呼び出されない**です。 クライアントの検証が無効になっている場合は、ブラウザーで JavaScript が無効にして HTTP POST`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。

`HttpPost Create` メソッドにブレークポイントを設定し、メソッドが呼び出されないことを確認できます。検証エラーが検出された場合、クライアント側の検証はフォームのデータを送信しません。 ブラウザーで JavaScript を無効にすると、エラーのあるフォームが送信され、ブレークポイントがヒットします。 JavaScript がなくても完全な検証が行われます。 次の図は、Internet Explorer で JavaScript を無効にする方法を示しています。

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

次の図では、FireFox ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation/_static/image7.png)

次の図では、Chrome ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation/_static/image8.png)

以下は、 *Create.cshtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。 これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。

この方法の非常によい点は、コントローラーも `Create` ビュー テンプレートも、適用される実際の検証規則や、表示される特定のエラー メッセージについて、何も知らないことです。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。 同じ検証規則が、`Edit` ビューおよびモデルを編集する他のユーザー作成のビュー テンプレートに、自動的に適用されます。

検証ロジックを後で変更する場合は、これを行う正確に 1 か所でモデルの検証属性を追加することで (この例では、`movie`クラス)。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 完全に従うことを意味し、*ドライ*原則です。

## <a name="using-datatype-attributes"></a>DataType 属性の使用

*Movie.cs* ファイルを開き、`Movie` クラスを調べます。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。 既に適用された、 [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、データを書式設定、ビュー エンジンのヒントを提供するだけ (などの属性を提供し、`<a>`の URL と`<a href="mailto:EmailAddress.com">`電子メールのです。 使用することができます、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)データの形式を検証する属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を使用してデータベースの組み込み型よりも特定のデータ型を指定、される***されません***検証属性。 この例では、追跡する必要があるのは、日付と時刻ではなく、日付のみです。 [DataType 列挙](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)などの多くのデータ型の提供*日付、時刻、PhoneNumber、通貨、EmailAddress*などです。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 たとえば、`mailto:`に対してリンクを作成できる[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)、日付選択を指定することができます、 [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)をサポートするブラウザーで[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、HTML 5 を出力[データ -](http://ejohn.org/blog/html-5-data-attributes/) (発音*データ ダッシュ*) HTML 5 ブラウザーで認識できる属性です。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、いずれかの検証を渡さないようにします。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データ フィールドを基に、サーバーの既定の形式に従って表示[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)です。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode`設定では、指定した書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。 (したくないをいくつかのフィールドの — たとえば、通貨の値のたくない、テキスト ボックス内の通貨記号を編集するためです)。

使用することができます、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)自体が、属性が使用することをお勧めでは一般に、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)も属性します。 `DataType`属性を伝達、*セマンティクス*データとして、画面に表示する方法ではなくおよびのでは得られない、次の利点を提供`DisplayFormat`:

- ブラウザーには、HTML5 機能 (たとえば、予定表コントロール、ロケールに応じた通貨記号、電子メールへのリンクなどを表示します。) が有効にすることができます。
- 既定では、ブラウザーがに基づいて、正しい形式を使用してデータを表示、[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)です。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性には、データを表示するために右フィールド テンプレートを選択する MVC が有効にすることができます (、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)文字列テンプレートを使用して自体によって使用されている場合)。 詳細については、Brad Wilson を参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)です。 (MVC 2 用に記述されたがこの記事の内容も適用 ASP.NET MVC の現在のバージョンに)。

使用する場合、`DataType`属性指定する必要が、日付フィールドに関連付け、 `DisplayFormat` Chrome ブラウザーで、フィールドが正常に表示されることを確認するためにも属性。 詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)です。

> [!NOTE]
> jQuery 検証では機能しません、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性と[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。 たとえば、次のコードでは、指定した範囲内の日付であっても、クライアント側の検証エラーが常に表示されます。
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> 使用する jQuery 日の検証を無効にする必要があります、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性が[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。 これは一般を使用して、モデル内のハードの日付をコンパイルすることをお勧め、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性と[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)をお勧めします。


次のコードは、1 行で複数の属性を組み合わせる例です。

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。

> [!div class="step-by-step"]
> [前へ](adding-a-new-field.md)
> [次へ](examining-the-details-and-delete-methods.md)
