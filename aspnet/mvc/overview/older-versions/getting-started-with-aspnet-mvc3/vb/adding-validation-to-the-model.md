---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: 検証 (VB) モデルを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 86058530aa00ecbc00aeebc6ed7b5cf019fdad72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872924"
---
<a name="adding-validation-to-the-model-vb"></a>検証 (VB) モデルを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-validation-to-the-model.md)このチュートリアルのです。


検証ロジックをここで追加、`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。

## <a name="keeping-things-dry"></a>ドライに保つこと

ASP.NET MVC の中核となる設計の基本思想の 1 つは、ドライ (「しないことを繰り返さない」) です。 ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。 これは、量が減り、コードを記述する必要があります、コードが維持するために非常に簡単に記述する操作を行います。

ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。 (モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、その後、それらのルールは、アプリケーションで everywhere 執行します。

ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。

## <a name="adding-validation-rules-to-the-movie-model"></a>ムービー モデルに検証規則を追加します。

いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。

開く、 *Movie.vb*ファイル。 追加、`Imports`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

名前空間は、.NET Framework の一部です。 すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。

更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性. 属性を適用する場所の例として、次のコードを使用します。

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

検証属性では、適用対象のモデル プロパティに適用する動作を指定します。 `Required`属性は、プロパティは値である必要がありますを示します。 このサンプルでは、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、と`Price`有効にするにはプロパティです。 `Range` 属性は、指定した範囲内に値を制限します。 `StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。

コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。 次のコードが例外をスローするなど、ときに、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティ値が不足していると、価格が (0 では、有効な範囲外です)。

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。 また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。

ここでは、完全なコードが、更新されたリスト*Movie.vb*ファイル。

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>ASP.NET MVC では、UI の検証エラー

アプリケーションを再実行しに移動し、 */Movies* URL。

をクリックして、**作成の映画**リンクを新しいムービーを追加します。 いくつかの無効な値にフォームに記入し、**作成**ボタンをクリックします。

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

フォームが自動的に使用方法、背景色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。 エラー メッセージは、注釈を付けるときに指定したエラーの文字列を検索、`Movie`クラスです。 (ユーザーは、JavaScript が無効になっている) 場合に、エラーはクライアント側の JavaScript を使用して) とサーバー側の両方が適用されます。

実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.vbhtml* UI この検証を有効にするために表示します。 コント ローラーと自動的にこのチュートリアルで先ほど作成したビューの選択、検証規則は、属性の使用を指定したことを`Movie`モデル クラス。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>作成の表示し、アクション メソッドを作成で発生する検証方法

コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。 [次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。 このチュートリアルで先ほどの作成から変更されていません。

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

最初のアクション メソッドには、初期作成フォームが表示されます。 2 つ目は、フォーム ポストを処理します。 2 番目`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。 このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。 オブジェクトに検証エラーがある場合、`Create`メソッドには、フォームが再表示します。 エラーがない場合、メソッドはデータベースに新しいムービーを保存します。

以下は、 *Create.vbhtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。 これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。 呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。 これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。 これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。

このアプローチのすばらしい点は、コント ローラーもビュー テンプレートの作成を知っているものが適用されている実際の検証規則の情報や、特定のエラー メッセージが表示されてです。 検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。

検証ロジックを後で変更する場合は、これを行う 1 つの場所にします。 アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。 これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。 また、これは DRY 原則に完全に従うことを意味します。

## <a name="adding-formatting-to-the-movie-model"></a>書式設定、ムービーのモデルを追加します。

開く、 *Movie.vb*ファイル。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。 適用する、 [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性と[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。 次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

代わりに、明示的に設定できます、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。 次のコードは、日付の書式設定文字列のリリース日付プロパティを示しています (つまり、"d") です。 リリース日の一部として、時間にするないを指定するのにには、これを使用します。

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

次のコードの形式、`Price`通貨としてのプロパティです。

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

完全な`Movie`クラスを次に示します。

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

アプリケーションを実行しを参照、`Movies`コント ローラー。

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

シリーズの次の部分で、アプリケーションを確認し、いくつかの改善を自動的に生成されたおを`Details`と`Delete`メソッド.

> [!div class="step-by-step"]
> [前へ](adding-a-new-field.md)
> [次へ](improving-the-details-and-delete-methods.md)
