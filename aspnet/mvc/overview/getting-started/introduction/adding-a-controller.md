---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: コント ローラーの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119264"
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC の略*モデル-ビュー-コント ローラー*します。 MVC は、うまく設計された、テスト可能な簡単に管理されるアプリケーションを開発するためのパターンです。 MVC ベースのアプリケーションが含まれます。

- **M**デル: アプリケーションのデータを表すし、そのデータに対するビジネス ルールを適用する検証ロジックを使用するクラス。
- **V**ュー: アプリケーションは、HTML 応答を動的に生成に使用されるテンプレート ファイル。
- **C**ントローラー: ブラウザーの着信要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。

このチュートリアル シリーズでこれらすべての概念についての説明がされ、アプリケーションの構築に使用する方法を示します。

> [!NOTE]
> 前の手順では、既定の MVC テンプレートが選択されました。 アカウントと既定のコント ローラーの管理、作成します。

コント ローラー クラスを作成してから始めましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*フォルダーをクリック**追加**、し**コント ローラー**します。


![](adding-a-controller/_static/image1.png)

**スキャフォールディングの追加**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラー - 空**、 をクリックし、**追加**。

![](adding-a-controller/_static/image2.png)  
 

新しいコント ローラー"HelloWorldController"という名前を**追加**します。

![コント ローラーを追加します。](adding-a-controller/_static/image3.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されたこと名前付き*HelloWorldController.cs*と新しいフォルダー *Views\HelloWorld*します。 コント ローラーは、IDE で開いています。

![](adding-a-controller/_static/image4.png)

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーのメソッドでは、例として、HTML の文字列を返します。 コント ローラーの名前は`HelloWorldController`最初のメソッドの名前は`Index`します。 ブラウザーから呼び出すことしましょう。 (F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。 ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (たとえば、次の図で`http://localhost:1234/HelloWorld.`)、ブラウザーでページが次のスクリーン ショットのようになります。 上記のメソッドでは、コードは、文字列を直接返します。 いくつかの HTML を返すだけに、システムの指示おり、そうしました。

![](adding-a-controller/_static/image5.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、このような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

ルーティングの形式を設定する、*アプリ\_Start/RouteConfig.cs*ファイル。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

アプリケーションを実行しない URL セグメントを指定すると、既定値は、"Home"コント ローラーと"Index"アクション メソッドには、上記のコードの既定値のセクションで指定します。

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。 URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。 したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったこと */HelloWorld*と`Index`メソッドが既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。 URL セグメントの 3 番目の部分 (`Parameters`) はルート データ用です。 ルート データは、このチュートリアルで後で表示されます。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;. 既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image6.png)

みましょうコント ローラーに、URL からいくつかのパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用していることを示す注、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> セキュリティに関する注意: 使用して上記コード[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)悪意のある入力 (つまり JavaScript) からアプリケーションを保護します。 詳細については、次を参照してください。[方法: 保護に対してスクリプトによる攻略の文字列を HTML エンコードを適用することによって Web アプリケーションで](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)します。


 アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 さまざまな値を試みることができます`name`と`numtimes`URL にします。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップされます。

![](adding-a-controller/_static/image7.png)

URL セグメント上のサンプル ( `Parameters`) が使用されていない、`name`と`numTimes`パラメーターとして渡されます[クエリ文字列](http://en.wikipedia.org/wiki/Query_string)します。 ? (疑問符) 区切り記号では、上記の URL とクエリ文字列に従ってください。 &amp; 文字は、クエリ文字列を区切ります。

[ようこそ] のメソッドを次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

アプリケーションを実行し、次の URL を入力します。 `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

この時間 3 番目の URL セグメントに一致するルート パラメーター `ID.` 、`Welcome`アクション メソッドにパラメーターが含まれています (`ID`) 内の URL 仕様に一致する、`RegisterRoutes`メソッド。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

ASP.NET MVC アプリケーションでは、クエリ文字列として渡すよりも、ルート データ (上記の ID を使用したとき) などとしてパラメーターを渡すより一般的です。 両方に渡すルートを追加することも、`name`と`numtimes`url ルート データとしてパラメーター。 *アプリ\_\routeconfig.cs*ファイルに、「こんにちは」のルートを追加します。

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

アプリケーションを実行しを参照する`/localhost:XXX/HelloWorld/Welcome/Scott/3`します。

![](adding-a-controller/_static/image9.png)

多くの MVC アプリケーションでは、既定のルートが正しく動作します。 モデル バインダーを使用してデータを渡すには、このチュートリアルの後半でを学習して、そのため、既定のルートを変更する必要はありません。

これらの例で、コント ローラーが実行されて、 &quot;VC&quot;の MVC 部分: ビューとコント ローラーの作業は、します。 コント ローラーは、HTML を直接返しています。 通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーは必要ありません。 代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。 そのため方法で [次へ] を見てみましょう。

> [!div class="step-by-step"]
> [前へ](getting-started.md)
> [次へ](adding-a-view.md)
