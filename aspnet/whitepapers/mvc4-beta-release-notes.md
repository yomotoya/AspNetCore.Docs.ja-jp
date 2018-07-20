---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816692"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。
> 
> > [!NOTE]
> > これは、最新のリリースではありません。 ASP.NET MVC 4 RC のリリース ノートは[ここ](mvc4-release-notes.md)します。


- [インストールに関する注記](#_Toc303253802)
- [ドキュメント](#_Toc303253803)
- [サポート](#_Toc303253804)
- [ソフトウェアの要件](#_Toc303253805)
- [ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。](#_Toc303253806)
- [ASP.NET MVC 4 Beta の新機能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET Single Page Application](#_Toc317096198)
    - [既定のプロジェクト テンプレートの機能強化](#_Toc303253808)
    - [モバイル プロジェクト テンプレート](#_Toc303253809)
    - [表示モード](#_Toc303253810)
    - [jQuery Mobile、ビュー スイッチャー、およびブラウザーのオーバーライド](#_Toc303253811)
    - [Visual Studio でのコード生成のレシピ](#_Toc303253812)
    - [非同期コント ローラーのタスクのサポート](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [既知の問題と重大な変更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>インストールに関する注記

Visual Studio 2010 の ASP.NET MVC 4 Beta をインストールすることができます、[ホーム ページの ASP.NET MVC 4](../mvc/mvc4.md) Web Platform Installer を使用します。

ASP.NET MVC 4 Beta をインストールする前に、ASP.NET MVC 4 の以前にインストールのプレビューをアンインストールする必要があります。

このリリースは、.NET Framework 4.5 Developer Preview と互換性がありません。 ASP.NET MVC 4 Beta をインストールする前に、.NET 4.5 Developer Preview をアンインストールする必要があります。

ASP.NET MVC 4 インストールでき、サイド バイ サイドでを実行できる ASP.NET MVC 3 でします。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC に関する次の URL で MSDN web サイトで提供されています。

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 のページで使用可能な ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>サポート

これは、プレビュー リリースであり、正式にサポートされていません。 このリリースの操作について質問等がございましたら、ASP.NET MVC フォーラムに投稿 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))、ASP.NET コミュニティのメンバーが頻繁に、非公式のサポートを提供することができます。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Visual Studio 2010 Service Pack 1 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。

ASP.NET MVC 4 は、ASP.NET MVC 4、ASP.NET MVC 3 アプリケーション アップグレードするときに選択できる柔軟性を提供する同じコンピューター上の ASP.NET MVC 3 サイドでインストールできます。

アップグレードする最も簡単な方法は、新しい ASP.NET MVC 4 プロジェクトとコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツ ファイルの既存の MVC 3 プロジェクトから新しいプロジェクトに、古いプロジェクトと一致する新しいプロジェクトで参照アセンブリを更新するのには、です。 MVC 3 プロジェクトの Web.config ファイルに変更を加えた場合は、MVC 4 プロジェクトの Web.config ファイルにもこれらの変更をマージする必要があります。

既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。

1. (は Views フォルダー、およびプロジェクト内の各領域の Views フォルダー内のプロジェクトのルートに 1 つずつ) プロジェクトのすべての Web.config ファイルでは、次のテキストのすべてのインスタンスを置き換えます。

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    次の対応するテキスト。

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. ルートの Web.config ファイルで更新、 *webPages:Version*要素「2.0.0.0」を新しい追加*PreserveLoginUrl*値"true"を持つキー。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (どの地点をバージョン 3 の DLL) です。 参照を追加し、 *System.Web.Mvc* (v4.0.0.0)。 具体的には、アセンブリ参照を更新する次の変更を行います。 その詳細な手順を次に示します。

    1. ソリューション エクスプ ローラーで、次のアセンブリへの参照を削除します。 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. 次のアセンブリへの参照を追加します。 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 名前をもう一度右クリックし、編集*ProjectName*.csproj します。
5. 検索、 *ProjectTypeGuids*要素と置換 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。
7. ルートの Web.config ファイルを開き、プロジェクトでは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*下の要素、 *構成*セクション。 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta の新機能

このセクションが導入された機能について説明します、ASP.NET MVC 4 Beta リリースにします。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

HTTP サービスを作成するための新しいフレームワークは、さまざまなブラウザーやモバイル デバイスなどのクライアントに到達できる、ASP.NET MVC 4 には、ASP.NET Web API にはようになりましたが含まれます。 ASP.NET Web API は RESTful サービスを構築するための理想的なプラットフォームではもです。

ASP.NET Web API には、次の機能のサポートが含まれています。

- **最新の HTTP プログラミング モデル:** 直接アクセスし、HTTP 要求と応答を返す新しい、厳密に型指定された HTTP オブジェクト モデルを使用して、Web Api を操作します。 同じプログラミング モデルと HTTP パイプラインは、新しい種類の HttpClient を介してクライアントで対称的に使用します。
- **完全にサポート ルート**: Web Api では、常に、ルート パラメーターや制約を含む、Web スタックの一部であったルート機能の完全なセットをサポートします。 さらに、アクションへのマッピングに、規則の完全なサポート [HttpPost] などの属性を適用するが不要になったため、クラスとメソッド。
- **コンテンツ ネゴシエーション**: クライアントとサーバーが連携 API から返されるデータの適切な形式を判断します。 XML、JSON、およびフォームの URL でエンコードされた形式では、既定のサポートを提供して、独自のフォーマッタを追加することでこのサポートを拡張または既定のコンテンツ ネゴシエーション戦略を置き換えることもできます。
- **モデル バインドと検証:** モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できる .NET オブジェクトに変換する簡単な方法を提供します。
- **フィルター:** Web Api では、[Authorize] 属性などのよく知られているフィルターなどのフィルターをサポートします。 作成し、操作、承認、および例外処理フィルターを接続できます。
- **クエリの構成:** IQueryable を返すだけで&lt;T&gt;、Web API は OData URL 表記規則を使用してクエリをサポートします。
- **HTTP の詳細のテストの容易性の向上:** 静的コンテキスト オブジェクトでは、HTTP の詳細を設定するのではなく、Web API アクションは HttpRequestMessage と HttpResponseMessage のインスタンスを持つ作業ようになりましたことができます。 これらのオブジェクトのジェネリック バージョンは、HTTP の種類に加えて、カスタムの型を操作するためにも存在します。
- **制御の反転 (IoC) DependencyResolver 経由での改善:** Web API は、多くのさまざまな機能のインスタンスを取得する MVC の依存関係競合回避モジュールによって実装されるサービス ロケーターのパターンを使用するようになりました。
- **コード ベースの構成:** コードを通じてのみ Web API の構成には、ファイルのクリーンアップの設定のままです。
- **自己ホスト:** Web Api は、ルートの完全な機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。

ASP.NET Web API の詳細についてを参照してくださいの[ https://www.asp.net/web-api](../web-api/index.md)します。

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET Single Page Application

ASP.NET MVC 4 には、JavaScript、および Web Api を使用して重要なクライアント側の対話をシングル ページ アプリケーションを構築するためのエクスペリエンスの早期プレビュー版が含まれています。 このサポートは次のとおりです。

- キャッシュされたデータとローカルのやり取りを高度な JavaScript ライブラリのセット
- 作業単位と DAL サポートの追加の Web API コンポーネント
- すぐに開始するためにスキャフォールディングでの MVC プロジェクト テンプレート

シングル ページ アプリケーションの詳細については、ASP.NET MVC 4 のサポートを参照してください[ https://www.asp.net/single-page-application](../single-page-application/index.md)します。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>既定のプロジェクト テンプレートの機能強化

現代的な外観の web サイトを作成する、新しい ASP.NET MVC 4 プロジェクトを作成するために使用するテンプレートが更新されました。

![](mvc4-beta-release-notes/_static/image1.png)

表面的な機能強化、だけでなく向上には、新しいテンプレートの機能がいます。 テンプレートでは、デスクトップ ブラウザーとモバイル ブラウザーのカスタマイズの両方でスケッチをアダプティブ レンダリングと呼ばれる手法を採用しています。

![](mvc4-beta-release-notes/_static/image2.png)

アダプティブ レンダリングの動作を表示するはモバイル エミュレーターを使用したり小さくなるように、デスクトップ ブラウザー ウィンドウのサイズを変更してみてください。 ブラウザー ウィンドウが十分に小さいと、ページのレイアウトが変わります。

既定のプロジェクト テンプレートを別の拡張機能より豊富な UI を提供する JavaScript の使用です。 テンプレートで使用されるログインと登録のリンクは、jQuery UI ダイアログを使用して、豊富なログイン画面を表示する方法の例を示します。

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>モバイル プロジェクト テンプレート

新しいプロジェクトを開始しているモバイル専用のサイトとタブレットのブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。 これは、jQuery Mobile、タッチに最適化された UI を構築するため、オープン ソース ライブラリに基づいています。

![](mvc4-beta-release-notes/_static/image4.png)

このテンプレートには、インターネット アプリケーション テンプレートと同じアプリケーションの構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示して、タッチ ベースのモバイル デバイスで適切に動作するスタイルを設定します。 構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)します。

デスクトップ指向のサイト、モバイルに最適化されたビューを追加するデスクトップとモバイル ブラウザー スタイルの異なるビューを提供する 1 つのサイトを作成する場合に既にある場合は、新しい表示モード機能を使用できます。 (詳しくは、次のセクションを参照してください)。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>表示モード

新しい表示モード機能は、要求を行っているブラウザーによってビューを選択してアプリケーションを使用できます。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求する場合に、アプリケーションは Views\Home\Index.cshtml テンプレートを使用可能性があります。 モバイル ブラウザーでは、ホーム ページを要求するアプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す可能性があります。

レイアウト、パーシャルは、特定のブラウザーの種類を無効にできます。 例えば:

- Views \shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイルブラウザーからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求中にします。
- フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_MyPartial.mobile.cshtml 携帯電話からの要求時にブラウザー、および\_MyPartial.cshtml 他の要求中にします。

新しい登録することがより特定のビュー、レイアウト、またはその他のデバイス部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たすときに検索する名前を指定するインスタンス。 次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザー要求を作成するときに適用される表示モードとして文字列"iPhone"を登録する Global.asax ファイル内のメソッド。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

アプリケーションで、views \shared を使用して、Apple iPhone ブラウザー要求を作成するときにこのコードが実行された後、\\_Layout.iPhone.cshtml レイアウト (存在する場合)。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile、ビュー スイッチャー、およびブラウザーのオーバーライド

jQuery Mobile は、タッチに最適化された web UI を構築するためのオープン ソース ライブラリです。 ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用する場合は、ダウンロードして開始するのに役立つ NuGet パッケージをインストールできます。 Visual Studio パッケージ マネージャー コンソールからインストールするには、次のコマンドを入力します。

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

これには、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。

- Views/shared/\_Layout.Mobile.cshtml、jQuery Mobile ベースのレイアウトであります。
- ビュー/共有で構成されるビュー スイッチャーのコンポーネント/\_ViewSwitcher.cshtml 部分ビューと ViewSwitcherController.cs コント ローラー。

パッケージをインストールした後は、モバイル ブラウザーを使用してアプリケーションを実行 (または、Firefox など、同等の[ユーザー エージェント切り替え](http://chrispederick.com/work/user-agent-switcher/)アドオン)。 JQuery モバイル レイアウトを処理するため、ページ検索は、まったく異なることがわかりますおよびスタイル設定します。 これを活用するには、次の操作を行うことができます。

- モバイル専用ビューの上書きを作成する」の説明に従って[表示モード](#_Toc303253810)以前 (たとえば、モバイル ブラウザーの Views\Home\Index.cshtml をオーバーライドする Views\Home\Index.mobile.cshtml を作成します)。
- 読み取り、 [jQuery モバイル ドキュメント](http://jquerymobile.com/)モバイル ビューで、タッチに最適化された UI 要素を追加する方法の詳細を表示します。

モバイルに最適化された web ページ用の規則では、デスクトップ ビューなど、ページのデスクトップ バージョンに切り替えることができますサイトの完全なモードはテキストがリンクを追加します。 JQuery.Mobile.MVC パッケージには、この目的のためのサンプル ビュー スイッチャーのコンポーネントが含まれています。 Views \shared 既定で使用される\\_Layout.Mobile.cshtml ビュー、およびページがレンダリングされるようになります。

![](mvc4-beta-release-notes/_static/image5.png)

ユーザーは、リンクをクリックする、同じページのデスクトップ バージョンに切り替えられますしています。

デスクトップ レイアウトには既定のビューの切り替え機能が含まれていないために、訪問者はモバイルのモードを取得する方法を必要はありません。 これを有効にするには、次の参照を追加 *\_ViewSwitcher* 、デスクトップ レイアウトに内部でだけ、 *&lt;本文&gt;* 要素。

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

ビュー スイッチャーは、ブラウザーのオーバーライドと呼ばれる新しい機能を使用します。 この機能により、アプリケーションで送られてきますかのように要求を扱うよりも、別のブラウザー (ユーザー エージェント) から、あなたが実際にします。 次の表では、ブラウザーのオーバーライドを提供する方法を示します。

| `HttpContext.SetOverriddenBrowser(userAgentString)` | 指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。 |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | オーバーライドが指定されていない場合は、要求のユーザー エージェントの上書き値、または、実際のユーザー エージェント文字列を返します。 |
| `HttpContext.GetOverriddenBrowser()` | 返します、 *HttpBrowserCapabilitiesBase*要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライド) します。 この値を使用して、プロパティを取得します。 *IsMobileDevice*します。 |
| `HttpContext.ClearOverriddenBrowser()` | 現在の要求でオーバーライドされたユーザー エージェントを削除します。 |

ブラウザー オーバーライドは ASP.NET MVC 4 のコア機能でありは、jQuery.Mobile.MVC パッケージをインストールしていない場合でも使用できます。 影響ただし、ビュー、レイアウト、および部分ビューの選択-その他の ASP.NET 機能に依存するのには影響しません、 *Request.Browser*オブジェクト。

既定では、cookie を使用して、ユーザー エージェントの上書きが格納されます。 既定のプロバイダーを置き換えることができます (たとえば、データベース) に別の場所の上書きを保存する場合は、(*BrowserOverrideStores.Current*)。 このプロバイダーのドキュメントは、ASP.NET MVC の今後のリリースと共に使用できるになります。

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio でのコード生成のレシピ

新しいレシピ機能は、NuGet を使用してをインストールするパッケージに基づくソリューションに固有のコードを生成する Visual Studio を使用できます。 レシピのフレームワークを簡単に書き込むコード生成のプラグインは、領域の追加、コント ローラーの追加、およびビューの追加の組み込みコード ジェネレーターを置換する使用することもできます。 開発者向け。 レシピは NuGet パッケージとして展開するため、簡単にソース管理にチェックインでき自動的にプロジェクトのすべての開発者と共有します。 これらは、ソリューションごとに入手できます。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同期コント ローラーのタスクのサポート

これで記述できる非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;* します。

たとえば、Visual c# 5 を使用している場合 (またはを使用して、 [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx))、次のような非同期アクション メソッドを作成することができます。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

前のアクション メソッドへの呼び出しで*newsService.GetHeadlinesAsync*と*sportsService.GetScoresAsync*非同期に呼び出され、スレッド プールからスレッドをブロックされません。

非同期アクション メソッドが返す*タスク*インスタンスは、タイムアウトもサポートできます。 アクション メソッドをキャンセルするためには、型のパラメーターを追加*CancellationToken*アクション メソッドのシグネチャにします。 次の例では、2500 時間をミリ秒単位のタイムアウトを持つし、表示する非同期アクション メソッドを*TimedOut*タイムアウトが発生した場合、クライアントに表示します。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta には、Windows Azure SDK の 2011 年 9 月 1.5 のリリースがサポートしています。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

- **ASP.NET MVC 4 Beta をインストールした後、CSHTML と VBHTML エディターが Visual Studio 2010 Service Pack 1 CSHTML と VBHTML エディターでは可能性があります cshtml"や"vbhtml ファイル内で作成するか、JavaScript を入力した後に長時間一時停止します。** これは、作成された直後、まだコンパイルされていないと ASP.NET MVC 4 アプリケーションでのみ発生します。

    回避策では、bin フォルダーにアセンブリを取得するプロジェクトをコンパイルします。 エディターの問題が返される、アセンブリを bin フォルダーから削除され、プロジェクトをクリーンアップする場合に注意してください。

    これは、次のリリースで修正される予定です。
- **Visual Studio 11 Beta 用の c# プロジェクト テンプレートには、global.asax.cs、不適切な接続文字列が含まれます。** アプリケーションで指定された既定の接続\_Start メソッドを Visual Studio 11 Beta で作成されたプロジェクトにエスケープされていない円記号を含む LocalDB 接続文字列が含まれています (\)文字。 これは、結果、接続エラー、SqlException を生成する Entity Framework の DbContext にアクセスしようとします。

    この問題を修正するには、アプリでバック スラッシュ文字をエスケープ\_Global.asax.cs のメソッドを起動して、そこに次のようにします。

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 を対象とする ASP.NET MVC 4 アプリケーションが、FileLoadException System.Net.Http.dll アセンブリが .NET 4.0 で実行する場合にアクセスしようとするとスローされます。** .NET 4.5 で作成された ASP.NET MVC 4 アプリケーションには、バインドが含まれているという FileLoadException 原因となるリダイレクト「を読み込めませんでしたファイルまたはアセンブリ 'System.Net.Http' またはその依存関係の 1 つ」。 ときに、アプリケーションは、.NET 4.0 がインストールされたシステムで実行されます。 この問題を修正するには、web.config ファイルから次のバインド リダイレクトを削除します。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    変更した web.config 内のアセンブリのバインド要素には、次のように表示されます。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Visual Basic プロジェクトで「コント ローラーの追加」の項目テンプレートが、正しくない名前空間が呼び出されたときに生成されます</strong><strong>から領域内で。</strong> Visual Basic を使用する ASP.NET MVC プロジェクト内の領域に、コント ローラーを追加すると、項目テンプレートは、コント ローラーに間違った名前空間を挿入します。 コント ローラーでのアクションに移動すると、「ファイルが見つかりません」エラーになります。  
  
  生成された名前空間は、ルート名前空間の後にすべてを省略します。 たとえば、生成された名前空間は*RootNamespace*する必要がありますが、 *RootNamespace.Areas.AreaName.Controllers*します。
- **Razor ビュー エンジンにおける重大な変更。** Razor パーサーの書き換えの一環として、次の種類はから削除された*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  次のメソッドも削除されます。 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll が、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに含まれる、フォーム認証の URL にかかります。** ログオン/アカウントへのリダイレクトを認証ログインが上書きされます (たとえば、配置可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択) して WebMatrix.WebData.dll アセンブリをアプリケーションに追加するのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。 この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用して、PreserveLoginUrl と呼ばれる、appSetting を追加および true に設定できます。 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet パッケージ マネージャーは、Visual Studio 2010 および Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールするときのインストールに失敗します。** Visual Studio 2010 と ASP.NET MVC 4 と並んで、Visual Web Developer 2010 を実行するには、両方のバージョンの Visual Studio を既にインストールした後、ASP.NET MVC 4 をインストールする必要があります。
- **前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。** ASP.NET MVC を完全にアンインストールするには、4you は Visual Studio をアンインストールする前に、ASP.NET MVC 4 をアンインストールする必要があります。
- **既定の Web API プロジェクトを実行すると、存在しない RegisterApis メソッドを使用してルートを追加するユーザーを正しく指示する命令が表示されます。** ASP.NET のルート テーブルを使用して、RegisterRoutes メソッドでは、ルートを追加する必要があります。
- **ASP.NET MVC 4 Beta をインストールするには、ASP.NET MVC 3 RTM アプリケーションが中断されます。** 作成された ASP.NET MVC 3 アプリケーション (ASP.NET MVC 3 Tools Update リリース) ではなくリリースは RTM で、ASP.NET MVC 4 Beta と並行して動作させるには、次の変更を必要とします。 これらの更新プログラムの結果にコンパイル エラーが発生せず、プロジェクトをビルドします。 

    **必要な更新プログラム**

  1. ルートの Web.config ファイルでは、新しい追加*&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*値*1.0.0.0*します。

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 名前をもう一度右クリックし、編集*ProjectName*.csproj します。
  3. 次のアセンブリ参照を見つけます。 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      次のように、それらを置き換えます。

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. 変更を保存、編集していたし、プロジェクトを右クリックして選択して再読み込み、プロジェクト (.csproj) ファイルを閉じます。
