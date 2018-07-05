---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: コント ローラーの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 100f5623bafa33548f9c979e98765c92327eb369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373290"
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


MVC の略*モデル-ビュー-コント ローラー*します。 MVC は、うまく設計された、テスト可能な簡単に管理されるアプリケーションを開発するためのパターンです。 MVC ベースのアプリケーションが含まれます。

- **M**デル: アプリケーションのデータを表すし、そのデータに対するビジネス ルールを適用する検証ロジックを使用するクラス。
- **V**ュー: アプリケーションは、HTML 応答を動的に生成に使用されるテンプレート ファイル。
- **C**ントローラー: ブラウザーの着信要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。

このチュートリアル シリーズでこれらすべての概念についての説明がされ、アプリケーションの構築に使用する方法を示します。

コント ローラー クラスを作成してから始めましょう。 **ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**します。

![](adding-a-controller/_static/image1.png)

新しいコント ローラー &quot;HelloWorldController&quot;します。 既定のテンプレートとしてのままに**空の MVC コント ローラー**  をクリック**追加**します。

![コント ローラーを追加します。](adding-a-controller/_static/image2.png)

**ソリューション エクスプ ローラー**新しいファイルが作成されたこと名前付き*HelloWorldController.cs*します。 ファイルは、IDE で開いています。

![](adding-a-controller/_static/image3.png)

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

コント ローラーのメソッドでは、例として、HTML の文字列を返します。 コント ローラーの名前は`HelloWorldController`と上記の最初のメソッドが名前付き`Index`します。 ブラウザーから呼び出すことしましょう。 (F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。 ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。 (たとえば、次の図で`http://localhost:1234/HelloWorld.`)、ブラウザーでページが次のスクリーン ショットのようになります。 上記のメソッドでは、コードは、文字列を直接返します。 いくつかの HTML を返すだけに、システムの指示おり、そうしました。

![](adding-a-controller/_static/image4.png)

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、このような形式を使用して、呼び出すには、どのようなコードを確認します。

`/[Controller]/[ActionName]/[Parameters]`

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。 URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。 したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。 のみを参照しなければならなかったこと */HelloWorld*と`Index`メソッドが既定で使用されました。 これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。

`http://localhost:xxxx/HelloWorld/Welcome` を参照します。 `Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;. 既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。 この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。 URL の `[Parameters]` の部分はまだ使っていません。

![](adding-a-controller/_static/image5.png)

みましょうコント ローラーに、URL からいくつかのパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。 変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。 コードでは、C# で省略可能なパラメーターの機能を使用していることを示す注、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`します。 さまざまな値を試みることができます`name`と`numtimes`URL にします。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップされます。

![](adding-a-controller/_static/image6.png)

これら両方の例で、コント ローラーが実行されて、 &quot;VC&quot;の MVC 部分: ビューとコント ローラーの作業は、します。 コント ローラーは、HTML を直接返しています。 通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーは必要ありません。 代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。 そのため方法で [次へ] を見てみましょう。

> [!div class="step-by-step"]
> [前へ](intro-to-aspnet-mvc-4.md)
> [次へ](adding-a-view.md)
