---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: プロジェクトの作成 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: e5d6fa312ce0375eee5ca456aaea9f088d806cfc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384489"
---
<a name="create-the-project"></a>プロジェクトの作成
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでが作成、および確認するには、ASP.NET の機能に精通することができる、Visual Studio で、既定のプロジェクトを実行します。 また、Visual Studio 環境を確認します。

## <a name="what-youll-learn"></a>学習内容。

- 新しい Web フォーム プロジェクトを作成する方法。
- Web フォーム プロジェクトのファイルの構造体。
- Visual Studio でプロジェクトを実行する方法。
- 既定の Web フォーム アプリケーションのさまざまな機能です。
- Visual Studio 環境を使用する方法についての基本です。

## <a name="creating-the-project"></a>プロジェクトの作成

1. Visual Studio を開きます。
2. 選択**新しいプロジェクト**から、**ファイル**Visual Studio のメニュー。 

    ![プロジェクトの新しいプロジェクト メニュー項目を作成します。](create-the-project/_static/image1.png)
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレート グループ。
4. 選択、 **ASP.NET Web アプリケーション**中央の列のテンプレート。  
 このチュートリアル シリーズでは、.NET Framework 4.5.2 を使用しています。
5. プロジェクトに名前を*WingtipToys*選択、 **OK**ボタンをクリックします。 

    ![-プロジェクトを作成するには、新しいプロジェクト ダイアログ ボックス](create-the-project/_static/image2.png)

    > [!NOTE]
    > このチュートリアル シリーズで、プロジェクトの名前は**WingtipToys**します。 これを使用することをお勧め*正確な*プロジェクト名をされるので、コードがチュートリアルのシリーズ全体で予想通りに機能を提供します。

6. をクリックして、**認証の変更**ボタンをクリックします。 選択**個々 のユーザー アカウント** をクリックし、 **OK**ボタン。

7. 選択、 **Web フォーム**テンプレートをクリックして、 **OK**ボタンをクリックします。

    ![プロジェクトの新しいプロジェクト テンプレートを作成します。](create-the-project/_static/image3.png)

プロジェクトには、作成するを少し時間がかかります。 準備ができたら、開く、 **Default.aspx**ページ。

![プロジェクトの新しいプロジェクト テンプレートを作成します。](create-the-project/_static/image4.png)

切り替えることができます**デザイン**ビューと**ソース**センター ウィンドウの下部にあるオプションを選択して表示します。 **デザイン**ビューは、ASP.NET Web ページ、マスター ページ、コンテンツ ページ、HTML ページが表示されます。 および、WYSIWYG に近いビューを使用してユーザーを制御します。 **ソース**ビューを編集できる Web ページの HTML マークアップが表示されます。

> [!TIP] 
> 
> **ASP.NET フレームワークを理解します。**
> 
> ASP.NET Web フォームでは、使い慣れたドラッグ アンド ドロップ、イベント ドリブン モデルを使用して動的な web サイトをビルドできます。 デザイン サーフェスや数百のコントロールとコンポーネントを洗練された強力な UI 駆動型サイトとデータ アクセスを迅速に構築できます。 Wingtip の玩具店は、ASP.NET Web フォームに基づきますが、このチュートリアル シリーズで学習する概念の多くは ASP.NET のすべてに適用可能。
> 
> ASP.NET には、次の 4 つの主要な開発フレームワークが用意されています。
> 
> - [ASP.NET Web フォーム](../../../index.md)  
>  Web フォーム フレームワークは、Microsoft Windows フォーム (WinForms)、WPF、XAML、/Silverlight などの宣言型とコントロール ベースのプログラミングを使用する開発者を対象とします。 Web 開発の迅速なアプリケーション開発 (RAD) 環境を探している開発者によく使用されますので、WYSIWYG デザイナー駆動型開発モデルを提供します。 Web プログラミングの経験は (たとえば、Visual Basic および Visual c#)、従来の Microsoft の RAD クライアント開発ツールに精通していて、HTML および JavaScript でのエクスペリエンスをしなくても web アプリケーションをすばやく作成できます。
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC は、パターンとテスト駆動開発、懸念事項の分離、制御の反転 (IoC) や依存関係の注入 (DI) 原則に関心がある開発者を対象とします。 このフレームワークは、そのプレゼンテーション層から web アプリケーションのビジネス ロジック層を分離することをお勧めします。
> - [ASP.NET Web ページ](../../../../web-pages/index.md)  
>  ASP.NET Web ページは、PHP の線に沿って、単純な web 開発のストーリーを望む開発者を対象とします。 Web ページのモデルでは、HTML ページを作成し、ページに動的にそのマークアップをレンダリングする方法を制御するためにサーバー ベースのコードを追加します。 Web ページは軽量なフレームワークでは、目的に設計され、HTML を理解、広範なプログラミング エクスペリエンスをたとえばいない可能性がありますが、人、受講者または趣味の ASP.NET に最も簡単なエントリ ポイントです。 ASP.NET の使用を開始するには、PHP または類似のフレームワークを理解している web 開発者にとって良いです。
> - [ASP.NET Single Page Application](../../../../single-page-application/index.md)  
>  ASP.NET Single Page Application (SPA) を使用して、HTML 5、CSS 3、JavaScript を使用して重要なクライアント側の対話を含むアプリケーションを構築できます。 ASP.NET および Web Tools 2012.2 Update には、knockout.js と ASP.NET Web API を使用してシングル ページ アプリケーションを構築するための新しいテンプレートが付属しています。 だけでなく、新しい SPA テンプレート新しい SPA テンプレートのコミュニティが作成したもダウンロードできます。
> 
> 次の 4 つの主要な開発フレームワークだけでなく、ASP.NET は、認識し、使用すると、使い慣れたにとって重要なこのチュートリアル シリーズでは説明しませんが、その他のテクノロジを提供します。
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -さまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスを構築するためのフレームワークです。
> - [ASP.NET SignalR](../../../../signalr/index.md) -をリアルタイム web 機能の開発を容易にするライブラリ。


### <a name="reviewing-the-project"></a>プロジェクトのレビュー

Visual Studio で、**ソリューション エクスプ ローラー**ウィンドウを使用して、プロジェクトのファイルを管理できます。 アプリケーションに追加されたフォルダーを見ていきましょう**ソリューション エクスプ ローラー**します。 Web アプリケーション テンプレートは、基本的なフォルダー構造を追加します。

![プロジェクトのソリューション エクスプ ローラーを作成します。](create-the-project/_static/image5.png)

Visual Studio では、いくつかの初期のフォルダーと、プロジェクトのファイルを作成します。 このチュートリアルの後半で使用して、最初のファイルは次のとおりです。

| **ファイル** | **目的** |
| --- | --- |
| *Default.aspx* | 通常、最初のページをブラウザーで、アプリケーションの実行時に表示されます。 |
| *Site.Master* | このページは、アプリケーションのページの一貫したレイアウトと使用標準動作を作成することができますです。 |
| *Global.asax* | ASP.NET または HTTP モジュールによって発生したアプリケーション レベルおよびセッション レベルのイベントに応答するコードを含む省略可能なファイル。 |
| *Web.config* | アプリケーションの構成データ。 |

### <a name="running-the-default-web-application"></a>既定の Web アプリケーションを実行しています。

既定の Web アプリケーションでは、組み込みの機能とサポートに基づく豊富なエクスペリエンスを提供します。 既定の Web フォーム プロジェクトに変更を加えず、アプリケーションがローカルの Web ブラウザーで実行する準備ができてです。

1. キーを押して、 ***F5*** Visual Studio の中にキー。   
 アプリケーションがビルドされ、Web ブラウザーで表示します。  

    ![プロジェクトの作成 - 既定のページ](create-the-project/_static/image6.png)
2. 実行中のアプリケーションでは、完了したレビューをした後は、ブラウザー ウィンドウを閉じます。

この既定の Web アプリケーションでは次の 3 つのメイン ページが: *Default.aspx* (ホーム)、 *About.aspx*、および*Contact.aspx*します。 これらの各ページは、上部のナビゲーション バーからアクセスできます。 アカウントのフォルダーに格納された、2 つのページ、Register.aspx ページおよびの Login.aspx ページもあります。 これら 2 つのページを使用すると、作成、保存、およびユーザーの資格情報の検証に ASP.NET のメンバーシップ機能を使用できます。

## <a name="aspnet-web-forms-background"></a>ASP.NET Web フォームの背景

ASP.NET Web フォームでは、ページは、サーバーで動的に実行されるコードに、ブラウザーまたはクライアント デバイスに Web ページの出力が生成されます、Microsoft ASP.NET のテクノロジに基づいてです。 ASP.NET Web フォーム ページには、適切なブラウザー準拠 HTML スタイルやレイアウトなどの機能を自動的に表示します。 Web フォームは、.NET 共通言語ランタイム、Microsoft Visual Basic や Microsoft Visual c# などでサポートされている任意の言語との互換性。 また、Web フォームは上に構築された、 [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123)、管理された環境、タイプ セーフ、および継承などの利点を提供します。

ASP.NET Web フォーム ページを実行すると、ページは、一連の処理手順が実行するライフ サイクルについて説明します。 次の手順には、初期化、コントロールをインスタンス化、復元、および状態を維持するには、イベント ハンドラーのコードを実行中かつレンダリングが含まれます。 ASP.NET Web フォームの強力な機能についての理解になると、理解するための重要なは、 [ASP.NET ページのライフ サイクル](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx)する効果の適切なライフ サイクルの段階でコードを記述するようにします。

ページに対する要求を受信すると、Web サーバーとのページを検索処理、ブラウザーに送信しますし、すべてのページの情報を破棄します。 ユーザーは、同じページをもう一度要求している場合、サーバーには、最初からページを再処理シーケンス全体が繰り返されます。 別の言い方を行うには、サーバーを持たない処理ページを使用しているページのメモリはステートレスです。 ASP.NET ページ フレームワークは、ページとそのコントロールの状態を維持するためのタスクを自動的に処理し、アプリケーション固有の情報の状態を維持するために明示的な方法が提供します。

> [!TIP] 
> 
> **Web フォーム アプリケーション テンプレートでの web アプリケーションの機能**
> 
> ASP.NET Web フォーム アプリケーション テンプレートは、組み込みの機能の豊富なセットを提供します。 提供するだけでなく、 *Home.aspx*  ページで、 *About.aspx*  ページで、 *Contact.aspx* 、ページしますが、ユーザーを登録し、保存するメンバーシップ機能も含まれています自分の資格情報、web サイトにログインできるようにします。 ここでは、ASP.NET Web フォーム アプリケーション テンプレートと、Wingtip Toys アプリケーションでの使い方に含まれている機能の一部の詳細について説明します。
> 
> **メンバーシップ**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Identity は、アプリケーションによって作成されたデータベースでユーザーの資格情報を格納します。 ユーザーがログイン時に、アプリケーションは、データベースの読み取りで自分の資格情報を検証します。 プロジェクトの*アカウント*フォルダーにはメンバーシップのさまざまな部分を実装するファイルが含まれています。 登録、ログイン、パスワードの変更、およびアクセスの承認。 さらに、ASP.NET Web フォームは、OAuth と OpenID をサポートします。 これらの認証拡張機能として、Facebook、Twitter、Windows Live、Google およびそのようなアカウントから、既存の資格情報を使用して、サイトにログインできるようにします。
> 
> ![プロジェクトのソリューション エクスプ ローラー (ASP.NET Identity) の作成します。](create-the-project/_static/image7.png)
> 
> 既定では、SQL Server Express LocalDB は、Visual Studio Express 2013 for Web に付属している開発データベース サーバーのインスタンスの既定のデータベース名を使用してメンバーシップ データベースを作成します。
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx)は SQL Server データベースの多くのプログラミング機能を持つ SQL Server の軽量バージョンです。 SQL Server Express LocalDB は、ユーザー モードで実行され、インストールの前提条件の一覧が短い、高速の構成のインストールが。 Microsoft SQL Server、任意のデータベースまたは TRANSACT-SQL コード内に移動できます SQL Server Express LocalDB から SQL Server と SQL Azure のアップグレード手順なし。 そのため、SQL Server Express LocalDB は SQL Server のすべてのエディションを対象とするアプリケーションの開発環境として使用できます。 SQL Server Express LocalDB は、SQL Server Compact では使用できませんするストアド プロシージャ、ユーザー定義関数と集計、.NET Framework の統合、空間型および他のユーザーなどの機能を有効します。
> 
> **マスター ページ**
> 
> [ASP.NET マスター ページ](https://msdn.microsoft.com/library/wtxbf3hh.aspx)アプリケーションで一貫した外観およびすべてのページの動作を定義します。 マスター ページのレイアウトは、ユーザーに表示される最後のページを生成するために個々 のコンテンツ ページのコンテンツをマージします。 Wingtip Toys アプリケーションで変更して、 *Site.master* Wingtip Toys の web サイトのすべてのページは、同じ特徴的なロゴとナビゲーション バーを共有できるように、マスター ページ。
> 
> **HTML5**
> 
> ASP.NET Web フォーム アプリケーション テンプレートは、サポート[HTML5](http://www.w3schools.com/html/html5_intro.asp)、これは、HTML マークアップ言語の最新バージョンです。 HTML5 では、新しい要素と Web サイトを作成するが簡単にする機能をサポートします。
> 
> **Modernizr**
> 
> HTML5 をサポートしていないブラウザーでは、使用することができます[Modernizr](http://www.modernizr.com/)します。 Modernizr は、ブラウザーが HTML5 機能をサポートし、そうでない場合に有効にするかどうかを検出できるオープン ソース JavaScript ライブラリです。 ASP.NET Web フォーム アプリケーション テンプレートでは、Modernizr は NuGet パッケージとしてインストールされます。
> 
> **Bootstrap**
> 
> Visual Studio 2013 のプロジェクト テンプレートを使用して、[ブートス トラップ](http://getbootstrap.com/)、Twitter によって作成されたレイアウトとテーマのフレームワークです。 ブートス トラップでは、CSS3 を使用して、レイアウトは、別のブラウザー ウィンドウのサイズに動的に対応できることを意味するレスポンシブ デザインを提供します。 アプリケーションの外観の変更を簡単に影響するのにブートス トラップのテーマ機能を使用することもできます。 既定では、Visual Studio 2013 で ASP.NET Web アプリケーション テンプレートには、NuGet パッケージとしてのブートス トラップが含まれています。
> 
> **NuGet パッケージ**
> 
> ASP.NET Web フォーム アプリケーション テンプレートにはセットが含まれています[NuGet](http://www.nuget.org/)パッケージ。 これらのパッケージは、オープン ソース ライブラリとツールのフォームでのコンポーネント化された機能を提供します。 さまざまなパッケージを作成およびアプリケーションをテストすることがあります。 Visual Studio では、簡単に追加、削除、および NuGet パッケージを更新できます。 開発者は、作成し、パッケージを NuGet にも追加できます。
> 
> ![プロジェクトの NuGet ダイアログ ボックスを作成します。](create-the-project/_static/image8.png)
> 
> パッケージをインストールするときに、NuGet は、ソリューションにファイルがコピーされ、自動的に参照を追加して、Web アプリケーションに関連付けられている構成を変更するなど、どのような変更が必要になります。 ライブラリを削除する場合は、NuGet はファイルを削除し、煩雑さが残っていないようにに、プロジェクトでこれが行われた変更を反転させます。 NuGet は、**ツール**Visual Studio のメニュー。
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/)は HTML ドキュメントの走査、イベント処理、アニメーション化、および迅速な web 開発用の Ajax の相互作用を簡略化、高速かつ簡潔な JavaScript ライブラリです。 JQuery JavaScript ライブラリは、NuGet パッケージとして ASP.NET Web フォーム アプリケーション テンプレートに追加されます。
> 
> **控え目な検証**
> 
> クライアント側検証ロジックに控え目な JavaScript を使用する組み込みの検証コントロールが構成されています。 これが大幅にインラインで、ページのマークアップでレンダリングを JavaScript の量が減少し、全体的なページ サイズを縮小します。 設定に基づいて、ASP.NET Web フォーム アプリケーション テンプレートにグローバルに控え目な検証を追加、 &lt;appSettings&gt;の要素、 *Web.config*アプリケーションのルートにあるファイル。
> 
> **エンティティ フレームワーク コード ファースト**
> 
> Wingtip Toys アプリケーションを使用して、ASP.NET Web フォーム アプリケーション テンプレートの機能だけでなく[Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx)、ライブラリは NuGet ライブラリ データを操作する場合は、コードを中心とした開発を可能です。 簡単に言うと、記述するコードに基づき、アプリケーションのデータベースの部分を作成します。 Entity Framework を使用して取得し、厳密に型指定されたオブジェクトとしてデータを操作します。 これによりデータへのアクセス方法の詳細ではなく、アプリケーションでビジネス ロジックに集中できます。
> 
> インストール済みのライブラリおよび ASP.NET Web フォーム テンプレートに含まれているパッケージの詳細については、インストールされている NuGet パッケージの一覧を参照してください。 Visual Studio が新しい Web フォーム プロジェクトを作成するのには、**ツール** - &gt; **ライブラリ パッケージ マネージャー**  - &gt; **管理ソリューションの NuGet パッケージ**、選び**パッケージがインストールされている**で、 **NuGet パッケージの管理** ダイアログ ボックス。


### <a name="touring-visual-studio"></a>Visual Studio を touring

Visual Studio の主なウィンドウが含まれて、**ソリューション エクスプ ローラー**、**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Express で)、**プロパティウィンドウ**、**ツールボックス**、**ツールバー**、および**ドキュメント ウィンドウ**します。

![プロジェクトの NuGet ダイアログ ボックスを作成します。](create-the-project/_static/image9.png)

Visual Studio の詳細については、次を参照してください。 [Visual Web Developer を視覚的なガイド](https://msdn.microsoft.com/library/ee410104.aspx)します。

## <a name="summary"></a>まとめ

このチュートリアルで作成、確認して、既定の Web フォーム アプリケーションを実行します。 既定の Web フォーム アプリケーションのさまざまな機能を確認し、Visual Studio 環境を使用する方法についての基本を学習します。 以下のチュートリアルでは、データ アクセス層を作成します。

## <a name="additional-resources"></a>その他のリソース

[正しいプログラミング モデルを選択します。](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web アプリケーション プロジェクトと Web サイト プロジェクト](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web フォーム ページの概要](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [前へ](introduction-and-overview.md)
> [次へ](create_the_data_access_layer.md)
