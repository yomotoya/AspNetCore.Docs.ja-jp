---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: ページ (Razor) サイトを ASP.NET Web のデバッグの概要 |Microsoft Docs
author: tfitzmac
description: デバッグは、検出とコード ページ内エラーを修正のプロセスです。 この章ではいくつかのツールとデバッグに使用できる手法と率の分析をしています.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: fb9a69d568e10ddafd71e2b9600b3dae21576807
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794990"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>ページ (Razor) サイトを ASP.NET Web のデバッグの概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトのページをデバッグするさまざまな方法について説明します。 デバッグは、検出とコード ページ内エラーを修正のプロセスです。
>
> **学習内容。**
>
> - 役立つ情報を表示する方法は、分析し、ページをデバッグします。
> - デバッグを使用する方法は、Visual Studio のツールです。
>
>
> この記事で導入された ASP.NET 機能を次に示します。
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
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。 WebMatrix 3 を使用できますが、統合デバッガーがサポートされていません。


エラーと、コード内の問題のトラブルシューティングの重要な側面は、最初にそれらを回避するためには。 エラーが発生する可能性のあるコードのセクションを配置することで行うことができます`try/catch`ブロックします。 詳細については、上でのエラー処理セクションをご覧ください。 [Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=202890)します。

`ServerInfo`ヘルパーは、ページをホストする web サーバー環境に関する情報の概要を示す診断ツール。 ブラウザーのページを要求時に送信される HTTP 要求の情報も示します。 `ServerInfo`ヘルパーは、現在のユーザー id の要求を行ったブラウザーの種類を表示します。 これにします。 この種の情報は、一般的な問題をトラブルシューティングするのに役立ちます。

1. という名前の新しい web ページを作成する*ServerInfo.cshtml*します。
2. ページの末尾には、直前の終了`</body>`タグを追加`@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    追加することができます、`ServerInfo`ページ内の任意の場所のコード。 末尾に追加するには、出力から分離して他のページのコンテンツを読みやすくなります。

    > [!NOTE]
    >
    > **重要な**実稼働サーバーへの web ページを移動する前に、web ページから診断コードを削除する必要があります。 これに適用されます、`ServerInfo`ヘルパーとこの記事でコード ページへの追加に関連するその他の診断技法。 この種の情報が悪意のある方に役立つ可能性があるため、web サイトの訪問者、server、および同様の詳細については、サーバー名、ユーザー名、パスを参照してください。 には必要ありません。
3. ページを保存し、ブラウザーで実行します。

    ![デバッグ-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo`ヘルパー ページの情報の 4 つのテーブルを表示します。

   - サーバーの構成。 このセクションでは、コンピューター名、ASP.NET を実行している、ドメイン名、およびサーバーの時刻のバージョンを含む、ホスト web サーバーについてを説明します。
   - ASP.NET サーバー変数。 このセクションでは、多くの HTTP プロトコルの詳細 (呼び出された HTTP 変数) について詳しく説明し、値を各 web ページ要求の一部であります。
   - HTTP ランタイム情報。 このセクションでは、バージョンの Microsoft .NET Framework で web ページが実行されている、パス、キャッシュについての詳細については、詳細を提供します。 (で説明したよう[Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=202890)構文は、広範なソフトウェア上に構築自体がマイクロソフトの ASP.NET web server テクノロジに基づいて構築され、Razor を使用して ASP.NET Web ページ開発ライブラリ、.NET Framework と呼ばれます。)
   - 環境変数。 このセクションは、web サーバー上のすべてのローカルの環境変数とその値の一覧を示します。

     すべてのサーバーと要求の情報の完全な説明については、この記事の範囲外、ことを確認できますが、`ServerInfo`ヘルパーが、多くの診断情報を返します。 値の詳細についてを`ServerInfo`戻り値を参照してください[環境変数の認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)、Microsoft TechNet web サイトと[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN web サイト。

## <a name="embedding-output-expressions-to-display-page-values"></a>ページの値を表示する埋め込み式

コードで何が起こっているかを確認するもう 1 つの方法では、ページに出力式を埋め込むです。 ようなものを追加することで、変数の値を直接出力ご存知のように、`@myVariable`または`@(subTotal * 12)`ページにします。 デバッグについては、コードで重要なポイントをこれらの式の出力を配置できます。 これにより、ページの実行時に、主要な変数または計算の結果の値を参照してください。 完了したら、デバッグ、式を削除したり、コメント アウトします。この手順は、ページをデバッグする際に埋め込み式を使用する一般的な方法を示しています。

1. という新しい WebMatrix ページ作成*OutputExpression.cshtml*します。
2. 次のようにコンテンツのページに置き換えます。

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    この例では、`switch`の値を確認するステートメント、`weekday`変数とはどの曜日に応じて異なる出力メッセージを表示します。 この例で、`if`最初のコード ブロック内のブロックが 1 日を現在の曜日の値に追加することで週の曜日を任意に変更します。 これは、わかりやすくするために導入されたエラーです。
3. ページを保存し、ブラウザーで実行します。

    ページでは、間違った曜日のメッセージが表示されます。 実際には、その週のすべての日、1 日以降、メッセージが表示されます。 (コードは、意図的に正しくない日付の値を設定する) ためにのオフ、メッセージが理由はわかってここでは、実際には多くの場合、処理は、コードで問題がどこかを知ることが難しい。 デバッグするには、何が起こっているかの主要なオブジェクトと変数の値になどを確認する必要があります`weekday`します。
4. 挿入して出力式を追加`@weekday`コード内のコメントで示される 2 つの場所で示すようにします。 これらの出力式では、コードの実行に、変数の値をその時点で表示されます。

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 保存し、ブラウザーでページを実行します。

    ページが最初に、実際の曜日を表示した後 1 日とし、結果のメッセージからの追加を結果の更新された曜日、`switch`ステートメント。 2 つの変数の式からの出力 (`@weekday`) 任意の HTML を追加していないため、日の間のスペースがない`<p>`出力にタグをテストするためだけは、式は。

    ![デバッグ-2](introduction-to-debugging/_static/image2.jpg)

    エラーを含むことがわかります。 表示すると、`weekday`コードに変数が、正しい日はすべて表示します。 表示すると、2 番目の時間後に、`if`コードのブロック、1 日が 1 つがオフです。 したがって、曜日の変数の最初と 2 つ目の外観の間で何かが発生したことがわかります。 これが、実際のバグの場合、この種のアプローチに役立つ、問題の原因となっているコードの場所を絞り込みます。
6. ページで、追加した 2 つの出力の式を削除して、週の曜日を変更するコードを削除して、コードを修正します。 コードの残り、完了ブロックは、次の例のようになります。

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. ブラウザーでページを実行します。 この時間、実際の曜日が表示されます正しいメッセージが表示します。

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>ObjectInfo ヘルパーを使用して、オブジェクトの値を表示するには

`ObjectInfo`ヘルパー型とそれに渡す各オブジェクトの値を表示します。 (出力式は、前の例で行った) など、コード内の変数およびオブジェクトの値を表示する使用すると、データ型のオブジェクトに関する情報を確認できます。

1. という名前のファイルを開く*OutputExpression.cshtml*先ほど作成しました。
2. ページ内のすべてのコードを次のコード ブロックに置き換えます。

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 保存し、ブラウザーでページを実行します。

    ![デバッグ-4](introduction-to-debugging/_static/image3.jpg)

    この例で、`ObjectInfo`ヘルパーは、2 つの項目を表示します。

   - 型。 最初の変数の型は`DayOfWeek`します。 2 番目の変数の型は`String`します。
   - 値。 この場合は、既に ページで、あいさつの変数の値を表示するため、値が表示されますもう一度に変数を渡すときに`ObjectInfo`します。

     複雑なオブジェクトの場合、`ObjectInfo`ヘルパーは、詳細情報を表示できる&#8212;基本的には、型とすべてのオブジェクトのプロパティの値を表示できます。

## <a name="using-debugging-tools-in-visual-studio"></a>Visual Studio でのデバッグ ツールの使用

デバッグ エクスペリエンスをより包括的な使用[Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)します。 Visual Studio を使用して検査する行で、コードでブレークポイントを設定できます。

![ブレークポイントの設定](introduction-to-debugging/_static/image1.png)

Web サイトをテストするときに実行中のコードは、ブレークポイントで停止します。

![ブレークポイントに到達します。](introduction-to-debugging/_static/image2.png)

変数、およびコードで行をステップの現在の値を確認することができます。

![値を参照してください。](introduction-to-debugging/_static/image3.png)

Visual Studio で、統合デバッガーを使用して、ASP.NET Razor ページをデバッグする方法については、次を参照してください。[プログラミング ASP.NET Web Pages (Razor) を使用して Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)します。

## <a name="additional-resources"></a>その他のリソース

- [Visual Studio を使用して ASP.NET Web ページ (Razor) のプログラミング](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)
- [環境変数を認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)
