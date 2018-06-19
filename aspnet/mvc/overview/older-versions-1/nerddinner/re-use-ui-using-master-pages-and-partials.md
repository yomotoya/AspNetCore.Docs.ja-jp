---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: マスター ページとパーシャルを使用して UI を再利用 |Microsoft ドキュメント
author: microsoft
description: 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去するには、当社ビュー テンプレート内で 'ドライ原則' を適用した方法で検索します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870012"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>マスター ページとパーシャルを使用して UI を再利用します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 7 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去するには、当社ビュー テンプレート内で「ドライ原則」を適用した方法で検索します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner 手順 7: パーシャルとマスター ページ

ASP.NET MVC 持ち込デザインの理念の 1 つは、「はいない繰り返さない」原則 (「ドライ」とも呼ばれます) です。 ドライ デザインは、コードおよびアプリケーションを作成する迅速かつ容易に管理するため、最終的にはロジックの重複を解消します。

NerdDinner シナリオのいくつか適用性、既に説明しました。 いくつかの例: 両方の編集に適用され、コント ローラーでシナリオを作成できるように、モデル レイヤー内で、検証ロジックを実装再を使用する、"NotFound"のビュー テンプレート メソッド間で、編集、詳細、および Delete アクションです。View() ヘルパー メソッドを呼び出すと、名前を明示的に指定する必要があるビュー テンプレートを取得するには、規則の名前付けパターンを使用します。両方の編集用 DinnerFormViewModel クラスを再利用し、アクションのシナリオを作成します。

今すぐ方法を見てみましょう「ドライ原則」を適用したことができますもあるコードの重複を排除するには、当社ビュー テンプレート内で。

### <a name="re-visiting-our-edit-and-create-view-templates"></a>再度、編集を訪問し、テンプレートの表示を作成します。

現在は 2 つの異なるビュー テンプレート –"Edit.aspx"と"Create.aspx"– 使用 Dinner フォーム UI を表示します。 これらのビジュアル簡単に比較するは、類似性が強調表示されます。 作成するフォームの外観を次に示します。

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

"Edit"フォームは次のようになります。

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

異なる量はありますか。 タイトルとヘッダーの文字列以外は、フォームのレイアウトおよび入力コントロールが同じです。

ビューのテンプレートを検索します"Edit.aspx"と"Create.aspx"を開く場合と同じフォームのレイアウトおよび入力コントロールのコードが含まれます。 この重複は最終的におを導入したり、適切ではない - 新しい Dinner プロパティを変更するたびに 2 回変更することを意味します。

### <a name="using-partial-view-templates"></a>部分ビューのテンプレートを使用します。

ASP.NET MVC には、ページのサブ部分ビューのレンダリング ロジックをカプセル化に使用できる「部分的なビュー」テンプレートを定義する機能がサポートされています。 「パーシャル」1 回、ビューのレンダリング ロジックを定義する便利な方法を提供し、再で使用して複数の場所、アプリケーション全体でします。

「ドライ アップ」、Edit.aspx および Create.aspx ビュー テンプレートの複製のため、フォームのレイアウトと両方に共通の入力要素をカプセル化"DinnerForm.ascx"をという名前の部分ビュー テンプレートを作成できます。 それでは、当社/ビュー/ディナー ディレクトリを右クリックして を選択して、"追加-&gt;ビュー"メニュー コマンド。

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

これにより、「ビューの追加」ダイアログが表示されます。 新しいビューお"DinnerForm"を作成、ダイアログ ボックスに「部分ビューの作成」チェック ボックスをオンにしてを渡していること、DinnerFormViewModel クラスを示すという名前を。

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

「追加」ボタンをクリックすると Visual Studio は新しい"DinnerForm.ascx"ビュー テンプレートを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。

おできますし、コピー/貼り付け、重複するフォームのレイアウトを新しい"DinnerForm.ascx"部分ビュー テンプレートに、Edit.aspx/Create.aspx ビュー テンプレートからのコントロールのコードを入力/。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

編集および作成するテンプレートの表示、DinnerForm 部分的なテンプレートを呼び出すし、フォームの重複を排除するし、更新することができます。 We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Html.RenderPartial を呼び出すときに使用する部分的なテンプレートのパスを明示的に修飾することができます (例: ~ Views/Dinners/DinnerForm.ascx") です。 ただし、コードでは、上、ASP.NET MVC 内の規則に基づく名前付けパターンの利用と表示するために一部の名前として"DinnerForm"を指定するだけがおです。 この作業を行うときに ASP.NET MVC ビューの規約に基づくディレクトリに最初 (を検索 DinnersController/ビュー/ディナーとなります)。 テンプレートの部分的なが見つからない場合がありますは探します、/Views/Shared ディレクトリにします。

Html.RenderPartial() は部分ビューの名前だけで呼び出されると、ASP.NET MVC に渡されます部分ビュー、同じモデルと ViewData 辞書オブジェクト、呼び出し元のビュー テンプレートで使用されます。 代わりに、オーバー ロードされたバージョンがの Html.RenderPartial()、別のモデル オブジェクトや、部分ビューを使用するための ViewData ディクショナリを渡すことができるようにします。 これは、のみしようとする場合にフル モデル/ViewModel のサブセットを渡すに役立ちます。

| **側トピックは、なぜ&lt;%%&gt;の代わりに&lt;% = %&gt;しますか?** |
| --- |
| 使用されていることに気付きます上記のコードで軽度のいずれかが、 &lt;%%&gt;ブロックの代わりに、 &lt;% = %&gt; Html.RenderPartial() を呼び出す場合は、ブロックします。 &lt;% = %&gt;開発者が、指定した値を表示するためには、ASP.NET でのブロックを示す (例: &lt;% =「こんにちは」%&gt; 「こんにちは」と表示)。 &lt;%%&gt;ブロックを示す代わりに、開発者がコードを実行する場合、そのいずれかによって、それらに含まれる出力が表示されることし、明示的に行う必要があります (例: &lt;%response.write("hello")&gt;です。 使用する理由、 &lt;%%&gt;上記 Html.RenderPartial コード ブロックでは Html.RenderPartial() メソッドは、文字列を返さないし、代わりに、呼び出し元のビュー テンプレートに直接コンテンツのストリームの出力を出力します。 これは、パフォーマンス効率向上のため、により、一時的な (場合によっては非常に大きい可能性があります) の文字列オブジェクトを作成する必要性を回避できます。 これにより、メモリ使用率を削減し、アプリケーション全体のスループットを向上させます。 1 つの一般的なミス内にある場合、呼び出しの最後に、セミコロンを追加することを忘れがちを Html.RenderPartial() を使用する場合、 &lt;%%&gt;ブロックします。 たとえば、このコードでは、コンパイラ エラーが発生: &lt;%html.renderpartial("dinnerform")&gt;代わりに記述する必要があります: &lt;Html.RenderPartial("DinnerForm"); %library;&gt;これは、ため&lt;%%&gt;ブロックは、自己完結型のコード ステートメント (C#) ステートメントをセミコロンで終了する必要のあるコードを使用する場合とします。 |

### <a name="using-partial-view-templates-to-clarify-code"></a>部分ビューのテンプレートを使用したコードを明確にします。

複数の場所でビューのレンダリング ロジックと重複しないように、"DinnerForm"部分ビューのテンプレートを作成しました。 これは、部分ビューのテンプレートを作成する最も一般的な理由です。

場合によってまだ理にかなってを 1 か所でのみ呼び出すされている場合でも、部分ビューを作成します。 テンプレートの表示が非常に複雑な多くの場合がの場合、ビューのレンダリング ロジック抽出し 1 つにパーティション分割またはよりも名前付き部分のテンプレートを読みやすくなります。

たとえば、(ここでする検索は間もなく) をプロジェクトに、Site.master ファイルからのコード スニペットの下。 コードが比較的簡単を読み取る – 一部のログインとログアウトを表示するためのロジックが上部にあるリンクは、画面の右側は"LogOnUserControl"部分内にカプセル化します。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

混乱気づいたらされるたびにしようとしてビュー テンプレート内の html/コード マークアップを理解を検討してくださいかどうかしなければならない場合は抽出され、適切な名前の部分ビューへのリファクタリングの一部がより明確です。

### <a name="master-pages"></a>マスター ページ

ASP.NET MVC には、部分ビューをサポートしているだけでなく、サイトの最上位の html および一般的なレイアウトを定義するために使用する「マスター ページ」テンプレートを作成する機能もサポートしています。 コンテンツの上書きまたはビューによって「設定」できる置き換え可能な領域を識別するマスター ページにコントロールを追加することができますし、プレース ホルダーです。 これにより、非常に効果的な (およびドライ) をアプリケーション全体で共通のレイアウトを適用します。

新しい ASP.NET MVC プロジェクトでは既定では、それらに自動的に追加のマスター ページ テンプレートが存在します。 このマスター ページは、"Site.master"および \Views\Shared\ フォルダー内での生活にという名前は。

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

既定の Site.master ファイルは、次のようになります。 上部のナビゲーション メニューと共に、サイトの外部の html を定義します。 2 つの置き換え可能コンテンツ プレース ホルダー コントロール – タイトル、およびその他のページの主要なコンテンツを置き換える必要がありますのいずれかが含まれています。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

NerdDinner アプリケーション ("List"、「詳細」、"Edit"、「作成」、"NotFound"など) を作成しましたビュー テンプレートのすべては、この Site.master テンプレートに基づくされています。 これは、最上位に既定で追加された"MasterPageFile"属性を使用して表示&lt;% ページ % @&gt;ディレクティブ「ビューの追加」ダイアログを使用して、ビューを作成したとき。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

これが意味であり、Site.master コンテンツを変更することができますが、変更に自動的に適用し、これらのビューのテンプレートのいずれかのレンダリング時に使用します。

アプリケーションのヘッダーが"My MVC Application"ではなく"NerdDinner"ようにしましょう、Site.master のヘッダー セクションを更新します。 みましょうも更新して、ナビゲーション メニューように最初のタブは、「を検索、夕食」(HomeController の Index() アクション メソッドによって処理されます)、および"ホストを Dinner"(DinnersController の Create() アクション メソッドによって処理されます) と呼ばれる新しいタブを追加してみましょう。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Site.master ファイルと更新を保存して、ブラウザー、ヘッダーを見てみましょうは、アプリケーション内のすべてのビューで表示を変更します。 例えば:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

使用して、 */Dinners/編集/[id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>次の手順

パーシャルおよびマスター ページは、クリーンにビューを整理する非常に柔軟なオプションを提供します。 ビューと重複しないようにするのに役立ちますコンテンツ/コードし、テンプレートの表示を容易に読み取りおよびメンテナンスがあります。

みましょう今すぐ前に作成した一覧のシナリオを再確認し、スケーラブルなページングのサポートを有効にします。

> [!div class="step-by-step"]
> [前へ](use-viewdata-and-implement-viewmodel-classes.md)
> [次へ](implement-efficient-data-paging.md)
