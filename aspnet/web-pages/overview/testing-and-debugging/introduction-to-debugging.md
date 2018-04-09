---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduction to デバッグの ASP.NET Web Pages (Razor) サイト |Microsoft ドキュメント
author: tfitzmac
description: デバッグは、検索、およびコード ページにエラーの修正のプロセスです。 この章ではするが表示されるツールやデバッグに使用できる手法と analyz をしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introduction to デバッグの ASP.NET Web Pages (Razor) サイト
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトにページをデバッグするさまざまな方法について説明します。 デバッグは、検索、およびコード ページにエラーの修正のプロセスです。
> 
> **学習する内容。** 
> 
> - ときに役立つ情報を表示する方法は、分析して、ページをデバッグします。
> - デバッグを使用する方法は、Visual Studio のツールです。
>   
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - `ServerInfo`ヘルパー。
> - `ObjectInfo` ヘルパー。
>   
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。 WebMatrix 3 を使用できますが、統合されたデバッガーがサポートされていません。


エラーと、コード内の問題のトラブルシューティングの重要な側面は、まずそれを回避することです。 エラーが発生する可能性のあるコードのセクションを設定することによって行うことができます`try/catch`ブロックします。 詳細については、上でのエラー処理セクションを参照してください。 [Introduction to ASP.NET Web プログラミング構文を使用して、Razor](https://go.microsoft.com/fwlink/?LinkId=202890)です。

`ServerInfo`ヘルパーは、ページをホストする web サーバー環境に関する情報の概要を説明する診断ツールです。 ブラウザーがページを要求時に送信される HTTP 要求の情報も示します。 `ServerInfo`ヘルパーは、現在のユーザー id の要求を行ったブラウザーの種類を表示にします。 このような情報は、一般的な問題をトラブルシューティングするのに役立ちます。

1. という名前の新しい web ページを作成する*ServerInfo.cshtml*です。
2. ページの最後に、直前の終了`</body>`タグ、追加`@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    追加することができます、`ServerInfo`ページ内の任意の場所のコード。 末尾に追加すると、その出力から切り離しておきます、他のページ コンテンツを読みやすくなります。

    > [!NOTE] 
    > 
    > **重要な**実稼働サーバーに web ページを移動する前に、web ページからの診断コードを削除する必要があります。 これに適用されます、`ServerInfo`ヘルパーだけでなく、他の診断方法この記事の内容を含むコード ページを追加します。 この種の情報が悪意のある方に役立つ可能性があるために、サーバー、そのような詳細については、サーバー名、ユーザー名、パスに関する情報を表示する、web サイトの訪問者をしたくありません。
3. ページを保存し、ブラウザーで実行します。

    ![デバッグ 1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo`ヘルパー ページの情報の 4 つのテーブルが表示されます。

   - サーバーの構成。 このセクションでは、コンピューター名、バージョンの ASP.NET を実行している、ドメイン名、およびサーバーの時刻を含む、ホストする web サーバーに関する情報を提供します。
   - ASP.NET サーバー変数。 このセクションでは、さまざまな HTTP プロトコル詳細 (と呼ばれる HTTP 変数) の詳細を説明し、値を各 web ページ要求の一部であります。
   - HTTP ランタイム情報。 このセクションでは、バージョンの web ページが実行されている Microsoft .NET Framework では、パス、キャッシュについての詳細については、について詳しく説明します。 (で学習したよう[Introduction to ASP.NET Web プログラミング構文を使用して、Razor](https://go.microsoft.com/fwlink/?LinkId=202890)構文が広範なソフトウェア上に構築された自体である Microsoft の ASP.NET web サーバー テクノロジに基づいて構築されて Razor を使用して ASP.NET Web ページ開発ライブラリ、.NET Framework と呼ばれます。)
   - 環境変数。 このセクションでは、web サーバーですべてのローカルの環境変数とその値の一覧を示します。

     すべてのサーバーと要求情報の詳細についてはこの記事の範囲外ですがあることがわかります、`ServerInfo`ヘルパーは、多くの診断情報を返します。 値の詳細についてを`ServerInfo`戻り値を参照してください[環境変数の認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)、Microsoft TechNet web サイトと[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN web サイトです。

## <a name="embedding-output-expressions-to-display-page-values"></a>埋め込み式をページの値を表示するには

別の方法で、コードで何が起こっているかを参照してください ページで、出力の式を埋め込むを開始します。 ようなものを追加することで、変数の値を直接出力ご存じのように、`@myVariable`または`@(subTotal * 12)`ページにします。 をデバッグする場合は、コード内で重要なポイントこれらの式の出力を配置できます。 これにより、ページの実行時に、主要な変数またはの計算結果の値を参照してください。 終了すると、デバッグ、式を削除したり、コメント アウトします。この手順は、ページをデバッグする際に埋め込み式を使用する一般的な方法を示しています。

1. 名前は新しい WebMatrix ページを作成する*OutputExpression.cshtml*です。
2. 次のページに次のようにコンテンツを置き換えます。

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    この例では、`switch`の値を確認するステートメント、`weekday`変数とは、週の曜日によって別の出力メッセージを表示します。 この例で、`if`最初のコード ブロック内のブロックが 1 日を現在の曜日の値に追加して週の曜日を任意に変更します。 これは、わかりやすくするために導入されたエラーです。
3. ページを保存し、ブラウザーで実行します。

    ページは、間違った曜日のメッセージを表示します。 すべての日、週の実際には、1 日の後で、メッセージが表示されます。 ここではわかっている理由、メッセージがオフ (ため、コードでは、意図的に正しくない日付の値を設定します) が、実際には多くの場合、作業がうまくいかないコード内を知ることが難しい。 デバッグするには、何が起こっているかの主要なオブジェクトおよび変数の値になどを確認する必要があります`weekday`です。
4. 出力の式を挿入することによって追加`@weekday`コード内のコメントで示される 2 つの場所で示すようにします。 これらの式の出力は、コードの実行に、変数の値をその時点で表示されます。

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 保存し、ブラウザーでページを実行します。

    ページが、実際の曜日を最初に、表示し、更新された日、週の結果得られる、1 日とし、結果のメッセージからの追加、`switch`ステートメントです。 2 つの変数の式からの出力 (`@weekday`)、スペースを含まない、日の間で、任意の HTML を追加していないため`<p>`出力にタグを式は、テストするためだけです。

    ![デバッグ 2](introduction-to-debugging/_static/image2.jpg)

    これで、エラーが表示できます。 表示するとき、`weekday`コードに変数が、正しい日はすべて表示します。 表示するときに、2 回目の後に、`if`コードのブロックは、1 日が 1 つではオフです。 したがって、曜日の変数の最初と 2 番目の外観の間で問題が発生したことがわかります。 これが実際のバグの場合、このようなアプローチが役立つ問題の原因となったコードの場所を絞り込みます。
6. ページで、コードを修正して追加した 2 つの出力式の消去を週の曜日を変更するコードを削除します。 コードの残り、完全なブロックは、次の例のようになります。

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. ブラウザーでページを実行します。 今度は、メッセージが表示正しいを週の実際の日付が表示されます。

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>ObjectInfo ヘルパーを使用してオブジェクトの値を表示するには

`ObjectInfo`ヘルパーは、型とそれに渡す各オブジェクトの値が表示されます。 使用できます (前の例で出力式で行った) 同じように、コード内の変数およびオブジェクトの値を表示すると、データ型のオブジェクトに関する情報を確認できます。

1. という名前のファイルを開く*OutputExpression.cshtml*先ほど作成しました。
2. ページ内のすべてのコードを次のコード ブロックに置き換えます。

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 保存し、ブラウザーでページを実行します。

    ![デバッグ 4](introduction-to-debugging/_static/image3.jpg)

    この例では、`ObjectInfo`ヘルパーには、2 つの項目が表示されます。

   - 型。 最初の変数の型は`DayOfWeek`します。 2 番目の変数の型は`String`します。
   - 値。 この場合、ページに既にのあいさつの変数の値を表示するため、値が表示されますもう一度に変数を渡すときに`ObjectInfo`です。

     複雑なオブジェクトの場合、`ObjectInfo`ヘルパーは、詳細情報を表示できます&#8212;型とすべてのオブジェクトのプロパティの値を表示、基本的に、します。

## <a name="using-debugging-tools-in-visual-studio"></a>Visual Studio でのデバッグ ツールの使用

Visual Studio 2013 または無料を使用して、包括的なデバッグ機能の[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)です。 Visual Studio では、調査対象の行で、コードでブレークポイントを設定できます。

![ブレークポイントの設定](introduction-to-debugging/_static/image1.png)

Web サイトをテストするときにコードの実行がブレークポイントで停止します。

![ブレークポイントに到達します。](introduction-to-debugging/_static/image2.png)

変数、およびステップ実行、コードで行の現在の値を確認することができます。

![値を参照してください。](introduction-to-debugging/_static/image3.png)

Visual Studio での統合デバッガーを使用して、ASP.NET Razor ページをデバッグする方法については、次を参照してください。[プログラミング ASP.NET Web Pages (Razor) を使用してクトリ](https://go.microsoft.com/fwlink/?LinkId=205854)です。

## <a name="additional-resources"></a>その他のリソース

- [Visual Studio を使用してプログラミングする ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)
- [環境変数を認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)
