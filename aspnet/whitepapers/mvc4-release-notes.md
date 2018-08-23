---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、ASP.NET MVC 4 のリリースについて説明します。
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: a4f78061850ef5ad8c3381daafdb5ea6bca4cb2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837171"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> このドキュメントでは、ASP.NET MVC 4 のリリースについて説明します。


- [インストールに関する注記](#_Toc303253802)
- [ドキュメント](#_Toc303253803)
- [サポート](#_Toc303253804)
- [ソフトウェアの要件](#_Toc303253805)
- [ASP.NET MVC 4 の新機能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [既定のプロジェクト テンプレートの機能強化](#_Toc303253808)
    - [モバイル プロジェクト テンプレート](#_Toc303253809)
    - [表示モード](#_Toc303253810)
    - [jQuery Mobile、ビュー スイッチャー、およびブラウザーのオーバーライド](#_Toc303253811)
    - [非同期コント ローラーのタスクのサポート](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [データベースの移行](#_Toc303253818)
    - [空のプロジェクト テンプレート](#_Toc303253819)
    - [コント ローラーを任意のプロジェクト フォルダーに追加します。](#_Toc303253820)
    - [バンドルと縮小](#_Toc303253821)
    - [Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。](#_Toc303253822)
- [ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。](#_Toc303253806)
- [ASP.NET MVC 4 のリリース候補からの変更](#_Toc303253817)
- [既知の問題と重大な変更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>インストールに関する注記

Visual Studio 2010 の ASP.NET MVC 4 をインストールすることができます、[ホーム ページの ASP.NET MVC 4](../mvc/mvc4.md) Web Platform Installer を使用します。

ASP.NET MVC 4 をインストールする前に、ASP.NET MVC 4 の以前にインストールのプレビューをアンインストールすることをお勧めします。 ASP.NET MVC 4 Beta および Release Candidate は、アンインストールせずに ASP.NET MVC 4 にアップグレードできます。

このリリースは、.NET Framework 4.5 の任意のプレビュー リリースと互換性がありません。 ASP.NET MVC 4 をインストールする前に、最終バージョンに .NET Framework 4.5 のインストール済みのプレビュー リリースをアップグレードすることとは別にする必要があります。

ASP.NET MVC 4 がインストールされ、実行のサイド ASP.NET MVC 3 を指定できます。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC に関する次の URL で MSDN web サイトで提供されています。

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 のページで使用可能な ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>サポート

ASP.NET MVC 4 は完全にサポートします。 このリリースの使用に関する質問がある場合は、投稿することできますもに、ASP.NET MVC フォーラムに ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))、ASP.NET コミュニティのメンバーが頻繁に、非公式のサポートを提供することができます。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Visual Studio 2010 Service Pack 1 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4 の新機能

このセクションが導入された機能について説明します、ASP.NET MVC 4 リリースでします。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 には、ASP.NET Web API、広範なブラウザーやモバイル デバイスなどのクライアントにアクセスできる HTTP サービスを作成するための新しいフレームワークが含まれています。 ASP.NET Web API は RESTful サービスを構築するための理想的なプラットフォームではもです。

ASP.NET Web API には、次の機能のサポートが含まれています。

- **最新の HTTP プログラミング モデル:** 直接アクセスし、HTTP 要求と応答を返す新しい、厳密に型指定された HTTP オブジェクト モデルを使用して、Web Api を操作します。 同じ対称的には、新しいクライアントで使用可能なプログラミング モデルと HTTP パイプラインが*HttpClient*型。
- **完全なルートのサポート:** ASP.NET Web API は、ルート パラメーターや制約など、ASP.NET のルーティングのルートの機能の完全なセットをサポートしています。 さらに、シンプルな規約を使用して、HTTP メソッドにアクションをマップします。
- **コンテンツ ネゴシエーション:** クライアントとサーバーが連携 web API から返されるデータの適切な形式を判断します。 ASP.NET Web API は、XML、JSON、既定のサポートを提供し、フォームの URL でエンコードされた形式とすることができます独自のフォーマッタを追加することでこのサポートを拡張または既定のコンテンツ ネゴシエーション戦略を置き換えることもします。
- **モデル バインドと検証:** モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できる .NET オブジェクトに変換する簡単な方法を提供します。 検証は、データ注釈に基づいてアクション パラメーターにも実行されます。
- **フィルター:** ASP.NET Web API など、よく知られているフィルターを含むフィルターをサポートしている、 *[Authorize]* 属性。 作成し、操作、承認、および例外処理フィルターを接続できます。
- **クエリの構成:** 使用、 *[Queryable]* を返すアクション フィルター属性*IQueryable* OData のクエリ規則を使用して、web API を照会するためのサポートを有効にします。
- **テストの容易性の向上:** 静的コンテキスト オブジェクトでは、HTTP の詳細を設定するのではなく web API アクションの作業のインスタンスと*HttpRequestMessage*と*HttpResponseMessage*します。 Web API の機能の単体テストをすばやく記述を開始する Web API プロジェクトと単体テスト プロジェクトを作成します。
- **コード ベースの構成:** コードを通じてのみ ASP.NET Web API の構成には、ファイルのクリーンアップの設定のままです。 機能拡張ポイントを構成するのにには、指定されたサービス ロケーターのパターンを使用します。
- **制御の反転 (IoC) コンテナーのサポート強化:** ASP.NET Web API が強化された依存関係競合回避モジュールの抽象化を通じて IoC コンテナーの優れたサポートを提供します
- **自己ホスト:** Web Api は、ルートの完全な機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。
- **カスタム ヘルプを作成し、ページのテスト:** するようになりましたことができ、簡単にカスタム ヘルプの作成、新しいを使用して web Api のページのテスト*IApiExplorer*サービス、web Api の完全なランタイムの説明を取得します。
- **監視と診断:** ASP.NET Web API は今すぐを System.Diagnostics、ETW およびサード パーティ製のログ記録フレームワークなどの既存のログ記録ソリューションと統合するが容易にする軽量のトレース インフラストラクチャを提供します。 提供することでトレースを有効にすることができます、 *ITraceWriter*実装と、web API の構成に追加します。
- **リンクの生成:** ASP.NET Web API を使用して、 *UrlHelper*同じアプリケーション内の関連リソースへのリンクを生成します。
- **Web API プロジェクト テンプレート:** 新しい Web API プロジェクトのフォームをすぐに稼働している ASP.NET Web API を使用した新しい MVC 4 プロジェクト ウィザードを選択します。
- **スキャフォールディング:** 使用、**コント ローラーの追加**迅速に、Entity Framework に基づく web API コント ローラーをスキャフォールディングするためのダイアログ ベースのモデルの型。

ASP.NET Web API の詳細についてを参照してくださいの[ https://www.asp.net/web-api](../web-api/index.md)します。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>既定のプロジェクト テンプレートの機能強化

現代的な外観の web サイトを作成する、新しい ASP.NET MVC 4 プロジェクトを作成するために使用するテンプレートが更新されました。

![](mvc4-release-notes/_static/image1.png)

表面的な機能強化、だけでなく向上には、新しいテンプレートの機能がいます。 テンプレートでは、デスクトップ ブラウザーとモバイル ブラウザーのカスタマイズの両方でスケッチをアダプティブ レンダリングと呼ばれる手法を採用しています。

![](mvc4-release-notes/_static/image2.png)

アダプティブ レンダリングの動作を表示するはモバイル エミュレーターを使用したり小さくなるように、デスクトップ ブラウザー ウィンドウのサイズを変更してみてください。 ブラウザー ウィンドウが十分に小さいと、ページのレイアウトが変わります。

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>モバイル プロジェクト テンプレート

新しいプロジェクトを開始しているモバイル専用のサイトとタブレットのブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。 これは、jQuery Mobile、タッチに最適化された UI を構築するため、オープン ソース ライブラリに基づいています。

![](mvc4-release-notes/_static/image3.png)

このテンプレートには、インターネット アプリケーション テンプレートと同じアプリケーションの構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示して、タッチ ベースのモバイル デバイスで適切に動作するスタイルを設定します。 構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)します。

デスクトップ指向のサイト、モバイルに最適化されたビューを追加するデスクトップとモバイル ブラウザー スタイルの異なるビューを提供する 1 つのサイトを作成する場合に既にある場合は、新しい表示モード機能を使用できます。 (詳しくは、次のセクションを参照してください)。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>表示モード

新しい表示モード機能は、要求を行っているブラウザーによってビューを選択してアプリケーションを使用できます。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求する場合に、アプリケーションは Views\Home\Index.cshtml テンプレートを使用可能性があります。 モバイル ブラウザーでは、ホーム ページを要求するアプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す可能性があります。

レイアウト、パーシャルは、特定のブラウザーの種類を無効にできます。 例えば:

- Views \shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイルブラウザーからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求中にします。
- フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_MyPartial.mobile.cshtml 携帯電話からの要求時にブラウザー、および\_MyPartial.cshtml 他の要求中にします。

新しい登録することがより特定のビュー、レイアウト、またはその他のデバイス部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たすときに検索する名前を指定するインスタンス。 次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザー要求を作成するときに適用される表示モードとして文字列"iPhone"を登録する Global.asax ファイル内のメソッド。

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

アプリケーションで、views \shared を使用して、Apple iPhone ブラウザー要求を作成するときにこのコードが実行された後、\\_Layout.iPhone.cshtml レイアウト (存在する場合)。 表示モードの詳細については、次を参照してください。 [ASP.NET MVC 4 モバイル機能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)します。 DisplayModeProvider を使用してアプリケーションをインストールする必要があります、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet パッケージ。 [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322)が含まれています、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet パッケージで新しいプロジェクト テンプレート。 参照してください[ASP.NET MVC 4 モバイル キャッシュ バグ Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile と Mobile 機能

ASP.NET MVC 4 でモバイル アプリケーションを構築する方法についての jQuery Mobile を使用してチュートリアルを参照、 [ASP.NET MVC 4 モバイル機能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)します。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同期コント ローラーのタスクのサポート

これで記述できる非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;* します。

 詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)します。

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 には、Windows Azure sdk 1.6 以降のリリースがサポートされています。

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>データベースの移行

これで、ASP.NET MVC 4 プロジェクトには、Entity Framework 5 が含まれます。 Entity Framework 5 の優れた機能の 1 つは、データベースを移行するためのサポートです。 この機能では、データベース内のデータを維持しながらコードを重視した移行を使用して、データベース スキーマを簡単に進化することができます。 データベースの移行の詳細については、次を参照してください。 [Movie モデルとテーブルに新しいフィールドを追加](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md)で、 [ASP.NET MVC 4 のチュートリアルの概要](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)します。

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>空のプロジェクト テンプレート

完全にクリーンな状態から開始できるように、空の MVC プロジェクト テンプレートは本当に空ではようになりました。 空のプロジェクト テンプレートの以前のバージョンは、基本的に変更されています。

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>コント ローラーを任意のプロジェクト フォルダーに追加します。

ここで右クリックし、選択できる**コント ローラーの追加**MVC プロジェクトで任意のフォルダーから。 これによりより柔軟に別々 のフォルダーで、MVC と Web API のコント ローラーを維持する方法を含め、希望するコント ローラーを整理できます。

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>バンドルと縮小

バンドルと縮小フレームワークでは、Web ページは、スクリプトや CSS バンドルされている 1 つのファイルに個別のファイルを組み合わせることでする必要がある HTTP 要求の数を削減することができます。 バンドルのコンテンツを縮小して、それらの要求の全体的なサイズを小さくできます。 縮小すると、短縮することも、セマンティクスに基づく CSS セレクターを折りたたみ、変数名に空白文字を排除することのようなアクティビティを含めることができます。 バンドルの宣言し、で構成されているコードとは簡単に参照されているいずれかを生成するヘルパー メソッドを使用してビューでバンドルを 1 つのリンク、または複数のバンドルの個々 のコンテンツにリンク、デバッグ時にします。 詳細については、次を参照してください。[バンドルと縮小](../mvc/overview/performance/bundling-and-minification.md)します。

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。

ASP.NET MVC 4 のインターネットのプロジェクト テンプレートの既定のテンプレートには、DotNetOpenAuth ライブラリを使用して OAuth と OpenID のログインのサポートが含まれています。 OAuth または OpenID プロバイダーの構成方法の詳細については、次を参照してください。 [WebForms、MVC、web ページの Oauth/openid サポート](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx)と[OAuth と OpenID の機能のドキュメントでは、ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup)します。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。

ASP.NET MVC 4 は、ASP.NET MVC 4、ASP.NET MVC 3 アプリケーション アップグレードするときに選択できる柔軟性を提供する同じコンピューター上の ASP.NET MVC 3 サイドでインストールできます。

アップグレードする最も簡単な方法は、新しい ASP.NET MVC 4 プロジェクトとコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツ ファイルの既存の MVC 3 プロジェクトから新しいプロジェクトにし、任意の非 MVC テンプレートに一致する新しいプロジェクトで参照アセンブリを更新します使用している著名アセンブリ。 MVC 3 プロジェクトの Web.config ファイルに変更を加えた場合は、MVC 4 プロジェクトの Web.config ファイルにもこれらの変更をマージする必要があります。

既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。

1. すべての Web.config では、(は Views フォルダー、およびプロジェクト内の各領域の Views フォルダー内のプロジェクトのルートに 1 つずつ) プロジェクトのファイルは、次のテキストのすべてのインスタンスを置き換えます (注: System.Web.WebPages、バージョン = 1.0.0.0 に含まれていない。作成されたプロジェクトで Visual Studio 2012)。 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    次の対応するテキスト。

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. ルートの Web.config ファイルで更新、 *webPages:Version*要素「2.0.0.0」を新しい追加*PreserveLoginUrl*値"true"を持つキー。 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. ソリューション エクスプ ローラーで、参照を右クリックし、NuGet パッケージの管理を選択します。 左側のウィンドウで次のように選択します。 **Online\NuGet オフィシャル パッケージ ソース**、し、次の更新。

    - ASP.NET MVC 4
    - (省略可能) jQuery、jQuery の検証と jQuery UI
    - (省略可能)Entity Framework
    - (Optonal)Modernizr
4. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 名前をもう一度右クリックし、編集*ProjectName*.csproj します。
5. 検索、 *ProjectTypeGuids*要素と置換 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。
7. ルートの Web.config ファイルを開き、プロジェクトでは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*下の要素、 *構成*セクション。 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>ASP.NET MVC 4 のリリース候補からの変更

ASP.NET MVC 4 の Release Candidate のリリース ノートはここにあります。

このリリースでの ASP.NET MVC 4 のリリース候補から大きな変更は、以下にまとめます。

- **コント ローラーの構成ごと:** ASP.NET Web API コント ローラーを実装するカスタム属性を割り当てることのできる*IControllerConfiguration*独自のフォーマッタ、操作セレクターとパラメーターのバインダーをセットアップするには. *HttpControllerConfigurationAttribute*が削除されました。
- **ルートのメッセージ ハンドラーごと:** 特定のルートを要求チェーンの最後のメッセージ ハンドラーを指定できます。 これにより、ルーティングを使用して、独自にディスパッチする乗り物に沿ってフレームワークのサポート (非*IHttpController*) エンドポイント。
- **進行状況の通知:** 、 *ProgressMessageHandler*アップロードされる要求エンティティとダウンロードされる応答エンティティの両方の進行状況の通知が生成されます。 このハンドラーを使用することはアップロード要求本文または応答本文をダウンロードするどの程度の追跡です。
- **プッシュのコンテンツ:** 、 *PushStreamContent*クラスは、データ プロデューサーが、要求または応答のストリームを使用して (同期または非同期) に直接書き込むたいシナリオを実現できます。 ときに、 *PushStreamContent*出力ストリームに関連付け、action デリゲート呼び出しデータをそのまま使用する準備ができました。 開発者は、書き込み時にストリームが完了したために必要なと閉じるいる限りのストリームに記述できます。 *PushStreamContent*ストリームの終わりを検出し、基になる非同期完了*タスク*内容を記述するためです。
- **エラー応答を作成する:** 使用、 *HttpError*を一貫しても保持しながら検証エラーや例外などからの情報のエラーを表す型、 *IncludeErrorDetailPolicy*. 使用して、新しい*CreateErrorResponse*でエラー応答を簡単に作成する拡張メソッド*HttpError*コンテンツとして。 *HttpError*コンテンツが完全にコンテンツをネゴシエートします。
- **削除 MediaRangeMapping:** メディア型の範囲は既定のコンテンツ ネゴシエーターによって処理されるようになりました。
- **単純型のパラメーターの既定のパラメーターのバインドが、[FromUri]:** ASP.NET Web API の既定のモデル バインドに使用される単純型のパラメーター バインドされているパラメーターの以前のリリース。 単純型のパラメーターの既定のパラメーターのバインドは、今すぐ *[FromUri]* します。
- **アクションの選択は、必要なパラメーター:** ASP.NET Web API でアクションの選択のみを選択しますアクション URI に由来するすべての必須パラメーターが指定されている場合。 パラメーターは、アクション メソッドのシグネチャの引数の既定値を提供することで、省略可能として指定できます。
- **HTTP パラメーター バインディングのカスタマイズ:** を使用して、 *ParameterBindingAttribute*を特定のアクション パラメーターのパラメーターのバインドをカスタマイズまたはを使用して、 *ParameterBindingRules*で、*HttpConfiguration*パラメーター バインドのカスタマイズをより広い意味で。
- **MediaTypeFormatter の機能強化:** フォーマッタに完全なへのアクセスがあるようになりました*HttpContent*インスタンス。
- **ホスト ポリシーの選択をバッファリング:** 実装し、構成、 *IHostBufferPolicySelector*にポリシーを使用するときに、バッファリングを有効にする ASP.NET Web API でサービス。
- **ホストに依存しない方法でクライアント証明書へのアクセス:** 使用、 *GetClientCertificate*要求メッセージから、指定されたクライアント証明書を取得する拡張メソッド。
- **コンテンツ ネゴシエーションの機能拡張:** から派生することによって、コンテンツ ネゴシエーションをカスタマイズ、 *DefaultContentNegotiator*したいコンテンツ ネゴシエーションのさまざまな面をオーバーライドします。
- **406 Not Acceptable 応答を返すためのサポート:** するようになりましたを返せる 406 Not Acceptable 応答で ASP.NET Web API を作成して適切なフォーマッタが見つからない場合に、 *DefaultContentNegotiator* で*excludeMatchOnTypeOnly*パラメーターに設定*true*します。
- **NameValueCollection または JToken としてフォーム データを読み取る:** URI クエリ文字列または要求本文としてフォーム データを読み取ることができます、 *NameValueCollection*を使用して、 *ParseQueryString*と*ReadAsFormDataAsync*拡張メソッドそれぞれします。 同様に、URI クエリ文字列または要求本文としてフォーム データを読み取ることができます、 *JToken*を使用して、 *TryReadQueryAsJson*と*ReadAsAsync*&lt;T&gt;拡張メソッドそれぞれします。
- **マルチパートの機能強化:** を記述することは今すぐ、 *MultipartStreamProvider*を完全にはマルチパート データを読み取り/ユーザーに最適な方法で結果を表示できることは、MIME の種類にお応えします。 後処理のステップをフックすることも、 *MultipartStreamProvider*処理で MIME マルチパート ボディ パーツがどのような投稿を行うに実装することができます。 など、 *MultipartFormDataStreamProvider*実装は、HTML フォーム データ部分を読み取るしに追加されます、 *NameValueCollection*簡単に、呼び出し元からの取得されるためです。
- **リンクの生成の機能強化:** 、 *UrlHelper*に依存しなく*HttpControllerContext*します。 今すぐアクセスすることができます、 *UrlHelper*任意のコンテキストからで、 *HttpRequestMessage*は使用できます。
- **メッセージのハンドラーの実行順序の変更:** の代わりに逆の順序で構成されている順序でメッセージのハンドラーが実行されるようになりました。
- **メッセージ ハンドラーを配線のヘルパー:** 新しい*HttpClientFactory*を接続する*表します*を作成し、 *HttpClient*で、目的のパイプラインを準備できます。 代替の内部ハンドラーを使用して接続の機能もあります (既定値は*HttpClientHandler*) を使用する場合配線を行うほか*HttpMessageInvoker*別または*DelegatingHandler*の代わりに*HttpClient*上部呼び出しとして。
- **ASP.NET Web の最適化の Cdn のサポート:** ASP.NET Web の最適化は、CDN の代替パスごとに指定できるようにバンドルのコンテンツ配信ネットワークで同じリソースを示す追加の URL のようになりましたサポートを提供します。 Cdn をサポートするには、Web アプリケーションのエンド コンシューマーに、スクリプトとスタイル バンドル地理的に近い場所を取得することができます。 実稼働アプリは、CDN が利用できない場合、フォールバックを実装する必要があります。 フォールバックをテストします。
- **ASP.NET Web API をルーティングし、構成を移動する*WebApiConfig.Register*テスト コードで再利用できる静的メソッド。** ASP.NET Web API のルートで追加された以前*RouteConfig.RegisterRoutes*と共に、標準の MVC ルーティングします。 既定の ASP.NET Web API のルーティングと構成が別で処理されるようになりました*WebApiConfig.Register*テストを容易にします。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

- **正しく、RC および RTM バージョンの ASP.NET MVC 4 は、モバイル ビューを返す必要があるときにキャッシュされたデスクトップ ビューに返さされません。**

    - 参照してください[ASP.NET MVC 4 モバイル キャッシュ バグ フィックス](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。 修正プログラムをインストールすることができます、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet パッケージ。
- **Razor ビュー エンジンにおける重大な変更**します。 次の種類はから削除された*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  次のメソッドも削除されます。 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll が、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに含まれる、フォーム認証の URL にかかります。** ログオン/アカウントへのリダイレクトを認証ログインが上書きされます (たとえば、配置可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択) して WebMatrix.WebData.dll アセンブリをアプリケーションに追加するのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。 この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用して、PreserveLoginUrl と呼ばれる、appSetting を追加および true に設定できます。 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet パッケージ マネージャーは、Visual Studio 2010 および Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールするときのインストールに失敗します。** Visual Studio 2010 と ASP.NET MVC 4 と並んで、Visual Web Developer 2010 を実行するには、両方のバージョンの Visual Studio を既にインストールした後、ASP.NET MVC 4 をインストールする必要があります。
- **前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。** ASP.NET MVC を完全にアンインストールするには、4you は Visual Studio をアンインストールする前に、ASP.NET MVC 4 をアンインストールする必要があります。
- **ASP.NET MVC 4 をインストールするには、ASP.NET MVC 3 RTM アプリケーションが中断されます。** RTM で作成された ASP.NET MVC 3 アプリケーションのリリース (ではなく、 [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491)リリース) ASP.NET MVC 4 と並行して動作させるには、次の変更が必要です。 これらの更新プログラムの結果にコンパイル エラーが発生せず、プロジェクトをビルドします。 

    **必要な更新プログラム**

  1. ルートの Web.config ファイルでは、新しい追加*&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*値*1.0.0.0*します。 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 名前をもう一度右クリックし、編集*ProjectName*.csproj します。
  3. 次のアセンブリ参照を見つけます。 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      次のように、それらを置き換えます。

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. 変更を保存、編集していたし、プロジェクトを右クリックして選択して再読み込み、プロジェクト (.csproj) ファイルを閉じます。

- **4.5 から 4.0 をターゲットに ASP.NET MVC 4 プロジェクトを変更するは、EntityFramework アセンブリ参照を更新できません:** EntityFramework アセンブリへの参照が指すはまだ 4.5 をターゲットの後、4.0 を対象に、ASP.NET MVC 4 プロジェクトを変更したかどうか4.5 バージョンです。 この問題のアンインストールを修正し、EntityFramework NuGet パッケージを再インストールします。
- **4.5 からターゲット 4.0 に変更した後、Azure で ASP.NET MVC 4 アプリケーションを実行するときに 403 許可されていません:** 4.5 を対象とした後 4.0 をターゲットに、ASP.NET MVC 4 プロジェクトを変更し、Azure にデプロイし、実行時に 403 Forbidden エラーを参照してください可能性があります。 回避策には、この問題は、web.config に次のコードを追加します。 `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 のクラッシュを入力すると、'\' Razor ファイルで文字列リテラルでします。** 作業に問題を回避場合は、まず文字列リテラルの終わりの引用符を入力します。
- <strong>参照する&quot;アカウント/管理&quot;インターネット テンプレート結果、CHS、TRK、CHT 語のランタイム エラーが発生します。</strong> 分離するためページを変更して、問題を解決する<em>@User.Identity.Name</em>内の唯一のコンテンツとして配置することで、 <em>&lt;強力な&gt;</em>タグ。
- **Google、LinkedIn のプロバイダーでは、Azure Web サイト内ではサポートされません。** Azure Websites にデプロイするときに、代替の認証プロバイダーを使用します。
- **UriPathExtensionMapping と IIS 8 Express または IIS を使用する場合、拡張機能を使用するとき、404 見つかりません」エラーが表示されるは。** 静的ファイル ハンドラーが使用する web Api への要求に干渉*UriPathExtensionMappings*します。 設定*runAllManagedModulesForAllRequests = true* web.config、問題を回避します。
- **Controller.Execute メソッドが呼び出されない。** すべての MVC コント ローラーは今すぐ常に非同期的に実行します。
