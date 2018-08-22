---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: キャッシュ |Microsoft Docs
author: microsoft
description: キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x には、3 つの異なるキャッシュ オプションが提供されています出力キャッシュとしてください.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 5c97464ee50291338a80120a86b1b86b07bc672d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831871"
---
<a name="caching"></a>キャッシュ
====================
によって[Microsoft](https://github.com/microsoft)

> キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x には、3 つの異なるキャッシュ オプションが提供されています出力キャッシュ、フラグメント キャッシュ、および、キャッシュ API。


キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x には、3 つの異なるキャッシュ オプションが提供されています出力キャッシュ、フラグメント キャッシュ、および、キャッシュ API。 ASP.NET 2.0 には、これらのメソッドの 3 つすべてが用意されていますが、いくつかの重要な追加機能を追加します。 いくつかの新しいキャッシュ依存関係があるし、開発者にもカスタム キャッシュの依存関係を作成するオプションがあるようになりました。 キャッシュの構成は、ASP.NET 2.0 で大幅にも強化されています。

## <a name="new-features"></a>新機能

## <a name="cache-profiles"></a>キャッシュ プロファイル

キャッシュ プロファイルを使用すると、開発者は、個々 のページに適用できる特定のキャッシュ設定を定義します。 たとえば、12 時間後にキャッシュから期限切れにするいくつかのページがあれば、それらのページに適用できるキャッシュ プロファイルを簡単に作成することができます。 新しいキャッシュ プロファイルを追加するには、使用、 &lt;outputCacheSettings&gt;構成ファイルのセクション。 たとえば、以下と呼ばれるキャッシュ プロファイルの構成は、 *twoday* 12 時間のキャッシュ期間を構成します。

[!code-xml[Main](caching/samples/sample1.xml)]

特定のページにこのキャッシュ プロファイルを適用するには、次に示す @ OutputCache ディレクティブの cacheprofile です属性を使用します。

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>カスタム キャッシュ依存関係

ASP.NET 1.x の開発者向けのカスタム キャッシュ依存関係は、ときは泣きました。 Asp.net 1.x では、CacheDependency クラスの独自のクラスを派生する開発者は、妨げられている封印されています。 ASP.NET 2.0 では、制限が削除され、開発者は独自のカスタム キャッシュ依存関係を開発します。 CacheDependency クラスは、ファイル、ディレクトリ、またはキャッシュ キーに基づくカスタム キャッシュ依存関係を作成できます。

たとえば、次のコードは、Web アプリケーションのルートにある stuff.xml という名前のファイルに基づいて新しいカスタム キャッシュ依存関係を作成します。

[!code-csharp[Main](caching/samples/sample3.cs)]

このシナリオで stuff.xml ファイルが変更されたときに、キャッシュされた項目が無効にします。

キャッシュ キーを使用してカスタム キャッシュ依存関係を作成することもできます。 キャッシュ キーの削除のこのメソッドを使用して、キャッシュされたデータが無効になります。 次に例を示します。

[!code-csharp[Main](caching/samples/sample4.cs)]

上に挿入された項目を無効にするには、キャッシュのキーとして動作するキャッシュに挿入された項目を削除します。

[!code-csharp[Main](caching/samples/sample5.cs)]

キャッシュ キーとして機能する項目のキーが、キャッシュ キーの配列に追加された値と同じでなければならないことに注意してください。

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>SQL キャッシュ依存関係のポーリングに基づいた<em>(テーブル ベースの依存関係とも呼ばれます)</em>

SQL Server 7、2000、SQL キャッシュ依存関係のポーリングに基づいたモデルを使用します。 ポーリング ベースのモデルでは、テーブル内のデータを変更するときにトリガーされるデータベース テーブルにトリガーを使用します。 更新プログラムのトリガーとなる、 **changeId** ASP.NET を定期的にチェックする通知テーブルのフィールド。 場合、 **changeId**フィールドが更新されましたが、ASP.NET を知っている、データが変更され、キャッシュされたデータは無効になります。

> [!NOTE]
> SQL Server 2005 は、ポーリング ベースのモデルを使用もできますが、ポーリング ベースのモデルは、最も効率的なモデルではないためには、SQL Server 2005 で (後述)、クエリ ベースのモデルを使用することをお勧めします。


正常に動作するポーリング ベースのモデルを使用して SQL キャッシュ依存関係の順序は、テーブルによって通知を有効にする必要があります。 これは SqlCacheDependencyAdmin クラスを使用してプログラムで実行できますか、aspnet を使用して\_regsql.exe ユーティリティ。

次のコマンド ラインという名前の SQL Server インスタンス上にある、Northwind データベースの Products テーブルの登録*dbase* SQL の依存関係をキャッシュします。

[!code-console[Main](caching/samples/sample6.cmd)]

上記のコマンドで使用するコマンド ライン スイッチの説明を次に示します。

| **コマンド ライン スイッチ** | **目的** |
| --- | --- |
| -S *server* | サーバー名を指定します。 |
| -ed | SQL キャッシュ依存関係のデータベースを有効にすることを指定します。 |
| -d *database\_name* | SQL キャッシュ依存関係を有効にするデータベース名を指定します。 |
| -E | その aspnet を指定します\_regsql がデータベースに接続するときに Windows 認証を使用する必要があります。 |
| -et | SQL キャッシュ依存関係のデータベース テーブルを有効にするを指定します。 |
| -t *table\_name* | SQL キャッシュ依存関係を有効にするデータベース テーブルの名前を指定します。 |

> [!NOTE]
> Aspnet に使用できるその他のスイッチは\_regsql.exe します。 完全な一覧について実行 aspnet\_regsql.exe - でしょうか。 コマンドライン。


このコマンドの実行時は、SQL Server データベースに、次の変更が行われます。

- **AspNet\_SqlCacheTablesForChangeNotification**テーブルが追加されます。 このテーブルには、SQL キャッシュ依存関係の有効な対象のデータベースのテーブルごとに 1 つの行が含まれています。
- 次のストアド プロシージャは、データベース内に作成されます。


| AspNet\_SqlCachePollingStoredProcedure | クエリを AspNet\_SqlCacheTablesForChangeNotification テーブルし、SQL キャッシュ依存関係と changeId テーブルごとの値が有効にするすべてのテーブルを返します。 このストアド プロシージャは、データが変更されたかどうかを判断するポーリングするために使用されます。 |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | すべての SQL キャッシュ依存関係、AspNet のクエリを実行して有効にするテーブルを返します\_SqlCacheTablesForChangeNotification テーブルを返しますが SQL のすべてのテーブルが有効になっている依存関係をキャッシュします。 |
| AspNet\_SqlCacheRegisterTableStoredProcedure | 通知テーブルに必要なエントリを追加することで SQL キャッシュ依存関係のテーブルを登録し、トリガーを追加します。 |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | 通知テーブルにエントリを削除することで、テーブルを SQL キャッシュ依存関係の登録を解除し、トリガーを削除します。 |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | 変更されたテーブルの changeId をインクリメントして通知テーブルを更新します。 ASP.NET では、この値を使用して、データが変更されたかどうかを判断します。 次に示す、このストアド プロシージャ、トリガー、テーブルが有効になっているときに作成して実行されます。 |


- SQL Server のトリガーと呼ばれる ***テーブル\_名前 *\_AspNet\_SqlCacheNotification\_トリガー**テーブルが作成されます。 このトリガーの実行、AspNet\_SqlCacheUpdateChangeIdStoredProcedure、INSERT、UPDATE、または DELETE がテーブルで実行されるとします。
- SQL Server のロールと呼ばれる**aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess**データベースに追加されます。

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL サーバーの役割が、AspNet に EXEC アクセス許可を持つ\_SqlCachePollingStoredProcedure します。 ポーリング モデル正常に動作するためには、aspnet にプロセス アカウントを追加する必要があります\_ChangeNotification\_ReceiveNotificationsOnlyAccess ロール。 Aspnet\_regsql.exe ツールはこの処理を実行しません。

### <a name="configuring-polling-based-sql-cache-dependencies"></a>ポーリング ベースの SQL キャッシュ依存関係を構成します。

ポーリングに基づいた SQL キャッシュ依存関係を構成するために必要ないくつかの手順があります。 最初の手順では、データベースおよび前述の表に、有効にします。 その手順が完了すると、残りの部分構成のとおりです。

- ASP.NET 構成ファイルを構成します。
- SqlCacheDependency の構成

### <a name="configuring-the-aspnet-configuration-file"></a>ASP.NET 構成ファイルを構成します。

に加えて、前のモジュールで説明したように接続文字列を追加する必要がありますも構成する、&lt;キャッシュ&gt;を持つ要素を&lt;sqlCacheDependency&gt;次に示す要素。

[!code-xml[Main](caching/samples/sample7.xml)]

この構成で SQL キャッシュ依存関係を有効に、 *pubs*データベース。 属性のポーリング間隔に注意してください、 &lt;sqlCacheDependency&gt; 60000 ミリ秒にまたは 1 分間に要素が既定値します。 (この値は、500 ミリ秒未満をすることはできません)この例で、&lt;追加&gt;要素は、新しいデータベースを追加し、9000000 ミリ秒に設定すると、ポーリング間隔よりも優先されます。

#### <a name="configuring-the-sqlcachedependency"></a>SqlCacheDependency の構成

次の手順では、SqlCacheDependency を構成します。 これを実現する最も簡単な方法では、次のように @ Outcache ディレクティブでの SqlDependency 属性の値を指定します。

[!code-aspx[Main](caching/samples/sample8.aspx)]

上記の @ OutputCache ディレクティブで SQL キャッシュ依存関係は用に構成された、*作成者*テーブルに、 *pubs*データベース。 セミコロンで区切ることによって、複数の依存関係を構成できますようになります。

[!code-aspx[Main](caching/samples/sample9.aspx)]

SqlCacheDependency の構成の別の方法では、プログラムによって行います。 次のコードで新しい SQL キャッシュ依存関係を作成する、*作成者*テーブルに、 *pubs*データベース。

[!code-csharp[Main](caching/samples/sample10.cs)]

プログラムで SQL キャッシュ依存関係を定義する利点の 1 つは、発生する例外を処理することができます。 たとえば、通知を有効になっていないデータベースの SQL キャッシュ依存関係を定義しようとした場合、 **DatabaseNotEnabledForNotificationException**例外がスローされます。 その場合は、呼び出すことによって、データベースの通知を有効にするを試行できます、 **SqlCacheDependencyAdmin.EnableNotifications**メソッドとデータベースの名前を渡します。

同様に、通知を有効になっていないテーブルの SQL キャッシュ依存関係を定義しようとした場合、 **TableNotEnabledForNotificationException**がスローされます。 呼び出して、 **SqlCacheDependencyAdmin.EnableTableForNotifications**データベース名とテーブル名を引数としてメソッド。

次のコード サンプルは、SQL キャッシュ依存関係を構成するときに、例外処理を正しく構成する方法を示しています。

[!code-csharp[Main](caching/samples/sample11.cs)]

詳細情報: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>クエリ ベースの SQL キャッシュ依存関係 (SQL Server 2005 のみ)

SQL Server 2005 の SQL キャッシュ依存関係を使用する場合、ポーリング ベースのモデルは必要はありません。 SQL キャッシュ依存関係が (それ以上の構成は必要ありません)、SQL Server インスタンスへの SQL 接続経由で直接通信 SQL Server 2005 を併用すると、SQL Server 2005 のクエリ通知を使用します。

クエリ ベースの通知を有効にする最も簡単な方法は宣言を設定するには、 **SqlCacheDependency**するデータ ソース オブジェクトの属性**CommandNotification** を設定および**EnableCaching**属性を**true**します。 このメソッドを使用して、コードは必要ありません。 コマンドの結果は、データに対してソースの変更を実行する場合は、キャッシュ データが無効になります。

次の例は、SQL キャッシュ依存関係のデータ ソース コントロールを構成します。

[!code-aspx[Main](caching/samples/sample12.aspx)]

この場合は、クエリがで指定されている場合、 **SelectCommand**よりも、異なる結果が同じ戻り値は、キャッシュされた結果は無効になります。

すべてのデータ ソースを有効にする SQL キャッシュ依存関係の設定を指定することも、 **SqlDependency**の属性、 **@ OutputCache**ディレクティブを**CommandNotification**. 次の例を示します。

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> SQL Server 2005 のクエリ通知の詳細については、SQL Server Books Online を参照してください。


別のクエリに基づく SQL キャッシュ依存関係を構成する方法は SqlCacheDependency クラスを使用してプログラムでこれを行うには。 次のコード サンプルでは、これを行う方法を示しています。

[!code-csharp[Main](caching/samples/sample14.cs)]

詳細情報: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>キャッシュ後の置換

ページをキャッシュすると、Web アプリケーションのパフォーマンスを大幅に向上させることができます。 ただし、場合によっては動的であるページ内にキャッシュされるように、ページの多くといくつかのフラグメントが必要です。 たとえばに設定された期間が完全に静的なニュース記事のページを作成する場合は、キャッシュされるようにページ全体を設定できます。 ページ要求ごとに変更された回転を表す ad ヘッダーを含める場合は、提供情報が含まれるページの一部を動的にする必要があります。 ページをキャッシュが、一部のコンテンツを動的に置換できるように、ASP.NET キャッシュ後の置換を使用することができます。 キャッシュ後の置換では、ページ全体は、キャッシュからの除外とマークされている特定の部分と共にキャッシュされる出力を示します。 AdRotator コントロールでは、広告バナーの例で、ユーザーごとに、各ページの更新の広告が動的に作成されるように、キャッシュ後置換の活用できます。

キャッシュ後の置換を実装するために 3 つの方法はあります。

- ここでは、代替コントロールを使用してください。
- プログラムでは、API の代替コントロールを使用します。
- AdRotator コントロールを使用して、暗黙的にします。

### <a name="substitution-control"></a>代替コントロール

ASP.NET の代替コントロールでは、キャッシュされているのではなく、動的に作成されますが、キャッシュされたページのセクションを指定します。 動的なコンテンツを表示するページ上の場所には、代替コントロールを配置します。 実行時に、代替コントロールは、MethodName プロパティで指定したメソッドを呼び出します。 メソッドは、切り替えコントロールの内容を置き換える文字列を返す必要があります。 メソッドは、格納しているページまたはユーザー コントロールのコントロールの静的メソッドである必要があります。 切り替えコントロールを使用するという問題と、ページは、クライアントでキャッシュされませんするためのサーバーのキャッシュ機能に変更するクライアント側のキャッシュ可能性です。 これにより、それ以降の要求、ページが動的なコンテンツを生成するためのメソッドを呼び出すことです。

### <a name="substitution-api"></a>代替 API

キャッシュされたページの動的なコンテンツをプログラムで作成するを呼び出すことができます、 [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx)ページ コードでメソッドの名前をパラメーターとして渡す方法です。 動的なコンテンツの作成を処理するメソッドを 1 つの受け取り[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)パラメーター文字列を返します。 戻り値の文字列は、指定した位置に置換されるコンテンツです。 代替コントロールを宣言して使用する代わりに WriteSubstitution メソッドを呼び出すことの利点は、ページまたはユーザー コントロール オブジェクトの静的メソッドを呼び出すのではなく、任意のオブジェクトのメソッドを呼び出すことです。

WriteSubstitution メソッドを呼び出すと、クライアント側のキャッシュ サーバーのキャッシュ機能に変更するページは、クライアントでキャッシュされません。 これにより、それ以降の要求、ページが動的なコンテンツを生成するためのメソッドを呼び出すことです。

### <a name="adrotator-control"></a>AdRotator コントロール

サーバー コントロールを実装して AdRotator を内部的にキャッシュ後の置換のサポートします。 AdRotator コントロール、ページ上に配置する場合、親ページがキャッシュされているかどうかに関係なく、要求ごとに一意の提供情報を表示します。 その結果、AdRotator コントロールを含むページがのみキャッシュ サーバーの側です。

## <a name="controlcachepolicy-class"></a>ControlCachePolicy クラス

ControlCachePolicy クラスは、ユーザー コントロールを使用して、キャッシュのフラグメントのプログラムによる制御できます。 ASP.NET 内のユーザー コントロールが埋め込まれます、 [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx)インスタンス。 BasePartialCachingControl クラスは、出力キャッシュを有効にするユーザー コントロールを表します。

アクセスするときに、 [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx)のプロパティを[PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx)コントロール、有効な ControlCachePolicy オブジェクトは常に受信します。 ただしにアクセスする場合、 [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx)のプロパティを[UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx)コントロール、ユーザー コントロールがでまだラップされる場合にのみに有効な ControlCachePolicy オブジェクトが表示される、BasePartialCachingControl コントロール。 ラップされていない場合、関連付けられている BasePartialCachingControl があるないため、操作しようとしたときに、プロパティによって返される ControlCachePolicy オブジェクトは例外をスローします。 ユーザー コントロールのインスタンスが例外を生成せずにキャッシュをサポートしているかどうかを確認するには、調査、 [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)プロパティ。

ControlCachePolicy クラスを使用すると、出力キャッシュを有効にするいくつかの方法の 1 つです。 次の一覧には、出力キャッシュを有効に使用できる方法について説明します。

- 使用して、 [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)ディレクティブを有効にするのには、宣言型のシナリオでのキャッシュを出力します。
- 使用して、 [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx)属性の分離コード ファイル内のユーザー コントロールのキャッシュを有効にします。
- ControlCachePolicy クラスを使用して、を使用して動的に読み込みおよびキャッシュが有効な上記の方法のいずれかを使用してされたBasePartialCachingControlインスタンスで作業してプログラムのシナリオでキャッシュ設定を指定する[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx)メソッド。

ControlCachePolicy インスタンスは、コントロールのライフ サイクルの Init と PreRender ステージの間でのみ正常に操作できます。 PreRender フェーズ後に ControlCachePolicy オブジェクトを変更する場合、ASP.NET は、コントロールが表示された後に行われた変更ことはできません (コントロールは、レンダリング段階中にキャッシュ済み) のキャッシュ設定に影響を実際にため例外をスローします。 最後に、ユーザー コントロールのインスタンス (およびそのため、ControlCachePolicy オブジェクト) はのみ使用できますをプログラムで操作が実際にレンダリングされるとき。

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>キャッシュの構成 - 変更、&lt;キャッシュ&gt;要素

ASP.NET 2.0 でのキャッシュの構成にいくつかの変更があります。 &lt;キャッシュ&gt;要素は、ASP.NET 2.0 の新機能であり、構成ファイルでキャッシュの構成の変更を行うことができます。 次の属性は使用できます。

| **要素** | **説明** |
| --- | --- |
| **cache** | 省略可能な要素です。 アプリケーションのグローバル キャッシュ設定を定義します。 |
| **outputCache** | 省略可能な要素です。 アプリケーション全体の出力キャッシュ設定を指定します。 |
| **outputCacheSettings** | 省略可能な要素です。 アプリケーションのページに適用できる出力キャッシュ設定を指定します。 |
| **sqlCacheDependency** | 省略可能な要素です。 ASP.NET アプリケーションの SQL キャッシュ依存関係を構成します。 |

### <a name="the-ltcachegt-element"></a>&lt;キャッシュ&gt;要素

次の属性が表示されます、&lt;キャッシュ&gt;要素。

| **属性** | **説明** |
| --- | --- |
| **disableMemoryCollection** | 省略可能な**ブール**属性。 取得またはコンピューターのメモリ負荷がときに発生するキャッシュ メモリ コレクションが無効になっているかどうかを示す値を設定します。 |
| **disableExpiration** | 省略可能な**ブール**属性。 取得またはキャッシュの有効期限が無効になっているかどうかを示す値を設定します。 キャッシュされた項目が切れない無効にすると、期限切れのキャッシュ項目の背景の清掃は発生しません。 |
| **privateBytesLimit** | 省略可能な**Int64**属性。 取得またはメモリを解放しようとすると、キャッシュがフラッシュを開始する前に、アプリケーションのプライベート バイトの最大サイズに項目が期限切れを示す値を設定します。 この制限には、通常のメモリ オーバーヘッドが実行中のアプリケーションからだけでなく、キャッシュによって使用されるメモリが含まれています。 ゼロに設定するには、ASP.NET がメモリを再利用を開始するタイミングを決定するため、独自のヒューリスティックを使用することを示します。 |
| **percentagePhysicalMemoryUsedLimit** | 省略可能な**Int32**属性。 取得または期限切れの項目のキャッシュがフラッシュを開始する前に、アプリケーションで使用できるコンピューターの物理メモリの最大パーセンテージを示すため、このメモリ使用量には、同様のキャッシュで使用されるメモリには両方が含まれています。 メモリを再利用しようとしています。 値の設定として実行中のアプリケーションの通常のメモリ使用量。 ゼロに設定するには、ASP.NET がメモリを再利用を開始するタイミングを決定するため、独自のヒューリスティックを使用することを示します。 |
| **privateBytesPollTime** | 省略可能な**TimeSpan**属性。 取得またはアプリケーションのプライベート バイトのメモリ使用量のポーリング間の時間間隔を示す値を設定します。 |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt;要素

次の属性は使用できる、 &lt;outputCache&gt;要素。


|       <strong>属性</strong>        |                                                                                                                                                                                                                                                       <strong>説明</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          省略可能な<strong>ブール</strong>属性。 ページ出力キャッシュを有効/無効にします。 無効の場合、プログラムまたは宣言型の設定に関係なくページはキャッシュされません。 既定値は<strong>true</strong>します。                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                省略可能な<strong>ブール</strong>属性。 アプリケーションのフラグメント キャッシュを有効/無効にします。 関係なくページはキャッシュされませんが無効の場合、 [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx)ディレクティブまたはキャッシュ プロファイルを使用します。 アップ ストリーム プロキシ サーバーとブラウザー クライアントがページ出力キャッシュを試みる必要がありますいないことを示すキャッシュ制御ヘッダーが含まれています。 既定値は<strong>false</strong>します。                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      省略可能な<strong>ブール</strong>属性。 取得または設定を示す値かどうか、<strong>キャッシュ-control: private</strong>ヘッダーは、既定で出力キャッシュ モジュールによって送信されます。 既定値は<strong>false</strong>します。                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | 省略可能な<strong>ブール</strong>属性。 Http の送信を有効/無効にします"<strong>Vary: \</strong ><em>"応答ヘッダー。False で、既定の設定で、"</em>* Vary: \* <strong>"出力キャッシュ ページのヘッダーを送信します。Vary ヘッダーを送信するときに、さまざまなキャッシュされるバージョンはに基づいて Vary ヘッダーで指定されています。たとえば、 <em>Vary: ユーザー-エージェント</em>要求を発行するユーザー エージェントに基づいてページの異なるバージョンを格納します。既定値は * * false</strong>します。 |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt;要素

&lt;OutputCacheSettings&gt;要素を前に説明されているキャッシュ プロファイルの作成に使用します。 唯一の子要素、 &lt;outputCacheSettings&gt;要素は、 &lt;outputCacheProfiles&gt;キャッシュ プロファイルを構成するための要素。

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt;要素

次の属性は使用できる、 &lt;sqlCacheDependency&gt;要素。

| **属性** | **説明** |
| --- | --- |
| **enabled** | 必要な**ブール**属性。 変更をポーリングするかどうかを示します。 |
| **pollTime** | 省略可能な**Int32**属性。 SqlCacheDependency がデータベース テーブルの変更をポーリングする頻度を設定します。 この値は、連続する当たります間のミリ秒数に対応します。 500 ミリ秒未満に設定することはできません。 既定値は、1 分です。 |

### <a name="more-information"></a>説明

キャッシュの構成に関するの注意する必要がある追加情報があります。

- ワーカー プロセスのプライベート バイトの制限が設定されていない場合、次の制限のいずれかのキャッシュが使用されます。 

    - x86 2 GB: 800 MB または物理メモリの 60% か小さい方です
    - x86 3 GB: 1800 MB または物理メモリの 60% か小さい方です
    - x 64: 1 テラバイトまたは物理メモリの 60% か小さい方です
- 場合は、両方のワーカー プロセス プライベート バイトの制限と&lt;キャッシュ privateBytesLimit/&gt;設定、キャッシュは、2 つの最小値が使用されます。
- 同じように 1.x では、キャッシュ エントリを削除お GC を呼び出します。2 つの理由を収集します。 

    - プライベート バイト制限に近づいている非常にしています
    - 使用可能なメモリが近くまたは 10% 未満
- 効果的にトリミングを無効にして、使用可能なメモリ不足の状態をキャッシュすることによって設定できます&lt;キャッシュ percentagePhysicalMemoryUseLimit/&gt; 100。
- 1.x とは異なり 2.0 は場合は、中断、トリムおよび収集の呼び出しは前回の GC。プライベート バイト、または (キャッシュ) のメモリ制限の 1% 以上のマネージ ヒープのサイズ、収集は縮小されませんでした。

## <a name="lab1-custom-cache-dependencies"></a>実習 1: カスタム キャッシュ依存関係

1. 新しい Web サイトを作成します。
2. Cache.xml と呼ばれる新しい XML ファイルを追加し、Web アプリケーションのルートに保存します。
3. 次のコード ページを追加\_default.aspx の分離コードでメソッドを読み込みます。 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. ソース ビューで default.aspx の先頭に、次を追加します。 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Default.aspx を参照します。 時間は何と言うでしょうか。
6. ブラウザーを更新します。 時間は何と言うでしょうか。
7. Cache.xml を開き、次のコードを追加します。 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.xml を保存します。
9. お使いのブラウザーを更新します。 時間は何と言うでしょうか。
10. 以前にキャッシュされた値を表示するのではなく、時間を更新する理由について説明します。

## <a name="lab-2-using-polling-based-cache-dependencies"></a>演習 2: を使用してポーリング ベースのキャッシュ依存関係

この演習では、GridView および DetailsView コントロールを使用して Northwind データベース内のデータの編集では、前のモジュールで作成したプロジェクトを使用します。

1. Visual Studio 2005 でプロジェクトを開きます。
2. 実行、aspnet\_データベースと Products テーブルを有効にする Northwind データベースに対して regsql ユーティリティ。 Visual Studio コマンド プロンプトから次のコマンドを使用します。 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Web.config ファイルには、次を追加します。 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Showdata.aspx と呼ばれる新しい web フォームを追加します。
5. Showdata.aspx ページに @ outputcache ディレクティブ、次を追加します。 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. 次のコード ページを追加\_showdata.aspx の読み込み。 

    [!code-html[Main](caching/samples/sample21.html)]
7. Showdata.aspx に新しい SqlDataSource コントロールを追加し、Northwind データベースの接続を使用するように構成します。 [次へ] をクリックします。
8. ProductName と ProductID のチェック ボックスを選択し、[次へ] をクリックします。
9. [完了] をクリックします。
10. 新しい GridView を showdata.aspx ページに追加します。
11. SqlDataSource1 をドロップダウン リストから選択します。
12. 保存し、showdata.aspx を参照します。 表示される時刻をメモしてをおきます。
