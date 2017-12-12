---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: "(C#) アプリケーションの起動時にデータのキャッシュ |Microsoft ドキュメント"
author: rick-anderson
description: "一部のデータが頻繁に使用する Web アプリケーションでと、一部のデータを頻繁に使用されます。 この ASP.NET アプリケーション b のパフォーマンス機能を向上させる."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: ccf22f9e72777242ca0239aee69045ab03d56960
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-at-application-startup-c"></a>(C#) アプリケーションの起動時にデータのキャッシュ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF をダウンロードします。](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> 一部のデータが頻繁に使用する Web アプリケーションでと、一部のデータを頻繁に使用されます。 ASP.NET アプリケーションのパフォーマンスを向上させる、頻繁に使用されるデータと呼ばれる手法を事前に読み込むことによりおことができます。 このチュートリアルでは、プロアクティブなを読み込み、これはアプリケーションの起動時に、キャッシュにデータを読み込む方法の 1 つを示します。


## <a name="introduction"></a>はじめに

2 つの前のチュートリアルは、プレゼンテーション層と層のキャッシュ内のデータをキャッシュに検査します。 [、ObjectDataSource でデータをキャッシュ](caching-data-with-the-objectdatasource-cs.md)ObjectDataSource s キャッシュ プレゼンテーション層でデータをキャッシュする機能を使用してきました。 [アーキテクチャのデータ キャッシュ](caching-data-in-the-architecture-cs.md)新しい、個別のキャッシュ層でキャッシュを確認します。 これらのチュートリアルのために使用両方*事後対応型の読み込み*データ キャッシュを使用して操作するためにします。 事後対応型の読み込み、データが要求されるたびに、システム最初に確認を行う場合、キャッシュで s。 以外の場合、データベースなど、元のソースからデータを取得してキャッシュに格納しておく。 事後対応型の読み込みを主な利点は、簡単に実装します。 不均一なパフォーマンスは、要求間での欠点の 1 つです。 前のチュートリアルからキャッシュ レイヤーを使用して製品情報を表示するページを想像してください。 このページが初めて、閲覧またはメモリの制約または指定した有効期限に到達したことにより、キャッシュされたデータが削除された後に初めてアクセスした、ときに、データベースからデータを取得する必要があります。 そのため、キャッシュによってこれらのユーザー要求を処理可能なユーザー要求よりも長くかかります。

*プロアクティブな読み込み*のために必要な前にキャッシュされたデータを読み込むことにより要求間で代替のキャッシュ管理戦略パフォーマンスをその平滑化を提供します。 通常、プロアクティブな読み込みは、定期的にチェック、またはを基になるデータへの更新されたときに通知がいくつかのプロセスを使用します。 このプロセスは、その状態に保つにキャッシュを更新します。 事前対応型の読み込みが低速のデータベース接続、Web サービス、または他の動作が遅く特にデータ ソースから基になるデータが送られた場合に特に便利です。 事前読み込みするには、この方法は、作成、管理、および変更を確認し、キャッシュを更新するためのプロセスを展開する必要がありますを実装するのには困難です。

プロアクティブを読み込み、およびおは、このチュートリアルでは探索を種類別のフレーバーでは、アプリケーションの起動時にキャッシュにデータを読み込んでいます。 この方法は、データベースのルックアップ テーブル内のレコードなどの静的データをキャッシュに特に便利です。

> [!NOTE]
> 主体的および対応を読み込み、さらに長所と短所、および実装に関する推奨事項の一覧の相違点の詳細についてを参照してください、 [、キャッシュの内容を管理する](https://msdn.microsoft.com/en-us/library/ms978503.aspx)のセクションで、 [キャッシュは、.NET Framework アプリケーションのアーキテクチャ ガイド](https://msdn.microsoft.com/en-us/library/ms978498.aspx)です。


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>手順 1: アプリケーションの起動時にキャッシュするデータの種類を決定します。

生成するデータを定期的に変更し、長い exorbitantly 受け取らないで前の 2 つのチュートリアル作業に検査お事後対応型の読み込みを使用してキャッシュの例。 事後対応型の読み込みで使用される有効期限が不要な場合は、キャッシュされたデータを決して変更しません。 同様に、キャッシュされるデータに生成する極端に時間がある場合は、パラメーターは、それらのユーザー要求は検索に時間がかかる待機中、基になるデータに置いておくキャッシュを空になりますが取得されます。 静的データとアプリケーションの起動時に生成する非常に長い時間がかかるデータ キャッシュを検討してください。

データベースがある多数のダイナミック、頻繁に変化する値、最ももことは、相当量の静的データです。 たとえば、ほぼすべてのデータ モデルでは、選択肢の固定セットから特定の値を含む 1 つまたは複数の列があります。 A`Patients`データベース テーブルがあります、`PrimaryLanguage`列を持つ一連の値は、英語、スペイン語、フランス語、ロシア語、日本語、およびに可能性があります。 多くの場合、このような列を使用して実装*ルックアップ テーブル*です。 英語またはフランス語での文字列を格納するのではなく、`Patients`テーブル、2 番目のテーブルが作成を持つ、一般的には、それぞれの値のレコードと 2 つの列の一意の識別子と文字列による説明。 `PrimaryLanguage`内の列、`Patients`テーブルが参照テーブルに対応する一意識別子を格納します。 図 1 の患者 John doe さん s の第一言語は英語、Ed Johnson s はロシア語です。


![言語の表は、Patients テーブルで使用される参照テーブルです。](caching-data-at-application-startup-cs/_static/image1.png)

**図 1**:`Languages`テーブルは、ルックアップ テーブルで使用される、`Patients`テーブル


内のレコードによって設定可能な言語のドロップダウン リストには編集または新しい患者を作成するためのユーザー インターフェイスが含まれます、`Languages`テーブル。 このインターフェイスは、毎回キャッシュを使用しない、システムがアクセスしたクエリを実行する必要があります、`Languages`テーブル。 これは無駄と不要なので、非常にまれに、参照テーブルの値を変更ことです。

キャッシュは、`Languages`前のチュートリアルで確認事後対応型の読み込みと同じ手法を使用してデータをします。 事後対応型の読み込み、ただし、静的なルックアップ テーブルのデータの不要な時間ベースの期限を使用します。 事後対応型の読み込みを使用するキャッシュは、キャッシュなしでよりも高くなりますが、中に最善の方法は能動的に、参照テーブルにデータを読み込む、キャッシュ アプリケーションの起動時になります。

このチュートリアルでは、ルックアップ テーブルのデータをキャッシュし、その他の静的な情報をする方法について取り上げます。

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>手順 2: データをキャッシュするさまざまな方法を確認します。

情報は、さまざまな方法を使用して ASP.NET アプリケーションでプログラムによってキャッシュされます。 お ve 既に前のチュートリアルで、データ キャッシュを使用する方法を説明します。 または、オブジェクトできるプログラムでキャッシュを使用して*静的メンバー*または*アプリケーション状態*です。

クラスを使用するときに通常クラス必要があります最初にインスタンス化前に、そのメンバーにアクセスすることができます。 たとえば、当社のビジネス ロジック層内のクラスのいずれかのメソッドを呼び出す必要があります最初インスタンスを作成クラスの。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

呼び出すことができます前に*SomeMethod*または使用*SomeProperty*、最初にクラスを使用して、インスタンスを作成する必要があります、`new`キーワード。 *SomeMethod*と*SomeProperty*特定のインスタンスに関連付けられています。 これらのメンバーの有効期間は、それらの関連オブジェクトの有効期間に関連付けられています。 *静的メンバー*、変数、プロパティ、および間で共有される方法は、その一方で、*すべて*クラスのインスタンスと、その結果、クラスと同じ長さの有効期間があります。 静的メンバーが、キーワードで表される`static`です。

静的メンバーに加えアプリケーション状態を使用してデータをキャッシュできます。 各 ASP.NET アプリケーションでは、すべてのユーザーとアプリケーションのページ間で共有される s 名前/値のコレクションを保持します。 使用してこのコレクションにアクセスできる、 [ `HttpContext`クラス](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx)s [ `Application`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.application.aspx)、ASP.NET ページの分離コード クラスを使用して次のようにします。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

データ キャッシュは、時間および依存関係に基づく切れ、キャッシュ項目の優先度などのメカニズムを提供するデータのキャッシュの多くの豊富な API を提供します。 静的メンバーとアプリケーションの状態、ページの開発者によってこのような機能が手動で追加する必要があります。 アプリケーションの有効期間中にアプリケーションの起動時にデータをキャッシュする場合は、データ キャッシュの利点が行われます。 このチュートリアルでは、静的データのキャッシュのすべての 3 つの手法を使用するコードを紹介します。

## <a name="step-3-caching-thesupplierstable-data"></a>手順 3: キャッシュ、`Suppliers`テーブル データ

データベース テーブル日付を実装したら、Northwind は、従来のルックアップ テーブルを含めないでください。 次の 4 つのデータ テーブルとして実装された、DAL は静的でない値がすべてのモデル テーブルです。 DAL とし、新しいクラスと、BLL をメソッドに、新しい DataTable を追加する時間を費やすのではなくこのチュートリアルでは s をだけ使用できますのふりを`Suppliers`テーブル %s のデータは静的です。 そのため、アプリケーションの起動時にこのデータをキャッシュする可能性があります。

開始するには、という名前の新しいクラスを作成`StaticCache.cs`で、`CL`フォルダーです。


![CL フォルダーに StaticCache.cs クラスを作成します。](caching-data-at-application-startup-cs/_static/image2.png)

**図 2**: 作成、`StaticCache.cs`クラス内で、`CL`フォルダー


このキャッシュからデータを返すメソッドと同様に、適切なキャッシュ ストアに起動時にデータを読み込むメソッドを追加する必要があります。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

上記の例では、静的メンバー変数、`suppliers`からの結果を保持するために、`SuppliersBLL`クラス s`GetSuppliers()`から呼び出されるメソッド、`LoadStaticCache()`メソッドです。 `LoadStaticCache()`メソッドは、まず、アプリケーションの中に呼び出されます。 このデータはアプリケーションの起動時に読み込まれると、仕入先データを使用する必要がある任意のページを呼び出すことができます、`StaticCache`クラスの`GetSuppliers()`メソッドです。 したがって、仕入先を取得するデータベースへの呼び出しのみアプリケーションの起動時に 1 回は発生します。

静的メンバー変数を使用して、キャッシュ ストアとしてではなくまたはを使用アプリケーションの状態や、データ キャッシュします。 次のコードでは、アプリケーションの状態を使用するによるクラスを示します。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

`LoadStaticCache()`、仕入先の情報が、アプリケーション変数に格納されている*キー*です。 適切な型として返されます (`Northwind.SuppliersDataTable`) から`GetSuppliers()`です。 アプリケーションの状態を使用して ASP.NET ページの分離コード クラスでアクセスできるときに`Application["key"]`、使用する必要があります、アーキテクチャの`HttpContext.Current.Application["key"]`現在を取得するために`HttpContext`です。

同様に、データ キャッシュは、次のコードに示すように、キャッシュ ストアとして使用できます。


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

時間ベース期限を指定しないと、データ キャッシュにアイテムを追加するには、使用、`System.Web.Caching.Cache.NoAbsoluteExpiration`と`System.Web.Caching.Cache.NoSlidingExpiration`入力パラメーターとして値。 データ キャッシュ s のこの特定のオーバー ロード`Insert`メソッドがオンにして指定できます、*優先度*キャッシュ項目の。 優先順位を使用して、使用可能なメモリが不足しているときに、キャッシュから清掃を行うには、どのような項目を決定します。 ここで、優先度使用`NotRemovable`、清掃このキャッシュ項目に t が勝利したことが保証されます。

> [!NOTE]
> このチュートリアルのダウンロードを実装して、`StaticCache`クラスの静的メンバー変数の方法を使用します。 アプリケーションの状態とデータのキャッシュ方法のコードは、クラス ファイル内のコメントで使用できます。


## <a name="step-4-executing-code-at-application-startup"></a>手順 4: アプリケーションの起動時にコードを実行します。

Web アプリケーションが初めて起動したときに、コードを実行する必要がありますをという名前の特殊なファイルを作成する`Global.asax`です。 このファイルは、アプリケーションで、セッションでのイベント ハンドラーを含めることができ、要求レベルのイベント、およびそれがここで、アプリケーションが開始されるたびに実行されるコードを追加できます。

追加、`Global.asax`を Visual Studio のソリューション エクスプ ローラーで web サイト プロジェクト名を右クリックし、[新しい項目の追加] を選択して web アプリケーションのルート ディレクトリのファイルです。 [新しい項目の追加] ダイアログ ボックスからグローバル アプリケーション クラス項目の種類を選択し、[追加] ボタンをクリックします。

> [!NOTE]
> 既に存在する場合、`Global.asax`プロジェクト、項目の種類は、新しい項目の追加 ダイアログ ボックスは表示されませんグローバル アプリケーション クラス内のファイルです。


[![Global.asax ファイルを Web アプリケーションのルート ディレクトリに追加します。](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**図 3**: 追加、 `Global.asax` Web アプリケーション ルート ディレクトリにファイル ([フルサイズのイメージを表示するをクリックして](caching-data-at-application-startup-cs/_static/image5.png))


既定値`Global.asax`ファイル テンプレートには、サーバー側内の 5 つのメソッドが含まれています。`<script>`タグ。

- **`Application_Start`**web アプリケーションを初めて起動したときに実行します。
- **`Application_End`**アプリケーションがシャット ダウン中と実行されます。
- **`Application_Error`**未処理の例外がアプリケーションに到達するたびに実行します。
- **`Session_Start`**新しいセッションが作成されるときに実行します。
- **`Session_End`**セッションが期限切れまたは破棄すると実行されます。

`Application_Start`イベント ハンドラーがアプリケーション ライフ サイクル中に 1 回だけ呼び出されます。 アプリケーションの開始と、最初に、ASP.NET のリソースがアプリケーションから要求されたアプリケーションが再起動されるまで実行を続けるの内容を変更することによって発生することができますが、`/Bin`フォルダー、変更`Global.asax`、変更、内容、`App_Code`フォルダー、または変更する、`Web.config`原因は他の間でのファイルです。 参照してください[ASP.NET アプリケーションのライフ サイクルの概要](https://msdn.microsoft.com/en-us/library/ms178473.aspx)詳細についてはアプリケーションのライフ サイクルにします。

これらのチュートリアルののみいただくためにコードを追加して、`Application_Start`メソッドでは自由に、他のユーザーを削除します。 `Application_Start`を呼び出すだけで、`StaticCache`クラスの`LoadStaticCache()`は読み込まれて、仕入先の情報をキャッシュするメソッド。


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

すべてが s です! アプリケーションの起動時に、`LoadStaticCache()`メソッドは、BLL から供給業者の情報を取得し、静的メンバー変数に保存 (でを使用して最終的なキャッシュを格納するか、`StaticCache`クラス)。 この動作を確認するためにブレークポイントを設定、`Application_Start`メソッドと、アプリケーションを実行します。 アプリケーションの開始時に、ブレークポイントにヒットすることに注意してください。 ただし、後続の要求も、実行、`Application_Start`メソッドを実行します。


[![Application_Start イベント ハンドラーが実行されていることを確認するブレークポイントを使用してください。](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**図 4**: ことを確認するブレークポイントを使用している、`Application_Start`イベント ハンドラーが実行されている ([フルサイズのイメージを表示するをクリックして](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> ヒットしない場合、`Application_Start`デバッグを開始するときに、ブレークポイントは、アプリケーションが既に開始します。 強制的に変更することで再起動するアプリケーション、`Global.asax`または`Web.config`ファイルし、し、もう一度やり直してください。 単に追加 (または削除できます)、これらのファイルをすばやくアプリケーションを再起動する 1 つの最後の空白行です。


## <a name="step-5-displaying-the-cached-data"></a>手順 5: キャッシュされたデータを表示します。

この時点で、`StaticCache`クラス経由でアクセスできるアプリケーションの起動時にキャッシュされた仕入先データのバージョンには、その`GetSuppliers()`メソッドです。 プレゼンテーション層からこのデータを使用するには、ObjectDataSource を使用またはプログラムによって呼び出すおことができます、`StaticCache`クラスの`GetSuppliers()`ASP.NET ページの分離コード クラスのメソッドです。 ObjectDataSource、GridView コントロールを使用して、キャッシュされた仕入先の情報を表示する見て s を使用できます。

開いて開始、 `AtApplicationStartup.aspx`  ページで、`Caching`フォルダーです。 GridView を設定、デザイナーには、ツールボックスからドラッグその`ID`プロパティを`Suppliers`です。 次に、GridView から s スマート タグを選択してという名前の新しい ObjectDataSource を作成する`SuppliersCachedDataSource`です。 構成を使用する ObjectDataSource、`StaticCache`クラスの`GetSuppliers()`メソッドです。


[![構成、ObjectDataSource StaticCache クラスを使用するには](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`StaticCache`クラス ([フルサイズのイメージを表示するをクリックして](caching-data-at-application-startup-cs/_static/image11.png))


[![GetSuppliers() メソッドを使用して、キャッシュされた仕入先データを取得するには](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**図 6**: を使用して、`GetSuppliers()`キャッシュ仕入先データを取得する方法 ([フルサイズのイメージを表示するをクリックして](caching-data-at-application-startup-cs/_static/image14.png))


ウィザードを完了すると、Visual Studio が自動的に追加 BoundFields 内のデータ フィールドの各`SuppliersDataTable`です。 GridView と ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

図 7 は、ブラウザーで表示したときに、ページを示します。 出力は、同じ BLL s からデータをプルおが`SuppliersBLL`クラスを使用して、`StaticCache`クラスは、アプリケーションの起動時にキャッシュされたとして仕入先データを返します。 ブレークポイントを設定することができます、`StaticCache`クラスの`GetSuppliers()`この動作を確認するメソッド。


[![GridView に、キャッシュされた仕入先データが表示されます。](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**図 7**: GridView に、キャッシュされた仕入先データが表示されます ([フルサイズのイメージを表示するをクリックして](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>概要

すべてのほとんどのデータ モデルには、かなりルックアップ テーブルの形式で実装される通常の静的なデータにはが含まれています。 この情報は、静的でないの理由もなく継続的にデータベースにアクセスするたびにこの情報を表示する必要があります。 さらに、その静的な性質により、データをキャッシュする場合が s、有効期限の必要はありません。 このチュートリアルでは、このようなデータを取得し、データ キャッシュ、アプリケーションの状態、および静的メンバー変数を介した、それをキャッシュする方法を説明しました。 この情報は、アプリケーションの起動時にキャッシュされ、アプリケーション秒の有効期間全体でキャッシュに残ります。

このチュートリアルを過去の 2 つのお ve アプリケーション秒の有効期間の間のデータのキャッシュだけでなく切れの時間ベースを使用して説明しました。 データベースのデータをキャッシュする場合は、時間ベースの有効期限がより小さい理想的なあります。 キャッシュを定期的にフラッシュするではなく、基になるデータベースのデータが変更されたときにのみ、キャッシュされた項目を削除する最適ながあります。 この理想は、[次へ]、チュートリアルについて見ていきましょう SQL キャッシュ依存関係を使用することができます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよび Zack Jones がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](caching-data-in-the-architecture-cs.md)
[次へ](using-sql-cache-dependencies-cs.md)
