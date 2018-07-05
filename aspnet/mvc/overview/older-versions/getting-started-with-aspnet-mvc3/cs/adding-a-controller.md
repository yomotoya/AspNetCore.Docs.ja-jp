---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: コント ローラー (c#) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express サービス パック 1、どの i を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: ba6cc715f8c8eaf624ab5314e3afbfd68da11485
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370893"
---
<a name="adding-a-controller-c"></a>コント ローラー (c#) の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの。


MVC の略*モデル-ビュー-コント ローラー*します。 MVC は、うまく設計された、簡単に管理されるアプリケーションを開発するためのパターンです。 MVC ベースのアプリケーションが含まれます。

- コント ローラー: アプリケーションへの着信要求を処理するクラスは、モデル、データを取得し、クライアントに応答を返すビュー テンプレートを指定します。
- モデルのクラス、アプリケーションのデータを表すし、そのデータに対するビジネス ルールを適用する検証ロジックを使用します。
- HTML 応答を動的に生成するアプリケーションを使用するテンプレート ファイルのビュー:

このチュートリアル シリーズでこれらすべての概念についての説明がされ、アプリケーションの構築に使用する方法を示します。

コント ローラー クラスを作成してから始めましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**します。

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

新しいコント ローラー"HelloWorldController"の名前を付けます。 既定のテンプレートとしてのままに**空のコント ローラー**  をクリック**追加**します。

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されたこと名前付き*HelloWorldController.cs*します。 ファイルは、IDE で開いています。

![](adding-a-controller/_static/image5.png)

内で、`public class HelloWorldController`ブロックで、次のコードのような 2 つのメソッドを作成します。 コント ローラーでは、例として、HTML の文字列を返します。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーの名前は`HelloWorldController`と上記の最初のメソッドが名前付き`Index`します。 ブラウザーから呼び出すことしましょう。 (F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。 ブラウザーでは、アドレス バーにパスに"HelloWorld"を追加します。 (たとえば、次の図で`http://localhost:43246/HelloWorld.`)、ブラウザーでページが次のスクリーン ショットのようになります。 上記のメソッドでは、コードは、文字列を直接返します。 いくつかの HTML を返すだけに、システムの指示おり、そうしました。

![](adding-a-controller/_static/image6.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、このような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。 URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。 したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったこと */HelloWorld*と`Index`メソッドが既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome` メソッドが実行され、"This is the Welcome action method..." という文字列を返します。 既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image7.png)

みましょうコント ローラーに、URL からいくつかのパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用していることを示す注、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`します。 さまざまな値を試みることができます`name`と`numtimes`URL にします。 システムでは、メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターが自動的にマップされます。

![](adding-a-controller/_static/image8.png)

これら両方の例で、コント ローラーが実行されて MVC の"VC"部分: ビューとコント ローラーの作業は、します。 コント ローラーは、HTML を直接返しています。 通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーは必要ありません。 代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。 そのため方法で [次へ] を見てみましょう。

> [!div class="step-by-step"]
> [前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)
