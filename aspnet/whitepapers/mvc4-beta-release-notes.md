---
uid: whitepapers/mvc4-beta-release-notes
title: "ASP.NET MVC 4 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: d6797d1dbacff7503f74782d325ff5a9598970c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。
> 
> > [!NOTE]
> > これは、最新のリリースではありません。 ASP.NET MVC 4 RC のリリース ノートは[ここ](mvc4-release-notes.md)です。


- [インストールに関する注意事項](#_Toc303253802)
- [ドキュメント](#_Toc303253803)
- [サポート](#_Toc303253804)
- [ソフトウェアの要件](#_Toc303253805)
- [ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。](#_Toc303253806)
- [ASP.NET MVC 4 Beta の新機能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET の単一ページ アプリケーション](#_Toc317096198)
    - [既定のプロジェクト テンプレートの機能強化](#_Toc303253808)
    - [モバイルのプロジェクト テンプレート](#_Toc303253809)
    - [表示モード](#_Toc303253810)
    - [jQuery Mobile、ビュー スイッチャーとブラウザーのオーバーライド](#_Toc303253811)
    - [Visual Studio でのコード生成のレシピ](#_Toc303253812)
    - [非同期コント ローラーのタスクのサポート](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [既知の問題と重大な変更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

ASP.NET MVC 4 Beta for Visual Studio 2010 をインストールすることができます、 [ASP.NET MVC 4 のホーム ページ](../mvc/mvc4.md)Web Platform Installer を使用します。

ASP.NET MVC 4 Beta をインストールする前に、ASP.NET MVC 4 の以前にインストールされたプレビューをアンインストールする必要があります。

このリリースは、.NET Framework 4.5 Developer Preview と互換性がありません。 ASP.NET MVC 4 Beta をインストールする前に、.NET 4.5 Developer Preview をアンインストールする必要があります。

ASP.NET MVC 4 をインストールしてサイド バイ サイドで実行できる ASP.NET MVC 3 でします。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC のドキュメントは、次の URL で MSDN web サイトで入手できます。

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 ページで使用できる ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>サポート

これは、プレビュー リリースであり、公式にサポートされていません。 このリリースの操作についての質問がある場合、ASP.NET MVC フォーラムに投稿 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio 用の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Service Pack 1 の Visual Studio 2010 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。

ASP.NET MVC 4 は、する柔軟性が ASP.NET MVC 4 に ASP.NET MVC 3 アプリケーションをアップグレードするときに選択する際に、同じコンピューターに ASP.NET MVC 3 のサイド バイ サイドでインストールできます。

アップグレードする最も簡単な方法は、新しい ASP.NET MVC 4 プロジェクトおよびコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツのファイル既存の MVC 3 プロジェクトからを新しいプロジェクトと、古いプロジェクトと一致する新しいプロジェクトで参照アセンブリを更新するのです。 MVC 3 プロジェクトの Web.config ファイルに変更を行った場合は、MVC 4 プロジェクトの Web.config ファイルにもそれらの変更をマージする必要があります。

既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。

1. (はビュー フォルダーにし、プロジェクト内の各領域の Views フォルダーのプロジェクトのルートに 1 つ)、プロジェクト内のすべての Web.config ファイルでは、次のテキストのすべてのインスタンスに置き換えます。

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    次の対応するテキスト。

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. ルートの Web.config ファイルで更新、 *webPages:Version* 「2.0.0.0」に要素を追加、新しい*PreserveLoginUrl* "true"の値を持つキー。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (どの地点をバージョン 3 の DLL) です。 参照を追加*System.Web.Mvc* (v4.0.0.0)。 具体的には、次の変更をアセンブリ参照を更新します。 その詳細な手順を次に示します。

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
4. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 [名前を再度右クリックして編集] を選択*ProjectName*.csproj です。
5. 検索、 *ProjectTypeGuids*要素と置換 {e53f8fea-eae0-44a6-8774-ffd645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。
7. ルートの Web.config ファイルを開き、プロジェクトは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*の下の要素、 *構成*セクション。 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta の新機能

このセクションの内容が導入された機能について説明します、ASP.NET MVC 4 Beta リリースにします。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 には、ASP.NET Web API は、HTTP サービスを作成するための新しいフレームワークは、多様なブラウザーやモバイル デバイスを含む、クライアントに到達できます。 ASP.NET Web API は RESTful サービスを構築するための最適なプラットフォームもです。

ASP.NET Web API には、次の機能のサポートが含まれています。

- **最新の HTTP プログラミング モデル:**直接アクセスし、HTTP 要求と応答を厳密に型指定された新しい HTTP オブジェクト モデルを使用して、Web Api を操作します。 同じプログラミング モデルと HTTP パイプラインは新しい HttpClient 型を使用してクライアントで対称的に使用できます。
- **ルートの完全なサポート**: Web Api では、ルート パラメーターや制約など、Web スタックの一部であった常にルート機能の完全なセットをサポートします。 さらに、アクションへのマッピングがの表記規則の完全なサポート不要になった [HttpPost] などの属性を適用するようにクラスとメソッドをします。
- **コンテンツ ネゴシエーション**: クライアントとサーバーを連携を API から返されるデータの適切な形式を決定します。 XML、JSON、およびフォームの URL でエンコードされた形式の既定のサポートを提供して、独自のフォーマッタを追加することでこのサポートを拡張またはでも既定コンテンツ ネゴシエーションの戦略を置換することができます。
- **モデル バインディングと検証:**モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できるように .NET オブジェクトに変換する簡単な方法を提供します。
- **フィルター:** Web Api が [Authorize] 属性などのよく知られているフィルターを含む、フィルターになりました。 作成し、独自のフィルター操作、承認、および例外処理用にプラグインします。
- **クエリの構成:** IQueryable を単に返すことによって&lt;T&gt;、Web API は OData URL 規則によってクエリをサポートします。
- **HTTP の詳細のテストの容易性の向上:**静的コンテキスト オブジェクトの HTTP の詳細を設定するのではなく、Web API 操作は HttpRequestMessage と HttpResponseMessage のインスタンスで使用できるようことができます。 これらのオブジェクトのジェネリック バージョンは、HTTP の種類に加えて、カスタムの型を操作するためにも存在します。
- **制御の反転 (IoC) DependencyResolver 経由での向上:** Web API は、多くのさまざまな機能のインスタンスを取得する MVC の依存関係競合回避モジュールによって実装されたサービス ロケーターのパターンを使用するようになりました。
- **コード ベースの構成:**コードを通じてのみ Web API の構成には、ファイルのクリーンアップの設定のままです。
- **自己ホスト:** Web Api は、ルートの全機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。

ASP.NET Web API の詳細についてはご参照ください[https://www.asp.net/web-api](../web-api/index.md)です。

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET の単一ページ アプリケーション

ASP.NET MVC 4 には、さまざまな相互重要なクライアント側 JavaScript および Web Api を使用して単一ページ アプリケーションを構築するためのエクスペリエンスの初期のプレビューが含まれています。 このサポートは次のとおりです。

- キャッシュされたデータのローカルの相互作用を高度な JavaScript ライブラリのセット
- 単位の作業と DAL のサポートの追加の Web API コンポーネント
- すぐに開始するスキャフォールディングでの MVC プロジェクト テンプレート

単一ページ アプリケーションの詳細については、ASP.NET MVC 4 のサポートを参照してください[https://www.asp.net/single-page-application](../single-page-application/index.md)です。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>既定のプロジェクト テンプレートの機能強化

現代の外見の複数の web サイトを作成する新しい ASP.NET MVC 4 プロジェクトの作成に使用されるテンプレートが更新されました。

![](mvc4-beta-release-notes/_static/image1.png)

表面的な機能強化に加え、新しいテンプレートの機能をきました。 テンプレートには、デスクトップ ブラウザーとカスタマイズすることがなくモバイル ブラウザーの両方できれいに表示するアダプティブと呼ばれる技術が採用しています。

![](mvc4-beta-release-notes/_static/image2.png)

アダプティブのレンダリングの動作を表示するには、モバイル エミュレーターを使用するか小さくなるようにデスクトップ ブラウザーのウィンドウのサイズを変更してみてください。 ブラウザーのウィンドウは、大きすぎて取得、ページのレイアウトが変わります。

別の拡張機能で、既定のプロジェクト テンプレートは、豊富な UI を提供する JavaScript の使用です。 テンプレートで使用されるログインおよび登録のリンクは、jQuery UI ダイアログを使用して、豊富なログイン画面を表示する方法の例を示します。

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>モバイルのプロジェクト テンプレート

新しいプロジェクトを開始しているモバイル向けサイトおよびタブレット ブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。 これは、jQuery Mobile、タッチ最適化の UI を構築するため、オープン ソース ライブラリに基づいています。

![](mvc4-beta-release-notes/_static/image4.png)

このテンプレートには、インターネット アプリケーションのテンプレートと同じアプリケーション構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示し、モバイル デバイスのタッチ ベースで適切に動作するスタイルを設定します。 構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)です。

デスクトップ向けのサイトをモバイル用に最適化されたビューを追加する場合や、デスクトップとモバイル ブラウザー スタイルの異なるビューが機能する 1 つのサイトを作成する場合がある場合は、新しい表示モード機能を使用することができます。 (詳しくは、次のセクションを参照してください)。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>表示モード

新しい表示モード機能には、要求を行って、ブラウザーによってビューを選択して、アプリケーションができます。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.cshtml テンプレートを使用できます。 モバイル ブラウザーは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す場合があります。

レイアウトとパーシャルは、特定のブラウザーの種類を無効にできます。 例:

- \Shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイル ブラウザーとからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求時にします。
- フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_携帯電話からの要求時に MyPartial.mobile.cshtmlブラウザー、および\_MyPartial.cshtml 他の要求時にします。

新しいを登録することがより特定のビュー、レイアウト、またはその他のデバイスの部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たす場合に検索する名前を指定するインスタンス。 次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザーは要求時に適用される表示モードとして、文字列"iPhone"を登録する Global.asax ファイル内のメソッド。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

アプリケーションで、\shared を使用して、Apple iPhone ブラウザーが要求するときに、このコードを実行すた後、\\_Layout.iPhone.cshtml レイアウト (ある場合)。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile、ビュー スイッチャーとブラウザーのオーバーライド

jQuery Mobile は、タッチ最適化 web UI を構築するため、オープン ソース ライブラリです。 ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用する場合は、ダウンロードして、作業を開始するのに役立つ NuGet パッケージをインストールします。 Visual Studio パッケージ マネージャー コンソールからインストールするには、次のコマンドを入力します。

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

これには、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。

- Views/shared/\_Layout.Mobile.cshtml、jQuery Mobile ベースのレイアウトであります。
- ビュー/共有から構成されるビュー スイッチャーコンポーネント/\_ViewSwitcher.cshtml 部分ビューと ViewSwitcherController.cs コント ローラー。

パッケージをインストールした後は、モバイル ブラウザーを使用してアプリケーションを実行 (または、Firefox など、同等の[ユーザー エージェント スイッチャー](http://chrispederick.com/work/user-agent-switcher/)アドオン)。 エンドユーザー表示 jQuery Mobile はレイアウトを処理するため、ページがまったく異なることを確認し、スタイルを設定します。 これを利用するには、次の操作を行うことができます。

- 下の説明に従って、mobile に固有のビューの上書きを作成[表示モード](#_Toc303253810)前 (たとえば、モバイル ブラウザーの Views\Home\Index.cshtml を上書きする Views\Home\Index.mobile.cshtml を作成します)。
- 読み取り、 [jQuery Mobile ドキュメント](http://jquerymobile.com/)モバイル ビューでタッチ最適化の UI 要素を追加する方法の詳細についてはします。

モバイルに最適化された web ページの規則では、テキストはデスクトップ ビューまたはユーザーがページのデスクトップ バージョンに切り替えるできるサイトの完全モードと同様に、何かのリンクを追加します。 JQuery.Mobile.MVC パッケージには、この目的のためのビュー スイッチャー コンポーネント サンプルにはが含まれています。 既定 \shared で使用されている\\_Layout.Mobile.cshtml 表示し、ページが表示されるときに次のように。

![](mvc4-beta-release-notes/_static/image5.png)

閲覧者は、リンクをクリックして、同じページのデスクトップ バージョンに切り替えられますしています。

デスクトップのレイアウトには既定のビュー スイッチャーが含まれていないために、訪問者はモバイル モードを取得する方法を必要はありません。 これが有効にする次の参照を追加 *\_ViewSwitcher*内だけ使用してデスクトップのレイアウトを*&lt;本文&gt;*要素。

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

ビュー スイッチャーは、ブラウザーのオーバーライドと呼ばれる新しい機能を使用します。 この機能により、アプリケーションは、飛び出してきたかのように要求を扱うものの別のブラウザー (ユーザー エージェント) から、あなたが実際にします。 次の表は、ブラウザーのオーバーライドを提供する方法を示します。

| `HttpContext.SetOverriddenBrowser(userAgentString)` | 指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。 |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | オーバーライドが指定されていない場合は、要求のユーザー エージェントの上書き値、または実際のユーザー エージェント文字列を返します。 |
| `HttpContext.GetOverriddenBrowser()` | 返します、 *HttpBrowserCapabilitiesBase*要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライドされる)。 この値を使用するにはプロパティを取得するよう*IsMobileDevice*です。 |
| `HttpContext.ClearOverriddenBrowser()` | 現在の要求に対するすべてのオーバーライドされたユーザー エージェントを削除します。 |

ブラウザー オーバーライドは、ASP.NET MVC 4 の中心的な機能し、jQuery.Mobile.MVC パッケージをインストールしない場合でもは有効です。 ただし、影響ビュー、レイアウト、および部分ビューの選択-その他の ASP.NET 機能に依存するのには影響しません、 *Request.Browser*オブジェクト。

既定では、ユーザー エージェントの上書きは、cookie を使用して格納されます。 既定のプロバイダーを置き換えることができます (たとえば、データベース内の) で他の場所で、上書きを保存する場合は、(*BrowserOverrideStores.Current*)。 このプロバイダーのマニュアルをできるようにする ASP.NET MVC の以降のリリースに付属します。

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio でのコード生成のレシピ

新しいレシピ機能では、NuGet を使用してをインストールするパッケージに基づくソリューション固有のコードを生成する Visual Studio を使用します。 レシピ framework 簡単開発者用を置換する組み込みのコード ジェネレーターを追加の領域、コント ローラーの追加、およびビューの追加使用することもできます。 コード生成プラグインを作成します。 レシピは、NuGet パッケージとして展開される、ため、簡単にソース管理にチェックインして自動的にプロジェクトのすべての開発者と共有します。 ソリューションごとに記載されています。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同期コント ローラーのタスクのサポート

記述できるように非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;*です。

たとえば、Visual C# 5 を使用している場合 (またはを使用して、 [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx))、次のような非同期アクション メソッドを作成することができます。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

前のアクション メソッドへの呼び出しで*newsService.GetHeadlinesAsync*と*sportsService.GetScoresAsync*非同期に呼び出され、スレッド プールのスレッドはブロックされません。

返す非同期アクション メソッド*タスク*インスタンスは、タイムアウトにもサポートできます。 アクション メソッドをキャンセル可能にするために、型のパラメーターを追加*CancellationToken*アクション メソッドのシグネチャにします。 次の例では、2500 ミリ秒のタイムアウトを持つし、表示する非同期アクション メソッド、 *TimedOut*タイムアウトが発生した場合、クライアントに表示します。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta は、Windows Azure SDK の 2011 年 9 月 1.5 のリリースをサポートします。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

- **ASP.NET MVC 4 Beta をインストールすると、Visual Studio 2010 Service Pack 1 CSHTML/VBHTML エディターで、CSHTML/VBHTML エディターは可能性があります cshtml"や"vbhtml ファイル内のスニペットまたは JavaScript の入力後に長時間一時停止します。** これは、作成された直後、まだコンパイルされていないと ASP.NET MVC 4 アプリケーションでのみ発生します。

    回避策では、bin フォルダーにアセンブリを取得するプロジェクトをコンパイルします。 エディターの問題が返される、bin フォルダーからアセンブリが削除され、プロジェクトをクリーンアップする場合に注意してください。

    これは、次のリリースで修正される予定です。
- **Visual Studio 11 Beta 用の c# プロジェクト テンプレートには、Global.asax.cs 内の不適切な接続文字列が含まれます。** アプリケーションで指定された既定の接続\_Visual Studio 11 Beta で作成したプロジェクトには、エスケープ解除されたバック スラッシュを含んでいる LocalDB 接続文字列が含まれているメソッドを開始 (\)文字です。 これは、結果、接続エラー、SqlException が生成されるエンティティ フレームワーク DbContext にアクセスしようとします。

    この問題を修正するアプリのバック スラッシュ文字をエスケープ\_Global.asax.cs の方法を開始するように読むことができるようにします。

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **.NET 4.5 を対象とする ASP.NET MVC 4 アプリケーションでは、.NET 4.0 で実行する場合、System.Net.Http.dll アセンブリにアクセスしようとしたときに FileLoadException をスローします。** .NET 4.5 で作成した ASP.NET MVC 4 アプリケーションは、バインディングを格納するようになる FileLoadException の原因となるリダイレクト「を読み込めませんでしたファイルまたはアセンブリ 'System.Net.Http' またはその依存関係の 1 つです」。 ときに、アプリケーションは、.NET 4.0 がインストールされたシステムで実行されます。 この問題を解決するには、web.config ファイルから次のバインド リダイレクトを削除します。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    変更した web.config でアセンブリのバインディング要素として次のように表示されます。

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Visual Basic プロジェクトで「コント ローラーの追加」の項目テンプレートを生成、正しくない名前空間呼び出されたときに * * * から領域内です。** Visual Basic を使用する ASP.NET MVC プロジェクト内の領域に、コント ローラーを追加すると、項目テンプレートは、コント ローラーに間違った名前空間を挿入します。 コント ローラーのどのアクションに移動するときに、「ファイルが見つかりません」エラーになります。  
  
 生成された名前空間は、ルート名前空間の後にすべてのものを省略します。 たとえば、生成された名前空間は*RootNamespace*する必要がありますが、 *RootNamespace.Areas.AreaName.Controllers*です。
- **Razor ビュー エンジンにおける重大な変更です。** Razor パーサーの書き換えの一環として、次の種類はから削除された*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 次の方法も削除されます。 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll が含まれる場合、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに、フォーム認証の URL になります。** (たとえば、展開可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択する) を WebMatrix.WebData.dll アセンブリをアプリケーションに追加すると、認証ログインへのリダイレクト/アカウント/ログオンよりも優先されますのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。 この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用する PreserveLoginUrl と呼ばれる、appSetting を追加およびを true に設定できます。 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet パッケージ マネージャーは、Visual Studio 2010 と Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールしようとするときのインストールに失敗します。** Visual Studio 2010 と ASP.NET MVC 4 サイド バイ サイドで Visual Web Developer 2010 を実行するには、Visual Studio の両方のバージョンを既にインストールされている後に ASP.NET MVC 4 をインストールする必要があります。
- **前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。** ASP.NET MVC を完全にアンインストールするには、4you は、Visual Studio をアンインストールする前に ASP.NET MVC 4 をアンインストールする必要があります。
- **既定の Web API プロジェクトを実行しているが存在しない、RegisterApis メソッドを使用してルートを追加するユーザーを正しく指示する命令を示しています。** ASP.NET ルート テーブルを使用して、RegisterRoutes メソッドでは、ルートを追加する必要があります。
- **ASP.NET MVC 4 Beta をインストールすると、アプリケーションの ASP.NET MVC 3 RTM が中断されます。** ASP.NET MVC 3 アプリケーションを作成したリリース (ASP.NET MVC 3 Tools Update リリース) ではなく、RTM のサイド バイ サイド ASP.NET MVC 4 Beta で動作するために、次の変更を必要とします。 コンパイル エラーにこれらの更新プログラムの結果を加えずにプロジェクトをビルドします。 

    **必要な更新プログラム**

    1. ルートの Web.config ファイルで追加、新しい *&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*および値*1.0.0.0*です。

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 [名前を再度右クリックして編集] を選択*ProjectName*.csproj です。
    3. 次のアセンブリ参照を見つけます。 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        次のように、それらを置き換えます。

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. 変更を保存、閉じるプロジェクト (.csproj) ファイルを編集、およびし、プロジェクトを右クリックし、再読み込みを選択します。
