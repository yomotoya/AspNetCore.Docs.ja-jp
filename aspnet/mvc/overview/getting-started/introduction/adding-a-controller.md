---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "コント ローラーの追加 |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: c8f317b2ac133f560461917af1588b7a1fa51c4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

MVC*モデル-ビュー-コント ローラー*です。 MVC が適切に設計された、テストが容易な保守し、やすいアプリケーションの開発パターンです。 MVC ベースのアプリケーションが含まれます。

- **M** odels: アプリケーションのデータを表すし、そのデータにビジネス ルールを適用する検証ロジックを使用するクラス。
- **V** iews: アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイル。
- **C** ontrollers: ブラウザーの入力方向の要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。

このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。

> [!NOTE]
> 前の手順では、既定の MVC テンプレートが選択されました。 アカウントと既定ではコント ローラーの管理、作成します。

コント ローラー クラスを作成してみましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*フォルダーをクリックして**追加**、し**コント ローラー**です。


![](adding-a-controller/_static/image1.png)

**追加 Scaffold**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラー - 空**、順にクリック**追加**です。

![](adding-a-controller/_static/image2.png)  
 

新しいコント ローラー"HelloWorldController"という名前をクリックして**追加**です。

![コント ローラーを追加します。](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*と新しいフォルダー *Views\HelloWorld*です。 コント ローラーは、IDE で開いています。

![](adding-a-controller/_static/image4.png)

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーのメソッドでは、例として、HTML の文字列を返します。 コント ローラーの名前は`HelloWorldController`で最初のメソッドの名前が`Index`です。 ブラウザーから呼び出すことしてみましょう。 (F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。 ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (たとえば、以下の図で`http://localhost:1234/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。 上記のメソッドで返される、コード、文字列直接です。 システムだけの HTML を返すように通知してと同じです。

![](adding-a-controller/_static/image5.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

ルーティングの書式を設定する、*アプリ\_Start/RouteConfig.cs*ファイル。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

アプリケーションを実行し、URL セグメントを指定しないで、既定値は「ホーム」コント ローラーと"Index"アクション メソッドが上記のコードの既定値 セクションで指定されます。

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。 URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。 したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったことを確認*/HelloWorld*と`Index`メソッドは、既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。 URL セグメントの 3 番目の部分 (`Parameters`) はルート データ用です。 ルート データは、このチュートリアルで後で表示されます。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;. 既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image6.png)

みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> セキュリティに関する注意: 使用して上記コード[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)悪意のある入力 (つまり JavaScript) から、アプリケーションを保護します。 詳細については、次を参照してください。[する方法: 保護に対してスクリプトによる攻略の文字列を HTML エンコードを適用することによって、Web アプリケーションで](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)です。


 アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 値が異なるを試みることができます`name`と`numtimes`URL にします。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターが自動的にマップします。

![](adding-a-controller/_static/image7.png)

URL セグメントの上のサンプル ( `Parameters`) を使用していない、`name`と`numTimes`パラメーターとして渡されます[クエリ文字列](http://en.wikipedia.org/wiki/Query_string)です。 ? (疑問符) 上記の URL では、区切り記号を次のクエリ文字列。 &amp; 文字は、クエリ文字列を区切ります。

ようこそメソッドを次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

アプリケーションを実行し、次の URL を入力します。`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

今度は、3 番目の URL セグメントが一致するルートのパラメーター `ID.` 、`Welcome`アクション メソッドにパラメーターが含まれています (`ID`) の URL の仕様に一致する、`RegisterRoutes`メソッドです。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

ASP.NET MVC アプリケーションでは、一般的なクエリ文字列として渡すことよりもルート データ (同じように上記の ID を持つ) としてパラメーターを渡す、します。 両方を渡すへのルートを追加することも、`name`と`numtimes`url ルート データとして使用されるパラメーターにします。 *アプリ\_Start\RouteConfig.cs*ファイルに「こんにちは」のルートを追加します。

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

アプリケーションを実行しを参照`/localhost:XXX/HelloWorld/Welcome/Scott/3`です。

![](adding-a-controller/_static/image9.png)

多くの MVC アプリケーションの既定のルートは問題なく動作します。 モデル バインダーを使用してデータを渡すには、このチュートリアルで後ほど説明して、その既定のルートを変更する必要はありません。

これらの例で、コント ローラーが実行されて、 &quot;VC&quot; MVC 一部分: ビューとコント ローラーの作業は、します。 コント ローラーは、直接 HTML を返しています。 通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。 代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。 結果がどのようにこれを次のことを確認してみましょう。

>[!div class="step-by-step"]
[前へ](getting-started.md)
[次へ](adding-a-view.md)
