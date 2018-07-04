---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: ビューの追加 |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 942d329cf0451101a24da4c3facd38b2813653d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365673"
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。


このセクションでは、ビュー テンプレート ファイルを使用して、クライアントへの生成の HTML 応答を明確にカプセル化、HelloWorldController クラスがあっても方法を見てはいきます。

Index メソッドをテンプレートの表示を使用して開始しましょう。 メソッドには、インデックスが呼び出され、HelloWorldController になります。 現在、Index() メソッドは、コント ローラー クラス内でハードコーディングされているメッセージ文字列を返します。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

代わりに次のように Index メソッドを今すぐ変更してみましょう。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Index() 方法を使用できるプロジェクトにビュー テンプレートを今すぐ追加してみましょう。 Index メソッドの途中でどこかにマウスを右クリックし、[ビューの追加] をクリックしてください.

![イメージ](getting-started-with-mvc-part3/_static/image1.png)

私たちの Index メソッドで使用できるテンプレートの表示を作成する方法の一部のオプションを提供する「ビューの追加」ダイアログが表示されます。 ここでは、しないは、内容を変更し、[追加] ボタンをクリックします。

[![[追加] ダイアログの表示](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

[追加] をクリックすると、新しいフォルダーと新しいファイルに表示されます、ソリューション フォルダー以下のようです。 そのフォルダー内で下にあるビュー、および、Index.aspx ファイル HelloWorld フォルダーがあります。

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

新しいインデックスのファイルも既に開かれていると編集を開始できます。 最初のいくつかのテキストを追加する&lt;h2&gt;インデックス&lt;/h2&gt;などの"Hello World."

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

アプリケーションを実行しを参照してください[ `http://localhost:xx/HelloWorld` ](http://localhostxx)お使いのブラウザーでもう一度です。 この例では、コント ローラーに Index メソッドは、作業を実行していないが、"戻り View()"応答をクライアントにレンダリングするビュー テンプレート ファイルを使用したいことが示されているを呼び出すでした。 明示的に使用するビュー テンプレート ファイルの名前を指定しましたいない、ため \Views\HelloWorld フォルダー内、Index.aspx ビュー ファイルを使用する ASP.NET MVC が既定値。 これで、文字列をハードコーディングしたため、ビューに表示されています。

[![Windows Internet Explorer のインデックス](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

ように見えますね。 ただしに、ブラウザーのタイトルは"Index"、ページの大きなタイトルは"My MVC Application です"注意してください。 これらの名前を変更してみましょう。

### <a name="changing-views-and-master-pages"></a>ビューおよびマスター ページを変更します。

最初に、"My MVC Application です"テキストを変更してみましょう。 そのテキストは、共有され、すべてのページに表示されます。 アプリ内の各ページ上にある場合でも実際に、コード内の 1 か所で表示します。 ソリューション エクスプ ローラーで/Views/Shared フォルダーに移動し、Site.Master ファイルを開きます。 このファイルは、マスター ページと呼ばれ、共有"シェルが"、その他のすべてのページを使用します。

このファイルには、プレース ホルダー"MainContent"と書かれたテキストに注意してください。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

プレース ホルダーは、場所を作成するすべてのページが表示されます、マスター ページで「ラップされた」です。 アプリを実行し、複数のページを参照してください。 その後、タイトルを変更してみてください。 複数のページで、1 つの変更が表示されることがわかります。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

「My MVC ムービー アプリケーションです」- H1 はすべてのページは、プライマリ見出しの必要があります。 上部にあるすべてのページで共有されている白いテキストを処理するとします。

変更されたタイトルで全体、Site.Master を示します。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

ここで、インデックス ページのタイトルを変更してみましょう。

/HelloWorld/Index.aspx を開きます。 これは、2 つの場所を変更します。 最初に、タイトルに表示される H2 でもあるセカンダリのヘッダーに、ブラウザーのタイトル。 アプリのどの部分を変更するビットのコードを表示できるように、若干異なる各描いておきます。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

アプリを実行し、/Movies を参照してください。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されたことに注意してください。 ビューに小さな変更を使用してアプリケーションで大きな変更を加えるには簡単です。

[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

(この場合は、"Hello World!"を「データ」のごく一部 メッセージ) は、コード化されたハードでした。 V (ビュー) が発生しましたし、C (コント ローラー) がないの M (モデル) まだがあります。 方法を説明しますすぐデータベースを作成し、モデル データを取得します。

## <a name="passing-a-viewmodel"></a>ViewModel を渡す

データベースに移動してモデルについて説明する前に、まずについて説明しましょう"ViewModels" これらは、クライアントに、HTML 応答を表示するために必要とビュー テンプレートを表すオブジェクトです。 コント ローラー クラスによって、ビュー テンプレートに渡され、テンプレートの表示が必要です - データと以上のみを含める必要があります、通常は作成されます。

以前、HelloWorld サンプルを使用して、Welcome() アクション メソッドが名前と numTimes パラメーターと、ブラウザーに出力します。 この応答を直接表示するために引き続きコント ローラーがあるのではなく、小さなクラスにそのデータを保持し、バックアップを使用して、HTML 応答をレンダリングするビュー テンプレートを経由で渡すことを加えてみましょう代わりに。 こうすると、コント ローラーは 1 つと、ビュー テンプレートに関係別 – クリーン""関心の分離、アプリケーション内で維持するためにできるようになりました。

HelloWorldController.cs ファイルに戻ると新しい"WelcomeViewModel"クラスを追加し、コント ローラー内の開始方法を変更します。 同じファイル内の新しいクラスを使用して完全な HelloWorldController.cs を次に示します。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

場合でも、複数の行には、開始の方法が本当に 2 つだけのコード ステートメントにします。 最初のステートメントは、結果のオブジェクトをビューに ViewModel オブジェクト、および 2 番目のパスに 2 つのパラメーターをパッケージ化します。

[ようこそ] ビュー テンプレート必要があります。 [ようこそ] のメソッドを右クリックし、ビューの追加を選択します。 今回が「厳密に型指定されたビューの作成」のチェックされ、ドロップダウン リストから、WelcomeViewModel クラスを選択します。 この新しいビューが認識するだけ WelcomeViewModels とオブジェクトの他の種類がありません。

> *注: ドロップダウン リストに表示するため、WelcomeViewModel を追加した後に 1 回コンパイルする必要があります。*


次のように確認する必要があります、ビューの追加 ダイアログに示します。 [追加] ボタンをクリックします。 ![ビューの丸を追加します。](getting-started-with-mvc-part3/_static/image10.png)

下には、このコードを追加、 &lt;h2&gt;新しい Welcome.aspx でします。 ループの作成をユーザーの質問が何回にあいさつします!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

また、ためこの先ほど言ったとおり、WelcomeViewModel に関するビューを入力している間に注意してください (既婚に注意してください)。 以下のスクリーン ショットに見られるよう、モデル オブジェクトを参照するたびに、便利な Intellisense を取得します。

[![NumTime ソース コード](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`もう一度です。 URL からデータを移動しています、これで、コント ローラーに自動的に渡されるという、コント ローラーは ViewModel にデータをパッケージ化し、そのオブジェクトのビューに渡します。 ビューよりも、ユーザーに HTML としてデータが表示されます。

[![Windows Internet Explorer の開始](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

ある種のモデルでは、"M"がデータベースの種類ではありませんでした。 学習した確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](getting-started-with-mvc-part2.md)
> [次へ](getting-started-with-mvc-part4.md)
