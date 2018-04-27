---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要 |Microsoft ドキュメント
author: tfitzmac
description: この付録概要を説明する ASP.NET Web pages でのプログラミングの Visual basic で、Razor 構文を使用します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: aad951a0e4344dbaafbdcc3b3980307a26fa75fc
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ここで、プログラミングの概要 ASP.NET Web Pages を Razor 構文と Visual Basic を使用します。 ASP.NET は、web サーバーでの動的な web ページを実行するための Microsoft のテクノロジです。
> 
> **学習する内容**:
> 
> - プログラミングの Razor 構文を使用して ASP.NET Web Pages のプログラミングの概要に関するヒントの最上位 8 です。
> - 基本的なプログラミング概念する必要があります。
> - どのような ASP.NET サーバー コードと Razor 構文に関するです。
>   
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


Razor 構文を使用する ASP.NET Web Pages を使用するほとんどの例は c# を使用します。 Razor 構文では、Visual Basic もサポートされています。 Visual Basic での ASP.NET web ページをプログラムで web ページを作成する、 *.vbhtml*ファイル名拡張子、し、Visual Basic コードを追加します。 この記事では、Visual Basic 言語と ASP.NET web ページを作成するための構文での作業の概要を示します。

> [!NOTE]
> Microsoft WebMatrix の既定の web サイト テンプレート (**ベーカリー**、**フォト ギャラリー**、および**スターター サイト**など) は c# および Visual Basic のバージョンで使用します。 NuGet パッケージとしてでは、Visual Basic テンプレートをインストールすることができます。 Web サイト テンプレートがという名前のフォルダーで、サイトのルート フォルダーにインストールされている*Microsoft テンプレート*です。


## <a name="the-top-8-programming-tips"></a>最上位の 8 プログラミングのヒント

このセクションでは、絶対に必要な Razor 構文を使用して ASP.NET サーバー コードの記述を開始すると、いくつかのヒントを示します。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.コードを使用してページを追加する、@ 文字

`@`文字は、インライン式、単一ステートメント ブロック、および複数ステートメントのブロックを開始します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

ブラウザーに表示される結果:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML エンコード**
> 
> ページを使用して、コンテンツを表示した場合、`@`文字、上記の例では、ASP.NET は、出力を HTML でエンコードします。 これには、HTML の予約された文字が置き換えられます (など`<`と`>`と`&`) コードを HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするとします。 HTML エンコードせず、サーバー コードからの出力は正しく表示されない場合があり、セキュリティ上のリスクにページを公開する可能性があります。
> 
> 目標は、マークアップとしてタグがレンダリングされる HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調する) を参照してください[結合のテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。
> 
> 詳細での HTML エンコードに関する[HTML フォームの ASP.NET Web Pages サイトで使用](https://go.microsoft.com/fwlink/?LinkId=202892)です。


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2.コードを使って、コード ブロックで囲むとしています.終了コード

コード ブロックが 1 つまたは複数のコード ステートメントが含まれ、キーワードで囲まれた`Code`と`End Code`です。 配置開始`Code`キーワード後すぐに、`@`文字&#8212;間それらを空白にすることはできませんがあります。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

ブラウザーに表示される結果:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3.ブロック内で改行するのには、各コード ステートメントを終了します。

Visual Basic のコード ブロックには、各ステートメントは、改行で終わります。 (記事の後半で必要な場合は、複数の行に長いコード ステートメントをラップする方法を参照します)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4.値を格納するのに変数を使用します。

内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`Dim`キーワード。 変数の値を挿入するにはページを使用して、直接`@`です。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

ブラウザーに表示される結果:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.リテラル文字列の値を二重引用符で囲みます。

A*文字列*テキストとして扱われる文字のシーケンスです。 文字列を指定するには、二重引用符で囲みます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

文字列値の中では二重引用符を埋め込むには、次の 2 つの二重引用符文字を挿入します。 ページ出力に 1 回出現する二重引用符文字を挿入する場合は、入力として`""`内で、引用符で囲まれた文字列、および 2 回表示する場合は入力として`""""`引用符で囲まれた文字列内で。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

ブラウザーに表示される結果:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6.Visual Basic のコードは、大文字小文字を区別はありません。

Visual Basic 言語では、大文字小文字を区別されません。 プログラミング キーワード (のように`Dim`、 `If`、および`True`) と変数名 (など`myString`、または`subTotal`) いずれの場合も記述できます。

次のコード行は、変数に値を割り当てる`lastname`小文字を使用して名前、および大文字の名前を使用して変数の値を出力します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

ブラウザーに表示される結果:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7.コーディングのほとんどには、オブジェクトによる操作が含まれます

オブジェクトをプログラミングで使用できることを表す&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求、電子メール メッセージ、顧客レコード (データベースの行) などです。オブジェクトの特性を記述するプロパティがある&#8212;テキスト ボックスのオブジェクトが、`Text`プロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージには、`From`プロパティ、および顧客オブジェクトが、 `FirstName`プロパティ。 オブジェクトでは、実行されるメソッドもがある、&quot;動詞&quot;それらを実行できます。 例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッドです。

使って作業を多くの場合、`Request`ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL (テキスト ボックスなど)、ページ上のフォームの値などの情報を提供するオブジェクトのフィールドです。この例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバー上のページの絶対パスを表示します。

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

ブラウザーに表示される結果:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.意思決定を行うコードを記述することができます。

動的な web ページの主な機能は、条件に基づき、新機能を決定できます。 これを行う最も一般的な方法は、`If`ステートメント (と省略可能な`Else`ステートメント)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

ステートメント`If IsPost`書き込みのためのショートハンド方法`If IsPost = True`です。 と共に`If`ステートメントはさまざまな方法は、コードのブロックを繰り返し、条件をテストするために、か、この記事の後半で説明されています。

結果がブラウザーで表示されます (クリックした後**送信**)。

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET と POST メソッドおよび IsPost プロパティ**
> 
> Web ページ (HTTP) に使用されるプロトコルは、メソッドの非常に限られた数をサポートしています (&quot;動詞&quot;) に、サーバー要求に使用されます。 2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用します。 一般に、ユーザーの要求 ページでは、最初に、ページが要求された GET を使用します。 かどうか、ユーザーは、フォームに入力して、クリックして**送信**ブラウザー、サーバーへの POST 要求。
> 
> Web プログラミング、かどうか、ページが要求されて、GET または POST として、ページの処理方法がわかるように、知っておくと役立ちますがしばしばあります。 ASP.NET Web Pages で行うこともできます、`IsPost`プロパティを要求が GET または POST があるかどうかを参照してください。 要求が、投稿の場合、`IsPost`プロパティが true の場合が返され、フォーム上のテキスト ボックスの値読み取りなどを行うことができます。 多くの例を確認する方法を示しますの値に応じて異なる方法でページを処理する`IsPost`です。


## <a name="a-simple-code-example"></a>単純なコード例

この手順では、基本的なプログラミング技術について説明するページを作成する方法を示します。 例では、それらを追加し、結果を表示し、2 つの数値を入力できるように、ページを作成します。

1. エディターで、新しいファイルを作成し、名前*AddNumbers.vbhtml*です。
2. ページで、ページ内の既存のものを置き換えるには、次のコードとマークアップをコピーします。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    ここでは、いくつかの点に注意してください。

    - `@`文字 ページで、コードの最初のブロックを開始して、前に指定する、`totalMessage`下部近くにある変数が埋め込まれています。
    - ページの上部にあるブロックを囲む`Code...End Code`です。
    - 変数`total`、 `num1`、 `num2`、および`totalMessage`いくつかの数字と文字列を格納します。
    - 割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内です。
    - Visual Basic コードにないため、大文字と小文字が区別すると、`totalMessage`ページの下部にある変数を使用、その名前は、ページの上部にある変数の宣言のスペルが一致するようにのみ必要があります。 大文字と小文字は関係ありません。
    - 式`num1.AsInt()`  +  `num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示します。 `AsInt`各変数のメソッドを追加できる整数 (整数)、ユーザーが入力した文字列に変換します。
    - `<form>`タグが含まれています、`method="post"`属性。 これを指定する、ユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用してサーバーに送信されます。 ページを送信するときに、コード`If IsPost`true に評価され、条件付きコードの実行、数値を加算した結果を表示します。
3. ページを保存し、ブラウザーで実行します。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic 言語と構文

ASP.NET web ページを作成する方法とサーバー コードを HTML マークアップを追加する方法の基本的な例は前述です。 ここで Visual Basic を使用して、Razor 構文を使用して ASP.NET サーバー コードを記述の基本を学習&#8212;プログラミング言語の規則では、します。

(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミングを使用した経験が場合、次を参照する新機能の多くについて理解されます。 習熟するのみでのマークアップに WebMatrix のコードを追加する方法が必要 *.vbhtml*ファイル。

### <a id="BM_CombiningTextMarkupAndCode"></a>  テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること

サーバー コード ブロックでは、多くの場合にしておくテキスト、およびページ マークアップを出力します。 サーバー コード ブロックのコードではないとする代わりに表示するかは、テキストが含まれている場合、ASP.NET をコードからそのテキストを識別できる必要があります。 これにはいくつかの方法があります。

- ような HTML ブロック要素内のテキストを囲みます`<p></p>`または`<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。 ASP.NET で開始 HTML タグがどのように表示される場合 (たとえば、 `<p>`)、要素をレンダリングすべてと、ブラウザー (および、サーバー コード式を解決) には、その内容として、します。

- 使用して、`@:`演算子または`<text>`要素。 `@:`プレーン テキストまたは一致しない HTML タグを含むコンテンツの 1 つの行が出力、`<text>`要素を出力する複数の行で囲みます。 これらのオプションは、出力の一部としての HTML 要素を表示したくない場合に役立ちます。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    次の例は、前の例を繰り返しますの 1 つのペアを使用して`<text>`タグをレンダリングするテキストを囲みます。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    次の例で、`<text>`と`</text>`タグがいくつか含まれていないテキストと一致しない HTML タグがあるすべての 3 つの行を囲みます (`<br />`)、およびサーバー コードと一致した HTML タグ。 もう一度、ごとに 1 行の前でしたも、`@:`演算子以外のいずれかの方法の動作です。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。 (ASP.NET はサーバーのコード式と末尾に挿入されるサーバー コード ブロックの出力はエンコード前に述べたよう`@`、このセクションで説明した特殊なケースでは可します)。

### <a name="whitespace"></a>Whitespace

ステートメントで (および、文字列リテラルの外部) 余分なスペースは、ステートメントに影響しません。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>長いステートメントを複数の行に分割

アンダー スコア文字を使用して、複数の行に長いコード ステートメントを中断できます`_`(Visual Basic では、呼び出される、*連結文字*) コードの各行の後にします。 次の行にステートメントを中断するには、行の末尾にスペースと連結文字を追加します。 次の行に、ステートメントを続行します。 読みやすさを向上させるために必要な数の行にステートメントをラップすることができます。 次のステートメントでは、同じです。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

ただし、文字列リテラルの途中で行をラップすることはできません。 次の例が機能しません。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

上記のコードのように複数の行に折り返す長い文字列を結合するには、まず必要がありますを使用する、*連結演算子*(`&`)、この記事で後述があります。

### <a name="code-comments"></a>コードのコメント

コメントでは、自分自身または他のユーザーのノートのままにできます。 Razor 構文のコメントの先頭が付いている`@*`で終わります`*@`です。

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

コード ブロック内でコメントを使用して、Razor 構文、または単一引用符は、通常の Visual Basic のコメント文字を使用することができます (`'`) の先頭行ごとに指定します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>変数

変数は、データの格納に使用する名前付きオブジェクトです。 名前を付ける変数、何もが、名前の先頭の文字は英字にする必要があり、空白文字または予約文字は使用できません。 Visual Basic では、前に説明したとおり、変数名の文字の大文字と小文字は関係ありません。

### <a name="variables-and-data-types"></a>変数とデータ型

変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。 文字列値を格納する文字列変数があることができます (のような&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付値を格納する date 型の変数). いて、その他の多くのデータ型を使用することができます。

ただし、変数の型を指定する必要はありません。 ほとんどの場合 ASP.NET は、変数内のデータの使用方法に基づいて型を見つけることができます。 (場合によっては、型を指定する必要があります。 これが true の例が表示されます。)

使用して型を指定せず、変数を宣言する`Dim`変数名を加えた (たとえば、 `Dim myVar`)。 型の変数を宣言するには使用`Dim`続けて、変数名を加えた`As`と型名では、(たとえば、 `Dim myVar As String`)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

次の例では、web ページの変数を使用するインライン式を示します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

ブラウザーに表示される結果:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>変換して、データ型のテスト

ASP.NET はこのデータ型を自動的に決定できますは通常があります、できません。 そのため、明示的な変換を実行して ASP.NET を手助けする必要があります。 型変換を持っていない場合でもいる場合もありますデータの種類が使用することを確認するためのテストに役立ちます。

最も一般的なケースでは、整数や日付など、他の型を文字列に変換していることです。 次の例は、文字列を数値に変換する必要があります、一般的な事例を示しています。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

原則として、ユーザー入力に文字列として。 ユーザーに、数値の入力を要求した場合でも、ユーザー入力が送信されたコードで読み取るときに、数字を入力したした場合でも、データは文字列の形式です。 そのため、文字列を数値に変換する必要があります。 例では、したり、変換せず、値に対して算術演算を実行しようとする場合、次のエラーにより、ASP.NET は、2 つの文字列を追加できないため。

`Cannot implicitly convert type 'string' to 'int'.`

呼び出すの値を整数に変換する、`AsInt`メソッドです。 変換が成功した場合は、数値を追加できます。

次の表は、変数の一般的ないくつかの変換とテスト方法を示します。


0x%2 行 0x%2 0x%2 列 0x%2<strong>メソッド</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>説明</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>例</strong>0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AsInt(), IsInt()` 0x%2 列終了 0x%2 0x%2 列 0x%2 数を示す整数を表す文字列に変換します (など&quot;593&quot;) 整数にします。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AsBool(), IsBool()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のように文字列に変換します&quot;true&quot;または&quot;false&quot; Boolean 型にします。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AsFloat(), IsFloat()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AsDecimal(), IsDecimal()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を 10 進数。 (ASP.NET では、10 進数は、浮動小数点数よりも正確です。)0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AsDateTime(), IsDateTime()` 0x%2 列終了 0x%2 0x%2 列 0x%2 を ASP.NET に日付と時刻の値を表す文字列を変換`DateTime`型です。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `ToString()` 0x%2 列終了 0x%2 0x%2 列 0x%2 の他の任意のデータ型を文字列に変換します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2


## <a name="operators"></a>演算子

演算子は、キーワードまたは ASP.NET を式の中で実行するコマンドの種類を示す文字です。 Visual Basic は、多くの演算子をサポートしていますが、ASP.NET web pages の開発を開始するいくつかを認識するだけです。 次の表は、最も一般的な演算子をまとめたものです。


0x%2 行 0x%2 0x%2 列 0x%2<strong>演算子</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>説明</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>例</strong>0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `+ - * /` 0x%2 列終了 0x%2 0x%2 列 0x%2 算術演算子が数値式で使用します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `=` 0x%2 列終了 0x%2 0x%2 列 0x%2 の割り当てと等しいかどうか。 コンテキストに応じて、左側にあるオブジェクトにステートメントの右側にある値を割り当てますか、等しいかどうかの値をチェックします。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `<>` 0x%2 列終了 0x%2 0x%2 列 0x%2 非等値。 返します`True`値が等しくない場合。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `< > <= >=` 0x%2 列終了 0x%2 0x%2 列 0x%2 よりも小さい、大きい、小さいよりまたは等しい、およびより大きいまたは等しい。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `&` 0x%2 列終了 0x%2 0x%2 列 0x%2 連結、文字列を結合するために使用します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `+= -=` 0x%2 列終了 0x%2 0x%2 列 0x%2、インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `.` 0x%2 列終了 0x%2 0x%2 列 0x%2 ドットします。 オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `()` 0x%2 列終了 0x%2 0x%2 列 0x%2 かっこです。 、グループ式にパラメーターを渡すメソッド、および配列およびコレクションのメンバーにアクセスするために使用します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `Not` 0x%2 列終了 0x%2 0x%2 列 0x%2 されません。 False またはその逆は真の値を反転させます。 通常、テストするための短縮形方法として使用`False`(用には、いない`True`)。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2
* * *
0x%2 行 0x%2 0x%2 列 0x%2 `AndAlso OrElse` 0x%2 列終了 0x%2 0x%2 列 0x%2 論理的かつ条件をまとめてリンクを使用すると、します。
0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    0x%2 列終了 0x%2 0x%2 行の終わり 0x%2

## <a name="working-with-file-and-folder-paths-in-code"></a>ファイルとフォルダーのパス コードでの操作

コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。 開発用コンピューターで表示されるように、web サイトの物理的なフォルダー構造の例を次に示します。

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Url およびパスの重要な詳細を次に示します。

- URL がいずれかで、ドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。
- URL は、ホスト コンピューター上の物理パスに対応します。 たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバーにします。
- 仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。 ドメインまたはサーバー名に続く URL の部分が含まれています。 仮想パスを使用する場合は、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。

相違点の把握に役立つ例を次に示します。

| 完全な URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| サーバー名 | *mycompanyserver* |
| [仮想パス] | */humanresources/CompanyPolicy.htm* |
| 物理パス | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

仮想ルートは、/、ドライブは、c: のルートと同じように \ です。 (仮想フォルダーのパス常にフォワード スラッシュを使用します。)フォルダーの仮想パスは、物理フォルダー; として同じ名前である必要はありません。エイリアスがあります。 (運用サーバーで仮想パスほとんどと一致する正確な物理パスです。)

コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを扱う場合によっては、仮想パスを参照する必要があります。 ASP.NET では、これらのツールでコード ファイルとフォルダーのパスを操作するため:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッドです。

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>物理仮想パスに変換する: Server.MapPath メソッド

`Server.MapPath`メソッドは、仮想パスを変換 (と同様に */default.cshtml*) への絶対物理パス (と同様に*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 このメソッドを使用すると、いつでも、完全な物理パスを作成する必要がありますにします。 典型的な例は、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成するときです。

通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、このメソッドは、パスを変換できるようにするかが分かって — 仮想パス: サーバー上の対応するパスにします。 ファイルまたはフォルダーをメソッドに仮想パスを渡すし、物理パスを返します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>仮想ルートを参照している: ~ 演算子と Href メソッド

*.Cshtml*または *.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。 これは、機能は、サイトでは、のページを移動することができ、他のページに含まれているすべてのリンクが壊れているされませんので非常に便利です。 これまで、web サイトを別の場所に移動する場合にも便利です。 次にいくつかの例を示します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

場合は、web サイトは`http://myserver/myapp`、方法 ASP.NET が扱うこれらのパス、ページの実行時に次に示します。

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(変数の値としてこれらのパスは実際には表示されませんが、ASP.NET は、パスとして扱うは何でした)。

使用することができます、 `~` (上記と同じ) のサーバー コードと次のように、マークアップの両方の演算子。

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

使用するマークアップで、`~`オペレーターをイメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。 ASP.NET がページ (コードとマークアップの両方) を検索し、すべて解決、ページの実行時に、`~`適切なパスへの参照。

## <a name="conditional-logic-and-loops"></a>条件付きロジック、ループ

ASP.NET サーバー コード タスクを実行できます条件とループを実行しているステートメントは、コードの指定の回数を繰り返し実行する書き込みコードに基づく)。

### <a name="testing-conditions"></a>条件をテストします。

使用する単純な条件をテストする、`If...Then`ステートメントでは、返す`True`または`False`を指定するテストに基づきます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If`キーワード、ブロックを開始します。 実際のテスト (条件) に依存して、`If`キーワードと true または false を返します。 `If`文の末尾に`Then`です。 テストが true の場合に実行するステートメントが囲ま`If`と`End If`です。 `If`ステートメントを含めることができます、`Else`ブロックで、条件が false の場合に実行するステートメントを指定します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

場合、`If`ステートメントには、コード ブロックが開始され、通常を使用する必要はありません`Code...End Code`ブロックを含めるようにステートメントをします。 追加するだけ`@`ブロックにも、正しく機能します。 このアプローチに`If`だけでなく他の Visual Basic の含むコード ブロックが続くキーワードをプログラミング`For`、 `For Each`、 `Do While`, などです。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

1 つまたは複数を使用して複数の条件を追加する`ElseIf`ブロック。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

最初の条件の場合、この例では、`If`ブロックが真ではありません、`ElseIf`条件をチェックします。 その条件が満たされた場合内のステートメント、`ElseIf`ブロックが実行されます。 条件も満たさない場合、内のステートメント、`Else`ブロックが実行されます。 任意の数を追加する`ElseIf`をブロックしてを閉じて、`Else`としてブロック、&quot;他のすべて&quot;条件。

多くの条件をテストするには、使用、`Select Case`ブロック。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

テストする値は、(例では、weekday 変数) のかっこ内です。 各テストを使用して、`Case`ステートメントが、値を一覧表示されます。 場合の値、`Case`ステートメントはそのコードのテスト値に一致`Case`ブロックが実行されます。

ブラウザーに表示される最後の 2 つの条件付きブロックの結果:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>コードのループ

多くの場合、同じステートメントを繰り返し実行する必要があります。 これを行うをループします。 たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。 使用することが正確に何回かループすることがわかっている場合、`For`ループします。 このようなループ カウントまたはカウント ダウンの際に特に役立ちます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

ループはで始まり、`For`キーワード、3 つの要素に続きます。

- 直後に、`For`カウンター変数を宣言するステートメントでは、(を使用する必要はありません`Dim`) ように、範囲を示す`i = 10 to 20`です。 つまり、変数`i`10 にカウントを開始および 20 (包括) に到達するまで続行されます。
- 間、`For`と`Next`ステートメントは、ブロックのコンテンツ。 これは、1 つまたは複数のコード ステートメントを実行する各ループが発生を含めることができます。
- `Next i`ステートメントはループを終了します。 カウンターをインクリメントし、ループの次のイテレーションを開始します。

コードの間の行、`For`と`Next`行には、ループのイテレーションごとに実行されるコードが含まれています。 マークアップは、新しい段落を作成する (`<p>`要素) の値を表示する出力に行を追加し、それぞれ i (カウンター)。 このページを実行すると、例は、11 行の行のテキスト項目数を示す行ごとに、出力の表示を作成します。

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

コレクションまたは配列を扱う場合場合、多くの場合、使用する、`For Each`ループします。 コレクションは、類似のオブジェクトのグループと`For Each`ループに、コレクション内の各項目にタスクを実行することができます。 この種類のループは、コレクションの便利なためとは異なり、`For`ループすることも、カウンターをインクリメントまたは制限を設定します。 代わりに、`For Each`が終了するまで、このループのコードは、単にコレクションを続行されます。

この例は、内の項目を返します、 `Request.ServerVariables` (コレクションを web サーバーに関する情報が含まれています)。 使用して、`For Each`を新規作成して各アイテムの名前を表示するループ`<li>`HTML 箇条書きリスト内の要素。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each`キーワードの後に、コレクション内の 1 つの項目を表す変数 (例では、 `myItem`) と、その後、`In`キーワードのコレクションをループするか後にします。 本体で、`For Each`ループ、先ほど宣言した変数を使用する現在のアイテムにアクセスすることができます。

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

汎用的なループを作成するには、使用、`Do While`ステートメント。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

このループはで始まり、`Do While`状態は、後に、キーワードの後に、ブロックを繰り返します。 ループが通常インクリメント (に追加する) または減分 (減算) 変数またはオブジェクト カウントで使用されます。 この例で、`+=`演算子に 1 を加算する変数の値、ループが実行されるたびにします。 (カウント ダウンをループ内の変数をデクリメントするためには、デクリメント演算子を使用すると`-=`)。

## <a name="objects-and-collections"></a>オブジェクトとコレクション

ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。 このセクションでは、演習では多くの場合、コードでいくつかの重要なオブジェクトについて説明します。

### <a name="page-objects"></a>ページのオブジェクト

ASP.NET の最も基本的なオブジェクトは、ページです。 オブジェクトを修飾せずに直接 page オブジェクトのプロパティにアクセスすることができます。 次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

プロパティを使用することができます、`Page`など多くの情報を取得するオブジェクト。

- `Request`。 既に説明したようにこれは、ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL を含む、現在の要求に関する情報のコレクション。
- `Response`。 これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。 たとえば、このプロパティを使用すると、情報を応答に書き込みます。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>コレクション オブジェクト (配列とディクショナリ)

コレクションのコレクションなど、同じ種類のオブジェクトのグループは、`Customer`データベースからのオブジェクト。 同様に ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。

多くの場合、コレクション内のデータを操作します。 2 つの共通コレクション型とは、*配列*と*ディクショナリ*です。 配列は、類似した項目のコレクションを格納するたくない個別の各項目を保持する変数を作成するときに便利です。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

配列、特定のデータ型を宣言するなど`String`、 `Integer`、または`DateTime`です。 変数が配列を含めることができます、宣言で変数名にかっこを追加することを示すために (など`Dim myVar() As String`)。 アイテムの位置 (インデックス) を使用して配列またはを使用してアクセスできます、`For Each`ステートメントです。 配列のインデックスは 0 から始まる&#8212;は、最初の項目がある位置 0、2 番目の項目位置は 1 というようにします。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

取得することによって、配列内のアイテムの数を指定できます、`Length`プロパティです。 配列内の特定の項目の位置を取得する (つまり、配列を検索します)、使用、`Array.IndexOf`メソッドです。 行うことができますも逆方向のような配列の内容 (、`Array.Reverse`メソッド) するか、内容を並べ替える (、`Array.Sort`メソッド)。

ブラウザーで表示されている文字列配列のコードの出力:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

設定または対応する値を取得するキー (または名前) を指定したキー/値ペアのコレクションをディクショナリには。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

使用するディクショナリを作成する、`New`を新規作成しているかを示すキーワード`Dictionary`オブジェクト。 変数を使用して、ディクショナリを割り当てることができます、`Dim`キーワード。 かっこを使用してディクショナリ内の項目のデータ型を指定する ( `( )` )。 宣言の最後に、これは、実際には、メソッド、新しいディクショナリを作成するために、別のかっこのペアを追加してください。

ディクショナリに項目を追加するに呼び出せる、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。 または、かっこを使用して、キーが示され、次の例のように、単純な代入を実行することができます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

をディクショナリから値を取得するには、かっこ内にキーを指定します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>パラメーターを持つメソッドを呼び出す

この記事の前半で説明したとおりにプログラム オブジェクトには方法があります。 たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッドです。 多くのメソッドには、1 つまたは複数のパラメーターがあります。 A*パラメーター*メソッドに渡す値は、そのタスクを完了する方法を有効にします。 たとえばの宣言を見て、`Request.MapPath`を 3 つのパラメーターを受け取るメソッド。

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

このメソッドは、指定した仮想パスに対応するサーバーの物理パスを返します。 次の 3 つのパラメーター、メソッドは、 `virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`です。 (宣言では、パラメーターが表示されることを承諾するデータのデータ型とに注意してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。

メソッドにパラメーターを渡すための 2 つのオプションを Visual Basic を使用して、Razor 構文を使用している場合がある:*位置指定パラメーター*または*名前付きパラメーター*です。 位置指定パラメーターを使用して、メソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。 (通常わかりますこの順序メソッドのドキュメントを読み取ることによってです。)、順序に従う必要があります、パラメーターのいずれかをスキップすることはできませんと&#8212;かどうか必要に応じて、空の文字列を渡す (`""`) またはの値がない位置指定パラメーターに null です。

次の例は、という名前のフォルダーがある前提としています。*スクリプト*、web サイトにします。 コードの呼び出し、`Request.MapPath`メソッドとパスの値が正しい順序で 3 つのパラメーターです。 結果のマップされたパスが表示されます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

メソッドの多くのパラメーターが存在する場合、維持する、コード クリーナーより読みやすい名前付きパラメーターを使用しています。 名前付きパラメーターを使用してメソッドを呼び出すには、続けてパラメーター名を指定`:=`し、値を指定します。 名前付きパラメーターの利点は、任意の順序で追加できることです。 (欠点は、メソッドの呼び出しがコンパクトではないことです。)

次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターを使用します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

ご覧のように、異なる順序でパラメーターが渡されます。 ただし、前の例、この例を実行する場合は、同じ値を返すがあります。

## <a name="handling-errors"></a>エラー処理

### <a name="try-catch-statements"></a>Try-catch ステートメント

ステートメントは、上の理由から、コントロールの外部できない可能性があるコードの多くの場合があります。 例えば:

- コードは、開く、作成、読み取り、またはファイルの書き込みを試みると、あらゆる種類のエラーが表示される場合があります。 目的のファイルが存在しない可能性があります、ロックされる可能性があります、コード可能性がありますいないアクセス許可を持つし、なります。
- 同様に、コードでは、データベース内のレコードを更新しようとすると、アクセス許可の問題があります、データベースへの接続を削除する場合があります、無効な場合とに、データを保存する場合があります。

プログラミング用語では、このような状況が呼び出されます*例外*です。 場合に、コードには、例外が発生すると、(スロー) を生成するエラー メッセージが表示されている、最高でユーザーに迷惑なです。

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するために、使用する`Try/Catch`ステートメントです。 `Try`をチェックする場合のコードを実行するステートメントでは、します。 1 つまたは複数`Catch`ステートメントを検索できる固有の仕様が発生したエラー (特定の種類の例外)。 多くとして含めることができます`Catch`するとしてステートメントを予測しているエラーを確認する必要があります。

> [!NOTE]
> 使用しないことをお勧め、`Response.Redirect`メソッド`Try/Catch`ステートメント、ページに、例外を引き起こす可能性があるためです。


次の例では、最初の要求でテキスト ファイルを作成し、ユーザーがファイルを開くできるボタンを表示するページを示します。 意図的を使って不適切なファイル名と、例外を使用することができるようにします。 コードが含まれています`Catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET はフォルダーにも見つからない場合に発生します。 (できますコメントを解除する例では、内のステートメントすべてが正しく機能しているときの動作を確認するためにします。)

コードが例外を処理していない場合、前のスクリーン ショットのようなエラー ページが表示されます。 ただし、`Try/Catch`セクションでは、ユーザーがこの種のエラーを表示するを防ぐのに役立ちます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>その他のリソース

### <a name="reference-documentation"></a>リファレンス ドキュメント

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic 言語](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
