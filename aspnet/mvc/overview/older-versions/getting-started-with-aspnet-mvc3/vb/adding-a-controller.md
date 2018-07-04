---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: コント ローラー (VB) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 75451be20bd39f37bb692a06389ed7f04bc75a2a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367853"
---
<a name="adding-a-controller-vb"></a>コント ローラー (VB) の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-controller.md)このチュートリアルの。


MVC の略*モデル-ビュー-コント ローラー*します。 MVC は、各部分がある別の責任になるようにアプリケーションを開発するためのパターンです。

- モデル: アプリケーションのデータ。
- ビュー: 動的 HTML 応答を生成する、アプリケーションが使用されるテンプレート ファイル。
- コント ローラー: アプリケーションへの受信 URL 要求を処理するクラスは、モデル、データを取得し、クライアントへの応答をレンダリングするビュー テンプレートを指定します。

このチュートリアルでは、これらすべての概念をカバーするがされ、アプリケーションの構築に使用する方法を示します。

右クリックし、新しいコント ローラーを作成、*コント ローラー*フォルダー**ソリューション エクスプ ローラー**選択**コント ローラーの追加**します。

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

新しいコント ローラー &quot;HelloWorldController&quot;クリック**追加**します。

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー** 、右側の新しいファイルが作成されたことという名前*HelloWorldController.cs*ファイルが IDE で開かれているとします。

新しい内部`public class HelloWorldController`ブロックで、次のコードのように 2 つの新しいメソッドを作成します。 例として、コント ローラーから直接 HTML の文字列を返しますします。

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

コント ローラーの名前は`HelloWorldController`と新しいメソッドが名前付き`Index`します。 (F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。 お使いのブラウザーが開始されると追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (私のコンピューターでが`http://localhost:43246/HelloWorld`)、ブラウザーは次のスクリーン ショットのようになります。 上記のメソッドでは、コードは、文字列を直接返します。 いくつかの HTML を返すだけに、システムと言いましたおり、そうしました。

![](adding-a-controller/_static/image5.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、このような形式を使用して、どのようなコードが呼び出されるかを制御します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。 URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。 したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照してくださいあったことに注意してください */HelloWorld*上、`Index`メソッドが既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;. 既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。 コント ローラーは、この url で`HelloWorld`と`Welcome`メソッドです。 私たちが使用されていない、`[Parameters]`まだ URL の一部です。

![](adding-a-controller/_static/image6.png)

みましょうコント ローラーに、URL からの一部のパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 いることを示す、VB の省略可能なパラメーターの機能を使用したことに注意してください、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

アプリケーションを実行しを参照する`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**します。** さまざまな値を試みることができます`name`と`numtimes`します。 システムでは、メソッドのパラメーターに、アドレス バーで、クエリ文字列から名前付きパラメーターが自動的にマップされます。

![](adding-a-controller/_static/image7.png)

これら両方の例で、コント ローラーが実行されて MVC の VC 部分: ビューとコント ローラーの作業です。 コント ローラーは、HTML を直接返しています。 通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーのたくはありません。 代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。 そのため方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)
