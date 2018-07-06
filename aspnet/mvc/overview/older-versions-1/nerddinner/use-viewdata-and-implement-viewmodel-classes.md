---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: ViewData を使用し、ViewModel クラスの実装 |Microsoft Docs
author: microsoft
description: 手順 6 の表示には、シナリオの編集より豊富なフォームのサポートが有効にする方法もコント ローラーからビューにデータを渡すために使用できる 2 つの方法を説明しています:.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 70b264def11b55fc08165dda307ff9e82fd4be7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826960"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>ViewData を使用し、ViewModel クラスを実装
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 6 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 6 の表示には、シナリオの編集より豊富なフォームのサポートが有効にする方法もコント ローラーからビューにデータを渡すために使用できる 2 つの方法を説明しています: ViewData とビューモデルです。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner 手順 6: ViewData とビューモデル

多くのフォーム post のシナリオを対象として実装する方法について説明しました作成、更新、削除 (CRUD) のサポート。 DinnersController の実装をさらにかかるようになりましたされ高度なフォームのシナリオの編集のサポートを有効にします。 これを行う際にコント ローラーからビューにデータを渡すために使用できる 2 つの方法について説明します。 ViewData とビューモデルです。

### <a name="passing-data-from-controllers-to-view-templates"></a>コント ローラーからビュー テンプレートにデータを渡す

厳密な"関心の分離"MVC パターンの定義特性の 1 つは、アプリケーションの異なるコンポーネント間を適用できるようにします。 モデル、コント ローラーとビューの各適切に定義のロールと職務と適切に定義された方法で互いの間で通信します。 これにより、テストの容易性を昇格させるし、コードの再利用できます。

クライアントに、HTML 応答を表示するために、コント ローラー クラスが決定したらは、応答を表示するために必要なすべてのデータのビュー テンプレートを明示的に渡す責任を負います。 ビュー テンプレートでは、– データの取得やアプリケーション ロジックを実行することはありませんし、代わりに、自体は、コント ローラーによって渡されたモデル/データ駆動するレンダリング コードのみを制限する必要があります。

今すぐモデル データ、DinnersController で渡されたテンプレートを表示するクラスは単純であり簡単 – の場合は、Index() Dinner オブジェクトの一覧、および 1 つの Dinner オブジェクト Details()、Edit()、Create() および Delete() の場合。 アプリケーションに UI 機能を追加すると、多くの場合、ここ、ビュー テンプレート内での HTML 応答を表示するためにこのデータだけを渡す必要があります。 たとえば、dropdownlist に HTML をテキスト ボックスの中からビューを作成、編集内の"Country"フィールドを変更する必要な可能性があります。 ハード コード国名ビュー テンプレートでのドロップダウン リストではなく動的に設定しますが、サポートされている国の一覧からそれを生成する可能性があります。 Dinner オブジェクトを渡す方法は必要があります*と*ビュー テンプレートに、コント ローラーからサポートされている国の一覧。

そのためにできる 2 つの方法を見てみましょう。

### <a name="using-the-viewdata-dictionary"></a>ViewData 辞書を使用します。

コント ローラーの基本クラスは、コント ローラーからビューを他のデータ項目を渡すために使用できる"ViewData"ディクショナリ プロパティを公開します。

たとえば、dropdownlist に HTML をテキスト ボックスから編集ビューで、「国」テキスト ボックスを変更しシナリオをサポートするために更新できます (Dinner オブジェクト) だけでなく、m として使用できる SelectList オブジェクトを渡す、Edit() アクション メソッド国の dropdownlist のデルします。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

上記の SelectList のコンス トラクターでは、郡を使用すると、ドロップ ダウン リストを設定すると、現在選択されている値の一覧を受け付けてください。

Html.DropDownList() ヘルパー メソッドを使用して、以前に使いました Html.TextBox() ヘルパー メソッドではなく、テンプレートの Edit.aspx 表示し、更新できます。

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

上記の Html.DropDownList() ヘルパー メソッドは、2 つのパラメーターを受け取ります。 最初は、出力を HTML フォーム要素の名前です。 2 つ目は、それは ViewData 辞書を使用して渡された"SelectList"モデルです。 使用、c#"キーワード as"を SelectList としてディクショナリ内で型をキャストします。

今すぐ実行するとき、アプリケーションとアクセスと、 */Dinners/Edit/1* URL、ブラウザー内で見て、テキスト ボックスの代わりに国の dropdownlist を表示、編集 UI が更新されたこと。

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

また、HTTP POST メソッドの編集 (でエラーが発生したときに使用する場合) から、テンプレートの編集ビューをレンダリングしますので、エラーのシナリオで、ビュー テンプレートが表示されるときに、SelectList を ViewData に追加するには、このメソッドも更新するかどうかを確認する必要があります。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

これで、DinnersController 編集シナリオが DropDownList をサポートしています。

### <a name="using-a-viewmodel-pattern"></a>ViewModel パターンを使用します。

ViewData 辞書のアプローチでは、非常に高速で簡単に実装されているのメリットがあります。 一部の開発者が気文字列ベースのディクショナリを使用して、入力ミスがコンパイル時にキャッチされず、エラー発生する可能性があるためです。 型指定されていない ViewData 辞書は、"as"演算子を使用して、またはキャスト ビュー テンプレートで c# のような厳密に型指定された言語を使用する場合にも必要です。

使用して別のアプローチでは、"ViewModel"パターンと呼ばれます。 このパターンを使用する場合、特定のビューのシナリオ用に最適化されたと、ビュー テンプレートで必要な動的値/コンテンツ プロパティを公開する厳密に型指定されたクラスを作成します。 コント ローラー クラスを設定し、使用するには、このビュー テンプレートにこれらのビューに最適化されたクラスを渡します。 これにより、タイプ セーフ、コンパイル時のチェック、およびビュー テンプレート内のエディターの intellisense。

たとえば、厳密に型指定された 2 つのプロパティを公開する dinner フォーム"DinnerFormViewModel"を作成編集シナリオは次のようなクラスを有効にする: Dinner オブジェクト、および国の dropdownlist を設定するために必要な SelectList モデル。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

リポジトリから取得する Dinner オブジェクトを使用して DinnerFormViewModel を作成する、Edit() アクション メソッドを更新して、ビュー テンプレートに渡します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

よう edit.aspx ページの上部にある"inherits"属性を変更することでオブジェクトのため、その it"Dinner"ではなく"DinnerFormViewModel"が必要ですが、ビュー テンプレートを更新し、作成します。

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

このビュー テンプレート内の"Model"プロパティの intellisense を渡している DinnerFormViewModel 型のオブジェクト モデルを反映するように更新されます。

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

これを使用するコードの表示、私たちを更新できます。 ここでは作成方法入力要素の名前を変更しない次の通知 (フォーム要素がある名前は"Title"、「国」) – DinnerFormViewModel クラスを使用して値を取得する HTML ヘルパー メソッドを更新していますが。

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

エラーを表示するときに、DinnerFormViewModel クラスを使用して、編集の post メソッドも更新します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

今後も更新できる、Create() のアクション メソッドを再利用、正確な同じ*DinnerFormViewModel* DropDownList 内も国を有効にするクラス。 HTTP GET の実装を次に示します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

作成する HTTP POST メソッドの実装を次に示します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

国を選択するため、両方の編集および作成画面がドロップダウン リストをサポートするようになりました。

### <a name="custom-shaped-viewmodel-classes"></a>カスタム型の ViewModel クラス

上記のシナリオでは、DinnerFormViewModel クラスは SelectList モデル プロパティのサポートと共に、プロパティとして Dinner のモデル オブジェクトを直接公開します。 この方法で正しく動作のシナリオで、ビュー テンプレートで作成する HTML UI に対応して比較的密接にドメイン モデル オブジェクト。

シナリオではない場合は、使用できる 1 つのオプションではビュー – によって消費のオブジェクト モデルがより最適化されており、基になるドメイン モデル オブジェクトとはまったく異なるになりますにカスタムの形 ViewModel クラスを作成します。 たとえば、さまざまなプロパティ名や複数のモデル オブジェクトから収集した集計のプロパティを公開、でした可能性があります。

カスタム型の ViewModel クラスは、両方をコント ローラーからビューをレンダリングするだけでなく、コント ローラーのアクション メソッドにポストバックされたフォーム データの処理に役立つデータを渡すために使用します。 この後のシナリオでは、アクション メソッドで、フォーム ポストされたデータを ViewModel オブジェクトを更新し、ViewModel インスタンスを使用して、マップまたは実際のドメイン モデル オブジェクトを取得するがあります。

ViewModel クラスのカスタムの形では、大量の柔軟性を指定できるほかは複雑すぎる取得を開始しています、アクション メソッド内で、テンプレートの表示またはフォーム ポスト コード内でレンダリング コードを検索する任意の時点を調査するものです。 これは、多くの場合、記号、ドメイン モデルが生成すると、UI に対応していないクリーンにして、カスタムの形の中間の ViewModel クラスが役立つのです。

### <a name="next-step"></a>次の手順

これで、パーシャルとマスター ページを使用すると、再利用し、アプリケーション全体で UI を共有した方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [次へ](re-use-ui-using-master-pages-and-partials.md)
