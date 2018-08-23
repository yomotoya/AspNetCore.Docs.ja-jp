---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET および Web Tools 2012.2 のリリース ノート |Microsoft Docs
author: rick-anderson
description: ASP.NET and Web Tools 2012.2 のリリース ノート。
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: 0566a362b36f6cfb73b6479cd490e82c63455459
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828037"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET および Web Tools 2012.2 のリリース ノートします。
====================
> このドキュメントでは、ASP.NET and Web Tools 2012.2 のリリースについて説明します。 Visual Studio Web ツールおよび ASP.NET の更新プログラムになります。


- [インストールに関する注記](#_Installation)
- [ドキュメント](#_Documentation)
- [サポート](#_Support)
- [ソフトウェアの要件](#_Software_Requirements)
- [ASP.NET および Web Tools 2012.2 の新機能](#_New_Features_in)

    - [ツール](#_Tooling)
    - [Web の発行](#_Web_Publishing)
    - [ASP.NET MVC テンプレート](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly Url](#_ASP.NET_Friendly_URLs)
- [既知の問題と重大な変更](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>インストールに関する注記

使用して、ASP.NET と Visual Studio 2012 用 Web ツール 2012.2 をインストールできる[Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)します。 これは、Visual Studio 2012 または Visual Studio Express 2012 for Web を必要とされる更新プログラムです。 Visual Studio がインストールされていない場合は、Visual Studio Express 2012 for Web がインストールされます。

ASP.NET and Web Tools 2012.2 を手動でインストールすることもできます。 Visual Studio 2012 または Visual Studio Express 2012 for Web がインストールされていることが必要です。 次の手順を使用しています。 

1. ダウンロード[ASP.NET および Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe)インストーラーをダウンロード センターから。
2. ときにメッセージが表示されたらクリックを実行します。 後で実行するファイルを保存することもできます。
3. 更新は、Visual Studio のバージョンを確認します。 更新する場合、Visual Studio を起動することで、これを行うことができます。 ヘルプのメニュー項目をクリックします。   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. メニュー項目を表示する場合&quot;について Microsoft Visual Studio 2012 for Web&quot;ダウンロード[Web Developer Tools 2012.2 - Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228)します。 それ以外の場合ダウンロード[Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)します。
5. ときにメッセージが表示されたらクリックを実行します。 後で実行するファイルを保存することもできます。

> [!NOTE]
> ASP.NET and Web Tools 2012.2 のリリースでは、SQL Server Data Tools は含まれません。 SQL Server および Windows Azure SQL データベースは、データベース プロジェクトを基盤とオフラインの開発、スキーマ比較拡張データベースの配置機能などのツールの豊富なセットを提供します。 詳細については、または SQL Server Data Tools をインストールするを参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)します。

<a id="_Documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルとその他の情報は、ASP.NET and Web Tools 2012.2 は ASP.NET web サイトから入手できます (https://www.asp.net)します。

<a id="_Support"></a>
## <a name="support"></a>サポート

ASP.NET and Web Tools 2012.2 が正式にリリースおよびサポートされています。 通常のサポート チャネルを使用することができます。 ASP.NET フォーラムに質問を投稿することもできます ([https://forums.asp.net/](https://forums.asp.net/))、ASP.NET コミュニティのメンバーが頻繁に、非公式のサポートを提供することができます。

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET および Web Tools 2012.2 Web 用 Visual Studio 2012 または Visual Studio Express 2012 が必要です。

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>ASP.NET および Web Tools 2012.2 の新機能

このセクションでは、ASP.NET and Web Tools 2012.2 のリリースで導入された機能について説明します。

<a id="_Tooling"></a>
### <a name="tooling"></a>ツール

- Page Inspector 

    - Page Inspector に対応する JavaScript コードに戻り、ページに動的に追加された項目をマップすることができます、JavaScript の選択範囲マッピングをサポートします。
    - リアルタイムで CSS 更新プログラムを表示する権限です。
    - 詳細については、読み取る[CSS 自動同期と Page Inspector 内でマッピングの選択範囲を JavaScript](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx)します。
- エディター 

    - CoffeeScript、Mustache、Handlebars、および JsRender の構文の強調表示をサポートします。
    - HTML エディターは、Knockout バインディング用の Intellisense を提供します。
    - 小さい編集とコンパイラは、以下を使用して動的な CSS の構築が可能にするサポートします。
    - .NET クラスとして JSON を貼り付けます。 この特別な貼り付けコマンドを使用して、c# または VB.NET コード ファイル、および Visual Studio に JSON を貼り付けるにすると、JSON から推論される .NET クラスが自動的に生成します。
- モバイル エミュレーターのサポートは、VSIX としてサード パーティ製のエミュレーターをインストールすることができますので、拡張性フックを追加します。 開発者はさまざまなモバイル デバイスで web サイトをプレビューできるように、インストールされているエミュレーターが f5 キーを押して、ドロップダウン リストに表示されます。 Scott Hanselman のブログ エントリでは、この機能の詳細について[Visual Studio と新しい BrowserStack 統合](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)します。

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Web の発行

- Web サイト プロジェクトでは、Windows Azure Web サイトへの発行などの Web アプリケーション プロジェクトと同じ発行エクスペリエンスがあるようになりました。
- 選択して公開&#8211;の 1 つまたは複数のファイルは、(Web Deploy のエンドポイントへの発行) の後、次の操作を行うことができます。 

    - 選択したファイルを発行します。
    - ローカル ファイルとリモートのファイルの違いを参照してください。
    - リモート ファイルを使用してローカルのファイルを更新するか、ローカルのファイルでリモート ファイルを更新します。

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC テンプレート

- 新しい Facebook Application テンプレートは、アプリケーションを簡単に Facebook Canvas を作成します。 いくつかの簡単な手順では、ログインしているユーザーからデータを取得し、その友達と統合する Facebook アプリケーションを作成できます。 テンプレートには、認証、Facebook データへのアクセスのアクセス許可を含む、Facebook アプリの作成に必要なすべてのパーツを処理する新しいライブラリが含まれています。 Facebook アプリケーション テンプレートを使用しての詳細については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)します。
- 新しい Single Page Application MVC テンプレートは、HTML 5、CSS 3、および人気のある Knockout、jQuery JavaScript ライブラリを ASP.NET Web API を使用して対話型のクライアント側 web アプリを構築する開発者を使用できます。 テンプレートには、RESTful サーバー API を使用した JavaScript の HTML5 アプリケーションを構築するための一般的なプラクティスを示す"todo"リスト アプリケーションが含まれています。 詳細に[ https://www.asp.net/single-page-application](../../../single-page-application/index.md)します。
- ASP.NET MVC の新しいプロジェクト ダイアログ ボックスに新しいテンプレートを追加する VSIX を作成できます。 方法について説明します。 [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes パッケージ&#8211;を MVC 4 でのバグの回避策を含む新しい 'FixedDisplayModes' NuGet パッケージを含めるに MVC プロジェクト テンプレートが更新されました。 パッケージに含まれている修正プログラムの詳細については、このブログの投稿を参照してください ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) MVC チームから。

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API は、いくつかの新機能で強化されています。

- ASP.NET Web API OData
- ASP.NET Web API のトレース
- ASP.NET Web API ヘルプ ページ

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData では、任意のデータ ソースの豊富なビジネス ロジックで OData エンドポイントを作成する必要がある柔軟性できます。 ASP.NET Web API OData では、公開する OData のセマンティクスの量を制御します。 ASP.NET Web API OData、ASP.NET MVC 4 プロジェクト テンプレートに含まれているし、NuGet からも使用 ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata))。

ASP.NET Web API OData は、現在、次の機能をサポートします。

- [Queryable] 属性を適用することにより、OData クエリのセマンティクスを有効にします。
- 簡単に OData クエリを検証し、サポートされるクエリ オプション、演算子と関数のセットを制限します。
- パラメーターは、検証および IEnumerable または IQueryable に適用できるは、クエリの抽象構文ツリー表現を取得するには、直接 ODataQueryOptions にバインドします。
- [Queryable] 属性の結果の制限を指定することで、サービス主導のページングと次ページ リンクの生成を有効にします。
- $Inlinecount を使用して一致するリソースの合計数のインライン展開の数を要求します。
- Null の伝達を制御します。
- $Filter ですべて]、[すべての演算子。
- 規則でエンティティ データ モデルを推論するか、明示的に Entity Framework Code First と同様の方法でモデルをカスタマイズします。
- 公開のエンティティを EntitySetController から派生することによって設定します。
- ナビゲーション プロパティを公開する、リンクを操作および OData アクションを実装するための単純なカスタマイズ可能な規則です。
- MapODataRoute の拡張メソッドを使用してルーティングを簡略化されます。
- 複数の EDM モデルを公開することでバージョン管理をサポートします。
- Web API のサービス ドキュメントと $metadata クライアント (.NET、Windows Phone、Windows ストアなど) を生成するようにを公開します。
- OData Atom、JSON、および JSON verbose 形式をサポートします。
- 作成、更新、部分的に更新 (PATCH) し、エンティティを削除します。
- クエリを実行して、エンティティ間のリレーションシップを操作します。
- 接続、ルートまでのリレーションシップ リンクを作成します。
- 複合型。
- エンティティ型の継承します。
- コレクションのプロパティ。
- 列挙型。
- OData アクション。
- WCF Data Services、つまり ODataLib と同じ基盤に基づいて構築されています ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata))。

ASP.NET Web API OData の詳細については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)します。

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API のトレース

ASP.NET Web API のトレースは、.NET トレースと web Api からのトレース データを統合します。 これは既定では、Web API プロジェクト テンプレートでは有効になりました。 Api は、web データのトレースは出力ウィンドウに送信し、は IntelliTrace から入手します。 トレースについては、Web API との統合により、Windows Azure でホストされている場合に使用する ASP.NET Web API Tracing [Windows Azure 診断](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx)します。 インストールして ASP.NET Web API Tracing NuGet パッケージを使用して任意のアプリケーションで ASP.NET Web API のトレースを有効にすることもできます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing))。

構成と ASP.NET Web API のトレースの使用の詳細については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874)します。

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API ヘルプ ページ

既定では、Web API プロジェクト テンプレートの ASP.NET Web API ヘルプ ページが含まれているようになりました。 ASP.NET Web API ヘルプ ページには、web Api の HTTP エンドポイント、サポートされる HTTP メソッド、パラメーターおよび例要求と応答メッセージのペイロードを含むドキュメントが自動的に生成されます。 ドキュメントは、コードのコメントから自動的にプルされます。 ASP.NET Web API ヘルプ ページ NuGet パッケージを使用して任意のアプリケーションに ASP.NET Web API ヘルプ ページを追加することもできます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage))。

設定して、ASP.NET Web API ヘルプ ページを参照のカスタマイズの詳細については[ https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)します。

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR では、使用可能な場合は Websocket を使用して、自動的にフォールバックを他の方法でない場合、単純に、ASP.NET アプリケーションでは、リアルタイム web 機能を追加します。

ASP.NET SignalR を使用しての詳細については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)します。

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly Url

ASP.NET FriendlyURLs では、クリーナー Url を検索 (.aspx 拡張子なし) を生成する web フォームの開発者にとって非常に簡単です。 No にほとんどの構成が必要ですし、既存の ASP.NET v4.0 アプリケーションで使用できます。 FriendlyURLs 機能では、デスクトップとモバイル ビュー間の切り替えをサポートすることによって、アプリケーションにモバイル デバイスのサポートを追加する開発者が簡単にします。

インストールして、ASP.NET Friendly Url を使用しての詳細については、次を参照してください。 [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)します。

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と ASP.NET and Web Tools 2012.2 のリリースでは重大な変更について説明します。

### <a name="installation-issues"></a>インストールの問題

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Visual Studio 2012 の誤順序のインストールします。

修復操作が必要になります、ASP.NET and Web Tools 2012.2 をインストールした後、追加の SKU の Visual Studio 2012 をインストールします。 次の順序を考慮してください。

1. Web 用の Visual Studio 2012 Express をインストールします。
2. ASP.NET および Web Tools 2012.2 をインストールします。
3. Visual Studio 2012 Professional、Premium または Ultimate をインストールします。

手順 2 のみを Express for Web 更新プログラムをインストールすることになります。 手順 3 をインストールする追加の SKU に更新プログラムが含まれていることを確認するのには、最後にインストールされている SKU の更新プログラムをインストールするには、ASP.NET と Web ツール 2012.2 を修復する必要があります。 これは、手順 1. で Sku と 3 が取り消された場合にも当てはまります。

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Visual Studio が開いているときは、Microsoft ASP.NET and Web Tools 2012.2 をインストールします。

Microsoft ASP.NET and Web Tools 2012.2 のインストール時に VS が開いている場合、Visual Studio は異常な状態に最終的可能性があります。 ユーザーが、インストールを続行する前に Visual Studio のすべてのインスタンスを閉じることをお勧めします。

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>インストールの途中で ASP.NET and Web Tools 2012.2 のセットアップの取り消し

ASP.NET and Web Tools 2012.2 のキャンセル セットアップ インストールの途中では状態のままに Visual Studio が正しくありません。 次の手順をこの問題に従ってくださいに対処します。 

- 追加のプログラムの削除 に移動
- 存在する場合は、Microsoft ASP.NET および Web Tools 2012.2 をアンインストールします。
- Microsoft ASP.NET および Web Tools 2012.2 を再インストールします。

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>ASP.NET and Web Tools 2012.2、ASP.NET MVC 4 をアンインストールした後にテンプレートと Razor v2 の Web サイト テンプレートがありません。

アンインストールしても ASP.NET and Web Tools 2012.2 もアンインストールのすべての ASP.NET MVC 4 と Razor v2 の Web サイト テンプレートから Visual Studio 2012。

回避策では、ASP.NET MVC 4 Razor v2 の Web サイト テンプレートを再インストールする、Visual Studio 2012 のインストールを修復します。

### <a name="tooling-issues"></a>ツールの問題

#### <a name="nuget-error-reported-during-project-creation"></a>プロジェクトの作成時に NuGet のエラー

ASP.NET and Web Tools 2012.2 をインストールした後、MVC 4 プロジェクトを作成するときに、次のエラーを表示可能性があります。

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET および Web Tools 2012.2 NuGet 2.1 に付属し、Visual Studio 2012 で拡張機能のアップグレードはします。 場合によっては、VSIX インストーラーは、VSIX を正しく更新に失敗します。 次の手順ではこの問題に対処することを許可します。

1. 管理者として Visual Studio 2012 を起動します。
2. ツールに移動して&gt;拡張機能と更新プログラムと NuGet をアンインストールします。
3. Visual Studio を閉じます。
4. ASP.NET and Web Tools 2012.2 のインストール フォルダーに移動します。

    1. Visual Studio 2012: **\microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012 のプログラミング**
    2. Visual Studio 2012 Express for Web の: **Program files \microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. NuGet を再インストールする NuGet.Tools.vsix をダブルクリックします。

### <a name="web-api-issues"></a>Web API の問題

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>解析の $filter と DateTime リテラルでの問題

OData URI パーサーは、部分的な datetime リテラルを正しく解析に失敗します。 例: $filter = 開始 eq datetime'2012-12-31T12:00' に正しく解析に失敗します。 回避策は、完全なリテラルの $filter を使用する = 開始 eq datetime'2012-12-31T12:00:00'。

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData では、大文字のプロパティ名をサポートしていません。

OData では、OData クエリと odata パスで大文字のプロパティ名をサポートしていません。 作業項目を参照してください。

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

ユーザーが javascript クライアント側とサーバー側で大文字小文字の区別を使用している場合、おそらくこの問題は発生します。 この問題は odata プロトコルの仕様です。 ただし、多くのユーザーは、この問題を報告します。 これを回避するには、ユーザーは、URL で、大文字と小文字を修正する必要です。

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>ナビゲーション プロパティの既定の OData ルーティング規約に POST または PUT をサポートしていません。

ナビゲーション プロパティの既定の OData ルーティング規約に POST または PUT をサポートしていません。 作業項目を参照してください。 [ http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)します。 私たちは、既定の規則でよく使用されるこの規則ではありません。

これを回避するには、ユーザーをサポートするように新しいルーティング規則を拡張する必要があります。

### <a name="facebook-template-issues"></a>Facebook テンプレートの問題

#### <a name="facebook-application-template-only-works-using-net-45"></a>.NET 4.5 を使用する Facebook アプリケーションのテンプレートとのみ動作します。

ASP.NET MVC 4 で Facebook アプリケーション テンプレートを表示する新しいプロジェクト ダイアログで、フレームワークのドロップダウン リストで、.NET 4.5 を選択する必要があります。

#### <a name="real-time-update-controller"></a>コント ローラーのリアルタイムの更新

Facebook アプリケーション テンプレートにより、簡単にユーザーの Facebook からのリアルタイムの更新を処理するために Web API コント ローラーを作成します。 開発用コンピューターが NAT の内側にある場合は、さらにネットワーク構成を行わなくても、コント ローラーは機能しません。 詳細についてはこちらを参照してください。 [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>文字列値が競合して Facebook OAuth パラメーターを持つクエリ

Facebook OAuth ダイアログ ボックスの呼び出しと競合する、次のフィールドの URL をバックアップします。 次の名前を持つ独自のクエリ文字列値を追加しないでください。 コード、エラー、エラー\_説明、エラー\_理由。

#### <a name="using-page-inspector-with-facebook-template"></a>Facebook テンプレートで Page Inspector の使用

Facebook アプリケーションのデバッグ中に Visual Studio 2012 で Page Inspector の機能を使用することはできません。 Page Inspector は、iframe を現在サポートしていません。

### <a name="single-page-application-template-issues"></a>シングル ページ アプリケーション テンプレートの問題

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Jquery 1.9/Knockout 2.2.1 更新プログラム、既定の MVC SPA プロジェクトで新しい todo 項目の編集を実行するときに入力フォーカス イベントが正しく処理されません。

JQuery 1.9/Knockout 2.2.1 更新プログラム、既定の MVC SPA プロジェクトを実行するときに新しい todo 項目の編集入力不要になったフォーカス新しい todo 項目の編集ボックスに戻ります、todo リストに新しい todo 項目を入力した後にします。

回避策の参照を[ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)、しのような修復を次のサンプル コード。

ファイル todo.model.js  
 todolist(data) の関数を追加次。  
 **self.isSelected ko.observable(false); を =**

todoList.prototype.addTodo を関数を次に黒くテキストを追加します。  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Index.cshtml をファイルに追加し、次の黒くテキスト。  
 &lt;データ バインド フォーム =&quot;送信: addTodo&quot;&gt;  
 &lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;  
 &lt;/form&gt;
