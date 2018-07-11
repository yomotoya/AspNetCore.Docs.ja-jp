---
uid: mvc/overview/getting-started/introduction/adding-validation
title: 検証の追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 97526d908dd3b835040453b53eae76f733a01c04
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38172194"
---
<a name="adding-validation"></a>検証の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションでは検証ロジックを追加します、`Movie`モデル ユーザーが作成またはアプリケーションを使用してムービーを編集するときに検証規則が適用されているを確認します。

## <a name="keeping-things-dry"></a>DRY に維持すること

ASP.NET MVC の設計原則の 1 つは[ドライ](http://en.wikipedia.org/wiki/Don't_repeat_yourself)(&quot;Don't Repeat Yourself&quot;)。 ASP.NET MVC には、機能や動作を 1 回だけ指定して、それをアプリケーションですべての場所で反映することがお勧めします。 これを記述する必要があるコードの量を削減なり、エラーが発生しやすく保守が簡単に作成するコード。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションを使用している DRY 原則の好例です。 検証規則は、1 つの場所 (モデル クラス) 内で宣言によって指定でき、アプリケーションで、規則をすべての場所で適用します。

どのムービー アプリケーションでこの検証のサポートの利用を行うを見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルへの検証規則の追加

いくつかの検証ロジックを追加することから始めます、`Movie`クラス。

*Movie.cs* ファイルを開きます。 通知、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間を含まない`System.Web`します。 DataAnnotations には、組み込みの宣言によって、クラスまたはプロパティに適用できる検証属性セットが提供します。 (などの書式設定属性も含まれています[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)を書式設定を支援し、検証が提供されません。)。

今すぐ更新、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)、および[`Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性。 置換、`Movie`を次のクラス。

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、文字列の最大長を設定、データベースに対してこの制限を設定して、そのため、データベース スキーマを変更します。 右クリックして、**映画**テーブルに**サーバー エクスプ ローラー**クリック**テーブル定義を開く**:

![](adding-validation/_static/image1.png)

上記の図で、文字列フィールドに設定すべてを表示できる[NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx)します。 スキーマを更新するのに移行を使用します。 ソリューションをビルドし、開き、**パッケージ マネージャー コンソール**ウィンドウし、次のコマンドを入力します。

[!code-console[Main](adding-validation/samples/sample2.cmd)]

このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`指定された名前の派生クラス (`DataAnnotations`)、し、`Up`メソッドのスキーマの制約を更新するコードを確認できます。

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre`フィールド値が許容されなく (つまり、値を入力する必要があります)。 `Rating`フィールドが 5 の最大長と`Title`が 60 の最大長。 上の 3 の最小長`Title`上の範囲と`Price`スキーマの変更は作成されませんでした。

ムービーのスキーマを確認します。

![](adding-validation/_static/image2.png)

文字列フィールドは、新しい長さの制限を表示し、 `Genre` null 許容型としてチェックされていません。

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required` および `MinimumLength` 属性は、プロパティに値が必要であることを示します。ただし、この検証を満たすためにユーザーが空白を入力することは禁止されていません。 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)属性を使用できる文字の制限を入力します。 上記のコードで、`Genre` と `Rating` は、文字のみを使用する必要があります (空白、数字、特殊文字は使用できません)。 [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性が指定した範囲内に値を制限します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。 値の型 (など`decimal, int, float, DateTime`) は本質的に必須であり必要はありません、`Required`属性。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスで指定した検証規則が適用されている最初でいます。 たとえば、次のコードがスローされます、 [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx)例外ときに、`SaveChanges`メソッドが呼び出されると、いくつか必要なため、`Movie`プロパティの値はありません。

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

上記のコードでは、次の例外がスローされます。

*1 つまたは複数のエンティティの検証に失敗しました。詳細については、'EntityValidationErrors' プロパティを参照してください。*

検証規則が、.NET Framework によって自動的に適用することで、アプリケーションより堅牢です。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC の検証エラー UI

アプリケーションを実行しに移動し、 */Movies* URL。

をクリックして、**新規作成**のリンクを新しいムービーを追加します。 フォームに無効な値をいくつか入力します。 jQuery クライアント側の検証でエラーが検出されるとすぐに、エラー メッセージが表示されます。

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (「,」)、小数点を NuGet を含める必要がありますこのチュートリアルで前述のようにグローバル化します。


どのフォームが自動的に赤い境界線の色を強調表示に使用無効なデータが含まれ、それぞれの横にある該当する検証エラー メッセージが生成するテキスト ボックスに注意してください。 エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。

実際のメリットは、1 行のコードを変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml*この検証 UI を有効にするために表示します。 このチュートリアルで前に作成したコントローラーとビューにより、`Movie` モデル クラスのプロパティで検証属性を使って指定した検証規則が自動的に取得されます。 `Edit` アクション メソッドを使って検証をテストします。同じ検証が適用されます。

クライアント側の検証エラーがなくなるまで、フォーム データはサーバーに送信されません。 これを確認するにを使用して、HTTP Post メソッドにブレークポイントを配置することで、 [fiddler ツール](http://fiddler2.com/fiddler2/)、または IE [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)します。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧表示、`Create`メソッド、`MovieController`クラスのようになります。 このチュートリアルで先ほどの作成方法を変更していません。

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

最初の (HTTP GET の) `Create` アクション メソッドは、初期の作成フォームを表示します。 2 番目の (`[HttpPost]`) バージョンは、フォームの送信を処理します。 2 番目の `Create` メソッド (`HttpPost` バージョン) は、`ModelState.IsValid` を呼び出してムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create` メソッドはフォームを再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。 このムービーの例で**サーバーには、クライアント側で検出された検証エラーがある場合に、フォームは送信されません、2 つ目** `Create`**メソッドが呼び出されない**します。 クライアントの検証が無効になっている場合は、ブラウザーで JavaScript を無効にして、HTTP POST`Create`メソッドの呼び出し`ModelState.IsValid`ムービーに検証エラーがあるかどうかを確認します。

`HttpPost Create` メソッドにブレークポイントを設定し、メソッドが呼び出されないことを確認できます。検証エラーが検出された場合、クライアント側の検証はフォームのデータを送信しません。 ブラウザーで JavaScript を無効にすると、エラーのあるフォームが送信され、ブレークポイントがヒットします。 JavaScript がなくても完全な検証が行われます。 次の図は、Internet Explorer で JavaScript を無効にする方法を示します。

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

次の図では、FireFox ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation/_static/image7.png)

次の図では、Chrome ブラウザーで JavaScript を無効にする方法を示します。

![](adding-validation/_static/image8.png)

以下は、 *Create.cshtml*チュートリアルの前半でスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

コードの使用に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティ。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッド。 これら 2 つのヘルパー メソッドがコント ローラーで、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 適切なモデルと表示のエラー メッセージに指定された検証属性に自動的に検索します。

この方法の非常によい点は、コントローラーも `Create` ビュー テンプレートも、適用される実際の検証規則や、表示される特定のエラー メッセージについて、何も知らないことです。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。 同じ検証規則が、`Edit` ビューおよびモデルを編集する他のユーザー作成のビュー テンプレートに、自動的に適用されます。

後で検証ロジックを変更する場合は、これを行うだけで、モデルに検証属性を追加することで (この例で、`movie`クラス)。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 完全に従うことを意味し、*ドライ*原則です。

## <a name="using-datatype-attributes"></a>DataType 属性の使用

*Movie.cs* ファイルを開き、`Movie` クラスを調べます。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、組み込みの検証属性セットだけでなく、書式設定属性を提供します。 既に適用された、 [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドに列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、データの書式設定、ビュー エンジンに対してヒントのみを提供 (などの属性を提供し、 `<a>` url のおよび`<a href="mailto:EmailAddress.com">`電子メールの。 使用することができます、 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)データの形式を検証する属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)は、データベースの組み込み型よりも具体的であるデータ型を指定する属性が使用される***いない***検証属性。 この例では、追跡する必要があるのは、日付と時刻ではなく、日付のみです。 [DataType 列挙](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)など多くのデータ型を提供します*日付、時刻、PhoneNumber、Currency、EmailAddress*など。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 など、`mailto:`に対してリンクを作成できる[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)、日付セレクターを指定することができますと[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)をサポートするブラウザーで[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、HTML 5 を出力[データ -](http://ejohn.org/blog/html-5-data-attributes/) ("と発音*データ ダッシュ*) HTML 5 ブラウザーが認識できる属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、いずれかの検証を行いません。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データ フィールドはサーバーの既定の書式に従って表示[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)します。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode`設定では、指定した書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。 (たくないをいくつかのフィールドをたとえば、通貨の値のたくない、テキスト ボックスに通貨記号を編集するためです)。

使用することができます、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)自体が、属性が使用することをお勧めでは通常、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性もします。 `DataType`属性の伝達、*セマンティクス*のデータとだけで画面上でのレンダリング方法ではなくられると、次の利点を提供します`DisplayFormat`:

- ブラウザーでは、(たとえば、カレンダー コントロール、ロケールに適した通貨記号、メール リンクなどを表示する)。 HTML5 機能を有効にできます。
- 既定では、ブラウザーがに基づいて正しい書式を使用してデータを表示、[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)します。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性には、データを表示するために正しいフィールド テンプレートの選択を MVC が有効にすることができます (、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)文字列テンプレートを使用してそれ自体で使用される場合)。 詳細については、Brad Wilson を参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)します。 (MVC 2 用に記述されたもこの記事でまだに適用 ASP.NET MVC の現在のバージョン)。

使用する場合、`DataType`属性指定する必要が、日付フィールドを持つ、 `DisplayFormat` Chrome ブラウザーで、フィールドが正常に表示されることを確認するためにも属性。 詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)します。

> [!NOTE]
> jQuery の検証では機能しません、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性と[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。 たとえば、次のコードでは、指定した範囲内の日付であっても、クライアント側の検証エラーが常に表示されます。
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> 使用する jQuery の日付検証を無効にする必要があります、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。 これは一般にコンパイルを使用して、モデルで日付をハードコーディングすることをお勧め、[範囲](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性と[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)をお勧めします。


次のコードは、1 行で複数の属性を組み合わせる例です。

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。

> [!div class="step-by-step"]
> [前へ](adding-a-new-field.md)
> [次へ](examining-the-details-and-delete-methods.md)
