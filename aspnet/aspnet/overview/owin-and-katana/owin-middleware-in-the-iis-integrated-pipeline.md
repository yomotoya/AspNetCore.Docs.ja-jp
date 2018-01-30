---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: "OWIN ミドルウェア iis 統合パイプライン |Microsoft ドキュメント"
author: Praburaj
description: "ここでは、IIS 統合パイプラインの OWIN ミドルウェア コンポーネント (OMCs) を実行する方法を説明し、OMC パイプライン イベントを設定する方法が実行します。 行う必要があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5f6ed1ae0309e9bdd3ca4ae229195835f20bc729
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS 統合パイプラインの OWIN ミドルウェア
====================
によって[Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> ここでは、IIS 統合パイプラインの OWIN ミドルウェア コンポーネント (OMCs) を実行する方法を説明し、OMC パイプライン イベントを設定する方法が実行します。 確認する必要があります[プロジェクト Katana の概要を](an-overview-of-project-katana.md)と[OWIN スタートアップ クラス検出](owin-startup-class-detection.md)このチュートリアルを読む前にします。 このチュートリアルは、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Chris Ross、Praburaj Thiagarajan および Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。


[OWIN](an-overview-of-project-katana.md)ミドルウェア コンポーネント (OMCs) がサーバーにもとらわれないパイプラインで実行する主な用途は、IIS 統合パイプラインも、OMC を実行することは (**クラシック モードが*いない*サポート**)。 パッケージ マネージャー コンソール (PMC) から、次のパッケージをインストールすることで、IIS 統合パイプラインで動作する、OMC を変更できます。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

これはまだ IIS と、System.Web の外部で実行できるものも、すべてのアプリケーション フレームワークが既存の OWIN ミドルウェア コンポーネントを活用できることを意味します。 

> [!NOTE]
> すべての`Microsoft.Owin.Security.*`Visual Studio 2013 で新しい Id システムと配布のパッケージ (例: Cookie、Microsoft アカウント、Google、Facebook、Twitter、[ベアラー トークン](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)OAuth、承認サーバー、JWT、Azure Activeディレクトリ、および Active directory フェデレーション サービス) OMCs、として作成されたおよびと IIS でホストされる自己ホスト型の両方のシナリオで使用できます。

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>IIS 統合パイプラインの OWIN ミドルウェアを実行する方法

、コンソール アプリケーションの OWIN アプリケーション パイプライン ビルドを使用して、[起動構成](owin-startup-class-detection.md)を使用して、コンポーネントが追加された順序によって設定されている、`IAppBuilder.Use`メソッドです。 OWIN パイプライン、 [Katana](an-overview-of-project-katana.md)ランタイムを使用して、登録済みの順序で OMCs の処理は`IAppBuilder.Use`します。 要求パイプラインは、IIS 統合パイプラインの[HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)など、定義済みの一連のパイプライン イベントにサブスクライブしている[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)、 [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)、 [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), などです。

比較、OMC の場合、 [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx)世界では、ASP.NET、OMC を正しい定義済みのパイプラインのイベントに登録する必要があります。 HttpModule など`MyModule`要求を受信ときに呼び出されるが、 [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)パイプラインのステージで。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

OMC この、イベント ベースの同時実行の順序に参加するために、 [Katana](an-overview-of-project-katana.md)ランタイム コードのスキャン、[起動構成](owin-startup-class-detection.md)のミドルウェア コンポーネントごとに定期受信し、統合パイプラインのイベントです。 たとえば、OMC と登録の次のコードを使用すると、ミドルウェア コンポーネントの既定のイベント登録を確認できます。 (詳細については、OWIN スタートアップ クラスを作成する手順についてを参照してください[OWIN スタートアップ クラス検出](owin-startup-class-detection.md))。

1. 空の web アプリケーション プロジェクトを作成し、名前を付けます**owin2**です。
2. パッケージ マネージャー コンソール (PMC) からは、次のコマンドを実行します。 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. 追加、`OWIN Startup Class`し名前を付けます`Startup`です。 生成されたコードを (変更が強調表示されます)、次に置き換えます。  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. F5 キーを押して、アプリを実行します。

スタートアップ構成は、次の 3 つのミドルウェア コンポーネントと、最初の 2 つの診断情報とイベントに応答する最後の 1 つの表示 (も診断情報の表示) パイプラインを設定します。 `PrintCurrentIntegratedPipelineStage`メソッドは、このミドルウェアはで呼び出される統合パイプラインのイベントとメッセージが表示されます。 出力ウィンドウには、次の情報が表示されます。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana ランタイムをマップする OWIN ミドルウェア コンポーネントの各[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) IIS パイプライン イベントに対応する既定では、 [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)です。

## <a name="stage-markers"></a>ステージ マーカー

使用して、パイプラインの特定の段階で実行する OMCs をマークすることができます、`IAppBuilder UseStageMarker()`拡張メソッド。 最後の要素の直後にステージ マーカーを挿入する特定のステージ中にミドルウェア コンポーネントのセットを実行するには、セットを登録中にします。 ミドルウェアを実行するパイプラインのどのステージで規則があるし、順序は、コンポーネントを実行する必要があります (ルールは、チュートリアルの後半で説明します)。 追加、`UseStageMarker`メソッドを`Configuration`コードの次のようにします。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)`呼び出しパイプラインの認証段階で実行する (ここでは、2 つの診断コンポーネント) のすべての以前に登録したミドルウェア コンポーネントを構成します。 実行されます (これ診断を表示し、要求に応答) の最後のミドルウェア コンポーネント、`ResolveCache`ステージ (、 [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)イベント)。

F5 キーを押して、アプリを実行します。次は、出力ウィンドウを示しています。

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>ステージ マーカーのルール

次の OWIN パイプラインのステージ イベントに実行する Owin ミドルウェア コンポーネント (OMC) を構成できます。

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. 既定では、OMCs は最後のイベントで実行 (`PreHandlerExecute`)。 その理由は、最初のコード例には"PreExecuteRequestHandler"が表示されます。
2. 使用することができます、`app.UseStageMarker`に OWIN パイプラインのいずれかの段階で以前に実行する OMC を登録する方法が示されている、`PipelineStage`列挙型。
3. OWIN パイプラインと IIS パイプラインが順序付け、そのために呼び出し`app.UseStageMarker`の順序である必要があります。 登録されている最後のイベントの前に、イベントにイベント ハンドラーを設定することはできません`app.UseStageMarker`です。 たとえば、*後*呼び出し。

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

 呼び出す`app.UseStageMarker`渡す`Authenticate`または`PostAuthenticate`受け入れられ、できず、例外はスローされません。 既定では、最新の段階で実行 OMCs`PreHandlerExecute`です。 以前を実行するようにするステージ マーカーが使用されます。 誤順序のステージ マーカーを指定する場合は、以前のマーカーにお丸められます。 つまり、ステージ マーカーを追加することを通知"Run ステージ X 以降"。 OWIN パイプラインの後に追加された最初のステージ マーカーで OMC の実行。
4. 呼び出しの最も早いステージ`app.UseStageMarker`wins です。 たとえばの順序を切り替えた場合`app.UseStageMarker`前の例からの呼び出し。

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

 出力ウィンドウに表示されます。 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

 OMCs で動作するように、`AuthenticateRequest`最後 OMC に登録されているため、ステージ、`Authenticate`イベント、および`Authenticate`イベントの前に他のすべてのイベントです。
