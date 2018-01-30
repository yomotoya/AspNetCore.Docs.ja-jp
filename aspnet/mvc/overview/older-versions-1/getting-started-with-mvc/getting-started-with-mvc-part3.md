---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "ビューの追加 |Microsoft ドキュメント"
author: shanselman
description: "これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 8725d054861c857ceac10a42b0cc3f2afe056aea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Scott Hanselman](https://github.com/shanselman)

> これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みを単純な web アプリケーションを作成します。 参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。


このセクションで行うビュー テンプレート ファイルを使用してクライアントに HTML 応答を生成するを明確にカプセル化、HelloWorldController クラスがあっても方法を確認します。

テンプレートを使用して、ビュー、インデックスのメソッドを使用して開始しましょう。 このメソッドはインデックスであり、HelloWorldController です。 現在、Index() メソッドには、コント ローラー クラスにハードコードされているメッセージを含む文字列が返されます。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

ようになりましたインデックスの代わりに次のような方法を変更してみましょう。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

この Index() メソッドを使用できるプロジェクトにテンプレートの表示を今すぐ追加してみましょう。 これを行うには、インデックス メソッドの途中で任意の場所をマウスで右クリックし、[ビューの追加] をクリックしてください.

![イメージ](getting-started-with-mvc-part3/_static/image1.png)

このインデックス メソッドで使用できるビュー テンプレートを作成する方法の一部のオプションを提供する「ビューの追加」ダイアログが表示されます。 ここでは、何も、変更おらず、のみ、[追加] をクリックします。

[![[追加] ダイアログの表示](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

追加 をクリックした後、新しいフォルダーとファイルの新規フォルダーに表示されます、ソリューション、次に示すようです。 そのフォルダー内にビュー、および Index.aspx ファイルの下の HelloWorld フォルダーがあります。

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

新しいインデックスのファイルも既に開かれていると編集の準備ができています。 1 つ目の下にあるテキストを追加&lt;h2&gt;インデックス&lt;/h2&gt;などの"Hello World"

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

アプリケーションを実行しを参照してください[ `http://localhost:xx/HelloWorld` ](http://localhostxx)お使いのブラウザーでもう一度です。 この例では、コント ローラーにインデックス メソッドは、作業を行っていないが、"戻り View()"クライアントへの応答を表示するためにテンプレート ファイルの表示を使用する場合は、示されるを呼び出すでした。 明示的を使用するビューのテンプレート ファイルの名前を指定していない、ために、ASP.NET MVC の既定値は、ファイルを使用して、Index.aspx ビュー \Views\HelloWorld フォルダー内にします。 今すぐハード コーディングされた、ビューで、文字列を確認します。

[![Windows Internet Explorer のインデックス](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

なかなか良いを検索します。 ただし、ブラウザーのタイトルは"Index"、そのページで、大きなタイトルは「My MVC アプリケーションです」に注意してください。 これらの名前を変更してみましょう。

### <a name="changing-views-and-master-pages"></a>ビューおよびマスター ページを変更します。

最初に、「My MVC アプリケーションです」というテキストを変更してみましょう。 そのテキストを使用して、共有は、すべてのページに表示されます。 アプリ内の各ページ上にあるいても実際には、コード内の 1 か所に表示します。 ソリューション エクスプ ローラーで/Views/Shared フォルダーに移動し、Site.Master ファイルを開きます。 このファイルは、マスター ページと呼ばれ、共有「シェル」、その他のすべてのページを使用することができます。

このファイルには、ContentPlaceholder「メイン」と表示されているいくつかのテキストに注意してください。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

プレース ホルダーは、ここで作成するすべてのページ表示されます、マスター ページで「ラップされた」です。 次に、アプリを実行し、複数のページを参照してください、タイトルを変更してください。 複数のページで、1 つの変更が表示されることがわかります。

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

「My MVC ムービー アプリケーションです」の - H1 はこれで、すべてのページにプライマリ見出しになります 上部にあるすべてのページで共有される白いテキストを処理するとします。

タイトルを変更しましたで全体として、Site.Master を次に示します。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

ここで、インデックス ページのタイトルを変更してみましょう。

Open /HelloWorld/Index.aspx. 2 つの場所を変更するのには 最初に、タイトルに表示される、セカンダリ ヘッダー - H2 - も、ブラウザーのタイトル。 それらをどの少量のコードを確認できるように、若干異なる、アプリのどの部分を変更する各を作成します。

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

アプリを実行し、/Movies を参照してください。 ブラウザーのタイトル、プライマリ見出しおよびセカンダリの見出しが変更されたことに注意してください。 変更が小さいと、アプリ内のビューに大きな変更を加えるには簡単です。

[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

"Data"(この場合は、"Hello World!"を少し メッセージ) ハードコードされていた場合。 安心感の V (Views) と、C (コント ローラー) がないの M (モデル) まだきました。 方法を見てみましょう間もなくデータベースを作成し、モデル データを取得します。

## <a name="passing-a-viewmodel"></a>ViewModel を渡す

データベースに移動し、モデルについて説明する前に、しましょう。 まず"ViewModels"について これらは、クライアントへの HTML 応答を表示するために必要とビューのテンプレートを表すオブジェクトです。 コント ローラー クラスによって、ビュー テンプレートに渡され、ビュー テンプレートが必要です - データおよび以上のみを含める必要があります、通常は、作成します。

以前、HelloWorld サンプルを使用して、Welcome() アクション メソッド名前および numTimes パラメーターと、ブラウザーに出力します。 この応答を直接表示するために続行コント ローラーがあるのではなく、小さなクラスにそのデータを保持し、バックアップを使用して、HTML 応答をレンダリングするビュー テンプレートを経由で渡すことを加えてみましょう代わりに。 こうすると、コント ローラーは、1 つの点と、ビュー テンプレート別 – クリーン"関心の分離"、アプリケーション内での管理を有効にするとします。

HelloWorldController.cs ファイルに戻り、新しい"WelcomeViewModel"クラスを追加、およびコント ローラー内の開始方法を変更します。 同じファイルに新しいクラスを使用した完全な HelloWorldController.cs を次に示します。

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

複数行にある場合でもへようこそ メソッドは本当に 2 つのコード ステートメントにします。 最初のステートメントでは、ビューには、結果のオブジェクトが ViewModel オブジェクト、および 2 番目のパスに、2 つのパラメーターをパッケージ化します。

ようこそビュー テンプレートいく必要があります。 ようこそメソッドを右クリックし、追加のビューを選択します。 このとき、おします「厳密に型指定されたビューを作成する」を確認し、ドロップ ダウン リストから、WelcomeViewModel クラスを選択します。 この新しいビューは、WelcomeViewModels しない他の種類のオブジェクトは次のトピックではのみです。

> *注: ドロップダウン リストに表示、WelcomeViewModel 用に追加した後に 1 回コンパイルする必要があります。*


ここでは新機能、ビューの追加 ダイアログのようになります。 [追加] ボタンをクリックします。 ![ビューの丸を追加します。](getting-started-with-mvc-part3/_static/image10.png)

このコードを追加、 &lt;h2&gt;新しい Welcome.aspx でします。 うまくループを作成し、ユーザーの質問がお回数だけ挨拶!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

またにため言いましたこの、WelcomeViewModel に関するビューにを入力しているときに注意してください (既婚、保存しますか?) の下のスクリーン ショットに見られるよう、モデル オブジェクトを参照するたびに役立つ Intellisense を取得します。

[![NumTime ソース コード](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`もう一度です。 URL からデータを移動しています、これで、コント ローラーに自動的に渡されるという、当社のコント ローラーが、ViewModel にデータをパッケージ化し、ビューには、そのオブジェクトを渡します。 ビューよりも、ユーザーに html 形式でデータが表示されます。

[![Windows Internet Explorer の開始](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

モデル、"M"の種類が、データベースの種類ではありませんでした。 学んだこと確認し、ムービーのデータベースを作成してみましょう。

>[!div class="step-by-step"]
[前へ](getting-started-with-mvc-part2.md)
[次へ](getting-started-with-mvc-part4.md)
