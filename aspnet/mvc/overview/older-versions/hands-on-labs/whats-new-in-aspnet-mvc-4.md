---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 の新機能 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET MVC 4 は、安定したデザイン パターンと、ASP.NET の機能を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4 の新機能

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 は、安定したデザイン パターンと、ASP.NET と .NET framework の機能を使用して、スケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。 この新しい、4 番目のバージョンの framework は、モバイル web アプリケーションの開発を容易に焦点を当てています。

最初に、新しい ASP.NET MVC 4 プロジェクトを作成するときにここで、モバイル アプリケーションのプロジェクト テンプレートをモバイル デバイス向けのスタンドアロンのアプリのビルドに使用することができます。 さらに、ASP.NET MVC 4 統合 jQuery Mobile jQuery.Mobile.MVC NuGet パッケージを使用します。 jQuery Mobile は、HTML5 ベース フレームワークと Windows Phone、iPhone、Android を含む、すべての一般的なモバイル デバイス プラットフォームと互換性がある web アプリを開発するためです。 ただし、特殊化する場合は、ASP.NET MVC 4 こともできますをさまざまなデバイスごとに異なるビューを提供し、デバイス固有の最適化を指定できます。

このハンズオン ラボでは、ASP.NET MVC 4 で起動されます&quot;インターネット アプリケーション&quot;フォト ギャラリーのアプリケーションを作成するプロジェクト テンプレート。 JQuery Mobile、および ASP.NET MVC 4 の新機能を使用して、さまざまなモバイル デバイスとデスクトップの web ブラウザーでの互換性を確保するアプリを段階的に強化されます。 コードの生成と ASP.NET MVC 4 簡単方法のタスクをサポートすることにより非同期アクション メソッドを記述するための新しいコード レシピについても学びます&lt;ActionResult&gt;タイプを返します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。 このラボに固有のプロジェクトは[ASP.NET 4.5 Web フォームの新](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)です。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートに機能強化の利用します。
- モバイル デバイスでの表示を向上させるために、HTML5 ビューポート属性と CSS のメディア クエリを使用します。
- プログレッシブの機能強化とタッチ最適化 web UI を構築するため、jQuery Mobile を使用します。
- Mobile に固有のビューを作成します。
- ビュー スイッチャー コンポーネントを使用して、アプリケーションでのモバイルとデスクトップのビューを切り替える
- タスクのサポートを使用して非同期コント ローラーを作成します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。
- [ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 のインストールに含まれています)
- Windows Phone エミュレーター (に含まれる、 [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- 省略可能: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)で**Electric Plum iPhone シミュレーター** (iPhone シミュレーターと web アプリケーションを参照するために使用する手順 3) の場合のみ拡張機能

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 必要に応じて、そのコードのほとんどは、Visual Studio のコード スニペットを手動で追加することを回避するのに Visual Studio 内から使用することができますとして提供されます。

コード スニペットをインストールします。

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**Source\Setup**フォルダーです。
2. ダブルクリックして、 **Setup.cmd**を Visual Studio のコード スニペットをインストールするには、このフォルダー内のファイルです。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;です。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [新しい ASP.NET MVC 4 プロジェクト テンプレート](#Exercise1)
2. [フォト ギャラリーの Web アプリケーションを作成します。](#Exercise2)
3. [モバイル デバイスのサポートを追加します。](#Exercise3)
4. [非同期コント ローラーを使用します。](#Exercise4)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **60 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>手順 1: 新しい ASP.NET MVC 4 プロジェクト テンプレート

この演習では、ASP.NET MVC 4 プロジェクト テンプレートの拡張機能を調査します。 インターネット アプリケーションのテンプレートだけでなく MVC 3 に既に存在このバージョンが追加されますモバイル アプリケーションの別のテンプレートです。 最初に、各テンプレートの関連する機能の一部になります。 次に、作業する適切なアプローチを使用して、さまざまなプラットフォームでは正常にページを表示します。

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>タスク 1 - インターネット アプリケーション テンプレートの表示

1. 開いている**Visual Studio**です。
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーションです。** プロジェクトに名前を**PhotoGallery**、場所を選択 (または既定値) をクリック**OK**です。

    > [!NOTE]
    > 後で作成するようになりました PhotoGallery ASP.NET MVC 4 ソリューションをカスタマイズします。

    ![新しいプロジェクトを作成する](whats-new-in-aspnet-mvc-4/_static/image1.png "新規プロジェクトの作成")

    *新しいプロジェクトを作成します。*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。 ビュー エンジンとして Razor を選択していないことを確認してください。

    ![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image2.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")

    *新しい ASP.NET MVC 4 インターネット アプリケーションの作成*

    > [!NOTE]
    > ASP.NET MVC 3 razor 構文が導入されました。 その目的は、文字の数および fast およびワークフローをコーディングする流体を有効にすると、ファイルに必要なキーストロークを最小限に抑えるです。 Razor を活用して既存 c#/VB (またはその他の) 言語スキルと優れた HTML 構築ワークフローを使用できるテンプレートのマークアップ構文を配信します。
4. キーを押して**f5 キーを押して**にソリューションを実行し、更新されたテンプレートを表示します。 次の機能を確認することができます。

    **モダン スタイル テンプレート**

    テンプレートが更新されました、モダン外見の他のスタイルを提供することです。

    ![ASP.NET MVC 4 のスタイルを変更テンプレート](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 テンプレートのスタイルを変更")

    *ASP.NET MVC 4 のスタイルを変更テンプレート*

    ![新しい連絡先 ページ](whats-new-in-aspnet-mvc-4/_static/image4.png "新しい連絡先 ページ")

    *新しい連絡先 ページ*

    **アダプティブ レンダリング**

    チェック アウト、ブラウザー ウィンドウのサイズを変更し、どのページ レイアウトに動的に適応新しいウィンドウのサイズに注意してください。 これらのテンプレートは、アダプティブ レンダリング手法をカスタマイズすることがなく、デスクトップおよびモバイルの両方のプラットフォームで正しく表示するために使用します。

    ![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](whats-new-in-aspnet-mvc-4/_static/image5.png "別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート")

    *別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート*

    **JavaScript での豊富な UI**

    既定のプロジェクト テンプレートを別の拡張機能より対話的な JavaScript を提供する JavaScript の使用です。 テンプレートで使用されているログインおよび登録のリンクには、jQuery 検証を使用して、クライアント側からの入力フィールドを検証する方法が実現しました。

    ![jQuery 検証](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery 検証*

    > [!NOTE]
    > 2 つログイン セクションでは、最初のセクションで、通知には、サイトから登録されてアカウントを使用しておよび altenativelly ログオンする (既定で無効になっている) google のように別の認証サービスを使用して 2 番目のセクションを記録できます。
5. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
6. ファイルを開く**AuthConfig.cs**の下にある、**アプリ\_開始**フォルダーです。
7. Google クライアントを登録する最後の行からコメントを削除する*OAuth*認証します。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. キーを押して**f5 キーを押して**ソリューションを実行し、ログイン ページに移動します。
9. 選択**Google**サービスにログインします。

    ![サービスのログを選択します。](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *サービスのログを選択します。*
10. Google アカウントを使用してをログインします。
11. Google アカウントから情報を取得するサイト (localhost) を許可します。
12. 最後に、Google アカウントに関連付けるサイトで登録する必要があります。

   ![Google アカウントを関連付ける](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Google アカウントの関連付け*
13. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
14. 他のプロジェクト テンプレートの ASP.NET MVC 4 で導入された新しい機能を確認するようにソリューションを調べるようになりました。

   ![ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")

   *ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート*

   - **HTML 5 マークアップ**

       新しいテーマ マークアップを調べるにはテンプレート ビューを参照します。

       ![Razor、HTML5 マークアップ About.cshtml を使用して新しいテンプレートです。] (whats-new-in-aspnet-mvc-4/_static/image10.png "Razor、HTML5 マークアップ About.cshtml を使用して、新しいテンプレート。")

       *Razor、HTML5 のマークアップ (About.cshtml) を使用して新しいテンプレートです。*
   - **更新された JavaScript ライブラリ**

       ASP.NET MVC 4 の既定のテンプレートには、KnockoutJS、豊富な作成できる JavaScript MVVM フレームワークと応答性の高い web アプリケーションの JavaScript と HTML を使ったが含まれています。 ような MVC3、jQuery、jQuery UI ライブラリにも含まれます ASP.NET MVC 4 です。

     > [!NOTE]
     > このリンクに KnockOutJS ライブラリに関する詳しい情報を入手できます: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/)です。 さらに、jQuery、jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)です。

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>タスク 2 - モバイル アプリケーション テンプレートの表示

ASP.NET MVC 4 には、モバイル web サイトやタブレットのブラウザーの開発が容易になります。 このテンプレート (通知をコント ローラーのコードがほぼ同一である)、インターネット アプリケーションのテンプレートと同じアプリケーション構造を持つが、モバイル デバイスのタッチ ベースで正しく表示するために、スタイルが変更されました。

1. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択、 **ASP.NET MVC 4 Web アプリケーションです。** プロジェクトに名前を**PhotoGallery.Mobile**、場所を選択 (または既定値)、選択&quot;ソリューションに追加&quot; をクリック**OK**です。
2. **新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、 **Mobile アプリケーション**プロジェクト テンプレートをクリック**OK**です。 ビュー エンジンとして Razor を選択していないことを確認してください。

    ![新しい ASP.NET MVC 4 モバイル アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image11.png "新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。")

    *新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。*
3. これで、ソリューションを探索し、モバイル用に ASP.NET MVC 4 ソリューション テンプレートで導入された新機能の一部を確認することは。

    - **jQuery Mobile ライブラリ**

        モバイル アプリケーション プロジェクト テンプレートには、モバイル ブラウザーの互換性のため、オープン ソース ライブラリは、jQuery モバイル ライブラリが含まれています。 jQuery Mobile では、CSS および JavaScript をサポートするモバイル ブラウザーに段階的な強化が適用されます。 段階的な強化は、豊富な内容を表示する最も強力なブラウザーだけことができます。 中に、web ページの基本コンテンツを表示するすべてのブラウザーを使用できます。 JQuery モバイル スタイルに含まれる、JavaScript および CSS ファイルでは、ページのマークアップでの変更を加えずに 画面で、コンテンツに合わせてモバイル ブラウザーを使用できます。

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *テンプレートに含まれる jQuery モバイル ライブラリ*
    - **HTML5 ベースのマークアップ**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *HTML5 マークアップ、(Login.cshtml および index.cshtml) を使用してテンプレートをモバイル アプリケーション*
4. キーを押して**f5 キーを押して**ソリューションを実行します。
5. 開く、 **Windows Phone 7 エミュレーター**です。
6. Phone のスタート画面で Internet Explorer を開きます。 チェック アウトし、デスクトップ アプリケーションが開始された URL および電話からその URL に移動 (例: `http://localhost:[PortNumber]/`)。
7. ログイン ページに入力するか、チェック アウトすることができるので、ページに関するします。 Web サイトのスタイルが mobile 用の新しい Metro アプリに基づくことに注意してください。 ASP.NET MVC 4 プロジェクト テンプレートは、ページのすべての要素が表示され、有効になっていることを確認、モバイル デバイスで正しく表示されます。 ヘッダーのリンクがクリックしてまたはタップするのに十分な大きさであることを確認します。

    ![プロジェクトのモバイル デバイス上でテンプレート ページ](whats-new-in-aspnet-mvc-4/_static/image14.png "モバイル デバイス上でテンプレート ページのプロジェクト")

    *モバイル デバイスでのプロジェクト テンプレートのページ*
8. 新しいテンプレートを使用しても、**ビューポート メタ タグ**です。 ほとんどのモバイル ブラウザーは、仮想のブラウザー ウィンドウの幅を定義または&quot;ビューポート&quot;、これは、モバイル デバイスの実際の幅よりも大きいです。 これにより、モバイル ブラウザーに仮想の表示内の web ページ全体を表示できます。 **ビューポート メタ タグ**により、モバイル デバイスで、幅、高さ、およびブラウザーの領域のスケールを設定する web 開発者**です。** ASP.NET MVC 4 アプリケーションにテンプレートをモバイル デバイスの幅に、ビューポートを設定する (&quot;幅デバイスの幅を =&quot;) レイアウト テンプレート (*\shared\_Layout.cshtml*) できるように、すべて、ページには、デバイスの画面幅を設定、ビューポートがあります。 ビューポートのメタ タグによって既定のブラウザー ビューは変更されないことに注意してください。
9. 開いている **\_Layout.cshtml**内にある、**ビュー |共有**フォルダー、およびコメント、ビューポートのメタ タグ。 アプリケーションを実行しない場合既に開き、相違点を確認します。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。
11. ビューポートのメタ タグのコメントを解除します。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>タスク 3 - アダプティブ レンダリングを使用します。

このタスクでは、モバイル デバイスと Web ブラウザーに正しく、Web ページをカスタマイズすることがなく、同時に表示するために別の方法を学習します。 同様の目的で、ビューポートのメタ タグを使用していた既にです。 これで、別の強力なメソッドを満たすことになります:*アダプティブ レンダリング*です。

アダプティブ レンダリングが使用する手法**CSS3 メディア クエリ**をページに適用されるスタイルをカスタマイズします。 メディア クエリは、特定の条件下の CSS スタイルをグループ化、スタイル シート内の条件を定義します。 条件が true の場合のみ、スタイルが宣言されたオブジェクトに適用されます。

アダプティブ レンダリング手法が提供する柔軟性は、さまざまなデバイスで、サイトを表示するためには、そのカスタマイズを使用できます。 必要な 1 つのスタイル シートのロジックのコードを記述することがなく、スタイルを選択する数だけのスタイルを定義することができます。 そのため、その量が減り、重複したコードと目的を表示するためのロジックとページのスタイルを適用する非常に便利な方法になります。 その一方で、帯域幅の消費量が増加、CSS ファイルのサイズが多少大きくなりすぎるとします。

アダプティブ レンダリング手法を使用すると、サイトになります**ブラウザーに関係なく、適切に表示されます。** ただし、余分な帯域幅を読み込む場合は、問題を考慮する必要があります。

> [!NOTE]
> メディア クエリの基本形式は: @media \[スコープ: すべて | ハンドヘルド | 印刷 | プロジェクション | 画面\]([プロパティ: 値].プロパティ: 値)


メディア クエリの例: &gt;  <strong>@mediaすべてと (幅の最大値: 1000px) と (幅の最小値: 700px) {}:</strong> 700px と 1000px 間のすべての解像度。

> <strong>@media 画面と (幅の最小値: 400 px と表) と (幅の最大値: 700px) {...}:</strong>画面に対してのみです。 解像度は、400 ~ 700px でなければなりません。
> 
> <strong>@media ハンドヘルドと (幅の最小値: 20em)、画面と (幅の最小値: 20em) {...}:</strong>のハンドヘルド デバイス (携帯とデバイス) とスクリーンです。 幅の最小値は、20em より大きくする必要があります。
> 
> 上の詳細については、これを見つけることができます、 [W3C サイト](http://www.w3.org/TR/css3-mediaqueries/)です。


アダプティブのレンダリングの動作方法について学びますようになりました、4 既定 web サイト テンプレートの ASP.NET MVC の読みやすさを向上します。

1. 開く、 **PhotoGallery.sln**ソリューション タスク 1 で作成し、選択、 **PhotoGallery**プロジェクト。 キーを押して**f5 キーを押して**ソリューションを実行します。
2. ブラウザーの幅、または元のサイズの四半期未満を半分に、windows の設定を変更します。 ヘッダー内の項目に対して行う処理に注意してください。 一部の要素は、ヘッダーの表示領域に表示されません。
3. 開いている<strong>Site.css</strong>ファイルにある、Visual Studio のソリューション エクスプ ローラーから<strong>コンテンツ</strong>プロジェクト フォルダーです。 キーを押して<strong>ctrl キーを押しながら F</strong> Visual Studio の統合された検索を開き、書き込む<strong>@media</strong>を検索、 <strong>CSS メディア クエリ</strong>です。

    このテンプレートで定義されているメディア クエリの条件は、この方法で機能します。 ブラウザーのウィンドウのサイズが以下の場合**850 px**、適用される CSS ルールがこのメディア ブロック内で定義されています。

    ![メディア クエリを検索する](whats-new-in-aspnet-mvc-4/_static/image16.png "メディア クエリの検索")

    *メディア クエリの検索*
4. 850 に設定された最大幅属性値を置き換えます px で**10 px**アダプティブのレンダリングを無効にするために、キーを押します**ctrl キーを押しながら S**変更を保存します。 キーを押して、ブラウザーに戻り**ctrl キーを押しながら f5 キーを押して**に加えた変更にページを更新します。 ウィンドウの幅を調整するときは、両方のページの相違点に注意してください。

    ![左のページを適用する、@media右上、スタイルのスタイルを省略すると](whats-new-in-aspnet-mvc-4/_static/image17.png "、左のページを適用する、@media右上、スタイルのスタイルを省略すると")

    <em>左のページを適用する、@media右上、スタイルのスタイルを省略した場合</em>

    ここで、モバイル デバイス上の動作を確認しましょう。

    ![左のページを適用する、@media右上、スタイルのスタイルを省略すると](whats-new-in-aspnet-mvc-4/_static/image18.png "、左のページを適用する、@media右上、スタイルのスタイルを省略すると")

    <em>左のページを適用する、@media右上、スタイルのスタイルを省略した場合</em>

    モバイル デバイスを使用する場合に、ページが Web ブラウザーで表示される場合は、その変更が非常に重要なされないことがわかりますが、違いがより明確になります。 イメージの左側にある、カスタム スタイルが読みやすさを向上することを確認できます。

    アダプティブ レンダリングは、条件付きスタイルの Web サイトと便利な方法で、スタイルの一般的な問題の解決を適用しやすくより多くの多くのシナリオで使用できます。

    ビューポートのメタ タグと CSS のメディア クエリに限定されない ASP.NET MVC 4 web アプリケーションでこれらの機能の利点を行えるようにします。
5. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>手順 2: フォト ギャラリーの Web アプリケーションの作成

この演習では、写真を表示する、フォト ギャラリーのアプリケーションで動作します。 ASP.NET MVC 4 プロジェクト テンプレートを使用するが起動し、サービスから写真を取得し、それらをホーム ページに表示するための機能を追加し。

次の手順でモバイル デバイスでの表示方法を強化するためにこのソリューションが更新されます。

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>タスク 1 - モック フォト サービスを作成します。

このタスクでは、ギャラリーに表示されるコンテンツを取得するフォト サービスのモックを作成します。 これを行うには、それぞれの写真データを含む JSON ファイルが返されるだけ、新しいコント ローラーを追加します。

1. 開いている**Visual Studio**開いていない場合。
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーションです。** プロジェクトに名前を**PhotoGallery**、場所を選択 (または既定値) をクリック**OK**です。 既存の ASP.NET MVC 4 から作業を継続する代わりに、**インターネット アプリケーション**からソリューション**演習 1**し、次の手順をスキップします。
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。 Razor ビュー エンジンとして選択されている必要があることを確認してください。
4. **ソリューション エクスプ ローラー**を右クリックし、**アプリ\_データ**フォルダー、プロジェクト、および選択の**追加 |既存の項目**です。 参照、 **Source\Assets\App\_データ**このラボのフォルダーを追加し、 **Photos.json**ファイル。
5. 名前を持つ新しいコント ローラーを作成**PhotoController**です。 これを行うを右クリックし、**コント ローラー**フォルダーに移動して**追加**選択**コント ローラー。** ままの状態で完了、コント ローラー名、**空の MVC コント ローラー**テンプレートとクリック**追加**です。

    ![追加、PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController を追加します。")

    *PhotoController を追加します。*
6. 置換、**インデックス**メソッドに次の**ギャラリー**アクション、およびプロジェクトに最近追加した JSON ファイルから返されるコンテンツ。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - ギャラリー アクション*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. キーを押して**f5 キーを押して**ソリューションを実行し、モック フォト サービスをテストするために、次の URL を参照する: `http://localhost:[port]/photo/gallery` ([ポート] の値は、アプリケーションが起動される現在のポートによって異なります)。 この URL に要求のコンテンツを取得する必要があります、 **Photos.json**ファイル。

    ![モック フォト サービスのテスト](whats-new-in-aspnet-mvc-4/_static/image20.png "モック フォト サービスのテスト")

    *モック フォト サービスのテスト*

実際の実装で使用する可能性があります[ASP.NET Web API](../../../../web-api/index.md)してフォト ギャラリーのサービスを実装します。 ASP.NET Web API は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達できる HTTP サービスを作成しやすくフレームワークです。 ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>タスク 2 - フォト ギャラリーを表示します。

このタスクでこの手順の最初のタスクで作成したモック サービスを使用して、フォト ギャラリーを表示するホーム ページが更新されます。 モデル ファイルを追加し、ギャラリー ビューを更新します。

1. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。
2. 作成、**フォト**クラス内で、**モデル**フォルダーです。 これを行うを右クリックし、**モデル**フォルダーを選択**追加** をクリック**クラス**です。 名前に設定し、 **Photo.cs**  をクリック**追加**です。
3. 次のメンバーを追加、**フォト**クラスです。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - フォト モデル*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. **Controllers** フォルダーから **HomeController.cs** ファイルを開きます。
5. 次の using ステートメントを追加します。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - HomeController Using*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. 更新プログラム、**インデックス**で使用するアクション**HttpClient**ギャラリーのデータを取得し、使用して、 **JavaScriptSerializer**ビュー モデルにシリアル化を解除します。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - インデックス アクション*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. 開く、 **Index.cshtml**の下にあるファイル、 **views \home**フォルダーと次のコードでのすべてのコンテンツを置換します。

    このコードは、サービスから取得したすべての写真をループし、それらが順序なしのリストに表示されます。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex02 - フォト一覧*)


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. **ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**フォルダー、プロジェクト、および選択の**追加 |既存の項目**です。 参照、 **Source\Assets\Content**このラボのフォルダーを追加し、 **Site.css**ファイル。 それに置き換わることを確認する必要があります。 ある場合、 **Site.css**ファイルを開く、また、ファイルを再読み込みすることを確認する必要があります。
9. ファイル エクスプ ローラーを開き、全体をコピー**写真**フォルダーの下にある、 **Source\Assets**ソリューション エクスプ ローラーでプロジェクトのルート フォルダーには、このラボのフォルダーです。
10. アプリケーションを実行します。 ギャラリーで写真を表示するホーム ページが表示されます。

    ![フォト ギャラリー](whats-new-in-aspnet-mvc-4/_static/image21.png "フォト ギャラリー")

    *フォト ギャラリー*
11. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>手順 3: モバイル デバイスのサポートを追加します。

ASP.NET MVC 4 のキーの更新の 1 つは、モバイル開発のサポートです。 この演習では、前述の手順で作成した PhotoGallery ソリューションを拡張してモバイル アプリケーションの ASP.NET MVC 4 の新機能を調査します。

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>タスク 1 - ASP.NET MVC 4 アプリケーションで jQuery Mobile をインストールします。

1. 開く、**開始**ソリューションにある**ソース/Ex3-MobileSupport/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **Package Manager Console**  をクリックして、**ツール** &gt; **ライブラリ パッケージ マネージャー** &gt; **パッケージ マネージャーコンソール**メニュー オプション。

    ![NuGet パッケージ マネージャー コンソールを開く](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet パッケージ マネージャー コンソールを開く")

    *NuGet パッケージ マネージャー コンソールを開く*
3. パッケージ マネージャー コンソールをインストールする次のコマンドを実行、 **jQuery.Mobile.MVC**パッケージです。

    jQuery Mobile は、タッチ最適化 web UI を構築するため、オープン ソース ライブラリです。 JQuery.Mobile.MVC NuGet パッケージには、ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用するヘルパーが含まれています。

    > [!NOTE]
    > 次のコマンドを実行するには Nuget から jQuery.Mobile.MVC ライブラリがダウンロードするされます。

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    このコマンドは、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。

    - **Views/shared/\_Layout.Mobile.cshtml**: jQuery Mobile ベース レイアウト小さいスクリーン用に最適化されています。 Web サイトは、モバイル ブラウザーから要求を受信したときに元のレイアウトが置き換えられます。 (\_Layout.cshtml) このものとします。
    - ビュー スイッチャー コンポーネント: から成る、 **Views/shared/\_ViewSwitcher.cshtml**部分ビュー、および**ViewSwitcherController.cs**コント ローラー。 このコンポーネントは、ページのデスクトップ バージョンに切り替えるにはユーザーを有効にするモバイル ブラウザーでリンクを表示します。  
        ![モバイル デバイスのサポートをフォト ギャラリー プロジェクト](whats-new-in-aspnet-mvc-4/_static/image23.png "モバイル デバイスのサポートをフォト ギャラリーのプロジェクト")

        *モバイル デバイスのサポートをフォト ギャラリーのプロジェクト*
4. モバイルのバンドルを登録します。 これを行うには、開く、 **Global.asax.cs**ファイルし、次の行を追加します。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex03 - 登録モバイル バンドル*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. デスクトップの web ブラウザーを使用してアプリケーションを実行します。
6. 開く、 **Windows Phone 7 エミュレーター**にある**[スタート] メニュー |すべてのプログラム |Windows Phone SDK 7.1 |Windows Phone エミュレーター。**
7. Phone のスタート画面で Internet Explorer を開きます。 アプリケーションの開始 URL を確認し、電話のブラウザーでその URL を参照 (例: `http://localhost:[PortNumber]/`)。

    JQuery.Mobile.MVC がモバイル デバイス用に最適化されたビューを表示する、プロジェクトに新しい資産を作成するように、アプリケーションを Windows Phone エミュレーターで、異なるが表示されることがわかります。

    電話、デスクトップ ビューに切り替える リンクが表示の上部にあるメッセージに注意してください。 さらに、  **\_Layout.Mobile.cshtml**がインストールされているパッケージによって作成されたレイアウトが、アプリケーションで別のレイアウトを含むです。

    > [!NOTE]
    > これまで、モバイル ビューに戻るにリンクすることはありません。 これは、以降のバージョンに含まれます。

    ![フォト ギャラリーのホーム ページのモバイル ビュー](whats-new-in-aspnet-mvc-4/_static/image24.png "フォト ギャラリーのホーム ページのモバイル ビュー")

    *フォト ギャラリーのホーム ページのモバイル ビュー*
8. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>タスク 2 - モバイル ビューの作成

このタスクでは、モバイル デバイスの優れた appareance に適応させるコンテンツをインデックス ビューのモバイル バージョンを作成します。

1. コピー、 **Views\Home\Index.cshtml**を表示し、コピーを作成、新しいファイルの名前を変更する貼り付けます**Index.Mobile.cshtml**です。
2. 開いている、新しく作成された**Index.Mobile.cshtml**を表示し、既存の置換&lt;ul&gt;このコードを持つタグ。 これにより、更新されます。、 &lt;ul&gt;から jQuery モバイルのテーマを使用して jQuery モバイル データ注釈を使用したタグです。


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. キーを押して**ctrl キーを押しながら S**変更を保存します。
4. 切り替えて、 **Windows Phone エミュレーター**と、サイトを更新します。 一番上にある新しい検索ボックスと同様に、ギャラリーのリストの新しいルック アンド フィールに注意してください。 検索ボックスに単語を入力し、(たとえば、 **Tulips**) フォト ギャラリーで検索を開始します。

    ![Listview のスタイルを使用して、フィルター選択をギャラリー](whats-new-in-aspnet-mvc-4/_static/image25.png "listview のスタイルを使用して、フィルター選択をギャラリー")

    *Listview のスタイルを使用して、フィルター選択をギャラリー*

    要約すると、インデックス ビューのコピーを作成するビューというレシピを使用した、&quot;モバイル&quot;サフィックス。 このサフィックスは ASP.NET MVC 4 にモバイル デバイスから生成されたすべての要求がこのインデックスのコピーを使用することを示します。 さらに、モバイル デバイスでのサイトのルック アンド フィールを強化するため、jQuery Mobile を使用するインデックス ビューのモバイル バージョンに更新されました。
5. Visual Studio に戻り、開いている**Site.Mobile.css**の下にある、**コンテンツ**フォルダーです。
6. イメージの右側に表示する写真のタイトルの配置を修正します。 これを行うには、次のコードを追加、 **Site.Mobile.css**ファイル。

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. キーを押して**ctrl キーを押しながら S**変更を保存します。
8. 戻り、 **Windows Phone エミュレーター**と、サイトを更新します。 写真のタイトルが正しく配置されているようになりましたことを確認します。

    ![イメージの右側に配置されているタイトル](whats-new-in-aspnet-mvc-4/_static/image26.png "イメージの右側に配置されているタイトル")

    *イメージの右側に配置されているタイトル*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>タスク 3 - jQuery Mobile テーマ

新しいオブジェクト指向 CSS フレームワーク サイトおよびアプリケーションに完全な統一されたビジュアル デ ザイン テーマを適用できるようにする軸は、すべてのレイアウトと jQuery Mobile でウィジェットを設計します。

jQuery モバイルの既定テーマにはには、文字が与えられている 5 見本が含まれています。 (a、b、c、d、e) のクイック リファレンスです。

このタスクで、既定よりも、さまざまなテーマを使用するモバイルのレイアウトが更新されます。

1. Visual Studio に切り替えます。
2. 開く、  **\_Layout.Mobile.cshtml**にあるファイル**\shared**です。
3. 設定のデータの役割を持つ div 要素を検索&quot;ページ&quot;し、更新、**データ テーマ**に&quot; **e**&quot;です。


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. キーを押して**ctrl キーを押しながら S**変更を保存します。
5. サイトを更新、 **Windows Phone エミュレーター**し、新しい色スキームのことを確認します。

    ![さまざまな画面の配色とレイアウトをモバイル](whats-new-in-aspnet-mvc-4/_static/image27.png "さまざまな画面の配色とモバイルのレイアウト")

    *さまざまな画面の配色とモバイルのレイアウト*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>タスク 4 - ビュー スイッチャー コンポーネントおよび機能をオーバーライドするブラウザーを使用して

モバイルに最適化された web ページの規則では、テキストはデスクトップ ビューまたはユーザーがページのデスクトップ バージョンに切り替えるできるサイトの完全モードと同様に、何かのリンクを追加します。 JQuery.Mobile.MVC パッケージには、サンプルが含まれています。**ビュー スイッチャー**コンポーネントをこの目的で使用される、  **\_Layout.Mobile.cshtml**ビュー。

![デスクトップ ビューに切り替えるにはリンク](whats-new-in-aspnet-mvc-4/_static/image28.png "デスクトップ ビューに切り替えてへのリンク")

*デスクトップ ビューに切り替えるにはリンクします。*

ビュー スイッチャーと呼ばれる新しい機能を使用して**ブラウザー オーバーライド**です。 この機能は、アプリケーションでから実際に送信されている 1 つの異なるブラウザー (ユーザー エージェント) から送信されているかのように要求を処理できます。

このタスクでは、jQuery.Mobile.MVC および ASP.NET MVC 4 の機能をオーバーライドする、新しいブラウザーにより追加されたビュー スイッチャーの実装サンプルを調査します。

1. Visual Studio に切り替えます。
2. 開く、  **\_Layout.Mobile.cshtml**ビューの下にある、 **\shared**フォルダーと、部分ビューとして参照されているビュー スイッチャー コンポーネントに注意してください。

    ![ビュー スイッチャー コンポーネントを使用してモバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image29.png "ビュー スイッチャー コンポーネントを使用してモバイルのレイアウト")

    *ビュー スイッチャー コンポーネントを使用してモバイルのレイアウト*
3. 開く、  **\_ViewSwitcher.cshtml**部分ビュー。

    部分ビューは、新しいメソッドを使用して**ViewContext.HttpContext.GetOverriddenBrowser()** web 要求の起点を特定し、デスクトップまたはモバイル ビューのいずれかを切り替えるには、対応するリンクを表示します。

    **GetOverridenBrowser**メソッドを返します、 **HttpBrowserCapabilitiesBase**要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライドされる)。 この値を使用するにはプロパティを取得するよう**IsMobileDevice**です。

    ![部分ビューの ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分ビュー")

    *ViewSwitcher 部分ビュー*
4. 開く、 **ViewSwitcherController.cs**クラス、**コント ローラー**フォルダーです。 チェック アウトする SwitchView アクションは ViewSwitcher コンポーネント内のリンクによって呼び出され、新しい HttpContext メソッドに注意してください。

    - **HttpContext.ClearOverridenBrowser()**メソッドは現在の要求に対するすべてのオーバーライドされたユーザー エージェントを削除します。
    - **HttpContext.SetOverridenBrowser()**メソッドは、指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。  
        ![ViewSwitcher コント ローラー](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher コント ローラー")  
*ViewSwitcher コント ローラー*

        ブラウザーのオーバーライドは ASP.NET MVC 4 の中心的な機能も使用可能な jQuery.Mobile.MVC パッケージをインストールしない場合でもです。 ただし、この機能は、ビュー、レイアウト、および部分ビューのみに影響ありの Request.Browser オブジェクトに依存する機能は影響しません。

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>タスク 5 - デスクトップ ビューでビュー スイッチャーを追加します。

このタスクでビュー スイッチャーを含めるデスクトップのレイアウトが更新されます。 これにより、デスクトップのビューを参照するときに、モバイル ビューに戻るモバイル ユーザーが許可されます。

1. サイトを更新、 **Windows Phone エミュレーター**です。
2. をクリックして、**デスクトップ ビュー**ギャラリーの上部にあるリンクします。 モバイル ビューに戻るようにデスクトップ ビューでビュー スイッチャーがないことを確認します。
3. Visual Studio に戻り、開く、  **\_Layout.cshtml**ビュー。
4. ログイン セクションを検索して表示するために呼び出しを挿入、  **\_ViewSwitcher**部分ビューの下、  **\_LogOnPartial**部分ビュー。 次に、キーを押して**ctrl キーを押しながら S**変更を保存します。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. キーを押して**ctrl キーを押しながら S**変更を保存します。
6. Windows Phone エミュレーターでページを更新し、拡大する画面をダブルクリックします。 ホーム ページの表示通知、**モバイル ビュー**モバイルからデスクトップ ビューに切り替えるリンクします。

    ![デスクトップ ビューに表示切り替えコントロールの表示](whats-new-in-aspnet-mvc-4/_static/image32.png "デスクトップ ビューに表示されるビュー スイッチャー")

    *デスクトップ ビューに表示されるビュー スイッチャー*
7. もう一度モバイル ビューに切り替えるしを参照<strong>に関する</strong>ページ (http://localhost[port] ホーム/について)。 、About.Mobile.cshtml ビューを作成していない場合でも、バージョン情報 ページが表示されているモバイルのレイアウトを使用してに注意してください (\_Layout.Mobile.cshtml)。

    ![ページに関する](whats-new-in-aspnet-mvc-4/_static/image33.png "ページについて")

    *ページについて*
8. 最後に、デスクトップ、Web ブラウザーでサイトを開きます。 デスクトップ ビューに影響なし前の更新プログラムのことに注意してください。

    ![PhotoGallery デスクトップ ビュー](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery デスクトップ ビュー")

    *PhotoGallery デスクトップ ビュー*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>タスク 6 - 新しい表示モードの作成

新しい表示モードの機能はにより、アプリケーションは、要求を生成しているブラウザーによってビューを選択です。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返す、 **Views\Home\Index.cshtml**テンプレート。 次に、モバイル ブラウザーは、ホーム ページを要求している場合、アプリケーションを返します、 **Views\Home\Index.mobile.cshtml**テンプレート。

このタスクで、iPhone デバイス用のカスタマイズされたレイアウトを作成しますおよび iPhone デバイスからの要求をシミュレートする必要があります。 これを行うには、いずれか、iPhone エミュレーターまたはシミュレーターを使用することができます (と同様に[Electric Mobile シミュレーター](http://www.electricplum.com/)) またはブラウザーのアドオンのユーザー エージェントを変更します。 IPhone をエミュレートするために、Safari ブラウザーでユーザー エージェント文字列を設定する方法については、次を参照してください。 [IE が見かけ上 Safari させる方法について](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison のブログでします。

**このタスクは省略可能なことと、実行せず、ラボ全体で続行することができますに注意してください。**

1. Visual Studio で、キーを押します。 **shift キーを押し** + **f5 キーを押して**アプリケーションのデバッグを停止します。
2. 開いている**Global.asax.cs**に次の追加およびステートメントを使用します。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. アプリケーションに次の強調表示されたコードを追加\_メソッドを開始します。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex03 - iPhone DisplayMode*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. コピーを作成、  **\_Layout.Mobile.cshtml**ファイルで、 **\shared**フォルダーへのコピーの名前を変更および&quot;  **\_Layout.iPhone.csthml**&quot;.
5. 開いている **\_Layout.iPhone.csthml**前の手順で作成します。
6. データ ロールの属性に設定された div 要素を検索**ページ**を変更して、**データ テーマ**属性を&quot; **、**&quot;です。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. キーを押して**f5 キーを押して**アプリケーションを実行し、サイトの参照を**Windows Phone エミュレーター**です。
8. 開いている、 **iPhone シミュレーター** (を参照してください[付録 C](#AppendixC)インストールして iPhone シミュレーターを構成する方法について)、サイトを参照するすぎるとします。 各電話番号が、特定のテンプレートを使用することに注意してください。

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *各モバイル デバイスのさまざまなビューを使用します。*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>手順 4: 非同期コント ローラーを使用します。

Microsoft .NET Framework 4.5 では、.NET プログラミングでの非同期性の新しい基盤を提供する c# および Visual Basic での言語の新機能について説明します。 この新しい foundation、非同期プログラミングのと同様と同期プログラミングと約簡単です。 使用して、ASP.NET MVC 4 で非同期アクション メソッドを記述することができるよう、 **AsyncController**クラスです。 CPU バインド以外の要求を実行時間の長いの非同期アクション メソッドを使用できます。 要求の処理中に作業を実行してから、Web サーバーのブロックを回避できます。 AsyncController クラスは通常、実行時間の長い Web サービス呼び出しに使用されます。

この演習では、ASP.NET MVC 4 での非同期操作の基本について説明します。 詳しく知りたい場合は、次の記事を確認できます。 [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>タスク 1 - 非同期コント ローラーを実装します。

1. 開く、**開始**ソリューションにある**ソース/Ex4-Async/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. 開く、 **HomeController.cs**クラス、**コント ローラー**フォルダーです。
3. 次の追加ステートメントを使用します。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. 更新プログラム、 **HomeController**クラスから継承する**AsyncController**です。 AsyncController から派生したコント ローラーが非同期の要求の処理に ASP.NET を有効にしてサービスの同期アクション メソッドは、できます。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. 追加、 **async**キーワードを**インデックス**メソッド型を返すと**タスク&lt;ActionResult&gt;**です。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. 置換、**クライアント。GetAsync()**次のように、完全非同期バージョンを使用して、使用して呼び出し await キーワード。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - されます*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. 次に示すように、新しいコードで行を置き換えることで、非同期実装を続行するコードを変更します。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - ReadAsStringAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. アプリケーションを実行します。 わかりますなし、主要な変更がコードでは、サーバー リソースの使用率の向上を行うと、パフォーマンスの向上は、スレッド プールのスレッドはブロックされません。

    > [!NOTE]
    > ラボで新しい非同期プログラミング機能について詳しく知ることができますできます&quot; **c# および Visual Basic と .NET 4.5 での非同期プログラミング**&quot; Visual Studio のトレーニング キットに含まれています。

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>タスク 2 - キャンセル トークンと処理のタイムアウト

タスク インスタンスを返す非同期アクション メソッドには、タイムアウトもサポートできます。 このタスクのキャンセル トークンを使用してタイムアウト シナリオを処理するインデックス メソッドのコードが更新されます。

1. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。
2. 以下を追加するステートメントを使用して、 **HomeController.cs**ファイル。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. 更新を受信するインデックス アクション、 **CancellationToken**引数。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. 更新プログラム、**されます**キャンセル トークンを渡してへの呼び出しです。

    (コード スニペットの*CancellationToken に ASP.NET MVC 4 ラボ - Ex04 - SendAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. 装飾、*インデックス*メソッドを**AsyncTimeout**属性は 500 ミリ秒に設定し、 **HandleError**属性を処理するように構成**TaskCanceledException**にリダイレクトすることで、 **TimedOut**ビュー。

    (コード スニペットの*ASP.NET MVC 4 ラボ - Ex04 - 属性*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. 開く、 **PhotoController**クラスと更新プログラム、**ギャラリー**実行時間の長いタスクをシミュレートするために実行 1000 ミリ秒 (1 秒) を遅延するメソッド。


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. 開く、 **Web.config**ファイルし、次の要素を追加することによってカスタム エラーを有効にします。


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. 新しいビューを作成**\shared**という**TimedOut**および既定のレイアウトを使用します。 ソリューション エクスプ ローラーで右クリックし、 **\shared**フォルダーと選択**追加 |ビュー**です。

    ![各モバイル デバイスのさまざまなビューを使用して](whats-new-in-aspnet-mvc-4/_static/image36.png "各モバイル デバイスのさまざまなビューを使用します。")

    *各モバイル デバイスのさまざまなビューを使用します。*
9. 更新プログラム、 **TimedOut**次に示すようにコンテンツを表示します。


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. アプリケーションを実行し、ルートの URL に移動します。 追加したときに、 **Thread.Sleep** 1000 ミリ秒のによって生成された、タイムアウト エラーが表示されます、 **AsyncTimeout**属性し catch、 **HandleError**属性です。

    ![処理のタイムアウト例外](whats-new-in-aspnet-mvc-4/_static/image37.png "処理のタイムアウト例外")

    *処理のタイムアウト例外*

> [!NOTE]
> Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 d: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixD)です。


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオンのラボ、するした経験でいくつかの新機能の ASP.NET MVC 4 に常駐しています。 次の概念を説明しています。

- ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートに機能強化の利用します。
- モバイル デバイスでの表示を向上させるために、HTML5 ビューポート属性と CSS のメディア クエリを使用します。
- プログレッシブの機能強化とタッチ最適化 web UI を構築するため、jQuery Mobile を使用します。
- Mobile に固有のビューを作成します。
- ビュー スイッチャー コンポーネントを使用して、アプリケーションでのモバイルとデスクトップのビューを切り替える
- タスクのサポートを使用して非同期コント ローラーを作成します。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>付録 a: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-aspnet-mvc-4/_static/image38.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペット名を入力する開始](whats-new-in-aspnet-mvc-4/_static/image39.png "スニペット名の入力を開始")

*スニペット名の入力を開始します。*

![Tab キーを押して、強調表示されているスニペットを選択する](whats-new-in-aspnet-mvc-4/_static/image40.png "強調表示されているスニペットを選択するキーを押してタブ")

*Tab キーを押して、強調表示されているスニペットを選択するには*

![キーを押して タブで再度と、スニペットが展開](whats-new-in-aspnet-mvc-4/_static/image41.png "キーを押して タブで再度と、スニペットが展開されます")

*キーを押して タブで再度と、スニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加するには***

1. コード スニペットを挿入する場所を右クリックします。
2. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
3. クリックして一覧から、関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](whats-new-in-aspnet-mvc-4/_static/image42.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

![クリックして一覧から、関連するスニペットを選択](whats-new-in-aspnet-mvc-4/_static/image43.png "クリックして一覧から、関連するスニペットを選択")

*クリックして一覧から、関連するスニペットを選択します。*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>付録 b: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express Web タイルを*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>付録 c: インストール WebMatrix 2 と iPhone シミュレーター

IPhone のシミュレートされたデバイスでは、サイトを実行するには、WebMatrix 拡張機能を使用することができます&quot;(iphone用) Electric Mobile シミュレーター&quot;です。 また、シミュレーターを Visual Studio 2012 から実行する同じ拡張機能を構成することができます。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>タスク 1 - インストール WebMatrix 2

1. 移動して[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>WebMatrix 2</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![WebMatrix 2 をインストール](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 のインストール")

    *WebMatrix 2 をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image50.png "ライセンス条項に同意")

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image51.png "インストールの進行状況")

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image52.png "インストールが完了しました")

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>タスク 2 - iPhone シミュレーターの拡張機能をインストールします。

1. 実行**WebMatrix**と既存の Web サイトを開くか、新しいものを作成します。
2. をクリックして、**実行**ボタンから、**ホーム**のリボンし、選択**新規追加**です。

    ![新しい WebMatrix 拡張機能を追加する](whats-new-in-aspnet-mvc-4/_static/image53.png "新しい WebMatrix 拡張機能を追加します。")

    *新しい WebMatrix 拡張機能を追加します。*
3. 選択**iPhone シミュレーター**  をクリック**インストール**です。

    ![WebMatrix 拡張機能の参照](whats-new-in-aspnet-mvc-4/_static/image54.png "参照 WebMatrix 拡張機能")

    *WebMatrix 拡張機能の参照*
4. パッケージの詳細 をクリックして**インストール**拡張機能のインストールを続行します。

    ![iPhone シミュレーター拡張子](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone シミュレーターの拡張機能")

    *iPhone シミュレーターの拡張機能*
5. 読み取りおよび拡張機能の使用許諾契約書に同意します。

    ![WebMatrix 拡張機能の使用許諾契約書](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 拡張機能の使用許諾契約書")

    *WebMatrix 拡張機能の使用許諾契約書*
6. ここで、iPhone シミュレーター オプションを使用して WebMatrix から Web サイトを実行できます。

    ![IPhone を使用して実行](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone を使用して実行")

    *IPhone を使用して実行します。*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>タスク 3 - iPhone シミュレーターを実行する Visual Studio 2012 の構成

1. 開く**Visual Studio 2012**と任意の Web サイトを開くか、新しいプロジェクトを作成します。
2. [実行] ボタンの下矢印をクリックし、選択**参照**です。

    ![参照](whats-new-in-aspnet-mvc-4/_static/image58.png "参照")

    *参照します。*
3. &quot;Browse With&quot;ダイアログ ボックスで、をクリックして**追加**です。
4. &quot;プログラムの追加&quot;ダイアログ ボックスで、次の値を使用します。

   - <strong>プログラム</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (パスを適宜更新)</em>
   - **引数**: &quot;1&quot;
   - **フレンドリ名**: iPhone シミュレーター

     ![プログラムの追加](whats-new-in-aspnet-mvc-4/_static/image59.png "プログラムの追加")

     *使用して参照するプログラムを追加します。*
5. をクリックして**OK**し、ダイアログ ボックスを閉じます。
6. これで、Visual Studio 2012 から iPhone シミュレーターで、Web アプリケーションを実行することができます。

    ![IPhone シミュレーターと参照](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone シミュレーターでの参照")

    *IPhone シミュレーターでの参照します。*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 d: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。

    ![Windows Azure ポータルにログオンする](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure ポータルにログオンします。")

    *Windows Azure 管理ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image62.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image63.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](whats-new-in-aspnet-mvc-4/_static/image64.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](whats-new-in-aspnet-mvc-4/_static/image65.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-aspnet-mvc-4/_static/image66.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](whats-new-in-aspnet-mvc-4/_static/image67.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](whats-new-in-aspnet-mvc-4/_static/image68.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *変更を確認します。*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image73.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-aspnet-mvc-4/_static/image74.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](whats-new-in-aspnet-mvc-4/_static/image75.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

   - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。
   - **ユーザー名**サーバー管理者のログイン名を入力します。
   - **パスワード**サーバー管理者のログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。

     ![対象の接続文字列を構成する](whats-new-in-aspnet-mvc-4/_static/image77.png "対象の接続文字列を構成します。")

     *対象の接続文字列を構成します。*
6. 次に、 **[OK]**をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](whats-new-in-aspnet-mvc-4/_static/image78.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]**をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image80.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

    ![アプリケーションが Windows Azure に発行](whats-new-in-aspnet-mvc-4/_static/image81.png "アプリケーションが Windows Azure に発行")

    *Windows Azure に発行されるアプリケーション*
