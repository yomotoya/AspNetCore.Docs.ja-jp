---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: "ASP.NET 4 Web アプリケーションに Entity Framework 4.0 とパフォーマンスを最大化 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 40a53a110115e5f6342d2a97d21b64470450fd3c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>ASP.NET 4 Web アプリケーションに Entity Framework 4.0 とパフォーマンスを最大化
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。 チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。


前のチュートリアルでは、同時実行の競合を処理する方法を説明します。 このチュートリアルでは、Entity Framework を使用する ASP.NET web アプリケーションのパフォーマンスを向上させるためのオプションを使用します。 パフォーマンスを最大化するため、またはパフォーマンスの問題を診断するためのいくつかの方法を学習します。

次のセクションに表示される情報をさまざまなシナリオで役に立つ可能性があります。

- 関連するデータを効率的に読み込みます。
- ビュー ステートを管理します。

次のセクションに表示される情報は、個々 のクエリを現在のパフォーマンス問題がある場合に役立ちます。 可能性があります。

- 使用して、`NoTracking`マージ オプションです。
- LINQ クエリを事前にコンパイルします。
- データベースに送信されるクエリ コマンドを確認します。

次のセクションに表示される情報は非常に大きなデータ モデルを持つアプリケーションに役立つ可能性のあります。

- ビューを事前に生成します。

> [!NOTE]
> Web アプリケーションのパフォーマンスは、要求と応答のデータのサイズ、データベース クエリ、キューできるように、サーバー要求の数、および任意の効率もをサービスにどの程度の速度の速度などを含む、さまざまな要因の影響を受けたクライアント スクリプト ライブラリが使用する場合があります。 パフォーマンスは、アプリケーションで重要なテストまたは経験は、アプリケーションのパフォーマンスが十分ではないことを示す場合や、パフォーマンス チューニングの通常のプロトコルに従ってください。 パフォーマンスのボトルネックの発生場所を確認するを測定し、アプリケーション全体のパフォーマンスに大きな影響を与える領域をアドレスします。
> 
> このトピックを具体的には ASP.NET では、Entity Framework のパフォーマンスを向上することができます可能性のある方法で主に焦点を当てています。 修正候補をここでは、データ アクセスでは、アプリケーションでパフォーマンスのボトルネックのいずれかの判断した場合に便利です。 ここで説明したメソッドと見なすべき述べたように、点を除いて&quot;ベスト プラクティス&quot;一般-これらの多くは例外的な状況でのみ、またはアドレス非常に特定の種類のパフォーマンスのボトルネックを適切な。


チュートリアルを開始するには、Visual Studio を起動し、前のチュートリアルで作業していた Contoso 大学 web アプリケーションを開きます。

## <a name="efficiently-loading-related-data"></a>効率的に関連するデータの読み込み

いくつかの方法が、Entity Framework が、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*です。 エンティティが読み込まれると、関連データが取得されていません。 ただし、ナビゲーション プロパティにアクセスしようとすると、最初にそのナビゲーション プロパティに必要なデータが自動的に取得します。 これは、結果、データベースに送信される複数のクエリで — と 1 つ、エンティティ自体に関連したエンティティのデータを毎回を取得する必要があります。 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*一括読み込み*です。 エンティティが読み込まれると、それに伴う関連データを取得します。 通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。 一括読み込みを使用して指定する、`Include`メソッドも、既に確認したこれらのチュートリアルです。

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *明示的な読み込み*です。 これは、似ていますが、遅延読み込みを明示的にデータを取得する、関連するコードです。ナビゲーション プロパティにアクセスするときに自動的に発生しません。 使用して手動で関連するデータを読み込む、`Load`のコレクション、またはするナビゲーション プロパティのメソッドを使用して、 `Load` reference プロパティの 1 つのオブジェクトを保持するプロパティのメソッドです。 (たとえばを呼び出す、`PersonReference.Load`を読み込む方法、`Person`のナビゲーション プロパティ、`Department`エンティティです)。

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

すぐに、プロパティの値を取得する、ので遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*です。

遅延読み込みは、デザイナーによって生成されたオブジェクト コンテキストの既定の動作です。 開く場合、 *SchoolModel.Designer.cs*ファイルは、オブジェクト コンテキスト クラスを定義する、できたら、次の 3 つのコンス トラクター メソッドおよびそれらの各に次のステートメントが含まれています。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

一般に、わかっている場合必要関連データのすべてのエンティティを取得した、一括読み込みでは、最高のパフォーマンスをデータベースに送信する 1 つのクエリが通常エンティティごとに取得される個別のクエリよりも効率的であるためです。 一方で、頻度、エンティティのナビゲーション プロパティにアクセスする必要がある場合、または小さなに対してのみ一連のエンティティ、遅延読み込みまたは明示的な読み込み可能性がありますより効率的な一括読み込みする必要がありますよりも多くのデータを取得します。

Web アプリケーションで遅延読み込み可能性があります比較的小さな値のまま、ページのレンダリングをオブジェクト コンテキストに接続がないと、ブラウザーで関連するデータの必要性に影響を与えるユーザーのアクションが実行します。 その一方で、わかっている場合に、データ バインド コントロールを通常データ一般的には、最高の一括読み込みまたはに基づいて遅延読み込みを選択して、必要とする各シナリオで適切なは新機能です。

さらに、データ バインドされたコントロールは、オブジェクト コンテキストが破棄された後に、エンティティ オブジェクトを使用できます。 その場合は、遅延読み込みナビゲーション プロパティには失敗します。 表示されるエラー メッセージは明らかです。&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource`コントロールは、既定では遅延読み込みを無効になります。 `ObjectDataSource`コントロールの現在のチュートリアルを使用している (またはページのコードから、オブジェクト コンテキストにアクセスするかどうか)、いくつかの方法を遅延することができますが読み込みを既定で無効にします。 オブジェクト コンテキストをインスタンス化するときに、それが無効にできます。 たとえばのコンス トラクター メソッドに次の行を追加することができます、`SchoolRepository`クラス。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso 大学アプリケーションにすれば、オブジェクト コンテキストに自動的にこのプロパティは、コンテキストがインスタンス化されるたびに設定する必要があるないように、遅延読み込みを無効にします。

開く、 *SchoolModel.edmx*データ モデル、デザイン画面をクリックして設定して、プロパティ ペインで、**遅延読み込み有効になっている**プロパティを`False`です。 保存して、データ モデルを閉じます。

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>ビュー ステートを管理します。

更新機能を提供するためにページが表示されるときに、ASP.NET web ページは、エンティティの元のプロパティ値を格納する必要があります。 ポストバック コントロールを処理中に、エンティティの元の状態を再作成でき、エンティティを呼び出す`Attach`変更を適用して、呼び出しの前にメソッド、`SaveChanges`メソッドです。 既定では、ASP.NET Web フォーム データ コントロールは、元の値を格納するのにビュー ステートを使用します。 ただし、ビュー状態に影響する可能性、パフォーマンス ブラウザーとの間に送信されるページのサイズを大幅に増加することが非表示のフィールドに格納されているためです。

このチュートリアルが詳しくは、このトピックに移動しないようにビュー状態、またはセッション状態などの選択肢を管理するための手法は対象の Entity Framework がありません。 詳細については、チュートリアルの最後に、リンクを参照してください。

ただし、バージョン 4 の ASP.NET により、新しい Web フォーム アプリケーションのすべての ASP.NET 開発者が注意する必要があるビュー ステートに作業を行う:`ViewStateMode`プロパティです。 ページまたはコントロールのレベルでこの新しいプロパティを設定することができ、既定では、ページのビュー ステートを無効にし、必要なコントロールに対してのみ有効にすることができます。

アプリケーションのパフォーマンスが重要な場合をお勧めを常に、ページ レベルでのビュー ステートを無効にし、それが必要なコントロールに対してのみ有効にします。 Contoso 大学ページでの表示状態のサイズは、このメソッドにより大幅に低下はありませんが、動作を理解すること、行うこと*Instructors.aspx*ページ。 そのページには含む、多くのコントロールが含まれています、`Label`ビューステートが無効になっているコントロール。 このページ上のコントロールのいずれも実際には、ビュー状態が有効にする必要があります。 (、`DataKeyNames`のプロパティ、`GridView`コントロールがポストバック間で維持する必要がありますの状態を指定しますが、これらの値がコントロールの状態は、影響を与えませんに保持される、`ViewStateMode`プロパティです)。

`Page`ディレクティブと`Label`コントロール マークアップ現在次の例のようになります。

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

次の変更を行います。

- 追加`ViewStateMode="Disabled"`を`Page`ディレクティブです。
- 削除`ViewStateMode="Disabled"`から、`Label`コントロール。

マークアップでは、次の例がようになります。

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

すべてのコントロール ビュー ステートが無効になります。 行うには必要なのは、後では、ビュー ステートを使用する必要があるコントロールを追加する場合、`ViewStateMode="Enabled"`そのコントロールの属性です。

## <a name="using-the-notracking-merge-option"></a>NoTracking マージ オプションを使用します。

オブジェクト コンテキストでは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定の追跡もそのオブジェクトの状態マネージャーを使用してこれらのエンティティ オブジェクト。 この追跡データはキャッシュとして機能し、エンティティを更新するときに使用します。 Web アプリケーションには、通常はコンテキストの有効期間が短いオブジェクト インスタンスがあるためクエリは多くの場合、そこにエンティティのいずれかが再度使用する前に、オブジェクト コンテキストを読み取ることが破棄されるため、追跡する必要があるデータを返すか、更新します。

Entity Framework ではコンテキストによってオブジェクトが設定してエンティティ オブジェクトを追跡するかどうかを指定できます、*マージ オプション*です。 エンティティ セットまたは個々 のクエリのマージ オプションを設定することができます。 エンティティ セットに対して設定すると場合、は、そのエンティティ セットに対して作成されるすべてのクエリの既定のマージ オプションを設定することを意味します。

追跡されていないためマージ オプションを設定することができます、リポジトリからアクセスするエンティティ セットのいずれかの必要となる Contoso 大学アプリケーション`NoTracking`のリポジトリ クラスのオブジェクト コンテキストのインスタンスを作成するときにそれらのエンティティ セット。 (このチュートリアルではマージ オプションを設定しませんあるアプリケーションのパフォーマンスに大きな影響が及ぶに注意してください。 `NoTracking`オプションは、特定のデータ量の多いシナリオでのみ、監視可能なパフォーマンスが向上する可能性があります)。

DAL フォルダーで開く、 *SchoolRepository.cs*ファイルし、エンティティ セット、リポジトリにアクセスするにはマージ オプションを設定するコンス トラクター メソッドを追加します。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>コンパイル済み LINQ クエリ

初めて、Entity Framework の有効期間内の Entity SQL クエリを実行するとき、指定された`ObjectContext`インスタンス、しばらくの間にかかるクエリをコンパイルします。 以降のクエリの実行が大幅に短縮されるので、コンパイルの結果がキャッシュされます。 LINQ クエリは、クエリが実行されるたびに、クエリのコンパイルに必要な作業の一部を行うことを除き、同様のパターンに従います。 つまり、LINQ クエリの既定ですべてのコンパイルの結果はキャッシュされます。

オブジェクト コンテキストの有効期間で繰り返し実行すると予想される LINQ クエリを使っている場合は、すべての結果を初めての LINQ クエリの実行がキャッシュに保存するコンパイルの原因となったコードを記述できます。

具体的なとしてがこれを行うという 2 つの`Get`内のメソッド、`SchoolRepository`うちの 1 つは、すべてのパラメーターを受け取らないクラス (、`GetInstructorNames`メソッド)、いずれかのパラメーターを必要として (、`GetDepartmentsByAdministrator`メソッド)。 これらのメソッド目立つようになりました実際にする必要はありません LINQ クエリではありませんので、コンパイルします。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

ただし、コンパイル済みクエリを試すことができます、ように、これらを次の LINQ クエリとして記述されていた場合とを続行します。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

これらのメソッドでコードを変更すると、何上に示したがあり、続行する前に動作することを確認するアプリケーションを実行する可能性があります。 それらのコンパイル済みのバージョンの作成時に、次の手順のすぐです。

クラス ファイルを作成、 *DAL*フォルダーで、名前を付けます*SchoolEntities.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

このコードでは、自動的に生成されたオブジェクト コンテキスト クラスを拡張する部分クラスを作成します。 部分クラスには使用して 2 つのコンパイル済み LINQ クエリが含まれています、`Compile`のメソッド、`CompiledQuery`クラスです。 クエリの呼び出しに使用できるメソッドも作成されます。 保存して、このファイルを閉じます。

次に、 *SchoolRepository.cs*、既存を変更する`GetInstructorNames`と`GetDepartmentsByAdministrator`リポジトリ内のメソッドはクラス、コンパイル済みクエリを呼び出すことができるようにします。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

実行、 *Departments.aspx*以前と同じように動作することを確認します。 `GetInstructorNames`メソッドは、管理者のドロップダウン リストに表示するために呼び出されますと`GetDepartmentsByAdministrator`をクリックすると、メソッドが呼び出されます**更新**インストラクターに 1 つ以上の管理者がないことを確認するのには部門。

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

パフォーマンスが多少向上ため、これを行う方法を表示するには、のみ Contoso 大学アプリケーションでは、事前にコンパイルされたクエリをしています。 LINQ クエリをプリコンパイルするはコードの複雑さのレベルに追加してください。 を実際には、アプリケーションでパフォーマンスのボトルネックを表すクエリに対してのみ実行することを確認します。

## <a name="examining-queries-sent-to-the-database"></a>データベースに送信されるクエリを確認します。

パフォーマンスの問題を調査している場合は、場合によっては、Entity Framework をデータベースに送信する正確な SQL コマンドを知っておくと便利です。 作業している場合、`IQueryable`オブジェクト、これを行う方法の 1 つを使用して、`ToTraceString`メソッドです。

*SchoolRepository.cs*、内のコードを変更、`GetDepartmentsByName`メソッドを次の例。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments`に変数をキャストする必要があります、`ObjectQuery`のみため、型、`Where`メソッド、前の行の最後に作成、`IQueryable`オブジェクト; せず、`Where`メソッド、キャストする必要はありません。

ブレークポイントを設定、`return`行、および実行し、 *Departments.aspx*デバッガー内のページです。 ブレークポイントをヒットしたときに確認、`commandText`に変数が、**ローカル**ウィンドウとテキスト ビジュアライザーを使用する (で虫眼鏡、**値**列)、にその値を表示する**テキスト ビジュアライザー**ウィンドウです。 このコードによって生成されるすべての SQL コマンドを確認できます。

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

代わりに、Visual Studio Ultimate の IntelliTrace の機能は、コードを変更またはもブレークポイントを設定する必要がない Entity Framework によって生成された SQL コマンドを表示する方法を提供します。

> [!NOTE]
> Visual Studio Ultimate がある場合にのみ、次の手順を行うことができます。


復元元のコードに、`GetDepartmentsByName`メソッド、および実行し、 *Departments.aspx*デバッガー内のページです。

Visual Studio で、選択、**デバッグ**メニュー、 **IntelliTrace**、し**IntelliTrace イベント**です。

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

**IntelliTrace**ウィンドウで、をクリックして**すべて中断**です。

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace**ウィンドウには、最近のイベントの一覧が表示されます。

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

クリックして、 **ADO.NET**行です。 これは、コマンド テキストを表示するに展開されます。

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

コマンド テキスト文字列全体をからクリップボードにコピーすることができます、**ローカル**ウィンドウです。

データベースが複数のテーブル、リレーションシップ、およびより単純な列が使用するいると仮定`School`データベース。 すべての情報を収集するクエリに必要な 1 つあります`Select`ステートメントが複数含まれている`Join`句が複雑すぎるため、作業を効率的になります。 その場合は、クエリを簡潔に明示的な読み込みへの読み込み eager から切り替えることができます。

たとえば、内のコードを変更してみてください、`GetDepartmentsByName`メソッド*SchoolRepository.cs*です。 現在することでメソッドを持つオブジェクト クエリ`Include`ためのメソッド、`Person`と`Courses`ナビゲーション プロパティ。 置換、`return`次の例のように、明示的な読み込みを実行するコードを使用してステートメント。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

実行、 *Departments.aspx*デバッガーでページを確認、 **IntelliTrace**する前にウィンドウをもう一度がでした。 ここで、前に 1 つのクエリがあった場合は、それらの長いシーケンス参照してください。

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

1 つ目のクリックして**ADO.NET**起こった複雑なクエリを表示する行が前に表示します。

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

部門からのクエリは、単純なになった`Select`のないクエリ`Join`元によって返される各部門の 2 つのクエリのセットを使用して、句が、これは、関連 courses に、管理者を取得する個別のクエリに続く、クエリ。

> [!NOTE]
> 遅延のままにする場合の遅延読み込みを読み込みを有効にすると、ここに表示される、繰り返される多くの場合、同じクエリを使用して、パターンがあります。 回避する通常されるパターンでは、主テーブルのすべての行に関連するデータを遅延読み込みです。 1 つの結合クエリが複雑すぎて効率的ことを確認できたので、しない限り、通常ことができますを一括読み込みを使用するプライマリのクエリを変更することでこのような場合にパフォーマンスを向上させるためにします。


## <a name="pre-generating-views"></a>ビューを事前に生成します。

ときに、`ObjectContext`オブジェクトが、新しいアプリケーション ドメインで最初に作成、Entity Framework には、一連のデータベースへのアクセスに使用されるクラスが生成されます。 これらのクラスと呼ばれます*ビュー*、非常に大きなデータ モデルがあれば、これらのビューを生成するが遅れる原因、web サイトのページの最初の要求に応えて、新しいアプリケーション ドメインが初期化されるとします。 実行時ではなく、コンパイル時にビューを作成することでこの初回要求の遅延時間を減らすことができます。

> [!NOTE]
> アプリケーションは、極端に大きなデータ モデルを持っていない場合、または大きなデータ モデルには、IIS のリサイクル後に最初のページ要求のみに影響するパフォーマンスの問題に関する問題がない場合は、このセクションを省略できます。 ビューをインスタンス化するたびに、作成は行われない、`ObjectContext`オブジェクト、ビューは、アプリケーション ドメインでキャッシュされるためです。 そのため、頻繁に IIS でアプリケーションをリサイクルしている場合を除き、少数のページ要求が事前に生成したビューからも有効。


使用してビューを事前に生成することができます、 *EdmGen.exe*コマンド ライン ツールまたはを使用して、*テキスト テンプレート変換ツールキット*(T4) テンプレートです。 このチュートリアルでは、T4 テンプレートを使用します。

*DAL*フォルダーを使用してファイルを追加、**テキスト テンプレート**テンプレート (下では、**全般**内のノード、**インストールされたテンプレート**一覧)、名前を付けます*SchoolModel.Views.tt*です。 ファイル内の既存のコードを次のコードに置き換えます。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

このコード生成のビュー、 *.edmx*は、テンプレートと同じフォルダー内にあるテンプレート ファイルとして同じ名前を持つファイルです。 たとえば、次のテンプレートは、ファイルの名前は*SchoolModel.Views.tt*、という名前のデータ モデル ファイルが検索されます*SchoolModel.edmx*です。

内のファイルを右クリックし、ファイルを保存**ソリューション エクスプ ローラー**選択**カスタム ツールの実行**です。

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio の名前は、ビューを作成するコード ファイルを生成する*SchoolModel.Views.cs*テンプレートに基づきます。 (お気付きかもしれませんを選択する前に、コード ファイルの生成**カスタム ツールの実行**テンプレート ファイルを保存するとすぐに、します)。

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

ここで、アプリケーションを実行し、以前と同じように動作することを確認できます。

事前に生成されたビューの詳細については、次のリソースを参照してください。

- [方法: 事前生成クエリ パフォーマンスを向上させるビュー](https://msdn.microsoft.com/library/bb896240.aspx) MSDN web サイトのです。 使用する方法について説明します、`EdmGen.exe`ビューを事前に生成するコマンド ライン ツールです。
- [プリコンパイル済み/前 generated ビューと、Entity Framework 4 のパフォーマンスの分離](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx)on Windows Server AppFabric の Customer Advisory Team ブログ。

これは、Entity Framework を使用する ASP.NET web アプリケーションのパフォーマンス向上の概要を完了します。 詳細については、次のリソースを参照してください。

- [パフォーマンスに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) MSDN web サイトです。
- [Entity Framework チームのブログの投稿のパフォーマンスに関連する](https://blogs.msdn.com/b/adonet/archive/tags/performance/)です。
- [EF マージ オプションおよびコンパイル済みクエリ](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx)です。 コンパイル済みクエリとマージの予期しない動作を説明するブログの投稿などのオプション`NoTracking`です。 コンパイル済みクエリを使用するか、アプリケーションでのマージ オプションの設定を操作する場合は、この最初の読み取り。
- [フレームワークに関連するエンティティが、データとモデリングの Customer Advisory Team ブログの投稿](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/)です。 コンパイル済みクエリを使用して Visual Studio 2010 のプロファイラーをパフォーマンスの問題の検出の投稿が含まれます。
- [非常に複雑なクエリのパフォーマンスの向上に関するアドバイスを entity Framework フォーラムのスレッド](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6)です。
- [ASP.NET 状態管理に関する推奨事項](https://msdn.microsoft.com/library/z1hkazw7.aspx)です。
- [Entity Framework と、ObjectDataSource を使用: カスタム ページング](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx)です。 ブログの投稿でページングを実装する方法を説明するこれらのチュートリアルで作成した ContosoUniversity アプリケーション上に構築される、 *Departments.aspx*ページ。

次のチュートリアルでは、バージョン 4 で導入された Entity Framework に重要な拡張機能のいくつかは確認します。

>[!div class="step-by-step"]
[前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[次へ](what-s-new-in-the-entity-framework-4.md)
