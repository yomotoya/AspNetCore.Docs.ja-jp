---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "Razor 構文 (c#) を使用して ASP.NET Web プログラミングの概要 |Microsoft ドキュメント"
author: tfitzmac
description: "この章では、するプログラミングの概要については ASP.NET Web Pages を Razor 構文を使用します。 動的な web の pa を実行するための Microsoft のテクノロジを ASP.NET には."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: f054d574026ab6444cc59a126ef9dcdc323f7bff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Razor 構文 (c#) を使用して ASP.NET Web プログラミングの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ここで、プログラミングの概要 ASP.NET Web Pages を Razor 構文を使用します。 ASP.NET は、web サーバーでの動的な web ページを実行するための Microsoft のテクノロジです。 この記事に重点を置く c# プログラミング言語を使用します。
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


## <a name="the-top-8-programming-tips"></a>最上位の 8 プログラミングのヒント

このセクションでは、絶対に必要な Razor 構文を使用して ASP.NET サーバー コードの記述を開始すると、いくつかのヒントを示します。

> [!NOTE]
> Razor 構文は c# プログラミング言語に基づいており、ASP.NET Web Pages を最もよく使用される言語であります。 ただし、Razor 構文は、Visual Basic 言語と Visual Basic で実行することもできます。 表示されるものをもサポートします。 詳細については、「付録[Visual Basic 言語と構文](https://go.microsoft.com/fwlink/?LinkId=202908)です。


記事の後半でこれらのプログラミング手法のほとんどについての詳細が表示されます。

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1.コードを使用してページを追加する、@ 文字

`@`文字は、インライン式、1 つのステートメント ブロック、および複数ステートメントのブロックを開始します。

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

これらのステートメントがどのようなページがブラウザーで実行されるときに次に示します。

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML エンコード**
> 
> ページを使用して、コンテンツを表示した場合、`@`文字、上記の例では、ASP.NET は、出力を HTML でエンコードします。 これには、HTML の予約された文字が置き換えられます (など`<`と`>`と`&`) コードを HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするとします。 HTML エンコードせず、サーバー コードからの出力は正しく表示されない場合があり、セキュリティ上のリスクにページを公開する可能性があります。
> 
> 目標は、マークアップとしてタグがレンダリングされる HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調する) を参照してください[結合のテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。
> 
> 詳細での HTML エンコードに関する[フォームを使用する](https://go.microsoft.com/fwlink/?LinkId=202892)です。


### <a name="2-you-enclose-code-blocks-in-braces"></a>2.コード ブロックを中かっこで囲みます

A*コード ブロック*1 つまたは複数のコード ステートメントが含まれており、中かっこで囲まれています。

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

ブラウザーに表示される結果:

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3.セミコロンで各コード ステートメントを終了するブロック内

コード ブロックの内部完全なコードの各ステートメントをセミコロンで終了する必要があります。 インライン式は、セミコロンで終了しません。

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4.値を格納するのに変数を使用します。

内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`var`キーワード。 変数の値を挿入するにはページを使用して、直接`@`です。

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

ブラウザーに表示される結果:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5.リテラル文字列の値を二重引用符で囲みます。

A*文字列*テキストとして扱われる文字のシーケンスです。 文字列を指定するには、二重引用符で囲みます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

表示する文字列がバック スラッシュ文字を含むかどうか ( `\` ) または二重引用符 ( `"` ) を使用して、*逐語的文字列リテラル*が付いている、`@`演算子。 (C# の場合、\ 文字は、逐語的文字列リテラルを使用する場合に特別な意味を持つ)。

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

二重引用符を埋め込むには、逐語的文字列リテラルを使用して、引用符を繰り返します。

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

ページを使用してこれらの例の両方の結果を次に示します。

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> 注意して、 `@` verbatim 文字列リテラル (C#) をマークして、ASP.NET ページ内のコードをマークする、文字を使用します。


### <a name="6-code-is-case-sensitive"></a>6.コードは大文字小文字を区別

C# の場合、キーワード (と同様に`var`、 `true`、および`if`) し、変数名では大文字小文字を区別します。 次のコード行が 2 つの異なる変数を作成する`lastName`と`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

として変数を宣言する場合`var lastName = "Smith";`、ページとしてその変数を参照しようとする場合と`@LastName`、ために、エラーが発生`LastName`認識されません。

> [!NOTE]
> Visual Basic のキーワードと変数は、*いない*大文字小文字を区別します。


### <a name="7-much-of-your-coding-involves-objects"></a>7.コーディングのほとんどには、オブジェクトが含まれます

*オブジェクト*プログラミングで使用できる点 &#8212; を表すページ、テキスト ボックス、ファイル、画像、web 要求、電子メール メッセージ、顧客レコード (データベースの行) などです。オブジェクトの特性を記述するプロパティがあり、読み取ることができます、または変更 &#8212;テキスト ボックスのオブジェクトが、 `Text` (など) のプロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージには、`From`プロパティ、および顧客オブジェクトが、`FirstName`プロパティです。 オブジェクトでは、実行されるメソッドもがある、&quot;動詞&quot;それらを実行できます。 例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッドです。

使って作業を多くの場合、`Request`オブジェクト (フォーム フィールド) のテキスト ボックスの値などの情報を提供する ページで、ブラウザーの種類は、要求、ページや、ユーザー id などの URL を作成します。次の例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバー上のページの絶対パスを表示します。

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

ブラウザーに表示される結果:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8.意思決定を行うコードを記述することができます。

動的な web ページの主な機能は、条件に基づき、新機能を決定できます。 これを行う最も一般的な方法は、`if`ステートメント (と省略可能な`else`ステートメント)。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

ステートメント`if(IsPost)`書き込みのためのショートハンド方法`if(IsPost == true)`です。 と共に`if`ステートメントはさまざまな方法は、コードのブロックを繰り返し、条件をテストするために、か、この記事の後半で説明されています。

結果がブラウザーで表示されます (クリックした後**送信**)。

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET と POST メソッドおよび IsPost プロパティ
> 
> Web ページ (HTTP) に使用されるプロトコルには、ごく少数のサーバーに要求に使用されるメソッド (動詞) がサポートしています。 2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用します。 一般に、ユーザーの要求 ページでは、最初に、ページが要求された GET を使用します。 場合は、ユーザーは、フォームに入力し、送信ボタンをクリックし、ブラウザー、POST 要求、サーバーを行います。
> 
> Web プログラミング、かどうか、ページが要求されて、GET または POST として、ページの処理方法がわかるように、知っておくと役立ちますがしばしばあります。 ASP.NET Web Pages で行うこともできます、`IsPost`プロパティを要求が GET または POST があるかどうかを参照してください。 要求が、投稿の場合、`IsPost`プロパティが true の場合が返され、フォーム上のテキスト ボックスの値読み取りなどを行うことができます。 多くの例を確認する方法を示しますの値に応じて異なる方法でページを処理する`IsPost`です。


## <a name="a-simple-code-example"></a>単純なコード例

この手順では、基本的なプログラミング技術について説明するページを作成する方法を示します。 例では、それらを追加し、結果を表示し、2 つの数値を入力できるように、ページを作成します。

1. エディターで、新しいファイルを作成し、名前*AddNumbers.cshtml*です。
2. ページで、ページ内の既存のものを置き換えるには、次のコードとマークアップをコピーします。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    ここでは、いくつかの点に注意してください。

    - `@`文字 ページで、コードの最初のブロックを開始して、前に指定する、`totalMessage`ページの下部に埋め込まれている変数。
    - ページの上部にあるブロックは中かっこで囲まれます。
    - 上部にあるブロックでは、すべての行は、セミコロンで終了します。
    - 変数`total`、 `num1`、 `num2`、および`totalMessage`いくつかの数字と文字列を格納します。
    - 割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内です。
    - コードがときに、大文字小文字を区別があるため、`totalMessage`ページの下部にある変数を使用すると、その名前では、上部にある変数を完全に一致する必要があります。
    - 式`num1.AsInt() + num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示します。 `AsInt`各変数のメソッドに算術演算を実行できるように、数値 (整数) に、ユーザーが入力した文字列に変換します。
    - `<form>`タグが含まれています、`method="post"`属性。 これを指定する、ユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用してサーバーに送信されます。 ページが送信されるときに、`if(IsPost)`テスト true に評価され、条件付きコードの実行、数値を加算した結果を表示します。
3. ページを保存し、ブラウザーで実行します。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>基本的なプログラミング概念

この記事では、ASP.NET web プログラミングの概要を提供します。 完全な検査、最も頻繁に使用するプログラミングの概念を通じてだけクイック ツアーではありません。 それでも、ほとんどの ASP.NET Web Pages を開始する必要がありますを説明します。

最初に、ほとんどの技術的な背景。

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Razor 構文、サーバー コード、および ASP.NET

Razor 構文は、web ページで、サーバー ベースのコードを埋め込むための単純なプログラミング構文です。 Razor 構文を使用する web ページは、コンテンツの 2 つの種類: クライアントのコンテンツとサーバー コード。 クライアントにコンテンツを web ページを使用しているものは、: (要素) の HTML マークアップのスタイルを設定、CSS などの情報もなど、JavaScript、およびプレーン テキストの一部のクライアント スクリプトです。

Razor 構文を使用して、このクライアント コンテンツをサーバー コードを追加できます。 ページにサーバー コードがある場合、サーバーは、ブラウザーにページを送信する前に最初に、そのコードを実行します。 サーバーで実行すると、コードは、ためにクライアント コンテンツを単独で、サーバー ベースのデータベースへのアクセスなどを使用してもっと複雑なことができるタスクを実行できます。 最も重要なは、サーバー コードを動的に作成クライアント コンテンツ &#8212;HTML マークアップまたはその他のコンテンツを即座に生成し、ページを含む可能性がある静的な HTML と共にブラウザーに送信できます。 ブラウザーのパースペクティブから、サーバー コードによって生成されたクライアント コンテンツはその他のクライアント コンテンツと違いはありません。 既に説明したように、サーバーが必要なコードは非常にシンプルです。

Razor 構文を含む ASP.NET web ページが特別なファイル拡張子を持つ (*.cshtml*または*.vbhtml*)。 サーバーは、これらの拡張機能を認識して、Razor 構文でマークされているし、ブラウザーにページを送信するコードを実行します。

### <a name="where-does-aspnet-fit-in"></a>ASP.NET 適合する位置ですか。

Razor 構文は、Microsoft .NET Framework に基づいたは、ASP.NET と呼ばれる Microsoft のテクノロジに基づいています。 .Net Framework は、実質的にあらゆる種類のコンピューター アプリケーションを開発するための Microsoft のビッグ、包括的なプログラミング フレームワークです。 ASP.NET は、web アプリケーションの作成用に作られている .NET Framework の一部です。 開発者は、最大値と最高トラフィックの web サイトの多くは世界中の作成に ASP.NET を使いました。 (いつでも、ファイル名拡張子を表示*.aspx*サイトの URL の一部として、ASP.NET を使用して、サイトが作成されたことを知るします)。

Razor 構文を使用すると、専門知識をしているかどうかは、初心者とを使用すると、生産性の向上を理解しやすくなりますが簡略化された構文を使用して、ASP.NET のすべての電源ができます。 この構文は、簡単に使用できるが、ASP.NET と .NET Framework のファミリ関係は、web サイトが複雑になると、あるに使用できる大規模なフレームワークの電源を意味します。

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **クラスとインスタンス**
> 
> ASP.NET サーバー コードでは、クラスの概念にさらに作成されるオブジェクトを使用します。 クラスは、定義またはオブジェクトのテンプレートです。 たとえば、アプリケーションが含まれます、`Customer`プロパティと任意の顧客オブジェクトが必要なメソッドを定義するクラス。
> 
> アプリケーションは、実際の顧客情報を処理する必要がありますのインスタンスが作成されます (または*をインスタンス化*) customer オブジェクト。 個々 の顧客が別のインスタンスの`Customer`クラスです。 すべてのインスタンスを同じプロパティとメソッドをサポートしていますが、各顧客オブジェクトが一意であるために、各インスタンスのプロパティの値は、通常は異なります。 1 人の顧客オブジェクトに、`LastName`プロパティを"Smith"にすることがあります以外の場合は顧客の別のオブジェクトで、`LastName`プロパティを"Jones"にすることがあります
> 
> 同様に、サイト内の個別の web ページは、`Page`オブジェクトのインスタンスでは、`Page`クラスです。 ページのボタンは、`Button`オブジェクトのインスタンスでは、`Button`クラス、およびなです。 各インスタンスには独自の特性がすべてに基づいている新機能は、オブジェクトのクラスの定義で指定します。


## <a name="basic-syntax"></a>基本構文

サーバー コードを HTML マークアップを追加する方法と、ASP.NET Web Pages ページを作成する方法の基本的な例は前述です。 ここで、Razor 構文 &#8212; を使用して ASP.NET サーバー コードの記述の基本を学習します。プログラミング言語の規則では、します。

(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミングを使用した経験が場合、次を参照する新機能の多くについて理解されます。 習熟するのみでのマークアップにサーバー コードを追加する方法が必要*.cshtml*ファイル。

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること

サーバー コード ブロックでは、多くの場合に出力テキストかマークアップ (あるいはその両方) のページにします。 サーバー コード ブロックのコードではないとする代わりに表示するかは、テキストが含まれている場合、ASP.NET をコードからそのテキストを識別できる必要があります。 これにはいくつかの方法があります。

- ような HTML 要素にテキストを囲みます`<p></p>`または`<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。 ASP.NET で開始 HTML タグがどのように表示される場合 (たとえば、 `<p>`)、レンダリングされる要素を含むすべての式およびとしてその内容は、ブラウザーには、サーバー コード式を解決するときにそれをします。
- 使用して、`@:`演算子または`<text>`要素。 `@:`プレーン テキストまたは一致しない HTML タグを含むコンテンツの 1 つの行が出力、`<text>`要素を出力する複数の行で囲みます。 これらのオプションは、出力の一部としての HTML 要素を表示したくない場合に役立ちます。  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    複数行テキストまたは一致しない HTML タグを出力する場合は、各行の前に記述できます`@:`、または内の行を囲むことができ、`<text>`要素。 同様に、`@:`演算子、`<text>`タグはテキスト コンテンツを識別する、ASP.NET で使用され、ページの出力には常にレンダリングします。

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    最初の例は、前の例を繰り返しますの 1 つのペアを使用して`<text>`タグをレンダリングするテキストを囲みます。 2 番目の例では、`<text>`と`</text>`タグがいくつか含まれていないテキストと一致しない HTML タグがあるすべての 3 つの行を囲みます (`<br />`)、およびサーバー コードと一致した HTML タグ。 もう一度、ごとに 1 行の前でしたも、`@:`演算子以外のいずれかの方法の動作です。

    > [!NOTE]
    > ここで &#8212; のようにテキストを出力する場合HTML 要素を使用して、`@:`演算子、または`<text>`要素 &#8212;ASP.NET に、HTML エンコード、出力をしません。 (ASP.NET はサーバーのコード式と末尾に挿入されるサーバー コード ブロックの出力はエンコード前に述べたよう`@`、このセクションで説明した特殊なケースでは可します)。

### <a name="whitespace"></a>Whitespace

ステートメントで (および、文字列リテラルの外部) 余分なスペースは、ステートメントに影響しません。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

ステートメント内での改行には、ステートメントに影響がなく、読みやすくするためのステートメントをラップすることができます。 次のステートメントでは、同じです。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

ただし、文字列リテラルの途中で行をラップすることはできません。 次の例が機能しません。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

上記のコードのように複数の行に折り返す長い文字列を結合するには、2 つのオプションがあります。 連結演算子を使用することができます (`+`)、この記事で後述があります。 使用することも、`@`文字に逐語的文字列リテラル、この記事の前半で説明したとおりです。 行にわたって verbatim 文字列リテラルを中断することができます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>コード (およびマークアップ) コメント

コメントでは、自分自身または他のユーザーのノートのままにできます。 これらも無効にすること (*をコメント アウト*) コードまたはを実行したくないが、当面は、ページに維持するマークアップのセクションです。

Razor コードの構文と HTML マークアップのコメントを付けるさまざまながあります。 すべての Razor コードでは、Razor コメントは処理 (と同様、削除)、ページがブラウザーに送信される前に、サーバー上。 したがって、Razor コメントの構文では、ファイルを編集するものの、ページのソースであっても、ユーザーが表示されない場合に表示されるコメント コードに (またはマークアップにあっても) を挿入することができます。

ASP.NET Razor コメントのコメントを開始する`@*`終了することで、`*@`です。 1 つの行または複数の行にコメントができます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

コード ブロック内でコメントを次に示します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

ここでは、同じコードのブロック、コードの行をコメント アウトされて実行されないようにします。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Razor コメントの構文を使用する代わりに、コード ブロックの内側を使用して、c# など、プログラミング言語のコメント構文を使用できます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

C# の場合は、単一行コメントが付きます、`//`文字、および複数行コメントで始まる`/*`で終わります`*/`です。 (Razor コメントと c# コメントは、ブラウザーにレンダリングされません)。

マークアップ、ご存知のとおりは、HTML コメントを作成することができます。

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML コメントで始まる`<!--`文字、末尾が`-->`です。 HTML コメントを使用すると、囲むだけでなく、テキスト、またすべての HTML マークアップの表示にしたくないが、使用ページ内に維持することがあります。 この HTML コメントには、タグとが含まれているテキストの内容全体が非表示には。

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Razor コメント、HTML コメントとは異なり*は*のページに表示し、ユーザーがページのソースを表示して確認できます。

Razor では、c# の入れ子になったブロックに制限があります。 詳細については、次を参照してください[という名前の変数を使用する C# の場合、入れ子になったブロック壊れたコードの生成。](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>変数

変数は、データの格納に使用する名前付きオブジェクトです。 名前を付ける変数、何もが、名前の先頭の文字は英字にする必要があり、空白文字または予約文字は使用できません。

### <a name="variables-and-data-types"></a>変数とデータ型

変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。 文字列値を格納する文字列変数があることができます (のような&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付値を格納する date 型の変数). いて、その他の多くのデータ型を使用することができます。

ただし、一般にする必要はありません、変数の型を指定します。 ほとんどの場合、ASP.NET は、変数内のデータの使用方法に基づいて型を見つけることができます。 (場合によっては、型を指定する必要があります。 これが true の例が表示されます。)

使用して変数を宣言する、`var`キーワード (型を指定しない) 場合や、型の名前を使用しています。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

次の例では、web ページで変数の一般的な使用を示します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

ページで、前の例を組み合わせると、ブラウザーで表示されます。 これが表示されます。

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>変換して、データ型のテスト

ASP.NET はこのデータ型を自動的に決定できますは通常があります、できません。 そのため、明示的な変換を実行して ASP.NET を手助けする必要があります。 型変換を持っていない場合でもいる場合もありますデータの種類が使用することを確認するためのテストに役立ちます。

最も一般的なケースでは、整数や日付など、他の型を文字列に変換していることです。 次の例は、文字列を数値に変換する必要があります、一般的な事例を示しています。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

原則として、ユーザー入力に文字列として。 数値を入力するユーザーを要求した場合でも、ユーザー入力が送信されたコードで読み取るときに、数字を入力したした場合でも、データは文字列の形式です。 そのため、文字列を数値に変換する必要があります。 例では、したり、変換せず、値に対して算術演算を実行しようとする場合、次のエラーにより、ASP.NET は、2 つの文字列を追加できないため。

*型 'string' から 'int' を暗黙的に変換できません。*

呼び出すの値を整数に変換する、`AsInt`メソッドです。 変換が成功した場合は、数値を追加できます。

次の表は、変数の一般的ないくつかの変換とテスト方法を示します。

| **メソッド** | **説明** | **例** |
| --- | --- | --- |
| `AsInt(), IsInt()` | 整数 (「593」) のような自然数を表す文字列に変換します。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | ように文字列に変換します&quot;true&quot;または&quot;false&quot; Boolean 型にします。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を 10 進数。 (ASP.NET では、10 進数は、浮動小数点数よりも正確です。) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | Asp.net の日付と時刻の値を表す文字列に変換します`DateTime`型です。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | その他の任意のデータ型を文字列に変換します。 | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>演算子

演算子は、キーワードまたは ASP.NET を式の中で実行するコマンドの種類を示す文字です。 C# 言語 (および基になる Razor 構文) は、多くの演算子をサポートしますが、のみ開始するいくつかを認識する必要があります。 次の表は、最も一般的な演算子をまとめたものです。

| **Operator** | **説明** | **例** |
| --- | --- | --- |
| `+` `-` `*` `/` | 算術演算子: 数値式で使用します。 | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | 代入。 左側にあるオブジェクトにステートメントの右側にある値を割り当てます。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | 等値。 返します`true`値が等しい場合。 (上での違いに注意してください、`=`演算子および`==`演算子です)。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | 非等値。 返します`true`値が等しくない場合。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | 以下のより大きい-より、以下によりも-または-等しく、大きい-よりも-または-等しいかどうか。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | 連結、文字列の結合に使用します。 ASP.NET では、この演算子と式のデータ型に基づいて、加算演算子の違いを認識します。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=` `-=` | インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | ドットします。 オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | かっこです。 グループ式とをメソッドにパラメーターを渡すために使用します。 | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | 角かっこです。 配列またはコレクション内の値にアクセスするために使用します。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | じゃない。 逆に、`true`値を`false`およびその逆です。 通常、テストするための短縮形方法として使用`false`(用には、いない`true`)。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&` <code>&#124;&#124;</code> | 論理 AND 条件をまとめてリンクに使用されるかとします。 | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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

`Server.MapPath`メソッドは、仮想パスを変換 (と同様に*/default.cshtml*) への絶対物理パス (と同様に*C:\WebSites\MyWebSiteFolder\default.cshtml*)。 このメソッドを使用すると、いつでも、完全な物理パスを作成する必要がありますにします。 典型的な例は、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成するときです。

通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、このメソッドは、パスを変換できるようにするかが分かって — 仮想パス: サーバー上の対応するパスにします。 ファイルまたはフォルダーをメソッドに仮想パスを渡すし、物理パスを返します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>仮想ルートを参照している: ~ 演算子と Href メソッド

*.Cshtml*または*.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。 これは、機能は、サイトでは、のページを移動することができ、他のページに含まれているすべてのリンクが壊れているされませんので非常に便利です。 これまで、web サイトを別の場所に移動する場合にも便利です。 次にいくつかの例を示します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

場合は、web サイトは`http://myserver/myapp`、方法 ASP.NET が扱うこれらのパス、ページの実行時に次に示します。

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(変数の値としてこれらのパスは実際には表示されませんが、ASP.NET は、パスとして扱うは何でした)。

使用することができます、 `~` (上記と同じ) のサーバー コードと次のように、マークアップの両方の演算子。

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

使用するマークアップで、`~`オペレーターをイメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。 ASP.NET がページ (コードとマークアップの両方) を検索し、すべて解決、ページの実行時に、`~`適切なパスへの参照。

## <a name="conditional-logic-and-loops"></a>条件付きロジック、ループ

ASP.NET サーバー コードでは、条件に基づいて、タスクを実行し、特定回数のステートメントを繰り返し実行するコードを記述することができます (つまり、ループを実行しているコード)。

### <a name="testing-conditions"></a>条件をテストします。

使用する単純な条件をテストする、`if`ステートメントで、指定したテストに基づく true または false を返します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if`キーワード、ブロックを開始します。 実際のテスト (条件) では、かっこ内には、し、true または false を返します。 テストが true の場合に実行するステートメントは、中かっこで囲まれています。 `if`ステートメントを含めることができます、`else`ブロックで、条件が false の場合に実行するステートメントを指定します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

使用して複数の条件を追加することができます、`else if`ブロック。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

場合は、最初の条件の場合、この例ではブロックが真ではありません、`else if`条件をチェックします。 その条件が満たされた場合内のステートメント、`else if`ブロックが実行されます。 条件も満たさない場合、内のステートメント、`else`ブロックが実行されます。 場合、その他の任意の数を追加することができます、ブロックを閉じて、`else`としてブロック、&quot;他のすべて&quot;条件。

多くの条件をテストするには、使用、`switch`ブロック。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

テストする値がかっこ内 (この例で、`weekday`変数)。 各テストを使用して、`case`コロン (:) で終了するステートメント。 場合の値、`case`ステートメントには、テスト値が一致する、そのケース ブロック内のコードを実行します。 各 case ステートメントを閉じる、`break`ステートメントです。 (それぞれに改行を含めるを忘れた場合`case`次のコードをブロックします`case`ステートメントも実行されます。)。A`switch`ブロックに多くの場合は、`default`ステートメントの最後のケースとして、&quot;他のすべて&quot;の他のケースに該当しない場合に実行するオプションです。

ブラウザーに表示される最後の 2 つの条件付きブロックの結果:

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>コードのループ

多くの場合、同じステートメントを繰り返し実行する必要があります。 これを行うをループします。 たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。 使用することが正確に何回かループすることがわかっている場合、`for`ループします。 このようなループ カウントまたはカウント ダウンの際に特に役立ちます。

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

ループはで始まり、`for`キーワード、丸かっこ、3 つのステートメントに続くそれぞれをセミコロンで終了します。

- 最初のステートメントでかっこの内側 (`var i=10;`) カウンターを作成し、10 を初期化します。 カウンターの名前を付ける必要はありません`i`&#8212; 任意の変数を使用することができます。 ときに、`for`ループが実行される、カウンターが自動的にインクリメントされます。
- 2 番目のステートメント (`i < 21;`) をカウントするどの程度の条件を設定します。 ここでは、希望に最大 20 まで移動する (つまり、読み進めて、カウンターが 21 未満)。
- 3 番目のステートメント (`i++` ) は、インクリメント演算子、単に 1 が加算、ループが実行されるたびにカウンターを使用するを指定します。

中かっこ内には、ループの各反復処理を実行するコードを示します。 マークアップは、新しい段落を作成する (`<p>`要素) の値を表示する出力に行を追加し、各`i`(カウンター)。 このページを実行すると、例は、11 行の行のテキスト項目数を示す行ごとに、出力の表示を作成します。

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

コレクションまたは配列を扱う場合場合、多くの場合、使用する、`foreach`ループします。 コレクションは、類似のオブジェクトのグループと`foreach`ループに、コレクション内の各項目にタスクを実行することができます。 この種類のループは、コレクションの便利なためとは異なり、`for`ループすることも、カウンターをインクリメントまたは制限を設定します。 代わりに、`foreach`が終了するまで、このループのコードは、単にコレクションを続行されます。

たとえば、次のコードが内の項目を返します、`Request.ServerVariables`コレクションは、これは、web サーバーに関する情報を格納するオブジェクト。 使用して、`foreac`を新規作成して各アイテムの名前を表示する h ループ`<li>`HTML 箇条書きリスト内の要素。

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach`キーワードが続くかっこをコレクション内の 1 つの項目を表す変数を宣言する (例では、 `var item`) と、その後、`in`キーワード、ループ処理コレクション。 本体で、`foreach`ループ、先ほど宣言した変数を使用する現在のアイテムにアクセスすることができます。

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

汎用的なループを作成するには、使用、`while`ステートメント。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A`while`ループから始まります、`while`後にかっこをループを続行する期間を指定するキーワード (ここでは、限り`countNum`50 未満) を繰り返すブロックします。 ループが通常インクリメント (に追加する) または減分 (減算) 変数またはオブジェクト カウントで使用されます。 例では、`+=`演算子を追加する場合は 1`countNum`ループが実行されるたびにします。 (カウント ダウンをループ内の変数をデクリメントするためには、デクリメント演算子を使用すると`-=`)。

## <a name="objects-and-collections"></a>オブジェクトとコレクション

ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。 このセクションでは、演習では多くの場合、コードでいくつかの重要なオブジェクトについて説明します。

### <a name="page-objects"></a>ページのオブジェクト

ASP.NET の最も基本的なオブジェクトは、ページです。 オブジェクトを修飾せずに直接 page オブジェクトのプロパティにアクセスすることができます。 次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

ようにクリアのプロパティと、現在のページ オブジェクト メソッドを参照していること、必要に応じてキーワードを使用できます、`this`をコード内のページ オブジェクトを表します。 前のコード例を次に示します`this`を追加して、ページを表します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

プロパティを使用することができます、`Page`など多くの情報を取得するオブジェクト。

- `Request`。 既に説明したようにこれは、ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL を含む、現在の要求に関する情報のコレクション。
- `Response`。 これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。 たとえば、このプロパティを使用すると、情報を応答に書き込みます。 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>コレクション オブジェクト (配列とディクショナリ)

A*コレクション*のコレクションなど、同じ種類のオブジェクトのグループは、`Customer`データベースからのオブジェクト。 同様に ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。

多くの場合、コレクション内のデータを操作します。 2 つの共通コレクション型とは、*配列*と*ディクショナリ*です。 配列は、類似した項目のコレクションを格納するたくない個別の各項目を保持する変数を作成するときに便利です。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

配列、特定のデータ型を宣言するなど`string`、 `int`、または`DateTime`です。 変数が配列を含めることができます、宣言に角かっこを追加することを示すために (など`string[]`または`int[]`)。 アイテムの位置 (インデックス) を使用して配列またはを使用してアクセスできます、`foreach`ステートメントです。 配列のインデックスは 0 から始まる &#8212;つまり、最初の項目がある位置 0、2 番目の項目位置は 1 とにします。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

取得することによって、配列内のアイテムの数を指定できます、`Length`プロパティです。 (配列を検索します)、配列内の特定の項目の位置を取得するを使用して、`Array.IndexOf`メソッドです。 行うことができますも逆方向のような配列の内容 (、`Array.Reverse`メソッド) するか、内容を並べ替える (、`Array.Sort`メソッド)。

ブラウザーで表示されている文字列配列のコードの出力:

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

設定または対応する値を取得するキー (または名前) を指定したキー/値ペアのコレクションをディクショナリには。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

使用するディクショナリを作成する、`new`新しい辞書オブジェクトを作成していることを示すキーワードです。 変数を使用して、ディクショナリを割り当てることができます、`var`キーワード。 山かっこを使用してディクショナリ内の項目のデータ型を指定する ( `< >` )。 宣言の最後に、これは、実際には、メソッド、新しいディクショナリを作成するため、かっこのペアを追加する必要があります。

ディクショナリに項目を追加するに呼び出せる、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。 または、キーが示され、次の例のように、単純な割り当てを行うには角かっこを使用できます。

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

ディクショナリから値を取得するには、角かっこでキーを指定します。

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>パラメーターを持つメソッドを呼び出す

この記事の前半の読み取りとメソッド オブジェクトをプログラミングすることができます。 たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッドです。 多くのメソッドには、1 つまたは複数のパラメーターがあります。 A*パラメーター*メソッドに渡す値は、そのタスクを完了する方法を有効にします。 たとえばの宣言を見て、`Request.MapPath`を 3 つのパラメーターを受け取るメソッド。

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(行は読みやすくするためにラッピングされています。 ほぼすべての場所を inside 文字列以外の改行を配置することができますに注意してくださいは引用符で囲まれます)。

このメソッドは、指定した仮想パスに対応するサーバーの物理パスを返します。 次の 3 つのパラメーター、メソッドは、 `virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`です。 (宣言では、パラメーターが表示されることを承諾するデータのデータ型とに注意してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。

Razor 構文は、メソッドにパラメーターを渡すための 2 つのオプションを使用:*位置指定パラメーター*と*名前付きパラメーター*です。 位置指定パラメーターを使用して、メソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。 (通常わかりますこの順序メソッドのドキュメントを読み取ることによってです。)注文を行う必要があり、パラメーターのいずれかの &#8212; をスキップすることはできません。かどうか必要に応じて、空の文字列を渡す (`""`) または`null`位置指定パラメーターの値がないためです。

次の例は、という名前のフォルダーがある前提としています。*スクリプト*、web サイトにします。 コードの呼び出し、`Request.MapPath`メソッドとパスの値が正しい順序で 3 つのパラメーターです。 結果のマップされたパスが表示されます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

メソッドは、多くのパラメーターを持ち、ときにするおくと、コードより読みやすい名前付きパラメーターを使用しています。 名前付きパラメーターを使用してメソッドを呼び出すには、パラメーター名とそれに続くコロン (:)、し、値を指定します。 名前付きパラメーターの利点は、任意の順序で渡すことができます、です。 (欠点は、メソッドの呼び出しがコンパクトではないことです。)

次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターを使用します。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

ご覧のように、異なる順序でパラメーターが渡されます。 ただし、前の例、この例を実行する場合は、同じ値を返すがあります。

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>エラー処理

### <a name="try-catch-statements"></a>Try-catch ステートメント

ステートメントは、上の理由から、コントロールの外部できない可能性があるコードの多くの場合があります。 例:

- コードを作成またはファイルにアクセスしようとすると、あらゆる種類のエラーが表示される場合があります。 目的のファイルが存在しない可能性があります、ロックされる可能性があります、コード可能性がありますいないアクセス許可を持つし、なります。
- 同様に、コードでは、データベース内のレコードを更新しようとすると、アクセス許可の問題があります、データベースへの接続を削除する場合があります、無効な場合とに、データを保存する場合があります。

プログラミング用語では、このような状況が呼び出されます*例外*です。 (スロー) が生成されますが、コードには、例外が発生すると、エラー メッセージの最高でユーザーに迷惑な。

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するために、使用する`try/catch`ステートメントです。 `try`をチェックする場合のコードを実行するステートメントでは、します。 1 つまたは複数`catch`ステートメントを検索できる固有の仕様が発生したエラー (特定の種類の例外)。 多くとして含めることができます`catch`としてするステートメントが予測は、エラーを確認する必要があります。

> [!NOTE]
> 使用しないことをお勧め、`Response.Redirect`メソッド`try/catch`ステートメント、ページに、例外を引き起こす可能性があるためです。


次の例では、最初の要求でテキスト ファイルを作成し、ユーザーがファイルを開くできるボタンを表示するページを示します。 意図的を使って不適切なファイル名と、例外を使用することができるようにします。 コードが含まれています`catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET はフォルダーにも見つからない場合に発生します。 (できますコメントを解除する例では、内のステートメントすべてが正しく機能しているときの動作を確認するためにします。)

コードが例外を処理していない場合、前のスクリーン ショットのようなエラー ページが表示されます。 ただし、`try/catch`セクションでは、ユーザーがこの種のエラーを表示するを防ぐのに役立ちます。

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>その他のリソース

**Visual Basic を使用したプログラミング**


[付録: Visual Basic 言語と構文](https://go.microsoft.com/fwlink/?LinkId=202908)


**リファレンス ドキュメント**


[ASP.NET](https://msdn.microsoft.com/en-us/library/ee532866.aspx)

[C# 言語](https://msdn.microsoft.com/en-us/library/kx37x362.aspx)
