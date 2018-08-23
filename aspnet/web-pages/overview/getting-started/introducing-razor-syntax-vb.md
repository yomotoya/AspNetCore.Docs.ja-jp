---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要 |Microsoft Docs
author: tfitzmac
description: この付録概要 ASP.NET Web ページを使用したプログラミングの Visual basic で Razor 構文を使用します。
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833549"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、プログラミングの概要 ASP.NET Web Pages を Razor の構文と Visual Basic を使用します。 ASP.NET は、web サーバー上で動的な web ページを実行するためのマイクロソフトのテクノロジです。
> 
> **学習内容**:
> 
> - プログラミングの Razor 構文を使用して ASP.NET Web Pages のプログラミングの概要に関するヒント上位 8。
> - 基本的なプログラミング概念必要があります。
> - どのような ASP.NET サーバー コードと Razor 構文についてです。
>   
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


Razor 構文を使用する ASP.NET Web ページを使用するほとんどの例は c# を使用します。 Razor 構文では、Visual Basic もサポートされています。 Visual Basic での ASP.NET web ページをプログラミングで web ページを作成する、 *.vbhtml*ファイル名拡張子、Visual Basic コードを追加します。 この記事では、Visual Basic 言語と ASP.NET web ページを作成する構文の概要を示します。

> [!NOTE]
> Microsoft WebMatrix の既定の web サイト テンプレート (**パン屋さん**、**フォト ギャラリー**、および**スターター サイト**など) は c# および Visual Basic のバージョンで使用できます。 NuGet パッケージとしてでは、Visual Basic テンプレートをインストールできます。 Web サイト テンプレートがという名前のフォルダーで、サイトのルート フォルダーにインストールされている*Microsoft テンプレート*します。


## <a name="the-top-8-programming-tips"></a>最上位の 8 つのプログラミングのヒント

このセクションでは、Razor 構文を使用して ASP.NET サーバー コードの記述を開始する際に知っておく必要ないくつかのヒントを示します。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.コードを追加するページを使用して、@ 文字

`@`文字は、インライン式、単一ステートメント ブロック、および複数ステートメントのブロックを開始します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

ブラウザーで表示される結果:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML エンコード**
> 
> ページを使用して、コンテンツを表示すると、 `@` ASP.NET HTML エンコードの出力を前の例のように文字します。 これにより、HTML の予約文字が置き換えられます (など`<`と`>`と`&`) の HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするコードを持つ。 HTML エンコードをせずサーバー コードからの出力は正しく表示されない場合があり、セキュリティのリスクについてのページを公開する可能性があります。
> 
> 目的がマークアップとしてタグをレンダリングする HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調するために) を参照してください[を組み合わせることのテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。
> 
> 詳細での HTML エンコードについて[ASP.NET Web Pages サイトでの HTML フォームを使用する](https://go.microsoft.com/fwlink/?LinkId=202892)します。


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2.コードのコード ブロックを囲むとしています.終了コード

コード ブロックが 1 つまたは複数のコード ステートメントが含まれていて、キーワードで囲まれた`Code`と`End Code`します。 配置開始`Code`キーワードの直後に、`@`文字&#8212;それらの間を空白にすることはできませんがあります。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

ブラウザーで表示される結果:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3.ブロック内で改行するのには、各コード ステートメントを終了します。

Visual Basic のコード ブロックには、各ステートメントは、改行で終わります。 (記事の後半で必要な場合は、複数の行に長いコード ステートメントをラップする方法を参照します)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4.値を格納するのに変数を使用します。

内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`Dim`キーワード。 変数の値を挿入するにを使用してページに直接`@`します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

ブラウザーで表示される結果:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.リテラル文字列値を二重引用符で括る

A*文字列*テキストとして扱われる文字のシーケンスです。 文字列を指定するには、二重引用符で囲みます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

文字列値内で二重引用符を埋め込むには、するには、2 つの二重引用符文字を挿入します。 ページの出力に 1 回出現する二重引用符文字にする場合は、入力として`""`内で、引用符で囲まれた文字列、および 2 回表示する場合は入力として`""""`引用符で囲まれた文字列内で。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

ブラウザーで表示される結果:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6.Visual Basic のコードは大文字小文字を区別しません。

Visual Basic 言語は大文字小文字が区別されません。 プログラミング キーワード (など`Dim`、 `If`、および`True`) と変数名 (など`myString`、または`subTotal`) いずれの場合も記述できます。

次のコード行では、変数に値を割り当てる`lastname`小文字を使用して、名前し、ページの大文字の名前を使用する変数の値を出力します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

ブラウザーで表示される結果:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7.コーディングのほとんどには、オブジェクトの操作が含まれます

オブジェクトをプログラミングできることを表します&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求を電子メール メッセージ、(データベースの行) の顧客レコードなど。オブジェクトの特性を記述するプロパティがある&#8212;テキスト ボックスのオブジェクトが、`Text`プロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージが、`From`プロパティ、および customer オブジェクトが、 `FirstName`プロパティ。 オブジェクトには、メソッドも含まれて、&quot;動詞&quot;を実行できます。 例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッド。

多くの場合、使用する、`Request`オブジェクトは、フォームの値などの情報を提供するフィールド (テキスト ボックスなど) のページにブラウザーの種類は、要求、ページ、ユーザー id などの URL を作成します。この例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバーで、ページの絶対パスを確認できます。

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

ブラウザーで表示される結果:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.意思決定を行うコードを記述することができます。

動的な web ページの重要な機能は、条件に基づいて対処方法を決定できます。 これを行う最も一般的な方法は、`If`ステートメント (と省略可能な`Else`ステートメント)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

ステートメント`If IsPost`は記事の執筆を簡略化`If IsPost = True`します。 と共に`If`ステートメント、さまざまなコードのブロックを繰り返し、条件をテストする方法があるしはこの記事の後半で説明されています。

結果がブラウザーで表示されます (クリックすると**送信**)。

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET と POST メソッドと IsPost プロパティ**
> 
> Web ページ (HTTP) で使用されるプロトコルは、メソッドの非常に限られた数をサポートしています (&quot;動詞&quot;) サーバーに要求を行うに使用します。 2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用されます。 一般に、初めてユーザーの要求 ページでは、ページの要求が GET を使用します。 かどうか、ユーザーは、フォームに入力して、クリックして**送信**ブラウザー、サーバーへの POST 要求。
> 
> Web プログラミング、かどうか、ページを要求する、POST または GET としてページを処理する方法がわかるように、知っておくと便利がよくあります。 使用する ASP.NET Web ページで、`IsPost`プロパティを要求が GET または POST がかどうかを参照してください。 要求が、投稿の場合、`IsPost`プロパティが true を返すし、フォーム上のテキスト ボックスの値などの読み取りを行うことができます。 値に応じて異なる方法でページを処理する方法がわかります多くの例に示します`IsPost`します。


## <a name="a-simple-code-example"></a>単純なコード例

この手順では、基本的なプログラミング手法を説明するページを作成する方法を示します。 例では、次にそれらを追加し、結果が表示されます、2 つの数値を入力できるようにするページを作成します。

1. エディターで、新しいファイルを作成し、名前*AddNumbers.vbhtml*します。
2. ページでは、既にページ内のあらゆるものを置き換えるには、次のコードとマークアップをコピーします。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    ここでは、いくつかの点に注意してください。

    - `@`文字 ページで、コードの最初のブロックを開始して、その直後にある、`totalMessage`下部にある変数が埋め込まれています。
    - ページの上部にあるブロックがで囲まれた`Code...End Code`します。
    - 変数`total`、 `num1`、 `num2`、および`totalMessage`複数の数値と文字列を格納します。
    - 割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内。
    - Visual Basic コードは小文字を区別しない場合があるため、`totalMessage`ページの下部にある変数が使用される、その名前は、ページの上部にある変数の宣言のスペルが一致するようにのみ必要があります。 大文字小文字の区別が重要でありません。
    - 式`num1.AsInt()`  +  `num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示しています。 `AsInt`各変数のメソッドに追加できる整数 (整数) に、ユーザーが入力した文字列を変換します。
    - `<form>`タグが含まれています、`method="post"`属性。 指定のユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用して、サーバーに送信されます。 ページを送信するときに、コード`If IsPost`true に評価され、条件付きコードの数値を加算の結果を表示するを実行します。
3. ページを保存し、ブラウザーで実行します。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic 言語と構文

以前、ASP.NET web ページを作成する方法と、HTML マークアップをサーバー コードを追加する方法の基本的な例を説明しました。 Visual Basic を使用して、Razor 構文を使用して ASP.NET サーバー コードを記述するの基礎を学習ここ&#8212;プログラミング言語の規則では、します。

(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミング経験豊富な場合は、ここで読んだの多くが使い慣れたになります。 内のマークアップに WebMatrix のコードを追加する方法でのみについて理解しておく必要がありますおそらく *.vbhtml*ファイル。

### <a id="BM_CombiningTextMarkupAndCode"></a>  テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること

サーバー コード ブロックでは、テキスト、およびページ マークアップを出力するを多くの場合。 サーバー コード ブロックにコードでないし、する代わりに表示するか、テキストが含まれている場合、ASP.NET をコードからそのテキストを区別するためにできる必要があります。 これにはいくつかの方法があります。

- などの HTML ブロック要素で、テキストを囲む`<p></p>`または`<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。 ASP.NET が HTML の開始タグを表示する場合 (たとえば、 `<p>`)、要素をレンダリングすべておよびとしてそのコンテンツがブラウザー (およびサーバー コード式を解決) に、します。

- 使用して、`@:`演算子または`<text>`要素。 `@:`プレーン テキストまたは HTML タグが一致していませんが含まれているコンテンツを 1 行の出力、`<text>`要素を出力する複数の行で囲みます。 これらのオプションは、出力の一部として HTML 要素を表示したくない場合に便利です。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    次の例では、前の例の繰り返しですがの 1 つのペアを使用して`<text>`タグを表示するテキストを囲みます。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    次の例では、`<text>`と`</text>`タグは、一部の非包含エンティティのテキストと HTML タグの不一致があるすべての 3 つの行を囲む (`<br />`)、サーバー コードと一致した HTML タグ。 ここでも、使用して個別に各の行の前でしたも、`@:`演算子です。 いずれかの方法の動作。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。 (ASP.NET がサーバーのコード式とが付いているサーバー コード ブロックの出力をエンコードする前述のように、 `@`、このセクションに記載されている特殊なケースでは可します)。

### <a name="whitespace"></a>Whitespace

余分なスペース (および、文字列リテラルの外部で) ステートメントでステートメントには影響しません。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>長いステートメントを複数の行に分割

アンダー スコア文字を使用して、複数の行に長いコード ステートメントを中断できます`_`(Visual basic と呼ばれる、*連結文字*) 各行のコードの後にします。 次の行にステートメントを中断するには、行の末尾にスペースと連結文字を追加します。 次の行にステートメントを続行します。 読みやすさを向上させるために必要な数の行にステートメントをラップすることができます。 次のステートメントでは、同じです。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

ただし、文字列リテラル内の行をラップすることはできません。 次の例が機能しません。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

使用する必要を上記のコードのような複数の行にラップする長い文字列を結合する、*連結演算子*(`&`)、この記事の後半でがあります。

### <a name="code-comments"></a>コードのコメント

コメントを使用して、自分自身または他のユーザーは、ノートをそのまま使用できます。 Razor 構文のコメントが付いて`@*`と末尾`*@`します。

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

コード ブロック内でコメントを使用して、Razor 構文、または単一引用符は、通常の Visual Basic のコメント文字を使用することができます (`'`) 各の行にプレフィックスとして付加されます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>変数

変数は、データの格納に使用する名前付きオブジェクトです。 名前を付ける変数、何かが、名前がアルファベットの文字で始める必要があり、空白文字または予約文字を含めることはできません。 Visual basic で、前に説明したように、変数名の文字の大文字と小文字は関係ありません。

### <a name="variables-and-data-types"></a>変数とデータ型

変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。 文字列値を格納する文字列変数があることができます (よう&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付の値を格納する date 型の変数). その他の多くのデータ型を使用することができます。

ただし、変数の型を指定する必要はありません。 ほとんどの場合 ASP.NET は、変数内のデータの使用方法に基づいて型を出すことができます。 (場合によっては、型を指定する必要があります。 これは true、例が表示されます)。

型を指定せずに変数を宣言するには使用`Dim`さらに、変数名 (たとえば、 `Dim myVar`)。 型の変数を宣言するには使用`Dim`さらに、変数名に続けて`As`してから、型名 (たとえば、 `Dim myVar As String`)。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

次の例では、web ページで、変数を使用する一部のインライン式を示します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

ブラウザーで表示される結果:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>変換して、データ型をテストします。

ASP.NET は、データ型を自動的に決定することができます、通常が場合があります、ことはできません。 そのため、明示的な変換を実行することで ASP.NET を手助けする必要があります。 型変換を持っていない場合でもデータの種類が使用することを確認するテストに便利です。

最も一般的なは、整数や日付など、別の型を文字列に変換することがあります。 次の例では、文字列を数値に変換する必要があります、一般的なケースを示します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

原則として、ユーザー入力では、文字列として。 番号を入力するユーザーを要求した場合でも、および数字をユーザー入力が送信され、コードでの読み取り時に入力する場合でも、データは文字列の形式です。 そのため、文字列を数値に変換する必要があります。 例では、変換、しなくても、値に対して算術演算を実行しようとする場合、次のエラーの結果、ASP.NET は、2 つの文字列を追加できないため。

`Cannot implicitly convert type 'string' to 'int'.`

呼び出すの値を整数に変換する、`AsInt`メソッド。 変換が成功した場合は、数値を追加できます。

次の表では、変数の一般的ないくつかの変換とテスト方法を示します。


:::row:::
    :::column:::
        <strong>メソッド</strong>
    :::column-end:::
    :::column:::
        <strong>説明</strong>
    :::column-end:::
    :::column:::
        <strong>例</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        整数を表す文字列に変換します (など&quot;593&quot;) 整数にします。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        などの文字列に変換します&quot;true&quot;または&quot;false&quot;ブール型にします。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot; 10 進数。 (ASP.NET では、10 進数、浮動小数点数よりも正確)。 :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Asp.net の日付と時刻の値を表す文字列に変換します`DateTime`型。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        その他の任意のデータ型を文字列に変換します。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a>演算子

演算子は、キーワードまたは式の中で実行するコマンドの種類を ASP.NET に指示する文字です。 Visual Basic は、多くの演算子をサポートしていますが、ASP.NET web ページの開発を開始するいくつかを認識するだけで済みます。 次の表では、最も一般的な演算子をまとめたものです。


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>説明</strong>
    :::column-end:::
    :::column:::
        <strong>例</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        算術演算子が数値式で使用します。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        割り当てと等しいかどうか。 、コンテキストに応じてはいずれか、左側にあるオブジェクトにステートメントの右側にある値を割り当てるか、値の等価性を確認します。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        非等値。 返します`True`値が等しくない場合。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        未満の場合、多い、少ないよりまたは等しいかと以上。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        連結、文字列を結合するために使用します。
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        ドットです。 オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        かっこです。 グループ式の場合に使用すると、メソッド、および配列とコレクションのメンバーにアクセスする、パラメーターを渡します。
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        じゃない。 False、またはその逆は真の値を反転させます。 通常のテストを簡略化として使用される`False`(には、いない`True`)。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        論理 AND またはと、条件をまとめてリンクに使用されます。
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>ファイルとコード内のフォルダー パスを使用します。

コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。 開発用コンピューターに表示される次の web サイトの物理フォルダー構造の例に示します。

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Url およびパスについていくつかの重要な詳細情報を次に示します。

- URL のいずれかでドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。
- URL は、ホスト コンピューター上の物理パスに対応します。 たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバー。
- 仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。 ドメインまたはサーバー名に続く URL の一部が含まれています。 仮想パスを使用すると、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。

相違点の理解に役立つ例を次に示します。

| 完全な URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| サーバー名 | *mycompanyserver* |
| [仮想パス] | */humanresources/CompanyPolicy.htm* |
| 物理パス | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

仮想ルートは、/、ドライブは、c: のルートと同じように \ です。 (仮想フォルダーのパス常にスラッシュを使用します。)フォルダーの仮想パスとして、物理フォルダー。 同じ名前を指定する必要はありません。エイリアスがあります。 (実稼働サーバー上の仮想パスことはほとんどありませんと一致する特定の物理パス。)

コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを使用しているに応じての仮想パスを参照する必要があります。 ASP.NET ツールを提供するこれらのコードでのファイルとフォルダーのパスの作業:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッド。

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>仮想物理パスに変換する: Server.MapPath メソッド

`Server.MapPath`メソッドが仮想パスに変換します (など */default.cshtml*) を絶対物理パス (など*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 このメソッドを使用すると、いつでも完全な物理パスを作成する必要がありますに。 典型的な例では、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成する場合です。

通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、わかっているため、このメソッドは、パスを変換できます: 仮想パス: サーバー上の対応するパスにします。 仮想パスをファイルまたはフォルダー、メソッドに渡すし、物理パスを返します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>仮想ルートを参照する: ~ 演算子と Href メソッド

*.Cshtml*または *.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。 これは、機能は、サイトでは、内のページを移動することができ、他のページが含まれているすべてのリンクが壊れているできませんので、非常に便利です。 これまで、web サイトを別の場所に移動する場合にも便利です。 次にいくつかの例を示します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

場合は、web サイトが`http://myserver/myapp`、ここでは、ASP.NET が処理する方法これらのパス、ページの実行時に。

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(実際には、変数の値としてこれらのパスは表示されませんが ASP.NET は、パスとして扱うだったです)。

使用することができます、 `~` (上記) としてサーバー コードと次のように、マークアップの両方の演算子。

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

マークアップでは、使用する、`~`演算子イメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。 ASP.NET が (コードとマークアップの両方) のページを探し、すべて解決、ページの実行時に、`~`適切なパスへの参照。

## <a name="conditional-logic-and-loops"></a>条件付きロジックやループ

ASP.NET サーバー コードにより、条件とループを実行する回数を特定のコードは、ステートメントを繰り返し実行する書き込みコードに基づいてタスクを実行する)。

### <a name="testing-conditions"></a>条件のテスト

使用する単純な条件をテストする、`If...Then`ステートメントでは、返す`True`または`False`指定したテストに基づきます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If`キーワードは、ブロックを開始します。 実際のテスト (条件) に依存して、`If`キーワードと true または false を返します。 `If`ステートメントが終わる`Then`します。 テストが true の場合に実行するステートメントがで囲まれた`If`と`End If`します。 `If`ステートメントを含めることができます、`Else`ブロックする条件が false の場合に実行するステートメントを指定します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

場合、`If`ステートメントがコード ブロックを開始、法線を使用する必要はありません`Code...End Code`要素を含めるようにステートメントをします。 追加するだけ`@`するブロックが機能します。 この方法を使用できます`If`他の Visual Basic のキーワードを含む、コード ブロックに続くのプログラミングと`For`、 `For Each`、`Do While`など。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

1 つまたは複数を使用して複数の条件を追加する`ElseIf`ブロック。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

最初の条件の場合、この例では、`If`ブロックが true でない、`ElseIf`条件がチェックされます。 その条件が満たされた場合、ステートメントで、`ElseIf`ブロックが実行されます。 どの条件が満たされた場合、ステートメントで、`Else`ブロックが実行されます。 任意の数を追加する`ElseIf`ブロック、およびを閉じて、`Else`としてブロック、&quot;他のすべて&quot;条件。

多くの条件をテストするには、使用、`Select Case`ブロック。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

テストする値は、(例、weekday 変数) ではかっこ内に示します。 各テストを使用して、`Case`値を一覧表示するステートメント。 場合の値を`Case`ステートメントには、そのコードのテスト値が一致すると`Case`ブロックが実行されます。

ブラウザーで表示される最後の 2 つの条件付きブロックの結果:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>ループのコード

多くの場合、同じステートメントを繰り返し実行する必要があります。 そのためには、ループ処理します。 たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。 使用することが正確に何回ループすることがわかっている場合、`For`ループします。 この種のループはまでカウントするか、カウント ダウン特に便利です。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

ループが始まり、`For`キーワード、3 つの要素に続きます。

- 直後に、`For`ステートメントでは、カウンター変数を宣言する (を使用する必要はありません`Dim`) ように、範囲を示すと`i = 10 to 20`します。 つまり、変数`i`10 からカウントが開始され、20 (両端を含む) に到達するまで続行します。
- 間、`For`と`Next`ステートメントは、ブロックのコンテンツ。 これは、1 つまたは複数のコード ステートメントを実行する各ループを含めることができます。
- `Next i`ステートメント、ループを終了します。 カウンターをインクリメントし、ループの次のイテレーションを開始します。

間のコード行、`For`と`Next`行には、ループの反復ごとに実行されるコードが含まれています。 マークアップは、新しい段落を作成する (`<p>`要素) の各時間し、の値を表示する出力に行を追加します。 i (カウンター)。 このページを実行すると、例は、11 行の行のテキスト項目の数を示す行ごとに、出力の表示を作成します。

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

多くの場合、使用、コレクションまたは配列で作業している場合、`For Each`ループします。 コレクションは、類似のオブジェクトのグループと`For Each`ループでコレクション内の各項目のタスクを実行することができます。 この種のループはコレクションの場合、便利なためとは異なり、`For`ループ カウンターを増分または制限を設定する必要はありません。 代わりに、`For Each`ループのコードだけを進め、コレクションが完了するまでです。

この例は、内の項目を返します、 `Request.ServerVariables` (コレクションを web サーバーに関する情報が含まれています)。 使用して、`For Each`新たに作成して各項目の名前を表示するループ`<li>`HTML の箇条書きリスト内の要素。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each`キーワードに続けて、コレクション内の 1 つの項目を表す変数 (例では、 `myItem`)、その後に、`In`キーワードのコレクションをループするか後にします。 本体で、`For Each`ループ、先ほど宣言した変数を使用して現在の項目にアクセスすることができます。

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

汎用的なループを作成するには、使用、`Do While`ステートメント。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

このループが始まり、`Do While`キーワード、後ろに、条件を繰り返すブロックが続きます。 ループの通常のインクリメント (に追加する) または減分 (減算)、変数またはオブジェクト カウントで使用されます。 この例で、`+=`演算子に 1 を加算する変数の値、ループが実行されるたびにします。 (カウント ダウンをループ内の変数をデクリメントするは、デクリメント演算子を使用して`-=`)。

## <a name="objects-and-collections"></a>オブジェクトとコレクション

ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。 このセクションでは、使用する多くの場合、コードでいくつかの重要なオブジェクトについて説明します。

### <a name="page-objects"></a>ページのオブジェクト

ASP.NET の最も基本的なオブジェクトは、ページです。 オブジェクトを修飾せずに直接ページ オブジェクトのプロパティにアクセスすることができます。 次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

プロパティを使用して、`Page`など多くの情報を取得するオブジェクト。

- `Request`。 既に説明したように行われる要求、ページ、ユーザー id などの URL をブラウザーの種類を含む、現在の要求に関する情報のコレクションになります。
- `Response`。 これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。 たとえば、このプロパティを使用すると、応答に情報を書き込みます。

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>コレクション オブジェクト (配列、ディクショナリ)

コレクションのコレクションなど、同じ型のオブジェクトのグループは、`Customer`データベースからのオブジェクト。 このような ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。

多くの場合、コレクション内のデータを操作します。 2 つの一般的なコレクション型は、*配列*と*ディクショナリ*します。 配列は、類似した項目のコレクションを格納するが、それぞれ個別の各項目を保持する変数を作成したくない場合に役立ちます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

配列の使用、特定のデータ型を宣言するなど`String`、 `Integer`、または`DateTime`します。 変数が配列を含めることができます、宣言で変数名にかっこを追加することを示すために (など`Dim myVar() As String`)。 またはを使用してそれらの位置 (インデックス) を使用して、配列内の項目にアクセスすることができます、`For Each`ステートメント。 配列のインデックスは 0 から始まる&#8212;つまり最初の項目がある 0 の位置は、2 番目の項目は、位置 1、という具合にします。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

取得することによって、配列内の項目の数を判断するその`Length`プロパティ。 配列内の特定の項目の位置を取得する (つまり、配列を検索します)、使用、`Array.IndexOf`メソッド。 行うことができますも逆の順序など、配列の内容 (、`Array.Reverse`メソッド) または内容を並べ替える (、`Array.Sort`メソッド)。

ブラウザーで表示されている文字列配列コードの出力:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

ディクショナリは設定または対応する値を取得するキー (または名前) を指定するキー/値のペアのコレクションです。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

使用するディクショナリを作成する、`New`を新規作成しているかを示すキーワード`Dictionary`オブジェクト。 変数を使用するディクショナリを割り当てることができます、`Dim`キーワード。 かっこを使用して、ディクショナリ内の項目のデータ型を指定する ( `( )` )。 これは、実際には、新しいディクショナリを作成するメソッド宣言の末尾には、もう 1 つのかっこのペアを追加する必要があります。

項目をディクショナリに追加するを呼び出すことができます、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。 または、キーを示すし、次の例のように、単純な割り当てを行うにはかっこを使用できます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

をディクショナリから値を取得するには、かっこで囲まれたキーを指定します。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>パラメーターを持つメソッドを呼び出す

この記事の前半で説明するようにプログラミングでのオブジェクトはメソッドがあります。 たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッド。 多くのメソッドは、1 つまたは複数のパラメーターを指定します。 A*パラメーター*メソッドに渡す値は、タスクが完了する方法を有効にします。 たとえばの宣言を見て、`Request.MapPath`メソッドは、次の 3 つのパラメーターを受け取る。

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

このメソッドは、指定した仮想パスに対応するサーバー上の物理パスを返します。 メソッドの 3 つのパラメーターは`virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`します。 (宣言で、パラメーターは許可されるデータのデータ型の一覧に注目してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。

Visual Basic と Razor 構文を使用しているときにパラメーターをメソッドに渡す 2 つのオプションがある:*位置指定パラメーター*または*名前付きパラメーター*します。 位置指定パラメーターを使用してメソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。 (が通常わかるこの順序メソッドのドキュメントを参照しています。)順序に従い、任意のパラメーターをスキップすることはできません&#8212;かどうか、必要に応じて空の文字列を渡す (`""`) の値がない位置指定パラメーターの場合は null です。

次の例は、という名前のフォルダーにあることを前提*スクリプト*web サイトにします。 コードの呼び出し、`Request.MapPath`を正しい順序で 3 つのパラメーターの値をメソッドを渡します。 結果のマップされたパスが表示されます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

メソッドの多くのパラメーターが存在する場合、するおくと、コードより読みやすい名前付きパラメーターを使用しています。 名前付きパラメーターを使用してメソッドを呼び出すには、指定のパラメーター名の後に`:=`し、値を指定します。 名前付きパラメーターの利点は、任意の順序で追加できることです。 (欠点は、メソッドの呼び出しが、compact ではないこと) です。

次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターの使用。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

ご覧のように、パラメーターは異なる順序で渡されます。 ただし、前の例と、この例を実行する場合、同じ値を返すします。

## <a name="handling-errors"></a>エラー処理

### <a name="try-catch-statements"></a>Try Catch ステートメント

上の理由から、コントロールの外部に障害が発生する、コード内のステートメントを多くの場合、があります。 例えば:

- コードは、開く、作成、読み取り、またはファイルの書き込みを試みると、あらゆる種類のエラーが発生する可能性があります。 目的のファイルが存在しない可能性があります、ロックアウトされることがあります、コードが、権限がありませんして具合可能性があります。
- 同様に場合は、コードでは、データベース内のレコードを更新しようとして、アクセス許可の問題があります、データベースへの接続が削除される可能性が、無効なおよびこれを保存するデータがあります。

プログラミング用語では、このような状況が呼び出されます*例外*します。 (スロー) が生成されますが、コードには、例外が発生すると、エラー メッセージは、最高でユーザーに面倒です。

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するためには、使用する`Try/Catch`ステートメント。 `Try`ステートメントでは、チェックするコードを実行します。 1 つまたは複数`Catch`ステートメントでは、特定の検索できます (特定の種類の例外の) エラーの発生した可能性があります。 多くとして含めることができます`Catch`とステートメントは、予測しているエラーを確認する必要があります。

> [!NOTE]
> 使用を避けることをお勧めします。、`Response.Redirect`メソッド`Try/Catch`ステートメント、ページで、例外が生じるためです。


次の例では、最初の要求でテキスト ファイルを作成し、ファイルを開くユーザーを切り替えるボタンを表示するページを示します。 例は、例外が発生されるように意図的に不適切なファイル名を使用します。 コードが含まれています`Catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET は、フォルダーにも見つからない場合に発生します。 (コメントを解除できます、ステートメントの例では、すべてが適切に動作しているときの動作を確認するためにします。)

コードが例外を処理していない場合は、前のスクリーン ショットのようなエラー ページが表示されます。 ただし、`Try/Catch`セクションでは、ユーザーがこの種のエラーが表示されることを防ぐのに役立ちます。

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>その他のリソース

### <a name="reference-documentation"></a>リファレンス ドキュメント

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic 言語](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
