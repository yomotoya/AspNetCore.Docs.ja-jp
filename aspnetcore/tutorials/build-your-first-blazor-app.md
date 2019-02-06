---
title: 最初の Blazor アプリを構築する
author: guardrex
description: Blazor アプリを段階的に構築して、Razor Components フレームワークの基本的な機能について簡単に学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668032"
---
# <a name="build-your-first-blazor-app"></a>最初の Blazor アプリを構築する

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

このチュートリアルでは、Blazor アプリを段階的に構築して、Razor Components フレームワークの基本的な機能について簡単に学習します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。 前提条件については、[作業開始](xref:razor-components/get-started)に関するトピックをご覧ください。

Visual Studio でプロジェクトを作成するには:

1. **[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。 **[Web]** > **[ASP.NET Core Web アプリケーション]** の順に選択します。 **[名前]** フィールドでプロジェクトに "BlazorApp1" という名前を付けます。 **[OK]** を選択します。

    ![新しい ASP.NET Core プロジェクト](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. **[新しい ASP.NET Core Web アプリケーション]** ダイアログが表示されます。 上部で **[.NET Core]** が選択されていることを確認します。 **[ASP.NET Core 2.1]** を選択します。 **[Blazor]** テンプレートを選択して **[OK]** を選択します。

    ![新しい Blazor アプリのダイアログ](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. プロジェクトが作成されたら、**Ctrl キーを押しながら F5 キー**を押して、"*デバッガーなしで*" アプリを実行します。 デバッガーを使った実行 (**F5 キー**) は、現時点ではでサポートされていません。

> [!NOTE]
> Visual Studio を使わない場合は、Windows、macOS、または Linux 上のコマンド プロンプトで Blazor アプリを作成します。
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> `dotnet run` の実行後にコンソール ウィンドウの出力に表示される localhost アドレスとポートを使って、アプリに移動します。 アプリをシャット ダウンするには、コンソール ウィンドウで **Ctrl キーを押しながら C キー**を押します。

ブラウザーで Blazor アプリが実行されます。

![Blazor アプリのホーム ページ](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>コンポーネントを構築する

1. アプリの 3 つのページにそれぞれ移動します。ホーム、カウンター、データのフェッチです。

    これらの 3 つのページは、*Pages* フォルダーにある 3 つの Razor ファイルによって実装されています。*Index.cshtml*、*Counter.cshtml*、*FetchData.cshtml* です。 これらの 3 つのファイルのそれぞれが、ブラウザーでクライアント側でコンパイルされて実行されるコンポーネントを実装しています。

1. カウンター ページ上のボタンを選択します。

    ![Blazor アプリのホーム ページ](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    ボタンを選択するたびに、ページを更新することなくカウンターがインクリメントされます。 通常、この種のクライアント側の動作は JavaScript で処理されますが、ここではそれが `Counter` コンポーネントによって C# と .NET で実装されています。

1. `Counter` コンポーネントの実装を、*Counter.cshtml* ファイルで見てみましょう。

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    `Counter` コンポーネントの UI は通常の HTML を使って定義されています。 動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。 HTML マークアップと C# のレンダリング ロジックは、ビルド時にコンポーネント クラスに変換されます。 生成される .NET クラスの名前はファイル名と同じです。

    コンポーネント クラスのメンバーは、`@functions` ブロック内で定義されています。 `@functions` ブロック内では、イベント処理や他のコンポーネント ロジックの定義のために、コンポーネントの状態 (プロパティ、フィールド) とメソッドを指定できます。 これらのメンバーは、コンポーネントのレンダリング ロジックの一部として、またイベントを処理するために使えます。

    ボタンが選択されると、`Counter` コンポーネントの登録済みの `onclick` ハンドラー (`IncrementCount` メソッド) が呼び出され、`Counter` コンポーネンによりそのレンダリング ツリーが再生成されます。 Blazor によって新旧のレンダリング ツリーが比較され、ブラウザーのドキュメント オブジェクト モデル (DOM) に変更が適用されます。 表示されている数が更新されます。

1. `Counter` コンポーネントのマークアップを更新して、最上位のヘッダーをより "*魅力的に*" します。

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. また、`Counter` コンポーネントの C# のロジックを変更して、カウントが 1 ではなく 2 ずつインクリメントされるようにします。

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. ブラウザーでカウンター ページを更新して、変更内容を確認します。

    ![魅力的なカウンター](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>コンポーネントを使う

コンポーネントを定義したら、そのコンポーネントを使って他のコンポーネントを実装できます。 コンポーネントを使うためのマークアップは、そのコンポーネントの種類をタグ名とする HTML タグのようになります。

1. アプリのホーム ページ (*Index.cshtml*) に `Counter` コンポーネントを追加します。

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. ブラウザーでホーム ページを更新します。 ホーム ページ上にある `Counter` コンポーネントの別のインスタンスに注意してください。

    ![カウンター付き Blazor ホーム ページ](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>コンポーネントのパラメーター

コンポーネントにパラメーターを持たせることもできます。これは、`[Parameter]` で修飾されたコンポーネント クラスのプライベート プロパティを使って定義します。 マークアップ内でコンポーネントの引数を指定するには、属性を使います。 

1. 既定値が 1 の `IncrementAmount` パラメーターを持つように `Counter` コンポーネントを更新します。

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > Visual Studio から、`para` スニペットを使ってコンポーネントのパラメーターを簡単に追加できます。 「`para`」と入力した後、`Tab` キーを 2 回押します。

1. ホーム ページ (*Index.cshtml*) 上で、コンポーネントのプロパティ `IncrementCount` の名前と一致する属性を設定して、`Counter` の増分量を 10 に変更します。

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. ページを再度読み込みます。

    これで、ホーム ページ上のカウンターは 10 ずつインクリメントされるようになりましたが、カウンター ページ上のカウンターは 1 ずつインクリメントされるままです。

    ![Blazor 10 ずつのカウント](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>コンポーネントにルーティングする

*Counter.cshtml* ファイルの上部にある `@page` ディレクティブは、このコンポーネントが要求をルーティングできるページであることを指定しています。 具体的には、`Counter` コンポーネントによって `/Counter` に送信された要求が処理されます。 `@page` ディレクティブがない場合、ルーティングされた要求はコンポーネントで処理されませんが、このコンポーネントは引き続き他のコンポーネントで使えます。

## <a name="dependency-injection"></a>依存関係の挿入

アプリのサービス プロバイダーに登録されたサービスは、[依存関係の挿入 (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) を介してコンポーネントから使用できます。 サービスは、`@inject` ディレクティブを使ってコンポーネントに挿入できます。

`FetchData` コンポーネントの実装を、*FetchData.cshtml* ファイルで見てみましょう。 コンポーネントに [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) インスタンスを挿入するために、`@inject` ディレクティブが使われています。

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

`FetchData` コンポーネントでは、挿入された `HttpClient` を使って、コンポーネントの初期化時にサーバーから JSON データが取得されます。 内部の処理では、Blazor のランタイムが提供する `HttpClient` は JavaScript 相互運用を使って実装されていて、要求を送信するために基になるブラウザーの Fetch API を呼び出しています (C# からは、あらゆる JavaScript ライブラリやブラウザー API を呼び出すことが可能です)。 取得したデータは、`WeatherForecast` の配列である C# 変数 `forecasts` に逆シリアル化されます。

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

各 forecast 変数を気象テーブルの行としてレンダリングするために、`@foreach` ループが使われています。

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a>Todo リストを構築する

シンプルな ToDo リストを実装した新しいページをアプリに追加します。

1. *Pages* フォルダーに、*Todo.cshtml* という名前の空のテキスト ファイルを追加します。

1. ページの最初のマークアップを指定します。

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. *Shared/NavMenu.cshtml* を更新して、ナビゲーション バーに ToDo ページを追加します。 以下のリスト アイテムのマークアップを既存のリスト アイテムの下に追加して、ToDo ページ用の `NavLink` を追加します。

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. ブラウザーでアプリを更新します。 新しい ToDo ページを確認します。

    ![Blazor ToDo の開始](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. ToDo アイテムを表すクラスを保持するために、プロジェクトのルートに *TodoItem.cs* ファイルを追加します。

1. `TodoItem` クラス用に次の C# コードを使います。

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. *Todo.cshtml* 内の `Todo` コンポーネントに戻って、ToDo 用のフィールドを `@functions` ブロックに追加します。 `Todo` コンポーネントでは、このフィールドを使って ToDo リストの状態を維持します。

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. 各 ToDo アイテムをリスト アイテムとしてレンダリングするために、順序のないリストのマークアップと `foreach` ループを追加します。

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. アプリには、リストに ToDo を追加するための UI 要素が必要です。 リストの下に、テキスト入力とボタンを追加します。

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. ブラウザーを更新します。

    ![[Add todo]\(ToDo の追加\) ボタン](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    **[Add todo]\(ToDo の追加\)** ボタンを選択しても何も起こりません。ボタンにイベント ハンドラーが関連付けられていないためです。

1. `Todo` コンポーネントに `AddTodo` メソッドを追加し、`onclick` 属性を使ってボタンのクリック用にこれを登録します。

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    ボタンが選択されるたびに、C# のメソッド `AddTodo` が呼び出されます。

1. 新しい ToDo アイテムのタイトルを取得するために、文字列フィールド `newTodo` を追加し、`bind` 属性を使ってこれをテキスト入力の値とバインドします。

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. 指定したタイトルを備えた `TodoItem` をリストに追加するように、`AddTodo` メソッドを更新します。 `newTodo` を空の文字列に設定して、テキスト入力の値をクリアすることを忘れないでください。

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. ブラウザーを更新します。 ToDo リストに ToDo をいくつか追加します。

    ![ToDo を追加する](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. 最後に、ToDo リストにチェック ボックスを追加しましょう。 各 ToDo アイテムのタイトル テキストも、編集可能にすることができます。 各 ToDo アイテムにチェック ボックス入力とテキスト入力を追加し、それらの値をそれぞれ `Title` および `IsDone` プロパティにバインドします。

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. それらの値がバインドされていることを確認するために、`h1` ヘッダーを更新して、まだ完了していない (`IsDone` が `false` の) ToDo アイテムの数のカウントを表示するようにします。

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. 完成した `Todo` コンポーネントは、次のようになります。

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

ブラウザーでアプリを更新します。 ToDo アイテムをいくつか追加してみてください。

![完成した Blazor ToDo リスト](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>発行と配置

Visual Studio を使う場合、ToDo Blazor アプリを Azure に発行するには、次の手順を実行します。

1. **[ソリューション エクスプローラー]** でプロジェクトを右クリックし、**[発行]** を選択します。

1. **[発行先を選択]** ダイアログで、**[App Service]** と **[新規作成]** を選択します。 **[発行]** を選びます。

    ![発行先を選択](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. **[App Service の作成]** ダイアログで、アプリの名前を選択し、サブスクリプション、リソース グループ、およびホスティング プランを選択します。 **[作成]** を選択して App Service を作成し、アプリを発行します。

    ![App Service の作成](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

アプリが配置されるまで 1 分ほど待機します。

これで、アプリが Azure で稼働されました。 最初の Blazor アプリを構築するという ToDo アイテムを、"*完了*" にマークしましょう。 お疲れさまでした。

![Azure 上の Blazor](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Visual Studio を使わない場合は、Windows、macOS、または Linux 上のコマンド プロンプトで Blazor アプリを発行します。
>
> ```console
> dotnet publish -c Release
> ```
>
> 配置は、*/bin/Release/\<ターゲット フレームワーク>/publish* フォルダーに作成されます。 *publish* フォルダーのコンテンツを、サーバーまたはホスティング サービスに移動します。
>
> 詳細については、[ホストと配置](xref:host-and-deploy/razor-components/index#publish-the-app)に関するトピックをご覧ください。

## <a name="additional-resources"></a>その他の技術情報

より複雑な Blazor のサンプル アプリについては、GitHub 上の [Flight Finder サンプル](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor)を参照してください。

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
