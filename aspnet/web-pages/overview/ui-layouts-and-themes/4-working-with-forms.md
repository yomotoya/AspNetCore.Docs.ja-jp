---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: ASP.NET Web Pages (Razor) のサイトでの HTML フォームの使用 |Microsoft ドキュメント
author: tfitzmac
description: フォームは、テキスト ボックス、チェック ボックス、ラジオ ボタン、およびプルダウンで表示されるリストと同様に、ユーザー入力コントロールを配置する HTML ドキュメントのセクションです。 フォームを使用して修正しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) のサイトでの HTML フォームの使用
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> (テキスト ボックスとボタン) を持つ、HTML フォームを処理する方法を説明する ASP.NET Web Pages (Razor) の web サイトで作業しているときにします。
> 
> **学習する内容。** 
> 
> - HTML フォームを作成する方法。
> - フォームからユーザーの入力を読み取る方法です。
> - ユーザー入力を検証する方法。
> - ページの送信後に、フォームの値を復元する方法です。
> 
> これらは、ASP.NET のアーティクルで導入された概念をプログラミングします。
> 
> - `Request` オブジェクト。
> - 入力の検証。
> - HTML エンコード。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


## <a name="creating-a-simple-html-form"></a>単純な HTML フォームを作成します。

1. 新しい web サイトを作成します。
2. ルート フォルダーの作成という名前の web ページ*Form.cshtml*し、次のマークアップを入力します。

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. お使いのブラウザーでページを起動します。 (WebMatrix での**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。次の 3 つの入力フィールドを持つ単純な形式と**送信**ボタンが表示されます。

    ![次の 3 つのテキスト ボックス、フォームのスクリーン ショット。](4-working-with-forms/_static/image1.jpg)

    この時点をクリックした場合、**送信**ボタンを何も起こりません。 フォームを便利にするために、サーバーで実行されるコードを追加する必要があります。

## <a name="reading-user-input-from-the-form"></a>フォームからユーザーの入力を読み取る

フォームを処理するには、送信されたフィールドの値を読み取り、それらに何かコードを追加します。 この手順では、フィールドを読み取りおよび ページで、ユーザー入力を表示する方法を示します。 (実稼働アプリケーションで一般的に処理を行うより興味深いユーザー入力をします。 行いますをデータベースの使用についての情報の記事です。)

1. 上部にある、 *Form.cshtml*ファイルを次のコードを入力します。

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    ユーザーは、最初のページを要求、空のフォームのみが表示されます。 (される)、ユーザーが、フォームに入力しをクリックして**送信**です。 これが提出 (投稿) をサーバーにユーザー入力します。 既定では、要求は同じページに移動 (つまり、 *Form.cshtml*)。

    この時点で、ページを送信するときに、フォーム上で入力した値が表示されます。

    ![入力した、ページに表示される値を示すスクリーン ショット。](4-working-with-forms/_static/image2.jpg)

    ページのコードを確認します。 最初に使用する、`IsPost`ページがポストされるかどうかを調べます&#8212;、かどうか、ユーザーがクリックされた、**送信**ボタンをクリックします。 これは、投稿場合`IsPost`は true を返します。 これは、最初の要求 (GET 要求など) またはポストバック (POST 要求) で作業しているかどうかを決定する標準的な方法では、ASP.NET Web ページです。 (GET と POST の詳細についてを参照してください"HTTP GET と POST と IsPost Property"で[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost))。

    次に、ユーザーから入力される値を取得する、`Request.Form`して、オブジェクトに置く変数後にします。 `Request.Form`オブジェクトには、キーによって識別される各ページに送信されたすべての値が含まれています。 キーは、該当するショートカットを`name`読みたいフォーム フィールドの属性です。 例については、読み取り、`companyname`フィールド (テキスト ボックス) を使用する`Request.Form["companyname"]`です。

    フォームの値が格納されている、`Request.Form`オブジェクトを文字列として。 そのため、数値、日付、またはその他の種類と値を使用する場合、その型への文字列から変換する必要があります。 例では、`AsInt`のメソッド、 `Request.Form` (を従業員数を含む) の従業員のフィールドの値を整数に変換するために使用します。
2. お使いのブラウザーでページを起動して、フォーム フィールドの入力 をクリックして**送信**です。 ページには、入力した値が表示されます。

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML エンコーディングの外観およびセキュリティ
> 
> HTML などの文字の特殊な用途を持つ`<`、 `>`、および`&`です。 これらの特殊文字が表示されませんに想定している場合、ことができますに支障をきたす、web ページの機能と外観です。 たとえば、ブラウザーでは、解釈、`<`と同様に、HTML 要素の先頭として (ない場合に空白が続いて) を文字`<b>`または`<input ...>`です。 始まる文字列を単に破棄ブラウザー認識しない場合、要素、`<`に達するまで何かをもう一度と認識します。 当然ながら、ページの奇妙ないくつか表示になります。
> 
> HTML エンコードすると、これら予約文字をブラウザーが、適正なシンボルとして解釈するコードに置き換えます。 たとえば、`<`文字で置換されます`&lt;`と`>`文字で置換されます`&gt;`です。 ブラウザーは、置換文字列を表示する文字として表示されます。
> 
> HTML エンコード文字列を表示する任意の時間を使用する (入力)、ユーザーから取得したことをお勧めすることをお勧めします。 ない場合は、ユーザーことができます、web ページに悪意のあるスクリプトを実行したり、別のものは、サイトのセキュリティを侵害するか、望ましくないだけであるを取得しようとします。 (これは、ユーザー入力を受け取り、場所、保管およびし、後で表示する場合に特に重要&#8212;など、コメント、ユーザーのレビュー、またはそのようなものです)。
> 
> 自動的に ASP.NET Web Pages、これらの問題を防ぐために役立つを HTML エンコード任意のテキスト コンテンツをコードから出力します。 たとえば、変数や式などを使用してコードの内容を表示する`@MyVar`、ASP.NET Web Pages は、出力を自動的にエンコードします。


## <a name="validating-user-input"></a>ユーザー入力の検証

ユーザーは間違いです。 フィールドに入力するように依頼することを忘れると、または従業員の数を入力するように依頼し、代わりに名前を入力します。 で、フォームが満杯になった正しく処理する前に確認するため、ユーザーの入力を検証します。

この手順では、ユーザーがありませんでした空白のままになっていることを確認するすべての 3 つのフォーム フィールドを検証する方法を示します。 確認することも、従業員の数の値が数値であります。 エラーがある場合は、エラーを表示するが、ユーザーにどのような値を示すメッセージが検証に合格しませんでした。

1. *Form.cshtml*ファイルでコードの最初のブロックを次のコードに置き換えます。 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    使用するユーザー入力を検証する、`Validation`ヘルパー。 呼び出して必要なフィールドを登録する`Validation.RequireField`です。 呼び出して他の種類の検証を登録する`Validation.Add`を検証するフィールドと、実行する検証の種類を指定することです。

    ページの実行時に ASP.NET では、すべての検証します。 呼び出して結果を確認できます`Validation.IsValid`、すべてのものが渡された場合は true が返されます、任意のフィールド検証に失敗した場合は false。 通常、呼び出す`Validation.IsValid`ユーザー入力にどのような処理を実行する前にします。
2. 更新プログラム、`<body>`要素を 3 回の呼び出しを追加することによって、`Html.ValidationMessage`次のように、メソッド。

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    検証エラー メッセージを表示するには、Html を呼び出すことができます。`ValidationMessage` そのメッセージを使用するフィールドの名前。
3. ページを実行します。 フィールドを空白のままにし、をクリックして**送信**です。 エラー メッセージが表示されます。

    ![ユーザー入力が検証に合格しなかったかどうかに表示されたエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image3.jpg)
4. 文字列 ("ABC"など) を追加、**従業員数**フィールドし、をクリックして**送信**もう一度です。 この時間のことを示すエラーが表示で、適切な形式、つまり、整数、文字列はありません。

    ![ユーザーが従業員フィールドの文字列を入力するかどうかに表示されたエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image4.jpg)

ASP.NET Web ページは、自動的にユーザーがブラウザーで即時のフィードバックを取得できるようにクライアント スクリプトを使用して検証を実行する機能など、ユーザー入力を検証するためのより多くのオプションを提供します。 参照してください、[その他のリソース](#Additional_Resources)詳細については後述します。

## <a name="restoring-form-values-after-postbacks"></a>ポストバックの後のフォーム値を復元します。

前のセクションで、ページをテストするときにお気付きする場合は、検証エラーが発生、(無効なデータだけでなく) を入力したすべてのものがなくなり、およびすべてのフィールドの値を再入力する必要がありました。 これは、重要な点を示しています。 ページが最初から再作成するとページを送信、処理、もう一度ページを表示、します。 説明したように含まれていたページが送信されたときに任意の値が失われることになります。

これを修正する簡単に、ただしです。 送信された値へのアクセスがある (で、`Request.Form`オブジェクト、ページが表示される場合はフォームのフィールドにそれらの値を入力することができます。

1. *Form.cshtml*ファイルで置き換えます、`value`の属性、`<input>`要素を使用して、`value`属性。 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value`の属性、`<input>`要素が動的な読み取りのフィールドの値に設定されて、`Request.Form`オブジェクト。 内の値、最初に、ページが要求される、`Request.Form`オブジェクトがすべて空にします。 これは、しますこのようにして、フォームは空白なので。
2. お使いのブラウザー内のページを起動、フォーム フィールドの入力または、空白のままにし、 をクリックして**送信**です。 送信された値を表示するページが表示されます。

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

- [Web ユーザーからの入力を取得する方法は 1,001](https://msdn.microsoft.com/library/ms971057.aspx)
- [フォームを使用して、ユーザー入力の処理](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [ASP.NET Web ページにおけるユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)
- [HTML フォームでのオートコンプリートの使用](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
