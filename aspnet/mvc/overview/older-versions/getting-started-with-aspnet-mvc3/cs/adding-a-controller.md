---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: コント ローラー (c#) の追加 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express サービス パック 1、どの i を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-c"></a>コント ローラー (c#) の追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。


MVC*モデル-ビュー-コント ローラー*です。 MVC は、適切に設計された、簡単に管理は、アプリケーションの開発パターンです。 MVC ベースのアプリケーションが含まれます。

- コント ローラー: アプリケーションへの着信要求を処理するクラスはモデル データを取得し、クライアントへの応答を返すビュー テンプレートを指定します。
- モデルのクラスを表すアプリケーションのデータとそのデータにビジネス ルールを適用する検証ロジックを使用します。
- アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイルのビュー:

このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。

コント ローラー クラスを作成してみましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

新しいコント ローラー"HelloWorldController"の名前を付けます。 として既定のテンプレートのままにして**空のコント ローラー**  をクリック**追加**です。

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*です。 ファイルは、IDE で開いています。

![](adding-a-controller/_static/image5.png)

内部、`public class HelloWorldController`ブロックは、次のコードのような 2 つのメソッドを作成します。 コント ローラーでは、例として、HTML の文字列を返します。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーの名前は`HelloWorldController`で上記の最初のメソッドの名前が`Index`です。 ブラウザーから呼び出すことしてみましょう。 (F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。 ブラウザーでは、アドレス バーにパスに"HelloWorld"を追加します。 (たとえば、以下の図で`http://localhost:43246/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。 上記のメソッドで返される、コード、文字列直接です。 システムだけの HTML を返すように通知してと同じです。

![](adding-a-controller/_static/image6.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。 URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。 したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったことを確認*/HelloWorld*と`Index`メソッドは、既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome` メソッドが実行され、"This is the Welcome action method..." という文字列を返します。 既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image7.png)

みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`です。 値が異なるを試みることができます`name`と`numtimes`URL にします。 システムは、メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップします。

![](adding-a-controller/_static/image8.png)

これら両方の例で、コント ローラーが実行されて、"VC"の部分 MVC: ビューとコント ローラーの作業は、します。 コント ローラーは、直接 HTML を返しています。 通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。 代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。 結果がどのようにこれを次のことを確認してみましょう。

> [!div class="step-by-step"]
> [前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)
