---
uid: whitepapers/mvc3-release-notes
title: "ASP.NET MVC 3 |Microsoft ドキュメント"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: a86fae5698c54a71cb598f508aa91e7d96d1b409
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [概要](#overview)
- [インストールに関する注意事項](#installation-notes)
- [ソフトウェアの要件](#software-requirements)
- [ドキュメント](#documentation)
- [サポート](#support)
- [3 Tools Update の ASP.NET MVC に ASP.NET MVC 2 プロジェクトをアップグレードします。](#upgrading)
- [ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)](#tu-changes)

    - ["コント ローラーの追加 ダイアログ ボックスがビューとデータのアクセス コードを含むコント ローラーをスキャフォールディングが可能](#tu-AddControllerDialog)
    - [機能強化、"ASP.NET MVC 3 プロジェクトを新しい" ダイアログ ボックス](#tu-ImprovementsNewDialogBox)
    - [プロジェクト テンプレートに Modernizr 1.7 が](#tu-Modernizr)
    - [プロジェクト テンプレートは、jQuery、jQuery UI、および jQuery の更新バージョンの検証](#tu-UpdatedJQuery)
    - [プロジェクト テンプレートでは、事前インストールされている NuGet パッケージとして ADO.NET Entity Framework 4.1 が追加されました](#tu-EF)
    - [プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める](#tu-JavaScriptLibsNuget)
    - [既知の問題](#tu-KI)
- [ASP.NET MVC 3 RTM (2011 年 1 月 13 日)](#MVC3RTM)

    - [変更: 1.8.7 に jQuery UI のバージョンを更新します。](#RTM-1)
    - [変更: ModelMetadataProvider バックアップ DataAnnotationsModelMetadataProvider 既定を変更します。](#RTM-2)
    - [修正: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける](#RTM-3)
    - [修正: エディターで開かれている Razor ファイルの名前を変更して無効にして構文の色表示機能 IntelliSense](#RTM-4)
    - [既知の問題](#RTM-KI)
    - [重大な変更](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (2010 年 12 月 10 日)](#_Toc2)

    - [1.4.4 jQuery、jQuery 検証 1.7 と jQuery UI 1.8.6y UI 1.8.6 を含むようにプロジェクト テンプレートの変更](#_Toc2_1)
    - [追加された"AdditionalMetadataAttribute"クラス](#_Toc2_2)
    - [強化されたビューのスキャフォールディング](#_Toc2_3)
    - [追加された Html.Raw メソッド](#_Toc2_3)
    - [名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ](#_Toc2_4)
    - ["SessionStateAttribute"に名前が変更された"ControllerSessionStateAttribute"クラス](#_Toc2_5)
    - [「ため」に名前を変更した RemoteAttribute"Fields"プロパティ](#_Toc2_6)
    - ["AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更](#_Toc2_7)
    - [最初の有用なエラー メッセージを表示して変更された"Html.ValidationMessage"メソッド](#_Toc2_8)
    - [固定@model宣言、ドキュメントに空白を追加するには](#_Toc2_9)
    - [エンジンに固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ](#_Toc2_10)
    - ["For"属性の適切な値を出力する固定"LabelFor"ヘルパー](#_Toc2_11)
    - [モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド](#_Toc2_12)
    - [重大な変更](#_Toc2_BC)
    - [既知の問題](#_Toc2_KI)
- [ASP.NET MVC 3 リリース候補 (2010 年 11 月 9 日)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 RC の新機能](#_Toc276711785)
    - [NuGet パッケージ マネージャー](#_Toc276711786)
    - [向上""新しいプロジェクト ダイアログ ボックス](#_Toc276711787)
    - [Sessionless コント ローラー](#_Toc276711788)
    - [新しい検証属性](#_Toc276711789)
    - ["LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード](#_Toc276711790)
    - [子アクション出力キャッシュ](#_Toc276711791)
    - ["ビューの追加 ダイアログ ボックスの機能強化](#_Toc276711792)
    - [詳細な要求の検証](#_Toc276711793)
    - [重大な変更](#_Toc276711794)
    - [既知の問題](#_Toc276711795)
- [ASP です。MVC 3 ベータ ノート (2010 年 10 月 6 日)](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 のベータ版の新機能](#0.1__Toc274034215)
    - [NuPack パッケージ マネージャー](#0.1__Toc274034216)
    - [[新しいプロジェクト] ダイアログ ボックスの向上](#0.1__Toc274034217)
    - [厳密に指定することは簡略化された Razor ビューでのモデルの入力](#0.1__Toc274034218)
    - [新しい ASP.NET Web ページのヘルパー メソッドをサポートします。](#0.1__Toc274034219)
    - [追加の依存関係の挿入のサポート](#0.1__Toc274034220)
    - [控えめな jQuery ベース Ajax のサポート](#0.1__Toc274034221)
    - [控えめな jQuery 検証用の新しいサポート](#0.1__Toc274034222)
    - [クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ](#0.1__Toc274034223)
    - [ビューの実行前に実行されるコードのサポート](#0.1__Toc274034224)
    - [VBHTML Razor 構文の新しいサポート](#0.1__Toc274034225)
    - [ValidateInputAttribute をより細かく制御](#0.1__Toc274034226)
    - [ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。](#0.1__Toc274034227)
    - [バグの修正](#0.1__Toc274034228)
    - [重大な変更](#0.1__Toc274034229)
    - [既知の問題](#0.1__Toc274034230)
- [免責事項](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>概要

このドキュメントでは、Visual Studio 2010 用の ASP.NET MVC 3 RTM のリリースについて説明します。 ASP.NET MVC は、モデル ビュー コント ローラー (MVC) パターンを使用して、Web アプリケーションを開発するためのフレームワークです。 ASP.NET MVC 3 インストーラーには、次のコンポーネントが含まれています。

- ASP.NET MVC 3 ランタイム コンポーネント
- ASP.NET MVC 3 Visual Studio 2010 ツール
- ASP.NET Web ページの実行時コンポーネント
- ASP.NET Web ページ Visual Studio 2010 ツール
- Microsoft Package Manager for .NET (NuGet)
- Razor 構文のサポートを有効にする Visual Studio 2010 用の更新プログラム。 (詳細については、サポート技術情報の記事 2483190 を参照してください)。

ASP.NET MVC 3 の各リリース前のバージョンのリリース ノートの完全なセットは、次の URL で ASP.NET web サイトで確認できます。

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

ASP.NET MVC 3 RTM、Web Platform Installer (Web PI) を使用してをインストールするには、次のページを参照してください。

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

代わりに、次のページから Visual Studio 2010 の ASP.NET MVC 3 RTM 用のインストーラーをダウンロードできます。

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 をインストールして、サイド バイ サイドで実行できる ASP.NET MVC 2 とします。

<a id="software-requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET MVC 3 ランタイム コンポーネントには、次のソフトウェアが必要です。

- .NET framework version 4。 

    ASP.NET MVC 3 Visual Studio 2010 ツールには、次のソフトウェアが必要です。
- Visual Studio 2010 または Visual Web Developer 2010 Express です。

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC のドキュメントは、次の URL で MSDN Web サイトで入手できます。

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

チュートリアルおよび ASP.NET MVC に関する他の情報を次の URL で ASP.NET Web サイトの MVC ページで使用できます。

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Support

これは、完全にサポートされるリリースです。 テクニカル サポートについての情報が見つかりません、 [Microsoft サポート web サイト](https://support.microsoft.com/)です。

自由にここで、ASP.NET コミュニティのメンバーは、頻繁に非公式のサポートを提供する、ASP.NET MVC フォーラムへのこのリリースに関する質問を投稿します。

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>3 Tools Update の ASP.NET MVC に ASP.NET MVC 2 プロジェクトをアップグレードします。

ASP.NET MVC 3 は、する柔軟性が ASP.NET MVC 3、ASP.NET MVC 2 アプリケーションをアップグレードするタイミングを選択する際に、同じコンピューターに ASP.NET MVC 2 のサイド バイ サイドでインストールできます。

バージョン 3 への既存の ASP.NET MVC 2 アプリケーションを手動でアップグレードするには、次の操作を行います。

1. コンピューターに新しい空の ASP.NET MVC 3 プロジェクトを作成します。 このプロジェクトにはアップグレードに必要ないくつかのファイルが含まれます。
2. ASP.NET MVC 3 プロジェクトから ASP.NET MVC 2 プロジェクトの対応する場所に、次のファイルをコピーします。 新しいファイル名 (jquery-1.5.1.js) のために、jQuery ライブラリへの参照を更新する必要があります。 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /コンテンツ/テーマ/\*です。\*
3. コピー、*パッケージ*ソリューションの .sln ファイルがあるディレクトリでは、ソリューションのルートには、空の ASP.NET MVC 3 プロジェクト ソリューションのルートにフォルダーです。
4. ASP.NET MVC 2 プロジェクトにすべての領域が含まれている場合、/Views/Web.config ファイルのコピー、*ビュー*区分のフォルダーです。
5. ASP.NET MVC 2 プロジェクトの両方の Web.config ファイルでグローバル検索し、ASP.NET MVC のバージョンを置換します。 次を検索します。 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    次に置き換えます。

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (指し示します DLL バージョン 2 から) への参照を追加し、 *System.Web.Mvc* (v3.0.0.0)。
7. System.Web.WebPages.dll および System.Web.Helpers.dll への参照を追加します。 これらのアセンブリは、次のフォルダーに配置されます。 

    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 [プロジェクト名を再度右クリックして編集] を選択*ProjectName*.csproj です。
9. 検索、 *ProjectTypeGuids*要素と置換 {f85e285d-a4e0-4152-9332-ab1d724d3325} を {e53f8fea-eae0-44a6-8774-ffd645390401} です。
10. 変更を保存し、プロジェクトを右クリックし、プロジェクトの再読み込みを選択します。
11. アプリケーションのルートの Web.config ファイル内に次の設定を追加、*アセンブリ*セクションです。 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. プロジェクトでは、ASP.NET MVC 2 を使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合は、強調表示されている、次を追加*bindingRedirect*下のアプリケーション ルートの Web.config ファイルに要素、 *構成*セクション。 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>ASP.NET MVC 3 での変更ツールを更新します。

このセクションでは、ASP.NET MVC 3 Tools Update リリースでは、ASP.NET MVC 3 RTM リリース以降の変更について説明します。

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>"コント ローラーの追加 ダイアログ ボックスがビューとデータのアクセス コードを含むコント ローラーをスキャフォールディングが可能

スキャフォールディングは、コント ローラーと、アプリケーションのビューをすばやく生成する方法です。 コードが生成された後をプロジェクトの要件を満たすように編集できます。

起動する、*コント ローラーの追加*ASP.NET MVC 3、ダイアログ ボックスを右クリックし、*コント ローラー*フォルダー*ソリューション エクスプ ローラー*をクリックして*追加*、クリックして*コント ローラー*です。 ダイアログ ボックスは、追加のスキャフォールディングのオプションを提供する拡張されています。

![](mvc3-release-notes/_static/image1.png)

次の 3 つスキャフォールディング テンプレートが使用可能な既定です。

#### <a name="empty-controller"></a>空のコント ローラー

このテンプレートは、空のコント ローラー ファイルを生成します。 このテンプレートは確認せずに相当*作成、編集、詳細のアクションを追加、削除のシナリオ*ASP.NET MVC の以前のバージョン。 この場合は、他のオプションはありません。

#### <a name="controller-with-empty-readwrite-actions"></a>空の読み取り/書き込みアクションのコント ローラー

このテンプレートは、メソッドのすべての必要なアクション メソッド コードではなく実装にコント ローラー ファイルを生成します。 このテンプレートは、することに相当*作成、編集、詳細のアクションを追加、削除のシナリオ*ASP.NET MVC の以前のバージョン。 この場合は、他のオプションはありません。

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>読み取り/書き込みアクションおよび Entity Framework を使用して、ビューとコント ローラー

このテンプレートでは、作業データ入力ユーザー インターフェイスをすばやく作成することができます。 一般的な要件と、次のように、シナリオの範囲を処理するコードが生成されます。

- *データ アクセス*です。 生成されたコードは、データベース内でエンティティを読み書きします。 既存のデータ コンテキスト クラスを選択した場合、または新しいを生成するテンプレートを使用する場合、Entity Framework Code First のアプローチと連携*DbContext*クラスです。 既存の選択した場合は、Entity Framework Database First または Model First アプローチででも動作*ObjectContext*クラスです。
- *検証*です。 生成されたコードは、モデル クラスで宣言されている規則に従ってフォームの送信が検証されるため、ASP.NET MVC モデル バインディング機能とメタデータ機能を使用します。 などの組み込みの検証規則が含まれます、*必要*と*StringLength*属性、およびカスタム検証規則。
- *一対多リレーションシップ*です。 モデル クラス間で一対多の外部キー リレーションシップを定義する場合、生成されたコードには関連エンティティを選択するためのドロップダウン リストが生成されます。 たとえば、次の Entity Framework Code First 規約に従って次のモデル クラスを定義する可能性があります。 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    コント ローラーをスキャフォールディングすると、*製品*クラス、そのビュー、ユーザーが選択、*カテゴリ*ごとにオブジェクト*製品*インスタンス。

    このテンプレートの追加のオプションを使用する、*コント ローラーの追加* ダイアログ ボックス。 *モデル クラス*データを作成または編集できるユーザーの種類を決定するソリューションでどのモデル クラスを選択できます。
- Entity Framework Code First を使用する場合は、どのモデル クラスを選択できます。
- Entity Framework Database First または Entity Framework Model First を使用している場合は、概念モデルで定義されているエンティティ クラスを選択することを確認します。

*データ コンテキスト クラス*、これらの選択を行うことができます。

- Code First を使用していない既存のデータ コンテキスト クラスを選択する場合*&lt;新しいデータ コンテキストしています.&gt;*". データ コンテキスト クラスは、生成されます。
- Code First を使用して既存のデータ コンテキスト クラスがある場合は、ここで選択します。 選択したモデル クラスを永続化に更新されます。
- Database First または Model First を使用している場合は、ここで、オブジェクト コンテキスト クラスを選択します。

ビューの場合、使用するビュー エンジンを選択するかのすべてのビューのスキャフォールディングしたくない場合は、[なし] を選択します。

選択できる、高度なさらに、生成されたビューのオプションを指定します。 たとえば、レイアウトまたはマスター ページを使用を選択できます。

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>機能強化、"ASP.NET MVC 3 プロジェクトを新しい" ダイアログ ボックス

新しい ASP.NET MVC 3 プロジェクトの作成に使用する ダイアログ ボックスには、次に示すように複数の機能強化が含まれます。

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>新しい「イントラネット プロジェクト」テンプレート

プロジェクト テンプレートの一覧には、新しいイントラネット アプリケーション テンプレートが含まれています。 このテンプレートには、フォーム認証ではなく Windows 認証を使用して web アプリケーションを構築するための設定が含まれています。 イントラネット アプリケーションには、プロジェクトのテンプレートにカプセル化することはできませんのある IIS 設定が必要であるため、テンプレートには、プロジェクト テンプレートを IIS で使用する方法についての記載された readme ファイルが含まれます。 ドキュメントを新しいイントラネット アプリケーション テンプレートは、次の URL で MSDN web サイトで使用できます。

[https://msdn.microsoft.com/en-us/library/gg703322 (VS.98).aspx](https://msdn.microsoft.com/en-us/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>プロジェクト テンプレートが HTML5 に対応

これで、新しいプロジェクト ダイアログ ボックスには、プロジェクト テンプレートに HTML5 固有の機能を追加するオプションが含まれています。 新しい HTML5 を含むビューを生成するオプションを選択すると、 *&lt;ヘッダー&gt;*、 *&lt;フッター&gt;*、および *&lt;ナビゲーション&gt;*要素。

以前のバージョンのブラウザーは HTML5 固有のタグをサポートしていませんことに注意してください。 この制限に対処するには、HTML5 プロジェクト テンプレートには、Modernizr ライブラリへの参照が含まれます。 (詳しくは、次のセクションを参照してください)。

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>プロジェクト テンプレートに Modernizr 1.7 が

Modernizr は、これらの機能をサポートしていないブラウザーで CSS 3 および HTML5 のサポートを有効にする JavaScript ライブラリです。 このライブラリは、ASP.NET MVC 3 プロジェクトのテンプレートでの事前インストールされている NuGet パッケージとして含まれています。 Modernizr の詳細については、次を参照してください。 [http://www.modernizr.com/](http://www.modernizr.com/)です。

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>プロジェクト テンプレートは、jQuery、jQuery UI、および jQuery の更新バージョンの検証

プロジェクト テンプレートには、次のバージョンの jQuery スクリプトが追加されました。

- jQuery 1.5.1
- jQuery 検証 1.8
- jQuery UI 1.8.11

これらのライブラリは、NuGet プレインストール パッケージとして含まれています。

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>プロジェクト テンプレートでは、事前インストールされている NuGet パッケージとして ADO.NET Entity Framework 4.1 が追加されました

ADO.NET Entity Framework 4.1 には、Code First 機能が含まれています。 コードは、既存のデータベースと Model First パターンに代えて使用できる ADO.NET Entity Framework 用の新しい開発パターンを最初です。

コードはまず、Visual Basic または c# で記述された POCO クラス ("plain old CLR object") を使用してモデルを定義するフォーカスしています。 これらのクラスは、既存のデータベースにマップすることができますか、データベース スキーマを生成するために使用します。 使用して追加の構成を指定することができます*DataAnnotations*属性または fluent Api を使用します。

コード Firstwith ASP.NET MVC の使用に関するドキュメントは、次の Url で ASP.NET web サイトで使用できます。

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める

プロジェクトがそれらをインストールすることで、JavaScript ファイルに記載されている (たとえば、Modernizr ライブラリ) を含む新しい ASP.NET MVC 3 プロジェクトを作成するときに、プロジェクト テンプレートで、Scripts フォルダーにスクリプトを直接追加する代わりに NuGet を使用します。内容。 これにより、NuGet を使用して、スクリプトの新しいバージョンがリリースされたときに、最新バージョンにスクリプトを更新することができます。

たとえば、jQuery の新しいリリースの頻度を指定するには、プロジェクト テンプレートに含まれる jQuery のバージョンがある時点で古くなってます。 ただし、jQuery はインストール済みの NuGet パッケージとして含まれているため、通知されます ダイアログ ボックスで NuGet jQuery の新しいバージョンが利用可能な場合です。

JQuery には、ファイル名にバージョン番号が含まれているため jQuery を最新バージョンに更新も更新が必要です、 *&lt;スクリプト&gt;*新しいファイル名を使用する、jQuery ファイルを参照しているタグ。 その他のスクリプト ライブラリでは、スクリプト名が、バージョン番号は含まれませんが最新バージョンをより簡単に更新されるようにします。

<a id="tu-KI"></a>
## <a name="known-issues"></a>既知の問題

- 場合によっては、インストールが失敗する、エラー メッセージ「のインストールに失敗しましたエラー コード (0x80070643)」です。 この問題を回避する方法については、次を参照してください。[サポート技術情報の記事 2531566](https://support.microsoft.com/kb/2531566)です。
- コント ローラーを追加するためのスキャフォールディングでは、Entity Framework 内のエンティティ継承サポートを利用するエンティティはスキャフォールディングされません。 など、基数を指定*Person*によって継承されるクラス、*学生*クラスをスキャフォールディング、*学生*クラスのコードはコンパイルされませんが生成されます。
- 原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。 回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper をインストールして ASP.NET MVC 3 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。
- Razor ビューを編集する場合 (.cshtml または*。vbhtml*ファイル) を表示します。 ASP.NET MVC 3 では、Razor ビューのスニペットは含まれません。スニペットを aspxselecting ASP.NET MVC 用のコード スニペットが表示されます。
- For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>ASP.NET MVC 3 RTM での変更

このセクションでは、変更とバグ修正が RC2 リリース後、ASP.NET MVC 3 RTM リリースで行われたについて説明します。

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>変更: 1.8.7 に jQuery UI のバージョンを更新します。

Visual Studio 用の ASP.NET MVC プロジェクト テンプレートは、jQuery UI ライブラリの最新バージョンを対象に更新されました。 テンプレートには、jQuery UI、関連付けられている CSS およびイメージ ファイルなどに必要なリソース ファイルの最小限のセットも含まれます。

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>変更: ModelMetadataProvider バックアップ DataAnnotationsModelMetadataProvider 既定を変更します。

RC2 リリースが導入された ASP.NET MVC 3 の*CachedDataAnnotationsMetadataProvider*指定すると、既存の上にキャッシュ クラス*DataAnnotationsModelMetadataProvider*クラスとして、パフォーマンスが向上します。 ただし、いくつかのバグが報告この実装でため、変更を元に戻すし、記載されている計画の MVC プロジェクトに移動[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)です。

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>修正: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける

ASP.NET MVC 3 のプレリリース版で Razor ファイルに空白を含む Razor 式の一部を貼り付けるときは、結果の式を逆にします。 たとえば、次の Razor コード ブロックがあるとします。

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

最初の方法でテキスト"最初 param"を選択してから 2 番目のメソッドに引数として貼り付けます、結果はとおりです。

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

正常に動作は、貼り付け操作は、次のことになります。

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

式が正しく貼り付け操作中に保持されるように、RTM のリリースでこの問題は修正されました。

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>修正: エディターで開かれている Razor ファイルの名前を変更して無効にして構文の色表示機能 IntelliSense

構文の強調表示し、そのファイルの動作を停止するための IntelliSense は、ファイルがエディター ウィンドウで開いているときに、ソリューション エクスプ ローラーを使用して、Razor ファイルの名前を変更します。 これは修正されました強調表示されるよう、IntelliSense は、名前の変更後に維持されます。

<a id="RTM-KI"></a>
## <a name="known-issues"></a>既知の問題

- 場合は、NuGet パッケージ マネージャー コンソールが開いているときに Visual Studio 2010 SP1 Beta を閉じると、Visual Studio がクラッシュし、再起動しようとしています。 これは、問題は修正する Visual Studio 2010 SP1 の RTM リリースでします。
- ASP.NET MVC 3 インストーラーは、初期のバージョンの NuGet package manager をインストールすることはのみです。 最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。 NuGet のインストールがある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。
- 原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。 回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。
- インストーラーは、完了に ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。 これは、Visual Studio 2010 のコンポーネントが更新されるためです。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper をインストールして ASP.NET MVC 3 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。
- ASP.NET MVC 3 のベータ版で作成した CCSHTML および VBHTML のビューには、ビルド アクションが正しく設定はありません、これらを表示する結果を含む型を省略すると、プロジェクトが発行されるときにします。 これらのファイルのビルド アクション値は、"Content"に設定する必要があります。 ASP.NET MVC 3 RTM はの新しいファイルには、この問題の修復がプレリリース版で作成されたプロジェクトの既存のファイルの設定を修正しません。
- ![](mvc3-release-notes/_static/image3.png)
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスが表示されます、ライセンス条項では、意図したものよりも小さいウィンドウを/li。&gt;
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。
- For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- ASP.NET MVC の以前のバージョンで 1 回のいくつかの場合を除く要求フィルターは、アクションを作成します。 この動作が保証された動作しますが、実装の詳細だけを実行されていないと、フィルターのコントラクトがステートレスを検討するでした。 ASP.NET MVC 3 では、フィルターは積極的にキャッシュされます。 したがって、不適切なインスタンスの状態を格納する任意のカスタム アクション フィルターは壊れている可能性があります。
- 例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。 ASP.NET MVC 2 以前のバージョンで同じであるコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されるため、アクション メソッドの値します。 例外フィルターが適用されるときに、大文字と小文字を一般にこのように、指定したせず*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラスです。 ASP.NET では、名前ではなく) パスを使用して、ビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。 これは、Web フォーム ビューのカスタムのファイル拡張子を有効にするために、カスタム ビルド プロバイダーが登録されているプロバイダーが名前ではなく、完全なパスを使用してそれらのビューを参照し、アプリケーションで重大な変更です。 回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。
- 直接実装するカスタム コント ローラー ファクトリの実装、 *IControllerFactory*インターフェイスは、新しい実装を提供する必要があります*GetControllerSessionBehavior*がメソッドこのリリースでは、インターフェイスに追加されます。 一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる*DefaultControllerFactory*です。

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 での変更

このセクションでは、RC のリリース以降、ASP.NET MVC 3 RC2 リリースで行われた変更 (新機能とバグ修正) について説明します。

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>1.4.4 jQuery、jQuery 検証 1.7 と jQuery UI 1.8.6 を含むようにプロジェクト テンプレートの変更

ASP.NET MVC 3 のプロジェクト テンプレートが加わりました最新バージョンの jQuery、jQuery 検証、および jQuery UI。 jQuery UI は、プロジェクト テンプレートに新たに追加し、便利なユーザー インターフェイスのウィジェットを提供します。 JQuery UI の詳細については、自分のホーム ページを参照してください: [http://jqueryui.com/](http://jqueryui.com/)です。

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>追加された"AdditionalMetadataAttribute"クラス

使用することができます、 *AdditionalMetadataAttribute*を設定するクラス、 *ModelMetadata.AdditionalValues*モデル プロパティのディクショナリ。

たとえば、ビュー モデルは、管理者にのみ表示されるプロパティを持ちます。 そのモデル AdminOnly をキーと true として次の例のように、値として使用して、新しい属性で注釈を付けることができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

製品ビュー モデルが表示される場合に、このメタデータは、表示またはエディターのテンプレートを使用可能になります。 メタデータ情報を解釈するアプリケーション開発者としてユーザーの責任です。

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>強化されたビューのスキャフォールディング

今すぐスキャフォールディング ビューで使用される T4 テンプレートなどのテンプレート ヘルパー メソッドの呼び出しの生成*EditorFor*などのヘルパーではなく*TextBoxFor*です。 この変更は、ビューの追加 ダイアログ ボックスのビューを生成するときに、データの注釈属性の形式でモデルのメタデータのサポートを向上します。

ビューのスキャフォールディングには、検出機能の向上および使用状況の規約に基づいて、モデルの主キーの情報も含まれています。 たとえば、ビューの追加 ダイアログ ボックスはこの情報を使用して、編集可能なフォームのフィールドとして主キーの値がないスキャフォールディングされたことを確認してください。

テンプレートの作成と編集を既定値は、クライアントの検証に必要な jQuery スクリプトへの参照を含めます。

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>追加された Html.Raw メソッド

既定では、Razor はすべての値を HTML エンコード エンジンを表示します。 次のコード スニペットがのあいさつの変数の内部 HTML をエンコードするとしてそのページに表示されるように、 &amp;lt; 厳密な&amp;gt;ハローワールド！&amp;lt;/strong&amp;gt;。

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

新しい*Html.Raw*メソッドは、念のために、コンテンツがわかっている場合は、エンコードされていない HTML を表示する簡単な方法を提供します。 次の例には、同じ文字列が表示されますが、文字列がマークアップとしてレンダリングされます。

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ

以前は、 *ViewModel*プロパティ*コント ローラー*に当てはめて考える、*ビュー*ビューのプロパティです。 これらのプロパティの両方の手段のアクセスの値、 *ViewDataDictionary*オブジェクトの動的なプロパティ アクセサーの構文を使用します。 両方のプロパティは、混乱を避けるためより一貫したものと同じである名前変更されています。

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>"SessionStateAttribute"に名前が変更された"ControllerSessionStateAttribute"クラス

*ControllerSessionStateAttribute*クラスは、ASP.NET MVC 3 RC のリリースで導入されました。 プロパティより簡潔なである名前が変更されました。

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>「ため」に名前を変更した RemoteAttribute"Fields"プロパティ

*RemoteAttribute*クラスの*フィールド*ユーザー間でのいくつかの混乱の原因となったプロパティ。 このプロパティの名前を変更する*ため*その目的を明確化します。

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更

*SkipRequestValidationAttribute*に属性が変更されました*AllowHtmlAttribute*用途をわかりやすく示したです。

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>最初の有用なエラー メッセージを表示して変更された"Html.ValidationMessage"メソッド

*Html.ValidationMessage*最初のエラーを表示するだけではなく最初の有用なエラー メッセージを表示するメソッドが修正されました。

モデル バインド中に、 *ModelState*ディクショナリは、モデル自体を含む、プロパティに関するエラー メッセージに複数のソースからデータを読み込むことができます (これを実装する場合*IValidatableObject*)、プロパティにアクセスするときにスローされた例外および検証属性のプロパティに適用されるからです。

ときに、 *Html.ValidationMessage*メソッドには、検証メッセージが表示されます、これらは一般的にないため、エンドユーザーに対して、例外が含まれているモデル状態エントリはスキップされます。 代わりに、メソッドは、例外に関連付けられていないと、そのメッセージが表示される最初の検証メッセージを探します。 このようなメッセージが見つからない場合の既定値は最初の例外に関連付けられている一般的なエラー メッセージです。

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>固定@model宣言、ドキュメントに空白を追加するには

以前のリリースで、  *@model* ビューの上部にある宣言は、レンダリングされた HTML 出力に空白行を追加します。 これは、宣言は空白を挿入していないように修正されました。

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>エンジンに固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ

ビュー エンジンは、次の例のように、明示的なビューのパスを使用してビューを返すことができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

最初のビュー エンジンは、常に、ビューをレンダリングしようとします。 既定では、Web フォーム ビュー エンジンは、最初のビュー エンジンです。Web フォーム エンジンは、Razor ビューをレンダリングできません、ためエラーが発生します。 ビュー エンジンのようになりましたが、 *FileExtensions*を使用するファイル拡張子を指定するプロパティをサポートします。 ASP.NET では、ビュー エンジンが、ファイルを表示できるかどうかが判断した場合、このプロパティがチェックされます。 これは、互換性に影響する変更点と、詳細についてに含まれる、[の重大な変更](#_Toc2_BC)このドキュメントの「します。

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>"For"属性の適切な値を出力する固定"LabelFor"ヘルパー

バグが修正されたは、where、 *LabelFor*レンダリング メソッド、*の*と一致する属性、*入力*要素の*名前*属性の代わりにその ID の W3C に従って、*の*属性に一致する必要があります、*入力*要素の id。

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド

以前のバージョンでは、明示的な値に渡された、 *RenderAction*メソッドは、子アクションの内部モデル バインド中に現在のフォーム値を優先するため無視されたされています。 解決策により、明示的な値がモデル バインド中に優先順位を取得します。

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- ASP.NET MVC の以前のバージョンでは、アクション フィルターは、いくつかの場合を除く要求ごと作成されました。 この動作が保証された動作しますが、実装の詳細だけを実行されていないと、フィルターのコントラクトがステートレスを検討するでした。 ASP.NET MVC 3 では、フィルターは積極的にキャッシュされます。 したがって、不適切なインスタンスの状態を格納する任意のカスタム アクション フィルターは壊れている可能性があります。
- 例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。 ASP.NET MVC 2 以前のバージョンで同じがあったコント ローラー上の例外がフィルター*順序*例外フィルター、アクション メソッドにする前に実行されたアクション メソッドのように値します。 例外フィルターが適用されたときに、大文字と小文字を一般にこのように、指定したせず*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラスです。 ASP.NET では、名前ではなく) パスを使用して、ビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。 これは、Web フォーム ビューのカスタムのファイル拡張子を有効にするために、カスタム ビルド プロバイダーが登録されているプロバイダーが名前ではなく、完全なパスを使用してそれらのビューを参照し、アプリケーションで重大な変更です。 回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。
- 直接実装するカスタム コント ローラー ファクトリの実装、 *IControllerFactory*インターフェイスは、新しい実装を提供する必要があります*GetControllerSessionBehavior* *このリリースでは、インターフェイスに追加されたメソッドに*です。 一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる*DefaultControllerFactory*です。

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>既知の問題

- ASP.NET MVC 3 インストーラーは、初期のバージョンの NuGet package manager をインストールすることはのみです。 最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。 NuGet のインストールがある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。
- 原因をソリューション フォルダー内の新しい ASP.NET MVC 3 プロジェクトを作成する、 *NullReferenceException*エラーです。 回避策は、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動します。
- インストーラーは、完了に ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。 これは、Visual Studio 2010 のコンポーネントが更新されるためです。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper をインストールして ASP.NET MVC 3 RC2 Razor IntelliSense サポートのメリットを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) Hadi Hariri のブログでは、現時点で併用する方法について説明します。
- ASP.NET MVC 3 のベータ版で作成した CSHTML および VBHTML のビューには、ビルド アクションが正しく設定はありません、これらを表示する結果を含む型を省略すると、プロジェクトが発行されるときにします。 *ビルド アクション*コンテンツにこれらのファイルを設定する必要がありますの値"です。 ASP.NET MVC 3 RC2 では、新しいファイルの場合は、この問題を修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。![](mvc3-release-notes/_static/image4.png)
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。
- For Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターに ASP.NET MVC 3 をインストールし、後で Visual Studio をインストールした場合、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードされるコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。
- ASP.NET MVC 3 RC 2 のインストールを更新しません NuGet 既にインストールされていること。 NuGet をアップグレードするには、Visual Studio 拡張機能マネージャーに移動する必要がありますとして表示、利用可能な更新します。 そこから最新のリリースには、NuGet をアップグレードできます。

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 のリリース候補

ASP.NET MVC リリース候補は、2010 年 11 月 9 日にリリースされました。

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 RC の新機能

このセクションの内容が導入された機能について説明します、ベータ リリース以降、ASP.NET MVC 3 RC のリリースでします。

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet パッケージ マネージャー

ASP.NET MVC 3 では、NuGet Package Manager (旧称 NuPack)、これはライブラリおよびツールを Visual Studio プロジェクトに追加するための統合パッケージ管理ツールが含まれます。 このツールは、開発者は、ソース ツリーにライブラリを取得する現在ための手順を自動化します。

コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと PowerShell コマンドレットのセットは、NuGet を使用することができます。

NuGet の詳細については、読み取り、 [Nuget のドキュメント](https://docs.microsoft.com/nuget/)です。

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>向上""新しいプロジェクト ダイアログ ボックス

新しいプロジェクトを作成するときに新しいプロジェクト ダイアログ ボックスでできるようになりました、ビュー エンジンと ASP.NET MVC プロジェクトの種類を指定します。

![](mvc3-release-notes/_static/image5.png)

このリリースでは、テンプレートとエンジンがダイアログ ボックスに表示するビューの一覧を変更するためのサポートが含まれます。

既定のテンプレートは次のとおりです。

空です。 最小の既定のディレクトリ構造 ASP.NET MVC プロジェクトの既定の ASP.NET MVC は、次のスタイル、および既定の JavaScript ファイルを含むスクリプト ディレクトリを含む Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイル セットが含まれています。

インターネット アプリケーションです。 ASP.NET MVC でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。

ダイアログ ボックスに表示されているプロジェクト テンプレートの一覧は、Windows レジストリで指定されます。

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sessionless コント ローラー

新しい*ControllerSessionStateAttribute*うえでセッション状態の動作をより細かく制御コント ローラーを指定して、 [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/en-us/library/system.web.sessionstate.sessionstatebehavior.aspx)列挙値。

次の例では、コント ローラーへのすべての要求のセッション状態をオフにする方法を示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

次の例では、コント ローラーにすべての要求に対する読み取り専用のセッション状態を設定する方法を示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>新しい検証属性

#### <a name="compareattribute"></a>CompareAttribute

新しい*CompareAttribute*検証属性を使用して、モデルの 2 つの異なるプロパティの値を比較できます。 次の例で、 *ComparePassword*プロパティに一致する必要があります、*パスワード*有効にするのにはフィールドです。

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

新しい*RemoteAttribute*メソッドを呼び出して、実際の検証ロジックを実行するサーバー上のクライアント側検証を有効、jQuery 検証プラグインのリモート検証の検証属性を活用します。

次の例で、 *UserName*プロパティには、 *RemoteAttribute*適用します。 クライアントの検証がという名前のアクションを呼び出して、編集ビューでは、このプロパティを編集するには、 *UserNameAvailable*上、 *UsersController*このフィールドを検証するためにクラスです。

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

次の例は、対応するコント ローラーを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

既定では、属性を適用するプロパティの名前は、クエリ文字列パラメーターとしてアクション メソッドに送信されます。

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード

新しいオーバー ロードが追加されて、 *LabelFor*と*LabelForModel*するのに便利な方法は、ラベルのテキストを指定します。 次の例では、これらのオーバー ロードを使用する方法を示します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>子アクション出力キャッシュ

*OutputCacheAttribute*を使用して呼び出す子アクションの出力がキャッシュをサポートしている、 *Html.RenderAction*または*Html.Action*ヘルパー メソッドです。 次の例では、別のアクションを呼び出すビューを示します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate*アクションの注釈が付いて、 *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

このコードを実行すると Html.Action("GetDate") への呼び出しの結果が 100 秒間キャッシュされます。

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"ビューの追加 ダイアログ ボックスの機能強化

厳密に型指定されたビューを追加するときに、ビューの追加 ダイアログ ボックスはよりも多くの中核となる .NET Framework 型などの以前のリリースで該当しない種類の詳細を今すぐフィルター処理します。 また、一覧は、クラス名と種類を検索しやすくなる完全修飾型名ではなくは今すぐ並べ替えられます。 たとえば、型名は、次の例のようにが表示されます。

クラス名 (名前空間)

以前のリリースでこのよう表示されている次のよう。

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>詳細な要求の検証

*除外*プロパティ*ValidateInputAttribute*は存在しません。 モデル バインド中に、モデルの特定のプロパティをスキップする要求の検証には、代わりに、新しい*SkipRequestValidationAttribute*です。

たとえば、アクション メソッドは、ブログの投稿を編集するために使用します。

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

次の例では、ブログの投稿のビュー モデルを示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

ユーザーは、Description プロパティの一部のマークアップを送信するときにモデル バインドは要求の検証のため失敗します。 ブログの投稿の説明のモデル バインド中に要求の検証を無効にするには、適用、 *SkipRequpestValidationAttribute*プロパティのこの例のように: です。

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

また、モデルのすべてのプロパティ用の要求の検証をオフに、次のように適用します。 *ValidateInputAttribute*の値を持つ*false*アクション メソッドに。

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- 例外フィルターが同じであるため、例外フィルターの実行の順序が変更された*順序*値。 ASP.NET MVC 2 以前のバージョンで同じがあったコント ローラー上の例外がフィルター*順序*例外フィルター、アクション メソッドにする前に実行されたアクション メソッド上のものとします。 例外フィルターが適用されたときに、大文字と小文字を一般にこのように、指定したせず*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という名前の新しいプロパティを追加*FileExtensions*を*VirtualPathProviderViewEngine*基本クラスです。 検索時に、ビュー パスを使用して、名前ではなく) に含まれているファイル拡張子を持つビューのみ、この新しいプロパティで指定されたリストと見なされます。 これは、web フォーム ビューのカスタムのファイル拡張子を有効にするプロバイダーを構築するカスタムを登録するユーザーの重大な変更と名前ではなく、完全なパスを使用してそれらのビューを参照しているとします。 回避策がの値を変更するには、 *FileExtensions*プロパティにカスタムのファイル拡張子を含めます。

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>既知の問題

- インストーラーは、Visual Studio 2010 のコンポーネントを更新するために完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。
- ビューのスキャフォールディング astrongly を選択するときに、スキャフォールディング書き込み専用プロパティの表示を入力します。 これらは、スキャフォールディングによって常に無視する必要があります。 ビューの追加 ダイアログもスキャフォールディング読み取り専用プロパティを「編集」または「作成」ビューを生成するときにします。 読み取り専用プロパティは、表示とリスト ビューのスキャフォールディングされたのみ必要があります。
- ASP.NET MVC 3 は、非同期 CTP と共にインストールされるときに、デバッグは機能しません。 ASP.NET MVC 3 では、非同期 CTP を並列でインストールされているをすることはできません。 デバッグを修復する非同期 CTP をアンインストールします。 詳細については、読み取り[このブログの投稿](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)について ASP.NET MVC 3 RC のすべての部分をアンインストールします。
- Razor Intellisense では、Resharper がインストールされている場合は機能しません。 ReSharper をインストールして ASP.NET MVC 3 RC をお読みください Razor intellisense サポートのメリットを利用する[このブログの投稿](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)JetBrains 現時点で併用する方法を説明するからです。
- ASP.NET MVC 3 のベータ版で作成した CSHTML および VBHTML のビューはありませんビルド アクションが正しくパブリッシュの対象から除外しています。 *ビルド アクション*これらのファイルは、"Content"に設定する必要があります。 ASP.NET MVC 3 RC では、新しいファイルには、この問題を修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。
- インストーラーは、Visual Studio 2010 のコンポーネントを更新するために完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。
- ビューの追加のスキャフォールディング強く"Edit"を選択すると、ビューのスキャフォールディングを型指定されたときに読み取り専用プロパティを指定します。 同様に、書き込み専用プロパティは「表示」ビューのスキャフォールディングされました。
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。
- インストール、 [Visual Studio Async CTP](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=18712f38-fcd2-4e9f-9028-8373dc5732b2&amp;displaylang=en)リリースでは、Razor ツールをインストール、ASP.NET MVC 3 の一部として含まれている競合が発生します。 同じコンピューターに Visual Studio Async CTP と Razor リリースの両方をインストールしてくださいいないを確認します。
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動するメニュー項目は使用できない、コード スニペットはありません。

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 のベータ版

ASP.NET MVC 3 のベータ版は、2010 年 10 月 6 日にリリースされました。 次の注意事項は、ベータ リリースに固有対象となります更新や変更前の ASP.NET MVC 3 のリリース候補セクションで参照されています。

## <a id="0.1__Toc274034215"></a>新しい Featuresin ASP.NET MVC 3 のベータ版

<a id="0.1__Default_validation_system"></a>このセクションの内容が導入された機能について説明します、ASP.NET MVC 3 のベータ リリースでします。

### <a id="0.1__Toc274034216"></a>NuGet パッケージ マネージャー

ASP.NET MVC 3 では、NuGet パッケージ マネージャーは、追加するライブラリ用の統合パッケージ管理ツールおよび Visual Studio プロジェクトにツールが含まれます。 ほとんどの場合、開発者は、ソース ツリーにライブラリを取得する現在ための手順を自動化します。

コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと PowerShell コマンドレットのセットは、NuGet を使用することができます。

NuGet の詳細については、読み取り、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)です。

### <a id="0.1__Toc274034217"></a>[新しいプロジェクト] ダイアログ ボックスの向上

新しいプロジェクトを作成するときに新しいプロジェクト ダイアログ ボックスでできるようになりました、ビュー エンジンと ASP.NET MVC プロジェクトの種類を指定します。

![](mvc3-release-notes/_static/image6.png)

このリリースでは、テンプレートとエンジンがダイアログ ボックスに表示するビューの一覧を変更するためのサポートが含まれていません。

既定のテンプレートは次のとおりです。

空です。 最小の既定のディレクトリ構造 ASP.NET MVC プロジェクトの既定の ASP.NET MVC は、次のスタイル、および既定の JavaScript ファイルを含むスクリプト ディレクトリを含む小さな Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイル セットが含まれています。

インターネット アプリケーションです。 ASP.NET MVC でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。

### <a id="0.1__Toc274034218"></a>厳密に指定することは簡略化された Razor ビューでのモデルの入力

新しいを使用して、厳密に型指定の Razor ビューのモデルの型を指定する方法を簡略化された@modelCSHTML ビューのディレクティブと@ModelTypeディレクティブ VBHTML ビューです。 ASP.NET MVC の以前のバージョンでは、Razor の厳密に型指定されたモデルは、この方法を表示を指定します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

このリリースでは、次の構文を使用できます。

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>新しい ASP.NET Web ページのヘルパー メソッドをサポートします。

新しい ASP.NET Web Pages テクノロジには、一連ビューとコント ローラーに一般的に使用される機能を追加するための便利なヘルパー メソッドにはが含まれています。 ASP.NET MVC 3 では、コント ローラーとビュー内でこれらのヘルパー メソッドを使用して、(必要な場合) をサポートします。 これらのメソッドは、System.Web.Helpers アセンブリに格納されます。 次の表は、ASP.NET Web Pages のヘルパー メソッドのいくつかを示します。

| **ヘルパー** | **説明** |
| --- | --- |
| グラフ | ビュー内のグラフを表示します。 Chart.ToWebImage、Chart.Save、Chart.Write などのメソッドが含まれています。 |
| Crypto | ハッシュを正しく作成するアルゴリズムを使用では、ソルトし、パスワードをハッシュします。 |
| WebGrid | オブジェクト (通常は、データベースからのデータ) のコレクションをグリッドとして表示します。 ページングや並べ替えをサポートします。 |
| WebImage | イメージを表示します。 |
| WebMail | 電子メール メッセージを送信します。 |

ヘルパーと基本的な構文を示すクイック リファレンス トピックでは、使用可能なドキュメントの一部として、ASP.NET Razor 構文で、次の URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>追加の依存関係の挿入のサポート

ASP.NET MVC 3 Preview 1 のリリースでの構築、現在のリリースには、2 つの新しいサービスや既存の 4 つのサービスのサポートを追加および依存関係の解決と共通サービス ロケーターのサポートの改善が含まれます。

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>粒度の細かいコント ローラー インスタンスを作成して新しい IControllerActivator インターフェイス

新しい IControllerActivator インターフェイスは、依存関係の挿入を使用してコント ローラーがインスタンス化する方法より詳細に制御を提供します。 次の例は、インターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

これは、コント ローラー ファクトリの役割です。 コント ローラー ファクトリは、コント ローラーの種類を検索するためとそのコント ローラーの種類のインスタンスをインスタンス化するための両方を担当する IControllerFactory インターフェイスの実装です。

コント ローラー アクティベーターは担当のみコント ローラーの種類のインスタンスをインスタンス化します。 コント ローラーの種類の参照は行いません。 適切なコント ローラーの種類を特定すると、コント ローラーの実際のインスタンス化を処理するコント ローラー ファクトリ IControllerActivator のインスタンスに委任する必要があります。

DefaultControllerFactory クラスには、新しい IControllerFactory インスタンスを受け取るコンス トラクターがあります。 これにより、既定のコント ローラーの種類のルックアップ動作をオーバーライドすることがなくコント ローラー作成のこの面を管理する依存関係の挿入を適用できます。

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IDependencyResolver を置き換え IServiceLocator インターフェイス

コミュニティからのフィードバックに基づき、ASP.NET MVC 3 のベータ リリースは置き換え IServiceLocator インターフェイスの使用する ASP.NET MVC のニーズに応じた取り除か IDependencyResolver インターフェイス。 次の例は、新しいインターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

この変更の一環として、ServiceLocator クラスも置き換えられた DependencyResolver クラスです。 依存関係競合回避モジュールの登録は、ASP.NET MVC の以前のバージョンと同様です。

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

このインターフェイスの実装は、要求された型に対して登録されているサービスを提供する基になる依存性の注入コンテナーに委任だけ必要があります。

要求された型の登録済みのサービスがない場合は、ASP.NET MVC は GetService から null を返すと、GetServices から空のコレクションを返すには、このインターフェイスの実装が必要です。

新しい DependencyResolver クラスを使用して、新しい IDependencyResolver インターフェイスまたは共通サービス ロケーターのインターフェイス (IServiceLocator) のいずれかを実装するクラスを登録できます。 共通サービス ロケーターの詳細については、次を参照してください。 [github CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)です。

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>粒度の細かいビュー ページ インスタンスを作成して新しい IViewActivator インターフェイス

新しい IViewPageActivator インターフェイスは、依存関係の挿入を使用してページの表示がインスタンス化する方法より詳細に制御を提供します。 これは、WebFormView インスタンスと RazorView インスタンスの両方に適用されます。 次の例は、新しいインターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

これらのクラスを受け付けることができます、ViewPage、ViewUserControl、および WebViewPage 型がインスタンス化する方法を制御する依存関係の挿入を使用する、IViewPageActivator コンス トラクター引数。

#### <a name="new-dependency-resolver-support-for-existing-services"></a>既存のサービスの新しい依存関係競合回避モジュールのサポート

新しいリリースには、次のサービスの依存関係の解像度のサポートが含まれています。

- モデル検証プロバイダー。 ModelValidatorProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムがクライアント側とサーバー側の検証をサポートするために使用されます。
- モデル メタデータ プロバイダー。 ModelMetadataProvider を実装する 1 つのクラスは、依存関係競合回避モジュールに登録することができ、システムを使用すると、テンプレートおよび検証システムのメタデータを提供する使用されます。
- 値プロバイダー。 ValueProviderFactory を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムは、コント ローラーで、モデル バインド中に使用される値プロバイダーを作成することを使用します。
- モデル バインダー。 IModelBinderProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが使用して作成するモデル バインド システムで使用されるモデル バインダー。

### <a id="0.1__Toc274034221"></a>控えめな jQuery ベース Ajax のサポート

ASP.NET MVC には、次のよう Ajax ヘルパー メソッドが含まれます。

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

これらのメソッドでは、JavaScript を使用して、完全なポストバックを使用するのではなく、サーバーでアクション メソッドを呼び出します。 この機能は利用するために jQuery の控えめな方法で更新されました。 インライン クライアント スクリプトをそのままの状態生成ではなくこれらのヘルパー メソッドの動作から分離マークアップを使用して HTML5 属性を生成することによって、*データ ajax*プレフィックス。 マークアップには、動作は、適切な JavaScript ファイルを参照することによって適用されます。 次の JavaScript ファイルが参照されていることを確認します。

- jquery 1.4.1.js
- jquery.unobtrusive.ajax.js

この機能は、ASP.NET MVC 3 プロジェクト、新しいテンプレートにある Web.config ファイルで既定で有効が、既存のプロジェクトに既定で無効にします。 詳細については、次を参照してください。[クライアント検証と控えめな JavaScript アプリケーション全体のフラグを追加](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。

### <a id="0.1__Toc274034222"></a>控えめな jQuery 検証用の新しいサポート

既定では、ASP.NET MVC 3 のベータ版は、クライアント側の検証を実行するために、控えめな方法で jQuery 検証を使用します。 控えめなクライアント検証を有効にするには、ビュー内から次のような呼び出しを行います。

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

これは、ViewContext.UnobtrusiveJavaScriptEnabled プロパティが true で、次の呼び出しを行うことによって行うことができますに設定されている必要があります。

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

また、次の JavaScript ファイルが参照されていることを確認してください。

- jquery 1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

この機能は、ASP.NET MVC 3 プロジェクト、新しいテンプレートにある Web.config ファイルでは既定で有効になっているが、既存のプロジェクトに既定で無効にします。 詳細については、次を参照してください。[クライアント検証と控えめな JavaScript アプリケーション全体のフラグを新しい](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ

有効にするか、クライアント検証と控えめな JavaScript が次の例のように、HtmlHelper クラスの静的メンバーを使用してグローバルに無効にすることができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

既定のプロジェクト テンプレートは、既定では、控えめな JavaScript を有効にします。 有効にしたり、次の設定を使用して、アプリケーションのルートの Web.config ファイルでこれらの機能を無効にしたりできます。

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

これらの機能を有効にするには既定ではため、次の例に示すように、既定の設定を上書きすることが HtmlHelper クラスへの新しいオーバー ロードの導入します。

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

旧バージョンとの互換性のため、既定ではこれら両方の機能は無効です。

### <a id="0.1__Toc274034224"></a>ビューの実行前に実行されるコードのサポート

という名前のファイルを配置できるようになりました\_viewstart.cshtml (または\_viewstart.vbhtml) Views ディレクトリにし、そのディレクトリとそのサブディレクトリに複数のビューの間で共有されるをコードを追加します。 次のコードを配置するなど、 \_viewstart.cshtml フォルダー ページに、~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

これは、Views フォルダー内のすべてのビューとすべてのサブフォルダーを再帰的にレイアウト ページを設定します。 ときに、ビュー、表示されるコード、 \_viewstart.cshtml ファイルはコードの表示を実行する前に実行されます。 \_Viewstart.cshtml コードは、そのフォルダー内のすべてのビューには適用されます。

既定では、コード、 \_viewstart.cshtml ファイルは、任意のサブフォルダー内のビューにも適用されます。 ただし、個別のサブフォルダーは、独自のバージョンを持つことができます、 \_viewstart.cshtml ファイルです。 その場合は、ローカル バージョンが優先されます。 たとえば、HomeController のすべてのビューに共通するコードを実行するには、次のように配置します。、 \_~/Views/Home フォルダーにある viewstart.cshtml ファイル。

### <a id="0.1__Toc274034225"></a>VBHTML Razor 構文の新しいサポート

ASP.NET MVC の以前のプレビューには、c# に基づいた Razor 構文を使用して、ビューのサポートが含まれています。 これらのビューは、.cshtml ファイルの拡張子を使用します。 Razor をサポートするために継続的な作業の一環として、ASP.NET MVC 3 のベータ版には、Razor 構文 .vbhtml ファイル拡張子を使用する Visual Basic でのサポートが導入されています。

VBHTML ページで Visual Basic 構文の使用の概要については、次の URL でチュートリアルを参照してください。

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>ValidateInputAttribute をより細かく制御

ASP.NET MVC には、受信要求に悪意のある入力が含まれていないことを確認してください ASP.NET 要求の検証のコア インフラストラクチャを呼び出す ValidateInputAttribute クラスが常に含まれます。 既定では、入力の検証を有効にします。 次の例のように、ValidateInputAttribute 属性を使用して要求の検証を無効にできます。

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

ただし、多くの web アプリケーションでは、その他のフィールドはいけない中に、HTML を使用する必要がある個々 のフォーム フィールドがあります。 ValidateInputAttribute クラスでは、要求の検証に含める必要があるないフィールドの一覧を指定できます。

たとえば、ブログ エンジンを開発している場合、フィールドの本文および概要のマークアップを許可する可能性があります。 これらのフィールドは、各プロパティの名前 ("Body"および「概要」) に対応する名前の属性を持つ 2 つの入力要素によって表現されます。 これらのフィールドの検証には、要求を無効にするには、(コンマ区切り) ValidateInput クラス、例を次の除外プロパティの名前のみを指定します。

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。

ヘルパー メソッドを使用して、次の例のように、匿名オブジェクトを使用して属性の名前/値ペアを指定できます。

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

このアプローチしないを使用できます、属性名にハイフンを使用するため、ASP.NET のプロパティ名にハイフンは使用できません。 ただし、HTML5 属性のカスタム重要なのハイフンたとえば、HTML5 では、「データの」プレフィックスが使用されます。

同時にアンダー スコアは HTML では、属性名は使用できませんが、プロパティ名で有効です。 そのため、匿名オブジェクトを使用して属性を指定して、属性名がアンダー スコアを含める場合は、およびに、ヘルパー メソッドは、アンダー スコアをハイフンに変換されます。 たとえば、次のヘルパー構文は、アンダー スコアを使用します。

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

ヘルパーの実行時に、前の例は、次のマークアップを表示します。

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>バグの修正

EditorFor と DisplayFor テンプレート ヘルパーの既定のオブジェクト テンプレートは、DisplayAttribute.Order プロパティで指定された順序付けできるようになりました。 (以前のバージョンで順序の設定が使用されません。)

現在、クライアントの検証には、検証属性が適用されているオーバーライドされたプロパティの検証がサポートされています。

JsonValueProviderFactory は既定で登録されているようになりました。

## <a id="0.1__Toc274034229"></a>重大な変更

例外フィルターの実行順序を同じ順序の値を持つ例外フィルターが変更されました。 ASP.NET MVC 2 以前のバージョンで上の例外フィルターと同じ順序で、コント ローラー アクション メソッドで例外フィルターの前に実行されたアクション メソッド上のものとします。 指定された順序値を指定せずに例外フィルターが適用されたときに、大文字と小文字を一般にこのようにします。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと Order プロパティが明示的に指定されている場合、フィルターが指定した順序で実行されます。

## <a id="0.1__Toc274034230"></a>既知の問題

インストール中に、使用許諾契約書への同意 ダイアログ ボックスでは、ものよりも小さいウィンドウをライセンス条項が表示されます。

Razor ビューには、IntelliSense のサポートも構文の強調表示はありません。 Visual Studio での Razor 構文のサポートが以降のリリースの一部として含めるなると予想されます。

Razor ビュー (CSHTML ファイル) を編集するとき、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Visual Studio でのコント ローラーに移動するメニュー項目は使用できないし、コード スニペットはありません。

使用する場合、 @model CSHTML を厳密に型を指定する構文を表示、言語固有のショートカットの種類は認識されません。 たとえば、 @model int は機能しませんが、 @model Int32 は機能します。 このバグの回避策では、モデルの種類を指定する場合は、実際の型名を使用します。

使用する場合、 @model CSHTML ビューを厳密に型を指定するための構文 (または@ModelTypeVBHTML ビューを厳密に型を指定)、null 許容型や配列の宣言はサポートされていません。 たとえば、 @model int? はサポートされていません。 代わりに、 @model Nullable&lt;Int32&gt;です。 構文@modelstring[] もサポートされていません; 代わりに、 @model IList&lt;文字列&gt;です。

ASP.NET MVC 3、ASP.NET MVC 2 プロジェクトをアップグレードするときに、Web.config ファイルの appSettings セクションに、次を追加することを確認してください。

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

常に認証されていないユーザーにリダイレクト ~/Account/ログイン、Web.config で使用されるフォーム認証設定を無視してフォーム認証の原因となる既知の問題があります。回避策では、次のアプリケーション設定を追加します。

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>免責事項

© 2011 Microsoft Corporation. All rights reserved. このドキュメントは提供される"としてでは、します"。 情報および見解 URL および他のインターネット Web サイトの参照を含む、このドキュメントでは、通知なく変更可能性があります。 お客様は、その使用に関するリスクを負うものとします。

このドキュメントは、Microsoft 製品の知的財産権に関する法的な権利をお客様に許諾するものではありません。 内部的な参照目的に限り、このドキュメントを複製して使用することができます。
