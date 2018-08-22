---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Iis の OWIN ミドルウェア パイプラインの統合 |Microsoft Docs
author: Praburaj
description: この記事では、IIS 統合パイプラインで OWIN ミドルウェア コンポーネント (OMCs) を実行する方法を示していて、上、OMC パイプライン イベントを設定する方法で実行します。 必要があります.
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 56bd145688e1ab0a70710cc80cb8f5fa10ee8968
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825971"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS 統合パイプラインの OWIN ミドルウェア
====================
によって[Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> この記事では、IIS 統合パイプラインで OWIN ミドルウェア コンポーネント (OMCs) を実行する方法を示していて、上、OMC パイプライン イベントを設定する方法で実行します。 確認する必要があります[プロジェクト Katana の概要を](an-overview-of-project-katana.md)と[OWIN スタートアップ クラス検出](owin-startup-class-detection.md)このチュートリアルを読む前にします。 このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Chris Ross、Praburaj Thiagarajan、Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。


[OWIN](an-overview-of-project-katana.md)ミドルウェア コンポーネント (OMCs) は、サーバーに依存しないパイプラインで実行する主な用途は、IIS 統合パイプラインも、OMC を実行することは (**クラシック モードが*いない*サポート**)。 パッケージ マネージャー コンソール (PMC) から、次のパッケージをインストールすることで、IIS 統合パイプラインで動作する、OMC ことができます。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

つまり、既存の OWIN ミドルウェア コンポーネントから IIS や System.Web、外部で実行することにはまだいる場合でも、すべてのアプリケーション フレームワークが役立ちます。 

> [!NOTE]
> すべての`Microsoft.Owin.Security.*`Visual Studio 2013 で新しい Id システムと配布のパッケージ (例: Cookie、Microsoft アカウント、Google、Facebook、Twitter、[ベアラー トークン](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)OAuth、承認サーバー、JWT では、Azure Activeディレクトリ、および Active directory フェデレーション サービス)、OMCs として作成されたおよびと IIS でホストされる自己ホスト型の両方のシナリオで使用できます。

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>IIS 統合パイプラインで OWIN ミドルウェアを実行する方法

コンソール アプリケーションの OWIN アプリケーション パイプライン ビルドを使用して、[スタートアップ構成](owin-startup-class-detection.md)を使用して、コンポーネントが追加された順序で設定されて、`IAppBuilder.Use`メソッド。 OWIN パイプラインでは、 [Katana](an-overview-of-project-katana.md)ランタイムを使用して登録された順番で OMCs の処理は`IAppBuilder.Use`します。 要求パイプラインは、IIS 統合パイプラインの[HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)などの事前定義された一連のパイプライン イベントをサブスクライブして[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)、 [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)、 [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)など。

OMC を比べると、 [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx)適切な定義済みのパイプライン イベントに登録する必要が、OMC ASP.NET の世界でします。 たとえば、HttpModule`MyModule`によって要求を受信ときに呼び出さ、 [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)パイプラインのステージ。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

OMC この同じ、イベント ベースの実行順序に参加するために、 [Katana](an-overview-of-project-katana.md)によってランタイム コードがスキャンされ、[スタートアップ構成](owin-startup-class-detection.md)各ミドルウェア コンポーネントの定期受信し、統合パイプラインのイベントです。 たとえば、次 OMC と登録コードを使用すると、ミドルウェア コンポーネントの既定のイベントの登録を参照してください。 (OWIN startup クラスを作成する手順について詳細を参照してください[OWIN スタートアップ クラス検出](owin-startup-class-detection.md)。)。

1. 空の web アプリケーション プロジェクトを作成し、名前**owin2**します。
2. パッケージ マネージャー コンソール (PMC) からは、次のコマンドを実行します。 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. 追加、`OWIN Startup Class`名前を付けます`Startup`します。 生成されたコードを (その変更が強調表示) を次に置き換えます。  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. アプリを実行するには、f5 キーを押します。

スタートアップ構成は、3 つのミドルウェア コンポーネント、最初の 2 つの診断情報と最後の 1 つのイベントに応答を表示する (および診断情報の表示も) を使用するパイプラインを設定します。 `PrintCurrentIntegratedPipelineStage`メソッドでこのミドルウェアが呼び出された統合パイプラインのイベントとメッセージを表示します。 出力ウィンドウには、次の情報が表示されます。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana ランタイムをマップする OWIN ミドルウェア コンポーネントの各[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) IIS パイプラインのイベントに対応する既定では、 [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)します。

## <a name="stage-markers"></a>ステージ マーカー

使用して、パイプラインの特定の段階で実行する OMCs をマークすることができます、`IAppBuilder UseStageMarker()`拡張メソッド。 特定のステージ中に一連のミドルウェア コンポーネントを実行するには、最後の要素の直後後のステージ マーカーの挿入の登録時に、セットです。 ミドルウェアを実行するパイプラインのステージで規則があるし、注文のコンポーネントを実行する必要があります (ルールは、チュートリアルの後半で説明します)。 追加、`UseStageMarker`メソッドを`Configuration`次に示すようにコードします。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)`呼び出しパイプラインの認証段階で実行する (この例では、2 つの診断コンポーネント) では、すべての以前に登録されたミドルウェア コンポーネントを構成します。 実行されます (が診断を表示され、要求に応答する) 最後のミドルウェア コンポーネント、`ResolveCache`ステージ (、 [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)イベント)。

アプリを実行するには、f5 キーを押します。次に、出力ウィンドウを示します。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>ステージ マーカーのルール

次の OWIN パイプラインのステージ イベントで実行する Owin ミドルウェア コンポーネント (OMC) を構成できます。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. 既定では、OMCs は最後のイベントで実行 (`PreHandlerExecute`)。 その理由は、最初のコード例には"PreExecuteRequestHandler"が表示されます。
2. 使用することができます、`app.UseStageMarker`で OWIN パイプラインのいずれかの段階で以前では、実行の OMC を登録するメソッドが一覧表示、`PipelineStage`列挙型。
3. OWIN パイプラインと、IIS パイプラインが順序付け、そのための呼び出しを`app.UseStageMarker`の順序である必要があります。 登録されている最後のイベントの前のイベントにイベント ハンドラーを設定することはできません`app.UseStageMarker`します。 たとえば、*後*呼び出し。

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   呼び出す`app.UseStageMarker`渡す`Authenticate`または`PostAuthenticate`が受け入れしないと、例外はスローされません。 既定では、最新の段階で実行 OMCs`PreHandlerExecute`します。 ステージ マーカーを使用して、先に実行できるようにします。 誤順序のステージ マーカーを指定する場合は、以前のマーカーに丸められます。 つまり、ステージ マーカーを追加するという「ステージ X 以降実行」します。 OWIN パイプラインの後に追加された最初のステージ マーカー OMC の実行。
4. 呼び出しの最も早いステージ`app.UseStageMarker`wins します。 たとえばの順序を切り替えること`app.UseStageMarker`前の例からの呼び出し。

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   出力ウィンドウが表示されます。 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs すべて内で実行、`AuthenticateRequest`最後 OMC に登録されているため、ステージング、`Authenticate`イベント、および`Authenticate`イベントの前に他のすべてのイベント。
