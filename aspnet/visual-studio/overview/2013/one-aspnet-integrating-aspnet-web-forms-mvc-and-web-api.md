---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "ハンズ オン ラボ: 1 つの ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET は、Web サイト、アプリケーション、MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 で ASP.NET h を拡張しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>ハンズ オン ラボ: 1 つの ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> ASP.NET は、Web サイト、アプリケーション、MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 展開と ASP.NET が作成されてから表示、明示する必要がありますこれらのテクノロジと統合された最近の取り組みに向けて作業**One ASP.NET**です。
> 
> Visual Studio 2013 には、アプリケーションを作成したり、すべての ASP.NET テクノロジを 1 つのプロジェクトで使用する新しい統合プロジェクト システムが導入されています。 この機能は、プロジェクトと関連付け、スティックの開始時に 1 つのテクノロジを選択する必要がある、代わりに 1 つのプロジェクト内の複数の ASP.NET フレームワークの使用が推奨します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- ベースの Web サイトを作成、 **One ASP.NET**プロジェクトの種類
- 使用して異なる**ASP.NET**のようなフレームワーク**MVC**と**Web API**同じプロジェクト内
- 主要なコンポーネントを識別、 **ASP.NET**アプリケーション
- 利用、 **ASP.NET スキャフォールディング**CRUD 操作を実行するには、コント ローラーとビューを自動的に作成するためにフレームワークが、モデルのクラスに基づく
- 同じジョブごとに適切なツールを使用してコンピューターの人間が判読できる形式で情報のセットを公開します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。

1. 開いている Windows エクスプ ローラーおよびテスト環境の参照**ソース**フォルダーです。
2. 右クリックして**Setup.cmd**選択と**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットを使用します。

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> 各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。 実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [新しい Web フォーム プロジェクトを作成します。](#Exercise1)
2. [スキャフォールディングを使用して、MVC コント ローラーを作成します。](#Exercise2)
3. [スキャフォールディングを使用して Web API コント ローラーを作成します。](#Exercise3)

この演習を完了する時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。 このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。 開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>手順 1: 新しい Web フォーム プロジェクトを作成します。

この演習では、Visual Studio 2013 を使用して新しい Web フォーム サイトを作成します、 **One ASP.NET**統合プロジェクトの環境は、同じアプリケーションで Web フォーム、MVC、Web API のコンポーネントを簡単に統合することができます。 生成されたソリューションを調査し、その部分を特定および操作で、Web サイトを最後に表示されます。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>作業 1: 1 つの ASP.NET エクスペリエンスを使用して新しいサイトを作成します。

このタスクの開始は Visual Studio で新しい Web サイトを作成するに基づいて、 **One ASP.NET**プロジェクトの種類。 **1 つの ASP.NET**にすべての ASP.NET テクノロジをまとめを混在させるし、必要に応じて、それらが一致するオプションを選択できます。 アプリケーション内でサイド バイ サイドで、ライブ Web フォーム、MVC、Web API のさまざまなコンポーネントを識別します。

1. 開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.**を新しいソリューションを開始します。

    ![新規プロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下にある、 **Visual c# |Web** ] タブの [し、確認**.NET Framework 4.5**が選択されています。 プロジェクトに名前を*MyHybridSite*、選択、**場所** をクリック**OK**です。

    ![新しい ASP.NET Web アプリケーション プロジェクト](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *新しい ASP.NET Web アプリケーション プロジェクトを作成します。*
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレートを選択、 **MVC**と**Web API**オプション。 また、ことを確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**です。 **[OK]** をクリックして続行します。

    ![Web API と MVC コンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Web API と MVC コンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトの作成*
4. ここで生成されたソリューションの構造を調べることができます。

    ![生成されたソリューションを調べる](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *生成されたソリューションを調べる*

    1. **アカウント:**このフォルダーには、登録にログインし、アプリケーションのユーザー アカウントを管理するには、Web フォーム ページが含まれています。 このフォルダーが追加されたときに、**個々 のユーザー アカウント**認証オプションは、Web フォーム プロジェクト テンプレートの構成中に選択します。
    2. **モデル:**このフォルダーは、アプリケーション データを表すクラスに格納されます。
    3. **コント ローラー**と**ビュー**: これらのフォルダーに必要な**ASP.NET MVC**と**ASP.NET Web API**コンポーネントです。 次の演習で MVC および Web API のテクノロジについて学びます。
    4. **Default.aspx**、 **Contact.aspx**と**About.aspx**ファイルに固有のページを構築する開始点として使用できる定義済みの Web フォーム ページは、アプリケーション。 これらのファイルのプログラミング ロジックと呼ばれる別のファイルに含まれて、&quot;分離コード&quot;を持つファイルを&quot;..aspx.vb ファイル&quot;または&quot;. aspx.cs&quot;拡張機能 (によって、使用される言語)。 分離コード ロジックは、サーバー上で実行し、動的に、ページの HTML 出力を生成します。
    5. **Site.Master**と**Site.Mobile.Master**ページ アプリケーションのルック アンド フィールとすべてのページの標準の動作を定義します。
5. ダブルクリックして、 **Default.aspx**ページのコンテンツを参照するファイル。

    ![Default.aspx ページを調べる](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Default.aspx ページを調べる*

    > [!NOTE]
    > **ページ**ファイルの上部にあるディレクティブが、Web フォーム ページの属性を定義します。 たとえば、 **MasterPageFile**属性は、マスターへのパスを指定 ページでこのケースでは、 *Site.Master*ページ - と**Inherits**属性を定義、ページを継承する分離コード クラスです。 このクラスは、によって決まりますファイルにある、**分離コード**属性。
    > 
    > **Asp:content**コントロールがページ (テキスト、マークアップとコントロール) の実際のコンテンツを保持しにマップされて、 **asp: ContentPlaceHolder**マスター ページ上のコントロールです。 内のページ コンテンツにレンダリングされるここで、*メイン*で定義されているコントロール、 *Site.Master*ページ。
6. 展開して、**アプリ\_開始**フォルダーとに注意してください、 **WebApiConfig.cs**ファイル。 Visual Studio には、テンプレートを 1 つの ASP.NET プロジェクトを構成するときに、Web API が含まれるために、そのファイルが生成されたソリューションに含まれます。
7. 開く、 **WebApiConfig.cs**ファイル。 *WebApiConfig*を HTTP にマップする Web API に関連付けられている構成を検索がクラスにルーティング**Web API コント ローラー**です。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. 開く、 **RouteConfig.cs**ファイル。 内部、 *RegisterRoutes*メソッドへの HTTP ルートをマップする MVC に関連付けられている構成を検索、 **MVC コント ローラー**です。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

生成されたソリューションを実行がこのタスクでされたら、アプリといくつかの URL の書き換えと組み込みの認証と同様に、その機能について説明します。

1. キーを押して、ソリューションを実行する**f5 キーを押して** をクリックして、**開始**ツールバーにボタンがあります。 アプリケーション ホーム ページは、ブラウザーで開く必要があります。

    ![ソリューションの実行](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Web フォーム ページが呼び出されていることを確認します。 これを行うには、次のように追加します。 **/contact.aspx**キーを押して、アドレス バーの URL に**Enter**です。

    ![フレンドリな Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *フレンドリな Url*

    > [!NOTE]
    > URL が変更されたことがわかります**にお問い合わせください/**です。 始まる**ASP.NET 4**URL ルーティング機能は、Web フォームに追加された、Url などのため、記述することができます、  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  ではなく*[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*です。 詳細についてを参照してください[URL ルーティング](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)です。
3. アプリケーションに統合認証の流れを調査します。 これを行うには、をクリックして**登録**ページの右上隅にします。

    ![新しいユーザーを登録します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *新しいユーザーを登録します。*
4. **登録** ページで、入力、**ユーザー名**と**パスワード**、クリックして**登録**です。

    ![登録 ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *登録 ページ*
5. アプリケーションが、新しいアカウントを登録し、ユーザーを認証します。

    ![認証されたユーザー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *認証されたユーザー*
6. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>手順 2: スキャフォールディングを使用して、MVC コント ローラーを作成します。

この演習では、アクションと Razor ビューを 1 行のコードを記述することがなく、CRUD 操作を実行すると、ASP.NET MVC 5 コント ローラーを作成する Visual Studio によって提供される ASP.NET スキャフォールディング フレームワークの利点がかかります。 スキャフォールディング プロセスは、SQL データベースのデータ コンテキストおよびデータベース スキーマを生成するのに Entity Framework Code First で使用されます。

**Entity Framework の Code First**

Entity Framework (EF) は、直接リレーショナル ストレージ スキーマを使用したプログラミングではなく、概念アプリケーション モデルを使用したプログラミング、データ アクセス アプリケーションを作成することができますオブジェクト リレーショナル マッパー (ORM) です。

Entity Framework Code First のモデリング ワークフロー使用する EF は、このクエリを実行するを実行するときに依存しているモデルを表す独自のドメイン クラスを使用して変更の追跡および更新機能できます。 Code First の開発のワークフローを使用する必要はありません、データベースを作成するか、スキーマを指定して、アプリケーションを開始します。 オブジェクトを定義する、最も適切なドメイン モデル、アプリケーションの標準の .NET クラスを作成して、Entity Framework をデータベースに作成されます。

> [!NOTE]
> Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)です。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>作業 1: 新しいモデルを作成します。

ここで定義する、**人**MVC コント ローラーとビューを作成する、スキャフォールディング プロセスによって使用されるモデルとなるクラスです。 作成することで開始されます、 **Person**モデル クラス、およびコント ローラーでの CRUD 操作が自動的に作成スキャフォールディング機能を使用します。

1. 開いている**Visual Studio Express 2013 for Web**と**MyHybridSite.sln**にソリューションがある、**ソース/Ex2-MvcScaffolding/開始**フォルダーです。 代わりに、前の手順で取得した、ソリューションを続行できます。
2. **ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |クラス.**.

    ![ユーザー モデル クラスを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *ユーザー モデル クラスを追加します。*
3. **新しい項目の追加**ダイアログ ボックスで、[ファイル名*Person.cs* ] をクリック**追加**です。

    ![ユーザー モデル クラスを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *ユーザー モデル クラスを作成します。*
4. コンテンツを置き換える、 **Person.cs**を次のコード ファイル。 キーを押して**ctrl キーを押しながら S**変更を保存します。

    (コード スニペットの*BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. **ソリューション エクスプ ローラー**を右クリックし、 **MyHybridSite**プロジェクトを選択**ビルド**、またはキーを押して**ctrl キーと shift キーを押しながら B**プロジェクトをビルドします。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>タスク 2-ある MVC コント ローラーを作成します。

これで、 **Person**モデルが作成されると、Entity Framework と ASP.NET MVC のスキャフォールディングを使用すると、CRUD コント ローラーのアクションおよびのビューを作成する**Person**です。

1. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |新しい項目のスキャフォールディングしています.**.

    ![新しいスキャフォールディング コント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *スキャフォールディングされた新しいコント ローラーの作成*
2. **追加 Scaffold**ダイアログ ボックスで、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して** をクリックし、**追加します。**

    ![ビューおよび Entity Framework を使用した MVC 5 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *ビューおよび Entity Framework を使用した MVC 5 コント ローラーを選択*
3. 設定*MvcPersonController*として、**コント ローラー名**、select、**非同期コント ローラー アクションを使用して**をクリックして**人 (MyHybridSite.Models)**として、**モデル クラス**です。

    ![スキャフォールディングがある MVC コント ローラーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *スキャフォールディングがある MVC コント ローラーを追加します。*
4. **データ コンテキスト クラス**をクリックして**新しいデータ コンテキストしています.**.

    ![新しいデータ コンテキストを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *新しいデータ コンテキストを作成します。*
5. **新しいデータ コンテキスト** ダイアログ ボックスに、名前の新しいデータ コンテキスト*PersonContext*  をクリック**追加**です。

    ![新しい PersonContext を作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *新しい PersonContext 型を作成します。*
6. をクリックして**追加**を新しいコント ローラーを作成する**Person**スキャフォールディングとします。 Visual Studio は、コント ローラーのアクション、Person データ コンテキストおよび Razor ビュー生成し、されます。

    ![MVC コント ローラーをスキャフォールディングで作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *MVC コント ローラーをスキャフォールディングで作成した後*
7. 開く、 **MvcPersonController.cs**ファイルで、**コント ローラー**フォルダーです。 CRUD アクション メソッドが自動的に生成されたことに注意してください。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > 選択して、**非同期コント ローラー アクションを使用して**前の手順でオプションのチェック ボックスをスキャフォールディングから、Visual Studio は、Person データ コンテキストへのアクセスに関連するすべてのアクションの非同期アクション メソッドを生成します。 実行時間の長いの非同期アクション メソッドを使用すると、CPU バインド以外の要求、要求の処理中に作業を実行してから、Web サーバーがブロックされないようにすることをお勧めします。

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3: ソリューションの実行

このタスクではソリューションを実行することを確認するには、もう一度ビュー**人**が想定どおりに機能します。 新しいデータベースに正常に保存することを確認する担当者を追加します。

1. キーを押して**f5 キーを押して**ソリューションを実行します。
2. 移動**/MvcPerson**です。 ユーザーの一覧を表示するスキャフォールディング ビューが表示されます。
3. をクリックして**新規作成**を新しいユーザーを追加します。

    ![スキャフォールディングの MVC ビューへの移動](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *スキャフォールディングの MVC ビューへの移動*
4. **作成**ビューで、提供、**名**と**Age**人、およびクリック**作成**。

    ![新しいユーザーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *新しいユーザーを追加します。*
5. 新しいユーザーは、一覧に追加されます。 要素の一覧でクリックして**詳細**人の詳細ビューを表示します。 その後、、**詳細**ビューで、をクリックして**一覧に戻る**リスト ビューに戻ります。

    ![ユーザーの詳細ビュー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *ユーザーの詳細ビュー*
6. クリックして、**削除**ユーザーを削除するリンクです。 **削除**ビューで、をクリックして**削除**操作を確認します。

    ![ユーザーを削除します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *ユーザーを削除します。*
7. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>手順 3: スキャフォールディングを使用して Web API コント ローラーを作成します。

Web API フレームワークでは、ASP.NET スタックの一部であるし、実装して HTTP サービスを簡単にするため、通常、RESTful API を通じて JSON または XML 形式のデータの送受信に設計されています。

この練習では、Web API コント ローラーを生成するのに ASP.NET スキャフォールディングをもう一度使用されます。 同じ使用する**Person**と**PersonContext** JSON 形式で同じ個人データを提供する前の手順からのクラスです。 同じ ASP.NET アプリケーション内でさまざまな方法で、同じリソースを公開する方法が表示されます。

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>作業 1: Web API コント ローラーの作成

このタスクでは新規に作成する**Web API コント ローラー**するには、JSON のようにコンピューターが使用できる形式でユーザー データを公開します。

1. 開いていない場合は開きます**Visual Studio Express 2013 for Web**を開くと、 **MyHybridSite.sln**にソリューションがある、**ソース/Ex3-WebAPI/開始**フォルダーです。 代わりに、前の手順で取得した、ソリューションを続行できます。

    > [!NOTE]
    > 手順 3 から開始ソリューションを起動した場合、以下のキーを押して**ctrl キーと shift キーを押しながら B**ソリューションをビルドします。
2. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |新しい項目のスキャフォールディングしています.**.

    ![新しいスキャフォールディング コント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *新しいスキャフォールディング コント ローラーの作成*
3. **追加 Scaffold**ダイアログ ボックスで、 **Web API**し、左ペインで**Web API 2 コント ローラーのアクション、Entity Framework を使用して**中央のペインでをクリックして**追加します。**

    ![アクションおよび Entity Framework と Web API 2 コント ローラーを選択すると](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "を選択すると Web API 2 コント ローラーおよびアクションと Entity Framework")

    *アクションおよび Entity Framework と Web API 2 コント ローラーを選択します。*
4. 設定*ApiPersonController*として、**コント ローラー名**、select、**非同期コント ローラー アクションを使用して**をクリックして**人 (MyHybridSite.Models)**と**PersonContext (MyHybridSite.Models)**として、**モデル**と**データ コンテキスト**それぞれクラスです。 次に、 **[追加]**をクリックします。

    ![スキャフォールディングが Web API コント ローラーを追加する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "スキャフォールディングで Web API コント ローラーの追加")

    *スキャフォールディングがある Web API コント ローラーを追加します。*
5. Visual Studio は生成し、 **ApiPersonController**データを操作する 4 つの CRUD 操作を持つクラス。

    ![スキャフォールディングで Web API コント ローラーを作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "スキャフォールディングで Web API コント ローラーを作成した後")

    *スキャフォールディングで Web API コント ローラーを作成した後*
6. 開く、 **ApiPersonController.cs**ファイルし、検査、 *GetPeople*アクション メソッド。 このメソッドはクエリの db フィールド**PersonContext**型、ユーザー データを取得するためにします。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. メソッドの定義の上にあるコメントに注目してください。 次のタスクで使用するこのアクションを公開する URI を提供します。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > 既定では、Web API が構成されている、クエリをキャッチする、 */api* MVC コント ローラーとの衝突を避けるためへのパス。 この設定を変更する必要がある場合を参照してください[ASP.NET Web API でルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

このタスクでは、Internet Explorer を使用して**F12 開発者ツール**を Web API コント ローラーから応答の全体を検査します。 アプリケーション データをより深く把握へのネットワーク トラフィックをキャプチャする方法が表示されます。

> [!NOTE]
> 確認して**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバー上にあるボタンをクリックします。
> 
> ![Internet Explorer オプション](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 開発者ツール**幅広いこのハンズオンのラボで説明されていない機能があります。 詳細情報を入手する場合を参照してください。 [F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))です。


1. キーを押して**f5 キーを押して**ソリューションを実行します。

    > [!NOTE]
    > このタスクを正常に従うために、アプリケーションをデータである必要があります。 データベースが空の場合は、手順 2 での作業 3 に戻るし、MVC ビューを使用して新しいユーザーを作成する方法の手順に従ってです。
2. ブラウザーで、キーを押して**F12**を開くには、 **Developer Tools**パネルです。 キーを押して**CTRL** + **4**  をクリックして、**ネットワーク**アイコンをクリックしてネットワーク トラフィックのキャプチャを開始する緑色の矢印ボタンをクリックします。

    ![Web API のネットワーク キャプチャを開始する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "ネットワーク キャプチャを開始する Web API")

    *Web API のネットワーク キャプチャを開始しています*
3. 追加**api/ApiPerson**ブラウザーのアドレス バーの URL にします。 応答の詳細を調査して、ここでは、 **ApiPersonController**です。

    ![Web API を使用してユーザー データを取得する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API を使用してユーザー データを取得します。")

    *Web API を使用してユーザー データを取得します。*

    > [!NOTE]
    > ダウンロードが終了した、ダウンロードしたファイルの操作を行うが求められます。 ダイアログ ボックスは、開発者ツール ウィンドウから応答のコンテンツを監視するために開いたままにしておきます。
4. ようになりましたが、応答の本文を検査します。 これを行うをクリックして、**詳細** タブをクリックして**応答本文**です。 ダウンロードされたデータにプロパティを持つオブジェクトの一覧があることを確認できます**Id**、**名前**と**Age**に対応する、 **Person**クラス。

    ![表示する Web API の応答本文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "表示 Web API の応答本文")

    *表示する Web API の応答本文*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>タスク 3: Web API のヘルプ ページの追加

Web API を作成するときは、他の開発者が API を呼び出す方法を知ることは、ヘルプ ページを作成すると便利です。 作成し、ドキュメントのページを手動で更新しますが、自動生成すると、メンテナンス作業を行う必要があるようにすることをお勧めします。 ここでは、Nuget パッケージを使用して、ソリューションに Web API のヘルプ ページを自動的に生成されます。

1. **ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。
2. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage**パッケージが必要なアセンブリをインストールし、下のヘルプ ページの MVC ビューを追加、**領域/HelpPage**フォルダーです。

    ![HelpPage 領域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 領域")

    *HelpPage 領域*
3. 既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。 XML ドキュメント コメントを使用するには、ドキュメントを作成します。 この機能を有効にするを開き、 **HelpPageConfig.cs**にあるファイル、**領域/HelpPage/アプリケーション\_開始**フォルダーと、次の行のコメントを解除。

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. **ソリューション エクスプ ローラー**、プロジェクトを右クリックして**MyHybridSite****プロパティ** をクリックし、**ビルド** タブ。

    ![[ビルド] タブ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "セクションのビルド")

    *[ビルド] タブ*
5. **出力** **XML ドキュメント ファイル**です。 編集ボックスで、次のように入力します。**アプリ\_Data/XmlDocument.xml**です。

    ![[ビルド] タブのセクションを出力](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "[ビルド] タブのセクションの出力")

    *[ビルド] タブで、出力セクション*
6. キーを押して**CTRL** + **S**変更を保存します。
7. 開く、 **ApiPersonController.cs**ファイルから、**コント ローラー**フォルダーです。
8. 間に新しい行を入力、 *GetPeople*メソッドのシグネチャと*/api/ApiPerson を取得/*コメントの追加、および 3 つのスラッシュを入力します。

    > [!NOTE]
    > Visual Studio は、メソッドのドキュメントを定義する XML 要素を自動的に挿入します。
9. 概要テキストと、戻り値の追加、 *GetPeople*メソッドです。 次のようになります。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. キーを押して**f5 キーを押して**ソリューションを実行します。
11. 追加**/help**ヘルプ ページに移動する、アドレス バーに URL にします。

    ![ASP.NET Web API のヘルプ ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ヘルプ ページを ASP.NET Web API")

    *ASP.NET Web API Help Page*

    > [!NOTE]
    > ページのメイン コンテンツは、コント ローラーでグループ化された Api のテーブルです。 テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイスです。 追加またはある API コント ローラーを更新する場合、テーブルが自動更新する次に、アプリケーションをビルドするとき。
    > 
    > **API** HTTP メソッドと相対 URI に列が一覧表示します。 **説明**列には、メソッドのドキュメントから抽出された情報が含まれています。
12. 説明 列に、メソッド定義の上に追加する説明が表示されることに注意してください。

    ![API メソッドの説明](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API メソッドの説明")

    *API メソッドの説明*
13. 応答例の本文を含む、詳細な情報をページに移動する、API のメソッドのいずれかをクリックします。

    ![詳細情報ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細情報 ページ")

    *詳細情報 ページ*

* * *

<a id="Summary"></a>
## <a name="summary"></a>概要

このハンズオン ラボを完了して学習した方法。

- Visual Studio 2013 で、1 つの ASP.NET エクスペリエンスを使用して新しい Web アプリケーションを作成します。
- 1 つの 1 つのプロジェクトに複数の ASP.NET テクノロジを統合します。
- ASP.NET のスキャフォールディングを使用して、モデル クラスからの MVC コント ローラーとビューを生成します。
- 非同期プログラミングおよび Entity Framework を介してデータ アクセスなどの機能を使用して Web API コント ローラーを生成します。
- コント ローラーの Web API のヘルプ ページを自動的に生成します。
