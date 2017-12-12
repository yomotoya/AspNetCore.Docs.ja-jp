---
uid: whitepapers/mvc4-release-notes
title: "ASP.NET MVC 4 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、ASP.NET MVC 4 のリリースについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: fb9d2eaa83fe7486279815c21aec204bdfdf122d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> このドキュメントでは、ASP.NET MVC 4 のリリースについて説明します。


- [インストールに関する注意事項](#_Toc303253802)
- [ドキュメント](#_Toc303253803)
- [サポート](#_Toc303253804)
- [ソフトウェアの要件](#_Toc303253805)
- [ASP.NET MVC 4 の新機能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [既定のプロジェクト テンプレートの機能強化](#_Toc303253808)
    - [モバイルのプロジェクト テンプレート](#_Toc303253809)
    - [表示モード](#_Toc303253810)
    - [jQuery Mobile、ビュー スイッチャーとブラウザーのオーバーライド](#_Toc303253811)
    - [非同期コント ローラーのタスクのサポート](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [データベースの移行](#_Toc303253818)
    - [空のプロジェクト テンプレート](#_Toc303253819)
    - [コント ローラーを任意のプロジェクト フォルダーに追加します。](#_Toc303253820)
    - [バンドルと縮小](#_Toc303253821)
    - [Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。](#_Toc303253822)
- [ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。](#_Toc303253806)
- [ASP.NET MVC 4 リリース候補からの変更](#_Toc303253817)
- [既知の問題と重大な変更](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

Visual Studio 2010 の ASP.NET MVC 4 をインストールすることができます、 [ASP.NET MVC 4 のホーム ページ](../mvc/mvc4.md)Web Platform Installer を使用します。

ASP.NET MVC 4 をインストールする前に、ASP.NET MVC 4 の以前にインストールされたプレビューをアンインストールすることをお勧めします。 アンインストールせずに、ASP.NET MVC 4 に、ASP.NET MVC 4 Beta とリリース候補をアップグレードできます。

このリリースは、.NET Framework 4.5 の任意のプレビュー リリースと互換性がありません。 ASP.NET MVC 4 をインストールする前に最終バージョンを .NET Framework 4.5 のインストール済みのプレビュー リリースをアップグレードすることとは別にする必要があります。

ASP.NET MVC 4 では、インストールおよび実行してサイド バイ サイド ASP.NET MVC 3 を指定できます。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>ドキュメント

ASP.NET MVC のドキュメントは、次の URL で MSDN web サイトで入手できます。

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 ページで使用できる ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>Support

ASP.NET MVC 4 が完全にサポートします。 このリリースの操作についての質問がある場合もに掲示できます、ASP.NET MVC フォーラムに ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>ソフトウェア要件

Visual Studio 用の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Service Pack 1 の Visual Studio 2010 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4 の新機能

このセクションの内容が導入された機能について説明します、ASP.NET MVC 4 のリリースでします。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 には、ASP.NET Web API、クライアント ブラウザーやモバイル デバイスなどの広範な範囲に到達可能な HTTP サービスを作成するための新しいフレームワークが含まれています。 ASP.NET Web API は RESTful サービスを構築するための最適なプラットフォームもです。

ASP.NET Web API には、次の機能のサポートが含まれています。

- **最新の HTTP プログラミング モデル:**直接アクセスし、HTTP 要求と応答を厳密に型指定された新しい HTTP オブジェクト モデルを使用して、Web Api を操作します。 同じ対称的には、新しいクライアントで使用可能なプログラミング モデルと HTTP パイプラインは*HttpClient*型です。
- **ルートの完全なサポート:** ASP.NET Web API には、ルート パラメーターや制約など、ASP.NET のルーティングのルートの機能の完全なセットがサポートしています。 さらに、HTTP メソッドにアクションをマップするのに単純な規則を使用します。
- **コンテンツ ネゴシエーション:**クライアントとサーバーを連携を web API から返されるデータの適切な形式を決定します。 ASP.NET Web API は、XML、JSON の既定のサポートを提供およびフォームの URL でエンコードされた形式とすることができます独自のフォーマッタを追加することでこのサポートを拡張またはであっても、既定のコンテンツ ネゴシエーション戦略を置き換えます。
- **モデル バインディングと検証:**モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できるように .NET オブジェクトに変換する簡単な方法を提供します。 データの注釈に基づいたアクション パラメーターの検証は実行もします。
- **フィルター:**などよく知られているフィルターを含むフィルターをサポートする ASP.NET Web API、 *[Authorize]*属性。 作成し、独自のフィルター操作、承認、および例外処理用にプラグインします。
- **クエリの構成:**を使用して、 *[Queryable]*を返すアクション フィルター属性*IQueryable* OData クエリ規則を使用して、web API を照会するためのサポートを有効にします。
- **テストの容易性の向上:**静的コンテキスト オブジェクトの HTTP の詳細を設定するのではなく web API 操作の作業のインスタンスと*HttpRequestMessage*と*HttpResponseMessage*です。 すぐに、Web API の機能の単体テストの作成を開始するように Web API プロジェクトと単体テスト プロジェクトを作成します。
- **コード ベースの構成:**コードを通じてのみ ASP.NET Web API の構成には、ファイルのクリーンアップの設定のままです。 機能拡張ポイントを構成するのにには、指定されたサービス ロケーター パターンを使用します。
- **制御の反転 (IoC) コンテナーのサポートが向上します**ASP.NET Web API は、強化された依存関係競合回避モジュールの抽象化を通じて IoC コンテナーの優れたサポートを提供。
- **自己ホスト:** Web Api は、ルートの全機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。
- **カスタム ヘルプを作成し、ページのテスト:**するようになりましたことができ、簡単にビルドのカスタム ヘルプ ページ、web Api を使用してテスト新しい*IApiExplorer*サービス、web Api の完全なランタイムの説明を取得します。
- **監視と診断:** ASP.NET Web API は今すぐ System.Diagnostics、ETW サード パーティ製のログ記録フレームワークなどの既存のログ記録ソリューションと統合しやすく軽量トレース インフラストラクチャを提供します。 提供することによりトレースを有効にすることができます、 *ITraceWriter*実装と、web API の構成に追加します。
- **リンクの生成:** ASP.NET Web API を使用して*UrlHelper*を同じアプリケーション内の関連リソースへのリンクを生成します。
- **Web API プロジェクト テンプレート:**新しい Web API プロジェクトのフォームをすばやく立ち上がっており実行中で ASP.NET Web API の新しい MVC 4 プロジェクト ウィザードを選択します。
- **スキャフォールディング:**を使用して、**コント ローラーの追加**を迅速に Entity Framework に基づく web API コント ローラーをスキャフォールディングするダイアログ ベースのモデルの種類。

ASP.NET Web API の詳細についてはご参照ください[https://www.asp.net/web-api](../web-api/index.md)です。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>既定のプロジェクト テンプレートの機能強化

現代の外見の複数の web サイトを作成する新しい ASP.NET MVC 4 プロジェクトの作成に使用されるテンプレートが更新されました。

![](mvc4-release-notes/_static/image1.png)

表面的な機能強化に加え、新しいテンプレートの機能をきました。 テンプレートには、デスクトップ ブラウザーとカスタマイズすることがなくモバイル ブラウザーの両方できれいに表示するアダプティブと呼ばれる技術が採用しています。

![](mvc4-release-notes/_static/image2.png)

アダプティブのレンダリングの動作を表示するには、モバイル エミュレーターを使用するか小さくなるようにデスクトップ ブラウザーのウィンドウのサイズを変更してみてください。 ブラウザーのウィンドウは、大きすぎて取得、ページのレイアウトが変わります。

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>モバイルのプロジェクト テンプレート

新しいプロジェクトを開始しているモバイル向けサイトおよびタブレット ブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。 これは、jQuery Mobile、タッチ最適化の UI を構築するため、オープン ソース ライブラリに基づいています。

![](mvc4-release-notes/_static/image3.png)

このテンプレートには、インターネット アプリケーションのテンプレートと同じアプリケーション構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示し、モバイル デバイスのタッチ ベースで適切に動作するスタイルを設定します。 構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)です。

デスクトップ向けのサイトをモバイル用に最適化されたビューを追加する場合や、デスクトップとモバイル ブラウザー スタイルの異なるビューが機能する 1 つのサイトを作成する場合がある場合は、新しい表示モード機能を使用することができます。 (詳しくは、次のセクションを参照してください)。

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>表示モード

新しい表示モード機能には、要求を行って、ブラウザーによってビューを選択して、アプリケーションができます。 たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.cshtml テンプレートを使用できます。 モバイル ブラウザーは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す場合があります。

レイアウトとパーシャルは、特定のブラウザーの種類を無効にできます。 例:

- \Shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイル ブラウザーとからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求時にします。
- フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_携帯電話からの要求時に MyPartial.mobile.cshtmlブラウザー、および\_MyPartial.cshtml 他の要求時にします。

新しいを登録することがより特定のビュー、レイアウト、またはその他のデバイスの部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たす場合に検索する名前を指定するインスタンス。 次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザーは要求時に適用される表示モードとして、文字列"iPhone"を登録する Global.asax ファイル内のメソッド。

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

アプリケーションで、\shared を使用して、Apple iPhone ブラウザーが要求するときに、このコードを実行すた後、\\_Layout.iPhone.cshtml レイアウト (ある場合)。 表示モードの詳細については、次を参照してください。 [ASP.NET MVC 4 のモバイル機能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)します。 DisplayModeProvider を使用してアプリケーションをインストールする必要があります、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet パッケージです。 [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322)が含まれています、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)新しいプロジェクト テンプレートでの NuGet パッケージです。 参照してください[ASP.NET MVC 4 Mobile キャッシュ バグ Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile、およびモバイル機能

ASP.NET MVC 4 によるモバイル アプリケーションの構築について jQuery Mobile を使用してチュートリアルを参照、 [ASP.NET MVC 4 のモバイル機能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)します。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>非同期コント ローラーのタスクのサポート

記述できるように非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;*です。

 詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)です。

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 には、Windows Azure sdk 1.6 以降のリリースがサポートされています。

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>データベースの移行

これで、ASP.NET MVC 4 プロジェクトには、Entity Framework 5 が含まれます。 Entity Framework 5 の優れた機能の 1 つは、データベースを移行するためのサポートです。 この機能では、データベース内のデータを保持しながらコード中心の移行を使用して、データベース スキーマを簡単に発展することができます。 データベースの移行の詳細については、次を参照してください。 [、ムービーのモデルとテーブルに新しいフィールドを追加する](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md)で、 [Introduction to ASP.NET MVC 4 のチュートリアル](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)です。

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>空のプロジェクト テンプレート

完全に白紙から開始できるように、空の MVC プロジェクト テンプレートが本当に空になったです。 空のプロジェクト テンプレートの以前のバージョンは、基本的に変更されています。

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>コント ローラーを任意のプロジェクト フォルダーに追加します。

これで右クリックし、選択できる**コント ローラーの追加**MVC プロジェクト内の任意のフォルダーからです。 より柔軟に、必要に応じて、別々 のフォルダーに、MVC、Web API コント ローラーを保持するなど、コント ローラーを整理できます。

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>バンドルと縮小

バンドルと縮小のフレームワークでは、スクリプトや CSS の 1 つのバンドル ファイルに個別のファイルを結合して作成する必要がある Web ページが HTTP 要求の数を削減することができます。 バンドルのコンテンツを縮小するによってそれらの要求の全体的なサイズをし、少なくなることができます。 縮小すると、短縮することも、セマンティクスに基づく CSS セレクターを縮小する変数名に空白を排除同様のアクティビティを含めることができます。 バンドルの宣言し、で構成されているコードであり、簡単に参照されているいずれかを生成するヘルパー メソッドを使用してビューでバンドルへのリンクが 1 つまたは複数のリンクが、バンドルの個々 の内容をデバッグする場合。 詳細については、次を参照してください。 [Bundling and Minification](../mvc/overview/performance/bundling-and-minification.md)です。

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。

ASP.NET MVC 4 インターネット プロジェクト テンプレートの既定のテンプレートには、OAuth および OpenID を DotNetOpenAuth ライブラリを使用してログインのサポートが含まれています。 OAuth または OpenID プロバイダーの構成方法の詳細については、次を参照してください。 [Oauth/openid WebForms、MVC およびサポートの web ページ](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx)と[機能ドキュメント ASP.NET Web Pages での OAuth および OpenID](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup)です。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。

ASP.NET MVC 4 は、する柔軟性が ASP.NET MVC 4 に ASP.NET MVC 3 アプリケーションをアップグレードするときに選択する際に、同じコンピューターに ASP.NET MVC 3 のサイド バイ サイドでインストールできます。

アップグレードする最も簡単な方法で、新しい ASP.NET MVC 4 プロジェクトおよびコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツのファイル既存の MVC 3 プロジェクトからを新しいプロジェクトの任意の非 MVC テンプレートに一致する新しいプロジェクトで参照アセンブリを更新するのには、します。使用している cluded アセンブリ。 MVC 3 プロジェクトの Web.config ファイルに変更を行った場合は、MVC 4 プロジェクトの Web.config ファイルにもそれらの変更をマージする必要があります。

既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。

1. すべての Web.config では、(はビュー フォルダーにし、プロジェクト内の各領域の Views フォルダーのプロジェクトのルートに 1 つ)、プロジェクト内のファイルは、次のテキストのすべてのインスタンスを置き換えます (注: System.Web.WebPages, Version = に 1.0.0.0 が見つかりません。作成したプロジェクト Visual Studio 2012)。 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    次の対応するテキスト。

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. ルートの Web.config ファイルで更新、 *webPages:Version* 「2.0.0.0」に要素を追加、新しい*PreserveLoginUrl* "true"の値を持つキー。 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. ソリューション エクスプ ローラーで、参照を右クリックし、NuGet パッケージの管理を選択します。 左のペインで選択**Online\NuGet 公式パッケージ ソース**、次の更新します。

    - ASP.NET MVC 4
    - (省略可能) jQuery、jQuery 検証と jQuery UI
    - (省略可能)Entity Framework
    - (Optonal)Modernizr
4. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 [名前を再度右クリックして編集] を選択*ProjectName*.csproj です。
5. 検索、 *ProjectTypeGuids*要素と置換 {e53f8fea-eae0-44a6-8774-ffd645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。
7. ルートの Web.config ファイルを開き、プロジェクトは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*の下の要素、 *構成*セクション。 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>ASP.NET MVC 4 リリース候補からの変更

ASP.NET MVC 4 Release candidate リリース ノートを参照してください。

このリリースでは、ASP.NET MVC 4 リリース候補から大きな変更は、以下にまとめます。

- **コント ローラーの構成ごと:**を実装するカスタム属性を持つ原因と考えられる ASP.NET Web API コント ローラー *IControllerConfiguration*独自フォーマッタ、操作セレクターおよびパラメーターのバインダーをセットアップするには. *HttpControllerConfigurationAttribute*は削除されました。
- **ルートのメッセージ ハンドラーごと:**特定のルートの要求のチェーンの最後のメッセージ ハンドラーを指定できます。 これによりのルーティングを使用して、独自にディスパッチするには、飛行に沿ってフレームワークのサポート (非*IHttpController*) エンドポイント。
- **進行状況の通知:** 、 *ProgressMessageHandler*アップロードされる要求エンティティとダウンロードされる応答エンティティの両方の進行状況の通知が生成されます。 このハンドラーを使用して行うことが要求本文をアップロードまたは応答本文をダウンロードするどの程度の追跡します。
- **コンテンツをプッシュ:** 、 *PushStreamContent*クラスは、データ プロデューサーが、要求または応答のストリームを使用して (同期または非同期) に直接書き込むたいシナリオを実現できます。 ときに、 *PushStreamContent*出力ストリームとアクション デリゲート呼び出してデータを受け入れる準備ができています。 開発者は、限り必要かつ閉じるストリームの書き込み時に完了したため、ストリームに書き込むことができます。 *PushStreamContent*ストリームの終わりを検出し、基になる非同期の完了*タスク*内容を記述するためです。
- **エラー応答を作成する:**を使用して、 *HttpError*を一貫してからのエラー情報など、検証エラーと例外もに従いながらを表す型、 *IncludeErrorDetailPolicy*. 使用して、新しい*CreateErrorResponse*エラー応答を簡単に作成する拡張メソッド*HttpError*コンテンツとして。 *HttpError*コンテンツは完全にコンテンツをネゴシエートします。
- **削除 MediaRangeMapping:**メディア型の範囲が既定のコンテンツ ネゴシエーターによって処理されるようになりました。
- **単純型のパラメーターの既定のパラメーター バインドが、[FromUri]:**以前のリリースの ASP.NET Web API の既定のパラメーターのバインディングは、単純型のパラメーターは、モデル バインディングを使用します。 単純型のパラメーターの既定のパラメーター バインディングは、今すぐ*[FromUri]*です。
- **操作の選択は、必要なパラメーター:** ASP.NET Web API の操作の選択のみを選択しますアクション URI に由来するすべての必要なパラメーターを指定する場合。 パラメーターは、アクション メソッドのシグネチャ内の引数に既定値を提供することで、省略可能として指定できます。
- **HTTP パラメーター バインディングをカスタマイズ:**を使用して、 *ParameterBindingAttribute*を特定のアクション パラメーターのパラメーター バインディングをカスタマイズまたはを使用して、 *ParameterBindingRules*上*HttpConfiguration*パラメーター バインディングをカスタマイズより広範にします。
- **MediaTypeFormatter の機能強化:**フォーマッタが今すぐに完全にアクセス権を持つ*HttpContent*インスタンス。
- **ホスト バッファー ポリシーの選択:**を実装し、構成、 *IHostBufferPolicySelector*サービスで ASP.NET Web API を使用するときにバッファリングのポリシーを確認するホストを有効にします。
- **ホストに依存しない方法でクライアント証明書にアクセスします。**を使用して、 *GetClientCertificate*要求メッセージから、指定されたクライアント証明書を取得する拡張メソッド。
- **コンテンツ ネゴシエーションの機能拡張:**から派生することで、コンテンツ ネゴシエーションをカスタマイズ、 *DefaultContentNegotiator*したいコンテンツ ネゴシエーションのすべての側面をオーバーライドします。
- **406 Not Acceptable 応答を返すためのサポート:**今すぐ戻れます 406 Not Acceptable 応答 ASP.NET Web API で作成することで、適切なフォーマッタが見つからない場合に、 *DefaultContentNegotiator* で*excludeMatchOnTypeOnly*パラメーターに設定*true*です。
- **NameValueCollection または JToken としてフォーム データの読み取り:**フォーム データの取得、URI クエリ文字列またはとしての要求本文を読み取ることができます、 *NameValueCollection*を使用して、 *ParseQueryString*と*ReadAsFormDataAsync*拡張メソッドそれぞれします。 同様に、フォーム データの取得、URI クエリ文字列またはとしての要求本文を読み取ることができます、 *JToken*を使用して、 *TryReadQueryAsJson*と*ReadAsAsync*&lt;T&gt;拡張メソッドそれぞれします。
- **マルチパートの機能強化:**を記述することで、 *MultipartStreamProvider*マルチパート データを読み取り/ユーザーに最適な方法で結果を表示できることは、MIME の種類に合わせて調整する、完全にします。 後処理のステップをフックすることも、 *MultipartStreamProvider*実装処理は、MIME マルチパート ボディ パーツの任意の投稿を行うことができます。 たとえば、 *MultipartFormDataStreamProvider*実装が HTML フォームのデータ部分を読み取り、追加して、 *NameValueCollection*簡単にでは、呼び出し元から取得されるようにします。
- **リンクの生成の機能強化:** 、 *UrlHelper*に依存しなく*HttpControllerContext*です。 今すぐアクセスすることができます、 *UrlHelper*任意のコンテキストからここで、 *HttpRequestMessage*は使用できます。
- **メッセージのハンドラーの実行順序の変更:**逆の順序での代わりに構成されている順序でメッセージのハンドラーが実行されるようになりました。
- **メッセージのハンドラーを組み立てるヘルパー:**新しい*HttpClientFactory*を接続する*DelegatingHandlers*を作成し、 *HttpClient*で、目的のパイプラインを開始します。 代替の内部ハンドラーを持つを組み立てる機能も用意されています (既定値は*HttpClientHandler*) を使用する場合配線を行うと*HttpMessageInvoker*別または*DelegatingHandler*の代わりに*HttpClient*上部 invoker として。
- **ASP.NET Web Optimization で Cdn のサポート:** ASP.NET Web 最適化は、CDN の代替パスごとに指定できるようにバンドルのコンテンツ配信ネットワークでその同じリソースをポイントする追加の URL に対する今すぐサポートを提供します。 Cdn をサポートするには、Web アプリケーションの終了のコンシューマーに、スクリプトおよびスタイル バンドル地理的に近い場所を取得することができます。
- **ASP.NET Web API ルーティングし、構成を移動する*WebApiConfig.Register*静的メソッドをテスト コードで再利用することができます。** ASP.NET Web API のルートで追加された以前*RouteConfig.RegisterRoutes*と共に標準的な MVC ルーティングします。 既定の ASP.NET Web API ルーティングと構成が個別で処理されるようになりました*WebApiConfig.Register*メソッドがテストを容易になります。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

- **ASP.NET MVC 4 の RC、RTM バージョン正しくが返されますキャッシュ デスクトップ ビュー モバイル ビューを返す必要があるときにします。**

    - 参照してください[ASP.NET MVC 4 Mobile キャッシュ バグ固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。 修正プログラムをインストールすることができます、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet パッケージです。
- **重大な変更は、Razor ビュー エンジン**です。 次の種類はから削除された*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 次の方法も削除されます。 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll に含めるときは、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに、フォーム認証の URL になります。** (たとえば、展開可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択する) を WebMatrix.WebData.dll アセンブリをアプリケーションに追加すると、認証ログインへのリダイレクト/アカウント/ログオンよりも優先されますのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。 この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用する PreserveLoginUrl と呼ばれる、appSetting を追加およびを true に設定できます。 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet パッケージ マネージャーは、Visual Studio 2010 と Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールしようとするときのインストールに失敗します。** Visual Studio 2010 と ASP.NET MVC 4 サイド バイ サイドで Visual Web Developer 2010 を実行するには、Visual Studio の両方のバージョンを既にインストールされている後に ASP.NET MVC 4 をインストールする必要があります。
- **前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。** ASP.NET MVC を完全にアンインストールするには、4you は、Visual Studio をアンインストールする前に ASP.NET MVC 4 をアンインストールする必要があります。
- **ASP.NET MVC 4 をインストールすると、アプリケーションの ASP.NET MVC 3 RTM が中断されます。** RTM を使用して作成された ASP.NET MVC 3 アプリケーションのリリース (ではなく、 [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/en-us/download/details.aspx?id=1491)リリース) サイド バイ サイドで ASP.NET MVC 4 を使用するために、次の変更を必要とします。 コンパイル エラーにこれらの更新プログラムの結果を加えずにプロジェクトをビルドします。 

    **必要な更新プログラム**

    1. ルートの Web.config ファイルで追加、新しい *&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*および値*1.0.0.0*です。 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。 [名前を再度右クリックして編集] を選択*ProjectName*.csproj です。
    3. 次のアセンブリ参照を見つけます。 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        次のように、それらを置き換えます。

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. 変更を保存、閉じるプロジェクト (.csproj) ファイルを編集、およびし、プロジェクトを右クリックし、再読み込みを選択します。
- **4.5 からのターゲット 4.0 に ASP.NET MVC 4 プロジェクトを変更する EntityFramework アセンブリ参照を更新しません**EntityFramework アセンブリへの参照がまだ指す 4.5 をターゲットの後に、ターゲット 4.0 に ASP.NET MVC 4 プロジェクトを変更する場合。4.5 バージョンです。 この問題のアンインストールを修正して EntityFramework NuGet パッケージを再インストールします。
- **4.5 からターゲット 4.0 に変更すると、ASP.NET MVC 4 アプリケーションを Azure で実行されているときに 403 アクセス不可:** 4.5 を対象とした後、ターゲット 4.0 に ASP.NET MVC 4 プロジェクトを変更を Azure に展開すると、実行時に 403 Forbidden エラーを参照してください可能性があります。 回避策には、この問題は、次のように、web.config を追加します。`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 クラッシュを入力すると、'\' Razor ファイルに文字列リテラルでします。** 作業を問題を回避文字列リテラルの終わりの引用符を入力してください。
- **参照する&quot;アカウント/管理&quot;CHS、TRK および CHT 言語の実行時エラーでインターネット テンプレート結果にします。** 問題を修正するを分離するページを変更する *@User.Identity.Name* 内のみのコンテンツとして配置することで、 *&lt;強力な&gt;*タグ。
- **Google および LinkedIn プロバイダーでは、Azure Web サイト内ではサポートされていません。** Azure Web サイトに配置する場合は、代替の認証プロバイダーを使用します。
- **UriPathExtensionMapping と IIS 8 Express と IIS を使用する場合は、拡張機能を使用しようとしたときに 404 Not Found エラーを受信はします。** 静的ファイル ハンドラーを使用している web Api への要求が妨げられます*UriPathExtensionMappings*です。 設定*runAllManagedModulesForAllRequests = true*問題を回避する web.config でします。
- **Controller.Execute メソッドが呼び出されない。** すべての MVC コント ローラーは、今すぐ常に非同期的に実行します。
