---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: アプリケーションの起動 (c#) にデータをキャッシュ |Microsoft Docs
author: rick-anderson
description: 一部のデータが頻繁に使用する Web アプリケーションで、一部のデータの使用は頻度の低い。 この ASP.NET アプリケーション b のパフォーマンスを改善できる.
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c7d00a21663746964e086a75fd4b64ed211ed5f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837419"
---
<a name="caching-data-at-application-startup-c"></a>アプリケーションの起動時 (c#) にデータをキャッシュします。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF のダウンロード](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> 一部のデータが頻繁に使用する Web アプリケーションで、一部のデータの使用は頻度の低い。 頻繁に使用されるデータと呼ばれる手法を事前に読み込むことによって、ASP.NET アプリケーションのパフォーマンスを改善できます。 このチュートリアルでは、事前対応型の読み込みは、アプリケーションの起動時にキャッシュにデータを読み込むを 1 つの方法を示します。


## <a name="introduction"></a>はじめに

2 つの前のチュートリアル、プレゼンテーション層と層のキャッシュのデータ キャッシュについて説明しました。 [ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-cs.md)ObjectDataSource s がプレゼンテーション層でデータをキャッシュする機能のキャッシュを使用しました。 [アーキテクチャでデータをキャッシュ](caching-data-in-the-architecture-cs.md)新しい、個別のキャッシュ レイヤーでのキャッシュを確認します。 両方のために使用するこれらのチュートリアル*事後対応型の読み込み*でデータ キャッシュを操作します。 事後対応型の読み込み、データが要求されるたびに、最初にチェック場合、キャッシュで s。 されていない場合、キャッシュに格納し、データベースなど、元のソースからデータを取得します。 事後対応型の読み込みの主な利点は、簡単に実装します。 不均一なパフォーマンスは、要求間での短所の 1 つです。 上記のチュートリアルからキャッシュ レイヤーを使用して、製品情報を表示するページを想像してください。 このページは、初めてアクセスしたか、メモリの制約または指定した有効期限に到達したことにより、キャッシュされたデータが削除された後に初めてアクセス、データをデータベースから取得する必要があります。 そのため、キャッシュによってこれらのユーザー要求を処理できるユーザーの要求よりも長くかかります。

*プロアクティブな読み込み*のために必要なの前にキャッシュされたデータを読み込むことで要求間で別のキャッシュ管理戦略を滑らかになり、パフォーマンスを提供します。 通常、事前対応型の読み込みは、定期的にチェック、または基になるデータへの更新されたときに通知するいくつかのプロセスを使用します。 このプロセスは、最新に保つためにキャッシュを更新します。 事前対応型の読み込みが遅いデータベース接続、Web サービス、またはその他の動作の遅い特にデータ ソースからデータを基になる場合に特に便利です。 事前対応型の読み込みには、この方法は、作成、管理、および展開プロセスの変更を確認し、キャッシュを更新する必要があるを実装することが困難。

事前対応型の読み込み、およびいきますでこのチュートリアルでは、型の別のフレーバーでは、アプリケーションの起動時にキャッシュにデータを読み込んでいます。 この方法は、データベース ルックアップ テーブル内のレコードなどの静的データをキャッシュに特に便利です。

> [!NOTE]
> 主体的および対応の読み込みと長所と短所、および実装の推奨事項のリスト間の相違点の詳細についてを参照してください、 [、キャッシュの内容を管理する](https://msdn.microsoft.com/library/ms978503.aspx)のセクション、 [.NET Framework アプリケーションのアーキテクチャ ガイドのキャッシュ](https://msdn.microsoft.com/library/ms978498.aspx)します。


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>手順 1: アプリケーションの起動時にキャッシュするデータを決定します。

キャッシュの例は、事後対応型の読み込みを使用して生成するデータを定期的に変更し、長い exorbitantly 受け取らないでうまく前の 2 つのチュートリアルの作業で調べる。 事後対応型の読み込みで使用される有効期限は余分な場合は、キャッシュされたデータが変更ことはありません。 同様に、キャッシュ データが生成する極端に時間を受け取る場合は、それらのユーザー要求が検索に時間がかかる待機中、基になるデータに耐えられるキャッシュを空になりますが取得されます。 静的なデータとアプリケーションの起動時に生成する非常に長い時間がかかるデータのキャッシュを検討してください。

データベースには、多くの動的がありますが、値を頻繁に変更する、ほとんどが、かなりの静的データもあります。 たとえば、ほぼすべてのデータ モデルでは、選択肢の固定セットから特定の値を含む 1 つまたは複数の列があります。 A`Patients`データベース テーブルがあります、`PrimaryLanguage`列を持つ一連の値は、英語、スペイン語、フランス語、ロシア語、日本語、およびの可能性があります。 多くの場合、このような列を使用して実装*ルックアップ テーブル*します。 英語またはフランス語の文字列を格納するのではなく、`Patients`テーブル、2 番目のテーブルが作成を一般的には、それぞれの値のレコードを持つ 2 つの列の一意の識別子と文字列による説明 - を持ちます。 `PrimaryLanguage`内の列、`Patients`テーブルが参照テーブルに対応する一意識別子を格納します。 図 1 の場合は、患者の John Doe の第一言語は英語、Ed Johnson s はロシア語です。


![言語の表は、Patients テーブルで使用される参照テーブルです。](caching-data-at-application-startup-cs/_static/image1.png)

**図 1**:`Languages`テーブルで使用される参照テーブル、`Patients`テーブル


内のレコードによって設定されます、使用可能な言語のドロップダウン リストには編集または作成の新しい患者のユーザー インターフェイスが含まれます、`Languages`テーブル。 このインターフェイスは、毎回キャッシュを使用しないシステムがアクセスしたクエリを実行する必要があります、`Languages`テーブル。 これは無駄な不要な参照テーブルの値の変更頻度の非常に低いため場合、これまでです。

キャッシュでした、`Languages`前のチュートリアルで同じ事後対応型の読み込みの手法を使用してデータ。 事後対応型の読み込みでは、ただし、静的な参照テーブルのデータは必要ありません時間ベースの期限を使用します。 事後対応型の読み込みを使用して、キャッシュがまったくないキャッシュよりも良いでしょう中、に事前に参照テーブルのデータをアプリケーションの起動時にキャッシュに読み込む最善の方法があります。

このチュートリアルでは、ルックアップ テーブルのデータをキャッシュおよびその他の静的情報する方法に注目します。

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>手順 2: データをキャッシュするさまざまな方法を確認します。

情報は、さまざまな方法を使用して ASP.NET アプリケーションでプログラムによってキャッシュされます。 私たち ve 既に前のチュートリアルで、データ キャッシュを使用する方法を説明します。 または、オブジェクトできるプログラムでキャッシュを使用して*静的メンバー*または*アプリケーション状態*します。

クラスを使用する場合通常クラスする必要があります最初にインスタンス化前に、そのメンバーにアクセスすることができます。 たとえばから、ビジネス ロジック層のクラスのいずれかのメソッドを呼び出すためにする必要があります最初にインスタンスを作成、クラスの。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

呼び出すことができます前に*SomeMethod*操作または*SomeProperty*を使用して、クラスのインスタンスを作成する必要があります最初、`new`キーワード。 *SomeMethod*と*SomeProperty*は特定のインスタンスに関連付けられます。 これらのメンバーの有効期間は、関連付けられているオブジェクトの有効期間に関連付けられています。 *静的メンバー*、一方では、変数、プロパティ、メソッド間で共有される*すべて*クラスのインスタンスと、その結果、有効期間は、クラスと同じくらいにあります。 静的メンバーがキーワードで表される`static`します。

静的メンバーだけでなくアプリケーションの状態を使用してデータをキャッシュできます。 各 ASP.NET アプリケーションでは、すべてのユーザーとアプリケーションのページ間で共有される s 名前/値コレクションを保持します。 使用してこのコレクションにアクセスできる、 [ `HttpContext`クラス](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)s [ `Application`プロパティ](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)、ASP.NET ページの分離コード クラスから使用して次のようにします。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

データ キャッシュは、時間および依存関係に基づく切れ、キャッシュ項目の優先度などのメカニズムを提供するデータのキャッシュの多くの高度な API を提供します。 静的メンバーとアプリケーションの状態の場合は、このような機能をページの開発者によって手動で追加する必要があります。 アプリケーションの有効期間にわたってアプリケーションの起動時にデータをキャッシュする場合はただし、データ キャッシュの利点は議論の余地です。 このチュートリアルでは、静的データのキャッシュのすべての 3 つの手法を使用するコードを紹介します。

## <a name="step-3-caching-thesupplierstable-data"></a>手順 3: キャッシュ、`Suppliers`テーブル データ

データベース テーブルの日付に実装した Northwind は、従来のルックアップ テーブルを含めないでください。 4 つのデータ テーブルは、値が静的でないすべてのモデル テーブルを DAL に実装されます。 DAL とし、新しいクラスと、BLL をメソッドに、新しい DataTable を追加する時間を費やすのではなくできるように s だけをこのチュートリアルのふりを`Suppliers`テーブル %s のデータは静的です。 そのため、アプリケーションの起動時にこのデータをキャッシュします。

という名前の新しいクラスの作成を開始する`StaticCache.cs`で、`CL`フォルダー。


![CL フォルダーに StaticCache.cs クラスを作成します。](caching-data-at-application-startup-cs/_static/image2.png)

**図 2**: 作成、`StaticCache.cs`クラス、`CL`フォルダー


このキャッシュからデータを返すメソッドと同様に、適切なキャッシュ ストアに起動時にデータを読み込むメソッドを追加する必要があります。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

上記の例では、静的メンバー変数、`suppliers`からの結果を保持するために、`SuppliersBLL`クラス s`GetSuppliers()`メソッドから呼び出される、`LoadStaticCache()`メソッド。 `LoadStaticCache()`メソッドは、まず、アプリケーションの中に呼び出されます。 このデータはアプリケーションの起動時に読み込まれると、仕入先データを使用する必要がある任意のページを呼び出すことができます、`StaticCache`クラスの`GetSuppliers()`メソッド。 そのため、仕入先を取得するデータベースへの呼び出しのみアプリケーションの起動時に 1 回行われます。

静的メンバー変数を使用して、キャッシュ ストアとしてではなく別の方法として使用できますアプリケーションの状態やデータのキャッシュ。 次のコードでは、アプリケーションの状態の使用によるクラスを示します。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

`LoadStaticCache()`、仕入先の情報がアプリケーション変数に格納されている*キー*します。 適切な型として返されます (`Northwind.SuppliersDataTable`) から`GetSuppliers()`します。 アプリケーションの状態を使用して ASP.NET ページの分離コード クラスにアクセスできる間`Application["key"]`を使用しなければならないアーキテクチャで`HttpContext.Current.Application["key"]`現在を取得するために`HttpContext`します。

同様に、データ キャッシュは、次のコードに示すとしてのキャッシュ ストアとして使用できます。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

時間ベースの期限なしでデータ キャッシュに項目を追加するには、使用、`System.Web.Caching.Cache.NoAbsoluteExpiration`と`System.Web.Caching.Cache.NoSlidingExpiration`入力パラメーターとして値。 S データ キャッシュのこの特定のオーバー ロード`Insert`指定できればようにメソッドが選択されて、*優先度*のキャッシュ項目。 優先順位を使用して、使用可能なメモリが不足しているときに、キャッシュから清掃を行うには、どのような項目を決定します。 ここでは、優先順位を使用`NotRemovable`、清掃このキャッシュ項目が、t が勝利したことが保証されます。

> [!NOTE]
> このチュートリアルのダウンロードの実装、`StaticCache`クラスの静的メンバー変数のアプローチを使用します。 アプリケーションの状態とデータのキャッシュの手法のコードは、クラス ファイル内のコメントで使用できます。


## <a name="step-4-executing-code-at-application-startup"></a>手順 4: アプリケーションの起動時にコードを実行します。

という名前の特殊なファイルの作成に必要なコードを実行するには、web アプリケーションの初回起動時に`Global.asax`します。 このファイルは、アプリケーションで、セッションでのイベント ハンドラーを含めることができ、要求レベルのイベントとそれがここで、アプリケーションが起動されるたびに実行されるコードを追加できます。

追加、`Global.asax`ファイルを Visual Studio のソリューション エクスプ ローラーで web サイト プロジェクト名を右クリックし、新しい項目の追加を選択して web アプリケーションのルート ディレクトリ。 新しい項目の追加 ダイアログ ボックスで、グローバル アプリケーション クラスの項目の種類を選択し、追加 ボタンをクリックします。

> [!NOTE]
> 既にある場合、`Global.asax`ファイルで、プロジェクトでは、グローバル アプリケーション クラスが項目の種類は、新しい項目の追加 ダイアログ ボックスは表示されません。


[![Global.asax ファイルを Web アプリケーションのルート ディレクトリに追加します。](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**図 3**: 追加、 `Global.asax` Web アプリケーション ルート ディレクトリにファイル ([フルサイズの画像を表示する をクリックします](caching-data-at-application-startup-cs/_static/image5.png))。


既定の`Global.asax`ファイル テンプレートには、サーバー側で 5 つのメソッドが含まれています。`<script>`タグ。

- **`Application_Start`** 初回起動時に web アプリケーションを実行します。
- **`Application_End`** アプリケーションのシャット ダウン時に実行
- **`Application_Error`** ハンドルされない例外がアプリケーションに到達するたびに実行します。
- **`Session_Start`** 新しいセッションが作成されるときに実行します
- **`Session_End`** セッションが期限切れか、破棄されたときに実行されます。

`Application_Start`イベント ハンドラーは、アプリケーションのライフ サイクル中に 1 回だけ呼び出されます。 アプリケーションの起動時と、最初に、ASP.NET のリソースが、アプリケーションから要求されたアプリケーションが再起動されるまでの実行を継続の内容を変更することによって発生することが、`/Bin`フォルダーを変更する`Global.asax`、変更、内容、`App_Code`フォルダー、または変更、`Web.config`ファイル、その他の原因です。 参照してください[ASP.NET アプリケーションのライフ サイクルの概要](https://msdn.microsoft.com/library/ms178473.aspx)の詳細については、アプリケーションのライフ サイクルにします。

これらのチュートリアルにのみ必要があるコードを追加、`Application_Start`メソッドは、そのを自由に、他のユーザーを削除します。 `Application_Start`を呼び出すだけで、`StaticCache`クラスの`LoadStaticCache()`メソッドは、読み込みおよび仕入先の情報をキャッシュします。


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

すべてが s で終了です。 アプリケーションの起動時に、`LoadStaticCache()`メソッドは、BLL から供給業者の情報を取得し、静的メンバー変数に保存 (を使用して最終的にどのようなキャッシュに格納するか、`StaticCache`クラス)。 この動作を確認するためにブレークポイントを設定、`Application_Start`メソッドと、アプリケーションを実行します。 アプリケーションの開始時にブレークポイントをヒットしたことに注意してください。 ただし、後続の要求も、実行、`Application_Start`メソッドを実行します。


[![Application_Start イベント ハンドラーが実行されていることを確認するブレークポイントを使用してください。](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**図 4**: ことを確認するブレークポイントを使用している、`Application_Start`イベント ハンドラーが実行されている ([フルサイズの画像を表示する をクリックします](caching-data-at-application-startup-cs/_static/image8.png))。


> [!NOTE]
> ヒットしない場合、`Application_Start`ブレークポイント デバッグを開始するときに、アプリケーションが既に開始します。 強制的に変更して再度実行するアプリケーション、`Global.asax`または`Web.config`ファイルし、し、もう一度お試しください。 単に追加 (または削除できます)、アプリケーションを迅速に再起動するこれらのファイルのいずれかの末尾に空白行。


## <a name="step-5-displaying-the-cached-data"></a>手順 5: キャッシュされたデータを表示します。

この時点で、`StaticCache`クラスを介してアクセスできるアプリケーションの起動時にキャッシュされた仕入先データのバージョンには、その`GetSuppliers()`メソッド。 プレゼンテーション層からこのデータを使用するには、ObjectDataSource を使用して、またはプログラムで呼び出すことができます、`StaticCache`クラスの`GetSuppliers()`ASP.NET ページの分離コード クラスのメソッド。 キャッシュされた仕入先情報を表示する、ObjectDataSource コントロールと GridView コントロールの使い方を見て s を使用できます。

開いて開始、`AtApplicationStartup.aspx`ページで、`Caching`フォルダー。 GridView をデザイナーの設定には、ツールボックスからドラッグしてその`ID`プロパティを`Suppliers`します。 次に、GridView から s のスマート タグを選択してという名前の新しい ObjectDataSource を作成する`SuppliersCachedDataSource`します。 構成を使用する ObjectDataSource、`StaticCache`クラスの`GetSuppliers()`メソッド。


[![StaticCache クラスを使用する ObjectDataSource を構成します。](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`StaticCache`クラス ([フルサイズの画像を表示する をクリックします](caching-data-at-application-startup-cs/_static/image11.png))。


[![GetSuppliers() メソッドを使用して、キャッシュされた仕入先データを取得するには](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**図 6**: 使用して、`GetSuppliers()`キャッシュ仕入先データを取得するメソッド ([フルサイズの画像を表示する をクリックします](caching-data-at-application-startup-cs/_static/image14.png))。


ウィザードを完了すると、Visual Studio が自動的に追加 BoundFields のデータ フィールドの各`SuppliersDataTable`します。 GridView コントロールと ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

図 7 では、ブラウザーで表示する際、ページを示します。 出力は同じ BLL s から、データをプルしますが`SuppliersBLL`クラスが使用して、`StaticCache`クラスは、アプリケーションの起動時にキャッシュされたとして仕入先データを返します。 ブレークポイントを設定することができます、`StaticCache`クラスの`GetSuppliers()`メソッドをこの動作を確認します。


[![キャッシュの仕入先データが GridView に表示されます。](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**図 7**: GridView で [仕入先データのキャッシュが表示されます ([フルサイズの画像を表示する] をクリックします](caching-data-at-application-startup-cs/_static/image17.png))。


## <a name="summary"></a>まとめ

すべてのほとんどのデータ モデルには、かなり静的データ、通常、ルックアップ テーブルの形式で実装にはが含まれています。 この情報は、静的であるため s がこの情報を表示する必要があるたびに、データベースを継続的にアクセスする理由。 さらに、その静的な性質により、データをキャッシュする場合が s、有効期限の必要はありません。 このチュートリアルでは、このようなデータを取得し、データ キャッシュ、アプリケーションの状態、および静的メンバー変数を介した、それをキャッシュする方法を説明しました。 この情報は、アプリケーションの起動時にキャッシュされはアプリケーションの有効期間全体でキャッシュに残ります。

このチュートリアルで、過去の 2 つ ve アプリケーション秒の有効期間の間のデータのキャッシュと切れの時間ベースの使用について説明しました。 データベースのデータをキャッシュする場合、時間ベースの有効期限が遅くなりますあります。 キャッシュを定期的にフラッシュするではなく、基になるデータベースのデータが変更されたときにのみ、キャッシュされた項目を削除するのに最適があります。 この理想は、次のチュートリアルで取り上げる SQL キャッシュ依存関係を使用して可能です。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy および Zack Jones でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](caching-data-in-the-architecture-cs.md)
> [次へ](using-sql-cache-dependencies-cs.md)
