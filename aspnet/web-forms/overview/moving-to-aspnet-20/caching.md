---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: "キャッシュ |Microsoft ドキュメント"
author: microsoft
description: "キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x に 3 つの異なるキャッシュ オプションが提供されています。出力キャッシュとしてください."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: d3ef613f625d862314eb0bb60f083f60bb2317e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="caching"></a>キャッシュ
====================
によって[Microsoft](https://github.com/microsoft)

> キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x に 3 つの異なるキャッシュ オプションが提供されています。出力キャッシュ、フラグメントがキャッシュされ、キャッシュ API。


キャッシュの理解は、優れた ASP.NET アプリケーションにとって重要です。 ASP.NET 1.x に 3 つの異なるキャッシュ オプションが提供されています。出力キャッシュ、フラグメントがキャッシュされ、キャッシュ API。 ASP.NET 2.0 には、これらのメソッドの 3 つすべてが用意されていますが、重要ないくつかの機能が追加されます。 いくつかの新しいキャッシュ依存関係があるし、開発者にもカスタム キャッシュの依存関係を作成するオプションがあるようになりました。 キャッシュの構成は、ASP.NET 2.0 では大幅にも改良されています。

## <a name="new-features"></a>新機能

## <a name="cache-profiles"></a>キャッシュ プロファイル

キャッシュ プロファイルを使用すると、開発者は個々 のページに適用できる特定のキャッシュ設定を定義します。 たとえば、12 時間後にキャッシュから期限切れでは一部のページがあればは、それらのページに適用できるキャッシュ プロファイルを簡単に作成できます。 新しいキャッシュ プロファイルを追加するには、使用、&lt;いる&gt;構成ファイル内のセクションです。 たとえばと呼ばれるキャッシュ プロファイルの構成を次に示すは*twoday* 12 時間以内のキャッシュの存続期間を構成します。

[!code-xml[Main](caching/samples/sample1.xml)]

特定のページには、このキャッシュ プロファイルを適用するには、次のように @ OutputCache ディレクティブの cacheprofile です属性を使用します。

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>カスタム キャッシュの依存関係

カスタム キャッシュの依存関係ときは泣きました ASP.NET 1.x 開発者。 ASP.NET で 1.x では、CacheDependency クラスには、それから、独自のクラスを派生されませんでした開発者はシールされています。 ASP.NET 2.0 では、その制限が削除され、開発者が独自のカスタム キャッシュの依存関係を開発するために解放します。 CacheDependency クラスは、ファイル、ディレクトリ、またはキャッシュ キーに基づくカスタム キャッシュの依存関係の作成をできます。

たとえば、次のコードでは、Web アプリケーションのルートにある stuff.xml という名前のファイルに基づいて新しいカスタム キャッシュ依存関係を作成します。

[!code-csharp[Main](caching/samples/sample3.cs)]

このシナリオで stuff.xml ファイルが変更されたときにキャッシュされたアイテムは無効になります。

キャッシュ キーを使用してカスタム キャッシュの依存関係を作成することもできます。 キャッシュ キーの削除のこのメソッドを使用して、キャッシュされたデータが無効になります。 次に例を示します。

[!code-csharp[Main](caching/samples/sample4.cs)]

上に挿入されたアイテムを無効にするには、キャッシュ キーとして機能するキャッシュに挿入された項目を削除します。

[!code-csharp[Main](caching/samples/sample5.cs)]

キャッシュ キーとして機能する項目のキーがキャッシュ キーの配列に追加される値と同じである必要があることに注意してください。

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>SQL キャッシュ依存関係のポーリングに基づく*(テーブル ベースの依存関係とも呼ばれます)*

SQL Server 7、2000、SQL キャッシュ依存関係のポーリングに基づくモデルを使用します。 ポーリングに基づくモデルは、テーブル内のデータを変更するときにトリガーがデータベース テーブルにトリガーを使用します。 更新プログラムのトリガーとなる、 **changeId**フィールドが ASP.NET によってチェック定期的に通知テーブルにします。 場合、 **changeId**フィールドが更新されました、ASP.NET と認識されたデータが変更された、キャッシュされたデータは無効になります。

> [!NOTE]
> SQL Server 2005 は、ポーリングに基づくモデルにも行えますが、ポーリングに基づくモデルは、最も効率的なモデルではないため、SQL Server 2005 で、クエリに基づくモデル (後述) を使用することをお勧めします。


正常に動作するポーリングに基づくモデルを使用して SQL キャッシュ依存のためには、テーブルに通知を有効になっている必要があります。 これには、SqlCacheDependencyAdmin クラスを使用してプログラムからまたは aspnet を使用して\_regsql.exe ユーティリティです。

次のコマンド ラインという名前の SQL Server インスタンス上にある、Northwind データベースの Products テーブルの登録*dbase* SQL の依存関係をキャッシュします。

[!code-console[Main](caching/samples/sample6.cmd)]

上記のコマンドで使用されるコマンド ライン スイッチの説明を次に示します。

| **コマンド ライン スイッチ** | **目的** |
| --- | --- |
| -S*サーバー* | サーバー名を指定します。 |
| -ed | SQL キャッシュ依存のため、データベースを有効にすることを指定します。 |
| -d*データベース\_名* | SQL キャッシュ依存のために、有効にする必要があります、データベース名を指定します。 |
| E | その aspnet を示す\_regsql はデータベースに接続するときに Windows 認証を使用する必要があります。 |
| -et | 私たちが有効にすると SQL キャッシュ依存のためのデータベース テーブルを指定します。 |
| -t*テーブル\_名* | SQL キャッシュ依存関係を有効にするデータベース テーブルの名前を指定します。 |

> [!NOTE]
> Aspnet で利用できるその他のスイッチは\_regsql.exe です。 完全な一覧について実行 aspnet\_regsql.exe - しますか? コマンド ライン。


このコマンドの実行時は、SQL Server データベースに、次の変更が行われます。

- **AspNet\_SqlCacheTablesForChangeNotification**テーブルを追加します。 このテーブルには、SQL キャッシュ依存関係の有効な対象のデータベース内の各テーブルに 1 行が含まれています。
- 次のストアド プロシージャは、データベース内で作成されます。


| AspNet\_SqlCachePollingStoredProcedure | クエリ、AspNet\_SqlCacheTablesForChangeNotification テーブルし、SQL キャッシュ依存関係と changeId テーブルごとの値に有効になっているすべてのテーブルを返します。 ポーリングは、このストアド プロシージャを使用してデータが変更されたかどうかの判断されます。 |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | すべての SQL キャッシュ依存のため、AspNet のクエリを実行して有効になっているテーブルを返します\_SqlCacheTablesForChangeNotification テーブルを返す SQL のすべてのテーブルが有効になっている依存関係をキャッシュします。 |
| AspNet\_SqlCacheRegisterTableStoredProcedure | 通知テーブルに必要なエントリを追加することによって SQL キャッシュ依存のためのテーブルを登録し、トリガーを追加します。 |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | 通知テーブルにエントリを削除することによって SQL キャッシュ依存のためのテーブルの登録を解除し、トリガーを削除します。 |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | 変更したテーブルに対して changeId をインクリメントして通知テーブルを更新します。 ASP.NET では、この値を使用して、データが変更されたかどうかを調べます。 以下に示す、このストアド プロシージャ、トリガーによって実行された、テーブルが有効になっているときに作成します。 |


- SQL Server トリガーと呼ばれる***テーブル\_名前*\_AspNet\_SqlCacheNotification\_トリガー**テーブルに対して作成します。 このトリガーの実行、AspNet\_SqlCacheUpdateChangeIdStoredProcedure INSERT、UPDATE、または削除を行うときにテーブルです。
- SQL Server の役割の名称**aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess**がデータベースに追加します。

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server ロールが、AspNet に EXEC アクセス許可を持つ\_SqlCachePollingStoredProcedure です。 ポーリング モデル正常に動作するためには、aspnet にプロセス アカウントを追加する必要があります\_ChangeNotification\_ReceiveNotificationsOnlyAccess ロール。 Aspnet\_regsql.exe ツールはいないこの処理をします。

### <a name="configuring-polling-based-sql-cache-dependencies"></a>ポーリング ベースの SQL キャッシュ依存関係を構成します。

ポーリング SQL キャッシュ依存関係を構成するために必要ないくつかの手順があります。 最初の手順では、データベースおよび前述の表に、有効にします。 その手順が完了したら、残りの構成を次に示します。

- ASP.NET 構成ファイルを構成します。
- SqlCacheDependency を構成します。

### <a name="configuring-the-aspnet-configuration-file"></a>ASP.NET 構成ファイルを構成します。

前のモジュールで説明したように、接続文字列を追加する、に加えて構成する必要ありますも、&lt;キャッシュ&gt;を持つ要素を&lt;sqlCacheDependency&gt;要素の次のように。

[!code-xml[Main](caching/samples/sample7.xml)]

この構成で、SQL キャッシュ依存関係を有効に、 *pubs*データベース。 属性のポーリング間隔を&lt;sqlCacheDependency&gt;要素既定値は 60000 ミリ秒にまたは 1 分です。 (この値は、500 ミリ秒未満をすることはできません)この例では、&lt;追加&gt;要素は、新しいデータベースを追加し、9000000 ミリ秒に設定すると、ポーリング間隔をオーバーライドします。

#### <a name="configuring-the-sqlcachedependency"></a>SqlCacheDependency を構成します。

次の手順では、SqlCacheDependency を構成します。 これを実現する最も簡単な方法では、次のように、@ Outcache ディレクティブで SqlDependency 属性の値を指定します。

[!code-aspx[Main](caching/samples/sample8.aspx)]

上記の @ OutputCache ディレクティブで、SQL キャッシュ依存関係が構成されている、*作成者*テーブルに、 *pubs*データベース。 セミコロンで区切ることによって複数の依存関係を構成することが次のようにします。

[!code-aspx[Main](caching/samples/sample9.aspx)]

SqlCacheDependency を構成する別の方法では、プログラムによって行うです。 次のコードで、新しい SQL キャッシュ依存関係を作成する、*作成者*テーブルに、 *pubs*データベース。

[!code-csharp[Main](caching/samples/sample10.cs)]

SQL キャッシュ依存関係をプログラムで定義する利点の 1 つは、発生した例外を処理することができます。 たとえば、通知を有効になっていないデータベースに対して SQL キャッシュ依存関係を定義しようとする場合、 **DatabaseNotEnabledForNotificationException**例外がスローされます。 その場合は、しようとすることができますと呼び出すことによって、データベースの通知を有効にする、 **SqlCacheDependencyAdmin.EnableNotifications**メソッドと、データベース名を渡します。

同様に、通知を有効になっていないテーブルに対して、SQL キャッシュ依存関係を定義しようとする場合、 **TableNotEnabledForNotificationException**がスローされます。 呼び出すことができます、 **SqlCacheDependencyAdmin.EnableTableForNotifications**メソッドは、データベース名とテーブル名を渡します。

次のコード サンプルは、SQL キャッシュ依存関係を構成するときに、例外処理を正しく構成する方法を示しています。

[!code-csharp[Main](caching/samples/sample11.cs)]

詳細情報: [https://msdn.microsoft.com/en-us/library/t9x04ed2.aspx](https://msdn.microsoft.com/en-us/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>クエリ ベースの SQL キャッシュ依存関係 (SQL Server 2005 のみ)

SQL Server 2005 を SQL キャッシュ依存関係を使用する場合、ポーリングに基づくモデルは必要ありません。 SQL キャッシュ依存関係が (それ以上の構成は必要ありません)、SQL Server インスタンスへの SQL 接続経由で直接通信併用すると SQL Server 2005、SQL Server 2005 のクエリ通知を使用します。

クエリ ベースの通知を有効にする最も簡単な方法は宣言を設定して、 **SqlCacheDependency**するデータ ソース オブジェクトの属性**CommandNotification** を設定および**EnableCaching**属性を**true**です。 このメソッドを使用して、コードは必要ありません。 コマンドの結果は、データに対してソースの変更を実行する場合は、キャッシュ データが無効になります。

次の例は、SQL キャッシュ依存のためのデータ ソース コントロールを構成します。

[!code-aspx[Main](caching/samples/sample12.aspx)]

ここでは、クエリで指定される場合、 **SelectCommand**戻り値は、異なる結果が最初、キャッシュされた結果は無効です。

すべてのデータ ソースを有効にする SQL キャッシュ依存関係の設定を指定することも、 **SqlDependency**の属性、 **@ OutputCache**ディレクティブを**CommandNotification**. 次の例を示します。

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> SQL Server 2005 のクエリ通知の詳細については、SQL Server Books Online を参照してください。


クエリに基づいた SQL キャッシュ依存関係を構成する別のメソッドは、SqlCacheDependency クラスを使用してプログラムからこれを行うにです。 次のコード サンプルは、これを実現する方法を示しています。

[!code-csharp[Main](caching/samples/sample14.cs)]

詳細情報: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>キャッシュ後置換

ページのキャッシュと、Web アプリケーションのパフォーマンスを大幅に高めることができます。 ただし、場合によっては動的であるページ内のキャッシュに保存するページの多くといくつかのフラグメントが必要です。 たとえば、ニュース記事のページで、設定された期間は完全に静的なを作成する場合は、キャッシュに保存するページ全体を設定できます。 ページ要求ごとに変更された回転を表す ad ヘッダーを含める場合は、提供情報を含むページの一部を動的にする必要があります。 ページのキャッシュが、一部のコンテンツを動的に置換することを許可するのには、ASP.NET キャッシュ後置換を行うこともできます。 キャッシュ後置換は、ページ全体は、キャッシュから除外対象としてマークされている特定の部分とキャッシュの出力を示します。 AdRotator コントロールでは、広告バナーの例で、ユーザーごとに、各ページの更新の広告が動的に作成されるように、キャッシュ後置換の活用できます。

これにはキャッシュ後置換を実装する次の 3 つの方法があります。

- Substitution コントロールを宣言によってを使用します。
- Substitution コントロール API を使用してプログラムから、します。
- AdRotator コントロールを使用して、暗黙的にします。

### <a name="substitution-control"></a>Substitution コントロール

ASP.NET の代替コントロールでは、キャッシュされたのではなく、動的に作成がキャッシュされたページのセクションを指定します。 動的なコンテンツを表示するページ上の場所には、代替コントロールを配置します。 実行時に、代替コントロールは、MethodName プロパティで指定するメソッドを呼び出します。 メソッドは、代替コントロールの内容を置き換える文字列を返す必要があります。 メソッドは、含む Page または UserControl のコントロールの静的メソッドである必要があります。 Substitution コントロールを使用すると、サーバーのキャッシュに変更するクライアント側のキャッシュ ページは、クライアントではキャッシュされません。 これにより、ページを今後の要求が動的コンテンツを生成するためのメソッドを呼び出すことです。

### <a name="substitution-api"></a>代替 API

キャッシュされたページの動的なコンテンツをプログラムで作成するに呼び出せる、 [WriteSubstitution](https://msdn.microsoft.com/en-us/library/system.web.httpresponse.writesubstitution.aspx)メソッド ページ コードでメソッドの名前をパラメーターとして渡します。 動的なコンテンツの作成を処理するメソッドは、1 つ[HttpContext](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx)パラメーター文字列を返します。 返される文字列は、指定された場所に置換されるコンテンツです。 Substitution コントロールを宣言して使用する代わりに WriteSubstitution メソッドを呼び出すことの利点は、ことは、ページまたは UserControl オブジェクトの静的メソッドを呼び出すのではなく、任意のオブジェクトのメソッドを呼び出すことができます。

WriteSubstitution メソッドを呼び出すと、サーバーのキャッシュに変更するクライアント側のキャッシュ ページは、クライアントではキャッシュされません。 これにより、ページを今後の要求が動的コンテンツを生成するためのメソッドを呼び出すことです。

### <a name="adrotator-control"></a>AdRotator コントロール

サーバー コントロールを実装する AdRotator は内部的にキャッシュ後置換のサポートします。 AdRotator コントロール、ページ上に配置する場合、親ページがキャッシュされるかどうかに関係なく、要求ごとに一意の提供情報を表示します。 その結果、AdRotator コントロールを含むページがのみキャッシュ サーバーの側です。

## <a name="controlcachepolicy-class"></a>ControlCachePolicy クラス

ControlCachePolicy クラスは、ユーザー コントロールを使用するキャッシュ フラグメントのプログラムによる制御できます。 ASP.NET 内でユーザー コントロールが埋め込まれます、 [BasePartialCachingControl](https://msdn.microsoft.com/en-us/library/system.web.ui.basepartialcachingcontrol.aspx)インスタンス。 BasePartialCachingControl クラスは、キャッシュが有効な出力がユーザー コントロールを表します。

アクセスするときに、 [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx)のプロパティ、 [PartialCachingControl](https://msdn.microsoft.com/en-us/library/system.web.ui.partialcachingcontrol.aspx)コントロール、有効な ControlCachePolicy オブジェクトが常に表示されます。 ただし、アクセスする場合、 [UserControl.CachePolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.usercontrol.cachepolicy.aspx)のプロパティ、 [UserControl](https://msdn.microsoft.com/en-us/library/system.web.ui.usercontrol.aspx)コントロール、有効な ControlCachePolicy オブジェクト場合にのみ表示で、ユーザー コントロールがまだラップ、BasePartialCachingControl コントロールです。 ラップされていない場合、関連付けられている BasePartialCachingControl があるないため、操作しようとするプロパティによって返される ControlCachePolicy オブジェクトは例外をスローします。 ユーザー コントロールのインスタンスが例外を生成せずにキャッシュをサポートしているかどうかを確認するには、調査、 [SupportsCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.controlcachepolicy.supportscaching.aspx)プロパティです。

ControlCachePolicy クラスを使用すると、出力キャッシュを有効にするいくつかの方法の 1 つです。 次に、出力キャッシュの有効化に使用できる方法を説明します。

- 使用して、 [@ OutputCache](https://msdn.microsoft.com/en-us/library/hdxfb6cy.aspx)ディレクティブを有効にするは、宣言型のシナリオでのキャッシュを出力します。
- 使用して、 [PartialCachingAttribute](https://msdn.microsoft.com/en-us/library/system.web.ui.partialcachingattribute.aspx)属性を分離コード ファイル内のユーザー コントロールのキャッシュを有効にします。
- キャッシュが有効な前のメソッドのいずれかを使用してした、を使用して動的に読み込むBasePartialCachingControlインスタンスで作業してプログラムでのシナリオでキャッシュの設定を指定するControlCachePolicyクラスを使用して[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/en-us/library/system.web.ui.templatecontrol.loadcontrol.aspx)メソッドです。

ControlCachePolicy インスタンスは、コントロールの有効期間の初期化および PreRender 段階の間でのみ正常に操作できます。 PreRender 段階の後 ControlCachePolicy オブジェクトを変更する場合、コントロールが表示された後に行われた変更ことはできません (コントロールは、レンダリング段階キャッシュ済み) のキャッシュ設定に影響実際にため ASP.NET 例外をスローします。 最後に、ユーザー コントロールのインスタンス (とそのため、ControlCachePolicy オブジェクト) はプログラムで操作に使用できる場合にのみが実際にレンダリングされます。

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>キャッシュの構成の変更、&lt;キャッシュ&gt;要素

ASP.NET 2.0 でのキャッシュの構成をいくつかの変更があります。 &lt;キャッシュ&gt;要素は、ASP.NET 2.0 の新機能であり、構成ファイルでキャッシュ構成の変更を行うことができます。 属性は、次のとおりです。

| **要素** | **説明** |
| --- | --- |
| **キャッシュ** | 省略可能な要素です。 グローバル アプリケーションのキャッシュ設定を定義します。 |
| **outputCache** | 省略可能な要素です。 アプリケーション全体の出力キャッシュ設定を指定します。 |
| **いる** | 省略可能な要素です。 アプリケーションのページに適用できる出力キャッシュ設定を指定します。 |
| **sqlCacheDependency** | 省略可能な要素です。 ASP.NET アプリケーションの SQL キャッシュ依存関係を構成します。 |

### <a name="the-ltcachegt-element"></a>&lt;キャッシュ&gt;要素

次の属性はで使用できる、&lt;キャッシュ&gt;要素。

| **属性** | **説明** |
| --- | --- |
| **disableMemoryCollection** | 省略可能な**ブール**属性。 取得またはコンピューター メモリが不足しているときに発生するキャッシュ メモリのコレクションが無効になっているかどうかを示す値を設定します。 |
| **disableExpiration** | 省略可能な**ブール**属性。 取得またはキャッシュ有効期限が無効になっているかどうかを示す値を設定します。 無効の場合、キャッシュされた項目は有効期限はありませんし、有効期限が切れたキャッシュ項目の背景の清掃は発生しません。 |
| **privateBytesLimit** | 省略可能な**Int64**属性。 取得またはメモリを解放しようとしています。 期限切れの項目のキャッシュがフラッシュを開始する前に、アプリケーションのプライベート バイトの最大サイズを示す値を設定します。 この制限には、通常のメモリ オーバーヘッドが実行中のアプリケーションからだけでなく、キャッシュによって使用されるメモリが含まれています。 ゼロに設定するには、ASP.NET がメモリを再利用を開始するタイミングを決定するため、独自のヒューリスティックを使用することを示します。 |
| **percentagePhysicalMemoryUsedLimit** | 省略可能な**Int32**属性。 取得または期限切れの項目のキャッシュがフラッシュを開始する前に、アプリケーションで使用できる、コンピューターの物理メモリの最大パーセンテージを示すため、このメモリの使用には、キャッシュも使用している両方のメモリが含まれます。 メモリを解放しようとしています。 値の設定実行中のアプリケーションの通常のメモリ使用します。 ゼロに設定するには、ASP.NET がメモリを再利用を開始するタイミングを決定するため、独自のヒューリスティックを使用することを示します。 |
| **privateBytesPollTime** | 省略可能な**TimeSpan**属性。 取得またはアプリケーションのプライベート バイトのメモリ使用量のポーリングの間隔を示す値を設定します。 |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt;要素

次の属性は使用できる、 &lt;outputCache&gt;要素。

| **属性** | **説明** |
| --- | --- |
| **enableOutputCache** | 省略可能な**ブール**属性。 ページ出力キャッシュを有効/無効にします。 無効な場合、プログラムまたは宣言型の設定に関係なくページはキャッシュされません。 既定値は**true**です。 |
| **enableFragmentCache** | 省略可能な**ブール**属性。 アプリケーションのフラグメント キャッシュを有効/無効にします。 関係なくページがキャッシュされず無効にした場合、 [@ OutputCache](https://msdn.microsoft.com/en-us/library/hdxfb6cy.aspx)ディレクティブまたは使用されるプロファイルをキャッシュします。 あるアップ ストリーム プロキシ サーバーとブラウザー クライアントしようとしないでくださいページ出力キャッシュを示すキャッシュ制御ヘッダーが含まれます。 既定値は**false**です。 |
| **sendCacheControlHeader** | 省略可能な**ブール**属性。 取得または設定を示す値かどうか、**キャッシュ-コントロール: private**ヘッダーは既定では、出力キャッシュのモジュールによって送信されます。 既定値は**false**です。 |
| **omitVaryStar** | 省略可能な**ブール**属性。 Http 送信を有効/無効に"**変わる場合があります: \*** "応答ヘッダー。 False で、既定の設定で、"**変わる場合があります: \*** "出力キャッシュ ページのヘッダーを送信します。 Vary ヘッダーが送信されると、これは、さまざまなキャッシュに保存するバージョンが Vary ヘッダーで指定されるものに基づいています。 たとえば、*変わる場合があります。 ユーザーのエージェント*要求を発行するユーザー エージェントに基づいてページの異なるバージョンを格納します。 既定値は**false**です。 |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;いる&gt;要素

&lt;いる&gt;要素を前述したようにキャッシュ プロファイルの作成に使用します。 唯一の子要素、&lt;いる&gt;要素は、&lt;これ&gt;キャッシュ プロファイルを構成するための要素。

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt;要素

次の属性は使用できる、 &lt;sqlCacheDependency&gt;要素。

| **属性** | **説明** |
| --- | --- |
| **有効になっています。** | 必要な**ブール**属性。 変更をポーリングするかどうかを示します。 |
| **pollTime** | 省略可能な**Int32**属性。 SqlCacheDependency がデータベース テーブルの変更をポーリングする頻度を設定します。 この値は、連続する当たりますまでのミリ秒数に対応します。 500 ミリ秒未満に設定することはできません。 既定値は、1 分です。 |

### <a name="more-information"></a>説明

キャッシュの構成に関するの注意する必要があるいくつかの追加情報があります。

- キャッシュは、ワーカー プロセスのプライベート バイトの制限が設定されていないと、次の制限のいずれかが使用されます。 

    - x86 2 GB: 800 MB または物理メモリの 60% か小さい方です
    - x86 3 GB: 1800 MB または物理メモリの 60% か小さい方です
    - x 64: 1 テラバイトまたは物理メモリの 60% か小さい方です
- バイト数を制限する場合は、両方のワーカー プロセス プライベートおよび&lt;privateBytesLimit キャッシュ/&gt;が設定された場合、キャッシュが 2 つの最小値を使用します。
- 同じように 1.x では、おキャッシュ エントリを削除で GC を呼び出します。2 つの理由を収集します。 

    - プライベート バイトの制限に近付く非常には
    - 使用可能なメモリが近くまたは 10% 未満
- 効果的にトリムを無効にしてキャッシュの使用可能なメモリ不足の状態を設定して&lt;キャッシュ percentagePhysicalMemoryUseLimit/&gt; 100 です。
- 1.x とは異なり 2.0 が中断されますトリムと収集の呼び出し、最後の GC です。プライベート バイト、または (キャッシュ) のメモリ制限の 1% 以上のマネージ ヒープのサイズ、収集は縮小されませんでした。

## <a name="lab1-custom-cache-dependencies"></a>実習 1: カスタム キャッシュの依存関係

1. 新しい Web サイトを作成します。
2. Cache.xml と呼ばれる新しい XML ファイルを追加し、Web アプリケーションのルートに保存します。
3. 次のコード ページを追加\_default.aspx の分離コードでメソッドを読み込めません。 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. ソース ビューで default.aspx の先頭に、次を追加します。 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Default.aspx を参照します。 時間のような内容
6. ブラウザーを更新します。 時間のような内容
7. Cache.xml を開き、次のコードを追加します。 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.xml を保存します。
9. お使いのブラウザーを更新します。 時間のような内容
10. 以前にキャッシュされた値を表示するのではなく、時間を更新する理由を説明します。

## <a name="lab-2-using-polling-based-cache-dependencies"></a>演習 2: を使用してポーリング ベースのキャッシュ依存関係

この演習では、GridView と DetailsView コントロールを使用して、Northwind データベース内のデータの編集できます。 以前のモジュールで作成したプロジェクトを使用します。

1. Visual Studio 2005 でプロジェクトを開きます。
2. Aspnet を実行\_regsql ユーティリティに対して、Northwind データベースをデータベースと、Products テーブルを有効にします。 Visual Studio コマンド プロンプトから次のコマンドを使用します。 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Web.config ファイルに、次を追加します。 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Showdata.aspx と呼ばれる新しい web フォームを追加します。
5. Showdata.aspx ページには、@ outputcache ディレクティブ次を追加します。 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. 次のコード ページを追加\_showdata.aspx の読み込み。 

    [!code-html[Main](caching/samples/sample21.html)]
7. Showdata.aspx に新しい SqlDataSource コントロールを追加し、Northwind データベースの接続を使用するように構成します。 [次へ] をクリックします。
8. ProductName および ProductID のチェック ボックスを選択し、[次へ] をクリックします。
9. [完了] をクリックします。
10. 新しい GridView を showdata.aspx ページに追加します。
11. ドロップダウン リストから SqlDataSource1 を選択します。
12. 保存し、showdata.aspx を参照します。 表示される時刻をメモしてをおきます。
