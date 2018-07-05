---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: ASP.NET 4 Web アプリケーションで Entity Framework 4.0 でパフォーマンスを最大化 |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 2c5aa6c8a512ccd2ff4b94f5a8a60b31aaa39fc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828281"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>ASP.NET 4 Web アプリケーションで Entity Framework 4.0 でパフォーマンスの最大化
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。 チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。


前のチュートリアルでは、同時実行の競合を処理する方法を説明しました。 このチュートリアルでは、Entity Framework を使用する ASP.NET web アプリケーションのパフォーマンスを向上させるためのオプションを使用します。 パフォーマンスを最大化するため、またはパフォーマンスの問題を診断するためのいくつかの方法について説明します。

次のセクションで説明する情報は、幅広いシナリオで役に立ちますする可能性があります。

- 関連するデータを効率的に読み込みます。
- ビュー ステートを管理します。

個々 のクエリを現在のパフォーマンスの問題がある場合に役立ちます。 以下のセクションで説明する情報があります。

- 使用して、`NoTracking`マージ オプションです。
- LINQ クエリを事前にコンパイルします。
- データベースに送信されたクエリ コマンドを確認します。

次のセクションで説明する情報は非常に大きなデータ モデルを持つアプリケーションに役立つ可能性があります。

- ビューを事前に生成します。

> [!NOTE]
> Web アプリケーションのパフォーマンスは、要求と応答データのサイズのデータベースにクエリがキューに入れること、サーバー要求の数、およびどの程度の速度、および任意の効率も処理できる速度などを含む、さまざまな要因の影響を受けるクライアント スクリプト ライブラリを使用する場合があります。 パフォーマンスがアプリケーションでは、重要な場合、またはテストまたは経験によってアプリケーションのパフォーマンスが満足されていないことが表示されている場合は、パフォーマンス チューニングの通常のプロトコルに従ってください。 パフォーマンスのボトルネックの発生場所を確認する測定し、アプリケーション全体のパフォーマンスに大きな影響を与える点を解決します。
> 
> このトピックを具体的には ASP.NET で Entity Framework のパフォーマンスを向上することができます可能性のある方法で主に重点を置いています。 ここで、推奨事項は、データ アクセスでは、アプリケーションでパフォーマンスのボトルネックのいずれかの判断した場合に便利です。 ここで説明するメソッドと見なすべきではないように、点を除いて&quot;ベスト プラクティス&quot;一般に、これらの多くは例外的な状況でのみ、または非常に特定の種類をパフォーマンスのボトルネックのアドレスを適切な。


チュートリアルを開始するには、Visual Studio を起動し、前のチュートリアルで作業していた Contoso University web アプリケーションを開きます。

## <a name="efficiently-loading-related-data"></a>関連データの読み込みを効率的に

いくつかの方法が、Entity Framework がエンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*。 エンティティが最初に読み込まれるときに、関連データは取得されません。 ただし、ナビゲーション プロパティに初めてアクセスしようとすると、そのナビゲーション プロパティに必要なデータが自動的に取得されます。 これは、データベースに送信する複数のクエリ結果: 1 つ、エンティティ自体は、1 つに関連したエンティティのデータを毎回を取得する必要があります。 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*一括読み込み*。 エンティティが読み取られるときに、関連データがエンティティと共に取得されます。 これは通常、必要なデータをすべて取得する 1 つの結合クエリになります。 一括読み込みを使用して指定する、`Include`メソッドと、既に確認したこれらのチュートリアル。

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *明示的読み込み*。 はコードで、関連するデータを明示的に取得する点を除いて、遅延読み込みと同様にはナビゲーション プロパティにアクセスするときに自動的に発生しません。 使用して手動で関連データを読み込む、`Load`するか、コレクション ナビゲーション プロパティのメソッドを使用して、`Load`参照プロパティの 1 つのオブジェクトを保持するプロパティのメソッド。 (たとえばを呼び出す、`PersonReference.Load`を読み込むメソッド、`Person`のナビゲーション プロパティ、`Department`エンティティ)。

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

すぐに、プロパティの値を取得、ため、遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*します。

遅延読み込みは、デザイナーによって生成されたオブジェクト コンテキストの既定の動作です。 開く場合、 *SchoolModel.Designer.cs*ファイル、オブジェクト コンテキスト クラスを定義する、3 つのコンス トラクター メソッドが見つかりますおよびそれぞれに、次のステートメントが含まれています。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

一般に、わかっている場合必要関連データのすべてのエンティティを取得した、一括読み込みは、最適なパフォーマンスをデータベースに送信する 1 つのクエリが通常は分離したクエリの各エンティティを取得するよりも効率的であるためです。 その一方、頻度、エンティティのナビゲーション プロパティにアクセスする必要がある場合または小さなに対してのみは、一括読み込みでは、必要以上に多くのデータを取得するためエンティティ、遅延読み込みと明示的な読み込みのセットより効率的ですあります。

Web アプリケーションで遅延読み込みできなかった可能性があります比較的小さな値の関連データの必要性に影響を与えるユーザー アクションがページを表示するオブジェクト コンテキストへの接続がないと、ブラウザーで実行します。 その一方で、コントロールを databind を通常データを把握し、一括読み込みまたはに基づいて遅延読み込みを選択する最適なので、通常はときに各シナリオで適切なは。

さらに、データ バインド コントロールは、オブジェクト コンテキストが破棄された後に、エンティティ オブジェクトを使用可能性があります。 その場合は、遅延読み込みのナビゲーション プロパティの試行は失敗します。 表示されるエラー メッセージは明確です。 &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource`コントロールは、既定で遅延読み込みを無効にします。 `ObjectDataSource`コントロールの現在のチュートリアルを使用している (または、オブジェクト コンテキストは、ページ コードからアクセスするかどうか)、いくつかの方法を最短に行うことができます読み込みを既定で無効にします。 オブジェクト コンテキストをインスタンス化するときに、それが無効にできます。 たとえば、次の行を追加のコンス トラクター メソッドに、`SchoolRepository`クラス。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso University アプリケーションの場合、オブジェクト コンテキストが自動的にこのプロパティは、コンテキストをインスタンス化されるたびに設定する必要があるないように、遅延読み込みを無効にすることになります。

開く、 *SchoolModel.edmx*データがモデル化し、デザイン画面で、をクリックし、[プロパティ] ウィンドウで次のように設定します。、**遅延読み込みを有効に**プロパティを`False`します。 保存して、データ モデルを閉じます。

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>ビュー ステートを管理します。

更新機能を提供するために、ページが表示されるときに、ASP.NET web ページは、エンティティの元のプロパティ値を格納する必要があります。 ポストバック コントロールを処理中に、エンティティの元の状態を再作成して、エンティティの呼び出し`Attach`メソッドの変更を適用して、呼び出しの前に、`SaveChanges`メソッド。 既定では、ASP.NET Web フォームのデータ コントロールは、元の値を格納するのにビュー ステートを使用します。 ただし、ビュー ステートに影響する可能性、パフォーマンスとブラウザーの間に送信されるページのサイズを大幅に増加することが非表示フィールドに格納されているためです。

このチュートリアルは、このトピックの詳細に移動しないようにビュー状態、またはセッション状態などの代替手段を管理するための手法は一意で、Entity Framework にはありません。 詳細については、チュートリアルの最後に、リンクを参照してください。

ただし、ASP.NET のバージョン 4 が Web フォーム アプリケーションのすべての ASP.NET 開発者が注意する必要があるビュー ステートを使用した作業の新しい方法を提供します。、`ViewStateMode`プロパティ。 ページまたはコントロールのレベルでこの新しいプロパティを設定することができ、既定では、ページのビューステートを無効にし、必要なコントロールに対してのみ有効にできます。

アプリケーションのパフォーマンスが重要では、ことをお勧めは常にページ レベルでのビュー ステートを無効にしてが必要なコントロールに対してのみ有効にするのには。 Contoso University のページのビュー ステートのサイズは、このメソッドによって大幅に低下はありませんが、そのしくみについては、する仕事を*Instructors.aspx*ページ。 そのページには含む、多くのコントロールが含まれています、`Label`ビューステートが無効になっているコントロール。 このページのコントロールのいずれも実際には、ビュー状態が有効にする必要があります。 (、`DataKeyNames`のプロパティ、`GridView`コントロールがポストバック間で保持する必要がある状態を指定しますが、これらの値がコントロールの状態は、影響を与えませんに保持される、`ViewStateMode`プロパティです)。

`Page`ディレクティブと`Label`コントロール マークアップには、次の例では、現在に似ています。

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

次の変更を加えます。

- 追加`ViewStateMode="Disabled"`を`Page`ディレクティブ。
- 削除`ViewStateMode="Disabled"`から、`Label`コントロール。

マークアップでは、次の例では、今すぐようになります。

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

すべてのコントロールは、ビュー ステートが無効になりました。 含める行う必要があるすべてが後では、ビュー ステートを使用する必要があるコントロールを追加する場合、`ViewStateMode="Enabled"`そのコントロールの属性。

## <a name="using-the-notracking-merge-option"></a>NoTracking マージ オプションを使用します。

オブジェクト コンテキストでは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定の追跡も、オブジェクトの状態マネージャーを使用してそれらのエンティティ オブジェクト。 この追跡データはキャッシュとして機能し、エンティティを更新するときに使用されます。 Web アプリケーションには、通常は時間が短いオブジェクト コンテキストのインスタンスがあるためクエリは多くの場合、もう一度そこにエンティティのいずれかの使用前にそれらを読み取るオブジェクト コンテキストが破棄されるため、追跡する必要がないデータを返すまたは更新します。

Entity framework では、オブジェクト コンテキストが設定してエンティティ オブジェクトを追跡するかどうかを指定できます、*マージ オプション*します。 個々 のクエリに対してまたはのエンティティ セットのマージ オプションを設定することができます。 場合は、エンティティ セットに対して設定すると、そのエンティティ セットに対して作成されるすべてのクエリの既定のマージ オプションを設定することを意味します。

Contoso University のアプリケーションのエンティティ セット マージ オプションを設定できるように、リポジトリからのアクセスのいずれかの追跡は必要`NoTracking`のリポジトリ クラスにオブジェクト コンテキストをインスタンス化するときにこれらのエンティティ セット。 (このチュートリアルでマージ オプションを設定しませんがあるアプリケーションのパフォーマンスに大きな影響が及ぶに注意してください。 `NoTracking`オプションは、特定のデータ量の多いシナリオでのみ、監視可能なパフォーマンスが向上する可能性があります)。

DAL フォルダーで開き、 *SchoolRepository.cs*ファイルを開き、エンティティ セットがリポジトリにアクセスするは、マージ オプションを設定するコンス トラクター メソッドを追加します。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>コンパイル済み LINQ クエリ

Entity Framework の有効期間内の Entity SQL クエリを実行する最初の時間を指定した`ObjectContext`インスタンス、時間がかかるクエリをコンパイルします。 つまり、クエリの後続の実行が大幅に短縮するコンパイルの結果がキャッシュされます。 LINQ クエリは、クエリが実行されるたびに、クエリのコンパイルに必要な作業の一部が完了する点を除いて、同様のパターンに従います。 つまり、LINQ クエリでは、既定ですべてのコンパイルの結果のキャッシュされます。

オブジェクト コンテキストの有効期間で繰り返し実行すると思われる LINQ クエリがある場合は、すべてのキャッシュを初めて LINQ クエリを実行するコンパイルの結果を原因となるコードを記述できます。

例として、これを行う 2 つの`Get`メソッド、`SchoolRepository`任意のパラメーターをとらないうちの 1 つのクラス (、`GetInstructorNames`メソッド)、いずれかのパラメーターを必要として (、`GetDepartmentsByAdministrator`メソッド)。 これらのメソッド目立つようになりました実際にする必要はありませんので LINQ クエリをコンパイルします。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

ただし、コンパイル済みクエリを試すことができるように、これらを次の LINQ クエリとして記述されている必要がある場合とを続行します。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

前に示したし、続行する前に動作することを確認するアプリケーションの実行内容が、これらのメソッドでコードを変更できます。 次の手順はそれらのコンパイル済みのバージョンの作成に取りかかります。

クラス ファイルを作成、 *DAL*フォルダー、という名前を付けます*SchoolEntities.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

このコードは、自動的に生成されたオブジェクト コンテキスト クラスを拡張する部分クラスを作成します。 部分クラスには使用して 2 つのコンパイル済み LINQ クエリが含まれています、`Compile`のメソッド、`CompiledQuery`クラス。 クエリの呼び出しに使用できるメソッドも作成します。 保存して、このファイルを閉じます。

次に、 *SchoolRepository.cs*、既存の変更`GetInstructorNames`と`GetDepartmentsByAdministrator`コンパイル済みクエリを呼び出すことができるように、リポジトリ内のメソッドがクラスします。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

実行、 *Departments.aspx*前に、と同様に動作することを確認します。 `GetInstructorNames`管理者のボックスの一覧を設定するためにメソッドが呼び出されると`GetDepartmentsByAdministrator`をクリックすると、メソッドが呼び出された**Update**講師が 1 つ以上の管理者がないことを確認するには部門。

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Contoso University のアプリケーション パフォーマンスを向上させることはある程度までためではなく、これを行う方法について説明するだけでは、コンパイル済みクエリをしています。 LINQ クエリをプリコンパイルする、複雑さのレベルをコードに追加は、実際には、アプリケーションでパフォーマンスのボトルネックを表すクエリに対してのみ行うようにします。

## <a name="examining-queries-sent-to-the-database"></a>データベースに送信されるクエリの調査

パフォーマンスの問題を調査しているときに、Entity Framework がデータベースに送信する正確な SQL コマンドを把握する便利です。 使用している場合、`IQueryable`オブジェクト、これを行う方法の 1 つが使用するには、`ToTraceString`メソッド。

*SchoolRepository.cs*、コードを変更、`GetDepartmentsByName`メソッドを次の例。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments`に変数をキャストする必要があります、`ObjectQuery`型のためにのみ、 `Where` 、前の行の最後のメソッドを作成、`IQueryable`オブジェクト; せず、`Where`メソッドのキャストは必要ありません。

ブレークポイントを設定、`return`行、および実行し、 *Departments.aspx*デバッガーでのページ。 ブレークポイントがヒットしたら、確認、`commandText`変数、**ローカル**ウィンドウとテキスト ビジュアライザーを使用して (虫眼鏡の**値**列)、にその値を表示する**テキスト ビジュアライザー**ウィンドウ。 このコードによって生成される SQL コマンド全体を確認できます。

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

別の方法としては、Visual Studio Ultimate で IntelliTrace の機能は、コードを変更またはもブレークポイントを設定する必要がない Entity Framework によって生成された SQL コマンドを表示する方法を提供します。

> [!NOTE]
> Visual Studio Ultimate がある場合にのみ、次の手順を実行することができます。


復元元のコードに、`GetDepartmentsByName`メソッド、および実行し、 *Departments.aspx*デバッガーでのページ。

Visual Studio で、選択、**デバッグ**メニュー、 **IntelliTrace**、し**IntelliTrace イベント**します。

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

**IntelliTrace**ウィンドウで、をクリックして**すべて中断**します。

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace**ウィンドウには、最近のイベントの一覧が表示されます。

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

をクリックして、 **ADO.NET**行。 コマンド テキストを表示する展開します。

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

コマンド全体のテキスト文字列をからクリップボードにコピーすることができます、**ローカル**ウィンドウ。

複数のテーブル、リレーションシップ、およびより単純な列を持つデータベースを使用するいると仮定`School`データベース。 1 つで必要なすべての情報を収集するクエリを見つけることがあります`Select`複数を含むステートメント`Join`句が複雑すぎて効率的に作業になります。 その場合は一括クエリを簡単に明示的な読み込みに読み込みから切り替えることができます。

たとえば、でコードを変更してみてください、`GetDepartmentsByName`メソッド*SchoolRepository.cs*します。 メソッドを持つオブジェクト クエリがあることに現在`Include`のメソッド、`Person`と`Courses`ナビゲーション プロパティ。 置換、`return`ステートメントを次の例に示すように、明示的な読み込みを実行するコード。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

実行、 *Departments.aspx*デバッガーでページ、 **IntelliTrace**する前にもう一度ウィンドウがでした。 ここで、前に 1 つのクエリがあった場合は、それらの長いシーケンス参照してください。

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

1 つ目のクリックして**ADO.NET**に起こった複雑なクエリを表示する行が以前に表示します。

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

部門からのクエリは、単純ななった`Select`なしでクエリ`Join`元によって返される 2 つのクエリのセットを使用して、各部門の句が、これは、関連のコースと管理者を取得する個別のクエリに続く、クエリ。

> [!NOTE]
> 遅延のままにする遅延読み込みから読み込みを有効にすると、パターンの繰り返し、同じクエリをここでは、「することもあります。 回避する通常のパターンでは、主テーブルのすべての行に関連するデータの遅延読み込みです。 1 つの結合クエリが複雑すぎて効率的ことを確認できたので、しない限り、通常ことができます、一括読み込みを使用する主要なクエリを変更することでこのような場合のパフォーマンスを向上させるためにします。


## <a name="pre-generating-views"></a>生成前のビュー

ときに、`ObjectContext`オブジェクトが新しいアプリケーション ドメインで最初に作成、Entity Framework には、一連のデータベースへのアクセスに使用されるクラスが生成されます。 これらのクラスと呼ばれます*ビュー*、非常に大きなデータ モデルがあれば、これらのビューを生成する遅れることが、ページの最初の要求に応答を web サイトの新しいアプリケーション ドメインの初期化後にします。 実行時ではなく、コンパイル時に、ビューを作成して、この最初の要求の遅延を減らすことができます。

> [!NOTE]
> アプリケーションは、非常に大規模なデータ モデルを持っていない場合、または大きなデータ モデルが IIS のリサイクル後、最初のページ要求のみに影響するパフォーマンスの問題に関する問題がない場合は、このセクションをスキップできます。 ビューの作成がインスタンス化するたびに行われないとき、`ObjectContext`オブジェクト、ビューは、アプリケーション ドメインでキャッシュされるためです。 そのため、IIS でアプリケーションを頻繁にリサイクルしている場合を除き、ほとんどのページ要求が有利事前生成済みのビュー。


使用してビューを事前に生成することができます、 *EdmGen.exe*コマンド ライン ツールを使用して、または、*テキスト テンプレート変換ツールキット*(T4) テンプレート。 このチュートリアルでは、T4 テンプレートを使用します。

*DAL*フォルダーを使用してファイルを追加、**テキスト テンプレート**テンプレート (がある、**全般**内のノード、**インストールされたテンプレート**一覧)、名前を付けます*SchoolModel.Views.tt*します。 ファイルの既存のコードを次のコードに置き換えます。

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

このコード生成のビュー、 *.edmx*テンプレートと同じフォルダーにあるし、テンプレート ファイルと同じ名前を持つファイル。 たとえば、テンプレート ファイルの名前は*SchoolModel.Views.tt*、という名前のデータ モデル ファイルが検索される*SchoolModel.edmx*します。

ファイルを保存し、内のファイルを右クリックし、**ソリューション エクスプ ローラー**選択**カスタム ツールの実行**します。

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio の名前は、ビューを作成するコード ファイルを生成する*SchoolModel.Views.cs*テンプレートに基づきます。 (お気付きかもしれませんが、選択する前にもコード ファイルの生成**カスタム ツールの実行**テンプレート ファイルを保存するとすぐに、します)。

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

アプリケーションを実行し、以前と同じように動作することを確認できます。

事前に生成したビューの詳細については、次のリソースを参照してください。

- [方法: 事前に生成クエリ パフォーマンスを向上させるビュー](https://msdn.microsoft.com/library/bb896240.aspx) MSDN web サイト。 使用する方法について説明します、`EdmGen.exe`ビューを事前に生成するコマンド ライン ツール。
- [プリコンパイル済み/事前生成されたビューを含む Entity Framework 4 のパフォーマンスを分離する](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx)Windows Server AppFabric の Customer Advisory Team ブログ。

これは、Entity Framework を使用する ASP.NET web アプリケーションのパフォーマンス向上の概要を完了します。 詳細については、次のリソースを参照してください。

- [パフォーマンスに関する考慮事項 (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) MSDN web サイト。
- [Entity Framework チームのブログの投稿のパフォーマンスに関連する](https://blogs.msdn.com/b/adonet/archive/tags/performance/)します。
- [EF はマージ オプションおよびコンパイル済みクエリ](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx)します。 コンパイル済みクエリとマージの予期しない動作を説明するブログの投稿などのオプション`NoTracking`します。 コンパイル済みクエリを使用したり、アプリケーションでのマージ オプションの設定を操作する場合は、最初に読み取ります。
- [Entity Framework 関連のデータとモデリングの Customer Advisory Team のブログ投稿](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/)します。 コンパイル済みクエリと Visual Studio 2010 Profiler を使用してパフォーマンスの問題を検出する投稿が含まれています。
- [非常に複雑なクエリのパフォーマンスの向上に関するアドバイスをエンティティ フレームワークのフォーラム スレッド](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6)します。
- [ASP.NET 状態管理の推奨事項](https://msdn.microsoft.com/library/z1hkazw7.aspx)します。
- [Entity Framework と ObjectDataSource を使用して: カスタム ページング](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx)します。 ブログの投稿でページングを実装する方法を説明するこれらのチュートリアルで作成した ContosoUniversity アプリケーション上に構築される、 *Departments.aspx*ページ。

次のチュートリアルでは、バージョン 4 の新機能は、Entity Framework に重要な拡張機能の一部を確認します。

> [!div class="step-by-step"]
> [前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [次へ](what-s-new-in-the-entity-framework-4.md)
