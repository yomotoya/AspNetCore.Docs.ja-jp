---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: 使用して ViewData と実装 ViewModel クラス |Microsoft ドキュメント
author: microsoft
description: '手順 6 に示す方法、シナリオの編集より豊富なフォームのサポートを有効にして、コント ローラーからビューにデータを渡すために使用する 2 つの方法についても説明: しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>使用して ViewData と実装 ViewModel クラス
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 6. ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 6 に示しますがどの高度なフォームが、シナリオの編集のサポートを有効にし、コント ローラーからビューにデータを渡すために使用する 2 つの方法についても説明: ViewData と ViewModel です。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner 手順 6: ViewData と ViewModel

フォーム ポスト シナリオの数を説明および実装する方法を説明した作成、更新および削除 (CRUD) のサポート。 うまく DinnersController 実装がさらにかかるようになりましたしシナリオの編集より豊富なフォームのサポートを有効にします。 これを行う際に紹介するコント ローラーからビューにデータを渡すために使用する 2 つの方法: ViewData と ViewModel です。

### <a name="passing-data-from-controllers-to-view-templates"></a>コント ローラーからテンプレートを表示するデータの受け渡し

MVC パターンの定義特性の 1 つは、厳密な"関心の分離"アプリケーションのさまざまなコンポーネント間で適用すると役に立ちます。 モデル、コント ローラーとビューの各適切に定義の役割と責任を定義し、適切に定義された方法で互いの間で通信します。 これにより、テストの容易性を昇格させるし、コードの再利用できます。

クライアントへの HTML 応答を表示するために、コント ローラー クラスが決定したら、それは、応答を表示するために必要なすべてのデータ ビューのテンプレートを明示的に渡すこと担当します。 表示テンプレートは、データの取得やアプリケーション ロジック – を実行する必要がありますしないおよび自体を渡し、コント ローラーのモデル/データ駆動するレンダリング コードのみを制限する必要があります代わりにします。

今すぐモデル データ、DinnersController によって渡されるテンプレートを表示するクラスは単純であり簡単 – の場合は、Index() Dinner オブジェクトの一覧、および単一 Dinner オブジェクト Details()、Edit()、Create() および Delete() 場合。 アプリケーションに UI 機能を追加して、多くの場合、しようとして、ビュー テンプレート内での HTML 応答を表示するためにこのデータだけを通過する必要があります。 たとえば、お可能性がある、編集内の"Country"フィールドを変更 dropdownlist、HTML テキスト ボックスの中からビューを作成します。 ハード コード ビュー テンプレートの国名のドロップダウン リストではなく動的設定はサポートされている国の一覧から生成たい場合があります。 Dinner オブジェクトを渡す方法には必要が*と*テンプレートを表示する、コント ローラーからサポートされている国の一覧です。

このためにはできる 2 つの方法を見てみましょう。

### <a name="using-the-viewdata-dictionary"></a>ViewData ディクショナリを使用してください。

コント ローラーの基本クラスは、コント ローラーからビューに渡される追加のデータ項目に使用できる"ViewData"辞書プロパティを公開します。

たとえば、お dropdownlist、HTML テキスト ボックスの中から、編集ビュー内で、「国」テキスト ボックスを変更するシナリオをサポートするために更新することがオブジェクトを渡します (Dinner オブジェクト) だけでなく SelectList m として使用できる、Edit() アクション メソッド国の dropdownlist の odel します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

上記の SelectList のコンス トラクターでは、郡、ドロップ ダウン リストを設定するだけでなく、現在選択されている値の一覧が受け入れされます。

以前を使用した Html.TextBox() ヘルパー メソッドではなく Html.DropDownList() ヘルパー メソッドを使用して、テンプレートの Edit.aspx 表示し、更新できます。

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

上記の Html.DropDownList() ヘルパー メソッドは、次の 2 つのパラメーターを受け取ります。 1 つ目は、出力を HTML フォーム要素の名前です。 2 つ目は、ViewData ディクショナリを使用して渡された"SelectList"モデルです。 おは c# を使用して、"as"キーワード、SelectList としてディクショナリ内の型にキャストします。

今すぐ実行するとき、マイクロソフトのアプリケーションとアクセスと、 */Dinners/Edit/1* URL、ブラウザー内で後ほどお見せをテキスト ボックスではなく国の dropdownlist の表示、編集の UI が更新されたこと。

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

また、HTTP POST メソッドの編集 (でエラーが発生したときに使用する場合) からテンプレートを編集ビューをレンダリングおために、私たちもビュー テンプレートは、エラーのシナリオでレンダリングするときに、SelectList を ViewData に追加するには、このメソッドが更新することを確認してくださいお必要があります。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

今すぐ DinnersController 編集に紹介したシナリオが DropDownList をサポートしています。

### <a name="using-a-viewmodel-pattern"></a>ViewModel パターンの使用

ViewData ディクショナリ方法には、非常に高速で簡単に実装されているの利点があります。 一部の開発者に満足できない文字列ベースのディクショナリを使用して、ので、入力ミスがコンパイル時ではキャッチされないエラーにつながることができます。 型指定されていない ViewData ディクショナリは、「と」演算子を使用してまたはキャスト ビュー テンプレート c# のような厳密に型指定された言語を使用する場合にも必要です。

使用できるその他の方法では、"ViewModel"パターンと呼ばれます。 このパターンを使用する場合、特定のビューのシナリオ用に最適化されていると、テンプレートの表示に必要な動的な値/コンテンツのプロパティを公開する厳密に型指定されたクラスを作成します。 コント ローラー クラスでは設定し、ビュー用に最適化されたこれらのクラスを使用するには、このビュー テンプレートに渡すことができます。 これにより、タイプ セーフ、コンパイル時のチェック、およびエディターの intellisense ビュー テンプレート内で。

例については、厳密に型指定された 2 つのプロパティを公開する dinner フォーム"DinnerFormViewModel"を作成できるよう編集のシナリオは以下のようにクラスを有効にする: Dinner オブジェクト、および国の dropdownlist の作成に必要な SelectList モデル。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

このリポジトリから取得するまでお Dinner オブジェクトを使用して DinnerFormViewModel を作成する、Edit() アクション メソッドを更新したり、ビュー テンプレートに渡すことできます。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Edit.aspx ページの上部にある"inherits"属性を変更することによってオブジェクトのため、その it"Dinner"ではなく"DinnerFormViewModel"が必要ですが、ビュー テンプレート更新次のように、します。

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

この作業を行う、「モデル」プロパティの表示テンプレート内での intellisense を渡している DinnerFormViewModel 型のオブジェクト モデルを反映するように更新されます。

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

これを使用する、コードの表示更新できます。 作成する方法は、入力要素の名前を変更しない下通知 (フォーム要素があるという名前のまま"Title"、「国」) – DinnerFormViewModel クラスを使用して値を取得する HTML ヘルパー メソッドを更新しましたが、します。

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

エラーを表示するときに、DinnerFormViewModel クラスを使用して、編集の post メソッドも更新されます。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

更新することが、Create() アクション メソッドを正確な再使用する同じ*DinnerFormViewModel*国 DropDownList 内部のも有効にするクラス。 HTTP GET の実装を次に示します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

HTTP POST 作成メソッドの実装を次に示します。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

国を選択するため、両方の編集および作成する画面がドロップダウン リストをサポートするようになりました。

### <a name="custom-shaped-viewmodel-classes"></a>ViewModel クラスのカスタムの形

上記のシナリオに DinnerFormViewModel クラスは直接サポート SelectList モデル プロパティと、プロパティとして Dinner モデル オブジェクトを公開します。 この方法は問題なく動作のシナリオが、ビュー テンプレート内で作成する HTML UI に対応する比較的密接にドメイン モデル オブジェクト。

シナリオではありません、使用できる 1 つのオプションは、– ビューによって消費のオブジェクト モデルがより最適化されており、これが検索する、基になるドメイン モデル オブジェクトとはまったく異なるにカスタムの形 ViewModel クラスを作成します。 たとえば、可能性のある公開される可能性が別のプロパティの名前や複数のモデル オブジェクトから収集した集計のプロパティです。

ViewModel クラスのカスタムの形は、両方を表示するだけでなく、コント ローラーのアクション メソッドにポストバックされるフォーム データの処理に役立つビューに、コント ローラーからデータを渡すために使用します。 後でこのシナリオでは、ViewModel オブジェクトをフォーム ポストされたデータで更新し、ViewModel インスタンスを使用して、マップまたは実際のドメイン モデル オブジェクトを取得、アクション メソッドがあります。

ViewModel クラスのカスタムの形では、柔軟性、大幅に向上を提供し、複雑すぎて取得を開始して、アクション メソッドの中のテンプレートの表示またはフォーム ポスト コード内でレンダリング コードを検索する任意の時間を調査するものでは。 これは、多くの場合、ドメイン モデルが生成するには、UI に対応していないクリーンにおよび、カスタムの形の中間の ViewModel クラスを支援できる記号です。

### <a name="next-step"></a>次の手順

これで、パーシャルおよびマスター ページを使用する再利用し、アプリケーション間で UI を共有お方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [次へ](re-use-ui-using-master-pages-and-partials.md)
