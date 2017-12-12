---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: "ASP.NET および Web ツール 2012.2 のリリース ノート |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET および Web ツール 2012.2 リリース ノートです。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: b9abad56a5a5b9219f92cc5b96efee7250a97c55
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET および Web ツール 2012.2 リリース ノートには
====================
> このドキュメントでは、ASP.NET および Web ツール 2012.2 のリリースについて説明します。 Visual Studio Web ツールと ASP.NET を更新することをお勧めします。


- [インストールに関する注意事項](#_Installation)
- [ドキュメント](#_Documentation)
- [サポート](#_Support)
- [ソフトウェアの要件](#_Software_Requirements)
- [ASP.NET および Web ツール 2012.2 の新機能](#_New_Features_in)

    - [ツール](#_Tooling)
    - [Web の発行](#_Web_Publishing)
    - [ASP.NET MVC テンプレート](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly Url](#_ASP.NET_Friendly_URLs)
- [既知の問題と重大な変更](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>インストールに関する注意事項

使用して、ASP.NET および Web ツール 2012.2 for Visual Studio 2012 をインストールできる[Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)です。 これは、更新プログラムを Visual Studio 2012 または Visual Studio Express 2012 for Web が必要です。 Visual Studio がインストールされていない場合は、Visual Studio Express 2012 for Web がインストールされます。

ASP.NET および Web ツール 2012.2 を手動でインストールすることもできます。 Visual Studio 2012 または Visual Studio Express 2012 for Web をインストールする必要があります。 次の手順を使用します。 

1. ダウンロード[ASP.NET および Frameworks 2012.2 の Web](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe)インストーラーをダウンロード センターからです。
2. ときにメッセージが表示されたらクリックを実行します。 後で実行するファイルを保存することもできます。
3. 更新が表示される Visual Studio のバージョンを確認します。 更新する場合、Visual Studio を起動して、これを行うことができます。 ヘルプ メニューの項目 をクリックします。   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. メニュー項目を表示する場合は&quot;に関する Microsoft Visual Studio 2012 for Web&quot;ダウンロード[Web Developer Tools 2012.2 - Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228)です。 それ以外の場合にダウンロード[Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)です。
5. ときにメッセージが表示されたらクリックを実行します。 後で実行するファイルを保存することもできます。

> [!NOTE]
> ASP.NET および Web ツール 2012.2 のリリースでは、SQL Server Data Tools は含まれません。 SQL Server および Windows Azure SQL データベースは、豊富な一連のデータベース プロジェクトに基づくオフラインの開発、スキーマ比較の展開機能の強化されたデータベースなどのツールを提供します。 または SQL Server Data Tools をインストールする場合の詳細については、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)です。

<a id="_Documentation"></a>
## <a name="documentation"></a>ドキュメント

チュートリアルおよびその他の情報は、ASP.NET および Web ツール 2012.2 は ASP.NET web サイト (https://www.asp.net) から入手できます。

<a id="_Support"></a>
## <a name="support"></a>Support

ASP.NET および Web ツール 2012.2 正式にリリースされた、サポートします。 通常のサポート チャネルを使用することができます。 ASP.NET フォーラムに質問を投稿することもできます ([https://forums.asp.net/](https://forums.asp.net/)) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>ソフトウェア要件

ASP.NET および Web ツール 2012.2 Web の Visual Studio 2012 または Visual Studio Express 2012 が必要です。

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>ASP.NET および Web ツール 2012.2 の新機能

このセクションでは、ASP.NET および Web ツール 2012.2 リリースで導入された機能について説明します。

<a id="_Tooling"></a>
### <a name="tooling"></a>ツール

- Page Inspector 

    - マップ ページに対応する JavaScript コードに動的に追加されたアイテムに Page Inspector を許可する JavaScript の選択のマッピングをサポートします。
    - CSS を表示する機能は、リアルタイムで更新されます。
    - 詳細については、「 [CSS 自動同期と Page Inspector 内で JavaScript 選択マッピング](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx)です。
- エディター 

    - CoffeeScript、見える、ハンドル、および JsRender の構文の強調表示をサポートします。
    - HTML エディターは、Knockout バインドの Intellisense を提供します。
    - 以下の編集およびコンパイラがサポート以下を使用して動的な CSS のビルドを有効にします。
    - .NET クラスとして JSON ペーストしてください。 この特別な貼り付けコマンドを使用して、c#、VB.NET コード ファイル、および Visual Studio に JSON を貼り付けると、JSON から推論しようとする .NET クラスが自動的に生成します。
- モバイル エミュレーターのサポートは、VSIX としてサード パーティ製のエミュレーターをインストールできるように機能拡張フックを追加します。 開発者がさまざまなモバイル デバイス上の web サイトをプレビューできるように、インストールされているエミュレーターが f5 キーを押して、ドロップダウン リストに表示されます。 詳しくは、この機能に関するブログ エントリの Scott Hanselman に[Visual Studio で新しい BrowserStack 統合](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)です。

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Web の発行

- Web サイト プロジェクトでは、Windows Azure Web サイトへの発行などの Web アプリケーション プロジェクトと同じ公開機能があるようになりました。
- 選択的発行 &#8211;です。1 つまたは複数のファイル (Web 配置のエンドポイントへの発行) した後、次の操作を実行できます。 

    - 選択したファイルを発行します。
    - ローカル ファイルとリモートのファイルの差分を参照してください。
    - リモート ファイルをローカル ファイルを更新するか、ローカル ファイルで、リモート ファイルを更新します。

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC テンプレート

- 新しい Facebook Application テンプレートは、Facebook Canvas に簡単なアプリケーションを作成します。 いくつかの簡単な手順では、ログインしているユーザーからデータを取得し、その友達と統合し、Facebook アプリケーションを作成できます。 テンプレートには、認証、アクセス許可、Facebook データへのアクセスなど、Facebook アプリケーションのビルドに関連するすべての機能を処理する新しいライブラリが含まれています。 Facebook アプリケーション テンプレートを使用する方法については「 [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)です。
- 新しい Single Page Application MVC テンプレートは、HTML 5、CSS 3、および人気のある Knockout、jQuery JavaScript ライブラリを ASP.NET Web API を使用して対話型のクライアント側 web アプリをビルドする開発者を使用できます。 テンプレートには、RESTful サーバー API を使用する JavaScript HTML5 アプリケーションを構築するための一般的なプラクティスを示す"todo"リスト アプリケーションが含まれています。 詳細は、「を読み取ることができます[https://www.asp.net/single-page-application](../../../single-page-application/index.md)です。
- ASP.NET MVC の新しいプロジェクト ダイアログ ボックスに新しいテンプレートを追加するための VSIX を作成できます。 方法について説明します[https://go.microsoft.com/fwlink/?LinkId=275019。](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes パッケージ &#8211;です。MVC 4 のバグの回避策を含む新しい 'FixedDisplayModes' NuGet パッケージを含めるには、MVC プロジェクト テンプレートが更新されました。 パッケージに含まれている修正プログラムの詳細については、このブログの投稿を参照してください ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) MVC チームからです。

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API は、いくつかの新しい機能で強化されています。

- ASP.NET Web API OData
- ASP.NET Web API Tracing
- ASP.NET Web API Help Page

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData の柔軟性を任意のデータ ソースでも豊富なビジネス ロジックで OData エンドポイントを作成する必要があります。 ASP.NET Web API OData を使用して、公開する OData セマンティクスの量を制御します。 ASP.NET Web API OData、ASP.NET MVC 4 プロジェクト テンプレートに含まれても NuGet から ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata))。

現在、ASP.NET Web API OData には、次の機能がサポートされています。

- [Queryable] 属性を適用することにより、OData クエリのセマンティクスを有効にします。
- 簡単に OData のクエリを検証し、サポートされるクエリ オプション、演算子と関数のセットを制限します。
- パラメーターは、検証、および、IQueryable または IEnumerable に適用できるクエリの抽象構文ツリー表現を取得するには、直接 ODataQueryOptions にバインドします。
- [Queryable] 属性で結果の制限を指定することによってサービス ドリブン ページングと次のページ リンクの生成を有効にします。
- $Inlinecount を使用して一致するリソースの合計数のインライン展開の数を要求します。
- Null の伝達を制御します。
- $Filter ですべて/すべての演算子。
- Entity data model を規約によって推定または明示的に同様に Entity Framework コード優先の方法でモデルをカスタマイズします。
- EntitySetController から派生することで公開してエンティティを設定します。
- ナビゲーション プロパティを公開する、リンクを操作および OData アクションを実装するため、単純なカスタマイズ可能な規則です。
- MapODataRoute 拡張メソッドを使用してルーティングを簡略化されます。
- 複数の EDM モデルを公開することでバージョン管理をサポートします。
- Web api サービス ドキュメントおよび $metadata クライアント (.NET、Windows Phone、Windows ストアなど) を生成するようにを公開します。
- OData Atom、JSON、および JSON の冗長形式をサポートします。
- 作成、更新、部分的に更新 (PATCH) し、エンティティを削除します。
- クエリおよびエンティティ間のリレーションシップを操作します。
- ネットワーク上で、ルートまでリレーションシップ リンクを作成します。
- 複合型。
- エンティティ型の継承します。
- コレクションのプロパティです。
- 列挙型。
- OData アクション。
- WCF Data Services、つまり ODataLib として同じ基盤に構築された ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata))。

ASP.NET Web API OData の詳細については、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)です。

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API Tracing

ASP.NET Web API のトレースは、.NET トレースと web Api からのトレース データを統合できます。 これは既定では、Web API プロジェクト テンプレートでは有効になりました。 Web のデータをトレース Api は、出力ウィンドウに送信され、IntelliTrace をとおして利用可能な。 統合により、Windows Azure でホストされている場合は、Web API に関するトレース情報を有効にする ASP.NET Web API Tracing [Windows Azure 診断](https://msdn.microsoft.com/en-us/library/windowsazure/hh411529.aspx)です。 インストールし、ASP.NET Web API Tracing NuGet パッケージを使用して任意のアプリケーションで ASP.NET Web API のトレースを有効にすることができます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing))。

詳細については、構成して、ASP.NET Web API Tracing を使用して、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874)です。

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API Help Page

既定では、Web API プロジェクト テンプレートで、ASP.NET Web API のヘルプ ページが含まれているようになりました。 ASP.NET Web API のヘルプ ページには、web Api HTTP エンドポイント、サポートされる HTTP メソッド、パラメーターおよび例要求と応答メッセージのペイロードをなどのドキュメントが自動的に生成されます。 ドキュメントは自動的に、コードのコメントからプルされなくなります。 ASP.NET Web API のヘルプ ページ NuGet パッケージを使用しているアプリケーションを ASP.NET Web API のヘルプ ページを追加することもできます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage))。

詳細については、セットアップと ASP.NET Web API のヘルプ ページを参照してくださいをカスタマイズ[https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)です。

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR を簡単に、ASP.NET アプリケーションにリアルタイム web 機能を追加可能な場合は Websocket を使用して、自動的にフォールバックし他の手法でない場合。

ASP.NET SignalR を使用する方法については「 [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)です。

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly Url

ASP.NET FriendlyURLs なりますクリーナー Url を検索 (.aspx 拡張子なし) を生成する web フォーム開発者にとって非常に簡単です。 No にほとんどの構成が必要ですし、既存の ASP.NET v4.0 アプリケーションで使用できます。 FriendlyURLs 機能では、デスクトップおよびモバイル ビュー間の切り替えをサポートすることにより、アプリケーションにモバイル デバイスのサポートを追加する開発者を簡単にします。

詳細については、インストールして、ASP.NET Friendly Url を使用して、次を参照してください。 [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)です。

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>既知の問題と重大な変更

このセクションでは、既知の問題と ASP.NET および Web ツール 2012.2 リリースでは重大な変更について説明します。

### <a name="installation-issues"></a>インストールの問題

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Visual Studio 2012 を順序どおりにインストールします。

修復操作が必要になります、ASP.NET および Web ツール 2012.2 をインストールした後、追加の SKU の Visual Studio 2012 をインストールします。 次の順序を考慮してください。

1. Visual Studio 2012 Express for Web をインストールします。
2. ASP.NET および Web ツール 2012.2 をインストールします。
3. Visual Studio 2012 Professional、Premium または Ultimate をインストールします。

手順 2 は、Express for Web の更新プログラムのインストールでのみなります。 手順 3 をインストールする追加の SKU に、更新プログラムが含まれていることを確認するには、インストールされている最後の SKU の更新プログラムをインストールするには、ASP.NET および Web ツール 2012.2 を修復する必要があります。 これは、手順 1. で Sku と 3 が取り消された場合にも当てはまります。

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Visual Studio を開いている場合は、Microsoft ASP.NET および Web ツール 2012.2 をインストールします。

Microsoft ASP.NET および Web ツール 2012.2 のインストール中に VS が開かれている場合、Visual Studio が正しくない状態に至る可能性があります。 ユーザーがインストールを続行する前に Visual Studio のすべてのインスタンスを閉じることをお勧めします。

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>インストールの途中で ASP.NET および Web ツール 2012.2 のセットアップを取り消す

ASP.NET と Web ツール 2012.2 取り消しセットアップ インストールの途中では状態のままに Visual Studio が正しくありません。 次の手順をこの問題に従ってくださいに対処します。 

- プログラムの削除 を追加するには、移動してください。
- 存在する場合は、Microsoft ASP.NET および Web ツール 2012.2 をアンインストールします。
- Microsoft ASP.NET および Web ツール 2012.2 を再インストールします。

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>ASP.NET および Web ツール 2012.2 ASP.NET MVC 4 をアンインストールした後にテンプレートおよび Razor v2 の Web サイト テンプレートが表示されません。

ASP.NET および Web ツール 2012.2 のアンインストールもアンインストールされますすべて ASP.NET MVC 4、Razor v2 の Web サイト テンプレートの Visual Studio 2012 からです。

回避策では、ASP.NET MVC 4、Razor v2 の Web サイト テンプレートを再インストール、Visual Studio 2012 インストールを修復します。

### <a name="tooling-issues"></a>ツールの問題

#### <a name="nuget-error-reported-during-project-creation"></a>プロジェクト作成時に NuGet のエラー

ASP.NET および Web ツール 2012.2 をインストールした後、MVC 4 プロジェクトを作成するときに、次のエラーを表示可能性があります。

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET および Web ツール 2012.2 NuGet 2.1 に付属し、Visual Studio 2012 での拡張機能がアップグレードされます。 場合によっては、VSIX インストーラーは、VSIX を正しく更新に失敗します。 次の手順ではこの問題に対処することを許可します。

1. 管理者として Visual Studio 2012 を起動します。
2. ツールに移動して&gt;拡張機能と更新プログラムおよび NuGet をアンインストールします。
3. Visual Studio を閉じます。
4. ASP.NET および Web ツール 2012.2 インストール フォルダーに移動します。

    1. Visual Studio 2012: **\microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012 のプログラミング**
    2. Visual Studio 2012 Express for Web の: **Program files \microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. NuGet を再インストールする NuGet.Tools.vsix をダブルクリックします。

### <a name="web-api-issues"></a>Web API の問題

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>$Filter と日付時刻リテラルの解析を行って問題

OData URI パーサーは、部分的な datetime リテラルを正しく解析に失敗します。 たとえば、$filter = 開始 eq datetime'2012-12-31T12:00' を正しく解析できない場合します。 回避策は、完全リテラル $filter を使用する = 開始 eq datetime'2012-12-31T12:00:00' です。

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData は、大文字と小文字のプロパティ名をサポートしていません。

OData は、OData クエリおよび odata パスの大文字と小文字のプロパティ名をサポートしていません。 作業項目を参照してください。

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

ユーザーが javascript クライアント側とサーバー側で大文字小文字が異なる場合、おそらくこの問題は発生します。 この問題は odata プロトコルの仕様です。 ただし、多くのユーザーは、この問題を報告します。 問題を回避するには、ユーザーは URL 内のケースを修正する必要があります。

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>既定の OData ルーティング規約は、ナビゲーション プロパティに POST または PUT をサポートしていません。

既定の OData ルーティング規約は、ナビゲーション プロパティに POST または PUT をサポートしていません。 作業項目を参照してください[http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)です。 既定の規則でよく使用されるこの規約おがありません。

これを回避するには、ユーザーをサポートするように新しいルーティング規約を拡張する必要があります。

### <a name="facebook-template-issues"></a>Facebook テンプレートの問題

#### <a name="facebook-application-template-only-works-using-net-45"></a>.NET 4.5 を使用する Facebook Application テンプレートとのみ動作します。

.NET 4.5 を選択して、フレームワーク ドロップダウン リストで、新しいプロジェクト ダイアログ ボックスに ASP.NET MVC 4 の Facebook アプリケーション テンプレートを参照してください。

#### <a name="real-time-update-controller"></a>コント ローラーのリアルタイムの更新

Facebook Application テンプレートにより、簡単にユーザーの Facebook からのリアルタイムの更新を処理する Web API コント ローラーを作成します。 開発用コンピューターが NAT の内側にある場合は、コント ローラーがネットワーク構成を変更しない動作しない可能性があります。 詳細については、ここを参照してください: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>文字列値が競合して Facebook OAuth パラメーターを持つクエリ

Facebook OAuth ダイアログ ボックスの呼び出しと競合する、次のフィールドの URL をバックアップします。 次の名前、独自のクエリ文字列値を追加しないでください。 コード、エラー、エラー\_説明、エラー\_理由。

#### <a name="using-page-inspector-with-facebook-template"></a>Facebook テンプレートでの Page Inspector の使用

Facebook アプリケーションのデバッグ中に Visual Studio 2012 では、Page Inspector の機能を使うことはできません。 Page Inspector は、iframe を現在サポートしていません。

### <a name="single-page-application-template-issues"></a>単一ページ アプリケーション テンプレートの問題

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>JQuery 1.9/Knockout 2.2.1 update を既定の MVC SPA プロジェクト、新しい todo 項目の編集を実行している入力のフォーカス イベントが正しく処理されません。

JQuery 1.9/Knockout 2.2.1 更新プログラムが既定の MVC SPA プロジェクトを実行するときに新しい todo 項目の編集を不要になったフォーカス新しい todo 項目の編集ボックスに戻る、todo リストに、新しい todo 項目を入力した後にします。

回避策の参照を[http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)のような修正プログラムを次のサンプル コード。

ファイル todo.model.js  
 関数の todolist(data)、追加以下。  
 **self.isSelected ko.observable(false); を =**

次の黒くテキストを追加、todoList.prototype.addTodo を関数します。  
 **self.isSelected(true) です。**  
 self.newTodoTitle (&quot;&quot;) です。

Index.cshtml をファイルに追加し、次の黒くテキスト。  
 &lt;データ バインド フォーム =&quot;送信: addTodo&quot;&gt;  
 &lt;クラスの入力 =&quot;addTodo&quot;型 =&quot;テキスト&quot;データ バインド =&quot;値: newTodoTitle、プレース ホルダー: '型は、ここに追加する'、blurOnEnter: true の場合、**できる: isSelected**、イベント: {ぼかし: addTodo}&quot; /&gt;  
 &lt;/form&gt;
