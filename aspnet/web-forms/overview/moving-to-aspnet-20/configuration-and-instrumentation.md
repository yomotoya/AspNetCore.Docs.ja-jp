---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: "構成およびインストルメンテーション |Microsoft ドキュメント"
author: microsoft
description: "構成の主な変更と ASP.NET 2.0 の実装があります。 構成の変更を加える pr 新規の ASP.NET 構成 API を使用する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: f8d378d3332669ae4606dad8ada06de37e7dfd20
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuration-and-instrumentation"></a>構成およびインストルメンテーション
====================
によって[Microsoft](https://github.com/microsoft)

> 構成の主な変更と ASP.NET 2.0 の実装があります。 構成の変更をプログラムで実行できる新しい ASP.NET 構成 API を使用します。 さらに、多くの新しい構成設定が存在して新しい構成およびインストルメンテーションを許可します。


構成の主な変更と ASP.NET 2.0 の実装があります。 構成の変更をプログラムで実行できる新しい ASP.NET 構成 API を使用します。 さらに、多くの新しい構成設定が存在して新しい構成およびインストルメンテーションを許可します。

このモジュールでは読み取りおよび ASP.NET 構成ファイルへの書き込みに関連すると、ASP.NET インストルメンテーションについても説明 ASP.NET 構成 API を説明します。 ASP.NET トレースで使用できる新しい機能についても説明します。

## <a name="aspnet-configuration-api"></a>ASP.NET 構成 API

ASP.NET 構成 API を使用すると、開発、配置、および 1 つのプログラミング インターフェイスを使用してアプリケーションの構成データを管理できます。 構成 API を使用して、開発および ASP.NET 構成の完了をプログラムによって構成ファイル内の XML を直接編集せずに変更することができます。 さらに、API、Web ベースの管理ツール、および Microsoft 管理コンソール (MMC) スナップイン、コンソール アプリケーションと開発したスクリプトでの構成を使用することができます。

次の 2 つの構成管理ツールは、構成 API を使用し、.NET Framework version 2.0 に含まれています。

- ASP.NET MMC スナップインで、構成 API を使用して、管理タスクを簡略化、構成階層のすべてのレベルからローカルの構成データを統合ビューを提供します。
- ローカルおよびリモート アプリケーションの構成設定を管理できますが、Web サイト管理ツールは、ホストされているサイトを含むです。

ASP.NET 構成 API には、Web サイトおよびアプリケーションをプログラムで構成に使用できる ASP.NET 管理オブジェクトのセットが構成されています。 管理オブジェクトは、.NET Framework クラス ライブラリとして実装されます。 構成 API のプログラミング モデルにより、コンパイル時にデータ型を適用することでコードの整合性と信頼性を確認してください。 アプリケーション構成の管理を容易にできるように、構成 API を使用すると、異なる個別のコレクションとしてデータを表示するのではなく、1 つのコレクションとして、構成階層のすべてのポイントから結合されたデータを表示構成ファイル。 さらに、API の構成には、構成ファイル内の XML を直接編集せずにアプリケーション全体の構成を操作することができます。 最後に、API は、Web サイト管理ツールなどの管理ツールをサポートすることによって構成タスクを簡略化します。 構成 API は、コンピューター上の構成ファイルの作成をサポートし、複数のコンピューターの構成スクリプトを実行している展開を簡略化します。

> [!NOTE]
> 構成 API は、IIS アプリケーションの作成をサポートしていません。


## <a name="working-with-local-and-remote-configuration-settings"></a>ローカルとリモートの構成設定の操作

構成オブジェクトは、コンピューターなどの特定の物理エンティティ、または、アプリケーションまたは Web サイトなどの論理エンティティに適用される構成設定のマージされたビューを表します。 指定された論理エンティティは、ローカル コンピューターまたはリモート サーバー上に存在します。 特定のエンティティに対する構成ファイルが存在しない場合、構成オブジェクトは、Machine.config ファイルで定義されている既定の構成設定を表します。

構成オブジェクトを取得するには、次のクラスからのオープン構成方法のいずれかを使用します。

1. エンティティがクライアント アプリケーションの場合の ConfigurationManager クラスです。
2. エンティティが Web アプリケーションの場合の WebConfigurationManager クラスです。

これらのメソッドでは、さらに必要なメソッドとプロパティを基になる構成ファイルの処理を提供する、構成オブジェクトを返します。 これらのファイルの読み取りまたは書き込みにアクセスできます。

### <a name="reading"></a>読み取り

構成情報を読み取るには、GetSection または GetSectionGroup メソッドを使用します。 ユーザーまたはプロセスを読み取る必要がありますに対する読み取り権限のすべての階層の構成ファイル。

> [!NOTE]
> パスのパラメーターを受け取る静的 GetSection メソッドを使用する場合、path パラメーターは、コードが実行されているアプリケーションに参照しなければなりません。 それ以外の場合、パラメーターは無視され、現在実行中のアプリケーションの構成情報が返されます。


### <a name="writing"></a>書き込み

保存方法のいずれかを使用して、構成情報を書き込むことです。 ユーザーまたはプロセスの書き込みに必要な構成ファイルと現在の構成階層のレベルに存在するディレクトリに対する書き込みアクセス許可だけでなく、階層内の構成ファイルのすべての読み取りアクセス許可。

指定されたエンティティの継承された構成設定を表す構成ファイルを生成するには、次の構成の保存方法のいずれかの手順に従います。

1. 新しい構成ファイルを作成する Save メソッドです。
2. 別の場所で新しい構成ファイルを生成する SaveAs メソッドです。

## <a name="configuration-classes-and-namespaces"></a>構成のクラスと名前空間

多くの構成クラスとメソッドは、互いに似ています。 次の表は、最もよく使用される構成のクラスと名前空間について説明します。

| **構成クラスまたは名前空間** | **説明** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/en-us/library/system.configuration.aspx)名前空間 | すべての .NET Framework アプリケーションの主要な構成クラスを含みます。 セクション ハンドラー クラスを使用して、GetSection GetSectionGroup などのメソッドからセクションに対する構成データを取得できます。 これら 2 つのメソッドは、非静的です。 |
| System.Configuration.Configuration クラス | 構成データ、コンピューター、アプリケーション、Web ディレクトリ、またはその他のリソースのセットを表します。 このクラスには、構成設定を更新し、セクションおよびセクション グループへの参照を取得するために役立つメソッド、GetSection GetSectionGroup などが含まれています。 このクラスは、WebConfigurationManager および ConfigurationManager クラスのメソッドなど、デザイン時構成データを取得するメソッドの戻り値の型として使用します。 |
| System.Web.Configuration 名前空間 | 定義されている ASP.NET の構成セクションのセクション ハンドラー クラスを含む[ASP.NET の構成設定](https://msdn.microsoft.com/en-us/library/b5ysx397.aspx)です。 セクション ハンドラー クラスを使用して、GetSection GetSectionGroup などのメソッドからセクションに対する構成データを取得できます。 |
| System.Web.Configuration.WebConfigurationManager クラス | 実行時およびデザイン時の構成の設定への参照を取得するための便利なメソッドを提供します。 これらのメソッドは、System.Configuration.Configuration クラスを戻り値の型として使用します。 このクラスの静的 GetSection メソッドまたは System.Configuration.ConfigurationManager クラスの静的でない GetSection メソッドは、同じ意味で使用できます。 Web アプリケーションの構成、System.Web.Configuration.WebConfigurationManager クラスは System.Configuration.ConfigurationManager クラスの代わりにお勧めします。 |
| [System.Configuration.Provider](https://msdn.microsoft.com/en-us/library/system.configuration.provider.aspx)名前空間 | カスタマイズおよび構成プロバイダーを拡張する方法を提供します。 これは、構成システムですべてのプロバイダー クラスの基底クラスです。 |
| [System.Web.Management](https://msdn.microsoft.com/en-us/library/system.web.management.aspx)名前空間 | クラスとインターフェイスを管理および Web アプリケーションの正常性の監視が含まれています。 厳密には、この名前空間には、API の構成の一部はありません。 たとえば、トレースとイベントの発生は、この名前空間のクラスで実現されます。 |
| [System.Management.Instrumentation](https://msdn.microsoft.com/en-us/library/system.management.instrumentation.aspx)名前空間 | 管理情報と潜在的ユーザーにイベントを通じて Windows Management Instrumentation (WMI) を公開するアプリケーションのインストルメンテーションに必要なクラスを提供します。 ASP.NET の状態監視は、WMI を使用して、イベントを配信します。 厳密には、この名前空間には、API の構成の一部はありません。 |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET 構成ファイルからの読み取り

WebConfigurationManager クラスは、ASP.NET 構成ファイルからの読み取りに中核となるクラスです。 ASP.NET 構成ファイルの読み取りに基本的に 3 つの手順があります。

1. OpenWebConfiguration メソッドを使用して、構成オブジェクトを取得します。
2. 構成ファイルに必要なセクションへの参照を取得します。
3. 構成ファイルから必要な情報を読み取る。

オブジェクトを表す構成は、特定の構成ファイルを表していません。 代わりに、コンピューター、アプリケーション、または Web サイトの構成のマージされたビューを表します。 次のコード例と呼ばれる Web アプリケーションの構成を表す構成オブジェクトをインスタンス化*ProductInfo*です。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> /ProductInfo パスが存在しない場合、上記のコードが返される指定された既定の構成、machine.config ファイルに注意してください。


構成オブジェクトを作成したら、構成設定にドリルし GetSection または GetSectionGroup メソッドを使用できます。 次の例では、上記の ProductInfo アプリケーションの権限借用設定への参照を取得します。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>ASP.NET 構成ファイルへの書き込み

構成ファイルからの読み取り、WebConfigurationManager クラスは Asp.NET 構成ファイルに書き込むためのコアです。 ASP.NET 構成ファイルへの書き込みに 3 つの手順もあります。

1. OpenWebConfiguration メソッドを使用して、構成オブジェクトを取得します。
2. 構成ファイルに必要なセクションへの参照を取得します。
3. 名前を付けて保存または保存を使用して構成ファイルから必要な情報を書き込む方法です。

次のコードの変更、**デバッグ**の属性、&lt;コンパイル&gt;要素を false にします。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

このコードを実行すると、**デバッグ**の属性、&lt;コンパイル&gt;要素を false に設定されます、 *webApp*アプリケーションの web.config ファイルです。

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management 名前空間は、クラスとインターフェイスを管理および ASP.NET アプリケーションの正常性の監視を提供します。

ログ記録を実現するとを関連付けるイベント プロバイダー ルールを定義します。 このルールでは、プロバイダーに送信されるイベントの種類を定義します。 次の基本イベントは、ログオンするために使用できます。

| **WebBaseEvent** | すべてのイベントの基本イベント クラス。 含む、必要なイベント コード、イベント詳細コード、日付と時刻のイベントが発生したなどのすべてのイベントのプロパティ数、イベント メッセージ、およびイベントの詳細のシーケンスします。 |
| --- | --- |
| **WebManagementEvent** | アプリケーションの有効期間、要求、error、およびイベントの監査などの管理イベントの基本イベント クラス。 |
| **WebHeartbeatEvent** | 便利ランタイム状態情報をキャプチャする一定の間隔でアプリケーションによって生成されたイベントです。 |
| **WebAuditEvent** | 承認エラー、復号化失敗などの条件を示すために使用されるセキュリティ監査イベントの基本クラス*などです。* |
| **WebRequestEvent** | すべての情報要求イベントの基本クラス。 |
| **WebBaseErrorEvent** | エラー状態を示すイベントがすべての基底クラス。 |

使用可能なプロバイダーの種類では、イベントの出力イベント ビューアー、SQL Server、Windows Management Instrumentation (WMI)、および電子メールを送信できます。 事前構成済みのプロバイダーとイベントのマッピングは、ログに記録するイベント出力を取得するために必要な作業量を削減します。

ASP.NET 2.0 では、アプリケーション ドメインの開始し停止、だけでなく、未処理の例外をログに基づくイベントを記録するのに、イベント ログ プロバイダーの -標準を使用します。 これにより、基本的なシナリオをいくつか説明します。 、みましょうたとえばこと、アプリケーション、例外がスローが、ユーザーが、エラーを保存しないし、再現することはできません。 、既定のイベント ログ ルールが、どのような種類のエラーが発生しましたの理解を深める例外とスタックの情報を収集することになります。 別の例では、アプリケーションはセッション状態が失われる場合は適用されます。 その場合は、アプリケーション ドメインをリサイクルすると、かどうかと、アプリケーション ドメインが、まず第一に停止した理由を判断する、イベント ログで確認できます。

また、状態監視システムは拡張できます。 たとえば、カスタム Web イベントを定義、アプリケーション内で発生、およびイベント情報を電子メールなどのプロバイダーに送信する規則を定義します。 これにより、簡単に、インストルメンテーションをプロバイダーの監視のヘルスを妨害することができます。 別の例としてには、注文が処理され、SQL Server データベースに各イベントを送信するルールを設定するたびにイベントを発生する可能性があります。 ユーザーがログイン複数回、行と、メールをベースのプロバイダーを使用するイベントを設定に失敗したときにイベントを発生させることがもできます。

既定のプロバイダーとイベントの構成は、グローバルの Web.config ファイルに格納されます。 グローバルの Web.config ファイルを格納すべての Web ベース設定 ASP.NET 1 では、Machine.config ファイルに格納されていた x。 グローバルの Web.config ファイルは、次のディレクトリにあります。

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt;グローバルの Web.config ファイルのセクションでは既定の構成設定を提供します。 これらの設定をオーバーライドまたは実装することで、独自の設定を構成することができます、 &lt;healthMonitoring&gt;アプリケーションの Web.config ファイルのセクションです。

&lt;HealthMonitoring&gt;グローバルの Web.config ファイルのセクションには、次の項目が含まれています。

| **プロバイダー** | イベント ビューアー、WMI、および SQL Server の設定プロバイダーが含まれています。 |
| --- | --- |
| **eventMappings** | さまざまな WebBase クラスのマッピングが含まれます。 独自のイベント クラスを生成する場合は、このリストを拡張できます。 独自のイベント クラスを生成できます粒度の細かいに情報を送信するプロバイダー。 たとえば、電子メールに独自のカスタム イベントを送信中に SQL Server に送信される未処理の例外を構成できます。 |
| **ルール** | プロバイダーに、eventMappings をリンクします。 |
| **バッファリング** | SQL Server および電子メール プロバイダーを使用すると、イベントをプロバイダーにフラッシュする頻度を決定します。 |

グローバルの Web.config ファイルからのコード例を次に示します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>イベント ビューアーにイベントを格納する方法

前述イベントのログのプロバイダー、イベント ビューアーは用に構成するグローバルの Web.config ファイルにします。 既定で、すべてのイベントに基づいて**WebBaseErrorEvent**と**WebFailureAuditEvent**ログに記録されます。 追加情報をイベント ログに記録する追加の規則を追加することができます。 たとえば、すべてのイベント ログに記録する場合は (*つまり*、すべてのイベントに基づいて**WebBaseEvent**)、Web.config ファイルに次の規則を追加する可能性があります。

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

この規則はリンク、**すべてのイベント**イベント ログ プロバイダーにイベント マップします。 グローバルの Web.config ファイルでは、eventMapping とプロバイダーの両方が含まれています。

## <a name="how-to-store-events-to-sql-server"></a>SQL Server へのイベントを格納する方法

このメソッドを使用して、 **ASPNETDB** Aspnet、によって生成されるデータベース\_regsql.exe ツールです。 既定のプロバイダー文字列を使用して、LocalSqlServer 接続、アプリのいずれか、ファイル ベースのデータベースを使用する\_データ フォルダーまたは SQL Server のローカル SQLExpress インスタンス。 LocalSqlServer 接続文字列と、SqlProvider の両方が、グローバルの Web.config ファイルで構成されます。

グローバルの Web.config ファイルで LocalSqlServer 接続文字列は、このようになります。

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

を別の SQL Server インスタンスを使用する場合、Aspnet を使用する必要があります\_regsql.exe ツールは、%windir%\microsoft.net\framework\v2.0。 で見つかります\*フォルダーです。 Aspnet を使用して\_regsql.exe ツール、カスタムの生成を**ASPNETDB**アプリケーション構成ファイルを接続文字列を追加し、新しいを使用して、プロバイダーを追加して、SQL Server インスタンス上のデータベース接続文字列です。 作成したら、 **ASPNETDB**作成されたデータベースを sqlProvider、eventMapping にリンクするルールを設定する必要があります。

既定の SqlProvider 使用または独自のプロバイダーを構成するかどうかは event マップを使用してプロバイダーをリンク規則に追加する必要があります。 次の規則を作成した新しいプロバイダーのリンク、**すべてのイベント**イベント マップします。 このルールに基づくすべてのイベント ログに記録**WebBaseEvent** MYASPNETDB 接続文字列を使用する MySqlWebEventProvider に送信します。 次のコードは、イベント マップを使用してプロバイダーをリンクするルールを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

のみを SQL Server エラーを送信する場合は、次の規則を追加できます。

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Wmi イベントを転送する方法

Wmi イベントを転送することもできます。 グローバルの Web.config ファイルには、既定で、WMI プロバイダーが構成します。

次のコード例は、wmi イベントを転送するルールを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

プロバイダーとも、イベントをリッスンするように WMI リスナー アプリケーションを eventMapping を関連付けるにはルールを追加する必要があります。 次のコード例は、WMI プロバイダーをリンクする規則を追加、**すべてのイベント**イベント マップ。

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-e-mail"></a>電子メールで送信するイベントを転送する方法

電子メールで送信するイベントを転送することもできます。 どのイベント ルールが意図せず送信の場合と自分で多くの情報を電子メール プロバイダーにマップする可能性がありますの注意が適した SQL Server、またはイベント ログをします。 2 つの電子メール プロバイダーがあります。SimpleMailWebEventProvider と TemplatedMailWebEventProvider です。 それぞれが、どちらも、TemplatedMailWebEventProvider で使用できるのみ、"template"および"detailedTemplateErrors"属性を除き、同じ構成属性です。

> [!NOTE]
> どちらもこれらの電子メール プロバイダーが構成されます。 Web.config ファイルに追加する必要があります。


これらの 2 つの電子メール プロバイダーの主な違いは、SimpleMailWebEventProvider が変更できない汎用テンプレートで電子メールを送信することです。 サンプルの Web.config ファイルは、次の規則を使用して構成済みのプロバイダーの一覧にこの電子メール プロバイダーを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

次の規則は、電子メール プロバイダーに関連付けるにも追加、**すべてのイベント**イベント マップ。

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 のトレース

ASP.NET 2.0 でのトレースを次の 3 つの主な機能強化があります。

1. トレースの統合機能
2. トレース メッセージへのプログラムによるアクセス
3. アプリケーション レベルのトレースの向上

## <a name="integrated-tracing-functionality"></a>統合トレース機能

System.Diagnostics.Trace クラスをトレースの出力、ASP.NET によって生成されます。 メッセージをルーティングし、System.Diagnostics.Trace を ASP.NET のトレースによって生成されます。 メッセージをルーティングできます。 System.Diagnostics.Trace を ASP.NET インストルメンテーションのイベントを転送することもできます。 この機能は、新しい**writeToDiagnosticsTrace**の属性、&lt;トレース&gt;要素。 このブール値が true の場合、ASP.NET のトレース メッセージがトレース メッセージを表示する登録されているすべてのリスナーによって、System.Diagnostics トレース インフラストラクチャ用に転送されます。

## <a name="programmatic-access-to-trace-messages"></a>プログラムでメッセージをトレースにアクセス

ASP.NET 2.0 では、プログラムへのアクセスを使用してすべてのトレース メッセージ、 **TraceContextRecord**クラスおよび**TraceRecords**コレクション。 トレース メッセージをアクセスするための最も効率的な方法は、登録する、 **TraceContextEventHandler** (ASP.NET 2.0 では新しいも) のデリゲートを新しい処理**TraceFinished**イベント。 必要に応じて、トレース メッセージをループし、ことができます。

次のコード サンプルを示します。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

上記の例では TraceRecords コレクションをループし、応答ストリームに各メッセージを書き込みます。

## <a name="improved-application-level-tracing"></a>アプリケーション レベルのトレースの向上

アプリケーション レベルのトレースが、新しいの導入を使用して向上**最新**の属性、&lt;トレース&gt;要素。 この属性は、最新のアプリケーション レベルのトレース出力が表示され、requestLimit によって示された制限を超える古いトレース データは破棄されるかどうかを指定します。 False の場合、requestLimit 属性に到達するまで要求のトレース データが表示されます。

## <a name="aspnet-command-line-tools"></a>ASP.NET コマンド ライン ツール

ASP.NET の構成に役立ついくつかのコマンド ライン ツールがあります。 ASP.NET 開発者が、aspnet 知識がある\_regiis.exe ツールです。 ASP.NET 2.0 には、構成に役立つその他の 3 つのコマンド ライン ツールが用意されています。

次のコマンド ライン ツールを使用できます。

| **ツール** | **使用します。** |
| --- | --- |
| **aspnet\_regiis.exe** | IIS と ASP.NET を登録をできます。 このツールの 2 つのバージョンがある ASP.NET 2.0 に付属する、32 ビット システムがフレームワーク フォルダー内) および 64 ビット システムで、Framework64 フォルダーです。)64 ビット版を 32 ビット OS ではインストールされていません。 |
| **aspnet\_regsql.exe** | ASP.NET SQL Server の登録ツールは、ASP.NET では、SQL Server プロバイダーで使用するための Microsoft SQL Server データベースを作成するを追加または既存のデータベースからオプションが削除されます。 Aspnet\_regsql.exe ファイル フォルダーにある、[drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber、Web サーバーにします。 |
| **aspnet\_regbrowsers.exe** | ASP.NET ブラウザー登録ツールは、解析およびアセンブリにすべてのシステム全体のブラウザーの定義をコンパイルし、アセンブリをグローバル アセンブリ キャッシュにインストールします。 このツールの使用のブラウザー定義ファイル (です。ブラウザー ファイル)、.NET Framework のブラウザーのサブディレクトリからです。 このツールは %SystemRoot%\Microsoft.NET\Framework\version\ ディレクトリにあります。 |
| **aspnet\_compiler.exe** | ASP.NET コンパイル ツールでは、場所、または実稼働サーバーなどのターゲットの場所に配置するため、ASP.NET Web アプリケーションをコンパイルすることができます。 インプレースでコンパイルは、アプリケーションのコンパイル中にエンドユーザーがアプリケーションには、最初の要求で遅延を発生しないために、アプリケーションのパフォーマンスを向上します。 |

Aspnet\_解説しませんが、ここで、ASP.NET 2.0 に regiis.exe ツールはありません。

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server の登録ツール - aspnet\_regsql.exe

ASP.NET SQL Server の登録ツールを使用してオプションのいくつかの種類を設定することができます。 SQL 接続を指定して ASP.NET アプリケーション サービスでは、SQL Server を使用して、情報の管理で、SQL キャッシュ依存関係を使用するデータベースまたはテーブルを指定する指定し、追加するか、またはプロシージャとセッション状態を格納する SQL Server を使用するためのサポートを削除できます。

複数の ASP.NET アプリケーション サービスは、格納およびデータ ソースからデータを取得する管理をプロバイダーに依存します。 各プロバイダーは、データ ソースに固有です。 ASP.NET には、次の ASP.NET 機能の SQL Server プロバイダーが含まれています。

- メンバーシップ (、 [SqlMembershipProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx)クラス)。
- ロール管理 (、 [SqlRoleProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx)クラス)。
- プロファイル (、 [SqlProfileProvider](https://msdn.microsoft.com/en-us/library/system.web.profile.sqlprofileprovider.aspx)クラス)。
- Web パーツのパーソナル化 (、 [SqlPersonalizationProvider](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)クラス)。
- イベントを web (、 [SqlWebEventProvider](https://msdn.microsoft.com/en-us/library/system.web.management.sqlwebeventprovider.aspx)クラス)。

ASP.NET をインストールするときに、サーバーの Machine.config ファイルには、それぞれのプロバイダーに依存する ASP.NET 機能用の SQL Server プロバイダーを指定する構成要素が含まれています。 これらのプロバイダーは、既定では、SQL Server Express 2005 のローカルのユーザー インスタンスへの接続に構成されます。 プロバイダーによって使用される既定の接続文字列を変更する場合、マシンの構成で構成されている ASP.NET の機能のいずれかを使用する前にインストールする必要あります、SQL Server データベースとデータベース要素 Aspnetを使用して、選択した機能の\_regsql.exe です。 SQL 登録ツールを使用して指定したデータベースが既に存在しない場合 (aspnetdb になる既定のデータベース、コマンドラインで指定されていない場合)、現在のユーザーはスキーマ e を作成すると、SQL Server にデータベースを作成する権限が必要データベース内の要素。

### <a name="sql-cache-dependency"></a>SQL キャッシュ依存関係

ASP.NET 出力キャッシュの高度な機能は、SQL キャッシュ依存関係です。 SQL キャッシュ依存の操作の 2 つの異なるモードをサポートしています。 1 つをテーブルのポーリングと SQL Server 2005 のクエリ通知機能を使用する 2 つ目のモードの ASP.NET の実装を使用します。 テーブル ポーリング モードの動作を構成するのには、SQL 登録ツールを使用できます。

### <a name="session-state"></a>セッション状態

既定では、セッション状態の値と情報は、ASP.NET プロセス内のメモリに格納されます。 また、複数の Web サーバーで共有できる、SQL Server データベースにセッション データを格納することができます。 SQL 登録ツールを使用してセッション状態の指定したデータベースが既に存在しない場合、現在のユーザーは、データベース内にスキーマ要素を作成すると、SQL Server にデータベースを作成する権限が必要です。 データベースが存在する場合、現在のユーザーは、既存のデータベースにスキーマ要素を作成する権限が必要です。

セッション状態データベース SQL Server をインストールするには、Aspnet を実行\_regsql.exe ツールとコマンドを使用して、次の情報を供給します。

- SQL Server の名前を使用して、インスタンス、 **-s**オプション。
- SQL Server を実行しているコンピューターにデータベースを作成する権限を持つアカウントのログオン資格情報です。 使用して、 **-e**オプションを使用して、現在ログオンしているユーザーまたはを使用して、 **-u**と共にユーザー ID を指定するオプション、 **-p**パスワードを指定するにはオプションです。
- **-Ssadd**セッション状態のデータベースを追加するコマンド ライン オプションです。

既定では、Aspnet を使用することはできません\_regsql.exe ツールを SQL Server 2005 Express Edition を実行するコンピューターでセッション状態のデータベースをインストールします。

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET ブラウザー登録ツール - aspnet\_regbrowsers.exe

Asp.net version 1.1 では、Machine.config ファイルにというセクションが含まれている&lt;browserCaps&gt;です。 このセクションには、一連正規表現に基づくさまざまなブラウザーの構成を定義する XML エントリにはが含まれています。 ASP.NET version 2.0 では、新しいです。ブラウザー ファイルでは、XML エントリを使用して特定のブラウザーのパラメーターを定義します。 新しいブラウザーで情報を追加するには、新しい追加します。%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers システム上にあるフォルダーにブラウザー ファイルです。

ブラウザー情報が必要するたびに、アプリケーションは .config ファイルを読み込んでいないが、ために、新しいを作成できます。ブラウザー ファイルと実行"aspnet"\_regbrowsers.exe、アセンブリに必要な変更を追加します。 これにより、新しいブラウザー情報をすぐにアクセスして、すべての情報を取得するアプリケーションをシャット ダウンする必要はありませんのでするサーバー。 アプリケーションは、ブラウザーの機能を現在 HttpRequest のブラウザー プロパティを通じてアクセスできます。

Aspnet を実行している場合は、次のオプションは使用可能な\_regbrowser.exe:

| **オプション** | **説明** |
| --- | --- |
| **-?** | 表示、Aspnet\_regbbrowsers.exe コマンド ウィンドウでヘルプ テキスト。 |
| **は** | ランタイム ブラウザー機能アセンブリを作成し、グローバル アセンブリ キャッシュにインストールします。 |
| **-u** | ランタイム ブラウザー機能アセンブリをグローバル アセンブリ キャッシュからアンインストールします。 |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET コンパイル ツール - aspnet\_compiler.exe

ASP.NET コンパイル ツールは、2 つの一般的な方法で使用できます: インプレースでコンパイルおよび展開については、ターゲットの出力ディレクトリが指定されているコンパイルします。

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[場所にアプリケーションのコンパイル](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

ASP.NET コンパイル ツールは、アプリケーションの埋め込み先をコンパイルできます、つまり、正規のコンパイルを発生させる、アプリケーションに複数の要求を行うの動作を模倣します。 プリコンパイル済みサイトのユーザーには、最初の要求でページをコンパイルするによる遅延は発生しません。

場所にサイトをプリコンパイルする場合は、次の項目が適用されます。

- サイトには、そのファイルとディレクトリ構造が保持されます。
- サーバー上のサイトで使用されるすべてのプログラミング言語のコンパイラが必要です。
- 任意のファイルには、コンパイルが失敗した場合、サイト全体はコンパイルに失敗します。

新しいソース ファイルを追加した後、アプリケーションの埋め込み先再コンパイルすることもできます。 ツールは、新しいまたは変更されたファイルのみを含む場合を除き、 **-c**オプション。

> [!NOTE]
> 入れ子になったアプリケーションが含まれるアプリケーションのコンパイルでは、入れ子になったアプリケーションはコンパイルされません。 入れ子になったアプリケーションを個別にコンパイルする必要があります。


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[展開のアプリケーションのコンパイル](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

(ターゲットの場所にコンパイル) の展開用のアプリケーションをコンパイルするには、targetDir パラメーターを指定します。 TargetDir は、Web アプリケーションに対して、最終的な場所か、コンパイルされたアプリケーションを配置します。 使用して、 **-u**オプションがあることができますを変更する、コンパイルされたアプリケーションの特定のファイル再コンパイルせずようにアプリケーションをコンパイルします。 Aspnet\_compiler.exe が静的および動的なファイルの種類の違いになり、結果として得られるアプリケーションを作成するときに異なる方法で処理します。

- 静的ファイルの種類が、関連するコンパイラまたは元のファイルなど、プロバイダーをビルドをしない名前付き .css、.gif、.htm、.html、.jpg、.js などの拡張機能があるとします。 これらのファイルは、保持のディレクトリ構造での相対的な位置で、ターゲットの場所にコピーされるだけです。
- 動的なファイルの種類、関連するコンパイラかなど .asax、.ascx、.ashx、.aspx、.browser、.master、ASP.NET 固有のファイル名拡張子を持つファイルを含め、プロバイダーをビルドできるものです。 ASP.NET コンパイル ツールは、これらのファイルからアセンブリを生成します。 場合、 **-u**オプションを省略すると、ツールもファイル名拡張子を持つファイルを作成します。コンパイル済みのアセンブリに元のソース ファイルをマップします。 アプリケーションのソースのディレクトリ構造を保持するためには、ツールは、対象アプリケーションの対応する場所でプレース ホルダー ファイルを生成します。

使用する必要があります、 **-u**をコンパイルしたアプリケーションのコンテンツを変更できることを示します。 それ以外の場合、以降の変更は無視されます。 または、実行時エラーが発生します。

どのように、ASP.NET コンパイル ツールさまざまな種類のファイル処理時に次の表の説明、 **-u**オプションが含まれています。

| **ファイルの種類** | **コンパイラの処理** |
| --- | --- |
| .ascx、.master、.aspx | これらのファイルはマークアップとソース コードの分離コード ファイルおよびに囲まれているすべてのコードの両方を含む分割&lt;スクリプト runat ="server"&gt;要素。 ハッシュ アルゴリズムから派生した名前でアセンブリにソース コードをコンパイルし、アセンブリは Bin ディレクトリに配置します。 すべてのインライン コード、コードの間で囲まれた、  **&lt; %** と **% &gt;** 角かっこ、マークアップに含まれて、コンパイルされません。 ソース ファイルと同じ名前の新しいファイルがマークアップを格納するために作成し、対応する出力ディレクトリに置かれます。 |
| .ashx、.asmx | これらのファイルはコンパイルされずは同様に、コンパイルされない出力ディレクトリに移動されます。 ハンドラー コードをコンパイルする場合は、アプリのソース コード ファイルにコードを追加して\_コード ディレクトリ。 |
| (以前に一覧表示するファイルの種類の分離コード ファイルを除く) .cpp、.cs、.vb、.jsl | これらのファイルがコンパイルされ、それらを参照するアセンブリにリソースとして含まれます。 ソース ファイルは出力ディレクトリにコピーされません。 コード ファイルを参照していない場合はコンパイルされません。 |
| カスタムのファイルの種類 | これらのファイルはコンパイルされません。 これらのファイルは、対応する出力ディレクトリにコピーされます。 |
| アプリでコード ファイルをソース\_コード サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 |
| アプリ内の .resx と .resource ファイル\_GlobalResources サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 アプリ\_、メイン出力ディレクトリにある GlobalResources サブディレクトリが作成され、出力ディレクトリにソース ディレクトリに配置された .resx または .resources ファイルはコピーされません。 |
| アプリ内の .resx と .resource ファイル\_LocalResources サブディレクトリ | これらのファイルがコンパイルされていないと、対応する出力ディレクトリにコピーされます。 |
| アプリで .skin ファイル\_テーマ サブディレクトリ | .Skin ファイルと静的なテーマ ファイルはコンパイルされず、対応する出力ディレクトリにコピーされます。 |
| .browser Web.config 静的ファイル Bin ディレクトリ内の既存のアセンブリを型します。 | 出力ディレクトリには、これらのファイルがコピーされます。 |

次の表方法、ASP.NET コンパイル ツールさまざまな種類のファイル処理時に、 **-u**オプションを省略するとします。

| **ファイルの種類** | **コンパイラの処理** |
| --- | --- |
| .aspx、.asmx、.ashx、.master | これらのファイルはマークアップとソース コードの分離コード ファイルおよびに囲まれているすべてのコードの両方を含む分割&lt;スクリプト runat ="server"&gt;要素。 ソース コードは、ハッシュ アルゴリズムから派生した名前でアセンブリにコンパイルされます。 生成されたアセンブリは、Bin ディレクトリに配置されます。 すべてのインライン コード、コードの間で囲まれた、  **&lt; %** と **% &gt;** 角かっこ、マークアップに含まれて、コンパイルされません。 コンパイラでは、ソース ファイルと同じ名前のマークアップを格納する新しいファイルを作成します。 これらの結果として得られるファイルは、Bin ディレクトリに配置されます。 コンパイラでは、拡張子を持つが、ソース ファイルと同じ名前のファイルも作成します。マッピング情報が含まれているコンパイル済みです。 します。コンパイル済みのファイルは、ソース ファイルの元の場所に対応する出力ディレクトリに配置されます。 |
| .ascx | これらのファイルは、マークアップとソース コードに分割されます。 ソース コードがアセンブリにコンパイルされ、ハッシュ アルゴリズムから派生した名前を持つ、Bin ディレクトリに配置されます。 マークアップ ファイルは生成されません。 |
| (以前に一覧表示するファイルの種類の分離コード ファイルを除く) .cpp、.cs、.vb、.jsl | .Ascx、.ashx、または .aspx ファイルから生成されるアセンブリによって参照されるソース コードがアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 ソース ファイルはコピーされません。 |
| カスタムのファイルの種類 | 動的なファイルと同様に、これらのファイルがコンパイルされます。 、基になるファイルの種類に応じて、コンパイラは、出力ディレクトリにマッピング ファイルを配置できます。 |
| アプリ内のファイル\_コード サブディレクトリ | このサブディレクトリ内のソース コード ファイルがアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 |
| アプリ内のファイル\_GlobalResources サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 アプリ\_GlobalResources サブディレクトリはメイン出力ディレクトリを作成します。 構成ファイルで指定 appliesTo ="All".resx および .resources ファイルは出力ディレクトリにコピーします。 参照されている場合はコピーされません、 [BuildProvider](https://msdn.microsoft.com/en-us/library/system.web.configuration.buildprovider.aspx)です。 |
| アプリ内の .resx と .resource ファイル\_LocalResources サブディレクトリ | これらのファイルは一意の名前を持つアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 出力ディレクトリには、.resx ファイルまたは .resource ファイルはコピーされません。 |
| アプリで .skin ファイル\_テーマ サブディレクトリ | テーマはアセンブリにコンパイルされ、Bin ディレクトリに配置されます。 スタブ ファイルは .skin ファイル用に作成し、対応する出力ディレクトリに配置します。 静的ファイル (.css) などは、出力ディレクトリにコピーされます。 |
| .browser Web.config 静的ファイル Bin ディレクトリ内の既存のアセンブリを型します。 | 出力ディレクトリには、これらのファイルがコピーされます。 |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[固定のアセンブリ名](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

MSI Windows インストーラーを使用して Web アプリケーションの展開など、一部のシナリオでは、一貫性のあるファイルの名前と内容、アセンブリまたは更新プログラムの構成設定の識別に一貫したディレクトリ構造の使用が必要です。 ような場合、使用することができます、 **- fixednames** ASP.NET コンパイル ツールがアセンブリをコンパイルする必要がありますを指定するオプション、where を使用する代わりにソース ファイルごとに複数のページがアセンブリにコンパイルされます。 これにつながる、アセンブリの数が多いため、スケーラビリティを重視する場合は注意が必要で、このオプションを使用する必要があります。

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[厳密な名前のコンパイル](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

**-Aptca**、 **-delaysign**、 **-keycontainer**と**-keyfile** Aspnet を使用できるように、オプションが用意されていますが\_名前付きのアセンブリを使用せずに compiler.exe 強くを作成する、[厳密名ツール (Sn.exe)](https://msdn.microsoft.com/en-us/library/k5b5tt23.aspx)とは別にします。 これらのオプションはそれぞれ、 **AllowPartiallyTrustedCallersAttribute**、 **AssemblyDelaySignAttribute**、 **AssemblyKeyNameAttribute**、および**AssemblyKeyFileAttribute**です。

これらの属性の詳細については、このコースの範囲外です。

## <a name="labs"></a>ラボ

次のラボのそれぞれは、前の実習でビルドします。 その順序で実行する必要があります。

## <a name="lab-1-using-the-configuration-api"></a>演習 1: 構成 API を使用します。

1. 呼ばれる新しい Web サイトを作成*mod9lab*です。
2. 新しい Web 構成ファイルをサイトに追加します。
3. Web.config ファイルに、次を追加します。


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Web.config ファイルに変更を保存する権限があるようになります。

1. Default.aspx に新しいラベル コントロールを追加する ID を変更と**lblDebugStatus**です。
2. Default.aspx に新しいボタン コントロールを追加します。
3. ボタン コントロールの ID を変更**btnToggleDebug**および**トグル デバッグ状態**です。
4. Default.aspx の分離コード ファイルのコード ビューを開き、追加、**を使用して**の声明**System.Web.Configuration**次のようにします。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. 2 つのプライベート変数を追加するクラスと、ページ\_Init メソッドの次のようにします。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. 次のコード ページを追加\_負荷。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. 保存し、default.aspx を参照します。 ラベル コントロールが現在のデバッグ状態を表示することに注意してください。
2. デザイナーでボタン コントロールをダブルクリックし、ボタン コントロールのクリック イベントに、次のコードを追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. 保存し、default.aspx を [参照] ボタンをクリックします。
2. 各ボタンをクリックし、確認後に web.config ファイルを開き、**デバッグ**属性、&lt;コンパイル&gt;セクションです。

## <a name="lab-2-logging-application-restarts"></a>演習 2: アプリケーションが再起動をログ記録

このラボでは、アプリケーションをシャット ダウン、起動を行って、および再コンパイルのイベント ビューアーでログ記録をオフにすることを許可するコードを作成します。

1. Default.aspx を DropDownList を追加し、ddlLogAppEvents に ID を変更します。
2. 設定、 **AutoPostBack**プロパティに DropDownList を**true**です。
3. 次のドロップダウン リストの項目のコレクションには、3 つの項目を追加します。 ように、**テキスト**最初の項目の*Select Value*と、値-1 です。 ように、**テキスト**と**値**2 番目の項目の**True**と**テキスト**と**値**の 3 番目の項目**False**です。
4. Default.aspx に新しいラベルを追加します。 ID を変更する**lblLogAppEvents**です。
5. Default.aspx の分離コード ビューを開き、次に示すように、型 HealthMonitoringSection の新しい変数の宣言を追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. 次のコード ページ内の既存のコードを追加\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. DropDownList をダブルクリックし、SelectedIndexChanged イベントに次のコードを追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Default.aspx を参照します。
2. ドロップダウン リストに設定**False**です。
3. イベント ビューアーのアプリケーション ログをオフにします。
4. アプリケーションのデバッグ属性を変更する ボタンをクリックします。
5. イベント ビューアーのアプリケーション ログを更新します。 

    1. すべてのイベント ログに記録されたか。
    2. なぜですか。
6. ドロップダウン リストに設定**True です。**
7. アプリケーションのデバッグ属性を切り替えるボタンをクリックします。
8. イベント ビューアーのアプリケーションのログインを更新します。 

    1. すべてのイベント ログに記録されたか。
    2. アプリケーションのシャット ダウンの理由は何でしたか。
9. Web.config ファイルへの変更を見てをテストおよびログ記録を無効にします。

## <a name="more-information"></a>詳細情報:

ASP.NET 2.0 のプロバイダー モデルでは、多くの他の用途で同様のメンバーシップ、プロファイルなどが、アプリケーションの実装だけでなく、独自のプロバイダーを作成することができます。テキスト ファイルにアプリケーション イベント ログに記録するカスタム プロバイダーの作成方法の詳細については、次を参照してください。[このリンク](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/ASPNETProvMod_Prt6.asp)です。
