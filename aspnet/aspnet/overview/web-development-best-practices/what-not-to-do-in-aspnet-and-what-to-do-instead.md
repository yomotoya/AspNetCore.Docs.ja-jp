---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "ASP.NET では、操作を行わないとください |Microsoft ドキュメント"
author: tfitzmac
description: "このトピックでは、ASP.NET web プロジェクト内のユーザーが、いくつかの一般的な誤りを説明します。 これらの commo を避けるために行う必要がありますの推奨事項を提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 6790cd0deb36c9fb297ccd4df371f763dba17844
ms.sourcegitcommit: 17b025bd33f4474f0deaafc6d0447a4e72bcad87
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/27/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>ASP.NET では、操作を行わないと何を代わりに行うには
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、ASP.NET web プロジェクト内のユーザーが、いくつかの一般的な誤りを説明します。 これらの一般的な間違いを避けるために行う必要がありますの推奨事項を提供します。 基にして、[プレゼンテーション](http://vimeo.com/68390507)によって**Damian Edwards**開発者カンファレンスでノルウェー語。


## <a name="disclaimer"></a>免責情報

このトピックはありません完全なガイドとして、アプリケーションが安全かつ効率的なことを確認します。 このトピックに記載されていないいるセキュリティおよびパフォーマンスのベスト プラクティスに従う必要があります。 のみ、.NET クラスおよびプロセスに関連する一般的な誤りを回避する方法を提案します。

## <a name="overview"></a>概要

このトピックは、次のセクションで構成されています。

- [標準への準拠](#standards)

    - [コントロール アダプター](#adapters)
    - [コントロールのスタイル プロパティ](#styleprop)
    - [ページおよびコントロールのコールバック](#callback)
    - [ブラウザー機能の検出](#browsercap)
- [セキュリティ](#security)

    - [要求の検証](#validation)
    - [Cookie なしのフォーム認証とセッション](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [中程度の信頼](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [信頼性とパフォーマンス](#performance)

    - [PreSendRequestHeaders と PreSendRequestContent](#presend)
    - [Web フォーム ページの非同期イベント](#asyncevents)
    - [Fire 忘れた作業](#fire)
    - [要求エンティティ本体](#requestentity)
    - [Response.Redirect と Response.End](#redirect)
    - [エントリおよび ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [長いを実行している要求 (> 110 秒)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>標準への準拠

<a id="adapters"></a>

### <a name="control-adapters"></a>コントロール アダプター

に関する推奨事項: が、アダプティブ表示用コントロール アダプターの使用を停止し、CSS メディア クエリおよび標準に準拠した HTML を代わりに使用します。

コントロールのアダプターは、異なるデバイスや環境用にカスタマイズされたプレゼンテーション コードを表示するために .NET 2.0 で導入されました。 ここで、このアダプティブ レンダリングは、CSS、および HTML を実行できます。 コントロール アダプターの使用を停止し、CSS、および HTML を既存のアダプターに変換する必要があります。

詳細については、次を参照してください。[メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)と[How To: モバイル ページ、ASP.NET Web フォームを追加/MVC アプリケーション](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)です。

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>コントロールのスタイル プロパティ

推奨事項: は、コントロールのマークアップにスタイル値の設定を停止し、代わりに CSS スタイル シートの書式設定の値を設定します。

Web サーバー コントロールは、数十個の行のスタイル プロパティを設定するために使用するプロパティを含めます。 たとえば、前景色プロパティは、コントロールのテキストの色を設定します。 CSS スタイル シートをより効率的にこれと同じ効果を行うことができます。 スタイル シートを使用すると、スタイルの値を集中管理し、アプリケーション全体でこれらの値を設定しないでください。

次の例では、赤にセット テキスト、CSS クラスを示します。

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

次の例では、CSS クラスを動的に適用する方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>ページおよびコントロールのコールバック

に関する推奨事項: がページおよびコントロールのコールバックの使用を停止し、代わりに、次のいずれかを使用: AJAX、UpdatePanel、MVC アクション メソッド、Web API、または SignalR です。

以前のバージョンの ASP.NET では、ページおよびコントロールのコールバック メソッドには、ページ全体を更新することがなく、web ページの一部を更新することが有効になります。 部分ページ更新を実行できるようになりました[AJAX](../../../ajax/index.md)、 [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx)、 [MVC](../../../mvc/index.md)、 [Web API](../../../web-api/index.md)または[SignalR](../../../signalr/index.md). フレンドリな Url で問題が発生することがあるために、コールバック メソッドを使用して、ルーティングを停止する必要があります。 既定では、コントロールには、コールバック メソッドが有効にしていないが、コントロールに、この機能を有効にした場合は無効にします。

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>ブラウザー機能の検出

推奨事項: が静的なブラウザーの機能の検出を使用してを停止し、代わりに動的な機能の検出を使用します。

ASP.NET の以前のバージョンでは、各ブラウザーでサポートされる機能は、XML ファイルに保存されていました。 静的なルックアップによる検出の機能サポートは、最善の方法ではありません。 ここで、動的に検出できます、ブラウザーでサポートされている機能などの機能の検出のフレームワークを使用して、 [Modernizr](http://modernizr.com/)です。 機能の検出は、メソッドまたはプロパティを使用しようとして、ブラウザーで、目的の結果が生成されるかどうかはどうかを確認し、サポートを判断します。 既定では、Web アプリケーション テンプレートに Modernizr が含まれています。

<a id="security"></a>

## <a name="security"></a>セキュリティ

<a id="validation"></a>

### <a name="request-validation"></a>要求の検証

推奨事項: は、ユーザー入力を検証し、ユーザーからの出力をエンコードします。

要求の検証は、各要求を検査し、見かけ上の脅威が見つかった場合は、要求を停止する ASP.NET の機能です。 クロスサイト スクリプティング攻撃に対してアプリケーションを保護するための要求の検証に依存しません。 代わりに、ユーザーからのすべての入力を検証し、出力をエンコードします。 まれに、正規表現を使用して、入力を検証することができますが、値と一致する場合を決定する .NET クラスを使用してユーザー入力が値を許可するより複雑な場合は検証する必要があります。

次の例では、Uri クラスの静的メソッドを使用して、ユーザーによって提供される Uri が有効かどうかを判断する方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

ただし、Uri を十分に確認する必要がありますも確認するを指定しているかどうかを確認する`http`または`https`です。 次の例では、インスタンス メソッドを使用して、Uri が有効であることを確認します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

ユーザー入力を HTML としてレンダリングまたは SQL クエリでユーザー入力を含む、前に、悪意のあるコードが含まれていないことを確認する値をエンコードします。

HTML を作成することができますマークアップで値をエンコード、 &lt;%: %&gt;構文を次のようにします。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Razor 構文では、HTML ことができます。 または、を使用したエンコード @、次のようにします。

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

次の例に示す方法を HTML エンコード分離コード内の値。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

SQL コマンドの値を安全にエンコードする場合などのコマンド パラメーターを使用して、 [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx)です。 <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Cookie なしのフォーム認証とセッション

推奨事項では、cookie が必要です。

クエリ文字列内の認証情報を渡すことは安全ではありません。 そのため、アプリケーションに認証が含まれている場合は、cookie が必要です。 Cookie は、機密情報を保存する場合は、cookie に SSL を必要とすることを検討します。

次の例では、フォーム認証で SSL 経由で送信される cookie は、Web.config ファイルで指定する方法を示します。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

推奨事項: 決して false に設定します。

既定では、EnbableViewStateMac は設定を true にします。 アプリケーションがビュー状態を使用していない場合でも、false に EnableViewStateMac は設定されません。 この値を false に設定すると、アプリケーションがクロス サイト スクリプトに対する脆弱性。

以降で ASP.NET 4.5.2 は、ランタイムでは、 **EnableViewStateMac = true**です。 False に設定すると、場合でも、ランタイムはこの値は無視し、true に設定されている値が続行されます。 詳細については、次を参照してください。 [ASP.NET 4.5.2 および EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)です。

次の例では、EnableViewStateMac を true に設定する方法を示します。 実際には既定で true になっているために、true には、この値を設定する必要はありません。 ただし、設定した場合は false に任意のページで、アプリケーションは、この値を直ちに修正する必要があります。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>中程度の信頼

推奨事項に依存しない中程度の信頼 (またはその他の信頼レベル)、セキュリティ境界として。

部分信頼は、アプリケーションを適切に保護できないは使用できません。 代わりに、完全信頼を使用し、個別のアプリケーション プールで信頼されていないアプリケーションを分離します。 また、一意の id では、各アプリケーション プールを実行します。 詳細については、次を参照してください。 [ASP.NET 部分信頼でアプリケーションの分離が保証されない](https://support.microsoft.com/kb/2698981)です。

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

に関する推奨事項: セキュリティ設定を無効にしない&lt;appSettings&gt;要素。

AppSettings 要素には、セキュリティ更新プログラムの必要な多くの値が含まれています。 変更または、これらの値を無効にする必要がありますされません。 場合は、更新プログラムを展開するときに、これらの値を無効にする必要があります、すぐに再度有効に展開が完了後します。

詳細については、「 [ASP.NET appSettings 要素](https://msdn.microsoft.com/en-us/library/hh975440.aspx)です。

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

推奨事項: を使用して[UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx)代わりにします。

UrlPathEncode メソッドは、非常に特定のブラウザーの互換性の問題を解決するのには、.NET Framework に追加されました。 URL は適切にエンコードされませんし、クロス サイト スクリプトから、アプリケーションを保護しません。 アプリケーションで使用する必要がありますしないでください。 代わりに、 [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx)です。

次の例では、ハイパーリンク コントロール用のクエリ文字列パラメーターとしてエンコードされた URL を渡す方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>信頼性とパフォーマンス

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders と PreSendRequestContent

推奨事項では、マネージ モジュールでこれらのイベントは使用しません。 代わりに、必要なタスクを実行するネイティブの IIS モジュールを記述します。 参照してください[ネイティブ コード HTTP モジュールを作成する](https://msdn.microsoft.com/en-us/library/ms693629.aspx)です。

使用することができます、 [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx)と[PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx)ネイティブの IIS モジュールを持つイベント。
> [!WARNING]
> 使用しないでください`PreSendRequestHeaders`と`PreSendRequestContent`を実装するマネージ モジュールで`IHttpModule`です。 これらのプロパティを設定すると、非同期要求で問題が発生することができます。 アプリケーション要求ルーティング処理 (ARR) と websocket の組み合わせが例外が発生するアクセス違反 w3wp クラッシュする可能性があります。 たとえば、iiscore!W3_CONTEXT_BASE::GetIsLastNotification + 68 iiscore.dll でアクセス違反例外 (0xC0000005) が発生しました。

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Web フォーム ページの非同期イベント

推奨事項: が Web フォームで、ページ ライフ サイクル イベントの void メソッドを非同期に書き込みを回避し、代わりに使用して[Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx)非同期コードにします。

ページのイベントをマークする**非同期**と**void**、非同期コードが終了した場合を判断することはできません。 代わりに、Page.RegisterAsyncTask を使用して、その完了を追跡することができるように、非同期コードを実行します。

ボタンの次の例では、非同期コードを含むハンドラーをクリックします。 この例が含まれます文字列値を非同期的に読み取りと推奨される方法ではなく、非同期タスクの簡単な例としてのみ提供されています。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

非同期タスクを使用している場合は、Web.config ファイルで 4.5 に Http ランタイム ターゲット フレームワークを設定します。 新しい同期コンテキストで 4.5 ターンにターゲット フレームワークを設定すると、.NET 4.5 で追加されました。 この値は、既定では、Visual Studio 2012 の新しいプロジェクトに設定されているが、設定になっていない既存のプロジェクトで作業している場合は。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Fire 忘れた作業

推奨事項: ASP.NET 内で要求を処理する場合に、火災忘れた作業 (このような threadpool.queueuserworkitem にメソッドを呼び出すまたはデリゲートを繰り返し呼び出すタイマーを作成する) を起動しないでください。

アプリケーションに ASP.NET 内で実行される起動忘れた作業がある場合、アプリケーションは同期で取得できます。いつでも、アプリケーション ドメインが、継続的なプロセスは、アプリケーションの現在の状態を一致不要になった可能性があることを意味するを破壊することができます。

この種の ASP.NET の外部での作業を移動する必要があります。 進行中の作業を実行する Azure で Web ジョブ、Windows サービスまたはワーカー ロールを使用して別のプロセスからそのコードを実行します。

呼ばれる Nuget パッケージを追加することができる場合、ASP.NET 内でこの作業を実行する必要があります[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)コードを実行します。

<a id="requestentity"></a>

### <a name="request-entity-body"></a>要求エンティティ本体

推奨事項: は、Request.Form または Request.InputStream を読み取り、ハンドラーのイベントを実行する前にしないでください。

最も早い Request.Form または Request.InputStream から読み取る必要がありますが、ハンドラーの中にイベントを実行します。 MVC では、コント ローラーは、ハンドラーと、execute イベント、アクション メソッドを実行するとします。 Web フォームのページは、ハンドラーと、execute イベント Page.Init イベントの発生時です。 Execute イベントより前に要求エンティティ本体を読み取るする場合は、要求の処理干渉します。

Execute イベントの前に要求エンティティ本体を読み取る必要がある場合を使用するか[Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx)または[Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx)です。 GetBufferlessInputStream を使用する場合、要求の生のストリームを取得し、全体の要求を処理するための責任を負うものです。 GetBufferlessInputStream を呼び出した後 Request.Form および Request.InputStream は使用できません、ASP.NET によって作成されてがいないためです。 GetBufferedInputStream を使用する場合は、要求からストリームのコピーを取得します。 Request.Form と Request.InputStream が使用可能な要求の後で ASP.NET がもう一方のコピーを作成します。

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect と Response.End

に関する推奨事項: スレッドの呼び出し後の処理方法の相違点に注意してください[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx)です。

[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx)メソッド Response.End メソッドを呼び出します。 同期プロセスで Request.Redirect を呼び出すと、すぐに中止する現在のスレッドとします。 ただし、非同期的な処理で Response.Redirect を呼び出すことは中止されない、現在のスレッドのため、要求のコードの実行が続行されます。 非同期的な処理で、コードの実行を停止するメソッドからタスクを返す必要があります。

MVC プロジェクトでは、呼び出す必要はありません Response.Redirect です。 代わりに、RedirectResult を返します。

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>エントリおよび ViewStateMode

ビュー ステートを使用するコントロールをきめ細かく制御のエントリではなく、推奨事項: 使用 ViewStateMode です。

エントリ ページ ディレクティブで false に設定すると、ビュー ステートがページ内のすべてのコントロールで無効にされ、有効にすることはできません。 ページ内の特定のコントロールのみにビュー ステートを有効にする場合は、ページの [無効] に ViewStateMode を設定します。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

次に、ViewStateMode が有効にビュー ステートを実際に必要なコントロールのみに設定します。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

必要なコントロールのみのビュー ステートを有効にするには、web ページのビュー ステートのサイズを縮小することができます。

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

推奨事項は、Universal Providers を使用します。

現在のプロジェクト テンプレートで SqlMembershipProvider は置き換えられました[ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)、これは、NuGet パッケージとして入手できます。 SqlMembershipProvider テンプレートの以前のバージョンでビルドされたプロジェクトを使用している場合は、Universal Providers に切り替える必要があります。 ユニバーサル プロバイダーは、Entity Framework でサポートされているすべてのデータベースで動作します。

詳細については、次を参照してください。 [ASP.NET Universal Providers を導入](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)です。

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>実行時間の長い要求 (> 110 秒)

推奨事項: を使用して[Websocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx)または[SignalR](../../../signalr/index.md)接続されているクライアント、および非同期の I/O 操作を使用します。

実行時間の長い要求は、web アプリケーションで予期しない結果とパフォーマンスの低下の原因になることができます。 要求の既定のタイムアウト設定は、110 (秒) です。 実行時間の長い要求でセッション状態を使用している場合、ASP.NET は 110 秒後にセッション オブジェクトのロックが解除されます。 ただし、アプリケーションがありますセッション オブジェクトに対する操作の途中で、ロックを解除し、操作を完了しない可能性。 ユーザーから 2 番目の要求がブロックされた場合、最初の要求の実行中に、2 番目の要求は不整合な状態でセッション オブジェクトにアクセス可能性があります。

アプリケーションには、ブロックしている (または同期) の I/O 操作が含まれている場合は、アプリケーションは応答できません。

パフォーマンスを向上させるには、.NET Framework の非同期 I/O 操作を使用します。 また、Websocket または SignalR を使用して、サーバーにクライアントを接続するためです。 これらの機能は、実行時間の長い要求を効率的に処理する設計されています。
