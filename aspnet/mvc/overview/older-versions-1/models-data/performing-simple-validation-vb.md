---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: 単純な検証 (VB) を実行する |Microsoft Docs
author: StephenWalther
description: ASP.NET MVC アプリケーションで検証を実行する方法について説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして検証 HTML ヘルパーについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 26900229630b25affe21a5bb801ef0247711d26b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399462"
---
<a name="performing-simple-validation-vb"></a>単純な検証 (VB) を実行します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> ASP.NET MVC アプリケーションで検証を実行する方法について説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして、検証 HTML ヘルパーについて説明します。


このチュートリアルの目的では、ASP.NET MVC アプリケーション内での検証を実行する方法について説明します。 たとえば、だれかが必要なフィールドの値を含まないフォームを送信するようにする方法について説明します。 モデルの状態と検証の HTML ヘルパーを使用する方法について説明します。

## <a name="understanding-model-state"></a>モデルの状態について

モデルの状態、またはより正確には、モデル状態ディクショナリを使用する検証エラーを表します。 たとえば、リスト 1 で Create() アクションでは、Product クラスをデータベースに追加する前に Product クラスのプロパティを検証します。


コント ローラーに、検証またはデータベース ロジックを追加することは推奨していませんいます。 コント ローラーには、アプリケーション フローの制御に関連するロジックのみを含める必要があります。 単純化へのショートカットを取得しています。


**1 - Controllers\ProductController.vb を一覧表示します。**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

リストの 1 では、Product クラスの名前、説明、および UnitsInStock プロパティが検証されます。 これらのプロパティのいずれかの検証テストが失敗した場合、エラーは、(コント ローラー クラスの ModelState プロパティによって表される)、モデル状態ディクショナリに追加されます。

モデル状態エラーがある場合、ModelState.IsValid プロパティは false を返します。 その場合は、新しい製品を作成するための HTML フォームが再表示されます。 それ以外の場合、検証エラーがない場合は、新しい製品がデータベースに追加されます。

## <a name="using-the-validation-helpers"></a>検証ヘルパーの使用

ASP.NET MVC フレームワークには、2 つの検証ヘルパーが含まれています: Html.ValidationMessage() ヘルパーと Html.ValidationSummary() ヘルパー。 検証エラー メッセージを表示するのにには、ビューのこれら 2 つのヘルパーを使用します。

Html.ValidationMessage() と Html.ValidationSummary() ヘルパーは、ASP.NET MVC のスキャフォールディングによって自動的に生成される Create と Edit ビューで使用されます。 作成ビューを生成するこれらの手順に従います。

1. 製品のコント ローラーで Create() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 1 参照)。
2. **ビューの追加**ダイアログ ボックスで、というラベルの付いたチェック ボックスをオン**厳密に型指定されたビューを作成する**(図 2 参照)。
3. **データ クラスを表示**ドロップダウン リストで、Product クラスを選択します。
4. **コンテンツを表示**ドロップダウン リストで、作成を選択します。
5. **[追加]** ボタンをクリックします。


ビューを追加する前にアプリケーションをビルドすることを確認します。 それ以外の場合、クラスの一覧に表示されません、**データ クラスを表示**ドロップダウン リスト。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**図 01**: ビューの追加 ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image2.png))。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**図 02**: 厳密に型指定されたビューを作成する ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image4.png))。


次の手順を完了すると、リスト 2 でビューを作成するを取得します。

**2 - Views\Product\Create.aspx を一覧表示します。**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

リストの 2 では、HTML フォームの上、Html.ValidationSummary() ヘルパーが直ちに呼び出さします。 このヘルパーを使用して、検証エラー メッセージの一覧を表示します。 Html.ValidationSummary() ヘルパーは、箇条書きリストにエラーを表示します。

Html.ValidationMessage() ヘルパーは HTML フォームのフィールドの横にある呼び出されます。 このヘルパーを使用して、フォーム フィールドのすぐ隣に、エラー メッセージを表示します。 リストの 2 の場合、Html.ValidationMessage() ヘルパーでは、エラーがある場合にアスタリスクが表示されます。

図 3 のページは、不足しているフィールドと無効な値で、フォームが送信されたときに、検証ヘルパーによって表示されるエラー メッセージを示しています。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**図 03**: 問題を含めて提出して、作成ビュー ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image6.png))。


HTML の外観は、検証エラーがある場合にもフィールドが変更された入力に注意してください。 Html.TextBox() ヘルパーのレンダリングを*クラス =「の入力検証エラー」* Html.TextBox() ヘルパーによって表示されるプロパティに関連付けられている属性の検証エラーがある場合。

検証エラーの外観の制御に使用される 3 つのカスケード スタイル シート クラスがあります。

- 適用される入力の検証エラー -、&lt;入力&gt;タグ Html.TextBox() ヘルパーによってレンダリングされます。
- 適用されるフィールドの検証エラー -、 &lt;span&gt;タグ Html.ValidationMessage() ヘルパーによってレンダリングされます。
- 適用される検証の概要-エラー -、 &lt;ul&gt;タグ Html.ValidationSumamry() ヘルパーによってレンダリングされます。

これらカスケード スタイル シートのクラスを変更し、コンテンツのフォルダーにある Site.css ファイルを変更することでそのため、検証エラーの外観を変更できます。

> [!NOTE] 
> 
> HtmlHelper クラスには、CSS を関連する検証の名前を取得する読み取り専用の静的プロパティが含まれますクラス。 これらの静的プロパティは、ValidationInputCssClassName、ValidationFieldCssClassName、および ValidationSummaryCssClassName 名前です。


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding 検証と Postbinding の検証

製品を作成するための HTML フォームを送信する、[price] フィールドと UnitsInStock フィールドの値に無効な値を入力すると、図 4 に表示される検証メッセージを取得します。 これらの検証エラー メッセージからどのようになるでしょうか。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**図 04**: Prebinding 検証エラー ([フルサイズの画像を表示する をクリックします](performing-simple-validation-vb/_static/image8.png))。


実際に 2 種類の検証エラー メッセージの HTML フォームのフィールドは、クラスにバインドされ、フォームのフィールドがクラスにバインドされている後に生成された前に生成されたものがあります。 つまり、あるは検証エラーを prebinding と postbinding 検証エラー。

リスト 1 で製品のコント ローラーによって公開される Create() アクションは、Product クラスのインスタンスを受け入れます。 Create メソッドのシグネチャのようになります。

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

作成フォームから HTML フォーム フィールドの値は、モデル バインダーと呼ばれるもので、productToCreate クラスにバインドされます。 既定のモデル バインダー エラー メッセージに追加モデルの状態に自動的にフォーム フィールド、フォームのプロパティにバインドできないときにします。

既定のモデル バインダーは、Product クラスの価格のプロパティに文字列"apple"をバインドできません。 文字列は、10 進数のプロパティに割り当てることはできません。 そのため、モデル バインダーでは、モデルの状態をエラーを追加します。

既定のモデル バインダーことはできませんを割り当てることも、値 Nothing はない値をそのまま何もするプロパティ。 具体的には、モデル バインダーに代入できません値 Nothing UnitsInStock プロパティ。 ここでも、モデル バインダーを放棄し、モデルの状態をエラー メッセージを追加します。

エラー メッセージを prebinding これらの外観をカスタマイズする場合は、これらのメッセージのリソース文字列を作成する必要があります。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC フレームワークでの検証の基本的なしくみを説明することでした。 モデルの状態と検証の HTML ヘルパーを使用する方法を学習しました。 Prebinding と検証を postbinding の違いを説明します。 他のチュートリアルでは、コント ローラーからモデル クラスに検証コードを移動するためのさまざまな方法について説明します。

> [!div class="step-by-step"]
> [前へ](displaying-a-table-of-database-data-vb.md)
> [次へ](validating-with-the-idataerrorinfo-interface-vb.md)
