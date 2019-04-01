---
title: 最初の Razor Components アプリを構築する
author: guardrex
description: Razor Components アプリを段階的に構築し、Razor Components に関する基本的な概念について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 2a987b3f2e687cd9d4dffa2c573c938e68ea3cc8
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419366"
---
# <a name="build-your-first-razor-components-app"></a>最初の Razor Components アプリを構築する

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

このチュートリアルでは、Razor Components を使ってアプリを構築する方法を説明し、Razor Components に関する基本的な概念を示します。 Razor Components ベースのプロジェクト (.NET Core 3.0 以降でサポート)、Blazor ベースのプロジェクト (.NET Core の今後のリリースでサポート) のいずれかを使ってこのチュートリアルに取り組むことができます。

ASP.NET Core Razor Components を使ったエクスペリエンスの場合 ("*推奨*"):

* <xref:razor-components/get-started> のガイダンスに従って Razor Components ベースのプロジェクトを作成します。
* プロジェクトに `RazorComponents` という名前を付けます。

Blazor を使ったエクスペリエンスの場合:

* <xref:spa/blazor/get-started> のガイダンスに従って Blazor ベースのプロジェクトを作成します。
* プロジェクトに `Blazor` という名前を付けます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。 前提条件については、次のトピックをご覧ください。

## <a name="build-components"></a>コンポーネントを構築する

1. *Components/Pages* フォルダー (Blazor では *Pages* ) 内のアプリの 3 つのページをそれぞれ閲覧します。ホーム、カウンター、データのフェッチです。 これらのページは、次の Razor Component ファイル(*Index.razor*、*Counter.razor*、*FetchData.razor*) によって実装されています。 (Blazor は引き続き *.cshtml* ファイル拡張子(*Index.cshtml*、*Counter.cshtml*、*FetchData.cshtml*) を使用します。)

1. Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。 Web ページでカウンターをインクリメントする場合、通常は JavaScript を記述することが必要ですが、Razor Components には C# を使ったより優れた方法が用意されています。

1. Counter コンポーネントの実装を、*Counter.razor* ファイルで調べます。

   *Components/Pages/Counter.razor* (Blazor では *Pages/Counter.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   Counter コンポーネントの UI は、HTML を使って定義されています。 動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](xref:mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。 HTML マークアップと C# のレンダリング ロジックは、ビルド時にコンポーネント クラスに変換されます。 生成される .NET クラスの名前はファイル名と同じです。

   コンポーネント クラスのメンバーは、`@functions` ブロック内で定義されています。 `@functions` ブロック内では、イベント処理や他のコンポーネント ロジックの定義のために、コンポーネントの状態 (プロパティ、フィールド) とメソッドを指定します。 これらのメンバーは、コンポーネントのレンダリング ロジックの一部として、またイベントを処理するために使われます。

   **[クリックしてください]** ボタンを選択すると:

   * Counter コンポーネントの登録済みの `onclick` ハンドラーが呼び出されます (`IncrementCount` メソッドです)。
   * Counter コンポーネントによりそのレンダリング ツリーが再生成されます。
   * 新しいレンダリング ツリーが以前のものと比較されます。
   * ドキュメント オブジェクト モデル (DOM) に対する変更のみが適用されます。 表示されている数が更新されます。

1. Counter コンポーネントの C# のロジックを変更して、カウントが 1 ではなく 2 ずつインクリメントされるようにします。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. アプリをリビルドして実行し、変更を確認します。 **[クリックしてください]** ボタンを選択すると、カウンターは 2 ずつインクリメントされます。

## <a name="use-components"></a>コンポーネントを使う

HTML のような構文を使用して、別のコンポーネントにコンポーネントを含めます。

1. Index コンポーネントに `<Counter />` 要素を追加することで、アプリの Index (ホーム) コンポーネントにカウンター コンポーネントを追加します。

   このエクスペリエンスのために Blazor を使っている場合は、Survey Prompt コンポーネント (`<SurveyPrompt>` 要素) が Index コンポーネントに含まれています。 `<SurveyPrompt>` 要素を `<Counter>` 要素に置き換えます。

   *Components/Pages/Index.razor* (Blazor では *Pages/Index.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. アプリケーションをリビルドして実行します。 ホーム ページに、独自のカウンターがあります。

## <a name="component-parameters"></a>コンポーネントのパラメーター

コンポーネントにパラメーターを持たせることもできます。 コンポーネントのパラメーターは、`[Parameter]` で修飾されたコンポーネント クラス上の、パブリックでないプロパティを使って定義します。 マークアップ内でコンポーネントの引数を指定するには、属性を使います。

1. コンポーネントの `@functions` に関する C# コードを更新します。

   * `[Parameter]` 属性で修飾された `IncrementAmount` プロパティを追加します。
   * `currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。

   *Components/Pages/Counter.razor* (Blazor では *Pages/Counter.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. 属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。 カウンターを 10 ずつインクリメントするように値を設定します。

   *Components/Pages/Index.razor* (Blazor では *Pages/Index.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. ホーム ページを再度読み込みます。 カウンターは、**[クリックしてください]** ボタンを選択するたびに 10 ずつインクリメントされます。 Counter ページ上のカウンターは 1 ずつインクリメントされます。

## <a name="route-to-components"></a>コンポーネントにルーティングする

*Counter.razor* ファイルの上部にある `@page` ディレクティブは、このコンポーネントがルーティング エンドポイントであることを指定しています。 Counter コンポーネントによって `/Counter` に送信された要求が処理されます。 `@page` ディレクティブがない場合、ルーティングされた要求はコンポーネントで処理されませんが、このコンポーネントは引き続き他のコンポーネントで使えます。

## <a name="dependency-injection"></a>依存関係の挿入

アプリのサービス コンテナーに登録されたサービスは、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) を介してコンポーネントから使用できます。 `@inject` ディレクティブを使ってサービスをコンポーネントに挿入します。

サンプル アプリで FetchData コンポーネントのディレクティブを調べます。

Razor コンポーネントのサンプル アプリで、`WeatherForecastService` サービスは[シングルトン](xref:fundamentals/dependency-injection#service-lifetimes)として登録されているため、このサービスの 1 つのインスタンスをアプリ全体で使用できます。 コンポーネントに `WeatherForecastService` サービスのインスタンスを挿入するために、`@inject` ディレクティブが使われています。

*Components/Pages/FetchData.razor*: 

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

FetchData コンポーネントでは、`ForecastService` のような挿入されたサービスを使って、`WeatherForecast` オブジェクトの配列を取得します。

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Blazor バージョンのサンプル アプリでは、*wwwroot/sample-data* フォルダー内の *weather.json* から天気予報データを取得するために、`HttpClient` が挿入されています。

*Pages/FetchData.cshtml*: 

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

両方のサンプル アプリで、各 forecast 変数を気象データのテーブルの行としてレンダリングするために、[@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) ループが使われています。

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Todo リストを構築する

シンプルな ToDo リストを実装したアプリに新しいコンポーネントを追加します。

1. サンプル アプリに空のファイルを追加します。

   * Razor コンポーネントのエクスペリエンスのために、*Todo.razor* ファイルを *Components/Pages* フォルダーに追加します。
   * Blazor のエクスペリエンスのために、*Todo.cshtml* ファイルを *Pages* フォルダーに追加します。

1. コンポーネントに最初のマークアップを指定します。

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. ナビゲーション バーに Todo コンポーネントを追加します。

   NavMenu コンポーネント (*Components/Shared/NavMenu.razor* または Blazor では *Shared/NavMenu.cshtml*) は、アプリのレイアウトで使用されます。 レイアウトは、アプリ内でのコンテンツの重複を回避するために使うコンポーネントです。 詳細については、「<xref:razor-components/layouts>」を参照してください。

   *Components/Shared/NavMenu.razor* (Blazor では *Shared/NavMenu.cshtml*) ファイルで、以下のリスト アイテムのマークアップを既存のリスト アイテムの下に追加して、Todo コンポーネント用の `<NavLink>` を追加します。

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. アプリケーションをリビルドして実行します。 新しい Todo ページに移動して、Todo コンポーネントへのリンクが機能することを確認します。

1. Todo アイテムを表すクラスを保持するために、プロジェクトのルートに *TodoItem.cs* ファイルを追加します。 `TodoItem` クラス用に次の C# コードを使います。

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Todo コンポーネント (*Components/Pages/Todo.razor* または Blazor では *Pages/Todo.cshtml*) に戻ります。

   * Todo 用のフィールドを `@functions` ブロックに追加します。 Todo コンポーネントでは、このフィールドを使って Todo リストの状態を維持します。
   * 各 ToDo アイテムをリスト アイテムとしてレンダリングするために、順序のないリストのマークアップと `foreach` ループを追加します。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. アプリには、リストに ToDo を追加するための UI 要素が必要です。 リストの下に、テキスト入力とボタンを追加します。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. アプリケーションをリビルドして実行します。 **[Add todo]\(ToDo の追加\)** ボタンを選択しても何も起こりません。ボタンにイベント ハンドラーが関連付けられていないためです。

1. Todo コンポーネントに `AddTodo` メソッドを追加し、`onclick` 属性を使ってボタンのクリック用にこれを登録します。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   ボタンを選択すると C# のメソッド `AddTodo` が呼び出されます。

1. 新しい Todo アイテムのタイトルを取得するために、文字列フィールド `newTodo` を追加し、`bind` 属性を使ってこれをテキスト入力の値とバインドします。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. 指定したタイトルを備えた `TodoItem` をリストに追加するように、`AddTodo` メソッドを更新します。 `newTodo` を空の文字列に設定して、テキスト入力の値をクリアします。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. アプリケーションをリビルドして実行します。 Todo リストに Todo をいくつか追加して、新しいコードをテストします。

1. 各 Todo アイテムのタイトルのテキストは編集可能にすることができます。また、チェック ボックスはユーザーが完了したアイテムを追跡するのに役立ちます。 各 Todo アイテムにチェック ボックス入力を追加し、その値を `IsDone` プロパティにバインドします。 `@todo.Title` を、`@todo.Title` にバインドされた `<input>` 要素に変更します。

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. それらの値がバインドされていることを確認するために、`<h1>` ヘッダーを更新して、完了していない (`IsDone` が `false` の) Todo アイテムの数のカウントを表示するようにします。

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. 完了した Todo コンポーネント (*Components/Pages/Todo.razor* または Blazor では *Pages/Todo.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. アプリケーションをリビルドして実行します。 Todo アイテムを追加して新しいコードをテストします。

## <a name="publish-and-deploy-the-app"></a>アプリを発行および配置する

アプリの発行方法については、<xref:host-and-deploy/razor-components/index#publish-the-app> をご覧ください。
