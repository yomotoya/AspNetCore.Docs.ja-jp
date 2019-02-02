---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET では、しないとは |Microsoft Docs
author: Rick-Anderson
description: このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。 これらの共通を回避するために行う必要がありますの推奨事項を提供しています.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667714"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>ASP.NET では行わないことと、その代わりに行うこと

> このトピックでは、ASP.NET web プロジェクト内でユーザーが、いくつかの一般的なミスをについて説明します。 これらの一般的な誤りを回避するために行う必要がありますの推奨事項を提供します。 基にして、[プレゼンテーション](http://vimeo.com/68390507)によって**Damian Edwards** Norwegian Developers Conference でします。


## <a name="disclaimer"></a>免責情報

このトピックではありませんの完全なガイドとして、アプリケーションが安全かつ効率的なことを確認します。 このトピックに記載されていないいるセキュリティとパフォーマンスのベスト プラクティスに従う必要があります。 .NET クラスとプロセスに関連する一般的なミスを回避する方法のみお勧めします。

## <a name="overview"></a>概要

このトピックは、次のセクションで構成されています。

- [標準への準拠](#standards)

    - [コントロール アダプター](#adapters)
    - [コントロールのスタイル プロパティ](#styleprop)
    - [ページおよびコントロールのコールバック](#callback)
    - [ブラウザー機能の検出](#browsercap)
- [セキュリティ](#security)

    - [要求の検証](#validation)
    - [Cookieless フォーム認証とセッション](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [中程度の信頼](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [信頼性とパフォーマンス](#performance)

    - [PreSendRequestHeaders と PreSendRequestContent](#presend)
    - [Web フォームでの非同期ページ イベント](#asyncevents)
    - [ファイア アンド フォーゲット作業](#fire)
    - [要求エンティティ本体](#requestentity)
    - [Response.Redirect と Response.End](#redirect)
    - [EnableViewState およびに ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [長い実行されている要求 (> 110 秒)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>標準への準拠

<a id="adapters"></a>

### <a name="control-adapters"></a>コントロール アダプター

推奨事項:アダプティブ レンダリングは、コントロール アダプターの使用を停止し、代わりに CSS メディア クエリと標準に準拠した HTML を使用します。

コントロール アダプターは、さまざまなデバイスや環境向けにカスタマイズされたプレゼンテーション コードを表示するために .NET 2.0 で導入されました。 ここで、CSS、HTML によるこのアダプティブ レンダリングを実現できます。 コントロール アダプターの使用を停止して、CSS、HTML、既存のアダプターを変換する必要があります。

詳細については、次を参照してください[メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)と[How To:。モバイル ページを追加、ASP.NET Web フォーム/MVC アプリケーション](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)します。

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>コントロールのスタイル プロパティ

推奨事項:コントロールのマークアップにスタイル値の設定を停止し、代わりに CSS スタイル シートで書式設定の値を設定します。

Web サーバー コントロールには、多数行のスタイル プロパティを設定するために使用できるプロパティにはが含まれます。 たとえば、前景色プロパティは、コントロールのテキストの色を設定します。 CSS スタイル シートをより効率的にこれと同じ効果を行うことができます。 スタイル シートを使用すると、スタイルの値を一元化し、アプリケーション全体でこれらの値を設定しないでください。

次の例は、CSS クラスを赤にセットのテキストを示します。

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

次の例では、CSS クラスを動的に適用する方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>ページおよびコントロールのコールバック

推奨事項:ページおよびコントロールのコールバックの使用を停止し、代わりに、次のいずれかを使用します。AJAX、UpdatePanel、MVC アクション メソッド、Web API、または SignalR。

以前のバージョンの ASP.NET では、ページおよびコントロールのコールバック メソッドには、ページ全体を更新することがなく、web ページの一部を更新することが有効になります。 部分ページ更新を行うことができますようになりました[AJAX](../../../ajax/index.md)、 [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx)、 [MVC](../../../mvc/index.md)、 [Web API](../../../web-api/index.md)または[SignalR](../../../signalr/index.md). フレンドリな Url で問題が生じるため、コールバック メソッドを使用して、ルーティングを停止する必要があります。 既定では、コントロールがコールバック メソッドでは、有効にしないが、コントロールでは、この機能を有効にした場合無効する必要があります。

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>ブラウザー機能の検出

推奨事項:静的ブラウザー機能の検出の使用を停止し、代わりに動的機能検出を使用します。

以前のバージョンの ASP.NET では、各ブラウザーでサポートされている機能は、XML ファイルに保存されていました。 静的な参照を通じて検出機能のサポートは、最適な方法ではありません。 ここで、動的に検出できますなどの機能検出フレームワークを使用して、ブラウザーのサポート機能[Modernizr](http://modernizr.com/)します。 機能検出は、メソッドまたはプロパティを使用しようとして、ブラウザーで、目的の結果が生成されるかどうかはどうかを確認し、サポートを判断します。 既定では、Modernizr は、Web アプリケーション テンプレートに含まれます。

<a id="security"></a>

## <a name="security"></a>セキュリティ

<a id="validation"></a>

### <a name="request-validation"></a>要求の検証

推奨事項:ユーザーの入力を検証し、ユーザーからの出力をエンコードします。

要求の検証は、各要求を検査し、見かけ上の脅威が見つかった場合は、要求を停止します。 ASP.NET の機能です。 クロスサイト スクリプティング攻撃に対してアプリケーションを保護するための要求の検証に依存しません。 代わりに、ユーザーからのすべての入力を検証し、出力をエンコードします。 まれに、正規表現を使用して、入力を検証することができますが、値と一致する場合を決定する .NET クラスを使用してユーザー入力を検証する必要がありますより複雑な場合に、値が許可されています。

次の例では、Uri クラスの静的メソッドを使用して、ユーザーによって提供された Uri が有効かどうかを判断する方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

ただし、Uri を十分に確認する必要がありますもチェックを指定するかどうかを確認する`http`または`https`します。 次の例では、インスタンス メソッドを使用して、Uri が有効であることを確認します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

ユーザー入力を HTML としてレンダリングまたはユーザーの入力を含む SQL クエリで、前に、悪意のあるコードが含まれていないことを確認する値をエンコードします。

HTML のできますマークアップで値をエンコード、 &lt;%: %&gt;構文を次に示すよう。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Razor 構文では、HTML ことができます。 または、を使用したエンコード @ 以下に示すようにします。

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

例を次に示しますを HTML エンコード方法、分離コードでの値。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

SQL コマンドの値を安全にエンコードする場合などのコマンド パラメーターを使用して、 [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)します。 <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Cookieless フォーム認証とセッション

推奨事項:Cookie が必要です。

クエリ文字列での認証情報を渡すことは安全ではありません。 アプリケーションに認証が含まれているため、cookie が必要です。 Cookie は、機密情報を保存する場合は、クッキーに対して SSL を要求することを検討してください。

次の例では、フォーム認証が SSL 経由で送信される cookie が必要である、Web.config ファイルで指定する方法を示します。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

推奨事項:False に設定しないでください。

既定で EnbableViewStateMac が設定を true にします。 アプリケーションがビュー ステートを使用していない場合でも EnableViewStateMac を false に設定しないでください。 この値を false に設定すると、アプリケーションがクロスサイト スクリプティングに対する脆弱性。

ASP.NET 4.5.2 以降では、ランタイムが適用**EnableViewStateMac は true を =** します。 False に設定すると、場合でも、ランタイムはこの値は無視され、true に設定された値が続行されます。 詳細については、次を参照してください。 [ASP.NET 4.5.2 および EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)します。

次の例では、EnableViewStateMac を true に設定する方法を示します。 実際には既定で true が true には、この値を設定する必要はありません。 ただし、設定した場合が false に任意のページで、アプリケーションで、この値をすぐに修正する必要があります。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>中程度の信頼

推奨事項:セキュリティ境界として、中程度の信頼 (または他の信頼レベル) に依存しない操作を行います。

部分信頼では、アプリケーションを適切に保護できないは使用できません。 代わりに、完全な信頼を使用し、個別のアプリケーション プールで信頼されていないアプリケーションを分離します。 また、一意の id では、各アプリケーション プールを実行します。 詳細については、次を参照してください。 [ASP.NET 部分信頼でアプリケーションの分離が保証されない](https://support.microsoft.com/kb/2698981)します。

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

推奨事項:セキュリティ設定を無効にしない&lt;appSettings&gt;要素。

AppSettings 要素には、セキュリティ更新プログラムの必要な多くの値が含まれています。 変更または、これらの値を無効にする必要がありますされません。 場合は、更新プログラムを展開するときに、これらの値を無効にする必要があります、すぐに再有効化展開が完了後します。

詳細については、次を参照してください。 [ASP.NET appSettings 要素](https://msdn.microsoft.com/library/hh975440.aspx)します。

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

推奨事項:使用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)代わりにします。

UrlPathEncode メソッドは、非常に特定のブラウザーの互換性の問題を解決するのには、.NET Framework に追加されました。 URL を適切にエンコードしないと、クロス サイト スクリプティングからアプリケーションを保護しません。 アプリケーションで使用する必要がありますしないでください。 代わりに、 [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)します。

次の例では、ハイパーリンク コントロール用のクエリ文字列パラメーターとしてエンコードされた URL を渡す方法を示します。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>信頼性とパフォーマンス

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders と PreSendRequestContent

推奨事項:マネージ モジュールでは、これらのイベントを使用しません。 代わりに、必要なタスクを実行するネイティブ IIS モジュールを記述します。 参照してください[ネイティブ コード HTTP モジュールを作成して](https://msdn.microsoft.com/library/ms693629.aspx)します。

使用することができます、 [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx)と[PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) IIS のネイティブ モジュールでのイベント。
> [!WARNING]
> 使用しない`PreSendRequestHeaders`と`PreSendRequestContent`を実装するマネージ モジュールで`IHttpModule`します。 これらのプロパティを設定すると、非同期要求で問題が発生することができます。 アプリケーション要求ルーティング処理 (ARR) と websocket の組み合わせは、w3wp クラッシュを引き起こす可能性のあるアクセス違反例外に生じる可能性があります。 たとえば、iiscore!W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll で 68 では、アクセス違反例外 (0xC0000005) が原因です。

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Web フォームでの非同期ページ イベント

推奨事項:Web フォームで非同期ページのライフ サイクル イベントの void メソッドの書き込みを避けるため、代わりに使用[Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx)非同期コードの。

ページ イベントをマークすると**async**と**void**、非同期コードの実行が完了するとわかったことはできません。 代わりに、Page.RegisterAsyncTask を使用して、その完了を追跡することができる方法で非同期コードを実行します。

ボタンは、次の例では、非同期コードを含むハンドラーをクリックします。 この例では、推奨されるプラクティスではなく、非同期タスクの簡単な例としてのみ指定される、非同期的に文字列値の読み取り。

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

非同期タスクを使用している場合は、4.5 (またはそれ以降) に Http ランタイムのターゲット フレームワークを設定、Web.config ファイルでします。 新しい同期コンテキストで 4.5 ターンにターゲット フレームワークを設定すると、.NET 4.5 で追加されました。 この値は、Visual Studio で新しいプロジェクトで既定で設定されますが、設定になっていない既存のプロジェクトを使用している場合は。

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>ファイア アンド フォーゲット操作します。

推奨事項:ASP.NET 内の要求を処理する際は、ファイア アンド フォーゲット作業 (このような ThreadPool.QueueUserWorkItem メソッドの呼び出しまたはデリゲートを繰り返し呼び出すタイマーを作成する) を起動しないようにします。

ファイア アンド フォーゲット ASP.NET 内で実行される作業アプリケーションの場合は、アプリケーションは同期で取得できます。いつでもでも、アプリ ドメインがあり、継続的なプロセスが、アプリケーションの現在の状態と一致して不要になったを破壊することができます。

この種の ASP.NET の外部で作業を移動する必要があります。 進行中の作業を実行する Azure で Web ジョブ、Windows サービスまたはワーカー ロールを使用し、別のプロセスからそのコードを実行できます。

という名前の Nuget パッケージを追加することができる場合、ASP.NET 内でこの作業を実行する必要があります[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)コードを実行します。

<a id="requestentity"></a>

### <a name="request-entity-body"></a>要求エンティティ本体

推奨事項:ハンドラーのイベントを実行する前に、Request.Form または Request.InputStream の読み取りを回避します。

最も早い Request.Form または Request.InputStream から読み取る必要がありますが、ハンドラーの中にイベントを実行します。 MVC では、コント ローラーは、ハンドラーと、実行イベントは、アクション メソッドの実行時にします。 Web フォームで、ページは、ハンドラーと、execute イベント Page.Init イベントが発生します。 要求エンティティ本体を execute イベントより前読み取る場合は、要求の処理干渉します。

Execute イベントの前に、要求エンティティ本体を読み取る場合は、いずれかを使用して、 [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx)または[Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx)します。 GetBufferlessInputStream を使用すると、要求から生のストリームを取得し、要求全体を処理するための責任を負うものです。 呼び出し元 GetBufferlessInputStream、Request.Form および Request.InputStream は ASP.NET によって作成されてがいないために、使用できません後です。 GetBufferedInputStream を使用する場合は、要求からストリームのコピーを取得します。 Request.Form と Request.InputStream が使用可能な要求の後で ASP.NET の他のコピーを設定します。

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect と Response.End

推奨事項:スレッドを呼び出した後に処理する方法の相違点に注意してください[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)します。

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)メソッド Response.End メソッドを呼び出します。 同期プロセスでは、Request.Redirect を呼び出すとが、現在のスレッドがすぐに中止します。 ただし、非同期処理で Response.Redirect を呼び出すことは中止されない、現在のスレッドの要求は引き続きコードが実行されるようにします。 非同期の処理では、コードの実行を停止するメソッドからタスクを返す必要があります。

MVC プロジェクトでは、Response.Redirect を呼び出さないでください。 代わりに、RedirectResult を返します。

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState およびに ViewStateMode

推奨事項:EnableViewState、代わりに ViewStateMode を使用すると、コントロールがビュー ステートを使用する詳細な制御を提供します。

EnableViewState ページ ディレクティブで false に設定すると、ビュー ステートは、ページ内のすべてのコントロールで無効にされ、有効にすることはできません。 ページ内の特定のコントロールのみにビュー ステートを有効にする場合に ViewStateMode を無効にページの設定します。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

次に、ビュー ステートを実際に必要なコントロールのみで有効に ViewStateMode を設定します。

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

ビュー ステートが必要なコントロールのみを有効にすると、web ページのビュー ステートのサイズを縮小できます。

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

推奨事項:ユニバーサル プロバイダーを使用します。

現在のプロジェクト テンプレートで SqlMembershipProvider 代わられました[ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)、NuGet パッケージとして利用できます。 SqlMembershipProvider テンプレートの以前のバージョンでビルドされたプロジェクトを使用している場合は、ユニバーサル プロバイダーに切り替える必要があります。 Universal Providers Entity Framework でサポートされているすべてのデータベースを使用します。

詳細については、次を参照してください。 [ASP.NET ユニバーサル プロバイダーの導入](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)します。

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>実行時間の長い要求 (> 110 秒)

推奨事項:使用して、 [Websocket](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx)または[SignalR](../../../signalr/index.md)接続されているクライアントは、および非同期の I/O 操作を使用します。

実行時間の長い要求は、web アプリケーションで予期しない結果とパフォーマンスの低下の原因になることができます。 要求の既定のタイムアウト設定は、110 秒です。 実行時間の長い要求でセッション状態を使用する場合、ASP.NET は 110 秒後にセッション オブジェクトのロックが解除されます。 ただし、アプリケーションがありますセッション オブジェクトに対する操作の途中で、ロックがリリースされ、操作が正常に完了しない可能性。 最初の要求の実行中に、ユーザーからの 2 番目の要求がブロックされている場合、2 番目の要求は不整合な状態でセッション オブジェクトにアクセス可能性があります。

アプリケーションには、I/O 操作ブロックしている (または同期) にはが含まれている場合は、アプリケーションは応答できません。

パフォーマンスを向上させるのには、.NET Framework の非同期 I/O 操作を使用します。 また、サーバーにクライアントを接続するための Websocket や SignalR を使用します。 これらの機能は、実行時間の長い要求を効率的に処理するために設計されています。
