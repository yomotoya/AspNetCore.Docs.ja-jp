---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: 構成とインストルメンテーション |Microsoft Docs
author: microsoft
description: 主要な構成の変更と ASP.NET 2.0 のインストルメンテーションがあります。 構成の変更を加える pr 新しい ASP.NET 構成 API を使用する.
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 8437b13cae83208983f26e0a5042a5f6c19e516e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822908"
---
<a name="configuration-and-instrumentation"></a>構成とインストルメンテーション
====================
によって[Microsoft](https://github.com/microsoft)

> 主要な構成の変更と ASP.NET 2.0 のインストルメンテーションがあります。 新しい ASP.NET 構成 API では、プログラムで実行できる構成の変更ができます。 多くの新しい構成設定が存在するさらに、新しい構成とインストルメンテーションを許可します。


主要な構成の変更と ASP.NET 2.0 のインストルメンテーションがあります。 新しい ASP.NET 構成 API では、プログラムで実行できる構成の変更ができます。 多くの新しい構成設定が存在するさらに、新しい構成とインストルメンテーションを許可します。

このモジュールではから読み取ると、ASP.NET 構成ファイルへの書き込みに関連すると、ASP.NET インストルメンテーション方法についても説明 ASP.NET 構成 API を説明します。 ASP.NET トレースで使用できる新しい機能も取り上げます。

## <a name="aspnet-configuration-api"></a>ASP.NET 構成 API

ASP.NET 構成 API を使用すると、開発、展開、および 1 つのプログラミング インターフェイスを使用してアプリケーションの構成データを管理できます。 構成 API を使用するには開発および構成ファイル内の XML を直接編集することがなく、ASP.NET 構成の完了をプログラムで変更します。 さらに、構成 API を作成するコンソール アプリケーションやスクリプトで、Web ベースの管理ツール、および Microsoft 管理コンソール (MMC) スナップインを使用することができます。

次の 2 つの構成管理ツールでは、構成 API を使用し、.NET Framework version 2.0 に含まれています。

- ASP.NET の MMC スナップインで構成 API を使用して、管理タスクを簡略化、構成階層のすべてのレベルからローカルの構成データの統合ビューを提供します。
- Web サイトの管理ツールで、ローカルおよびリモート アプリケーションの構成設定を管理することができますがホストされているサイトを含みます。

ASP.NET 構成 API には、プログラムによる Web サイトおよびアプリケーションの構成に使用できる ASP.NET 管理オブジェクトのセットが構成されています。 管理オブジェクトは、.NET Framework クラス ライブラリとして実装されます。 構成 API のプログラミング モデルにより、コンパイル時にデータ型の強制によるコードの整合性と信頼性を確認します。 アプリケーションの構成を管理しやすいように、構成 API を使用すると、さまざまな別個のコレクションとしてデータを表示するのではなく、1 つのコレクションとして、構成階層内のすべてのポイントからマージされているデータを表示します。構成ファイル。 さらに、構成 API には、構成ファイル内の XML を直接編集することがなくアプリケーション全体の構成を操作することができます。 最後に、API では、Web サイトの管理ツールなどの管理ツールをサポートすることによって構成タスクを簡略化します。 構成 API は、コンピューター上の構成ファイルの作成をサポートしている複数のコンピューターの構成スクリプトを実行してデプロイを簡略化します。

> [!NOTE]
> 構成 API では、IIS アプリケーションの作成をサポートしません。


## <a name="working-with-local-and-remote-configuration-settings"></a>ローカルおよびリモートの構成設定の操作

構成オブジェクトまたはアプリケーションや Web サイトなどの論理エンティティに、コンピューターなどの特定の物理エンティティに適用される構成設定のマージされたビューを表します。 指定された論理エンティティは、ローカル コンピューターまたはリモート サーバー上に存在します。 指定されたエンティティの構成ファイルが存在しない場合、構成オブジェクトは、Machine.config ファイルで定義されている既定の構成設定を表します。

次のクラスから構成を開く方法のいずれかを使用して、構成オブジェクトを取得できます。

1. エンティティがクライアント アプリケーションの場合の ConfigurationManager クラスです。
2. エンティティが Web アプリケーションの場合の WebConfigurationManager クラスです。

これらのメソッドでは、さらに、必要なメソッドと基になる構成ファイルを処理するためにプロパティを提供する構成オブジェクトを返します。 これらのファイルの読み取りまたは書き込みにアクセスできます。

### <a name="reading"></a>読み取り

構成情報を読み取るには、getsection メソッドまたは GetSectionGroup メソッドを使用します。 ユーザーまたはプロセスを読み取るでは、階層内の構成ファイルのすべてのアクセス許可が読み取りが必要です。

> [!NOTE]
> Path パラメーターを受け取る静的 GetSection メソッドを使用する場合、path パラメーターは、コードが実行されているアプリケーションを指す必要があります。 それ以外の場合、パラメーターは無視され、現在実行中のアプリケーションの構成情報が返されます。


### <a name="writing"></a>書き込み

保存方法のいずれかを使用して、構成情報を記述します。 ユーザーまたはプロセスの書き込みに必要な構成ファイルと現在の構成階層のレベルのディレクトリに対する書き込みアクセス許可だけでなく、階層内の構成ファイルのすべての読み取りアクセス許可。

指定されたエンティティの継承された構成設定を表す構成ファイルを生成するには、次の構成の保存方法のいずれかを使用します。

1. 新しい構成ファイルを作成する Save メソッド。
2. 別の場所で新しい構成ファイルを生成する SaveAs メソッド。

## <a name="configuration-classes-and-namespaces"></a>構成クラスと名前空間

多くの構成クラスとメソッドは、互いに似ています。 次の表では、最もよく使用される構成クラスと名前空間について説明します。

| **構成クラスまたは名前空間** | **説明** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx)名前空間 | すべての .NET Framework アプリケーションの主要な構成クラスを含みます。 セクション ハンドラー クラスは、GetSection GetSectionGroup などのメソッドからセクションの構成データの取得に使用されます。 これら 2 つのメソッドは、非静的です。 |
| System.Configuration.Configuration クラス | コンピューター、アプリケーション、Web ディレクトリ、またはその他のリソースに対して構成データのセットを表します。 このクラスには、構成設定を更新し、セクションおよびセクション グループへの参照を取得するため、GetSectionGroup GetSection など便利なメソッドが含まれています。 このクラスは、WebConfigurationManager と ConfigurationManager クラスのメソッドなどのデザイン時の構成データを取得するメソッドの戻り値の型として使用されます。 |
| System.Web.Configuration 名前空間 | 定義されている ASP.NET の構成セクションでは、セクション ハンドラー クラスを含む[ASP.NET 構成設定](https://msdn.microsoft.com/library/b5ysx397.aspx)します。 セクション ハンドラー クラスは、GetSection GetSectionGroup などのメソッドからセクションの構成データの取得に使用されます。 |
| System.Web.Configuration.WebConfigurationManager class | 実行時およびデザイン時の構成設定への参照を取得するための便利なメソッドを提供します。 これらのメソッドは、戻り値の型として System.Configuration.Configuration クラスを使用します。 このクラスの静的 GetSection メソッドまたは System.Configuration.ConfigurationManager クラスの静的でない GetSection メソッドは、同じ意味で使用できます。 Web アプリケーションの構成では、System.Configuration.ConfigurationManager クラスではなく System.Web.Configuration.WebConfigurationManager クラスはお勧めします。 |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx)名前空間 | カスタマイズおよび構成プロバイダーを拡張する方法を提供します。 これは、構成システムですべてのプロバイダー クラスの基本クラスです。 |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx)名前空間 | クラスとインターフェイスを管理および Web アプリケーションの正常性の監視が含まれています。 厳密に言えば、この名前空間には、API の構成の一部はありません。 たとえば、トレースとイベントの発生は、この名前空間のクラスによって実現されます。 |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx)名前空間 | 管理情報と潜在的なコンシューマーにイベントを Windows Management Instrumentation (WMI) を公開するアプリケーションのインストルメンテーションの必要なクラスを提供します。 ASP.NET 状態監視は、WMI を使用して、イベントを配信します。 厳密に言えば、この名前空間には、API の構成の一部はありません。 |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET 構成ファイルからの読み取り

WebConfigurationManager クラスは、ASP.NET 構成ファイルからの読み取りのコア クラスです。 ASP.NET 構成ファイルの読み取りを基本的に 3 つの手順があります。

1. OpenWebConfiguration メソッドを使用して構成オブジェクトを取得します。
2. 構成ファイルで必要なセクションへの参照を取得します。
3. 必要な情報、構成ファイルから読み取ります。

オブジェクトを表す構成は、特定の構成ファイルを表していません。 代わりに、コンピューター、アプリケーション、または Web サイトの構成のマージされたビューを表します。 次のコード サンプルと呼ばれる Web アプリケーションの構成を表す構成オブジェクトをインスタンス化*ProductInfo*します。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> /ProductInfo パスが存在しない場合、上記のコードを返すこととして、指定した既定の構成、machine.config ファイルに注意してください。


構成オブジェクトを作成したらに、構成設定をドリルダウンし、getsection メソッドまたは GetSectionGroup メソッドを使用できます。 次の例では、上記の ProductInfo アプリケーションの権限借用設定への参照を取得します。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>ASP.NET 構成ファイルへの書き込み

構成ファイルからの読み取り、WebConfigurationManager クラスは Asp.NET 構成ファイルに書き込むためのコアです。 ASP.NET 構成ファイルへの書き込みに 3 つの手順もあります。

1. OpenWebConfiguration メソッドを使用して構成オブジェクトを取得します。
2. 構成ファイルで必要なセクションへの参照を取得します。
3. 構成ファイルを保存] または [名前を付けて保存を使用してから、必要な情報を書き込む方法。

次のコードの変更、**デバッグ**の属性、&lt;コンパイル&gt;要素を false にします。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

このコードが実行されたときに、**デバッグ**の属性、&lt;コンパイル&gt;要素に対して false に設定されます、 *webApp*アプリケーションの web.config ファイル。

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management 名前空間は、クラスとインターフェイスを管理および ASP.NET アプリケーションの正常性の監視を提供します。

ログ記録は、イベントをプロバイダーに関連付けられる規則を定義することで実現されます。 このルールは、プロバイダーに送信されるイベントの種類を定義します。 次の基本イベントは、ログに記録するために使用できます。

| **WebBaseEvent** | すべてのイベントのイベントの基本クラスです。 格納に必要な数、イベント メッセージ、およびイベントの詳細イベント コード、イベント詳細コード、日付と時刻、イベントが発生したなどのすべてのイベントのプロパティがシーケンス処理します。 |
| --- | --- |
| **WebManagementEvent** | アプリケーションの有効期間、要求、エラー、およびイベントの監査などの管理イベントのイベントの基本クラスです。 |
| **WebHeartbeatEvent** | 便利なランタイム状態情報をキャプチャする一定の間隔でアプリケーションによって生成されるイベントです。 |
| **WebAuditEvent** | 承認エラー、復号化の障害などの条件をマークするために使用されるセキュリティ監査イベントの基本クラス*など。* |
| **WebRequestEvent** | すべての情報の要求イベントの基本クラス。 |
| **WebBaseErrorEvent** | エラー状態を示すすべてのイベントの基本クラス。 |

使用可能なプロバイダーの種類には、イベントの出力イベント ビューアー、SQL Server、Windows Management Instrumentation (WMI)、および電子メールを送信することができます。 事前構成済みのプロバイダーとイベントのマッピングは、記録されたイベントの出力を取得するために必要な作業量を削減します。

ASP.NET 2.0 では、アプリケーション ドメインの開始し停止、だけでなく、未処理の例外をログに基づくイベントを記録するのに、イベント ログ プロバイダー-、-インボックスを使用します。 これにより、いくつかの基本的なシナリオについて説明します。 たとえばとをアプリケーションは、例外をスローしますが、ユーザーは、エラーを保存しないし、再現することはできません。 、既定のイベント ログ ルールが、どのような種類のエラーが発生したの理解を深めることを例外とスタックの情報を収集することになります。 別の例は、アプリケーションはセッション状態が失われる場合に適用されます。 その場合は、アプリケーション ドメインがリサイクルされると、アプリケーション ドメインが、最初に停止した原因かどうかを判断する、イベント ログで確認できます。

また、状態監視システムは拡張可能です。 たとえば、カスタムの Web イベントを定義、アプリケーション内でそれらを起動、およびイベント情報を電子メールなどのプロバイダーに送信するルールを定義し。 これにより、状態プロバイダーを監視する、インストルメンテーションを簡単に関連付けることができます。 別の例として、注文が処理され、SQL Server データベースに各イベントを送信する規則を設定するたびに、イベントを発生可能性があります。 ユーザーが行で、複数回にログオンし、電子メール ベースのプロバイダーを使用するイベントのセットアップに失敗したときにイベントを発生させることがもできます。

既定のプロバイダーとイベントの構成は、グローバルの Web.config ファイルに格納されます。 グローバルの Web.config ファイル設定を格納すべて Web ベースの ASP.NET 1 では、Machine.config ファイルに格納された x。 グローバルの Web.config ファイルは、次のディレクトリにあります。

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt;グローバルの Web.config ファイルのセクションは既定の構成設定を提供します。 これらの設定を上書きするか、または実装することで、独自の設定を構成、 &lt;healthMonitoring&gt;アプリケーションの Web.config ファイルのセクション。

&lt;HealthMonitoring&gt;グローバルの Web.config ファイルのセクションには、次のものが含まれています。

| **providers** | イベント ビューアー、WMI、および SQL Server の設定プロバイダーが含まれています。 |
| --- | --- |
| **eventMappings** | さまざまな WebBase クラスのマッピングが含まれます。 イベント クラスを生成する場合は、このリストを拡張できます。 イベント クラスを生成できますに細かい粒度に情報を送信するプロバイダー。 たとえば、電子メールに独自のカスタム イベントを送信中に、SQL Server に送信される未処理の例外を構成できます。 |
| **ルール** | EventMappings をプロバイダーにリンクします。 |
| **バッファリング** | SQL Server および電子メール プロバイダーで使用すると、イベントをプロバイダーにフラッシュする頻度を決定します。 |

グローバルの Web.config ファイルからのコード例を次に示します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>イベント ビューアーにイベントを格納する方法

以前では、イベントのログのプロバイダーを説明したように、イベント ビューアーで構成されますがグローバルの Web.config ファイル。 既定では、すべてのイベントに基づく**WebBaseErrorEvent**と**WebFailureAuditEvent**ログに記録されます。 追加情報をイベント ログに記録する追加の規則を追加することができます。 たとえば、すべてのイベント ログに記録する場合は (*つまり*、すべてのイベントに基づいて**WebBaseEvent**)、Web.config ファイルに次の規則を追加する可能性があります。

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

このルールはリンク、**すべてのイベント**イベント ログ プロバイダーにイベントのマップ。 グローバルの Web.config ファイルには、eventMapping とプロバイダーの両方が含まれます。

## <a name="how-to-store-events-to-sql-server"></a>SQL Server へのイベントを格納する方法

このメソッドを使用して、 **ASPNETDB** Aspnet によって生成されるデータベース、\_regsql.exe ツール。 既定のプロバイダーは、アプリのいずれか、ファイル ベースのデータベースを使用するように LocalSqlServer 接続文字列を使用して\_データ フォルダーまたは SQL Server のローカル SQLExpress インスタンス。 LocalSqlServer 接続文字列と、SqlProvider の両方が、グローバルの Web.config ファイルで構成されます。

グローバルの Web.config ファイルに LocalSqlServer 接続文字列のようになります。

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

別の SQL Server インスタンスを使用する場合は、Aspnet を使用する必要があります\_regsql.exe ツールは、%windir%\microsoft.net\framework\v2.0。 で見つかる\*フォルダー。 Aspnet を使用して、\_regsql.exe ツール、カスタムの生成を**ASPNETDB** SQL Server インスタンス上のデータベース、接続文字列をアプリケーション構成ファイルを追加し、新しいを使用してプロバイダーを追加接続文字列。 作成したら、 **ASPNETDB**作成されたデータベースを sqlProvider、eventMapping にリンクするルールを設定する必要があります。

SqlProvider 既定値を使用するか、または独自のプロバイダーを構成すると、かどうかは、リンク イベント マップとプロバイダーの規則を追加する必要があります。 次の規則を先ほど作成した新しいプロバイダーのリンク、**すべてのイベント**イベント マップします。 このルールはに基づいてすべてのイベント ログに記録**WebBaseEvent** MySqlWebEventProvider MYASPNETDB 接続文字列を使用するに送信します。 次のコードは、イベント マップを使用してプロバイダーをリンクするルールを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

のみ SQL Server にエラーを送信する場合は、次の規則を追加できます。

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>WMI にイベントを転送する方法

Wmi イベントを転送することもできます。 グローバルの Web.config ファイルには、既定で、WMI プロバイダーが構成します。

次のコード例では、wmi イベントを転送するルールを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

関連付ける、eventMapping、プロバイダーとも WMI イベントをリッスンするリスナー アプリケーションにルールを追加する必要があります。 次のコード例は、WMI プロバイダーをリンクする規則を追加、**すべてのイベント**イベント マップ。

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>電子メールへのイベントを転送する方法

電子メールへのイベントを転送することもできます。 どのイベント ルールのことが意図せず自分に送信する多くの情報をするように、電子メール プロバイダーにマップする場合がありますに注意が適した SQL Server またはイベント ログをします。 2 つの電子メール プロバイダー; にはSimpleMailWebEventProvider TemplatedMailWebEventProvider. それぞれが、どちらも、TemplatedMailWebEventProvider で使用できるのみ、"template"および"detailedTemplateErrors"属性を除き、同じ構成属性があります。

> [!NOTE]
> これらの電子メール プロバイダーのどちらが構成されています。 Web.config ファイルに追加する必要があります。


これらの 2 つの電子メール プロバイダーの主な違いは、SimpleMailWebEventProvider が変更できない汎用テンプレートで電子メールを送信します。 サンプルの Web.config ファイルは、次の規則を使用して構成済みのプロバイダーの一覧にこの電子メール プロバイダーを追加します。

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

次の規則は、電子メール プロバイダーを関連付けるも追加されます、**すべてのイベント**イベント マップ。

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 のトレース

ASP.NET 2.0 でのトレースを次の 3 つの主な機能強化があります。

1. 統合トレース機能
2. トレース メッセージへのプログラムによるアクセス
3. アプリケーション レベルのトレースの向上

## <a name="integrated-tracing-functionality"></a>統合トレース機能

Asp.net トレース出力、System.Diagnostics.Trace クラスによって出力されるメッセージをルーティングし、System.Diagnostics.Trace への ASP.NET トレースによって出力されるメッセージをルーティングできます。 System.Diagnostics.Trace を ASP.NET インストルメンテーション イベントを転送することもできます。 この機能が新しいによって提供される**writeToDiagnosticsTrace**の属性、&lt;トレース&gt;要素。 このブール値が true の場合、ASP.NET のトレース メッセージがトレース メッセージを表示する登録されているすべてのリスナーで使用するための System.Diagnostics トレース インフラストラクチャに転送されます。

## <a name="programmatic-access-to-trace-messages"></a>トレース メッセージへのプログラムによるアクセス

ASP.NET 2.0 では、プログラムからアクセスするすべてのトレース メッセージを使用して、 **TraceContextRecord**クラスおよび**TraceRecords**コレクション。 トレース メッセージにアクセスする最も効率的な方法では、登録、 **TraceContextEventHandler**デリゲート (この機能は、ASP.NET 2.0 では新しいも)、新しい処理するために**TraceFinished**イベント。 その後、必要に応じてトレース メッセージをループすることができます。

次のコード サンプルでは、これは示しています。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

上記の例では TraceRecords コレクションをループ処理し、応答ストリームに各メッセージを書き込みます。

## <a name="improved-application-level-tracing"></a>アプリケーション レベルのトレースの向上

アプリケーション レベルのトレースは、新しいの概要を使用して向上**mostRecent**の属性、&lt;トレース&gt;要素。 この属性は、最新のアプリケーション レベルのトレース出力が表示され、requestLimit で示された制限を超える以前のトレース データは破棄されるかどうかを指定します。 False の場合、requestLimit 属性に到達するまで要求のトレース データが表示されます。

## <a name="aspnet-command-line-tools"></a>ASP.NET コマンド ライン ツール

ASP.NET の構成を支援するためにいくつかのコマンド ライン ツールがあります。 ASP.NET 開発者は、aspnet 知識がある\_regiis.exe ツール。 ASP.NET 2.0 では、構成を支援するために他の 3 つのコマンド ライン ツールを提供します。

次のコマンド ライン ツールを使用できます。

| **ツール** | **使用** |
| --- | --- |
| **aspnet\_regiis.exe** | IIS と ASP.NET を登録をできます。 このツールの 2 つのバージョンがある (フレームワーク フォルダー) 内の 32 ビット システム用と (Framework64 フォルダー。 の) 内の 64 ビット システム用の ASP.NET 2.0 に付属64 ビット バージョンは、32 ビット OS ではインストールされません。 |
| **aspnet\_regsql.exe** | ASP.NET SQL Server の登録ツールは、ASP.NET では、SQL Server プロバイダーで使用するための Microsoft SQL Server データベースを作成するか、追加または既存のデータベースからオプションを削除に使用されます。 Aspnet\_regsql.exe ファイル フォルダーにある、[drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber を Web サーバー。 |
| **aspnet\_regbrowsers.exe** | ASP.NET ブラウザー登録ツールは、解析しアセンブリのすべてのシステム全体のブラウザー定義をコンパイルし、アセンブリをグローバル アセンブリ キャッシュにインストールします。 ブラウザー定義ファイルが使用されます (します。ブラウザー ファイル)、.NET Framework のブラウザーのサブディレクトリから。 このツールは %SystemRoot%\Microsoft.NET\Framework\version\ ディレクトリにあります。 |
| **aspnet\_compiler.exe** | ASP.NET コンパイル ツールでは、場所または実稼働サーバーなどのターゲットの場所に配置するため、ASP.NET Web アプリケーションをコンパイルすることができます。 インプレースでのコンパイルでは、アプリケーションのコンパイル中にエンドユーザーがアプリケーションに最初の要求で遅延を発生しないため、アプリケーションのパフォーマンスが役立ちます。 |

Aspnet\_regiis.exe ツールは初めて使用する ASP.NET 2.0 ではありません、ここで解説にしません。

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server の登録ツール - aspnet\_regsql.exe

ASP.NET SQL サーバーの登録ツールを使用してオプションのいくつかの種類を設定することができます。 SQL 接続を指定して ASP.NET アプリケーション サービスでは、SQL Server を使用して、情報の管理で、SQL キャッシュ依存関係を使用するデータベースまたはテーブルを指定する指定し、追加するか、またはプロシージャとセッション状態を格納する SQL Server を使用するためのサポートを削除できます。

いくつかの ASP.NET アプリケーション サービスは、データ ソースからデータの取得の格納および管理するプロバイダーに依存します。 各プロバイダーは、データ ソースに固有です。 ASP.NET には、次の ASP.NET 機能の SQL Server プロバイダーが含まれています。

- メンバーシップ (、 [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)クラス)。
- ロールの管理 (、 [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)クラス)。
- プロファイル (、 [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)クラス)。
- Web パーツ パーソナル化 (、 [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)クラス)。
- Web イベント (、 [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)クラス)。

ASP.NET をインストールするときに、サーバーの Machine.config ファイルには、各プロバイダーに依存する ASP.NET 機能の SQL Server プロバイダーを指定する構成要素が含まれます。 これらのプロバイダーは、SQL Server Express 2005 のユーザーのローカル インスタンスに接続するための既定で構成されます。 プロバイダーで使用される既定の接続文字列を変更する場合、マシンの構成で構成されている ASP.NET 機能のいずれかを使用する前にインストールする必要あります、SQL Server データベースとデータベース要素 Aspnetを使用して、選択した機能\_regsql.exe します。 SQL の登録ツールで指定したデータベースが存在しない場合 (aspnetdb データベースにする既定コマンドラインで指定されていない場合)、現在のユーザーはスキーマを作成すると、SQL Server にデータベースを作成する権限が必要データベース内の要素。

### <a name="sql-cache-dependency"></a>SQL キャッシュ依存関係

ASP.NET 出力キャッシュの高度な機能は、SQL キャッシュ依存関係です。 SQL キャッシュ依存関係の 2 つの異なるモードをサポートしています。 テーブルのポーリングと SQL Server 2005 のクエリ通知機能を使用する 2 つ目のモードの ASP.NET の実装を使用します。 操作のテーブルのポーリング モードを構成する SQL 登録ツールを使用できます。

### <a name="session-state"></a>セッション状態

既定では、セッション状態の値と情報は、ASP.NET プロセス内のメモリに格納されます。 または、複数の Web サーバーで共有できる、SQL Server データベースにセッション データを格納できます。 SQL の登録ツールでセッション状態の指定したデータベースが存在しない場合、現在のユーザーは、データベース内のスキーマ要素を作成すると、SQL Server にデータベースを作成する権限が必要です。 データベースが存在する場合、現在のユーザーは、既存のデータベースにスキーマ要素を作成する権限が必要です。

にセッション状態のデータベースを SQL Server をインストールする実行、Aspnet\_regsql.exe ツールとコマンドを使用して、次の情報を提供します。

- SQL Server の名前を使用して、インスタンス、 **-s**オプション。
- SQL Server を実行しているコンピューターにデータベースを作成する権限を持つアカウントのログオン資格情報。 使用して、 **-e** 、現在ログオンしてユーザーを使用するかを使用するオプション、 **-u**と共にユーザー ID を指定するオプション、 **-p**パスワードを指定するオプション。
- **- Ssadd**セッション状態のデータベースを追加するコマンド ライン オプション。

既定では、Aspnet を使用することはできません\_regsql.exe ツールを SQL Server 2005 Express Edition を実行するコンピューターでセッション状態のデータベースをインストールします。

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET のブラウザーの登録ツール - aspnet\_regbrowsers.exe

Asp.net version 1.1 では、Machine.config ファイルという名前のセクションに含まれている&lt;browserCaps&gt;します。 このセクションには、一連正規表現に基づくさまざまなブラウザーの構成を定義する XML エントリにはが含まれています。 ASP.NET version 2.0 では、新しいします。ブラウザー ファイルは、XML エントリを使用して特定のブラウザーのパラメーターを定義します。 新しいブラウザー情報を追加するには、新しい追加します。%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers システム上にあるフォルダーにファイルをブラウザーです。

ブラウザー情報が必要するたびに、アプリケーションは .config ファイルを読み込んでいないが、ため、新しいを作成できます。ブラウザー ファイルと実行 Aspnet\_regbrowsers.exe、アセンブリに必要な変更を追加します。 これにより、サーバーを新しいブラウザー情報をすぐにアクセスし、いずれかの情報を取得するようにアプリケーションをシャット ダウンする必要はありませんので。 アプリケーションの現在の HttpRequest ブラウザー プロパティを通じてブラウザーの機能にアクセスできます。

Aspnet を実行している場合は、次のオプションは使用可能な\_regbrowser.exe:

| **オプション** | **説明** |
| --- | --- |
| **-?** | 表示、Aspnet\_regbbrowsers.exe コマンド ウィンドウでヘルプ テキスト。 |
| **-i** | ランタイムのブラウザー機能のアセンブリを作成し、そのアセンブリをグローバル アセンブリ キャッシュにインストールします。 |
| **-u** | ランタイムのブラウザー機能のアセンブリをグローバル アセンブリ キャッシュからアンインストールします。 |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET コンパイル ツール - aspnet\_compiler.exe

ASP.NET コンパイル ツールは、2 つの一般的な方法で使用できます。 インプレース コンパイルと展開は、ターゲットの出力ディレクトリが指定されているコンパイルします。

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[場所にアプリケーションのコンパイル](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET コンパイル ツールが場所でアプリケーションをコンパイル、つまり、正規表現のコンパイルになり、アプリケーションに複数の要求を行うの動作を模倣します。 プリコンパイル済みサイトのユーザーでは、最初の要求でページをコンパイルすると発生する遅延は発生しません。

場所にサイトをプリコンパイルする場合は、次の項目が適用されます。

- サイトには、そのファイルとディレクトリ構造が保持されます。
- サーバー上のサイトで使用されるすべてのプログラミング言語のコンパイラが必要です。
- すべてのファイルには、コンパイルが失敗した場合、サイト全体のコンパイルが失敗します。

新しいソース ファイルを追加した後に、インプレース アプリケーション再コンパイルすることもできます。 含む場合を除き、ツールが追加または変更されたファイルのみをコンパイル、 **-c**オプション。

> [!NOTE]
> 入れ子になったアプリケーションを含むアプリケーションのコンパイルでは、入れ子になったアプリケーションをコンパイルしません。 入れ子になったアプリケーションを個別にコンパイルする必要があります。


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[展開のアプリケーションのコンパイル](https://msdn.microsoft.com/library/ms229863.aspx)

TargetDir パラメーターを指定するには、デプロイ (ターゲットの場所にコンパイル) 用にアプリケーションをコンパイルします。 TargetDir は、Web アプリケーションの最終的な場所またはコンパイル済みのアプリケーションをさらに展開できます。 使用して、 **-u**オプションは、このような方法を変更することは、コンパイルされたアプリケーションの特定のファイルを再コンパイルなしでアプリケーションをコンパイルします。 Aspnet\_compiler.exe 区別の静的および動的なファイルの種類を作成されたアプリケーションを作成するときに異なる方法で処理します。

- 静的ファイルの種類は、関連するコンパイラおよびビルド ファイルを持つなど、プロバイダーはありませんが、名前付き .css、.gif、.htm、.html、.jpg、.js などの拡張機能がある、という具合です。 これらのファイルは、保持、ディレクトリ構造内の相対的な位置で、ターゲットの場所に単にコピーされます。
- 動的なファイルの種類、関連するコンパイラまたは .asax、.ascx、.ashx、.aspx、.browser、.master、や ASP.NET に固有のファイル名拡張子を持つファイルなどのプロバイダーをビルドするものです。 ASP.NET コンパイル ツールは、これらのファイルからアセンブリを生成します。 場合、 **-u**オプションを省略すると、ツールは、ファイル名拡張子を持つファイルを作成することもできます。コンパイル済みのアセンブリに元のソース ファイルをマップします。 アプリケーションのソースのディレクトリ構造を保持するためには、ツールは、ターゲット アプリケーションの対応する場所でのプレース ホルダー ファイルを生成します。

使用する必要があります、 **-u**コンパイルされたアプリケーションのコンテンツを変更できることを示します。 それ以外の場合、その後の変更は無視されます。 または実行時エラーが発生します。

次の表では、どの ASP.NET コンパイル ツール ハンドルの別のファイル タイプについて説明します、 **-u**オプションを指定します。

| **ファイルの種類** | **コンパイラの処理** |
| --- | --- |
| .ascx, .aspx, .master | これらのファイルは分離コード ファイルとで囲まれたコードの両方を含むマークアップとソース コードに分割&lt;スクリプトの runat ="server"&gt;要素。 ソース コードは、ハッシュのアルゴリズムから派生した名前でアセンブリにコンパイルされ、アセンブリが Bin ディレクトリに配置されます。 すべてのインライン コード、コードの間で囲まれた、 **&lt; %** と**% &gt;** 角かっこ、マークアップに含まれている、コンパイルされません。 ソース ファイルと同じ名前で新しいファイルがマークアップを格納するために作成し、対応する出力ディレクトリに配置されます。 |
| .ashx, .asmx | これらのファイルはコンパイルされず、コンパイルされないであり、出力ディレクトリに移動されます。 ハンドラー コードをコンパイルする場合は、アプリでのソース コード ファイルにコードを配置\_コード ディレクトリ。 |
| (前に示したファイルの種類の分離コード ファイルを含まない) .cpp、.cs、.vb、.jsl | これらのファイルがコンパイルされ、それらを参照するアセンブリにリソースとして含まれています。 ソース ファイルは出力ディレクトリにコピーされません。 コード ファイルが参照されていない場合はコンパイルされません。 |
| カスタム ファイルの種類 | これらのファイルはコンパイルされません。 これらのファイルは、対応する出力ディレクトリにコピーされます。 |
| ソース コード ファイル、アプリで\_コード サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに格納します。 |
| アプリ内の .resx と .resource ファイル\_GlobalResources サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに格納します。 アプリなし\_GlobalResources サブディレクトリが、メイン出力ディレクトリの下に作成され、出力ディレクトリにソース ディレクトリに .resx または .resources ファイルはコピーされません。 |
| アプリ内の .resx と .resource ファイル\_LocalResources サブディレクトリ | これらのファイルはコンパイルされないと、対応する出力ディレクトリにコピーされます。 |
| アプリでファイルを .skin\_テーマ サブディレクトリ | .Skin ファイルと静的なテーマ ファイルがコンパイルされないと、対応する出力ディレクトリにコピーされます。 |
| .browser ファイルの Web.config の静的な型のアセンブリを Bin ディレクトリに既に存在します。 | 出力ディレクトリには、これらのファイルがコピーされます。 |

次の表では、どの ASP.NET コンパイル ツール ハンドルの別のファイル タイプについて説明します、 **-u**オプションを省略するとします。

| **ファイルの種類** | **コンパイラの処理** |
| --- | --- |
| .aspx、.asmx、.ashx、*.master | これらのファイルは分離コード ファイルとで囲まれたコードの両方を含むマークアップとソース コードに分割&lt;スクリプトの runat ="server"&gt;要素。 ソース コードは、ハッシュ アルゴリズムから派生した名前でアセンブリにコンパイルされます。 生成されたアセンブリは、Bin ディレクトリに配置されます。 すべてのインライン コード、コードの間で囲まれた、 **&lt; %** と**% &gt;** 角かっこ、マークアップに含まれている、コンパイルされません。 コンパイラは、ソース ファイルと同じ名前のマークアップを格納する新しいファイルを作成します。 これらの結果として得られるファイルは、Bin ディレクトリに配置されます。 コンパイラでは、拡張子を持つが、ソース ファイルと同じ名前のファイルも作成されます。マッピング情報が含まれているコンパイル済み。 します。コンパイル済みのファイルは、ソース ファイルの元の場所に対応する出力ディレクトリに配置されます。 |
| .ascx | これらのファイルは、マークアップとソース コードに分割されます。 ソース コードがアセンブリにコンパイルされ、ハッシュ アルゴリズムから派生した名前を持つ、Bin ディレクトリに配置します。 マークアップ ファイルは生成されません。 |
| (前に示したファイルの種類の分離コード ファイルを含まない) .cpp、.cs、.vb、.jsl | .Ascx、.ashx、または .aspx ファイルから生成されるアセンブリによって参照されるソース コードがアセンブリにコンパイルされ、Bin ディレクトリに格納します。 ソース ファイルはコピーされません。 |
| カスタム ファイルの種類 | これらのファイルは、動的ファイルのようにコンパイルされます。 、に基づいているファイルの種類によって、コンパイラは、出力ディレクトリにマッピング ファイルを配置できます。 |
| アプリ内のファイル\_コード サブディレクトリ | このサブディレクトリでソース コード ファイルがアセンブリにコンパイルされ、Bin ディレクトリに格納します。 |
| アプリ内のファイル\_GlobalResources サブディレクトリ | これらのファイルがアセンブリにコンパイルされ、Bin ディレクトリに格納します。 アプリなし\_メイン出力ディレクトリ下 GlobalResources サブディレクトリが作成されます。 構成ファイルを appliesTo を指定する場合は"All"を = .resx および .resources ファイルが出力ディレクトリにコピーします。 参照されている場合はコピーされません、 [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)します。 |
| アプリ内の .resx と .resource ファイル\_LocalResources サブディレクトリ | これらのファイルでは、一意の名前を持つアセンブリにコンパイルされ、Bin ディレクトリに格納します。 出力ディレクトリには、.resx ファイルまたは .resource ファイルはコピーされません。 |
| アプリでファイルを .skin\_テーマ サブディレクトリ | テーマでは、アセンブリにコンパイルされ、Bin ディレクトリに格納します。 スタブ ファイルが .skin ファイル用に作成し、対応する出力ディレクトリに配置します。 (.Css) などの静的ファイルは、出力ディレクトリにコピーされます。 |
| .browser ファイルの Web.config の静的な型のアセンブリを Bin ディレクトリに既に存在します。 | 出力ディレクトリには、これらのファイルがコピーされます。 |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[固定のアセンブリ名](https://msdn.microsoft.com/library/ms229863.aspx##)

MSI Windows Installer を使用して Web アプリケーションの配置など、一部のシナリオでは、一貫性のあるファイルの名前と内容、アセンブリまたは更新プログラムの構成設定を識別するために、一貫性のあるディレクトリの構造体の使用が必要です。 その場合、使用することができます、 **-fixednames** ASP.NET コンパイル ツールがアセンブリをコンパイルする必要がありますを指定するオプション、where を使用する代わりにソース ファイルごとに複数のページは、アセンブリにコンパイルされます。 これには、スケーラビリティを重視する場合は注意が必要で、このオプションを使用する必要がありますので、アセンブリの数が多いにつながることができます。

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[厳密な名前のコンパイル](https://msdn.microsoft.com/library/ms229863.aspx##)

**-Aptca**、 **-delaysign**、 **-keycontainer**と **-keyfile** Aspnet を使用できるように、オプションが用意されています\_厳密に作成する compiler.exe、名前付きアセンブリを使用せず、[厳密名ツール (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx)とは別にします。 これらのオプションの対応、それぞれを**AllowPartiallyTrustedCallersAttribute**、 **AssemblyDelaySignAttribute**、 **AssemblyKeyNameAttribute**と**AssemblyKeyFileAttribute**します。

これらの属性の詳細については、このコースの範囲外です。

## <a name="labs"></a>ラボ

次のラボのそれぞれは、前の演習に基づいています。 順序どおりに実行する必要があります。

## <a name="lab-1-using-the-configuration-api"></a>演習 1: 構成 API を使用します。

1. 呼ばれる新しい Web サイト作成*mod9lab*します。
2. 新しい Web 構成ファイルをサイトに追加します。
3. Web.config ファイルには、次を追加します。


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Web.config ファイルに変更を保存する権限があることをこれによりします。

1. Default.aspx に新しいラベル コントロールを追加する ID を変更と**lblDebugStatus**します。
2. Default.aspx に新しいボタン コントロールを追加します。
3. ボタン コントロールの ID を変更**btnToggleDebug**とテキストを**デバッグ ステータスを切り替える**します。
4. Default.aspx の分離コード ファイルのコード ビューを開き、追加、**を使用して**ステートメント**System.Web.Configuration**次のようにします。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. 2 つのプライベート変数を追加するクラスとページ\_次に示すように、Init メソッド。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. 次のコード ページを追加\_負荷。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. 保存し、default.aspx を参照します。 ラベル コントロールが現在のデバッグ状態を表示することに注意してください。
2. デザイナーでボタン コントロールをダブルクリックし、ボタン コントロールのクリック イベントを次のコードを追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. 保存し、[default.aspx の参照] ボタンをクリックします。
2. 各ボタンをクリックし、確認した後に、web.config ファイルを開き、**デバッグ**属性、&lt;コンパイル&gt;セクション。

## <a name="lab-2-logging-application-restarts"></a>演習 2: アプリケーションの再起動のログ記録

このラボでは、アプリケーションをシャット ダウン、新興企業、およびイベント ビューアーでの再コンパイルのログ記録をオフにすることを許可するコードを作成します。

1. DropDownList を default.aspx に追加し、ddlLogAppEvents に ID を変更します。
2. 設定、 **AutoPostBack**プロパティに DropDownList を**true**します。
3. 次のドロップダウン リストの項目のコレクションには、3 つの項目を追加します。 ように、**テキスト**最初の項目の*Select Value*と値の-1。 ように、**テキスト**と**値**2 番目の項目の**True**と**テキスト**と**値**の 3 番目の項目**False**します。
4. Default.aspx に新しいラベルを追加します。 変更する ID **lblLogAppEvents**します。
5. Default.aspx の分離コード ビューを開き、次に示すように、HealthMonitoringSection 型の新しい変数の宣言を追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. 次のコード ページで、既存のコードを追加\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. DropDownList をダブルクリックし、SelectedIndexChanged イベントを次のコードを追加します。


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Default.aspx を参照します。
2. ドロップダウン リストを設定**False**します。
3. イベント ビューアーのアプリケーション ログをオフにします。
4. アプリケーションの Debug 属性を変更するには、ボタンをクリックします。
5. イベント ビューアーのアプリケーション ログを更新します。 

    1. すべてのイベント ログに記録されたか。
    2. なぜですか。
6. ドロップダウン リストを設定**True です。**
7. アプリケーションの Debug 属性を切り替えるボタンをクリックします。
8. イベント ビューアーのアプリケーションのログインを更新します。 

    1. すべてのイベント ログに記録されたか。
    2. アプリのシャット ダウンの理由は何でしたか。
9. オンまたはオフ、ログ記録を実験し、web.config ファイルに加えられた変更確認します。

## <a name="more-information"></a>詳細情報:

ASP.NET 2.0 のプロバイダー モデルでは、多くの他の用途でも (メンバーシップ、プロファイルなど) が、アプリケーションの実装だけでなく、独自のプロバイダーを作成することができます。テキスト ファイルにアプリケーション イベント ログに記録するカスタム プロバイダーの作成の詳細については、次を参照してください。[このリンク](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp)します。
