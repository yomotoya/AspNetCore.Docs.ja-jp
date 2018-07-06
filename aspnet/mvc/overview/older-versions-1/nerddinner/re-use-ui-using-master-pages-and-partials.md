---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: マスター ページとパーシャルを使用して UI を再利用 |Microsoft Docs
author: microsoft
description: 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去する、ビュー テンプレート内で 'DRY 原則' を適用できる方法で検索します。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839595"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>マスター ページとパーシャルを使用して UI を再利用します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 7 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 7 では、部分ビュー テンプレートおよびマスター ページを使用して、コードの重複を除去する、ビュー テンプレート内で「DRY 原則」適用可能な方法で検索します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner 手順 7: パーシャルとマスター ページ

ASP.NET MVC を採用する設計哲学の 1 つは"実行しない Repeat Yourself"原則です (「ドライ」とも呼ばれます)。 コードと、最終的には、アプリケーションを構築する迅速かつ容易に管理するロジックの重複の排除ドライ設計に役立ちます。

NerdDinner シナリオのいくつか適用 DRY 原則を既に説明しました。 いくつかの例: 両方の編集に適用され、コント ローラーでシナリオを作成できるように、モデル レイヤー内で、検証ロジックが実装されています。編集、詳細、削除のアクション メソッド間で、"NotFound"のビュー テンプレートを再利用する、View() ヘルパー メソッドを呼び出すと、名前を明示的に指定する必要がなくなりますビュー テンプレートを取得するには、使用規則の名前付けパターンを使用していること両方の編集用 DinnerFormViewModel クラスを再利用はおアクション シナリオを作成します。

これで方法を見てみましょう「DRY 原則」を適用できますもあるコードの重複を除去する、ビュー テンプレート内で。

### <a name="re-visiting-our-edit-and-create-view-templates"></a>再度、編集にアクセスして、ビュー テンプレートを作成

現在は 2 つの異なるビュー テンプレート –"Edit.aspx"と"Create.aspx"– 使用、Dinner フォーム UI を表示します。 うち visual 簡単に比較するには、どのように類似しているが強調表示されます。 作成フォームの外観を次に示します。

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

ここでは、[編集] フォームがどのように.

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

ほどの違いはありますか。 タイトルとヘッダーの文字列以外のフォーム レイアウトと入力コントロールは同じです。

テンプレートの表示を検索します"Edit.aspx"と"Create.aspx"を開く場合と同じフォームのレイアウトと入力コントロールのコードが含まれます。 この重複回避の意味を導入した場合です。 新しい Dinner プロパティの変更またはたびに 2 回変更を行うことができ上がります。

### <a name="using-partial-view-templates"></a>部分ビュー テンプレートの使用

ASP.NET MVC では、ページのサブ部分ビューのレンダリング ロジックをカプセル化に使用できる「部分的なビュー」テンプレートを定義する機能をサポートします。 「パーシャル」ビューのレンダリング ロジックを 1 回、定義する便利な方法を提供し、再に複数の場所で、アプリケーション全体でします。

「ドライ アップ」、Edit.aspx と Create.aspx ビュー テンプレートの複製のため、フォームのレイアウトと両方に共通する入力要素をカプセル化する"DinnerForm.ascx"をという名前の部分ビュー テンプレートを作成できます。 これを/ビュー/Dinners ディレクトリを右クリックし、選択は、"追加-&gt;ビュー"メニュー コマンド。

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

これにより、「ビューの追加」ダイアログが表示されます。 という名前を"DinnerForm"を作成するには、ダイアログ ボックスで"部分ビューを作成する チェック ボックスをオンし、私たちが合格する、DinnerFormViewModel クラスを示すために、新しいビュー。

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

[追加] ボタンをクリックすると、ときに Visual Studio は新しい"DinnerForm.ascx"ビュー テンプレートを"\Views\Dinners"ディレクトリ内を作成します。

私たちできますし、コピー/貼り付け、重複するフォームのレイアウト/新しい"DinnerForm.ascx"部分ビュー テンプレートに、Edit.aspx/Create.aspx ビュー テンプレートからのコントロールのコードを入力します。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

私たち DinnerForm 部分的なテンプレートを呼び出すし、フォームの重複を排除して、編集および作成のビュー テンプレートを更新できます。 Html.RenderPartial("DinnerForm") 呼び出し元によって、ビュー テンプレート内ではことができます。

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Html.RenderPartial を呼び出すときに使用する一部のテンプレートのパスを明示的に修飾することができます (例: ~ Views/Dinners/DinnerForm.ascx")。 ただし、上記コードで ASP.NET MVC、内の規則に基づく名前付けパターンを活用とをレンダリングする部分の名前として"DinnerForm"を指定するだけはいます。 この作業を行う ASP.NET MVC は検索規則に基づく views ディレクトリに最初 (DinnersController Dinners/ビュー/になります)。 部分的なテンプレートが見つからない場合がありますを検索して/Views/Shared ディレクトリにします。

Html.RenderPartial() は部分ビューの名前だけで呼び出されると、ASP.NET MVC はビューに渡す部分的な呼び出し元のビュー テンプレートで使用される同じのモデルと ViewData 辞書のオブジェクト。 またはに、別のモデル オブジェクトや使用する部分ビューの ViewData 辞書を渡すことができるようにする Html.RenderPartial() のオーバー ロードされたバージョンがあります。 これは、場所のみをする完全なモデルとビューモデルのサブセットを渡す場合に便利です。

| **側のトピックは、なぜ&lt;%%&gt;の代わりに&lt;% = %&gt;でしょうか。** |
| --- |
| 使用されているは、上記のコードでお気付き微妙な点の 1 つ、 &lt;%%&gt;ブロックの代わりに、 &lt;% = %&gt; Html.RenderPartial() を呼び出すときにブロックします。 &lt;% = %&gt; ASP.NET 内のブロックは、開発者が指定された値を表示することを示します (例: &lt;% =「こんにちは」%&gt; 「こんにちは」となる)。 &lt;%%&gt;ブロックを示す代わりに、開発者がコードを実行する必要があること、およびそれらに含まれる出力が表示されたし、明示的に実行する必要があります (例: &lt;%response.write("hello")&gt;します。 使用している理由を&lt;%%&gt;上記 Html.RenderPartial コードを持つブロックが Html.RenderPartial() メソッドが、文字列を返さないため、代わりに、呼び出し元のビュー テンプレートに直接コンテンツのストリームの出力を出力します。 これは、パフォーマンスの効率向上のため、により、一時的な (場合によっては非常に大きな可能性があります) の文字列オブジェクトを作成する必要を回避できます。 これにより、メモリ使用量を削減し、アプリケーション全体のスループットを改善します。 1 つのよくある間違い内にある場合、呼び出しの最後にセミコロンを追加することを忘れ Html.RenderPartial() を使用する場合、 &lt;%%&gt;ブロックします。 たとえば、このコードでは、コンパイラ エラーが発生: &lt;%html.renderpartial("dinnerform")&gt;代わりに書き込む必要があります: &lt;Html.RenderPartial("DinnerForm"); %&gt;ため、これは&lt;%%&gt;ブロックは、自己完結型のコード ステートメント (C#) ステートメントをセミコロンで終了する必要があるコードを使用する場合。 |

### <a name="using-partial-view-templates-to-clarify-code"></a>部分ビュー テンプレートを使ってコードを明確にするには

複数の場所でビューのレンダリング ロジックを重複しないようにする"DinnerForm"部分ビュー テンプレートを作成しました。 これは、部分ビュー テンプレートを作成する最も一般的な理由です。

場合によってまだ理にかなってを 1 か所でのみ呼び出すされている場合でも、部分ビューを作成します。 テンプレートの表示が非常に複雑な多くの場合、ずっとときと、ビューのレンダリング ロジックの抽出し 1 つにパーティション分割またはよりも名前付き部分のテンプレートを読みやすくなります。

たとえば、(これは、私たちは見てすぐ) プロジェクトで Site.master ファイルからのコード スニペットの下。 コードは比較的簡単読む – ログイン/ログアウトを表示するためのロジックが上部にあるリンクのため、部分的に画面の右側は"LogOnUserControl"部分内にカプセル化します。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

自分で混乱を見つける場合にしようとしてビュー テンプレート内の html コードとマークアップを理解、明確抽出され、適切な名前の部分ビューへのリファクタリングがその一部、かどうかを検討してください。

### <a name="master-pages"></a>マスター ページ

ASP.NET MVC には、部分ビューをサポートしているだけでなく、一般的なレイアウトと最上位レベルの html サイトの定義に使用できる「マスター ページ」テンプレートを作成する機能もサポートしています。 コンテンツのプレース ホルダーの後に上書きまたはビューによって「入力」できる置き換え可能な領域を識別するためにマスター ページのコントロールを追加できます。 これは、アプリケーション全体で共通のレイアウトを適用する非常に効果的な (とドライ) 方法を提供します。

新しい ASP.NET MVC プロジェクトでは既定では、マスター ページ テンプレートが自動的に追加することがあります。 このマスター ページは、"Site.master"と \Views\Shared\ フォルダー内に存在する名前は。

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

既定の Site.master ファイルは、次のようになります。 上部のナビゲーションのためのメニューと、サイトの外部の html を定義します。 1 つは、タイトルをその他のページの主要なコンテンツを置き換える必要があります – 2 つの置き換え可能コンテンツ プレース ホルダー コントロールが含まれています。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

すべての ("List"、"Details"、「編集」、「作成」、"NotFound"など) は、NerdDinner アプリケーション用に作成したビュー テンプレートは、この Site.master テンプレートに基づくされてがあります。 これは既定では、上部に追加された"MasterPageFile"属性によって示されます&lt;ページ % @ %&gt;ディレクティブ「ビューの追加」ダイアログを使用してビューを作成したときに。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

この意味であり、Site.master のコンテンツを変更することができますが、変更に自動的に適用し、ビュー テンプレートのいずれかのレンダリング時に使用します。

アプリケーションのヘッダーが"My MVC Application"ではなく"NerdDinner"なるように、Site.master のヘッダー セクションを更新しましょう。 最初のタブが"を検索、Dinner"(HomeController の Index() アクション メソッドによって処理されます)、および"ホストを Dinner"(DinnersController の Create() アクション メソッドによって処理されます) と呼ばれる新しいタブを追加してみましょうように、ナビゲーション メニューも更新してみましょう。

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Site.master ファイルと更新を保存して、ヘッダーを見て、ブラウザーは、アプリケーション内のすべてのビューでまで表示を変更します。 例えば:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

使用して、 */Dinners/編集/[id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>次の手順

パーシャルとマスター ページは、クリーンにビューを整理するための非常に柔軟なオプションを提供します。 ビューを重複しないようにするのに役立ちますコンテンツ/コードし、テンプレートの表示を簡単に読み取りおよびメンテナンスが見つかります。

みましょう今すぐ前に作成した一覧のシナリオの見直しし、スケーラブルなページングのサポートを有効にします。

> [!div class="step-by-step"]
> [前へ](use-viewdata-and-implement-viewmodel-classes.md)
> [次へ](implement-efficient-data-paging.md)
