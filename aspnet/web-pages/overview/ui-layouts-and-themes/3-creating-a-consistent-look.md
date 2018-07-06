---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: ページ (Razor) サイトを ASP.NET Web で一貫性のあるレイアウトを作成する |Microsoft Docs
author: tfitzmac
description: サイトの web ページを作成する方が効率的に web サイト、および c を (ヘッダーとフッター) などのコンテンツの再利用可能なブロックを作成しています.
ms.author: aspnetcontent
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: d27cdc70417f380d596f4d07384a615586427643
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821215"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトで一貫したレイアウトを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事で使用方法を説明するレイアウト ページ、ASP.NET Web Pages (Razor) の web サイトで (ヘッダーとフッター) などのコンテンツの再利用可能なブロックを作成して、サイト内のすべてのページの一貫性のある外観を作成します。
> 
> **学習内容。** 
> 
> - ヘッダーとフッターなどのコンテンツの再利用可能なブロックを作成する方法。
> - レイアウトを使用して、サイト内のすべてのページの一貫性のある外観を作成する方法。
> - レイアウト ページに、実行時にデータを渡す方法。
> 
> この記事で導入された ASP.NET 機能を次に示します。
> 
> - コンテンツ ブロック、複数のページに挿入される HTML 形式のコンテンツを含むファイルです。
> - レイアウト ページ、ページは、web サイトのページで共有できる HTML 形式のコンテンツが含まれています。
> - `RenderPage`、 `RenderBody`、および`RenderSection`メソッドで、ページ要素を挿入する場所を ASP.NET に指示します。
> - `PageData`ディクショナリのコンテンツのブロックとレイアウト ページの間でデータを共有することができます。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


## <a name="about-layout-pages"></a>レイアウト ページの概要

多くの web サイトでは、ヘッダーとフッター、または、ボックス ログインしていることをユーザーに通知するように、すべてのページに表示されるコンテンツがあります。 ASP.NET では、テキスト、マークアップ、および標準の web ページと同様のコードを格納できるコンテンツのブロックを別のファイルを作成できます。 他のページに情報を表示するサイト コンテンツのブロックを挿入できます。 その方法をコピーして、すべてのページに同じコンテンツを貼り付ける必要はありません。 このような一般的なコンテンツの作成も簡単にサイトを更新します。 コンテンツを変更する必要がある、1 つのファイルだけを更新したことができます、および変更がすべての場所で反映される場合、コンテンツが挿入されました。

次の図は、コンテンツが作業をブロックする方法を示します。 ASP.NET がポイントにコンテンツのブロックを挿入する、ブラウザーでは、web サーバーからページを要求するときに場所、`RenderPage`メイン ページでメソッドが呼び出されます。 終了 (結合) ページがブラウザーに送信されます。

![現在のページに RenderPage メソッドが参照先のページを挿入する方法を示す概念図。](3-creating-a-consistent-look/_static/image1.jpg)

この手順では、個別のファイル内にある 2 つのコンテンツ ブロック (ヘッダーおよびフッター) を参照するページを作成します。 サイト内の任意のページで、これらの同じコンテンツ ブロックを使用できます。 完了したら、このようなページが表示されます。

![RenderPage メソッドの呼び出しが含まれているページの実行の結果をブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image2.jpg)

1. という名前のファイルを作成、web サイトのルート フォルダーに*Index.cshtml*します。
2. 次のように既存のマークアップに置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. という名前のフォルダーを作成、ルート フォルダーで*Shared*します。

    > [!NOTE]
    > という名前のフォルダー内の web ページ間で共有されるファイルを格納することを共通*Shared*します。
4. *Shared*フォルダー、という名前のファイルを作成する *\_Header.cshtml*します。
5. 次のように、既存のコンテンツを置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    ファイル名は *\_Header.cshtml*、アンダー スコア (\_) をプレフィックスとして。 ASP.NET は、その名がアンダー スコアで始まる場合、ブラウザーにページを送信しません。 これはユーザーが要求する (誤ってまたはそれ以外の場合) これらのページ直接することを防ぎます。 ユーザーがこれらのページを要求できる本当に必要ないため、コンテンツのブロックがある名前のページにアンダー スコアを使用することをお勧め&#8212;に他のページに挿入される厳密に存在します。
6. *Shared*フォルダー、という名前のファイルを作成する *\_Footer.cshtml*次の内容を置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. *Index.cshtml*ページで、2 つの呼び出しを追加、`RenderPage`メソッドを次に示します。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    これは、web ページにコンテンツのブロックを挿入する方法を示します。 呼び出す、`RenderPage`メソッドの内容がその時点で挿入するファイルの名前を渡すとします。 ここでは、内容を挿入している、  *\_Header.cshtml*と *\_Footer.cshtml*へのファイル、 *Index.cshtml*ファイル。
8. 実行、 *Index.cshtml*ブラウザーでページ。 (WebMatrix で、**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。
9. ブラウザーでページ ソースを表示します。 (たとえば、Internet Explorer で、ページを右クリックし、順にクリックします**ソースの表示**)。

    これにより、コンテンツ要素で、インデックス ページのマークアップを結合すると、ブラウザーに送信される web ページのマークアップを参照できます。 次の例では、レンダリングされたページのソース*Index.cshtml*します。 呼び出し`RenderPage`に挿入した*Index.cshtml*ヘッダーとフッターのファイルの実際の内容に置き換えられました。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>レイアウト ページを使用して一貫性のある外観を作成します。

これまでに複数のページに同じコンテンツを含めることを簡単に見てきました。 サイトの一貫性のある外観を作成する体系的な方法では、レイアウト ページを使用します。 レイアウト ページでは、web ページの構造を定義しますが、実際の内容が含まれていません。 レイアウト ページを作成した後、コンテンツが含まれ、レイアウト ページにリンクするにする web ページを作成できます。 これらのページが表示される場合は、レイアウト ページに従ってフォーマットされます。 (この意味で、レイアウト ページとして機能します他のページで定義されているコンテンツのテンプレートの一種。)

レイアウト ページですが、任意の HTML ページと同じようにへの呼び出しが含まれている、`RenderBody`メソッド。 位置、`RenderBody`コンテンツ ページからの情報が含まれるされる、レイアウト ページ メソッドを決定します。

次の図は、どのようにコンテンツ ページとレイアウト ページは、完成した web ページを生成するために実行時に結合されます。 ブラウザーでは、コンテンツ ページを要求します。 [コンテンツ] ページでは、ページの構造を使用するレイアウト ページを指定することでコードがあります。 ポイントでコンテンツを挿入、レイアウト ページで、`RenderBody`メソッドが呼び出されます。 呼び出すことによって、レイアウト ページに要素が挿入もできるコンテンツ、`RenderPage`メソッド、前のセクションで行った方法です。 Web ページが完了したら、ブラウザーに送信されます。

![RenderBody メソッドの呼び出しが含まれているページの実行の結果をブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image3.jpg)

次の手順では、ページとリンクのコンテンツ ページをレイアウトを作成する方法を示します。

1. *Shared*という名前のファイルを作成、web サイトのフォルダー  *\_Layout1.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    使用する、`RenderPage`コンテンツ ブロックを挿入するレイアウト ページ内のメソッド。 レイアウト ページは、1 つだけの呼び出しを含めることができます、`RenderBody`メソッド。
3. *Shared*フォルダー、という名前のファイルを作成する *\_Header2.cshtml*次の既存の内容を置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. ルート フォルダーで、新しいフォルダーを作成し、名前*スタイル*します。
5. *スタイル*フォルダー、という名前のファイルを作成*Site.css*し、次のスタイル定義を追加します。

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    これらのスタイルの定義は、ここでレイアウト ページでスタイル シートを使用する方法を表示するためだけです。 する場合は、これらの要素のスタイルを定義できます。
6. という名前のファイルを作成、ルート フォルダーで*Content1.cshtml*次の既存の内容を置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    これは、レイアウト ページを使用するページです。 ページの上部にあるコード ブロックでは、このコンテンツを書式設定に使用するレイアウト ページを示します。
7. 実行*Content1.cshtml*ブラウザーでします。 レンダリングされたページが、形式を使用しで定義されているスタイル シート *\_Layout1.cshtml*とテキスト (コンテンツ) で定義されている*Content1.cshtml*します。

    ![[イメージ]](3-creating-a-consistent-look/_static/image4.jpg)

    手順 6 には、同じレイアウト ページを共有できますし、追加のコンテンツ ページの作成を繰り返します。

    > [!NOTE]
    > フォルダー内のすべてのコンテンツ ページの同じレイアウト ページを自動的に使用できるように、サイトを設定することができます。 詳細については、次を参照してください。[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906)します。

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>複数のコンテンツ セクションのレイアウト ページのデザイン

コンテンツ ページは置き換え可能なコンテンツを含む複数の領域のレイアウトを使用する場合に便利です。 複数のセクションでは、ことができます。 コンテンツ ページで、各セクションに一意の名前を付けます。 (既定のセクションの左名前のない)。追加する、レイアウト ページ、`RenderBody`名前のない (既定) のセクションが表示される場所を指定します。 個別追加して`RenderSection`メソッドの名前のセクションを個別に表示するためにします。

次の図は、ASP.NET での複数のセクションに分割された内容の処理方法を示します。 各名前付きセクションは、コンテンツ ページのセクションのブロックに含まれます。 (名前と`Header`と`List`の例です)。フレームワークが挿入ポイントで、レイアウト ページのコンテンツ セクションで、`RenderSection`メソッドが呼び出されます。 名前のない (既定) のセクションは、ポイントに挿入場所、`RenderBody`メソッドが呼び出される前に説明したようにします。

![RenderSection メソッドが、現在のページに参照セクションを挿入する方法を示す概念図。](3-creating-a-consistent-look/_static/image5.jpg)

この手順では、複数のコンテンツ セクションのあるコンテンツ ページを作成する方法と複数のコンテンツ セクションをサポートするレイアウト ページを使用してレンダリングする方法を示します。

1. *Shared*フォルダー、という名前のファイルを作成する *\_Layout2.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    使用する、`RenderSection`メソッド ヘッダーとリストの両方のセクションを表示するためにします。
3. という名前のファイルを作成、ルート フォルダーで*Content2.cshtml*次の既存の内容を置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    このコンテンツ ページには、ページの上部にあるコード ブロックが含まれています。 各名前付きセクションは、セクション ブロックに含まれます。 ページの残りの部分には、既定の (無名) のコンテンツ セクションが含まれています。
4. 実行*Content2.cshtml*ブラウザーでします。

    ![RenderSection メソッドの呼び出しが含まれているページの実行の結果をブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>省略可能なコンテンツ セクションを行う

通常、コンテンツ ページで作成するためのセクションでは、セクションでは、レイアウト ページで定義されていると一致する必要です。 次のいずれかが発生した場合は、エラーを取得できます。

- [コンテンツ] ページには、レイアウト ページに対応するセクションがセクションが含まれていません。
- レイアウト ページには、対象のコンテンツがセクションが含まれていません。
- レイアウト ページには、同じセクションを 2 回以上表示しようとするメソッドの呼び出しが含まれています。

ただし、レイアウト ページで省略可能セクションを宣言することで、名前付きセクションのこの動作をオーバーライドできます。 これにより、複数のコンテンツ ページ、レイアウト ページを共有することができますが、または特定のセクションのコンテンツがない可能性がありますを定義できます。

1. 開いている*Content2.cshtml*し、次のセクションを削除します。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. ページを保存し、ブラウザーで実行することです。 [コンテンツ] ページは、レイアウト ページ、つまりヘッダー セクションで定義されているセクションのコンテンツを提供しないため、エラー メッセージが表示されます。

    ![ページを実行する場合に発生するエラーを示すスクリーン ショットが RenderSection メソッドを呼び出しますが、対応するセクションが指定されていません。](3-creating-a-consistent-look/_static/image7.jpg)
3. *Shared*フォルダーを開き、  *\_Layout2.cshtml*ページし、この行に置き換えます。

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    次のコードでは。

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    代わりに、次のコードのブロックは同じ結果を前のコード行を置き換えることができます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. 実行、 *Content2.cshtml*もう一度、ブラウザーでページ。 (まだこのページをブラウザーで開きますが場合、だけ更新できますが。)この時間、ページが表示されますエラーは、ヘッダーがありません。

## <a name="passing-data-to-layout-pages"></a>レイアウト ページにデータを渡す

レイアウト ページを参照する必要のあるコンテンツ ページで定義されているデータを使用するがあります。 そうである場合は、レイアウト ページにコンテンツ ページからデータを渡す必要があります。 など、ユーザーのログイン状態を表示するか、またはユーザー入力に基づいてコンテンツ領域を非表示にすることがあります。

値を配置するレイアウト ページにコンテンツ ページからデータを渡す、`PageData`コンテンツ ページのプロパティ。 `PageData`プロパティがページ間で渡すデータを保持する名前/値ペアのコレクション。 値を確認できる、レイアウト ページ、`PageData`プロパティ。

別の図を次に示します。 ASP.NET の使用方法を示しますこの 1 つ、`PageData`プロパティ、レイアウト ページのコンテンツ ページから値を渡します。 ASP.NET web ページの作成の開始時に作成、`PageData`コレクション。 データを格納するコードを記述するコンテンツのページで、`PageData`コレクション。 値、`PageData`コレクションは、他のセクションでは、[コンテンツ] ページで、追加のコンテンツのブロックにもアクセスできます。

![コンテンツ ページが PageData ディクショナリを設定し、レイアウト ページにその情報を渡す方法を示す概念図。](3-creating-a-consistent-look/_static/image8.jpg)

次の手順では、レイアウト ページにコンテンツ ページからデータを渡す方法を示します。 ページの実行時に、ユーザー、レイアウト ページで定義されている一覧を表示または非表示を切り替えるボタンが表示されます。 True または false (ブール値) の値を設定、ユーザーがボタンをクリック、`PageData`プロパティ。 レイアウト ページでは、その値を読み取って、false の場合は、リストを非表示にします。 表示するかどうかを判断する、[コンテンツ] ページで、値が使用も、**リストの非表示に**ボタンまたは**リストの表示**ボタンをクリックします。

![[イメージ]](3-creating-a-consistent-look/_static/image9.jpg)

1. という名前のファイルを作成、ルート フォルダーで*Content3.cshtml*次の既存の内容を置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    コード内のデータの 2 つの情報を格納する、`PageData`プロパティ&#8212;タイトルの web ページと true または false に設定すると、一覧を表示するかどうかを指定します。

    ASP.NET を使うと条件付きでコード ブロックを使用して、ページに HTML マークアップを格納できることに注意してください。 たとえば、`if/else`ブロック、ページの本文でかどうかに応じて表示する形式を決定します`PageData["ShowList"]`が設定を true にします。
2. *Shared*フォルダー、という名前のファイルを作成する *\_Layout3.cshtml*次の既存の内容を置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    レイアウト ページには内の式が含まれています、`<title>`からタイトルの値を取得する要素、`PageData`プロパティ。 また、使用、`ShowList`の値、`PageData`リスト コンテンツ ブロックを表示するかどうかを決定するプロパティ。
3. *Shared*フォルダー、という名前のファイルを作成する *\_List.cshtml*次の既存の内容を置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. 実行、 *Content3.cshtml*ブラウザーでページ。 ページの左側に表示される一覧で、ページが表示されます、**リストの非表示に**下部にあるボタンをクリックします。

    ![リストと「一覧の非表示にする」ことを示すボタンを含むページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image10.jpg)
5. クリックして**一覧を表示しない**します。 一覧が表示されなくなり、ボタンに変わります**リストの表示**します。

    ![リストとの一覧を表示する というボタンが含まれていないページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image11.jpg)
6. をクリックして、**リストの表示**ボタン、および一覧が再び表示されます。

## <a name="additional-resources"></a>その他のリソース


[ASP.NET Web ページのサイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
