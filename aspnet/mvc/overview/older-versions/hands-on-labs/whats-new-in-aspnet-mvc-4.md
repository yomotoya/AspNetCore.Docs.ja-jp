---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 の新機能新機能 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 は、既に確立されている設計パターンと、ASP.NET の機能を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8862c4da0d881a6f1084317e08697354c0ae6d48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374105"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>ASP.NET MVC 4 の新機能新機能

によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 は、既に確立されている設計パターンと、ASP.NET と .NET framework の機能を使用してスケーラブルな標準ベースの web アプリケーションを構築するためのフレームワークです。 この新しいフレームワークの 4 番目のバージョンは、モバイル web アプリケーションの開発を容易に重点を置いています。

最初に、新しい ASP.NET MVC 4 プロジェクトを作成するときに今すぐ、モバイル アプリケーションのプロジェクト テンプレートをモバイル デバイス向けのスタンドアロン アプリのビルドに使用することができます。 さらに、ASP.NET MVC 4 では、jQuery.Mobile.MVC NuGet パッケージからの jQuery Mobile と統合されます。 jQuery Mobile は、これに Windows Phone、iPhone、Android を含む、すべての一般的なモバイル デバイス プラットフォームと互換性のある web アプリを開発するための HTML5 ベースのフレームワーク。 ただし、特殊化する場合は、ASP.NET MVC 4 こともできますを各種デバイス用のさまざまなビューを提供し、デバイス固有の最適化を提供します。

このハンズオン ラボでは、開始、ASP.NET MVC 4 で&quot;インターネット アプリケーション&quot;フォト ギャラリーのアプリケーションを作成するプロジェクト テンプレート。 さまざまなモバイル デバイスとデスクトップの web ブラウザーでの互換性を確保する jQuery Mobile と ASP.NET MVC 4 の新機能を使用してアプリを段階的に強化されます。 コードの生成と ASP.NET MVC 4 簡単方法タスクをサポートすることによって、非同期アクション メソッドを記述するための新しいコード レシピについても学習&lt;ActionResult&gt;型を返します。

> [!NOTE]
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。 このラボに固有のプロジェクトは、「 [ASP.NET 4.5 Web フォームで新](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)します。

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートを機能強化を活用します。
- HTML5 ビューポート属性と CSS メディア クエリを使用して、モバイル デバイスの表示を改善するには
- プログレッシブの機能強化と、タッチに最適化された web UI を構築するために、jQuery Mobile を使用してください。
- モバイル専用ビューを作成します。
- ビュー スイッチャー コンポーネントを使用して、アプリケーションでモバイルおよびデスクトップのビューを切り替える
- タスクのサポートを使用して非同期コント ローラーを作成します。

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。
- [ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 のインストールに付属)
- Windows Phone エミュレーター (に含まれる、 [7.1.1 の Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- 省略可能 - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)で**Electric Plum iPhone シミュレーター**拡張機能 (手順 3 は、iPhone シミュレーターで、web アプリケーションを参照するために使用) の場合のみ

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。 便宜上、そのコードのほとんどは、Visual Studio コード スニペット、手動で追加することを回避するために Visual Studio 内から使用することができますとして提供されます。

コード スニペットをインストールするには

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**Source\Setup**フォルダー。
2. ダブルクリックして、 **Setup.cmd** Visual Studio のコード スニペットをインストールするには、このフォルダー内のファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;します。

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [新しい ASP.NET MVC 4 プロジェクト テンプレート](#Exercise1)
2. [フォト ギャラリーの Web アプリケーションを作成します。](#Exercise2)
3. [モバイル デバイスのサポートを追加します。](#Exercise3)
4. [非同期コント ローラーを使用します。](#Exercise4)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **60 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>手順 1: 新しい ASP.NET MVC 4 プロジェクト テンプレート

この演習では、ASP.NET MVC 4 プロジェクト テンプレートでの機能強化について説明します。 インターネット アプリケーション テンプレートに加えて MVC 3 に既に存在するこのバージョンになりましたモバイル アプリケーションの別のテンプレート。 最初に、各テンプレートの関連する機能の一部になります。 次に、適切なアプローチを使用して、さまざまなプラットフォームでは正常にページのレンダリングの作業します。

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>タスク 1 - インターネット アプリケーションのテンプレートの表示

1. 開いている**Visual Studio**します。
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーション。** プロジェクトに名前を**PhotoGallery**、場所を選択します (または既定値のままに) をクリック**OK**します。

    > [!NOTE]
    > 後で今すぐ作成する ASP.NET MVC 4 の PhotoGallery ソリューションをカスタマイズします。

    ![新しいプロジェクトを作成する](whats-new-in-aspnet-mvc-4/_static/image1.png "新しいプロジェクトを作成します。")

    *新しいプロジェクトを作成します。*
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリックします**OK**。 Razor ビュー エンジンとして選択したことを確認します。

    ![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image2.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")

    *新しい ASP.NET MVC 4 インターネット アプリケーションの作成*

    > [!NOTE]
    > Razor 構文は、ASP.NET MVC 3 で導入されました。 その目的は、文字と、高速とワークフローをコーディング流体を有効にするファイルでは、必要なキーストロークの数を最小限に抑えるです。 Razor を活用して既存の c#/VB (またはその他) の言語スキルと最高の HTML の構築ワークフローを有効にするテンプレート マークアップ構文を提供します。
4. キーを押して**F5**ソリューションを実行し、更新されたテンプレートを参照してください。 次の機能を確認することができます。

    **モダン スタイルのテンプレート**

    テンプレートが更新されました、現代的な外観の他のスタイルを提供します。

    ![ASP.NET MVC 4 のスタイル テンプレート](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 のテンプレートのスタイル")

    *ASP.NET MVC 4 のスタイルのテンプレート*

    ![新しい連絡先ページ](whats-new-in-aspnet-mvc-4/_static/image4.png "新しい連絡先 ページ")

    *新しい連絡先 ページ*

    **アダプティブ レンダリング**

    ブラウザー ウィンドウのサイズを確認し、どのページ レイアウトが動的に、新しいウィンドウ サイズに変化に注目してください。 これらのテンプレートでは、アダプティブ レンダリング手法を使用して、カスタマイズすることがなく、デスクトップとモバイルの両方のプラットフォームで正しく表示します。

    ![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](whats-new-in-aspnet-mvc-4/_static/image5.png "別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート")

    *別のブラウザーのサイズを使用した ASP.NET MVC 4 プロジェクト テンプレート*

    **JavaScript で高度な UI**

    既定のプロジェクト テンプレートを別の拡張機能より対話的な JavaScript を提供する JavaScript の使用です。 テンプレートで使用されるログインと登録のリンクは、jQuery の検証を使用して、クライアント側からの入力フィールドを検証する方法を含めました。

    ![jQuery の検証](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery の検証*

    > [!NOTE]
    > 2 つがする最初のセクションでのセクションではログには、サイトから登録されたアカウントを使用しておよび altenativelly ログオンする (既定で無効になっている) google などの別の認証サービスを使用して 2 番目のセクションを記録できます。
5. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
6. ファイルを開く**AuthConfig.cs**の下にある、**アプリ\_開始**フォルダー。
7. Google クライアントを登録する最後の行のコメントは削除*OAuth*認証します。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Facebook、Twitter、Microsoft などの任意の OpenID または OAuth サービスを使用して認証を簡単に実現できますに注意してください。
8. キーを押して**F5**ソリューションを実行し、ログイン ページに移動します。
9. 選択**Google**サービスにログインします。

    ![サービスでのログの選択](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *サービスでのログの選択*
10. Google アカウントを使用してをログインします。
11. Google アカウントから情報を取得する (localhost)、サイトを許可します。
12. 最後に、Google アカウントに関連付けるサイトに登録する必要があります。

   ![Google アカウントを関連付ける](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Google アカウントを関連付ける*
13. デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。
14. プロジェクト テンプレートの ASP.NET MVC 4 で導入されたいくつかその他の新機能を確認するソリューションを使ってみるようになりました。

   ![ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")

   *ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート*

   - **HTML 5 マークアップ**

       新しいテーマのマークアップを確認するテンプレート ビューを参照します。

       ![Razor、HTML5 マークアップ About.cshtml を使用して新しいテンプレートです。](whats-new-in-aspnet-mvc-4/_static/image10.png "Razor、HTML5 マークアップ About.cshtml を使用して、新しいテンプレート。")

       *Razor、HTML5 マークアップ (About.cshtml) を使用して新しいテンプレート。*
   - **更新された JavaScript ライブラリ**

       これで、ASP.NET MVC 4 の既定のテンプレートには、KnockoutJS、豊富な作成できる JavaScript の MVVM フレームワーク、および JavaScript と HTML を使った応答性に優れた web アプリケーションが含まれます。 ような MVC3 で jQuery と jQuery UI ライブラリも含まれます ASP.NET MVC 4 で。

     > [!NOTE]
     > このリンクに KnockOutJS ライブラリの詳細を表示できます: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/)します。 さらに、jQuery と jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)します。

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>タスク 2 - モバイルのアプリケーション テンプレートの表示

ASP.NET MVC 4 には、モバイル web サイトやタブレットのブラウザーの開発が容易になります。 このテンプレート (コント ローラー コードとほぼ同様ですが、注意)、インターネット アプリケーション テンプレートと同じアプリケーション構造を持つが、タッチ ベースのモバイル デバイスで正しく表示するためにそのスタイルが変更されました。

1. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリーを選び、 **ASP.NET MVC 4 Web アプリケーション。** プロジェクトの名前**PhotoGallery.Mobile**、場所を選択します (または既定値のままに)、選択&quot;ソリューションに追加&quot; をクリック**OK**。
2. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Mobile アプリケーション**プロジェクト テンプレートをクリックします**OK**。 Razor ビュー エンジンとして選択したことを確認します。

    ![新しい ASP.NET MVC 4 モバイル アプリケーションを作成する](whats-new-in-aspnet-mvc-4/_static/image11.png "新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。")

    *新しい ASP.NET MVC 4 モバイル アプリケーションを作成します。*
3. これは、ソリューションを探して、モバイル向け ASP.NET MVC 4 のソリューション テンプレートで導入された新しい機能の一部を確認できます。

    - **jQuery Mobile ライブラリ**

        モバイル アプリケーションのプロジェクト テンプレートには、モバイル ブラウザーの互換性のためのオープン ソース ライブラリは、jQuery Mobile ライブラリが含まれています。 jQuery Mobile では、CSS および JavaScript をサポートするモバイル ブラウザーにプログレッシブ エンハンスメントが適用されます。 プログレッシブ エンハンスメントしか有効に最も強力なブラウザーにリッチ コンテンツを表示中に、web ページの基本コンテンツを表示するすべてのブラウザーを使用できます。 JQuery モバイルのスタイルに含まれる、JavaScript と CSS ファイルには、ページ マークアップでの変更を加えずに、画面で、コンテンツに合わせてモバイル ブラウザーが役立ちます。

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *テンプレートに含まれる jQuery mobile ライブラリ*
    - **HTML5 ベースのマークアップ**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *HTML5 マークアップ、(Login.cshtml および index.cshtml) を使用してモバイル アプリケーション テンプレート*
4. キーを押して**F5**ソリューションを実行します。
5. 開く、 **Windows Phone 7 エミュレーター**します。
6. Phone のスタート画面では、Internet Explorer を開きます。 デスクトップ アプリケーションの開始場所の URL を確認し、携帯電話からその URL に移動 (例: `http://localhost:[PortNumber]/`)。
7. ログイン ページに入力するか、チェック アウトすることができるので、ページについて。 Web サイトのスタイルが mobile 用の新しい Metro アプリに基づいていることに注意してください。 モバイル デバイス、ページのすべての要素が表示され有効になっていることを確認するのには、ASP.NET MVC 4 プロジェクト テンプレートが正しく表示されます。 ヘッダーのリンクがクリックしてまたはタップする対処できるだけの十分なことに注意してください。

    ![プロジェクトのテンプレートのページはモバイル デバイスで](whats-new-in-aspnet-mvc-4/_static/image14.png "テンプレート ページはモバイル デバイスでのプロジェクト")

    *モバイル デバイスでのプロジェクト テンプレートのページ*
8. 新しいテンプレートを使用しても、**ビューポート メタ タグ**します。 ほとんどのモバイル ブラウザーが仮想ブラウザー ウィンドウの幅を定義または&quot;ビューポート&quot;、これは、モバイル デバイスの実際の幅より大きい。 これにより、モバイル ブラウザーに仮想表示内で web ページ全体を表示できます。 **ビューポート メタ タグ**により、web 開発者はモバイル デバイスで、幅、高さ、およびブラウザーの領域のスケールを設定する**します。** モバイル アプリケーション用の ASP.NET MVC 4 テンプレートは、デバイスの幅にビューポートを設定 (&quot;幅 = デバイスの幅&quot;) レイアウト テンプレート (*views \shared\_Layout.cshtml*) できるように、すべて、ページには、デバイスの画面幅に設定、ビューポートがあります。 ビューポートのメタ タグによって既定のブラウザー ビューは変更されないことに注意してください。
9. 開いている **\_Layout.cshtml**にある、**ビュー |共有**フォルダー、およびコメント ビューポート メタ タグ。 アプリケーションを実行されていない場合は既に開かれるとの相違点を確認します。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![ビューポートの meta タグをコメント後サイト](whats-new-in-aspnet-mvc-4/_static/image15.png "ビューポートのメタ タグをコメント化した後、サイト")

*ビューポートの meta タグをコメント化した後、サイト*
10. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。
11. ビューポートの meta タグをコメント解除します。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>タスク 3 - アダプティブ レンダリングを使用します。

このタスクでは、モバイル デバイスと Web ブラウザーを正しく、Web ページをカスタマイズすることがなく同時に表示するために別の方法を説明します。 既に同様の目的でビューポート メタ タグを使用します。 別の強力な方法に変化するようになりました:*アダプティブ レンダリング*します。

アダプティブ レンダリングを使用する手法は、 **CSS3 メディア クエリ**ページに適用されるスタイルをカスタマイズします。 メディア クエリでは、特定の条件下で CSS スタイルをグループ化、スタイル シート内の条件を定義します。 条件が true の場合のみ、スタイルは、宣言されたオブジェクトに適用されます。

アダプティブ レンダリング手法によって提供される柔軟性により、さまざまなデバイスで、サイトを表示するためのカスタマイズができます。 1 つのスタイル シートのロジックのコードを記述することがなく、スタイルを選択、同数のスタイルを定義することができます。 したがって、ページのスタイルに適合させる非常に便利な方法は、コードの重複と目的を表示するためのロジックの量が減ることが。 その一方で、帯域幅の消費量が増加、CSS ファイルのサイズが若干大きくなりすぎるとします。

アダプティブ レンダリング手法を使用して、サイトがなります**ブラウザーに関係なく、適切に表示されます。** ただしに問題が追加の帯域幅を読み込む場合を考慮する必要があります。

> [!NOTE]
> メディア クエリの基本形式です: @media \[スコープ: すべて | ハンドヘルド | 印刷 | プロジェクション | 画面\](プロパティ: 値と.プロパティ: 値)


メディア クエリの例: &gt;  <strong>@mediaすべてと (幅の最大値: 1000 ピクセル) と (幅の最小値: 700 px) {}:</strong> 700 px と 1000 ピクセルの間のすべての解像度。

> <strong>@media 画面と (幅の最小値: 400 px) と (幅の最大値: 700 px) {…}:</strong>画面に対してのみです。 解像度は、400 ~ 700 px にする必要があります。
> 
> <strong>@media ハンドヘルドと (幅の最小値: 20em)、画面と (幅の最小値: 20em) {…}:</strong>ハンドヘルド デバイス (モバイルおよびデバイス) と画面の。 幅の最小値は、20em より大きくする必要があります。
> 
> 詳細についてを見つけることができます、 [W3C サイト](http://www.w3.org/TR/css3-mediaqueries/)します。


アダプティブ レンダリングの動作方法を今すぐ調査、ASP.NET MVC の読みやすさを向上させる 4 既定の web サイト テンプレート。

1. 開く、 **PhotoGallery.sln**選択をタスク 1 で作成したソリューション、 **PhotoGallery**プロジェクト。 キーを押して**F5**ソリューションを実行します。
2. ブラウザーの幅を半分にまたはより小さいの元のサイズの四半期に、windows の設定のサイズを変更します。 ヘッダー内の項目で発生する内容に注意してください。 一部の要素は、ヘッダーの表示領域に表示されません。
3. 開いている<strong>Site.css</strong>ファイルで Visual Studio ソリューション エクスプ ローラーから<strong>コンテンツ</strong>プロジェクト フォルダーです。 キーを押して<strong>CTRL + F</strong>を Visual Studio の統合された検索を開いたり書き込んだりする<strong>@media</strong>を検索する、 <strong>CSS メディア クエリ</strong>します。

    このテンプレートで定義されているメディア クエリの条件は、この方法で機能します。 ときに、ブラウザーのウィンドウのサイズを下回って**850 px**、適用される CSS ルールがこのメディア ブロック内で定義されています。

    ![メディア クエリを検索する](whats-new-in-aspnet-mvc-4/_static/image16.png "メディア クエリを検索します。")

    *メディア クエリを検索します。*
4. 850 に設定された最大幅属性値を置き換えると px **10px**アダプティブのレンダリングを無効にするために、キーを押します**CTRL + S**変更を保存します。 キーを押して、ブラウザーに戻って**ctrl キーを押しながら f5 キーを押して**が変更されたページを更新します。 ウィンドウの幅を調整する場合は、両方のページの違いに注意してください。

    ![ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると](whats-new-in-aspnet-mvc-4/_static/image17.png "左で、ページを適用する、@mediaスタイル、スタイル、右を省略すると")

    <em>ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると</em>

    ここで、モバイル デバイス上の動作確認しましょう。

    ![ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると](whats-new-in-aspnet-mvc-4/_static/image18.png "左で、ページを適用する、@mediaスタイル、スタイル、右を省略すると")

    <em>ページの適用、左側で、@mediaスタイル、スタイル、右を省略すると</em>

    モバイル デバイスを使用する場合、Web ブラウザーで、ページをレンダリングすると、変更は、非常に重要ないないと表示されますが、違いはより明確になります。 イメージの左側にある、カスタムのスタイルが、読みやすさを向上することを確認できます。

    条件付きの Web サイトにスタイル設定、および便利な方法でスタイルの一般的な問題を解決するために適用しやすく、多くの詳細シナリオでは、アダプティブ レンダリングを使用できます。

    ビューポート meta タグと CSS メディア クエリに限定されない ASP.NET MVC 4 では、任意の web アプリケーションでこれらの機能を活用できるようにします。
5. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>手順 2: フォト ギャラリーの Web アプリケーションを作成します。

この演習では、写真を表示する、フォト ギャラリーのアプリケーションで機能します。 ASP.NET MVC 4 プロジェクト テンプレートから開始し、サービスから写真を取得し、ホーム ページに表示するための機能を追加します。

次の演習では、モバイル デバイスでの表示方法を強化するためには、このソリューションを更新します。

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>タスク 1 - 写真のモック サービスを作成します。

このタスクでは、ギャラリーに表示される内容を取得する写真のサービスのモックを作成します。 これを行うには、それぞれの写真データを含む JSON ファイルを返すだけですが、新しいコント ローラーを追加します。

1. 開いている**Visual Studio**開いていない場合。
2. 選択、**ファイル |新しい |プロジェクト**メニュー コマンド。 **新しいプロジェクト**ダイアログ ボックスで、 **(Visual C#) |Web**左側のウィンドウでテンプレート ツリー、および選択**ASP.NET MVC 4 Web アプリケーション。** プロジェクトに名前を**PhotoGallery**、場所を選択します (または既定値のままに) をクリック**OK**します。 または、既存の ASP.NET MVC 4 から作業を継続できます**インターネット アプリケーション**ソリューションから**演習 1**し、次の手順をスキップします。
3. **新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**プロジェクト テンプレートをクリックします**OK**します。 Razor ビュー エンジンとして選択されている必要があることを確認します。
4. **ソリューション エクスプ ローラー**を右クリックし、**アプリ\_データ**フォルダー、プロジェクト、および select の**追加 |既存項目の**します。 参照、 **Source\Assets\App\_データ**このラボのフォルダーを追加し、 **Photos.json**ファイル。
5. 名前の新しいコント ローラーを作成**PhotoController**します。 これを行うを右クリックし、**コント ローラー**フォルダーに移動して**追加**選択**コント ローラー。** コント ローラーの名前でそのまま使用、**空の MVC コント ローラー**テンプレートとクリック**追加**します。

    ![追加、PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController を追加します。")

    *PhotoController を追加します。*
6. 置換、**インデックス**メソッドを次**ギャラリー**アクション、およびプロジェクトに最近追加した JSON ファイルからコンテンツの戻り値。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - ギャラリー アクション*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. キーを押して**F5** 、ソリューションを実行し、写真のモック サービスをテストするには、次の URL を参照する: `http://localhost:[port]/photo/gallery` ([port] の値は、現在のポートが、アプリケーションが起動された場所によって異なります)。 この URL に要求のコンテンツを取得する必要があります、 **Photos.json**ファイル。

    ![写真のモック サービスをテスト](whats-new-in-aspnet-mvc-4/_static/image20.png "モック フォト サービスのテスト")

    *写真のモック サービスのテスト*

実際の実装で使用することが[ASP.NET Web API](../../../../web-api/index.md)フォト ギャラリーのサービスを実装します。 ASP.NET Web API をさまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスを構築するが容易にするフレームワークです。 ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>タスク 2 - フォト ギャラリーを表示します。

このタスクでは、この演習の最初のタスクで作成したモック サービスを使用して、フォト ギャラリーを表示するホーム ページを更新します。 モデル ファイルを追加し、ギャラリーのビューを更新します。

1. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。
2. 作成、**フォト**クラス、**モデル**フォルダー。 これを行うを右クリックし、**モデル**フォルダーで、**追加**クリック**クラス**。 名前に設定し、 **Photo.cs**クリック**追加**します。
3. 次のメンバーを追加、**フォト**クラス。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - 写真モデル*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. **Controllers** フォルダーから **HomeController.cs** ファイルを開きます。
5. 次の using ステートメントを追加します。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - HomeController Using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. 更新プログラム、**インデックス**に使用するアクション**HttpClient**ギャラリーのデータを取得し、使用する、 **JavaScriptSerializer**ビュー モデルに逆シリアル化します。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 の Index アクション*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. 開く、 **Index.cshtml**下にあるファイル、 **views \home**フォルダーと次のコードですべてのコンテンツを置換します。

    このコードは、サービスから取得したすべての写真をループ処理し、順不同のリストに表示されます。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex02 - 写真リスト*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. **ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**フォルダー、プロジェクト、および select の**追加 |既存項目の**します。 参照、 **Source\Assets\Content**このラボのフォルダーを追加し、 **Site.css**ファイル。 その置き換えを確認する必要があります。 ある場合、 **Site.css**ファイルを開く、ファイルを再読み込みも確認する必要があります。
9. ファイル エクスプ ローラーを開き、全体をコピーします**写真**フォルダーの下にある、 **Source\Assets**ソリューション エクスプ ローラーでプロジェクトのルート フォルダーには、このラボのフォルダー。
10. アプリケーションを実行します。 ギャラリーで写真を表示するホーム ページが表示されます。

    ![フォト ギャラリー](whats-new-in-aspnet-mvc-4/_static/image21.png "フォト ギャラリー")

    *フォト ギャラリー*
11. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>手順 3: モバイル デバイスのサポートの追加

ASP.NET MVC 4 でキーの更新プログラムの 1 つは、モバイル開発のためのサポートです。 この演習では、前の演習で作成した PhotoGallery ソリューションを拡張してモバイル アプリケーションの ASP.NET MVC 4 の新機能が学びます。

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>タスク 1 - ASP.NET MVC 4 アプリケーションで jQuery Mobile をインストールします。

1. 開く、**開始**ソリューションがある**ソース/Ex3-MobileSupport/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、**パッケージ マネージャー コンソール**をクリックして、**ツール** &gt; **ライブラリ パッケージ マネージャー** &gt; **パッケージ マネージャーコンソール**メニュー オプション。

    ![NuGet パッケージ マネージャー コンソールを開く](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet パッケージ マネージャー コンソールを開く")

    *NuGet パッケージ マネージャー コンソールを開く*
3. パッケージ マネージャー コンソールをインストールするには、次のコマンドを実行、 **jQuery.Mobile.MVC**パッケージ。

    jQuery Mobile は、タッチに最適化された web UI を構築するためのオープン ソース ライブラリです。 JQuery.Mobile.MVC NuGet パッケージには、ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用するヘルパーが含まれています。

    > [!NOTE]
    > 次のコマンドを実行するには Nuget から jQuery.Mobile.MVC ライブラリがダウンロードするされます。

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    このコマンドは、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。

    - **Views/shared/\_Layout.Mobile.cshtml**: 小さな画面用に最適化された jQuery Mobile ベース レイアウトです。 元のレイアウトが置き換えられ、web サイトでは、モバイル ブラウザーから要求を受信したときに (\_Layout.cshtml) をこのデータセット。
    - ビュー スイッチャー コンポーネント: から成る、 **Views/shared/\_ViewSwitcher.cshtml**部分ビューと**ViewSwitcherController.cs**コント ローラー。 このコンポーネントは、ページのデスクトップ バージョンに切り替えるユーザーを有効にするモバイル ブラウザーでリンクを表示します。  
        ![モバイル デバイスのサポートをフォト ギャラリー プロジェクト](whats-new-in-aspnet-mvc-4/_static/image23.png "フォト ギャラリーのプロジェクトをモバイル デバイスのサポート")

        *フォト ギャラリーのプロジェクトをモバイル デバイスのサポート*
4. モバイル バンドルを登録します。 これを行うには、開く、 **Global.asax.cs**ファイルを開き、次の行を追加します。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex03 - 登録モバイル バンドル*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. デスクトップの web ブラウザーを使用してアプリケーションを実行します。
6. 開く、 **Windows Phone 7 エミュレーター、** 内にある **[スタート] メニュー |すべてのプログラム |Windows Phone SDK 7.1 |Windows Phone エミュレーター。**
7. Phone のスタート画面では、Internet Explorer を開きます。 アプリケーションの開始 URL を確認し、スマート フォンのブラウザーでその URL に移動 (例: `http://localhost:[PortNumber]/`)。

    JQuery.Mobile.MVC がモバイル デバイス用に最適化されたビューを表示するプロジェクトに新しい資産を作成すると、アプリケーションが、Windows Phone エミュレーターでは検索が表示されます。

    電話、デスクトップ ビューに切り替えるリンクが表示の上部にあるメッセージに注意してください。 さらに、  **\_Layout.Mobile.cshtml**がインストールされているパッケージによって作成されたレイアウトには、アプリケーションで別のレイアウトが含まれています。

    > [!NOTE]
    > ここまでは、モバイル ビューに戻るにリンクすることはありません。 これは、以降のバージョンに含まれます。

    ![フォト ギャラリーのホーム ページのモバイル ビュー](whats-new-in-aspnet-mvc-4/_static/image24.png "フォト ギャラリーのホーム ページのモバイル ビュー")

    *フォト ギャラリーのホーム ページのモバイル ビュー*
8. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>タスク 2 - モバイル ビューの作成

このタスクでは、モバイル デバイスでより良い appareance に適応させるコンテンツを含む index ビューのモバイル バージョンを作成します。

1. コピー、 **Views\Home\Index.cshtml**を表示およびコピーを作成、新しいファイルの名前を変更する貼り付けます**Index.Mobile.cshtml**します。
2. 開いている、新しく作成された**Index.Mobile.cshtml**表示し、既存&lt;ul&gt;このコードを持つタグ。 これにより、更新されます。、 &lt;ul&gt;から jQuery モバイルのテーマを使用して jQuery モバイル データの注釈付きタグ。

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > 以下の点に注意してください。
    > 
    > - **データ ロール**属性に設定**listview** listview のスタイルを使用してリストをレンダリングします。
    > 
    > - **データ埋め込み**属性が true に設定が丸い境界線と余白の一覧に表示されます。
    > 
    > - **データ フィルター**属性に設定**true**検索ボックスが生成されます。
    > 
    > プロジェクト ドキュメントに jQuery モバイル規則の詳細を確認できます。 [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. キーを押して**CTRL + S**変更を保存します。
4. 切り替えて、 **Windows Phone エミュレーター**と、サイトを更新します。 上にある新しい検索ボックスと同様に、ギャラリーの一覧の新しいルック アンド フィールに注意してください。 検索ボックスに単語を入力し、(たとえば、 **Tulips**) フォト ギャラリーで検索を開始します。

    ![Listview のスタイルをフィルタ リングを使用してギャラリー](whats-new-in-aspnet-mvc-4/_static/image25.png "listview のスタイルを使用してフィルターを使用したギャラリー")

    *Listview のスタイルを使用してフィルターを使用したギャラリー*

    要約すると、Index ビューのコピーを作成するビューというレシピを使用した、&quot;モバイル&quot;サフィックス。 このサフィックスは ASP.NET MVC 4 モバイル デバイスから生成されたすべての要求がこのインデックスのコピーを使用することを示します。 さらに、jQuery Mobile を使用して、モバイル デバイスでのサイトのルック アンド フィールを強化するために、インデックス ビューのモバイル バージョンに更新されました。
5. Visual Studio に戻り、開いている**Site.Mobile.css**の下にある、**コンテンツ**フォルダー。
6. イメージの右側に表示する写真のタイトルの位置を修正します。 これを行うには、次のコードを追加、 **Site.Mobile.css**ファイル。

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. キーを押して**CTRL + S**変更を保存します。
8. 戻り、 **Windows Phone エミュレーター**と、サイトを更新します。 写真のタイトルが今すぐに位置付けることに注意してください。

    ![イメージの右側に配置されているタイトル](whats-new-in-aspnet-mvc-4/_static/image26.png "イメージの右側に配置されているタイトル")

    *イメージの右側に配置されているタイトル*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>タスク 3 - jQuery Mobile テーマ

すべてのレイアウトとウィジェットの jQuery Mobile では、新しいオブジェクト指向 CSS フレームワークを中心にサイトおよびアプリケーションに統合されたビジュアル デ ザインを完全なテーマを適用できるように設計されています。

jQuery モバイル デバイスの既定テーマにはには、文字が与えられている 5 つの見本が含まれています。 (a、b、c、d、e) のクイック リファレンス。

このタスクで、既定よりも、さまざまなテーマを使用するモバイル レイアウトが更新されます。

1. Visual Studio に切り替えます。
2. 開く、  **\_Layout.Mobile.cshtml**にあるファイル**views \shared**します。
3. 設定データ ロールを持つ div 要素を検索&quot;ページ&quot;し、更新、**データ テーマ**に&quot; **e**&quot;します。

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. キーを押して**CTRL + S**変更を保存します。
5. サイトを更新、 **Windows Phone エミュレーター**に新しい色スキームを注意してください。

    ![別の配色でモバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image27.png "別の配色とモバイルのレイアウト")

    *別の配色とモバイルのレイアウト*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>タスク 4 - ビュー スイッチャー コンポーネントと機能をオーバーライドするブラウザーを使用して

モバイルに最適化された web ページ用の規則では、デスクトップ ビューなど、ページのデスクトップ バージョンに切り替えることができますサイトの完全なモードはテキストがリンクを追加します。 JQuery.Mobile.MVC パッケージには、サンプルが含まれています。**ビュー スイッチャー**この目的で使用されるコンポーネント、  **\_Layout.Mobile.cshtml**ビュー。

![デスクトップ ビューに切り替えるリンク](whats-new-in-aspnet-mvc-4/_static/image28.png "デスクトップ ビューに切り替えますへのリンク")

*デスクトップ ビューに切り替えるリンクします。*

ビュー スイッチャーと呼ばれる新しい機能を使用して**ブラウザー オーバーライド**します。 この機能は、アプリケーションで別のブラウザー (ユーザー エージェント) から実際に送信されているものから、要求を処理できます。

このタスクでは、jQuery.Mobile.MVC と ASP.NET MVC 4 の機能をオーバーライドする、新しいブラウザーによって追加されたビュー スイッチャーの実装例を説明します。

1. Visual Studio に切り替えます。
2. 開く、  **\_Layout.Mobile.cshtml**ビューの下にある、 **views \shared**フォルダーと部分ビューとして参照されているビュー スイッチャー コンポーネントに注意してください。

    ![ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト](whats-new-in-aspnet-mvc-4/_static/image29.png "ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト")

    *ビュー スイッチャーのコンポーネントを使用して、モバイル レイアウト*
3. 開く、  **\_ViewSwitcher.cshtml**部分ビュー。

    部分ビューは、新しいメソッドを使用して**ViewContext.HttpContext.GetOverriddenBrowser()** web 要求の起点を特定して、デスクトップまたはモバイル ビューのいずれかを切り替えるに対応するリンクを表示します。

    **GetOverridenBrowser**メソッドを返します、 **HttpBrowserCapabilitiesBase**要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライド) します。 この値を使用して、プロパティを取得します。 **IsMobileDevice**します。

    ![ViewSwitcher 部分ビュー](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分ビュー")

    *ViewSwitcher 部分ビュー*
4. 開く、 **ViewSwitcherController.cs**クラス、**コント ローラー**フォルダー。 チェック アウトする SwitchView アクションは ViewSwitcher コンポーネントでリンクによって呼び出され、HttpContext の新しいメソッドに注目してください。

    - **HttpContext.ClearOverridenBrowser()** メソッドは、現在の要求でオーバーライドされたユーザー エージェントを削除します。
    - **HttpContext.SetOverridenBrowser()** メソッドは、指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。  
        ![ViewSwitcher コント ローラー](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher コント ローラー")  
*ViewSwitcher コント ローラー*

        ブラウザーのオーバーライドは ASP.NET MVC 4、コア機能も使用可能な場合でも、jQuery.Mobile.MVC パッケージをインストールしないでください。 ただし、この機能は、ビュー、レイアウト、および部分ビュー、のみに影響して、Request.Browser オブジェクトに依存する機能は影響しません。

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>タスク 5 - デスクトップ ビューにビュー切り替えを追加します。

このタスクでデスクトップ レイアウトに含めるビュー スイッチャーが更新されます。 これによって、モバイル ユーザー デスクトップのビューを参照するときに、モバイル ビューに戻ります。

1. サイトを更新、 **Windows Phone エミュレーター**します。
2. をクリックして、**デスクトップ ビュー**ギャラリーの上部にあるリンクです。 モバイル ビューに戻ることを許可するデスクトップ ビューにビュー切り替えがないことを確認します。
3. Visual Studio に戻り、開く、  **\_Layout.cshtml**ビュー。
4. ログイン セクションを見つけて表示するために呼び出しを挿入、  **\_ViewSwitcher**下の部分ビュー、  **\_LogOnPartial**部分ビュー。 次に、キーを押して**CTRL + S**変更を保存します。

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. キーを押して**CTRL + S**変更を保存します。
6. Windows Phone エミュレーターでページを更新し、拡大する画面をダブルクリックします。 ホーム ページが表示されますが、**モバイル ビュー**モバイルからデスクトップ ビューに切り替えるリンク。

    ![デスクトップ ビューに表示の切り替えを表示](whats-new-in-aspnet-mvc-4/_static/image32.png "デスクトップ ビューにレンダリングされるビュー スイッチャー")

    *デスクトップ ビューにレンダリングされるビュー スイッチャー*
7. もう一度モバイル ビューに切り替えるしを参照<strong>に関する</strong>ページ (http://localhost[port]/Home/About)。 、、About.Mobile.cshtml ビューを作成していない場合でも、[About] ページが表示されます、モバイル レイアウトを使用して (\_Layout.Mobile.cshtml)。

    ![ページについて](whats-new-in-aspnet-mvc-4/_static/image33.png "ページについて")

    *ページについて*
8. 最後に、デスクトップ、Web ブラウザーでサイトを開きます。 デスクトップ ビューに影響なし以前の更新プログラムのことに注意してください。

    ![PhotoGallery デスクトップ ビュー](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery デスクトップ ビュー")

    *PhotoGallery デスクトップ ビュー*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>タスク 6 - 新しい表示モードの作成

新しい表示モード機能には、要求を生成しているブラウザーによってビューを選択して、アプリケーションことができます。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返されます、 **Views\Home\Index.cshtml**テンプレート。 モバイル ブラウザーでは、ホーム ページを要求している場合、アプリケーションが返されます、 **Views\Home\Index.mobile.cshtml**テンプレート。

このタスクでは iPhone デバイスでは、カスタマイズされたレイアウトを作成して iPhone デバイスからの要求をシミュレートする必要があります。 これを行うには、いずれかを iPhone エミュレーターまたはシミュレーターを使用することができます (など[電気 Mobile シミュレーター](http://www.electricplum.com/)) またはユーザー エージェントを変更するアドオンを使用したブラウザー。 IPhone をエミュレートするために、Safari ブラウザーでユーザー エージェント文字列を設定する方法については、次を参照してください。 [Safari pretend IE ができるようにする方法](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison のブログにします。

**このタスクは省略可能なことと、実行せず、ラボで続行することができますに注意してください。**

1. Visual Studio で、キーを押します**SHIFT** + **f5 キーを押して**アプリケーションのデバッグを停止します。
2. 開いている**Global.asax.cs**し、以下の追加ステートメントを使用します。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. 次の強調表示されたコードをアプリケーションに追加\_メソッドを開始します。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

新しい登録が**DefaultDisplayMode という&quot;iPhone&quot;**、静的な内で**DisplayModeProvider.Instance.Modes**静的リストに対して照合されます。各受信要求。 受信要求には、文字列が含まれている場合&quot;iPhone&quot;、ASP.NET MVC は名前を含むビューを検索、 &quot;iPhone&quot;サフィックス。 パラメーターに 0 を特定する方法は、新しいモードを示しますたとえば、このビューは、一般的なよりも具体的&quot;.mobile&quot;モバイル デバイスからの要求と一致するルール。

このコードの実行、iPhone ブラウザーが要求を生成するときに、後に、アプリケーションで使用、 **views \shared\\_Layout.iPhone.cshtml**レイアウトは次の手順で作成されます。

> [!NOTE]
> この方法で iPhone のデモの目的で簡略化された、(テストの例では大文字小文字を区別) のすべての iPhone ユーザー エージェント文字列について想定どおり動作しない可能性があります、要求をテストします。

4. コピーを作成、  **\_Layout.Mobile.cshtml**ファイル、 **views \shared**フォルダーへのコピーの名前を変更および&quot;  **\_Layout.iPhone.csthml**&quot;.
5. 開いている **\_Layout.iPhone.csthml**前の手順で作成しました。
6. データ ロールの属性を設定すると div 要素を検索**ページ**を変更して、**データ テーマ**属性を&quot; **、**&quot;します。


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

これで、ASP.NET MVC 4 アプリケーションで 3 つのレイアウトがあります。

1. **\_Layout.cshtml**: 既定のレイアウトがデスクトップ ブラウザーを使用します。
2. **\_Layout.mobile.cshtml**: モバイル デバイスを使用する既定のレイアウト。
3. **\_Layout.iPhone.cshtml**: から区別するために別の配色を使用して、iPhone デバイス用の特定のレイアウト\_Layout.mobile.cshtml します。
7. キーを押して**F5**アプリケーションを実行し、サイトを参照する、 **Windows Phone エミュレーター**します。
8. 開く、 **iPhone シミュレーター** (を参照してください[付録 C](#AppendixC)インストールして、iPhone シミュレーターを構成する方法の詳細について)、しすぎて、サイトを参照します。 各電話番号が、特定のテンプレートを使用することに注意してください。

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *各モバイル デバイスのさまざまなビューを使用します。*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>手順 4: 非同期コント ローラーを使用します。

Microsoft .NET Framework 4.5 では、新しい基盤の .NET プログラミングでの非同期性を提供する c# および Visual Basic の新しい言語機能について説明します。 この新しい基盤は、-のようなと約同期プログラミングと同程度に簡単に非同期プログラミングになります。 使用して ASP.NET MVC 4 での非同期アクション メソッドを記述することができるよう、 **AsyncController**クラス。 実行時間の長い非同期アクション メソッドを使用する、非 CPU バインド要求です。 要求の処理中に作業を実行してから、Web サーバーのブロックを回避できます。 AsyncController クラスは、通常の実行時間の長い Web サービス呼び出しに使用されます。

この演習では、ASP.NET MVC 4 での非同期操作の基本について説明します。 詳細を紹介する場合は、次の記事を確認できます。 [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>タスク 1 - 非同期コント ローラーを実装します。

1. 開く、**開始**ソリューションがある**ソース/Ex4-非同期/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. 開く、 **HomeController.cs**クラスから、**コント ローラー**フォルダー。
3. 次の追加ステートメントを使用します。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. 更新プログラム、 **HomeController**クラスから継承する**AsyncController**します。 AsyncController から派生したコント ローラーが非同期の要求を処理する ASP.NET を有効にして、サービス同期アクション メソッドは、できます。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. 追加、 **async**キーワードを**インデックス**メソッドと型を返すように**タスク&lt;ActionResult&gt;** します。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **Async**キーワードは、.NET Framework 4.5 は、新しいキーワードの 1 つは、このメソッドが非同期コードを含むことをコンパイラに伝えます。 A**タスク**オブジェクトは、ある時点で、今後が完了する非同期操作を表します。
6. 置換、**クライアント。GetAsync()** 次に示す await キーワードを使用して、完全な非同期バージョンの呼び出し。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > 以前のバージョンで使用していた、**結果**プロパティから、**タスク**結果には、(同期バージョン) が返されるまでスレッドをブロックするオブジェクト。
    > 
    > 追加、 **await**キーワード、メソッドの呼び出しから返されるタスクを非同期に待機コンパイラに指示します。 これは待機中のメソッドが完了した後にのみ、コードの残りの部分は、コールバックとして実行されることを意味します。 もう 1 つ気付くはこの作業を行うためには、try-catch ブロックを変更する必要はありません: フォア グラウンドまたはバック グラウンドで発生する例外は、フレームワークによって提供されるハンドラーを使用して、追加作業なし引き続きキャッチできます。
7. 次に示すように、新しいコードで行を置き換えることで、非同期の実装を続行するコードを変更します。

    (コード スニペット - *ASP.NET MVC 4 ラボ - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. アプリケーションを実行します。 わかりますなし、主要な変更が、コードでは、サーバー リソースの使用率の向上を行うと、パフォーマンスの向上、スレッド プールのスレッドはブロックされません。

    > [!NOTE]
    > ラボで新しい非同期プログラミング機能の詳細を確認できます&quot; **c# および Visual Basic を使用して .NET 4.5 での非同期プログラミング**&quot; Visual Studio のトレーニング キットに含まれています。

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>タスク 2 - キャンセル トークンを使用した処理のタイムアウト

タスクのインスタンスを返す非同期アクション メソッドには、タイムアウトもサポートできます。 このタスクでは、キャンセル トークンを使用してタイムアウトのシナリオを処理するためにインデックス メソッドのコードを更新します。

1. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。
2. 以下を追加するステートメントを使用して、 **HomeController.cs**ファイル。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. 更新を受信するインデックス アクション、 **CancellationToken**引数。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. 更新プログラム、 **GetAsync**呼び出しキャンセル トークンを渡す。

    (コード スニペット - *CancellationToken を ASP.NET MVC 4 ラボ - Ex04 - SendAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. 装飾、*インデックス*メソッドを**AsyncTimeout**属性が 500 ミリ秒に設定し、 **HandleError**処理するために構成されている属性**TaskCanceledException**リダイレクトすることで、 **TimedOut**ビュー。

    (コード スニペット -*ラボ - Ex04 - 属性の ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. 開く、 **PhotoController**クラスと更新プログラム、**ギャラリー**実行時間の長いタスクをシミュレートするために実行 1000 ミリ秒 (1 秒) を遅延するメソッド。

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. 開く、 **Web.config**ファイルを開き、次の要素を追加してカスタム エラーを有効にします。

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. 新しいビューを作成**views \shared**という**TimedOut**および既定のレイアウトを使用します。 ソリューション エクスプ ローラーで右クリックし、 **views \shared**フォルダーと選択**追加 |ビュー**します。

    ![各モバイル デバイスのさまざまなビューを使用して](whats-new-in-aspnet-mvc-4/_static/image36.png "各モバイル デバイスのさまざまなビューを使用します。")

    *各モバイル デバイスのさまざまなビューを使用します。*
9. 更新プログラム、 **TimedOut**次に示すようにコンテンツを表示します。

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. アプリケーションを実行し、ルート URL に移動します。 追加したときに、 **Thread.Sleep** 1000 ミリ秒間に、によって生成された、タイムアウト エラーが表示されます、 **AsyncTimeout**属性し catch、 **HandleError**属性。

    ![処理のタイムアウト例外](whats-new-in-aspnet-mvc-4/_static/image37.png "処理のタイムアウト例外")

    *処理のタイムアウト例外*

> [!NOTE]
> また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 d: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixD)します。


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボでを見いくつかの新機能の ASP.NET MVC 4 に常駐しています。 次の概念を説明しています。

- ASP.NET MVC プロジェクト テンプレートなど、新しいモバイル アプリケーション プロジェクト テンプレートを機能強化を活用します。
- HTML5 ビューポート属性と CSS メディア クエリを使用して、モバイル デバイスの表示を改善するには
- プログレッシブの機能強化と、タッチに最適化された web UI を構築するために、jQuery Mobile を使用してください。
- モバイル専用ビューを作成します。
- ビュー スイッチャー コンポーネントを使用して、アプリケーションでモバイルおよびデスクトップのビューを切り替える
- タスクのサポートを使用して非同期コント ローラーを作成します。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>付録 a: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-aspnet-mvc-4/_static/image38.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

***キーボード (c# のみ) を使用するコード スニペットを追加するには***

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

![スニペットの名前の入力を開始](whats-new-in-aspnet-mvc-4/_static/image39.png "スニペット名の入力を開始")

*スニペットの名前の入力を開始します。*

![強調表示されているスニペットを選択して Tab キーを押して](whats-new-in-aspnet-mvc-4/_static/image40.png "キーを押してタブが強調表示されているスニペットを選択するには")

*Tab キーを押して、強調表示されているスニペットを選択します*

![キーを押して タブで再度とスニペットが展開](whats-new-in-aspnet-mvc-4/_static/image41.png "キーを押して タブで再度とスニペットが展開されます")

*キーを押して タブで再度とスニペットが展開されます。*

***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加するには***

1. コード スニペットを挿入するを右クリックします。
2. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
3. クリックして、一覧から関連するスニペットを選択します。

![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](whats-new-in-aspnet-mvc-4/_static/image42.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

*コード スニペットを挿入して、スニペットの挿入先の選択します。*

![クリックして、一覧から関連するスニペットを選択](whats-new-in-aspnet-mvc-4/_static/image43.png "クリックして、一覧から関連するスニペットを選択")

*クリックして、一覧から関連するスニペットを選択します。*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>付録 b: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web のタイル*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>付録 c: Installing WebMatrix 2 および iPhone シミュレーター

IPhone のシミュレートされたデバイスで、サイトを実行するには、WebMatrix の拡張機能を使用することができます&quot;iPhone 用の電気の Mobile シミュレーター&quot;します。 また、Visual Studio 2012 から、シミュレーターを実行する同じ拡張機能を構成できます。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>タスク 1 - 2 WebMatrix のインストール

1. 移動して[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>WebMatrix 2</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![WebMatrix 2 のインストール](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 のインストール")

    *WebMatrix 2 をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-mvc-4/_static/image50.png "ライセンス条項に同意")

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-mvc-4/_static/image51.png "インストールの進行状況")

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了した](whats-new-in-aspnet-mvc-4/_static/image52.png "インストールが完了しました")

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>タスク 2 - iPhone シミュレーターの拡張機能をインストールします。

1. 実行**WebMatrix**と既存の Web サイトを開くか、新しく作成します。
2. をクリックして、**実行**からボタン、**ホーム**のリボンし、選択**新規追加**。

    ![WebMatrix の新しい拡張機能の追加](whats-new-in-aspnet-mvc-4/_static/image53.png "WebMatrix の新しい拡張機能の追加")

    *WebMatrix の新しい拡張機能の追加*
3. 選択**iPhone シミュレーター**クリック**インストール**します。

    ![WebMatrix 拡張機能の参照](whats-new-in-aspnet-mvc-4/_static/image54.png "参照 WebMatrix 拡張機能")

    *WebMatrix 拡張機能の参照*
4. パッケージの詳細で次のようにクリックします。**インストール**拡張機能のインストールを続行します。

    ![iPhone シミュレーター拡張子](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone シミュレーターの拡張機能")

    *iPhone シミュレーターの拡張機能*
5. 読み、拡張機能の使用許諾契約書に同意します。

    ![WebMatrix 拡張機能の使用許諾契約書](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 拡張機能の使用許諾契約書")

    *WebMatrix 拡張機能の使用許諾契約書*
6. ここで、iPhone シミュレーター オプションを使用して WebMatrix から Web サイトを実行できます。

    ![IPhone を使用して実行](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone を使用して実行")

    *IPhone を使用して実行します。*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>タスク 3 - iPhone シミュレーターを実行する Visual Studio 2012 の構成

1. 開く**Visual Studio 2012**と任意の Web サイトを開くか、新しいプロジェクトを作成します。
2. [実行] ボタンの下矢印をクリックして選択します**参照**します。

    ![参照](whats-new-in-aspnet-mvc-4/_static/image58.png "と参照")

    *[参照] します。*
3. &quot;ブラウザー&quot;ダイアログ ボックスで、をクリックして**追加**します。
4. &quot;プログラムの追加&quot;ダイアログ ボックスで、次の値を使用します。

   - <strong>プログラム</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (パスを適宜更新する)</em>
   - **引数**: &quot;1&quot;
   - **フレンドリ名**: iPhone シミュレーター

     ![プログラムの追加](whats-new-in-aspnet-mvc-4/_static/image59.png "プログラムの追加")

     *使用して参照するプログラムを追加します。*
5. クリックして**OK**し、ダイアログ ボックスを閉じます。
6. Visual Studio 2012 から iPhone シミュレーターで、Web アプリケーションを実行できるように整いました。

    ![IPhone シミュレーターで参照](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone シミュレーターでの参照")

    *IPhone シミュレーターでの参照します。*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 d: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。

    ![Windows Azure ポータルにログオン](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure ポータルにログオン")

    *Windows Azure 管理ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image62.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。 簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-mvc-4/_static/image63.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](whats-new-in-aspnet-mvc-4/_static/image64.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](whats-new-in-aspnet-mvc-4/_static/image65.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-aspnet-mvc-4/_static/image66.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。

    ![発行プロファイルのダウンロード web サイト](whats-new-in-aspnet-mvc-4/_static/image67.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](whats-new-in-aspnet-mvc-4/_static/image68.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *変更を確認します。*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image73.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-aspnet-mvc-4/_static/image74.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](whats-new-in-aspnet-mvc-4/_static/image75.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。

     ![ターゲットの接続文字列を構成する](whats-new-in-aspnet-mvc-4/_static/image77.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](whats-new-in-aspnet-mvc-4/_static/image78.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](whats-new-in-aspnet-mvc-4/_static/image80.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

    ![Windows Azure にアプリケーションが発行される](whats-new-in-aspnet-mvc-4/_static/image81.png "アプリケーションは Windows Azure に発行")

    *Windows Azure に発行されたアプリケーション*
