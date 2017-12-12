---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: "プロジェクトを作成 |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 2678342891a87d591476a07e418c118b2ae94d4d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-the-project"></a>プロジェクトの作成
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでが作成、および確認するには、Visual Studio は、ASP.NET の機能を理解することがで既定のプロジェクトを実行します。 また、Visual Studio 環境を確認します。

## <a name="what-youll-learn"></a>学習する内容。

- 新しい Web フォーム プロジェクトを作成する方法。
- Web フォーム プロジェクトのファイルの構造体。
- Visual Studio でプロジェクトを実行する方法。
- 既定の Web フォーム アプリケーションのさまざまな機能です。
- Visual Studio 環境の使用方法に関するいくつかの基本。

## <a name="creating-the-project"></a>プロジェクトの作成

1. Visual Studio を開きます。
2. 選択**新しいプロジェクト**から、**ファイル**Visual Studio のメニュー。 

    ![プロジェクトの新しいプロジェクト メニュー項目を作成します。](create-the-project/_static/image1.png)
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。
4. 選択、 **ASP.NET Web アプリケーション**中央の列内のテンプレートです。  
 この一連のチュートリアルについては、.NET Framework 4.5.2 を使用しています。
5. プロジェクトの名前を付けます*WingtipToys*を選択し、 **[ok]**ボタンをクリックします。 

    ![プロジェクトの新しいプロジェクト ダイアログを作成します。](create-the-project/_static/image2.png)

    > [!NOTE]
    > このチュートリアルの一連のプロジェクトの名前は**WingtipToys**です。 これを使用することをお勧め*正確な*プロジェクト名をコードでは、予想通りに機能を一連のチュートリアル全体に提供できるようにします。
6. 次に、選択、 **Web フォーム**テンプレートを選択し、**プロジェクトの作成**ボタンをクリックします。  

    ![プロジェクトの新しいプロジェクト テンプレートを作成します。](create-the-project/_static/image3.png)

プロジェクトを作成するには少し時間となります。 その準備ができたら、開く、 **Default.aspx**ページ。

![プロジェクトの新しいプロジェクト テンプレートを作成します。](create-the-project/_static/image4.png)

間で切り替えることができます**デザイン**ビューと**ソース**センター ウィンドウの下部にあるオプションを選択して表示します。 **デザイン**ビューは、ASP.NET Web ページ、マスター ページ、コンテンツ ページ、HTML ページが表示されます。 および、WYSIWYG に近いビューを使用してユーザーを制御します。 **ソース**ビューを編集できる Web ページの HTML マークアップが表示されます。

> [!TIP] 
> 
> **ASP.NET フレームワークの理解**
> 
> ASP.NET Web フォームでは、使い慣れたドラッグ アンド ドロップ、イベント ドリブン モデルを使用して動的な web サイトをビルドできます。 デザイン サーフェスや数百のコントロールとコンポーネントを洗練された強力な UI 駆動型サイトとデータ アクセスを迅速に構築できます。 Wingtip Toy ストアが ASP.NET Web フォーム ベースが、このチュートリアルのシリーズに学習する概念の多くは、ASP.NET のすべてに適用します。
> 
> ASP.NET には、次の 4 つの主要な開発フレームワークが用意されています。
> 
> - [ASP.NET Web フォーム](../../../index.md)  
>  Web フォームのフレームワークでは、Microsoft Windows フォーム (WinForms) や WPF/XAML/Silverlight などの宣言型およびコントロール ベースのプログラミングを使用する開発者を対象します。 Web 開発用のアプリケーションの迅速な development (RAD) 環境を探している開発者でよく使用されるので、WYSIWYG デザイナー駆動型開発モデルを提供します。 Web プログラミングの経験がない (たとえば、Visual Basic および Visual C# の場合)、従来の Microsoft の RAD クライアント開発ツールに精通していて、HTML および JavaScript でのエクスペリエンスを持たない web アプリケーションをすばやく作成できます。
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC では、パターン、およびテスト駆動開発、関心の分離、制御の反転 (IoC) や依存性の注入 (DI) のような原則に関心がある開発者を対象します。 このフレームワークは、そのプレゼンテーション層から web アプリケーションのビジネス ロジック層を分離することをお勧めします。
> - [ASP.NET Web ページ](../../../../web-pages/index.md)  
>  ASP.NET Web ページは、PHP の線に沿った、単純な web 開発のストーリーを開発者を対象します。 モデルでは、Web ページ、HTML ページを作成し、ページに動的にそのマークアップを表示する方法を制御するためにサーバー ベースのコードを追加します。 Web ページが具体的には、軽量のフレームワークに設計されていますの HTML がない幅広いプログラミングの経験 - たとえば人、受講者またはアマチュア ASP.NET への最も簡単なエントリ ポイント。 ASP.NET の使用を開始するには、PHP またはのようなフレームワークを把握している web 開発者のことをお勧めです。
> - [ASP.NET の単一ページ アプリケーション](../../../../single-page-application/index.md)  
>  ASP.NET の単一ページ アプリケーション (SPA) を使用して、HTML 5、CSS 3、JavaScript を使用して重要なのクライアント側の相互作用を含むアプリケーションを構築できます。 ASP.NET および Web ツール 2012.2 更新 knockout.js と ASP.NET Web API を使用する単一ページ アプリケーションを構築するための新しいテンプレートに同梱されています。 新しい SPA テンプレート以外にも、新しいコミュニティによって作成された SPA テンプレートは、ダウンロード可能なもします。
> 
> ASP.NET には、次の 4 つの主な開発フレームワークに加えての対応であり、使い慣れたに重要ですが、このチュートリアルのシリーズに収録されていないその他のテクノロジも用意されています。
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達する HTTP サービスを構築するためのフレームワークです。
> - [ASP.NET SignalR](../../../../signalr/index.md) -するライブラリ リアルタイム web 機能の開発が容易です。


### <a name="reviewing-the-project"></a>プロジェクトを確認します。

Visual Studio で、**ソリューション エクスプ ローラー**ウィンドウを使用して、プロジェクトのファイルを管理できます。 アプリケーションに追加されているフォルダーを見てみましょう**ソリューション エクスプ ローラー**です。 Web アプリケーション テンプレートは、基本的なフォルダー構造を追加します。

![ソリューション エクスプ ローラーでプロジェクトを作成します。](create-the-project/_static/image5.png)

Visual Studio では、いくつかの初期のフォルダーおよびファイルのプロジェクトを作成します。 このチュートリアルの後半で操作する最初のファイルは次のとおりです。

| **ファイル** | **目的** |
| --- | --- |
| *Default.aspx* | 通常、最初のページをブラウザーで、アプリケーションの実行時に表示されます。 |
| *Site.Master* | アプリケーションのページの一貫したレイアウトと使用標準動作を作成できるようにするページです。 |
| *Global.asax* | ASP.NET または HTTP モジュールで発生したアプリケーション レベル、およびセッション レベルのイベントに応答するコードを含む省略可能なファイルです。 |
| *Web.config* | アプリケーションの構成データ。 |

### <a name="running-the-default-web-application"></a>既定の Web アプリケーションを実行しています。

既定の Web アプリケーションでは、組み込みの機能とサポートに基づくリッチ エクスペリエンスを提供します。 既定の Web フォーム プロジェクトに変更を加えず、アプリケーションは、ローカルの Web ブラウザーで実行する準備ができてです。

1. キーを押して、 ***f5 キーを押して***Visual Studio での中にキー。   
 アプリケーションはビルドして、Web ブラウザーに表示します。  

    ![プロジェクトの作成 - 既定のページ](create-the-project/_static/image6.png)
2. 完了したレビュー実行中のアプリケーションは、ブラウザー ウィンドウを閉じます。

この既定の Web アプリケーションでは次の 3 つのメイン ページが: *Default.aspx* (ホーム) *About.aspx*、および*Contact.aspx*です。 各ページは、上部のナビゲーション バーからアクセスできます。 アカウントのフォルダーに格納されている 2 つのページ、Register.aspx ページおよび Login.aspx ページもあります。 これら 2 つのページを使用すると、ASP.NET のメンバーシップ機能を使用して、作成、格納、およびユーザーの資格情報を検証できます。

## <a name="aspnet-web-forms-background"></a>ASP.NET Web フォームの背景

ASP.NET Web フォームは、サーバーで動的に実行されるコードに、ブラウザーまたはクライアント デバイスに Web ページの出力が生成されます、Microsoft ASP.NET テクノロジに基づいたページです。 ASP.NET Web フォーム ページに自動的に、正しいブラウザーに準拠していませんに HTML の表示などのスタイル、レイアウト、およびなどの機能です。 Web フォームは、.NET 共通言語ランタイム、Microsoft Visual Basic や Microsoft Visual c# でサポートされている任意の言語との互換性。 また、Web フォームは、上に構築された、 [Microsoft .NET Framework](https://msdn.microsoft.com/en-US/vstudio/aa496123)、管理された環境、タイプ セーフ、および継承などの利点も提供します。

ASP.NET Web フォーム ページを実行すると、ページ一連の処理手順を実行するライフ サイクルを通過します。 これらの手順には、初期化、コントロールをインスタンス化する、復元、および状態を維持するには、イベント ハンドラーのコードを実行して、表示が含まれます。 理解するための重要な ASP.NET Web フォームの機能に慣れているにつれて、 [ASP.NET ページのライフ サイクル](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx)する効果のライフ サイクルの適切な段階でコードを作成できるようにします。

Web サーバーは、ページの要求を受信したときに、ページ、処理、ブラウザーに送信して検出し、すべてのページの情報を破棄します。 ユーザーは、同じページをもう一度要求している場合、サーバーには、全体のシーケンスを最初からページを再処理が繰り返されます。 別の言い方を行うには、サーバーを持たない処理されたページを使用しているページのメモリはステートレスです。 ASP.NET ページ フレームワークが自動的に、ページと、そのコントロールの状態を維持するためのタスクを処理し、アプリケーション固有の情報の状態を維持するために明示的な方法では、します。

> [!TIP] 
> 
> **Web アプリケーションの機能を Web フォーム アプリケーション テンプレート**
> 
> ASP.NET Web フォーム アプリケーション テンプレートは、組み込みの機能の豊富なセットを提供します。 それだけでなくを提供しています、 *Home.aspx*  ページで、 *About.aspx*  ページで、 *Contact.aspx*  ページが、ユーザーを登録し、保存するメンバーシップ機能もあります自分の資格情報を web サイトにログインできるようにします。 この概要では、いくつかの ASP.NET Web フォーム アプリケーション テンプレートと Wingtip Toys のアプリケーションでの使い方に含まれている機能について詳しく説明します。
> 
> **メンバーシップ**
> 
> [ASP.NET](https://msdn.microsoft.com/en-us/library/yh26yfzy.aspx) Identity は、アプリケーションによって作成されたデータベースでユーザーの資格情報を格納します。 ユーザーがログイン時に、アプリケーションは、データベースの読み取りで自分の資格情報を検証します。 プロジェクトの*アカウント*フォルダーには、メンバーシップのさまざまな部分を実装するファイルが含まれています。 登録、でのログ記録、、パスワードの変更、およびアクセスの承認。 さらに、ASP.NET Web フォームには、OAuth および OpenID がサポートしています。 これらの認証拡張機能では、このようなアカウントとして、Facebook、Twitter、Windows Live、および Google からの既存の資格情報を使用して、サイトにログインするようにします。
> 
> ![プロジェクトのソリューション エクスプ ローラー (ASP.NET Identity) を作成します。](create-the-project/_static/image7.png)
> 
> 既定では、テンプレートは SQL Server Express LocalDB は、Visual Studio Express 2013 for Web に付属している開発データベース サーバーのインスタンスの既定のデータベース名を使用してメンバーシップ データベースを作成します。
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx)を SQL Server データベースのさまざまなプログラミング機能を持つ SQL Server の軽量バージョンです。 SQL Server Express LocalDB は、ユーザー モードで実行され、インストールの前提条件の短い一覧を含む、構成不要の高速インストールします。 Microsoft SQL Server、任意のデータベースまたは TRANSACT-SQL コードに移動できます SQL Server Express LocalDB から SQL Server および SQL Azure へアップグレード手順なし。 そのため、SQL Server Express LocalDB は、SQL Server のすべてのエディションを対象とするアプリケーションの開発環境として使用できます。 SQL Server Express LocalDB は、SQL Server Compact では使用できないストアド プロシージャ、ユーザー定義関数と集計、.NET Framework の統合、空間型および他のユーザーなどの機能を有効します。
> 
> **マスター ページ**
> 
> [ASP.NET マスター ページ](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)アプリケーションに一貫した外観とすべてのページの動作を定義します。 マスター ページのレイアウトは、ユーザーに表示される最後のページを生成するために個々 のコンテンツ ページの内容でマージします。 Wingtip Toys アプリケーションで変更して、 *Site.master*マスター ページを Wingtip Toys の web サイトのすべてのページが同じ印象的なロゴとナビゲーション バーを共有できるようにします。
> 
> **HTML5**
> 
> ASP.NET Web フォーム アプリケーション テンプレートをサポートしている[HTML5](http://www.w3schools.com/html/html5_intro.asp)、これは、HTML マークアップ言語の最新バージョンです。 HTML5 では、新しい要素と容易にする Web サイトを作成する機能をサポートします。
> 
> **Modernizr**
> 
> HTML5 をサポートしていないブラウザーでは、使用することができます[Modernizr](http://www.modernizr.com/)です。 Modernizr は、ブラウザーが HTML5 機能をサポートして、そうでない場合は有効にするかどうかを検出できるオープン ソース JavaScript ライブラリです。 ASP.NET Web フォーム アプリケーション テンプレートに Modernizr は NuGet パッケージとしてインストールされます。
> 
> **ブートス トラップ**
> 
> Visual Studio 2013 プロジェクト テンプレートを使用して[ブートス トラップ](http://getbootstrap.com/)レイアウトとテーマのフレームワークが Twitter で作成します。 ブートス トラップでは、CSS3 を使用して、レイアウトは、別のブラウザー ウィンドウのサイズに動的に対応できることを意味するレスポンシブ デザインを提供します。 ブートス トラップのテーマの機能は、アプリケーションの外観の変更を簡単に影響するを使用することもできます。 既定では、Visual Studio 2013 で ASP.NET Web アプリケーション テンプレートには、NuGet パッケージとしてブートス トラップが含まれます。
> 
> **NuGet パッケージの管理**
> 
> ASP.NET Web フォーム アプリケーション テンプレートにはセットが含まれています[NuGet](http://www.nuget.org/)パッケージです。 これらのパッケージは、オープン ソース ライブラリとツールの形式でコンポーネント化された機能を提供します。 パッケージを作成し、アプリケーションをテストするためのさまざまながあります。 Visual Studio では簡単に追加、削除、および NuGet パッケージを更新します。 開発者は、作成し、パッケージを NuGet にも追加できます。
> 
> ![プロジェクトの NuGet のダイアログ ボックスを作成します。](create-the-project/_static/image8.png)
> 
> パッケージをインストールするときに NuGet はソリューションにファイルをコピーし、自動的になり、参照を追加して、Web アプリケーションに関連付けられている構成を変更するなど、どのような変更が必要なします。 ライブラリを削除する場合は、NuGet はファイルを削除し、どのような変更が、プロジェクトの煩雑さが残らないように元に戻します。 NuGet はから利用可能な**ツール**Visual Studio のメニュー。
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/)高速かつ簡潔な JavaScript ライブラリを HTML ドキュメントを通過する、イベント処理、アニメーション、および迅速な web 開発用の Ajax の相互作用を簡略化します。 JQuery JavaScript ライブラリは、NuGet パッケージとして、ASP.NET Web フォーム アプリケーション テンプレートに含まれます。
> 
> **控えめな検証**
> 
> クライアント側の検証ロジックの控えめな JavaScript を使用するには、組み込みの検証コントロールを構成されています。 大幅に量が減り、JavaScript、ページ マークアップ内にインライン表示し、全体的なページ サイズを削減します。 控えめな検証グローバル テンプレートに追加した、ASP.NET Web フォーム アプリケーションの設定に基づいて、 &lt;appSettings&gt;の要素、 *Web.config*アプリケーションのルートにあるファイルです。
> 
> **Entity Framework Code First**
> 
> Wingtip Toys アプリケーションを使用して、ASP.NET Web フォーム アプリケーション テンプレートの機能だけでなく[Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx)、これはデータを操作する場合は、コード中心の開発を有効にする NuGet ライブラリです。 簡単に言うと、作成したコードに基づいて、アプリケーションのデータベースの部分を作成します。 Entity Framework を使用して取得し、厳密に型指定されたオブジェクトとしてデータを操作します。 これによりデータへのアクセス方法の詳細ではなく、アプリケーションでビジネス ロジックに集中できます。
> 
> 詳細については、インストールされているライブラリと ASP.NET Web フォーム テンプレートに含まれるパッケージは、インストールされている NuGet パッケージの一覧を参照してください。 これを行うには、Visual Studio でプロジェクトを作成、新しい Web フォーム**ツール** - &gt; **ライブラリ パッケージ マネージャー**  - &gt; **管理NuGet Packages for Solution**を選択して**インストールされているパッケージ**で、 **NuGet パッケージの管理** ダイアログ ボックス。


### <a name="touring-visual-studio"></a>Visual Studio を touring

Visual Studio での主なウィンドウには、**ソリューション エクスプ ローラー**、**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Express で)、**プロパティウィンドウ**、**ツールボックス**、**ツールバー**、および**ドキュメント ウィンドウ**します。

![プロジェクトの NuGet のダイアログ ボックスを作成します。](create-the-project/_static/image9.png)

Visual Studio の詳細については、次を参照してください。 [Visual Web Developer を視覚的なガイド](https://msdn.microsoft.com/library/ee410104.aspx)です。

## <a name="summary"></a>概要

このチュートリアルで作成、レビューし、既定の Web フォーム アプリケーションを実行します。 既定の Web フォーム アプリケーションのさまざまな機能を確認し、Visual Studio 環境の使用方法についての基本を学習します。 次のチュートリアルでは、データ アクセス レイヤーを作成します。

## <a name="additional-resources"></a>その他のリソース

[右側のプログラミング モデルを選択します。](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web アプリケーション プロジェクトと Web サイト プロジェクト](https://msdn.microsoft.com/en-us/library/dd547590.aspx)   
[ASP.NET Web フォーム ページの概要](https://msdn.microsoft.com/en-us/library/428509ah.aspx)

>[!div class="step-by-step"]
[前へ](introduction-and-overview.md)
[次へ](create_the_data_access_layer.md)
