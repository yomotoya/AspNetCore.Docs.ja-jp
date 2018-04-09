---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: コント ローラーの追加 |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: bb76c0a87d935322406b9d8e18fbdb3e41f327f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


MVC*モデル-ビュー-コント ローラー*です。 MVC が適切に設計された、テストが容易な保守し、やすいアプリケーションの開発パターンです。 MVC ベースのアプリケーションが含まれます。

- **M** odels: アプリケーションのデータを表すし、そのデータにビジネス ルールを適用する検証ロジックを使用するクラス。
- **V** iews: アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイル。
- **C** ontrollers: ブラウザーの入力方向の要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。

このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。

コント ローラー クラスを作成してみましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。

![](adding-a-controller/_static/image1.png)

新しいコント ローラーの名前を付けます&quot;HelloWorldController&quot;です。 として既定のテンプレートのままにして**空の MVC コント ローラー**  をクリック**追加**です。

![コント ローラーを追加します。](adding-a-controller/_static/image2.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*です。 ファイルは、IDE で開いています。

![](adding-a-controller/_static/image3.png)

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーのメソッドでは、例として、HTML の文字列を返します。 コント ローラーの名前は`HelloWorldController`で上記の最初のメソッドの名前が`Index`です。 ブラウザーから呼び出すことしてみましょう。 (F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。 ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (たとえば、以下の図で`http://localhost:1234/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。 上記のメソッドで返される、コード、文字列直接です。 システムだけの HTML を返すように通知してと同じです。

![](adding-a-controller/_static/image4.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。 URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。 したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったことを確認*/HelloWorld*と`Index`メソッドは、既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;. 既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image5.png)

みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`です。 値が異なるを試みることができます`name`と`numtimes`URL にします。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターが自動的にマップします。

![](adding-a-controller/_static/image6.png)

これら両方の例で、コント ローラーが実行されて、 &quot;VC&quot; MVC 一部分: ビューとコント ローラーの作業は、します。 コント ローラーは、直接 HTML を返しています。 通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。 代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。 結果がどのようにこれを次のことを確認してみましょう。

> [!div class="step-by-step"]
> [前へ](intro-to-aspnet-mvc-4.md)
> [次へ](adding-a-view.md)
