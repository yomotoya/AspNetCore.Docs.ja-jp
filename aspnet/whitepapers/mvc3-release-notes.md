---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 |Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: e6ad37d51e0475631314c1110f13fe8aef2f954c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831071"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [概要](#overview)
- [インストールに関する注記](#installation-notes)
- [ソフトウェアの要件](#software-requirements)
- [ドキュメント](#documentation)
- [サポート](#support)
- [ASP.NET mvc、ASP.NET MVC 2 プロジェクトをアップグレードする 3 つのツールの更新します。](#upgrading)
- [ASP.NET MVC 3 Tools Update (2011 年 4 月 12 日)](#tu-changes)

    - ["コント ローラーの追加 ダイアログ ボックスは、ビューとデータ アクセス コードを使用してコント ローラーをスキャフォールディングできますようになりました](#tu-AddControllerDialog)
    - [機能強化、"ASP.NET MVC 3 新しいプロジェクト ダイアログ ボックス](#tu-ImprovementsNewDialogBox)
    - [プロジェクト テンプレートに Modernizr 1.7 が追加されました](#tu-Modernizr)
    - [プロジェクト テンプレートは、更新されたバージョンの jQuery、jQuery UI、および jQuery の検証](#tu-UpdatedJQuery)
    - [プロジェクト テンプレートでは、NuGet プレインストール パッケージとして ADO.NET Entity Framework 4.1 が追加されました](#tu-EF)
    - [プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める](#tu-JavaScriptLibsNuget)
    - [既知の問題](#tu-KI)
- [ASP.NET MVC 3 RTM (2011 年 1 月 13 日)](#MVC3RTM)

    - [変更: 1.8.7 に jQuery UI のバージョンを更新します。](#RTM-1)
    - [変更: DataAnnotationsModelMetadataProvider を ModelMetadataProvider 既定値を変更します。](#RTM-2)
    - [修正済み: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける](#RTM-3)
    - [修正済み: エディターで開かれている Razor ファイル名の変更を無効にします構文の色付けと IntelliSense](#RTM-4)
    - [既知の問題](#RTM-KI)
    - [重大な変更](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (2010 年 12 月 10 日)](#_Toc2)

    - [JQuery 1.4.4、jQuery 検証 1.7、jQuery UI 1.8.6y UI 1.8.6 など、プロジェクト テンプレートの変更](#_Toc2_1)
    - [追加された"AdditionalMetadataAttribute"クラス](#_Toc2_2)
    - [強化されたビューのスキャフォールディング](#_Toc2_3)
    - [追加された Html.Raw メソッド](#_Toc2_3)
    - [名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ](#_Toc2_4)
    - [名前が変更された"ControllerSessionStateAttribute"クラス"SessionStateAttribute"を](#_Toc2_5)
    - [「ため」に名前を変更した RemoteAttribute"Fields"プロパティ](#_Toc2_6)
    - ["AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更します。](#_Toc2_7)
    - [最初に、役立つエラー メッセージを表示する変更された"Html.ValidationMessage"メソッド](#_Toc2_8)
    - [固定@model空白の文書に追加の宣言](#_Toc2_9)
    - [エンジン固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ](#_Toc2_10)
    - [固定"LabelFor"ヘルパー"For"属性の正しい値を出力するには](#_Toc2_11)
    - [モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド](#_Toc2_12)
    - [重大な変更](#_Toc2_BC)
    - [既知の問題](#_Toc2_KI)
- [ASP.NET MVC 3 リリース候補 (2010 年 11 月 9 日)](#TOC_ASP_NET_3_RC)

    - [ASP.NET MVC 3 RC の新機能](#_Toc276711785)
    - [NuGet パッケージ マネージャー](#_Toc276711786)
    - [新しいプロジェクト の改善 ダイアログ ボックス](#_Toc276711787)
    - [Sessionless コント ローラー](#_Toc276711788)
    - [新しい検証属性](#_Toc276711789)
    - ["LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード](#_Toc276711790)
    - [子アクションの出力キャッシュ](#_Toc276711791)
    - ["ビューの追加 ダイアログ ボックスの機能強化](#_Toc276711792)
    - [詳細な要求の検証](#_Toc276711793)
    - [重大な変更](#_Toc276711794)
    - [既知の問題](#_Toc276711795)
- [ASP します。MVC 3 Beta ノート (2010 年 10 月 6 日)](#TOC_ASP_NET_3_Beta)

    - [ASP.NET MVC 3 のベータ版の新機能](#0.1__Toc274034215)
    - [NuPack パッケージ マネージャー](#0.1__Toc274034216)
    - [強化された新しいプロジェクト ダイアログ ボックス](#0.1__Toc274034217)
    - [厳密に指定する方法を簡略化された Razor ビューで型指定されたモデル](#0.1__Toc274034218)
    - [新しい ASP.NET Web ページのヘルパー メソッドをサポートします。](#0.1__Toc274034219)
    - [追加の依存関係挿入をサポート](#0.1__Toc274034220)
    - [控えめな jQuery ベースの Ajax のサポート](#0.1__Toc274034221)
    - [控えめな jQuery 検証のサポート](#0.1__Toc274034222)
    - [クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ](#0.1__Toc274034223)
    - [ビューの実行前に実行されるコードの新しいサポート](#0.1__Toc274034224)
    - [VBHTML Razor 構文のサポート](#0.1__Toc274034225)
    - [ValidateInputAttribute をより細かく制御](#0.1__Toc274034226)
    - [ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。](#0.1__Toc274034227)
    - [バグの修正](#0.1__Toc274034228)
    - [重大な変更](#0.1__Toc274034229)
    - [既知の問題](#0.1__Toc274034230)
- [免責事項](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>概要

このドキュメントでは、Visual Studio 2010 用の ASP.NET MVC 3 RTM のリリースについて説明します。 ASP.NET MVC は、モデル-ビュー-コント ローラー (MVC) パターンを使用して、Web アプリケーションを開発するためのフレームワークです。 ASP.NET MVC 3 インストーラーには、次のコンポーネントが含まれています。

- ASP.NET MVC 3 ランタイム コンポーネント
- ASP.NET MVC 3 Visual Studio 2010 ツール
- ASP.NET Web ページの実行時コンポーネント
- ASP.NET Web ページ Visual Studio 2010 ツール
- Microsoft .NET (NuGet) 用のパッケージ マネージャー
- Razor 構文のサポートを有効にする Visual Studio 2010 用の更新プログラム。 (詳細は、サポート技術情報記事 2483190 を参照してください)。

ASP.NET MVC 3 の各リリース前のバージョンのリリース ノートの完全なセットは、次の URL で ASP.NET web サイトにあります。

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>インストールに関する注記

ASP.NET MVC 3 RTM、Web Platform Installer (Web PI) を使用してをインストールするには、次のページを参照してください。

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

または、次のページから Visual Studio 2010 の ASP.NET MVC 3 rtm インストーラーをダウンロードできます。

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 をインストールできるし、サイド バイ サイドでを実行できる ASP.NET MVC 2 とします。

<a id="software-requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET MVC 3 ランタイム コンポーネントには、次のソフトウェアが必要です。

- .NET framework version 4。 

    ASP.NET MVC 3 Visual Studio 2010 ツールには、次のソフトウェアが必要です。
- Visual Studio 2010 または Visual Web Developer 2010 Express。

<a id="documentation"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC のドキュメントは、次の URL で MSDN Web サイトで入手できます。

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

チュートリアルと ASP.NET MVC に関する他の情報は、次の URL で ASP.NET Web サイトの MVC ページ入手できます。

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>サポート

これは、完全にサポートされているリリースです。 テクニカル サポートの利用に関する情報をご覧、 [Microsoft サポート web サイト](https://support.microsoft.com/)します。

自由に、ASP.NET コミュニティのメンバーが非公式のサポートを提供することが頻繁に、ASP.NET MVC フォーラムへのこのリリースに関する質問を投稿します。

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>ASP.NET mvc、ASP.NET MVC 2 プロジェクトをアップグレードする 3 つのツールの更新します。

ASP.NET MVC 3 は、ASP.NET MVC 2 と並行 ASP.NET MVC 3、ASP.NET MVC 2 アプリケーションをアップグレードするタイミングを選択できる柔軟性を提供する、同じコンピューターにインストールできます。

バージョン 3 に既存の ASP.NET MVC 2 アプリケーションを手動でアップグレードするには、次の操作を行います。

1. コンピューターに新しい空の ASP.NET MVC 3 プロジェクトを作成します。 このプロジェクトは、アップグレードに必要ないくつかのファイルが格納されます。
2. ASP.NET MVC 3 プロジェクトから ASP.NET MVC 2 プロジェクトの対応する場所に、次のファイルをコピーします。 新しいファイル名 (jquery-1.5.1.js) に対応する jQuery ライブラリへの参照を更新する必要があります。 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Content/themes/\*.\*
3. コピー、*パッケージ*ソリューションの .sln ファイルがあるディレクトリでは、ソリューションのルートに空の ASP.NET MVC 3 プロジェクト ソリューションのルート フォルダー。
4. ASP.NET MVC 2 プロジェクトに区分が含まれている場合、/Views/Web.config ファイルのコピー、*ビュー*各領域のフォルダー。
5. ASP.NET MVC 2 プロジェクトの両方の Web.config ファイルでグローバルに検索し、ASP.NET MVC のバージョンを置き換えます。 次を検索します。 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    これは、次のように置き換えます。

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (を指す、DLL からバージョン 2) への参照を追加し、 *System.Web.Mvc* (v3.0.0.0 へ)。
7. System.Web.WebPages.dll および System.Web.Helpers.dll への参照を追加します。 これらのアセンブリは、次のフォルダーに配置されます。 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 プロジェクト名をもう一度右クリックし、編集*ProjectName*.csproj します。
9. 検索、 *ProjectTypeGuids*要素と置換 {F85E285D-A4E0-4152-9332-AB1D724D3325} {E53F8FEA-EAE0-44A6-8774-FFD645390401}。
10. 変更を保存し、プロジェクトを右クリックし、プロジェクトの再読み込みを選択します。
11. アプリケーションのルートの Web.config ファイルに次の設定を追加、*アセンブリ*セクション。 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. プロジェクトでは、ASP.NET MVC 2 を使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合は、強調表示されている次のコードを追加*bindingRedirect*要素下のアプリケーション ルートの Web.config ファイルを*構成*セクション。 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>ASP.NET MVC 3 での変更ツールを更新します。

このセクションでは、ASP.NET MVC 3 RTM リリース以降、ASP.NET MVC 3 Tools Update リリースで行われた変更について説明します。

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>"コント ローラーの追加 ダイアログ ボックスは、ビューとデータ アクセス コードを使用してコント ローラーをスキャフォールディングできますようになりました

スキャフォールディングをコント ローラーとビュー、アプリケーションをすばやく生成できます。 コードが生成されたら、プロジェクトの要件に合わせて編集できます。

起動する、*コント ローラーの追加*ASP.NET MVC 3、ダイアログ ボックスを右クリックし、*コント ローラー*フォルダー*ソリューション エクスプ ローラー*、 をクリックして*追加*、 をクリックし、*コント ローラー*します。 ダイアログ ボックスは、スキャフォールディングの追加オプションを提供する拡張されています。

![](mvc3-release-notes/_static/image1.png)

次の 3 つスキャフォールディング テンプレートは使用可能な既定です。

#### <a name="empty-controller"></a>空のコント ローラー

このテンプレートには、空のコント ローラー ファイルが生成されます。 このテンプレートはしないことに相当*作成、編集、詳細のアクションを追加、削除、シナリオ*ASP.NET MVC の以前のバージョン。 これを選択すると、さらにオプションの使用はありません。

#### <a name="controller-with-empty-readwrite-actions"></a>空の読み取り/書き込みアクションがコント ローラー

このテンプレートは、メソッドの実装コードではなく、すべての必要なアクション メソッドを含むコント ローラー ファイルを生成します。 このテンプレートはことに相当*作成、編集、詳細のアクションを追加、削除、シナリオ*ASP.NET MVC の以前のバージョン。 これを選択すると、さらにオプションの使用はありません。

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>読み取り/書き込みアクションと Entity Framework を使用して、ビュー コント ローラー

このテンプレートでは、作業データ入力ユーザー インターフェイスをすばやく作成することができます。 一般的な要件と、次のシナリオの範囲を処理するコードが生成されます。

- *データ アクセス*します。 生成されたコードは、データベース内でエンティティを読み書きします。 既存のデータ コンテキスト クラスを選択した場合、または新しい生成テンプレートを使用する場合、Entity Framework Code First アプローチで動作*DbContext*クラス。 既存の選択した場合は、Entity Framework Database First または Model First アプローチでも動作*ObjectContext*クラス。
- *検証*です。 生成されたコードは、モデル クラスで宣言されている規則に従ってフォーム送信が検証できるように、ASP.NET MVC のモデル バインドとメタデータ機能を使用します。 など、組み込みの検証の規則が含まれます、*必要*と*StringLength*属性、およびカスタム検証規則。
- *一対多リレーションシップ*します。 モデル クラス間に一対多外部キー リレーションシップを定義する場合、生成されたコードには関連エンティティを選択するためのドロップダウン リストが生成されます。 たとえば、次に Entity Framework Code First の規則に従うモデル クラスを定義します。 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    コント ローラーをスキャフォールディングすると、*製品*クラス、そのビューにより、ユーザーを選択する、*カテゴリ*オブジェクトごとに*製品*インスタンス。

    このテンプレートの追加のオプションを使用して、*コント ローラーの追加* ダイアログ ボックス。 *モデル クラス*ソリューションを作成または編集できるユーザー データの種類を決定するはどのモデル クラスを選択できます。
- Entity Framework Code First を使用する場合は、どのモデル クラスを選択できます。
- Entity Framework Database First または Entity Framework Model First を使用している場合は、概念モデルで定義されているエンティティ クラスを選択することを確認します。

*データ コンテキスト クラス*、これらの選択を行うことができます。

- Code First を使用して、既存のデータ コンテキスト クラスではありませんしたい場合 * * 新しいデータ コンテキスト * *。 データ コンテキスト クラスが、しが生成されます。
- 既存のデータ コンテキスト クラスを Code First を使用する場合は、ここで選択します。 選択したモデル クラスを永続化が更新されます。
- Database First または Model First を使用している場合は、ここで、オブジェクト コンテキスト クラスを選択します。

ビューの場合、使用するビュー エンジンを選択または任意のビューをスキャフォールディングしたくない場合、[なし] を選択します。

高度なオプションを選択するさらに、生成されたビューのオプションを指定します。 たとえば、レイアウトまたはマスター ページを使用を選択できます。

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>機能強化、"ASP.NET MVC 3 新しいプロジェクト ダイアログ ボックス

新しい ASP.NET MVC 3 プロジェクトの作成に使用する ダイアログ ボックスには、次に示すように複数の機能強化が含まれます。

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>新しい「イントラネット プロジェクト」テンプレート

プロジェクト テンプレートの一覧には、新しいイントラネット アプリケーション テンプレートが用意されています。 このテンプレートには、フォーム認証ではなく Windows 認証を使用して web アプリケーションを構築するための設定が含まれています。 イントラネット アプリケーションには、プロジェクトのテンプレートにカプセル化することはできません、いくつかの IIS 設定が必要であるため、テンプレートには、プロジェクト テンプレートを IIS で使用する方法についての readme ファイルが含まれます。 ドキュメントを新しいイントラネット アプリケーション テンプレートは、次の URL で MSDN web サイトで使用できます。

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>プロジェクト テンプレートが HTML5 に対応

新しいプロジェクト ダイアログ ボックスにプロジェクト テンプレートは、HTML5 固有の機能を追加するオプションが追加されました。 オプションをオンにすると、新たに HTML5 が含まれているビューを生成する`<header>`、 `<footer>`、および`<navigation>`要素。

以前のバージョンのブラウザーは、HTML5 固有のタグをサポートしないことに注意してください。 この制限に対処するため、HTML5 プロジェクト テンプレートに Modernizr ライブラリへの参照が含まれます。 (詳しくは、次のセクションを参照してください)。

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>プロジェクト テンプレートに Modernizr 1.7 が追加されました

Modernizr は、これらの機能をサポートしていないブラウザーで、CSS 3 および HTML5 のサポートを有効にする JavaScript ライブラリです。 このライブラリは、ASP.NET MVC 3 プロジェクト用のテンプレートに事前にインストールされている NuGet パッケージとして含まれています。 Modernizr の詳細については、次を参照してください。 [ http://www.modernizr.com/](http://www.modernizr.com/)します。

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>プロジェクト テンプレートは、更新されたバージョンの jQuery、jQuery UI、および jQuery の検証

プロジェクト テンプレートには、次のバージョンの jQuery スクリプトが追加されました。

- jQuery 1.5.1
- jQuery 検証 1.8
- jQuery UI 1.8.11

これらのライブラリは、NuGet プレインストール パッケージとして含まれています。

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>プロジェクト テンプレートでは、NuGet プレインストール パッケージとして ADO.NET Entity Framework 4.1 が追加されました

ADO.NET の Entity Framework 4.1 には、Code First 機能が含まれています。 コードは、既存の Database First と Model First パターンの代替を提供する ADO.NET Entity Framework の新しい開発パターンを最初です。

コードはまず、Visual Basic または c# で記述された POCO クラス ("plain old CLR object") を使用して、モデルの定義をフォーカスしています。 これらのクラスは、既存のデータベースにマップすることができますし、またはデータベース スキーマの生成に使用します。 使用して追加の構成を指定する*DataAnnotations*属性または fluent Api を使用します。

コード Firstwith ASP.NET MVC を使用するためのドキュメントは、次の Url で ASP.NET web サイトで使用できます。

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>プロジェクト テンプレートは、NuGet プレインストール パッケージとして JavaScript ライブラリを含める

それらをインストールすることで (たとえば、Modernizr ライブラリ) に記載されている JavaScript ファイルがプロジェクトに含まれています新しい ASP.NET MVC 3 プロジェクトを作成するときに、プロジェクト テンプレートで、Scripts フォルダーにスクリプトを直接追加する代わりに NuGet を使用します。内容。 これにより、NuGet を使用して、スクリプトの新しいバージョンがリリースされたときに、最新バージョンに、スクリプトを更新することができます。

たとえば、jQuery の新しいリリースの頻度を指定するには、プロジェクト テンプレートに含まれる jQuery のバージョンがある時点で期限切れ。 ただし、jQuery はインストールされている NuGet パッケージとして含まれるため、通知されます NuGet ダイアログ ボックスで、jQuery の新しいバージョンが利用可能な場合。

JQuery には、ファイル名にバージョン番号が含まれているため、jQuery を最新バージョンに更新も更新が必要です、`<script>`新しいファイル名を使用する jQuery ファイルを参照するタグ。 その他のスクリプトが含まれるライブラリでは、スクリプト名 でバージョン番号は含まれないので、最新バージョンにより簡単に更新することができます。

<a id="tu-KI"></a>
## <a name="known-issues"></a>既知の問題

- 場合によっては、インストール、エラー メッセージ「インストールに失敗しましたエラー コード (0x80070643)」で失敗します。 この問題を回避する方法については、次を参照してください。[サポート技術情報の記事 2531566](https://support.microsoft.com/kb/2531566)します。
- コント ローラーを追加するためのスキャフォールディングでは、Entity Framework 内でエンティティ継承サポートを利用するエンティティはスキャフォールディングされません。 たとえば、ベースを指定*人*クラスによって継承される、*学生*クラスをスキャフォールディング、*学生*クラスと、生成されたコード コンパイルされていません。
- ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。 回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper がインストールされているあり ASP.NET MVC 3 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。
- Razor ビューを編集するときに (.cshtml または *。vbhtml*ファイル)、ビュー。 ASP.NET MVC 3 では、Razor ビューのスニペットは含まれません.ASP.NET MVC 用のコード スニペットを aspxselecting スニペットが表示されます。
- ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>ASP.NET MVC 3 RTM での変更

このセクションでは、変更と RC2 リリース後、ASP.NET MVC 3 RTM リリースで行われたバグ修正について説明します。

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>変更: 1.8.7 に jQuery UI のバージョンを更新します。

Visual Studio の ASP.NET MVC プロジェクト テンプレートは、jQuery UI ライブラリの最新バージョンを含むように更新されました。 テンプレートには、jQuery、関連付けられている CSS とイメージ ファイルなどの UI に必要なリソース ファイルの最小セットも含まれます。

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>変更: DataAnnotationsModelMetadataProvider を ModelMetadataProvider 既定値を変更します。

導入された ASP.NET MVC 3 の RC2 リリースを*CachedDataAnnotationsMetadataProvider*クラスに提供される既存の上にキャッシュ*DataAnnotationsModelMetadataProvider*クラスとして、パフォーマンスの向上。 変更を元に戻すしで利用可能な計画の MVC プロジェクトに移動するため、この実装でに報告されたいくつかのバグのただし、 [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)します。

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>修正済み: 取り消し中に空白の結果を含む Razor 式の一部を貼り付ける

ASP.NET MVC 3 のプレリリース バージョンは、Razor ファイルに空白を含む Razor 式の一部を貼り付けると、結果の式が逆になります。 たとえば、次の Razor コード ブロックがあるとします。

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

最初のメソッドでテキスト"最初 param"を選択し、2 番目のメソッドに引数として貼り付けます場合、結果はとおりです。

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

正しい動作は、貼り付け操作は次のようになります。

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

式が正しく貼り付け操作中に保持されるように RTM リリースでこの問題は修正されています。

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>修正済み: エディターで開かれている Razor ファイル名の変更を無効にします構文の色付けと IntelliSense

エディター ウィンドウで、ファイルを開いたときに、ソリューション エクスプ ローラーを使用して Razor ファイルの名前を変更すると、構文の強調表示、IntelliSense ファイルの動作を停止します。 強調表示するためにこれを修正されていて、IntelliSense は、名前の変更後も保持します。

<a id="RTM-KI"></a>
## <a name="known-issues"></a>既知の問題

- 場合は、NuGet パッケージ マネージャー コンソールが開いているときに Visual Studio 2010 SP1 Beta を閉じると、Visual Studio がクラッシュして再起動しようとしています。 これは、問題を修正する予定の Visual Studio 2010 SP1 の RTM リリースでします。
- ASP.NET MVC 3 インストーラーでは、NuGet パッケージ マネージャーの初期のバージョンをインストールすることのみです。 最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。 NuGet がインストールされて既にある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。
- ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。 回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。
- インストーラーが完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。 Visual Studio 2010 のコンポーネントを更新するためです。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper がインストールされているあり ASP.NET MVC 3 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。
- ASP.NET MVC 3 のベータ版で作成した CCSHTML と VBHTML のビューでは、ビルド アクションが正しく設定されていない、プロジェクトが発行されたときにこれらを表示する結果の型が省略されています。 これらのファイルのビルド アクションの値は、"Content"に設定する必要があります。 ASP.NET MVC 3 RTM が、新しいファイルの場合は、この問題を修正、プレリリース版で作成されたプロジェクトの既存のファイルの設定を修正しません。
- ![](mvc3-release-notes/_static/image3.png)
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。
- ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- ASP.NET MVC の以前のバージョンでは、アクション フィルターは、いくつかの場合を除く要求ごと作成します。 この動作が保証された動作が実装の詳細だけで、されていないことと、フィルターのコントラクトがステートレスに考慮すべきでした。 ASP.NET MVC 3 では、フィルターがより積極的にキャッシュされます。 そのため、不適切なインスタンスの状態を格納するカスタム アクション フィルターは、壊れている場合があります。
- 例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。 ASP.NET MVC 2 以降では、同じであるコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されるため、アクション メソッドの値します。 通常、ある場合、この例外フィルターが適用されるときに指定されていない*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラス。 ASP.NET では、(名前) ではなくパスでビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。 これは、Web フォーム ビューのファイルのカスタム拡張機能を有効にするには、カスタム ビルド プロバイダーが登録されているし、プロバイダー名ではなく、完全なパスを使用してこれらのビューが参照して、アプリケーションで重大な変更です。 回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。
- 直接実装するカスタム コント ローラー ファクトリの実装、 *IControllerFactory*インターフェイスは、新しい実装を提供する必要があります*GetControllerSessionBehavior*がメソッドこのリリースでは、インターフェイスに追加されます。 一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる*DefaultControllerFactory*します。

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>ASP.NET MVC 3 RC2 での変更

このセクションでは、RC のリリース以降、ASP.NET MVC 3 RC2 リリースで行われた変更 (新機能とバグの修正) について説明します。

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>JQuery 1.4.4、jQuery 検証 1.7、jQuery UI 1.8.6 など、プロジェクト テンプレートの変更

ASP.NET MVC 3 のプロジェクト テンプレートでは、jQuery、jQuery の検証、および jQuery の最新バージョンが追加 UI。 jQuery UI は、プロジェクト テンプレートに新たに追加し、便利なユーザー インターフェイス ウィジェットを提供します。 JQuery UI の詳細については、自分のホーム ページを参照してください: [ http://jqueryui.com/](http://jqueryui.com/)します。

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>追加された"AdditionalMetadataAttribute"クラス

使用することができます、 *AdditionalMetadataAttribute*を設定するクラス、 *ModelMetadata.AdditionalValues*モデル プロパティのディクショナリ。

たとえば、ビュー モデルに、管理者にのみ表示されるプロパティがあるとします。 そのモデル AdminOnly をキーと true として次の例のように、値として使用して、新しい属性で注釈を付けることができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

このメタデータは、製品のビュー モデルが表示されるときに、表示またはエディターのテンプレートを使用されます。 メタデータ情報を解釈するアプリケーション開発者としての開発者次第です。

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>強化されたビューのスキャフォールディング

ここではビューのスキャフォールディングを使用する T4 テンプレートなどのテンプレート ヘルパー メソッドの呼び出しの生成*EditorFor*などのヘルパーではなく*TextBoxFor*します。 この変更は、ビューの追加 ダイアログ ボックスのビューを生成するときに、データ注釈属性の形式でモデルのメタデータのサポートを向上します。

ビューの追加のスキャフォールディングには、検出機能の向上と規則に基づいて、モデルの主キーの情報の使用状況も含まれています。 たとえば、ビューの追加 ダイアログ ボックスはこの情報を使用して主キーの値が編集可能なフォームのフィールドとスキャフォールディングしないことを確認します。

既定の編集とテンプレートの作成には、クライアントの検証に必要な jQuery スクリプトへの参照が含まれます。

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>追加された Html.Raw メソッド

既定では、Razor はすべての値を HTML エンコード エンジンを表示します。 次のコード スニペットがあいさつの変数の内部 HTML をエンコードするとしてページに表示されるように、`<strong>Hello World!</strong>`します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

新しい*Html.Raw*メソッドは、コンテンツの安全がわかっている場合は、エンコードされていない HTML を表示する簡単な方法を提供します。 次の例が、同じ文字列が表示されますが、文字列がマークアップとしてレンダリングされます。

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>名前が変更された"Controller.ViewModel"プロパティと"ViewBag"を"View"プロパティ

以前は、 *ViewModel*プロパティの*コント ローラー*に対応する、*ビュー*ビューのプロパティ。 値をアクセスする方法を提供これら両方のプロパティ、 *ViewDataDictionary*オブジェクトの動的なプロパティ アクセサーの構文を使用します。 両方のプロパティは、混乱を避けるためより一貫したものと同じである名前に変更されています。

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>名前が変更された"ControllerSessionStateAttribute"クラス"SessionStateAttribute"を

*ControllerSessionStateAttribute*クラスは、ASP.NET MVC 3 の RC リリースで導入されました。 簡潔にするプロパティが変更されました。

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>「ため」に名前を変更した RemoteAttribute"Fields"プロパティ

*RemoteAttribute*クラスの*フィールド*ユーザー間で混乱の原因となったプロパティ。 このプロパティの名前を変更する*ため*その意図を明確にします。

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"AllowHtmlAttribute"を"SkipRequestValidationAttribute"の名前を変更します。

*SkipRequestValidationAttribute*に属性が変更されました*AllowHtmlAttribute*をわかりやすく、用途を表します。

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>最初に、役立つエラー メッセージを表示する変更された"Html.ValidationMessage"メソッド

*Html.ValidationMessage*最初のエラーを表示するだけではなく最初の有用なエラー メッセージを表示するメソッドが修正されました。

モデルのバインド中に、 *ModelState*ディクショナリは、モデル自体からなど、プロパティに関するエラー メッセージで複数のソースから設定できます (実装している場合*IValidatableObject*)、およびプロパティがアクセスされているときにスローされる例外、プロパティに適用される検証属性から。

ときに、 *Html.ValidationMessage*メソッド検証メッセージを表示する、これらは一般にないため、エンドユーザーは、例外が含まれているモデル状態エントリをスキップします。 代わりに、このメソッドは最初の検証メッセージを例外に関連付けられていないと、そのメッセージが表示されます。 このようなメッセージが検出されない場合の既定値は、最初の例外に関連付けられている一般的なエラー メッセージです。

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>固定@model空白の文書に追加の宣言

以前のリリースで、 <em>@model</em>ビューの上部にある宣言が表示される HTML 出力に空白行を追加します。 これは、宣言は空白を挿入しないように修正されています。

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>エンジン固有のファイル名をサポートするために、ビュー エンジンに追加された"FileExtensions"プロパティ

ビュー エンジンは、次の例のように、明示的なビューのパスを使用してビューを返すことができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

最初のビュー エンジンは、常にビューをレンダリングしようとします。 既定では、Web フォーム ビュー エンジンは、最初のビュー エンジンです。Web フォームのエンジンは、Razor ビューをレンダリングできません、ためエラーが発生します。 ビュー エンジンのようになりましたが、 *FileExtensions*ファイル拡張子を指定するために使用するプロパティをサポートします。 このプロパティは、ASP.NET では、ビュー エンジンが、ファイルを表示できるかどうかが判断した場合にチェックされます。 これは重大な変更と詳細についてに含まれる、[重大な変更](#_Toc2_BC)このドキュメントの「します。

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>固定"LabelFor"ヘルパー"For"属性の正しい値を出力するには

バグはどこで修正された、 *LabelFor*レンダリング メソッドを*の*と一致する属性、*入力*要素の*名前*属性の代わりにID の W3C に従って、*の*属性に一致する必要があります、*入力*要素の id。

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>モデル バインド中に明示的な値を優先する固定"RenderAction"メソッド

以前のバージョンでは、明示的な値に渡された、 *RenderAction*メソッドは現在のフォーム値を優先して子アクション内でのモデル バインド中に無視されているされました。 この修正により、明示的な値がモデル バインド中に優先順位を取得します。

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- ASP.NET MVC の以前のバージョンでは、アクション フィルターが 1 回のような場合を除く要求作成されました。 この動作が保証された動作が実装の詳細だけで、されていないことと、フィルターのコントラクトがステートレスに考慮すべきでした。 ASP.NET MVC 3 では、フィルターがより積極的にキャッシュされます。 そのため、不適切なインスタンスの状態を格納するカスタム アクション フィルターは、壊れている場合があります。
- 例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。 ASP.NET MVC 2 以降では、同じがあったコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されたアクション メソッドの値します。 通常、ある場合、この例外フィルターが適用されたときに指定されていない*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という名前の新しいプロパティ*FileExtensions*に追加された、 *VirtualPathProviderViewEngine*基本クラス。 ASP.NET では、(名前) ではなくパスでビューを検索、この新しいプロパティで指定されたリストに含まれるファイル拡張子を持つビューのみが考慮されます。 これは、Web フォーム ビューのファイルのカスタム拡張機能を有効にするには、カスタム ビルド プロバイダーが登録されているし、プロバイダー名ではなく、完全なパスを使用してこれらのビューが参照して、アプリケーションで重大な変更です。 回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。
- 直接実装するカスタム コント ローラー ファクトリの実装、 <em>IControllerFactory</em>インターフェイスは、新しい実装を提供する必要があります<em>GetControllerSessionBehavior</em> <em>このリリースでは、インターフェイスに追加されたメソッド</em>します。 一般に、お勧めするこのインターフェイスを直接実装およびしない代わりに、クラスから派生させる<em>DefaultControllerFactory</em>します。

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>既知の問題

- ASP.NET MVC 3 インストーラーでは、NuGet パッケージ マネージャーの初期のバージョンをインストールすることのみです。 最初のバージョンをインストールした後、NuGet はインストールすることができ、Visual Studio 拡張機能マネージャーを使用して更新します。 NuGet がインストールされて既にある場合は、NuGet の最新バージョンに更新するには、Visual Studio 拡張機能ギャラリーに移動します。
- ソリューション フォルダー内で新しい ASP.NET MVC 3 プロジェクトを作成するには、原因、 *NullReferenceException*エラー。 回避策では、ソリューションのルートに ASP.NET MVC 3 プロジェクトを作成し、ソリューション フォルダーに移動しています。
- インストーラーが完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。 Visual Studio 2010 のコンポーネントを更新するためです。
- Razor 構文の IntelliSense では、ReSharper がインストールされている場合は機能しません。 ReSharper がインストールされているあり ASP.NET MVC 3 RC2 での Razor IntelliSense サポートを利用する場合、エントリを参照してください。 [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)併用する方法が説明されています Hadi Hariri のブログ、します。
- ASP.NET MVC 3 のベータ版で作成された CSHTML と VBHTML のビューでは、ビルド アクションが正しく設定されていない、プロジェクトが発行されたときにこれらを表示する結果の型が省略されています。 *ビルド アクション*値は、これらのファイルは、コンテンツに設定する必要があります"。 ASP.NET MVC 3 RC2 では、新しいファイルの場合は、この問題が修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。![](mvc3-release-notes/_static/image4.png)
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。
- ASP.NET MVC 3 for Visual Web Developer Express で Visual Studio がインストールされていない、コンピューターにインストールし、後で Visual Studio をインストールして場合に、ASP.NET MVC 3 を再インストールする必要があります。 Visual Studio と Visual Web Developer Express は、ASP.NET MVC 3 インストーラーによってアップグレードしたコンポーネントを共有します。 Visual Web Developer Express があるし、後で Visual Web Developer Express インストール コンピューターに Visual Studio for ASP.NET MVC 3 をインストールする場合、同じ問題が適用されます。
- ASP.NET MVC 3 RC 2 をインストールしても、NuGet は更新されない場合、既にインストールされていること。 NuGet をアップグレードするには、Visual Studio 拡張機能マネージャーに移動する必要がありますとして表示、利用可能な更新。 NuGet は、そこから最新のリリースにアップグレードできます。

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 のリリース候補

ASP.NET MVC リリース候補は、2010 年 11 月 9 日にリリースされました。

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>ASP.NET MVC 3 RC の新機能

このセクションが導入された機能について説明します、ベータ版リリース以降、ASP.NET MVC 3 RC リリースでします。

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet パッケージ マネージャー

ASP.NET MVC 3 には、(旧称 NuPack)、Visual Studio プロジェクトにライブラリとツールを追加するための統合パッケージ管理ツールである NuGet パッケージ マネージャーが含まれています。 このツールは、開発者は、ソース ツリーにライブラリを取得する現在実行中の手順を自動化します。

コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウと一連の PowerShell コマンドレットは、NuGet を使用することができます。

NuGet の詳細については、読み取り、 [Nuget のドキュメント](https://docs.microsoft.com/nuget/)します。

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>新しいプロジェクト の改善 ダイアログ ボックス

新しいプロジェクトを作成するときに、[新しいプロジェクト] ダイアログ ボックスできるようになりました、ビュー エンジンも ASP.NET MVC プロジェクトの種類を指定します。

![](mvc3-release-notes/_static/image5.png)

このリリースでは、テンプレート、およびビュー エンジンの表示 ダイアログ ボックスの一覧を変更するためのサポートが含まれます。

既定のテンプレート、次に示します。

空です。 最低限 ASP.NET MVC プロジェクトでの既定ディレクトリ構造、ASP.NET MVC をスタイル設定、既定値と既定の JavaScript ファイルを含む Scripts ディレクトリを含む Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイルにはが含まれています。

インターネット アプリケーションです。 ASP.NET MVC を使用したメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。

ダイアログ ボックスに表示されるプロジェクト テンプレートの一覧は、Windows レジストリで指定されます。

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sessionless コント ローラー

新しい*ControllerSessionStateAttribute*うえでセッション状態の動作の制御コント ローラーを指定して、 [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx)列挙値。

次の例では、すべての要求をコント ローラーのセッション状態をオフにする方法を示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

次の例では、コント ローラーにすべての要求の読み取り専用のセッション状態を設定する方法を示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>新しい検証属性

#### <a name="compareattribute"></a>CompareAttribute

新しい*CompareAttribute*検証属性では、モデルの 2 つの異なるプロパティの値を比較することができます。 次の例では、 *ComparePassword*プロパティに一致する必要があります、*パスワード*有効にするのにはフィールド。

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

新しい*RemoteAttribute*によって実際の検証ロジックを実行するサーバーでメソッドを呼び出すクライアント側の検証、jQuery 検証プラグインのリモート検証の検証属性を利用します。

次の例では、*ユーザー名*プロパティは、 *RemoteAttribute*適用します。 クライアント検証がという名前のアクションを呼び出して、編集ビューでは、このプロパティを編集するには、 *UserNameAvailable*上、 *UsersController*このフィールドを検証するためにクラス。

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

次の例は、対応するコント ローラーを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

既定では、属性が適用されるプロパティ名はクエリ文字列パラメーターとしてアクション メソッドに送信されます。

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>"LabelFor"と"LabelForModel"メソッドの新しいオーバー ロード

新しいオーバー ロードが追加されました、 *LabelFor*と*LabelForModel*できるメソッドは、ラベルのテキストを指定します。 次の例では、これらのオーバー ロードを使用する方法を示します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>子アクションの出力キャッシュ

*OutputCacheAttribute*を使用して呼び出される子アクションの出力がキャッシュをサポートしています、 *Html.RenderAction*または*Html.Action*ヘルパー メソッド。 次の例では、別のアクションを呼び出すビューを示します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

*GetDate*アクションは、注釈が付けられた、 *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

このコードを実行すると、Html.Action("GetDate") への呼び出しの結果が 100 秒間キャッシュされます。

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"ビューの追加 ダイアログ ボックスの機能強化

厳密に型指定されたビューを追加するときにビューの追加 ダイアログ ボックスは多くの中核となる .NET Framework 型などの以前のリリースでより詳細に該当しない種類を今すぐフィルター処理します。 また、クラスの名前と種類を検索しやすくなる完全修飾型名ではなくリストの並べ替えようになりました。 たとえば、型名は次の例のように表示されます。

クラス名 (名前空間)

以前のリリースでこのされて表示される次のよう。

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>詳細な要求の検証

*除外*プロパティの*ValidateInputAttribute*は存在しません。 代わりに、モデルの特定のプロパティのモデル バインド中にスキップされた要求の検証には、使用、新しい*SkipRequestValidationAttribute*します。

たとえば、アクション メソッドは、ブログの投稿を編集するために使用します。

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

次の例では、ブログの投稿のビュー モデルを示します。

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

ユーザーは、Description プロパティの一部のマークアップを送信する、モデル バインドは要求の検証のため失敗します。 ブログの投稿の説明のモデル バインド中に要求の検証を無効にするには、適用、 *SkipRequpestValidationAttribute*プロパティのこの例で示すように: です。

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

または、モデルのすべてのプロパティで要求の検証を無効に、次のように適用します。 *ValidateInputAttribute*の値を持つ*false*アクション メソッドに。

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>互換性に影響する変更点

- 例外フィルターの実行の順序が同じである例外フィルターの変更された*順序*値。 ASP.NET MVC 2 以降では、同じがあったコント ローラー上の例外がフィルター*順序*アクション メソッドで例外フィルターの前に実行されたアクション メソッドのようにします。 通常、ある場合、この例外フィルターが適用されたときに指定されていない*順序*値。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと場合、*順序*プロパティが明示的に指定されて、フィルターは、指定した順序で実行されます。
- という新しいプロパティを追加*FileExtensions*を*VirtualPathProviderViewEngine*基本クラス。 検索時にビューのパスで、名前ではなく) に含まれているファイル拡張子を持つビューにのみ、この新しいプロパティで指定された一覧と見なされます。 これは、web フォーム ビューのファイルのカスタム拡張機能を有効にするカスタム ビルド プロバイダーを登録し、名前ではなく、完全なパスを使用してこれらのビューを参照している方のための互換性に影響する変更です。 回避の値を変更するには、 *FileExtensions*プロパティをカスタムのファイル拡張子が含まれます。

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>既知の問題

- インストーラーは、Visual Studio 2010 のコンポーネントを更新するため、完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。
- ビューの追加のスキャフォールディング astrongly を選択するときに、ビューのスキャフォールディング書き込み専用プロパティを入力します。 これらは、スキャフォールディングによって常に無視する必要があります。 ビューの追加 ダイアログは、「編集」または「作成」のビューを生成するときにも読み取り専用プロパティをスキャフォールディングします。 読み取り専用プロパティを表示およびリスト ビューのスキャフォールディングのみ必要があります。
- ASP.NET MVC 3 を Async CTP と共にインストールすると、デバッグが機能しません。 ASP.NET MVC 3 では、Async CTP と並行してインストールをすることはできません。 デバッグを修復する Async CTP をアンインストールします。 詳細については、「[このブログの投稿](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html)ASP.NET MVC 3 RC すべて部分アンインストールの詳細。
- Razor Intellisense では、Resharper がインストールされている場合は機能しません。 ReSharper をインストールしてお読みください ASP.NET MVC 3 RC での Razor intellisense サポートの利用する[このブログの投稿](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/)jetbrains を併用する方法について説明します。
- ASP.NET MVC 3 のベータ版で作成された CSHTML と VBHTML のビューがありませんビルド アクションが正しく公開からそれらが省略されます。 *ビルド アクション*は、これらのファイルは、"Content"に設定する必要があります。 ASP.NET MVC 3 RC では、新しいファイルの場合は、この問題が修正がベータ版で作成されたプロジェクトの既存のファイルの設定を修正しません。
- インストーラーは、Visual Studio 2010 のコンポーネントを更新するため、完了する ASP.NET MVC の以前のバージョンよりもかなり長くかかる場合があります。
- ビューの追加のスキャフォールディング ビューのスキャフォールディング"Edit"を選択すると、厳密に型指定されたときに読み取り専用プロパティを指定します。 同様に、書き込み専用プロパティは「表示」ビューをスキャフォールディングします。
- インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。
- Visual Studio Async CTP をインストールすると、リリースでは、Razor、ASP.NET MVC 3 のツールのインストールの一部として含まれている競合が発生します。 Visual Studio Async CTP と Razor リリースの両方を同じコンピューターにインストールしないでことを確認します。
- Razor ビュー (.cshtml ファイル) を編集するときは、Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットはありません。

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 のベータ版

ASP.NET MVC 3 のベータ版は、2010 年 10 月 6 日にリリースされました。 次のノートは、ベータ リリースに固有し、更新や変更前の ASP.NET MVC 3 のリリース候補のセクションで参照されている影響を受けます。

## <a id="0.1__Toc274034215"></a>  新しい Featuresin ASP.NET MVC 3 のベータ版

<a id="0.1__Default_validation_system"></a>このセクションが導入された機能について説明します、ASP.NET MVC 3 のベータ リリースでします。

### <a id="0.1__Toc274034216"></a>  NuGet パッケージ マネージャー

ASP.NET MVC 3 では、NuGet パッケージ マネージャーは、追加のライブラリの統合パッケージ管理ツールおよび Visual Studio プロジェクトのツールが含まれています。 ほとんどの場合、開発者は、ソース ツリーにライブラリを取得する現在実行中の手順を自動化します。

コマンド ライン ツール、Visual Studio のコンテキスト メニューから Visual Studio 2010 内の統合されたコンソール ウィンドウおよび PowerShell コマンドレットのセットは、NuGet を使用することができます。

NuGet の詳細については、読み取り、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)します。

### <a id="0.1__Toc274034217"></a>  強化された新しいプロジェクト ダイアログ ボックス

新しいプロジェクトを作成するときに、[新しいプロジェクト] ダイアログ ボックスできるようになりました、ビュー エンジンも ASP.NET MVC プロジェクトの種類を指定します。

![](mvc3-release-notes/_static/image6.png)

このリリースでは、テンプレート、およびビュー エンジンの表示 ダイアログ ボックスの一覧を変更するためのサポートが含まれていません。

既定のテンプレート、次に示します。

空です。 最低限 ASP.NET MVC プロジェクトでの既定ディレクトリ構造を既定の ASP.NET MVC をスタイル設定、および既定の JavaScript ファイルを含む Scripts ディレクトリを含む小さい Site.css ファイルを含む、ASP.NET MVC プロジェクトのファイルにはが含まれています。

インターネット アプリケーションです。 ASP.NET MVC 内でのメンバーシップ プロバイダーを使用する方法を示すサンプルの機能が含まれています。

### <a id="0.1__Toc274034218"></a>  厳密に指定する方法を簡略化された Razor ビューで型指定されたモデル

新しいを使用して、厳密に型指定の Razor ビューのモデルの種類を指定する方法を簡略化された@modelCSHTML ビューのディレクティブと@ModelTypeVBHTML ビューのディレクティブ。 ASP.NET MVC の以前のバージョンでは、Razor の厳密に型指定されたモデル ビューのこのように指定します。

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

このリリースでは、次の構文を使用できます。

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  新しい ASP.NET Web ページのヘルパー メソッドをサポートします。

新しい ASP.NET Web Pages テクノロジには、一連ビューとコント ローラーによく使用される機能を追加するのに便利なヘルパー メソッドにはが含まれています。 ASP.NET MVC 3 では、コント ローラーとビュー内でこれらのヘルパー メソッドを使用して、(適切な場所) をサポートします。 これらのメソッドは、System.Web.Helpers アセンブリ内に格納されます。 次の表では、ASP.NET Web Pages のヘルパー メソッドのいくつかを示します。

| **ヘルパー** | **説明** |
| --- | --- |
| グラフ | ビュー内のグラフを表示します。 Chart.ToWebImage、Chart.Save、Chart.Write などのメソッドが含まれています。 |
| Crypto | 正常に作成するアルゴリズムのハッシュを使用してでは、ソルトし、パスワードをハッシュします。 |
| WebGrid | オブジェクト (通常は、データベースからのデータ) のコレクションをグリッドとして表示します。 ページングと並べ替えをサポートします。 |
| WebImage | イメージを表示します。 |
| WebMail | 電子メールを送信します。 |

ヘルパーと基本的な構文を示すクイック リファレンスのトピックでは使用可能な次の URL で ASP.NET Razor 構文のドキュメントの一部として。

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  追加の依存関係挿入をサポート

ASP.NET MVC 3 Preview 1 リリースのビルド、現在のリリースには、2 つの新しいサービスや既存の 4 つのサービスのサポートを追加し、依存関係の解決と共通サービス ロケーターのサポートの強化が含まれます。

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>きめ細かなコント ローラー インスタンスを作成して新しい IControllerActivator インターフェイス

新しい IControllerActivator インターフェイスは、依存関係の挿入を使用してコント ローラーがインスタンス化する方法をより細かく管理を提供します。 次の例は、インターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

コント ローラー ファクトリの役割には、この操作を比較します。 コント ローラー ファクトリは、コント ローラーの種類を検索するため、そのコント ローラーの種類のインスタンスをインスタンス化するための両方を担当する IControllerFactory インターフェイスの実装です。

コント ローラー アクティベーターがコント ローラーの種類のインスタンスをインスタンス化のみを担当します。 コント ローラーの種類の参照は行いません。 適切なコント ローラーの種類を特定すると、コント ローラー ファクトリは、コント ローラーの実際のインスタンス化を処理するために IControllerActivator のインスタンスに委任する必要があります。

DefaultControllerFactory クラスには、IControllerFactory インスタンスを受け取る新しいコンス トラクターがあります。 既定のコント ローラーの種類のルックアップ動作をオーバーライドすることがなく、コント ローラーの作成のこの側面を管理する依存関係の挿入を適用できます。

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IDependencyResolver を置き換え IServiceLocator インターフェイス

コミュニティからのフィードバックに基づき、ASP.NET MVC 3 のベータ リリースは、ASP.NET MVC のニーズに応じたスリムにした IDependencyResolver インターフェイスを持つ、IServiceLocator インターフェイスの使用を交換することが。 次の例は、新しいインターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

この変更の一環として、DependencyResolver クラスを使用して、ServiceLocator クラスも置き換えられました。 依存関係競合回避モジュールの登録は、ASP.NET MVC の以前のバージョンと同様です。

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

このインターフェイスの実装は、要求された型に対して登録されているサービスを提供する基になる依存関係挿入コンテナーに単に委任必要があります。

要求された型の登録済みのサービスがない場合は、ASP.NET MVC は GetService から null を返すと、GetServices から空のコレクションを返すには、このインターフェイスの実装が必要です。

新しい DependencyResolver クラスを使用して、新しい IDependencyResolver インターフェイスまたは共通サービス ロケーターのインターフェイス (IServiceLocator) のいずれかを実装するクラスを登録できます。 共通サービス ロケーターの詳細については、次を参照してください。 [github CommonServiceLocator](https://github.com/unitycontainer/commonservicelocator)します。

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>詳細なビュー ページ インスタンス化用の新しい IViewActivator インターフェイス

新しい IViewPageActivator インターフェイスは、依存関係の挿入を使用してページの表示がインスタンス化する方法をより細かく管理を提供します。 これは、WebFormView インスタンスと RazorView インスタンスの両方に適用されます。 次の例は、新しいインターフェイスを示しています。

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

これらのクラスは、できる ViewPage、ViewUserControl、WebViewPage 型がインスタンス化する方法を制御するために依存関係の挿入を使用する、IViewPageActivator コンス トラクター引数を受け入れるようになりました。

#### <a name="new-dependency-resolver-support-for-existing-services"></a>既存のサービスの新しい依存関係競合回避モジュールのサポート

新しいリリースには、次のサービスの依存関係解決のサポートが含まれています。

- モデル検証プロバイダー。 ModelValidatorProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムがクライアント側とサーバー側の検証をサポートするために使用されます。
- モデル メタデータ プロバイダー。 ModelMetadataProvider を実装する 1 つのクラスは、依存関係競合回避モジュールに登録することができ、システムは、テンプレート、および検証システムのメタデータを提供することを使用します。
- 値プロバイダー。 ValueProviderFactory を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが、コント ローラーによって、モデル バインド中に使用される値プロバイダーの作成に使用されます。
- モデル バインダー。 IModelBinderProvider を実装するクラスは、依存関係競合回避モジュールに登録することができ、システムが、モデル バインド システムで使用されるモデル バインダーの作成に使用されます。

### <a id="0.1__Toc274034221"></a>  控えめな jQuery ベースの Ajax のサポート

ASP.NET MVC には、次などの Ajax ヘルパー メソッドが含まれます。

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

これらのメソッドでは、JavaScript を使用して、完全なポストバックを使用するのではなく、サーバーに、アクション メソッドを呼び出します。 この機能が活用するために jQuery unobtrusive の方法で更新されました。 インライン クライアント スクリプトを生成するそのままの状態ではなくこれらのヘルパー メソッドの動作から分離マークアップを使用して HTML5 属性を生成することによって、*データ ajax*プレフィックス。 動作は、適切な JavaScript ファイルを参照することによって、マークアップに適用されます。 次の JavaScript ファイルが参照されていることを確認します。

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

この機能は、新規の ASP.NET MVC 3 プロジェクト テンプレートでは、Web.config ファイルで既定で有効ですが、既存のプロジェクトは既定では無効します。 詳細については、次を参照してください。[のクライアント検証と控えめな JavaScript アプリケーション全体のフラグを追加](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。

### <a id="0.1__Toc274034222"></a>  控えめな jQuery 検証のサポート

既定では、ASP.NET MVC 3 のベータ版は、控えめな方法でクライアント側の検証を実行するには jQuery の検証を使用します。 控えめなクライアント検証を有効にするには、ビュー内から次のような呼び出しを行います。

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

これは、ViewContext.UnobtrusiveJavaScriptEnabled プロパティが、次の呼び出しを行うにはできる true に設定されていることが必要です。

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

また、次の JavaScript ファイルが参照されていることを確認してください。

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

この機能は、新規の ASP.NET MVC 3 プロジェクト テンプレートでは、Web.config ファイルでは既定で有効になっているが、既存のプロジェクトは既定では無効になります。 詳細については、次を参照してください。[のクライアント検証と控えめな JavaScript アプリケーション全体のフラグを新しい](#0.1_AddedApplicationWideFlagsForClientValida)このドキュメントで後述します。

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  クライアント検証と控えめな JavaScript の新しいアプリケーション全体のフラグ

有効にするか、クライアントの検証と控えめな JavaScript の次の例のように、HtmlHelper クラスの静的メンバーを使用してグローバルに無効にします。

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

既定のプロジェクト テンプレートは、既定では、控えめな JavaScript を有効にします。 有効にするか、次の設定を使用して、アプリケーションのルート Web.config ファイルでこれらの機能を無効にします。

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

既定では、これらの機能を有効にすることができます、ため、新しいオーバー ロードは HtmlHelper クラスに次の例に示すようには、既定の設定を上書きすることが導入されました。

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

旧バージョンと互換性のため、これら両方の機能は既定で無効です。

### <a id="0.1__Toc274034224"></a>  ビューの実行前に実行されるコードの新しいサポート

という名前のファイルを挿入できるように\_viewstart.cshtml (または\_viewstart.vbhtml) で Views ディレクトリをそのディレクトリとそのサブディレクトリ内の複数のビューの間で共有されるコードを追加します。 次のコードを配置するなど、 \_viewstart.cshtml ~/Views フォルダー ページ。

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

これには、Views フォルダー内のすべてのビューとそのサブフォルダー再帰的にレイアウト ページを設定します。 ときに、ビュー、表示されるコード、 \_viewstart.cshtml ファイルは、コードの表示を実行する前に、実行します。 \_Viewstart.cshtml コードは、そのフォルダー内のすべてのビューに適用されます。

既定では、コードで、 \_viewstart.cshtml ファイルは、任意のサブフォルダー内のビューにも適用されます。 ただし、個別のサブフォルダーでは、独自のバージョンのことが、 \_viewstart.cshtml ファイルの場合も、ローカル バージョンは、優先順位にします。 たとえば、HomeController のすべてのビューに共通するコードを実行する配置を\_~/Views/Home フォルダーにある viewstart.cshtml ファイル。

### <a id="0.1__Toc274034225"></a>  VBHTML Razor 構文のサポート

ASP.NET MVC の以前のプレビューには、c# に基づく Razor 構文を使用してビューのサポートが含まれています。 これらのビューは、.cshtml ファイルの拡張機能を使用します。 Razor をサポートするために進行中の作業の一環として、ASP.NET MVC 3 のベータ版では、.vbhtml ファイル拡張機能を使用する Visual Basic での Razor 構文のサポートが導入されています。

VBHTML ページで Visual Basic 構文の使用の概要については、次の URL でチュートリアルを参照してください。

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  ValidateInputAttribute をより細かく制御

ASP.NET MVC には、受信要求に、可能性のある悪意のある入力が含まれていないことを確認するコアの ASP.NET 要求の検証インフラストラクチャを呼び出す ValidateInputAttribute クラスが常に含まれます。 既定では、入力の検証を有効にします。 次の例のように、ValidateInputAttribute 属性を使用して要求の検証を無効にすることができます。

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

ただし、多くの web アプリケーションでは、残りのフィールドがないときに、HTML を許可する必要がある個々 のフォーム フィールドがあります。 ValidateInputAttribute クラスでは、要求の検証に含まれないフィールドの一覧を指定できます。

たとえば、ブログ エンジンを開発している場合、フィールドの本文および概要のマークアップを許可する可能性があります。 これらのフィールドは、各プロパティの名前 ("Body"および「概要」) に対応する name 属性を持つ 2 つの入力要素によって表される可能性があります。 要求を無効にするのみ、これらのフィールドの検証が (コンマ区切り) 次の例のように、ValidateInput クラスの除外するプロパティの名前を指定します。

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  ヘルパーの匿名オブジェクトを使用して指定された HTML 属性名にアンダー スコアをハイフンに変換します。

ヘルパー メソッドを使用して、次の例のように、匿名オブジェクトを使用して属性の名前/値ペアを指定できます。

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

ハイフンは、ASP.NET でプロパティの名前を使用できないため、属性名でハイフンを使用するこのアプローチを使用しません。 ただし、ハイフンはカスタム HTML5 属性にとって重要です。たとえば、HTML5 では、「データの」プレフィックスが使用されます。

同時に、アンダー スコアは、HTML では、属性名は使用できませんが、プロパティ名で有効です。 そのため、匿名のオブジェクトを使用して属性を指定して、属性名がアンダー スコアを含める場合は、およびに、ヘルパー メソッドは、アンダー スコアをハイフンに変換されます。 たとえば、次のヘルパー構文は、アンダー スコアを使用します。

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

前の例は、ヘルパーの実行時に、次のマークアップを表示します。

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  バグの修正

EditorFor と DisplayFor テンプレート ヘルパーの既定のオブジェクト テンプレートは、DisplayAttribute.Order プロパティで指定された順序付けできるようになりました。 (以前のバージョンで順序の設定が使用されません。)

クライアント検証をここでは、検証属性が適用されているオーバーライドされたプロパティの検証をサポートします。

JsonValueProviderFactory は既定で登録されているようになりました。

## <a id="0.1__Toc274034229"></a>  重大な変更

例外フィルターの実行の順序には、同じ順序の値を持つ例外フィルターが変更されました。 ASP.NET MVC 2 以降では、上の例外フィルターと同じ順序で、コント ローラー アクション メソッドで例外フィルターの前に実行されたアクション メソッドのようにします。 指定された順序値を指定せずに例外フィルターが適用されたときにこの通常される場合。 ASP.NET MVC 3 では、この順序が逆になりました最も固有の例外ハンドラーが最初に実行できるようにします。 以前のバージョンと順序のプロパティが明示的に指定されている場合、フィルターが指定した順序で実行されます。

## <a id="0.1__Toc274034230"></a>  既知の問題

インストール中に、使用許諾契約書への同意 ダイアログ ボックスのものよりも小さなウィンドウで、ライセンス条項が表示されます。

Razor ビューには、IntelliSense のサポートも構文の強調表示はありません。 Visual Studio での Razor 構文のサポートを今後のリリースの一部として含めるされると予想されます。

Razor ビュー (CSHTML ファイル) を編集するとき、 <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Visual Studio でのコント ローラーに移動 メニュー項目を使用可能にすることはできませんし、コード スニペットではありません。

使用する場合、@model厳密に型指定された CSHTML を指定する構文を表示、言語固有のショートカットの種類が認識されません。 たとえば、 @model int は機能しませんが、 @model Int32 は動作します。 このバグの回避策では、モデルの種類を指定する場合は、実際の型名を使用します。

使用する場合、@model厳密に型指定された CSHTML ビューを指定するための構文 (または@ModelTypeVBHTML 厳密に型指定されたビューを指定)、null 許容型と配列の宣言がサポートされていません。 たとえば、 @model int? はサポートされていません。 代わりに、`@model Nullable<Int32>`します。 構文@modelstring[] もサポートされていません。 代わりに、`@model IList<string>`します。

ASP.NET MVC 2 プロジェクトを ASP.NET MVC 3 にアップグレードする場合は、Web.config ファイルの appSettings セクションに、次を追加することを確認してください。

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

フォーム認証を常に認証されていないユーザーをリダイレクト ~/Account/ログイン、Web.config で使用されるフォーム認証設定を無視するを原因となる既知の問題があります。回避策では、次のアプリ設定を追加します。

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  免責事項

© 2011 Microsoft Corporation. All rights reserved. このドキュメントが提供される"としてのです"。 情報および見解 URL やその他のインターネット Web サイトの参照を含め、このドキュメントでは、予告なく変更可能性があります。 お客様は、その使用に関するリスクを負うものとします。

このドキュメントは、Microsoft 製品の知的財産権に関する法的な権利をお客様に許諾するものではありません。 内部的な参照目的に限り、このドキュメントを複製して使用することができます。
