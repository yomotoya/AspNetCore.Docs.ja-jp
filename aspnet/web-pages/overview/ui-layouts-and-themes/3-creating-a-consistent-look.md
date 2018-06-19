---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: ページ (Razor) サイトの ASP.NET Web で一貫したレイアウトを作成 |Microsoft ドキュメント
author: tfitzmac
description: これをサイトの web ページを作成する方が効率的にするには、するには、web サイト、および c を (ヘッダーやフッター) などのコンテンツの再利用可能なブロックを作成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530241"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) のサイトで一貫したレイアウトを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事使用方法を説明するレイアウト ページの ASP.NET Web Pages (Razor) web サイト (ヘッダーやフッター) などのコンテンツの再利用可能なブロックを作成して、サイトのすべてのページで一貫した外観を作成します。
> 
> **学習する内容。** 
> 
> - ヘッダーやフッターなどのコンテンツの再利用可能なブロックを作成する方法。
> - レイアウトを使用して、サイト内のすべてのページの一貫した外観を作成する方法。
> - レイアウト ページに、実行時にデータを渡す方法。
> 
> アーティクルで導入された ASP.NET 機能を次に示します。
> 
> - コンテンツ ブロック、複数のページに挿入される HTML 形式のコンテンツを含むファイルです。
> - レイアウト ページ、web サイト上のページで共有できる HTML 形式のコンテンツが含まれるページします。
> - `RenderPage`、 `RenderBody`、および`RenderSection`メソッドで、ページの要素を挿入する場所を ASP.NET に指示します。
> - `PageData`コンテンツ ブロック レイアウト ページの間でデータを共有することができますをディクショナリ。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


## <a name="about-layout-pages"></a>レイアウト ページの概要

多くの web サイトでは、ヘッダーとフッター、またはログオンしているユーザーに通知するボックスのように、すべてのページに表示されているコンテンツが存在します。 ASP.NET では、テキスト、マークアップ、および標準の web ページと同じように、コードを含めることができるコンテンツ ブロックを別のファイルを作成できます。 情報を表示するサイトの他のページで、コンテンツ ブロックを挿入できます。 このようにコピーして、すべてのページに同じコンテンツを貼り付ける必要はありません。 次のような一般的なコンテンツの作成も簡単、サイトを更新できます。 コンテンツを変更する必要があります、1 つのファイルだけを更新できるし、変更はすべての場所で反映し、コンテンツが挿入されました。

次の図は、コンテンツが作業をブロックする方法を示します。 ASP.NET がポイントでコンテンツのブロックを挿入するブラウザーでは、web サーバーからページを要求するときにここで、`RenderPage`メイン ページでメソッドが呼び出されます。 完成した (マージ) ページがブラウザーに送信されます。

![RenderPage メソッドが、現在のページに、参照先のページを挿入する方法を示す概念図です。](3-creating-a-consistent-look/_static/image1.jpg)

この手順では、個別のファイル内にある 2 つのコンテンツ ブロック (ヘッダーおよびフッター) を参照するページを作成します。 サイト内の任意のページで、これらの同じコンテンツ ブロックを使用できます。 完了したら、次のようにページが表示されます。

![RenderPage メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image2.jpg)

1. という名前のファイルを作成、web サイトのルート フォルダーに*Index.cshtml*です。
2. 既存のマークアップを次のように置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. ルート フォルダーにという名前のフォルダーを作成する*Shared*です。

    > [!NOTE]
    > という名前のフォルダー内の web ページ間で共有されるファイルを保存することを共通*Shared*です。
4. *Shared*フォルダー、という名前のファイルを作成する *\_Header.cshtml*です。
5. 既存の内容を次のように置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    ファイル名は *\_Header.cshtml*、アンダー スコア (\_) をプレフィックスとして。 ASP.NET は、その名前がアンダー スコアで始まる場合、ブラウザーにページを送信しません。 これはユーザーが要求する (誤ってまたはそれ以外の場合) これらのページ直接することを防ぎます。 ユーザーがこれらのページ &#8212; を要求できるかないためには、コンテンツ ブロックがある名前のページにアンダー スコアを使用することをお勧めそれらは、他のページに挿入する厳密に存在します。
6. *共有*フォルダー、という名前のファイルを作成する *\_Footer.cshtml*コンテンツを次に置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. *Index.cshtml*  ページで、次の 2 つの呼び出しを追加、`RenderPage`メソッドを次に示すようにします。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    これは、web ページに、コンテンツ ブロックを挿入する方法を示しています。 呼び出す、`RenderPage`メソッドをその時点で挿入する内容を持つファイルの名前を渡します。 ここでは、内容を挿入している、  *\_Header.cshtml*と *\_Footer.cshtml*へのファイル、 *Index.cshtml*ファイル。
8. 実行、 *Index.cshtml*ブラウザーのページです。 (WebMatrix での**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。
9. ブラウザーでページ ソースを表示します。 (たとえば、Internet Explorer で、ページを右クリックし、をクリックして**ソースの表示**)。

    これにより、ブラウザーでは、インデックス ページのマークアップを組み合わせて、コンテンツ ブロックに送信される web ページのマークアップを表示できます。 次の例では、について表示されるページのソース*Index.cshtml*です。 呼び出し`RenderPage`に挿入した*Index.cshtml*ヘッダーとページ フッターのファイルの実際の内容に置き換えられています。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>レイアウト ページを使用して一貫した外観を作成します。

これまでは、簡単に複数のページに同じコンテンツを含めることを見てきました。 サイトの一貫した外観を作成するための構造化されたアプローチでは、レイアウト ページを使用します。 レイアウト ページでは、web ページの構造を定義していますが、実際の内容が含まれていません。 レイアウト ページを作成した後、コンテンツが含まれ、それらをレイアウト ページにリンクする web ページを作成することができます。 これらのページが表示される場合は、レイアウト ページに従ってフォーマットされます。 (この意味で、レイアウト ページとして動作は他のページで定義されているコンテンツのテンプレートの種類です。)

呼び出しが含まれていますが、任意の HTML ページと同じようには、レイアウト ページ、`RenderBody`メソッドです。 位置、`RenderBody`レイアウト ページのメソッドは、コンテンツ ページの情報が含まれるを決定します。

次の図は、コンテンツ ページが表示され、レイアウト ページは、完成した web ページを生成するために実行時に結合されます。 ブラウザーでは、コンテンツ ページを要求します。 コンテンツ ページでは、ページの構造を使用するレイアウト ページを指定することでコードを持っています。 ポイントでコンテンツを挿入レイアウト ページで、場所、`RenderBody`メソッドが呼び出されます。 コンテンツ ブロックも挿入可能、レイアウト ページを呼び出して、`RenderPage`メソッド、前のセクションで行った方法です。 Web ページが完了したら、ブラウザーに送信されます。

![RenderBody メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image3.jpg)

次の手順では、ページとリンクのコンテンツ ページをレイアウトを作成する方法を示します。

1. *Shared*という名前のファイルの作成、web サイトのフォルダー  *\_Layout1.cshtml*です。
2. 既存の内容を次のように置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    使用する、`RenderPage`コンテンツ ブロックを挿入するレイアウト ページ内のメソッドです。 レイアウト ページは、1 つだけの呼び出しを含めることができます、`RenderBody`メソッドです。
3. *Shared*フォルダー、という名前のファイルを作成する *\_Header2.cshtml*既存のコンテンツを次に置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. ルート フォルダーに新しいフォルダーを作成し、名前*スタイル*です。
5. *スタイル*フォルダー、という名前のファイルを作成*Site.css*し、次のスタイル定義を追加します。

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    これらのスタイルの定義は、レイアウト ページでスタイル シートを使用する方法を表示するためだけここでは。 する場合は、これらの要素に独自のスタイルを定義できます。
6. ルート フォルダーにという名前のファイルを作成する*Content1.cshtml*既存のコンテンツを次に置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    これは、レイアウト ページを使用するページです。 ページの上部にあるコード ブロックでは、このコンテンツを書式設定に使用するレイアウト ページを示します。
7. 実行*Content1.cshtml*ブラウザーでします。 表示されたページは、形式を使用し、スタイル シートがで定義されている *\_Layout1.cshtml*およびで定義されているテキスト (コンテンツ) *Content1.cshtml*です。

    ![[イメージ]](3-creating-a-consistent-look/_static/image4.jpg)

    同じレイアウト ページを共有することができますし、追加のコンテンツ ページを作成する 6 の手順を繰り返します。

    > [!NOTE]
    > フォルダー内のすべてのコンテンツ ページの同じページにレイアウトを自動的に使用できるように、サイトを設定することができます。 詳細については、「[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906)します。

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>複数のコンテンツ セクション レイアウト ページのデザイン

コンテンツ ページは、置き換え可能なコンテンツを持つ複数の領域のレイアウトを使用する場合に便利ですは複数のセクションを設定できます。 コンテンツ ページで、各セクションに一意の名前を付けます。 (既定のセクションの左無名です)。レイアウト ページでは追加、`RenderBody`名前のない (既定) のセクションが表示される場所を指定します。 追加する別`RenderSection`名前付きセクションを個別に表示するためにメソッドです。

次の図は、ASP.NET が複数のセクションに分割されたコンテンツを処理する方法を示します。 各名前付きセクションでは、コンテンツ ページのセクションでブロックに含まれます。 (名前が付けられますが`Header`と`List`の例です)。フレームワークが挿入ポイントで、レイアウト ページのコンテンツ セクションで、`RenderSection`メソッドが呼び出されます。 名前のない (既定) のセクションは、ポイントに挿入場所、`RenderBody`前述のメソッドが呼び出されます。

![RenderSection メソッドが、現在のページに参照セクションを挿入する方法を示す概念図です。](3-creating-a-consistent-look/_static/image5.jpg)

この手順では、複数のコンテンツ セクションのあるコンテンツ ページを作成する方法と複数のコンテンツ セクションをサポートするレイアウト ページを使用して表示する方法を示します。

1. *Shared*フォルダー、という名前のファイルを作成する *\_Layout2.cshtml*です。
2. 既存の内容を次のように置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    使用する、`RenderSection`ヘッダーとリストの両方のセクションを表示するメソッド。
3. ルート フォルダーにという名前のファイルを作成する*Content2.cshtml*既存のコンテンツを次に置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    このコンテンツ ページには、ページの上部にあるコード ブロックが含まれています。 各名前付きセクションでは、セクション ブロックに含まれます。 ページの残りの部分には、既定の (名前のない) のコンテンツ セクションが含まれています。
4. 実行*Content2.cshtml*ブラウザーでします。

    ![RenderSection メソッドへの呼び出しを含むページを実行して得た結果ブラウザーでページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>省略可能なコンテンツ セクションを行う

通常、コンテンツ ページで作成するためのセクションでは、セクションでは、レイアウト ページで定義されていると一致する必要です。 次のいずれかが発生した場合、エラーが発生することができます。

- コンテンツ ページには、レイアウト ページの該当するセクションを持たないセクションが含まれています。
- [レイアウト] ページには、対象のコンテンツがセクションが含まれていません。
- [レイアウト] ページには、同じセクションを 2 回以上表示しようとするメソッドの呼び出しが含まれています。

ただし、レイアウト ページで省略可能にするのには、セクションを宣言することで、名前付きセクションのこの動作をオーバーライドできます。 これは、操作により、複数のコンテンツ ページ、レイアウト ページを共有することができますが、特定のセクションのコンテンツがないこともありますを定義できます。

1. 開いている*Content2.cshtml*し、次のセクションを削除します。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. ページを保存し、ブラウザーで実行します。 コンテンツ ページ、レイアウト ページ、つまりヘッダー セクションで定義されているセクションのコンテンツは提供されないため、エラー メッセージが表示されます。

    ![ページを実行する場合に発生するエラーを示すスクリーン ショットが RenderSection メソッドを呼び出しますが、対応するセクションが指定されていません。](3-creating-a-consistent-look/_static/image7.jpg)
3. *Shared*フォルダーを開き、  *\_Layout2.cshtml*ページし、この行に置き換えます。

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    次のコードでは。

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    代わりに、次のコードのブロックは、同じ結果を生成前のコード行を置き換える可能性があります。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. 実行、 *Content2.cshtml*ブラウザーをもう一度のページです。 (このページをブラウザーで開くもあれば、するのみ更新できます。)このページが表示エラーは、ヘッダーを持たない場合でも。

## <a name="passing-data-to-layout-pages"></a>レイアウト ページにデータを渡す

レイアウト ページを参照する必要のあるコンテンツ ページで定義されているデータがあります。 場合は、レイアウト ページに、コンテンツ ページからデータを渡す必要があります。 ユーザーのログイン状態を表示するなど、ユーザー入力に基づいてコンテンツ領域を非表示にする可能性があります。

レイアウト ページには、コンテンツ ページからデータを渡すするに値を配置することができます、`PageData`コンテンツ ページのプロパティです。 `PageData`プロパティがページ間で通過するデータを保持する名前/値ペアのコレクション。 レイアウト ページは、値のうち、読み取ることができます、`PageData`プロパティです。

別のダイアグラムを次に示します。 ASP.NET の使用方法を示しますこのいずれか、`PageData`プロパティに値を渡すコンテンツ ページからページをレイアウトします。 ASP.NET web ページの作成の開始時に作成、`PageData`コレクション。 データを配置するコードを記述するコンテンツ ページで、`PageData`コレクション。 値が、`PageData`コレクションは、コンテンツ ページの他のセクションで、またはコンテンツ ブロックを追加してもアクセスできます。

![コンテンツ ページが PageData ディクショナリを設定およびレイアウト ページにその情報を渡す方法を示す概念図です。](3-creating-a-consistent-look/_static/image8.jpg)

次の手順では、レイアウト ページに、コンテンツ ページからデータを渡す方法を示します。 ページの実行時に、ユーザー、レイアウト ページで定義されている一覧を表示または非表示を切り替えるボタンが表示されます。 ユーザーは、ボタンをクリックして、内 (ブール値) を true または false 値のセット、`PageData`プロパティです。 レイアウト ページは、その値を読み込んで、これが false の場合は、一覧を非表示にします。 値はでも使用コンテンツ ページを表示するかどうかを決定する、**リストの非表示に**ボタンまたは**リストの表示**ボタンをクリックします。

![[イメージ]](3-creating-a-consistent-look/_static/image9.jpg)

1. ルート フォルダーにという名前のファイルを作成する*Content3.cshtml*既存のコンテンツを次に置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    コード内のデータの 2 つの部分を格納する、`PageData`プロパティ &#8212; タイトル web ページと true または false に設定すると、一覧を表示するかどうかを指定します。

    ASP.NET を使うと条件付きでコード ブロックを使用して、ページに HTML マークアップを格納できることに注意してください。 たとえば、`if/else`ページの本文内のブロックを決定かどうかに応じてを表示するには、どのフォーム`PageData["ShowList"]`が設定を true にします。
2. *Shared*フォルダー、という名前のファイルを作成する *\_Layout3.cshtml*既存のコンテンツを次に置き換えます。

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    レイアウト ページには内の式が含まれています、`<title>`からタイトルの値を取得する要素、`PageData`プロパティです。 また、使用、`ShowList`の値、`PageData`リスト コンテンツ ブロックを表示するかどうかを決定するプロパティです。
3. *Shared*フォルダー、という名前のファイルを作成する *\_List.cshtml*既存のコンテンツを次に置き換えます。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. 実行、 *Content3.cshtml*ブラウザーのページです。 ページの左側に表示される一覧で、ページが表示されます、**リストの非表示に**下部にあるボタンをクリックします。

    ![ページ スクリーン ショットを含むリストとボタンの一覧を非表示にする と表示されています。](3-creating-a-consistent-look/_static/image10.jpg)
5. をクリックして **ボックスの一覧を非表示に**です。 リストが消え、ボタンに変わります**リストの表示**です。

    ![リストとボタンの一覧を表示する と表示されているが含まれていません ページを示すスクリーン ショット。](3-creating-a-consistent-look/_static/image11.jpg)
6. クリックして、**リストの表示** ボタン、および一覧が再び表示されます。

## <a name="additional-resources"></a>その他のリソース


[ASP.NET Web ページのサイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
