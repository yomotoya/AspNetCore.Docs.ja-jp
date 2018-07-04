---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: コント ローラーの追加 |Microsoft Docs
author: shanselman
description: このチュートリアルでは、Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョン。 新しいチュートリアルでは、t に多くの機能強化を提供する ASP.NET MVC 5 を使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: f20889cf1c1cd9a2a69f0008d879b518c38334d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395670"
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)します。 新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。
> 
> 
> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


MVC は、モデル、ビュー、コント ローラーの略です。 MVC は、各パーツがある他とは異なる責任になるようにアプリケーションを開発するためのパターンです。

- アプリケーションのデータ モデル:
- ビュー: 動的 HTML 応答を生成する、アプリケーションが使用されるテンプレート ファイル。
- コント ローラー: アプリケーションへの受信 URL 要求を処理するクラスはモデル データを取得し、クライアントに応答をレンダリングするビュー テンプレートを指定

このチュートリアルでは、これらすべての概念をカバーするがされ、アプリケーションの構築に使用する方法を示します。

ソリューション エクスプ ローラーで controllers フォルダーを右クリックし、コント ローラーの追加を選択して、新しいコント ローラーを作成しましょう。

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

新しいコント ローラー"HelloWorldController"の名前し、[追加] をクリックします。

[![[追加] ダイアログのコント ローラー](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

HelloWorldController.cs と呼ばれるの新しいファイルが作成されでそのファイルが開いている右側のソリューション エクスプ ローラーで、 **IDE**します。

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

新しいパブリック クラス HelloWorldController 内で次のように 2 つの新しいメソッドを作成します。 例として、コント ローラーから直接 HTML の文字列を返しますします。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

HelloWorldController の名前は、コント ローラーと、新しいメソッドには、インデックスは呼び出されます。 アプリケーションを再実行以前と同様 (再生ボタンをクリックしてまたはこれを行うには F5 キーを押します)。 お使いのブラウザーが開始されると、アドレス バーのパスを変更します。 `http://localhost:xx/HelloWorld` xx は任意の数、コンピューターが選択されます。 今すぐお使いのブラウザーは、次のスクリーン ショットのようになります。 上記の方法で返された「コンテンツ」と呼ばれるメソッドに渡される文字列 先ほど言ったとおり、システムは、いくつかの HTML を返すだけおり、そうしました!

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれる別のアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、このような形式を使用して、どのようなコードの実行を制御します。

/[Controller]/[ActionName]/[パラメーター]

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって、HelloWorldController クラス/HelloWorld にマップします。 URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。 /HelloWorld/Index を実行する HelloWorldcontroller クラスの Index() メソッド。 上記/HelloWorld とインデックスが暗黙的に指定されたメソッドを参照してくださいにのみ必要があることを確認します。 これは、"Index"という名前のメソッドが明示的に指定されていない場合、コント ローラーで呼び出される既定の方法であるためです。

[![これは、既定のアクション](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

ここで、次を参照してください`http://localhost:xx/HelloWorld/Welcome.`へようこそ の方法が実行され、HTML 文字列を返すようになりました。

ここでも、/[Controller]/[ActionName]/[パラメーター] コント ローラーが HelloWorld とウェルカム メソッドをここでは、します。 パラメーターをまだ完了していないことはできます。

[![これは、[ようこそ] のアクション メソッドです。](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

みましょう URL からの一部の情報を渡すなどのように、コント ローラーをように、サンプルを少し変更:/helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes 4 を = です。 2 つのパラメーターと以下のような更新プログラムを含めるへようこそ 方法を変更します。 が渡されない場合 1 をパラメーター numTimes が既定を示す c# のオプション パラメーター機能を使用したことに注意してください。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`好きなように、名前と numtimes の値を変更します。 システムは、メソッドのパラメーターに、アドレス バーで、クエリ文字列から名前付きパラメーターを自動的にマップされます。

これら両方の例では、コント ローラーは、すべての作業を行ってされておりがされた HTML を直接返します。 通常、コント ローラーしたくないコードに非常に手間がかかるで終わるために、直接 HTML を返します。 代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。 そのため方法を見てみましょう。 お使いのブラウザーを閉じて、IDE に戻ります。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part1.md)
> [次へ](getting-started-with-mvc-part3.md)
