---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: "コント ローラー (VB) の追加 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 74113d76a74b1da27a7f9a33a13038a0c36ad036
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-vb"></a>コント ローラー (VB) の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-controller.md)このチュートリアルのです。


MVC*モデル-ビュー-コント ローラー*です。 MVC では、各部分がある個別の責任を持つようにアプリケーションを開発するためのパターンを示します。

- モデル: アプリケーションのデータ。
- ビュー: テンプレート ファイル、アプリケーションが HTML 応答を動的に生成に使用します。
- コント ローラー: アプリケーションへの着信 URL 要求を処理するクラスは、モデル、データを取得し、クライアントへの応答をレンダリングするテンプレートの表示を指定します。

このチュートリアルでは、これらすべての概念をカバーして行いますを使用してアプリケーションを構築する方法を示します。

右クリックして、新しいコント ローラーを作成、*コント ローラー*フォルダー**ソリューション エクスプ ローラー**を選択し**コント ローラーの追加**です。

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

新しいコント ローラーの名前を付けます&quot;HelloWorldController&quot;  をクリック**追加**です。

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー** 、右側の新しいファイルが作成されている名前付き*HelloWorldController.cs*ファイルが IDE で開かれているとします。

新しい内部`public class HelloWorldController`ブロックは、次のコードのような 2 つの新しいメソッドを作成します。 例として、コント ローラーから直接 HTML の文字列を返します。

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

コント ローラーの名前は`HelloWorldController`および新しい方法が名前付き`Index`します。 (F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。 お使いのブラウザー開始されると、追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (私のコンピューターでが`http://localhost:43246/HelloWorld`) お使いのブラウザーは次のスクリーン ショットのようになります。 上記のメソッドで返される、コード、文字列直接です。 だけの HTML を返すシステムと言いましたと同じです。

![](adding-a-controller/_static/image5.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、どのようなコードが呼び出されるかを制御します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。 URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。 したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照してくださいしなければならなかったことを確認*/HelloWorld*上と`Index`メソッドは、既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;. 既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。 コント ローラーは、この URL の`HelloWorld`と`Welcome`メソッドです。 使用していない、`[Parameters]`まだ URL の一部です。

![](adding-a-controller/_static/image6.png)

みましょうコント ローラーへの URL からの一部のパラメーター情報を渡すおができるように、例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 示す、VB 省略可能なパラメーターの機能を使い切ってしまったことに注意してください、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

アプリケーションを実行しを参照`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**です。** 値が異なるを試みることができます`name`と`numtimes`です。 システムは、メソッドのパラメーターに、アドレス バーに、クエリ文字列から名前付きパラメーターを自動的にマップします。

![](adding-a-controller/_static/image7.png)

これら両方の例で、コント ローラーが実行されて MVC の VC 部分: ビューとコント ローラーの作業です。 コント ローラーは、直接 HTML を返しています。 通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返すしたくありません。 代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。 結果がどのようにこれを見てみましょう。

>[!div class="step-by-step"]
[前へ](intro-to-aspnet-mvc-3.md)
[次へ](adding-a-view.md)
