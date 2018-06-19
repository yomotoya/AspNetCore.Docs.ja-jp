---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: コント ローラーの追加 |Microsoft ドキュメント
author: shanselman
description: このチュートリアルは Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョンです。 新しいチュートリアルを使用して ASP.NET MVC 5 は、t に対して多くの機能強化を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869024"
---
<a name="adding-a-controller"></a>コントローラーを追加する
====================
によって[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。 新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。
> 
> 
> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


MVC は、モデル、ビュー、コント ローラーを表します。 MVC は、パターンを各部分とは別に異なる責任を持つようにアプリケーションを開発するためです。

- アプリケーションのデータのモデルです。
- ビュー: テンプレート ファイル、アプリケーションが HTML 応答を動的に生成に使用します。
- コント ローラー: アプリケーションへの着信 URL 要求を処理するクラスはモデル データを取得し、クライアントへの応答をレンダリングするテンプレートの表示を指定

このチュートリアルでは、これらすべての概念をカバーして行いますを使用してアプリケーションを構築する方法を示します。

ソリューション エクスプ ローラーで controllers フォルダーを右クリックし、コント ローラーの追加を選択して、新しいコント ローラーを作成してみましょう。

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

新しいコント ローラー"HelloWorldController"の名前し、[追加] をクリックします。

[![[追加] ダイアログのコント ローラー](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

ソリューション エクスプ ローラーで、右側の HelloWorldController.cs と呼ばれる新しいファイルが作成されでそのファイルを開くようになりましたことに注意してください、 **IDE**です。

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

新しいパブリック クラス HelloWorldController の内部で次のように 2 つの新しいメソッドを作成します。 例として、コント ローラーから直接 HTML の文字列を返します。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

HelloWorldController の名前は、コント ローラーと、新しいメソッドには、インデックスは呼び出されます。 アプリケーションを実行して、もう一度同じように (再生ボタンをクリックしてまたはこれを行うには F5 キーを押します)。 お使いのブラウザーが起動した後は、アドレス バーのパスを変更`http://localhost:xx/HelloWorld`xx は任意の番号、コンピューターが選択されます。 今すぐお使いのブラウザーは、次のスクリーン ショットのようになります。 上記このメソッドでは返される「コンテンツ」と呼ばれるメソッドに渡される文字列 システムでは、いくつかの HTML だけを返し、同じと言いました。

ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれる別のアクション メソッド) を呼び出します。 ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、どのようなコードの実行を制御します。

/[Controller]/[ActionName]/[Parameters]

URL の最初の部分を実行するコント ローラー クラスを決定します。 したがって/HelloWorld は HelloWorldController クラスにマップされます。 URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。 したがって/HelloWorld/Index とで実行する HelloWorldcontroller クラスの Index() メソッドになります。 のみしなければならないこと/HelloWorld 上記とインデックスは暗黙的であったメソッドを参照してくださいに注意してください。 これは、"Index"という名前のメソッドがいずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法であるためです。

[![これは、既定のアクション](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

ここで、次を参照してください`http://localhost:xx/HelloWorld/Welcome.`これで、このようこそメソッドが実行され、HTML 文字列が返されました。

もう一度、/[コント ローラー]/[ActionName]/[パラメーター] コント ローラーが HelloWorld、[ようこそ] が、メソッドをここではします。 パラメーターはまだ完成はしていません。

[![これは、ようこそアクション メソッドです。](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

みましょうので、たとえば次のように、コント ローラーに、URL からの一部の情報を渡すお、この例を少し変更: HelloWorld/ウェルカムしますか? 名 = Scott&amp;numtimes 4 を = です。 など、2 つのパラメーター以下のような更新プログラムへようこそ、メソッドを変更します。 が渡されない場合パラメーター numTimes が 1 を既定ことを示すために、C# で省略可能なパラメーター機能を使い切ってしまったことに注意してください。

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`好きなように、名前と numtimes の値を変更します。 システムは、メソッドのパラメーターに、アドレス バーに、クエリ文字列から名前付きパラメーターを自動的にマップされます。

これら両方の例では、コント ローラーは、すべての作業を行ってされておりが直接に HTML を返すことされました。 通常、コント ローラーしたくありませんを最終的にコードに非常に手間がかかるので、HTML を直接返します。 代わりに別のビュー テンプレート ファイルを HTML 応答を生成するために通常使用おします。 結果がどのようにこれを見てみましょう。 お使いのブラウザーを閉じて、IDE に戻ります。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part1.md)
> [次へ](getting-started-with-mvc-part3.md)
