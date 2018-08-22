---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: 検証モデルに追加する (c#) |Microsoft Docs
author: Rick-Anderson
description: コント ローラーの作成
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 14db6184d3edd584fe35529cd4cf1dba5b528ab1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828940"
---
<a name="adding-validation-to-the-model-c"></a>(C#)、モデル検証の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの。


このセクションでは検証ロジックを追加します、`Movie`モデル ユーザーが作成またはアプリケーションを使用してムービーを編集するときに検証規則が適用されているを確認します。

## <a name="keeping-things-dry"></a>DRY に維持すること

ASP.NET MVC の設計原則の 1 つは、DRY ("Don't Repeat Yourself") です。 ASP.NET MVC には、機能や動作を 1 回だけ指定して、それをアプリケーションですべての場所で反映することがお勧めします。 これを記述する必要があるコードの量を削減なり、コードを維持するために非常に簡単に記述する操作を行います。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションを使用している DRY 原則の好例です。 検証規則は、1 つの場所 (モデル クラス) 内で宣言によって指定でき、それらのルールは、アプリケーションですべての場所で適用します。

どのムービー アプリケーションでこの検証のサポートの利用を行うを見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルへの検証規則の追加

いくつかの検証ロジックを追加することから始めます、`Movie`クラス。

*Movie.cs* ファイルを開きます。 追加、`using`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

名前空間は、.NET Framework の一部です。 組み込みの宣言によって、クラスまたはプロパティに適用できる検証属性セットを提供します。

今すぐ更新、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性. 属性を適用する場所の例として、次のコードを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required`属性では、プロパティ値がある必要がありますを示します。 このサンプルで、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、および`Price`プロパティを有効にするためにします。 `Range` 属性は、指定した範囲内に値を制限します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスで指定した検証規則が適用されている最初でいます。 たとえば、次のコードは例外をスロー時に、`SaveChanges`メソッドが呼び出されると、いくつか必要なため、`Movie`プロパティの値は不足していると、価格が (0 では有効な範囲外です)。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

検証規則が、.NET Framework によって自動的に適用することで、アプリケーションより堅牢です。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

ここで、更新された一覧を表示する完全なコードは、 *Movie.cs*ファイル。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC の検証エラー UI

アプリケーションを再実行しに移動し、 */Movies* URL。

をクリックして、**ムービーの作成**のリンクを新しいムービーを追加します。 いくつかの無効な値をフォームに入力し、**作成**ボタンをクリックします。

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

どのフォームが自動的に背景色を強調表示に使用無効なデータが含まれ、それぞれの横にある該当する検証エラー メッセージが生成するテキスト ボックスに注意してください。 エラー メッセージに注釈を付けるときに指定したエラーの文字列が一致する、`Movie`クラス。 エラーには、(この場合、ユーザーは、JavaScript を無効になっていますがある) (JavaScript を使用して) クライアント側とサーバー側の両方が適用されます。

実際のメリットは、1 行のコードを変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml*この検証 UI を有効にするために表示します。 コント ローラーとビューが自動的にこのチュートリアルで先ほど作成した検証規則で属性を使用して指定したことを選択、`Movie`モデル クラス。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧表示、`Create`メソッド、`MovieController`クラスのようになります。 このチュートリアルで先ほどの作成方法を変更していません。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

最初のアクション メソッドには、初期の作成フォームが表示されます。 2 つ目は、フォーム ポストを処理します。 2 番目の`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create`メソッドには、フォームが再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。

以下は、 *Create.cshtml*チュートリアルの前半でスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

コードの使用に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティ。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッド。 これら 2 つのヘルパー メソッドがコント ローラーで、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 適切なモデルと表示のエラー メッセージに指定された検証属性に自動的に検索します。

コント ローラーもビュー テンプレートの作成を知っているもの、または特定のエラー メッセージが表示されますが適用されている実際の検証ルールに関する、このアプローチのすばらしい点です。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。

後で検証ロジックを変更する場合は、正確に 1 か所で行うようことができます。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 また、これは DRY 原則に完全に従うことを意味します。

## <a name="adding-formatting-to-the-movie-model"></a>ムービー モデルへの書式設定を追加します。

*Movie.cs* ファイルを開きます。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間は、組み込みの検証属性セットだけでなく、書式設定属性を提供します。 適用、 [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性と[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドに列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

また、明示的に設定できます、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。 次のコードは日付の書式設定文字列とリリースの日付プロパティ (つまり、"d")。 リリース日の一部として、時間にするないを指定するのにには、これを使用します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

次のコードの形式、`Price`通貨としてのプロパティ。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

完全な`Movie`クラスを次に示します。

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

アプリケーションを実行しを参照、`Movies`コント ローラー。

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。

> [!div class="step-by-step"]
> [前へ](adding-a-new-field.md)
> [次へ](improving-the-details-and-delete-methods.md)
