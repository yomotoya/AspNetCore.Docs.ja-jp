---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Visual Studio 2013 で ASP.NET Web フォーム コードの編集 |Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 322a9aa6fb366eb9d5be33838fd6ac7ea51047f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371795"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013 でのコード編集の ASP.NET Web フォームします。
====================
によって[Erik Reitan](https://github.com/Erikre)

多くの ASP.NET Web フォーム ページでは、Visual Basic、c#、または別の言語でコードを記述します。 Visual Studio でコード エディターでは、エラーを回避することができ、コードをすばやく記述できます。 さらに、エディターには、行う必要がある作業量を減らすために再利用可能なコードを作成するための方法が用意されています。

このチュートリアルでは、Visual Studio コード エディターのさまざまな機能について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。

- インライン コード エラーを修正します。
- リファクタリングし、コードの名前を変更します。
- 変数およびオブジェクトの名前を変更します。
- コード スニペットを挿入します。

## <a name="prerequisites"></a>必須コンポーネント


このチュートリアルを完了するための要件は次のとおりです。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。 .NET Framework は、自動的にインストールされます。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。  
    >   
    > Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。

  Visual Studio および ASP.NET の概要については、次を参照してください。 [Visual Studio 2013 で基本的な ASP.NET 4.5 Web フォーム ページを作成する](creating-a-basic-web-forms-page.md)します。   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Web アプリケーション プロジェクトと、ページの作成

<a id="sectionToggle0"></a>

チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。

### <a name="to-create-a-web-application-project"></a>Web アプリケーション プロジェクトを作成するには

1. Microsoft Visual Studio を開きます。
2. **[ファイル]** メニューの **[新しいプロジェクト]** を選択します。  
    ![[ファイル] メニュー](code-editing-in-web-forms-pages/_static/image1.png)

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレート グループ。
4. 選択、 **ASP.NET Web アプリケーション**中央の列のテンプレート。
5. プロジェクトに名前を***BasicWebApp***  をクリックし、 **OK**ボタン。   
![新しいプロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image2.png)
6. 次に、選択、 **Web フォーム**テンプレートをクリックして、 **OK**プロジェクトを作成するボタンをクリックします。  
![新しい ASP.NET プロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio では、Web フォーム テンプレートに基づく事前構築済みの機能を含む新しいプロジェクトを作成します。


## <a name="creating-a-new-aspnet-web-forms-page"></a>新しい ASP.NET Web フォーム ページの作成


新しい Web フォーム アプリケーションを作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio の ASP.NET ページ (Web フォーム ページ) という名前を追加します*Default.aspx*他のいくつかのファイルだけでなく、フォルダーを選択します。 使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。 ただし、このチュートリアルでは作成し、新しいページを使用します。

### <a name="to-add-a-page-to-the-web-application"></a>Web アプリケーションにページを追加するには


1. **ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックし、**追加** - &gt;**新しい項目の**します。   
**[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。 次に、選択**Web フォーム**中央から一覧表示し、名前を*名前*します。   
    ![新しい項目 ダイアログ ボックスを追加します。](code-editing-in-web-forms-pages/_static/image4.png)
3. クリックして**追加**をプロジェクトに Web フォーム ページを追加します。  
 Visual Studio では、新しいページを作成し、それを開きます。
4. 次に、この新しいページを既定のスタートアップ ページとして設定します。 **ソリューション エクスプ ローラー**、という名前の新しいページを右クリックして*名前*選択**スタート ページとして設定**します。 次に、進行状況をテストするには、このアプリケーションを実行したとき、ブラウザーでこの新しいページに自動的に表示されます。


## <a name="correcting-inline-coding-errors"></a>インライン コード エラーを修正します。


Visual Studio でコード エディターを使用して、コードを記述すると、コード エディターに、エラーを修正するのに役立ちますエラーを加えた場合、エラーを回避するのに役立ちます。 このチュートリアルでは、コード エディターでのエラー修正機能を示す行を記述します。

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Visual Studio での単純なコーディング エラーを修正するには


1. **デザイン**ビューで、空白のページのハンドラーを作成するをダブルクリック、**ロード**ページのイベント。   
   コードを記述する場所としてのみ、イベント ハンドラーを使用しています。
2. ハンドラーの内部は、エラー メッセージとキーを押して、次の行を入力**ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   キーを押すと **」と入力**、コード エディターが配置される緑と赤の下線 (呼び出す機会が多い&quot;波線&quot;行) の問題があるコードの領域。 緑色の下線では、警告を示します。 赤い下線では、エラーを修正する必要がありますを示します。 

    マウス ポインターを置く`myStr`に関する警告を示すツールヒントを確認します。 また、エラー メッセージを表示する赤い下線の上にマウス ポインターを保持します。

    次の図は、下線を使用してコードを示します。

    ![デザイン ビューでの開始テキスト](code-editing-in-web-forms-pages/_static/image5.png "デザイン ビューでの開始テキスト")  
   セミコロンを追加することで、エラーを修正する必要があります`;`行の末尾にします。 警告通知するだけでを使用していない、`myStr`まだ変数。  

    > [!NOTE] 
    > 
    > Visual Studio での設定を選択して書式設定、現在のコードを表示する**ツール** - &gt; **オプション** - &gt; **フォントと色**します。


## <a name="refactoring-and-renaming"></a>リファクタリングと名前を変更します。

リファクタリングとは、ソフトウェアの方法を理解し、その機能を維持しながら、維持するために容易にできるように、コードを再構築を含むです。 簡単な例は、データベースからデータを取得するためのイベント ハンドラーにコードを記述する可能性があります。 ページを開発するとき、さまざまなハンドラーからデータにアクセスする必要があるがわかります。 そのため、ページのコードをリファクタリングするには、ページでデータ アクセス メソッドを作成し、ハンドラーのメソッドの呼び出しを挿入します。

コード エディターには、リファクタリングのさまざまなタスクを実行するためにツールが含まれています。 このチュートリアルでは、2 つのリファクタリング手法に精通する機能: 変数の名前を変更して、メソッドを抽出します。 その他のリファクタリング オプションには、フィールドをカプセル化する、メソッドのパラメーターにローカル変数の昇格およびメソッドのパラメーターの管理が含まれます。 これらのリファクタリング オプションの可用性は、コード内の場所に依存します。

### <a name="refactoring-code"></a>コードのリファクタリング

リファクタリングの一般的なシナリオは、(extract) を作成するメソッドなどの別のメンバー内にあるコードからのメソッド。 これにより、元のメンバーのサイズを縮小し、抽出されたコードを再利用可能な。

チュートリアルのこの部分では、単純なコードを記述し、そこからメソッドを抽出します。 リファクタリングはサポートされている c# の場合は、ため、プログラミング言語として c# を使用するページを作成します。

### <a name="to-extract-a-method-in-a-c-page"></a>C# のページのメソッドを抽出するには

1. 切り替える**デザイン**ビュー。
2. **ツールボックス**から、**標準** タブで、ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)をページにコントロール。
3. ダブルクリックして、**ボタン**のハンドラーを作成するコントロールをその[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント、し、次の強調表示されたコードを追加。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   このコードを作成、 **ArrayList**オブジェクト、ループを使用して値を持つロードおよび別のループを使用して、内容を表示、 **ArrayList**オブジェクト。
4. キーを押して**CTRL + F5**ページを実行し、**ボタン**次の出力が表示されるかどうかを確認します。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. コード エディターに戻り、イベント ハンドラーで、次の行を選択します。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. 選択範囲を右クリックし、をクリックして**リファクタリング**を選び、**メソッドの抽出**します。 

    **メソッドの抽出** ダイアログ ボックスが表示されます。
7. **新しいメソッドの名前**ボックスに「 **DisplayArray**、順にクリックします**OK**します。 

    コード エディターがという名前の新しいメソッドを作成`DisplayArray`で新しいメソッドの呼び出しを配置し、 **をクリックして**ループが元のハンドラー。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. キーを押して**CTRL + F5**ページを再度実行し、をクリックして、**ボタン**します。

    前に、と同様、同じページに機能します。 `DisplayArray`メソッドが任意の場所からの呼び出しにできるようになりましたページ クラスにします。

## <a name="renaming-variables"></a>変数の名前を変更します。

オブジェクトと変数を使用するときに、コードで既に参照されている後に名前を変更する可能性があります。 ただし、変数およびオブジェクトの名前を変更すると、参照のいずれかの名前の変更がない場合、コードが発生することができます。 そのため、リファクタリングを使用する名前の変更を実行できます。

### <a name="to-use-refactoring-to-rename-a-variable"></a>リファクタリングを使用して、変数の名前を変更するには


1. ** をクリックして**イベント ハンドラーでは、次の行を見つけます。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. 変数名を右クリックして`alist`、選択**リファクタリング**を選び、**の名前を変更**します。

    **の名前を変更** ダイアログ ボックスが表示されます。
3. **新しい名前**ボックスに「 **ArrayList1**を確認して、**参照の変更のプレビュー**チェック ボックスが選択されていますいます。 次に、 **[OK]** をクリックします。

    **変更のプレビュー**  ダイアログ ボックスが表示され、すべての参照の名前を変更する変数を含むツリーが表示されます。
4. クリックして**適用**を閉じる、**変更のプレビュー**  ダイアログ ボックス。

    選択したインスタンスを指している変数の名前が変更されます。 ただし、注意を変数`alist`次の行では、名前は変更されません。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    変数`alist`この行の名前は変更されず、変数と同じ値を表していないため、`alist`名前を変更しました。 変数`alist`で、`DisplayArray`宣言は、そのメソッドのローカル変数。 これは、リファクタリングを使用して、変数の名前を変更するが、エディターで単純に検索および置換操作を実行するよりも異なることを示しています操作している変数のセマンティクスの知識がある変数を名前変更をリファクタリングします。


## <a name="inserting-snippets"></a>スニペットの挿入

Web フォームの開発者が頻繁に実行する必要がある多くのコーディング作業があるため、コード エディターは、スニペット、または作成済みのコード ブロックのライブラリを提供します。 ページには、これらのスニペットを挿入できます。

Visual Studio で使用する各言語には、コード スニペットを挿入する方法にはわずかな違いがあります。 スニペットの挿入については、次を参照してください。 [Visual Basic の IntelliSense コード スニペット](https://msdn.microsoft.com/library/18yz4be4.aspx)します。 Visual c# スニペットの挿入については、次を参照してください。 [Visual c# コード スニペット](https://msdn.microsoft.com/library/z41h7fat.aspx)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、コードのエラーを修正、コードのリファクタリング、変数の名前を変更およびコードにコード スニペットの挿入の Visual Studio 2010 のコード エディターの基本的な機能を説明しました。 エディターで追加の機能には、アプリケーションの開発を高速かつ簡単を利用します。 たとえば、次の操作を行います。

- IntelliSense オプションの変更、コード スニペットを管理、コード スニペットをオンラインで検索などの IntelliSense の機能についてよりについて説明します。 詳細については、「[IntelliSense の使用](https://msdn.microsoft.com/library/hcw1s69b.aspx)」を参照してください。
- 独自のコード スニペットを作成する方法について説明します。 詳細については、次を参照してください[の作成と IntelliSense コード スニペットの使用。](https://msdn.microsoft.com/library/ms165392.aspx)
- IntelliSense コード スニペットでは、スニペットのカスタマイズやトラブルシューティングなどの Visual Basic 固有の機能について説明します。 詳細については、次を参照してください[Visual Basic の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/18yz4be4.aspx)
- 詳細については、c# は、リファクタリングとコード スニペットなど、IntelliSense の特定の機能。 詳細については、次を参照してください。 [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)します。
