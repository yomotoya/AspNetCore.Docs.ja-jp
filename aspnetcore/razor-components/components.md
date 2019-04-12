---
title: 作成し、Razor のコンポーネントを使用
author: guardrex
description: 作成してデータにバインドする、イベントを処理およびコンポーネントのライフ サイクルを管理する方法を含む、Razor のコンポーネントを使用する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515698"
---
# <a name="create-and-use-razor-components"></a>作成し、Razor のコンポーネントを使用

によって[Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)、および[Morné Zaayman](https://github.com/MorneZaayman)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。 前提条件については、[作業開始](xref:razor-components/get-started)に関するトピックをご覧ください。

使用して razor コンポーネント アプリを構築*コンポーネント*します。 コンポーネントは、ページ、ダイアログ ボックスで、フォームなどのユーザー インターフェイス (UI) の自己完結型のチャンクです。 コンポーネントには、HTML マークアップとデータを挿入または UI のイベントに応答するために必要な処理ロジックが含まれています。 コンポーネントは、柔軟かつ軽量なです。 入れ子になった、再利用してできるプロジェクト間で共有します。

## <a name="component-classes"></a>コンポーネント クラス

コンポーネントは、通常のコンポーネントの Razor ファイルで実装 (*.razor*) の組み合わせを使用してC#と HTML マークアップ (*.cshtml* Blazor アプリでファイルが使用されます)。

Razor のコンポーネントを使用してアプリでコンポーネントを作成することができます、 *.cshtml*ファイル拡張子のファイルを使用してコンポーネントの Razor ファイルとして識別される限り、 `_RazorComponentInclude` MSBuild プロパティです。 などのコンポーネントの Razor テンプレートを使用して作成されたアプリを指定するすべて *.cshtml*下でファイル、*コンポーネント*フォルダーがコンポーネントの Razor ファイルとして扱われます。

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

コンポーネントの UI を定義するには、HTML を使用します。 動的なレンダリング ロジック (たとえばループ、条件、式) が、[Razor](xref:mvc/views/razor) と呼ばれる埋め込みの C# 構文を使って追加されています。 Razor のコンポーネント アプリをコンパイルするときに、HTML マークアップとC#レンダリング ロジックは、コンポーネントのクラスに変換されます。 生成されたクラスの名前では、ファイルの名前と一致します。

コンポーネント クラスのメンバーが定義されている、`@functions`ブロック (1 つ以上`@functions`ブロックが許容されます)。 `@functions`ブロック、コンポーネントの状態 (プロパティ、フィールド) は、イベント処理のため、またはその他のコンポーネントのロジックを定義するための方法で指定します。

ロジックを使用して、コンポーネントの一部のレンダリングと、コンポーネントのメンバーを使用しできるC#で始まる式`@`します。 たとえば、C#フィールドを付けることによって表示`@`フィールド名にします。 次の例では、評価し、レンダリングします。

* `_headingFontStyle` CSS プロパティの値に`font-style`します。
* `_headingText` コンテンツを`<h1>`要素。

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

コンポーネントが最初にレンダリングした後、コンポーネントには、イベントに応答には、そのレンダリング ツリーが再生成します。 Razor のコンポーネントは、前の 1 つに対して新しいレンダリング ツリーを比較しますし、ブラウザーのドキュメント オブジェクト モデル (DOM) に変更を適用します。

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Razor ページと MVC アプリにコンポーネントを統合します。

既存の Razor ページと MVC アプリを使用してコンポーネントを使用します。 既存のページまたは Razor コンポーネントを使用するビューを書き直す必要はありません。 コンポーネントはレンダリング済みのページやビューを表示するときの 64 x 64&dagger;と同時にします。 

> [!NOTE]
> &dagger;サーバー側の事前は、Razor コンポーネント アプリの既定で有効です。 クライアント側 Blazor アプリは、今後の Preview 4 リリースで事前をサポートします。 詳細については、次を参照してください。 [MapFallbackToPage/ファイルを使用するテンプレート/ミドルウェアを更新](https://github.com/aspnet/AspNetCore/issues/8852)します。

ページまたはビューからコンポーネントを表示するために使用して、 `RenderComponentAsync<TComponent>` HTML ヘルパー メソッド。

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

ページ、およびビューから表示されるコンポーネントは、Preview 3 リリースの対話型です。 たとえば、ボタンを選択するメソッドの呼び出しは発生しません。 今後のプレビューはこの制限に対処し、通常の要素と属性の構文を使用してレンダリングのコンポーネントのサポートを追加します。

コンポーネントを使用できるは、ページ、およびビュー、その逆は true はありません。 コンポーネントは部分ビューとセクションなど、ビューおよびページ固有のシナリオで使用できません。 部分ビュー コンポーネントで、ロジックを部分ビューの要素からロジックを使用して、コンポーネントにします。

## <a name="using-components"></a>コンポーネントを使用します。

コンポーネントは、それらを宣言することでその他のコンポーネントを含めることができます HTML 要素の構文を使用します。 コンポーネントを使うためのマークアップは、そのコンポーネントの種類をタグ名とする HTML タグのようになります。

次のマークアップをレンダリングする`HeadingComponent`(*HeadingComponent.cshtml*) インスタンス。

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>コンポーネントのパラメーター

コンポーネントを持つことができます*コンポーネント パラメーター*を使用して定義されている*非パブリック*で装飾されたコンポーネントのクラスのプロパティを`[Parameter]`します。 マークアップ内でコンポーネントの引数を指定するには、属性を使います。

次の例では、`ParentComponent`の値を設定、`Title`のプロパティ、 `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>子コンテンツ

コンポーネントは、別のコンポーネントのコンテンツを設定できます。 割り当てのコンポーネントは、受信側のコンポーネントを指定するタグの間でコンテンツを提供します。 たとえば、`ParentComponent`を追加できますコンテンツ レンダリング子コンポーネント内のコンテンツを配置することで`<ChildComponent>`タグ。

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

子コンポーネントには、`ChildContent`プロパティを表す、`RenderFragment`します。 値`ChildContent`が配置されている子コンポーネントのマークアップでコンテンツをレンダリングする必要があります。 次の例の値で`ChildContent`親コンポーネントから受け取り、ブートス トラップのパネルの内部に表示されます`panel-body`します。

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> プロパティの受信、`RenderFragment`コンテンツを指定する必要があります`ChildContent`慣例です。

## <a name="data-binding"></a>データ バインディング

両方のコンポーネントと DOM 要素へのデータ バインディングに抑え、`bind`属性。 次の例では、バインド、 `ItalicsCheck`  チェック ボックスのプロパティは、状態を確認します。

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

プロパティの値が更新のチェック ボックスが選択され、クリア、`true`と`false`、それぞれします。

チェック ボックスは、プロパティの値の変更への応答ではなく、コンポーネントが表示される場合にのみ、UI で更新されます。 イベント ハンドラーのコードの実行後に、コンポーネント自体の表示に、ためプロパティの更新は、通常 UI に反映されます、すぐにします。

使用して`bind`で、`CurrentValue`プロパティ (`<input bind="@CurrentValue">`) は本質的には、次と同じです。

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

コンポーネントが表示されるときに、`value`からは、入力要素の`CurrentValue`プロパティ。 ユーザーがテキスト ボックスに、ときに、`onchange`イベントが発生した、`CurrentValue`プロパティが変更された値に設定します。 実際には、コード生成は、もう少し複雑なので`bind`型変換が実行するいくつかのケースを処理します。 原則として、`bind`を持つ式の現在の値を関連付けます、`value`属性とハンドルの変更が登録済みのハンドラーを使用します。

ほかに`onchange`などのイベントを使用して、プロパティをバインドできる`oninput`より明示的にバインドすることで。

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

異なり`onchange`、`oninput`テキスト ボックスに入力される文字ごとに発生します。

**書式指定文字列**

データ バインディングが連携<xref:System.DateTime>書式指定文字列。 この時点で、通貨、数値の形式などの他の式の形式は使用できません。

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`format-value`属性に適用する日付の書式を指定します、`value`の`input`要素。 値を解析するの形式を使用してもときに、`onchange`イベントが発生します。

**コンポーネントのパラメーター**

バインディングでは、コンポーネントのパラメーターも認識している`bind-{property}`コンポーネント間でプロパティ値をバインドすることができます。

次のコンポーネントを使用して`ChildComponent`し、バインド、`ParentYear`を親からのパラメーター、`Year`子コンポーネントにパラメーター。

親コンポーネント:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

子コンポーネント:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

読み込み、`ParentComponent`次のマークアップを生成します。

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

場合の値、`ParentYear`でボタンを選択してプロパティを変更、 `ParentComponent`、`Year`のプロパティ、`ChildComponent`が更新されます。 新しい値`Year`が UI に表示されるときに、`ParentComponent`持続は。

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

`Year`コンパニオンがあるために、パラメーターがバインド可能な`YearChanged`の型と一致するイベント、`Year`パラメーター。

慣例により、`<ChildComponent bind-Year="@ParentYear" />`は基本的に書き込みと同じです

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

対応するイベント ハンドラーを使用して、一般に、プロパティをバインドできる`bind-property-event`属性。

## <a name="event-handling"></a>イベント処理

Razor のコンポーネントは、イベント処理機能を提供します。 HTML 要素の属性の名前付き`on<event>`(たとえば、 `onclick`、 `onsubmit`) デリゲートに型指定された値では、Razor のコンポーネントがイベント ハンドラーとして属性の値を処理します。 属性の名前は常に始まる`on`します。

次のコードは、`UpdateHeading`メソッド、UI で、ボタンが選択されている場合。

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

次のコードは、`CheckboxChanged`メソッド、UI で、チェック ボックスが変更されたとき。

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

イベント ハンドラーと非同期の戻り値できます、<xref:System.Threading.Tasks.Task>します。 手動で呼び出す必要はありません`StateHasChanged()`します。 発生したとき、例外が記録されます。

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

一部のイベントのイベント固有のイベント引数の型が許可されます。 これらのイベントの種類のいずれかへのアクセスが必要でない場合は、メソッド呼び出しで必要はありません。

サポートされているイベントの引数の一覧です。

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

ラムダ式も使用できます。

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

など、追加の値を終了すると便利では多くの場合、一連の要素を反復処理するときにします。 次の例が 3 つを作成呼び出しの各ボタン`UpdateHeading`イベント引数を渡す (`UIMouseEventArgs`) やボタンの番号 (`buttonNumber`) UI で選択した場合。

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> **いない**ループ変数を使用して (`i`) で、`for`ラムダ式内で直接ループします。 すべてのラムダ式が原因で同じ変数を使用するそれ以外の場合`i`のすべてのラムダで同じにする値します。 ローカル変数にその値を常にキャプチャ (`buttonNumber`前の例)、それを使用します。

## <a name="capture-references-to-components"></a>コンポーネントへの参照をキャプチャします。

コンポーネントの参照方法 get コンポーネントのインスタンスへの参照をように指定などのそのインスタンスにコマンドを発行できます`Show`または`Reset`します。 コンポーネントの参照をキャプチャするには、追加、`ref`子コンポーネントに属性し、子コンポーネントと同じ名前と同じ型のフィールドを定義します。

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

コンポーネントが表示されるときに、`loginDialog`フィールドに入力されますが、`MyLoginDialog`子コンポーネントのインスタンス。 コンポーネントのインスタンス上の .NET メソッドを呼び出すことができます。

> [!IMPORTANT]
> `loginDialog`コンポーネントが表示され、その出力に含まれる後にのみ変数が設定されます、`MyLoginDialog`要素。 その時点までは何も参照します。 コンポーネントには、レンダリングが完了した後は、コンポーネントの参照を操作、使用、`OnAfterRenderAsync`または`OnAfterRender`メソッド。

同様の構文を使用してコンポーネントの参照をキャプチャ中に[要素の参照をキャプチャ](xref:razor-components/javascript-interop#capture-references-to-elements)、わけで、 [JavaScript の相互運用機能](xref:razor-components/javascript-interop)機能します。 JavaScript コードに渡されたいないコンポーネントの参照.NET コードでのみ使用されます。

> [!NOTE]
> **いない**コンポーネント参照を使用して子コンポーネントの状態を変更します。 代わりに、通常の宣言型のパラメーターを使用して、子コンポーネントにデータを渡します。 これにより、正しい時刻に自動的にレンダリングする子コンポーネントです。

## <a name="lifecycle-methods"></a>ライフ サイクル メソッド

`OnInitAsync` `OnInit`コンポーネントを初期化するコードを実行します。 非同期操作を実行するには使用`OnInitAsync`と`await`操作にキーワード。

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

同期操作では、使用`OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` `OnParametersSet`コンポーネントがその親からパラメーターを受信し、値がプロパティに割り当てられている場合に呼び出されます。 これらのメソッドは、コンポーネントの初期化後に実行され、たびに、コンポーネントがレンダリングされます。

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` `OnAfterRender`は、コンポーネントには、レンダリングが完了した後に呼び出されます。 要素とコンポーネントの参照は、この時点で設定されます。 この段階を使用すると、レンダリングされた DOM 要素を操作するサードパーティ製の JavaScript ライブラリをアクティブ化など、描画されるコンテンツを使用して追加の初期化手順を実行できます。

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` パラメーターが設定される前にコードを実行する上書きできます。

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

場合`base.SetParameters`いない呼び出されると、カスタム コードは必要な方法で受信したパラメーター値を解釈できます。 たとえば、受信パラメーターは、クラスのプロパティに割り当てられる必要ありません。

`ShouldRender` UI の更新を抑制するのにはオーバーライドできます。 実装を返す場合`true`UI が更新されます。 場合でも`ShouldRender`がオーバーライドされると、コンポーネントは常に最初にレンダリングします。

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>IDisposable とコンポーネントの破棄

コンポーネントを実装している場合<xref:System.IDisposable>、 [Dispose メソッド](/dotnet/standard/garbage-collection/implementing-dispose)UI からコンポーネントを削除するときに呼び出されます。 次のコンポーネントを使用して`@implements IDisposable`と`Dispose`メソッド。

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>ルーティング

Razor のコンポーネントでのルーティングは、アプリでアクセス可能な各コンポーネントにルート テンプレートを提供することによって実現されます。

ときに、 *.cshtml*ファイルと、`@page`ディレクティブをコンパイルすると、生成されたクラスが指定された、<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>ルート テンプレートを指定します。 ルーターが持つクラスでコンポーネントが、実行時に、`RouteAttribute`どのコンポーネントが要求された URL に一致するルート テンプレートを表示します。

コンポーネントには、複数のルート テンプレートを適用できます。 次のコンポーネントがの要求に応答`/BlazorRoute`と`/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>ルート パラメーター

コンポーネントで提供されるルート テンプレートからルート パラメーターを受け取ることができます、`@page`ディレクティブ。 ルーターでは、ルート パラメーターを使用して、対応するコンポーネントのパラメーターを設定します。

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

省略可能なパラメーターはサポートされていませんので、2 つ`@page`ディレクティブは、上記の例に適用されます。 最初のパラメーターを指定せず、コンポーネントへの移動を許可します。 2 番目の`@page`ディレクティブは、`{text}`ルート パラメーターと値を割り当てます、`Text`プロパティ。

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>「分離コード」エクスペリエンスのための基本クラスの継承

コンポーネント ファイル (*.cshtml*) の HTML マークアップを混在させるとC#同じファイル内のコードを処理します。 `@inherits`ディレクティブを使用して、Razor コンポーネント アプリ コンポーネントのマークアップを処理のコードから分離する「分離コード」エクスペリエンスを提供することができます。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/)コンポーネントが、基底クラスを継承する方法を示しています。 `BlazorRocksBase`、コンポーネントのプロパティとメソッドを提供します。

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

基底クラスの派生元`ComponentBase`します。

## <a name="razor-support"></a>Razor サポート

**Razor ディレクティブ**

Razor ディレクティブは、次の表に表示されます。

| ディレクティブ | 説明 |
| --------- | ----------- |
| [\@関数](xref:mvc/views/razor#section-5) | 追加、C#コンポーネントにコード ブロックです。 |
| `@implements` | 生成されたコンポーネントのクラスのインターフェイスを実装します。 |
| [\@継承](xref:mvc/views/razor#section-3) | コンポーネントが継承するクラスの完全に制御を提供します。 |
| [\@挿入します。](xref:mvc/views/razor#section-4) | サービスからの挿入のことができます、[サービス コンテナー](xref:fundamentals/dependency-injection)します。 詳しくは、「[ビューへの依存関係の挿入](xref:mvc/views/dependency-injection)」をご覧ください。 |
| `@layout` | レイアウト コンポーネントを指定します。 レイアウト コンポーネントは、コードの重複や不整合を回避するために使用されます。 |
| [\@ページ](xref:razor-pages/index#razor-pages) | コンポーネントの処理で要求が直接処理する必要がありますを指定します。 `@page`ディレクティブは、ルートと省略可能なパラメーターで指定できます。 Razor ページとは異なり、`@page`ディレクティブは、ファイルの先頭で最初のディレクティブを指定する必要はありません。 詳細については、[ルーティング](xref:razor-components/routing)に関するページを参照してください。 |
| [\@using](xref:mvc/views/razor#using) | 追加、 C# `using`ディレクティブを生成されたコンポーネントのクラスにします。 |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | 使用して、`@addTagHelper`アプリのアセンブリよりも別のアセンブリでコンポーネントを使用します。 |

**条件付き属性**

属性は、.NET の値に基づく条件付きで表示されます。 値が場合`false`または`null`属性は表示されません。 値が場合`true`属性はレンダリングを最小限に抑えます。

次の例では、`IsCompleted`かどうかを`checked`コントロールのマークアップに表示されます。

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

場合`IsCompleted`は`true`、チェック ボックスとしてレンダリングされます。

```html
<input type="checkbox" checked>
```

場合`IsCompleted`は`false`、チェック ボックスとしてレンダリングされます。

```html
<input type="checkbox">
```

**Razor で追加情報**

Razor の詳細については、次を参照してください。、 [Razor 構文リファレンス](xref:mvc/views/razor)します。

## <a name="raw-html"></a>生の HTML

文字列は通常を使用してレンダリング DOM テキスト ノード、つまりを含めることはすべてのマークアップは無視され、リテラル テキストとして扱われます。 生の HTML をレンダリングするの HTML コンテンツをラップする`MarkupString`値。 値が HTML または SVG として解析され、DOM に挿入

> [!WARNING]
> いずれかから構築された生の HTML のレンダリングには、ソースが信頼されていない、**セキュリティ リスク**と避ける必要があります。

次の例を使用して、`MarkupString`コンポーネントの出力に静的な HTML コンテンツのブロックを追加する型。

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>テンプレート化されたコンポーネント

テンプレート化されたコンポーネントは、コンポーネントのレンダリング ロジックの一部として使用できますし、パラメーターとして 1 つまたは複数の UI テンプレートをそのまま使用するコンポーネント。 テンプレート化されたコンポーネントを使用するより正規のコンポーネントも再利用可能なよりは上位レベルのコンポーネントを作成できます。 いくつかの例は次のとおりです。

* ユーザーがテーブルのヘッダー、行、およびフッターのテンプレートを指定できるテーブル コンポーネント。
* ユーザーがリスト アイテムのレンダリング用のテンプレートで指定できるリスト コンポーネント。

### <a name="template-parameters"></a>テンプレート パラメーター

型の 1 つまたは複数のコンポーネント パラメーターを指定して、テンプレート化されたコンポーネントが定義されている`RenderFragment`または`RenderFragment<T>`します。 レンダリングのフラグメントでは、コンポーネントによって表示される UI のセグメントを表します。 レンダリングのフラグメントは、必要に応じて、レンダリングのフラグメントが呼び出されたときに指定できるパラメーターを受け取ります。

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

パラメーターの名前に一致する子要素を使用して、テンプレート パラメーターを指定できます、テンプレート化されたコンポーネントを使用する場合 (`TableHeader`と`RowTemplate`次の例)。

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>テンプレートのコンテキスト パラメーター

型のコンポーネントの引数`RenderFragment<T>`要素があるという名前の暗黙のパラメーターとして渡された`context`(たとえば、上記のコード サンプルから`@context.PetId`)、使用して、パラメーター名を変更することが、`Context`子の属性要素。 次の例では、`RowTemplate`要素の`Context`属性を指定します、`pet`パラメーター。

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

または、指定することができます、`Context`コンポーネント要素の属性。 指定した`Context`属性は、すべての指定したテンプレート パラメーターに適用されます。 暗黙の型の子コンテンツのコンテンツのパラメーター名を指定する場合に便利なこのできます (任意の折り返しなしの子要素)。 次の例では、`Context`属性に表示されます、`TableTemplate`要素と、すべてのテンプレート パラメーターに適用されます。

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>汎用型のコンポーネント

テンプレート化されたコンポーネントは、多くの場合、一般的に入力します。 汎用リスト ビュー テンプレートのコンポーネントを使用して、表示するために、`IEnumerable<T>`値。 汎用のコンポーネントを定義するには、使用、`@typeparam`型パラメーターを指定するディレクティブ。

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

ジェネリック型指定されたコンポーネントを使用する場合、型パラメーターが可能であれば推論されます。

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

それ以外の場合、型パラメーターする必要があります明示的に指定する型パラメーターの名前に一致する属性を使用します。 次の例では、`TItem="Pet"`種類を指定します。

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>カスケード型の値とパラメーター

一部のシナリオで便利ではありませんフローのデータを使用して子孫コンポーネントに先祖コンポーネントから[コンポーネント パラメーター](#component-parameters)いくつかのコンポーネントのレイヤーがある場合は特に、します。 カスケード型の値およびパラメーターは、先祖コンポーネントをすべてその子孫のコンポーネントの値を指定する便利な方法を提供することでこの問題を解決します。 カスケード型の値とパラメーターは、コンポーネントを調整するための方法も提供します。

### <a name="theme-example"></a>テーマの使用例

次に*テーマ*例、サンプル アプリから、`ThemeInfo`クラスのすべてのアプリの特定の部分内でボタンと同じスタイルを共有できるように、コンポーネントの階層の下位に伝達するテーマ情報を指定します。

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

先祖、コンポーネントには、カスケード型の値のコンポーネントを使用してカスケード型の値を指定できます。 値のカスケード コンポーネントは、コンポーネントの階層のサブツリーをラップし、そのサブツリー内のすべてのコンポーネントの 1 つの値を提供します。

たとえば、サンプル アプリには、テーマ情報を指定します。 (`ThemeInfo`) のレイアウトの本文を構成するすべてのコンポーネントのカスケード型パラメーターとして、アプリのレイアウトのいずれかで、`@Body`プロパティ。 `ButtonClass` 値が割り当てられている`btn-success`レイアウト コンポーネント。 任意の子孫のコンポーネントを使用してこのプロパティを使用できる、`ThemeInfo`カスケード型のオブジェクト。

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

させるのカスケード型の値を使用して、コンポーネントを使用してカスケード型パラメーターの宣言、`[CascadingParameter]`属性または名前の文字列値に基づきます。

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

文字列名の値を持つバインドは、同じサブツリー内で区別する必要があり、同じ種類の複数のカスケード型の値をした場合に関連します。

カスケード型の値は、型でカスケード型パラメーターにバインドされます。

サンプル アプリで、値パラメーターのテーマのカスケード コンポーネントのバインドを`ThemeInfo`カスケード型パラメーターをカスケード型の値。 パラメーターは、コンポーネントによって表示されるボタンのいずれかの CSS クラスの設定に使用されます。

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>タブの例

カスケード型パラメーターでは、コンポーネント、コンポーネントの階層全体で共同作業を行うこともできます。 たとえば、次*タブ*サンプル アプリでの使用例です。

サンプル アプリには、`ITab`タブを実装するインターフェイス。

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

値パラメーターのタブのカスケード コンポーネント タブのいくつかのコンポーネントが含まれています、タブ設定コンポーネントを使用します。

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

タブの子コンポーネントは タブの設定を明示的にパラメーターとして渡されます。 代わりに、タブの子コンポーネントには、タブのセットの子コンテンツの一部です。 ただし、タブの設定も知る必要がある タブの各コンポーネント、ヘッダーとアクティブなタブを表示するためです。コードを追加するコンポーネント タブの設定を必要とせずにこの調整を有効にする*自体をカスケード型の値として入力できます*し、取得される子孫タブ コンポーネント。

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

子孫のタブのコンポーネントのキャプチャ、カスケード型パラメーターとして含むのタブの設定 タブのコンポーネントが、タブのセットと座標に自分自身を追加、どの タブではアクティブです。

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Razor テンプレート

レンダリングのフラグメントは、Razor テンプレートの構文を使用して定義できます。 Razor テンプレートでは、UI のスニペットを定義し、次の形式を想定する方法です。

```cshtml
@<tag>...</tag>
```

次の例は、指定する方法を示しています。`RenderFragment`と`RenderFragment<T>`値。

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Razor テンプレート化されたコンポーネントに引数として渡されるまたは直接レンダリングできるテンプレートを使用して定義されたフラグメントをレンダリングします。 たとえば、以前のテンプレートは次の Razor マークアップで直接レンダリングされます。

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

表示される出力:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>手動の RenderTreeBuilder ロジック

`Microsoft.AspNetCore.Components.RenderTree` コンポーネントおよびコンポーネントを手動で構築などの要素を操作するためのメソッドを提供しますC#コード。

> [!NOTE]
> 使用`RenderTreeBuilder`高度なシナリオは、コンポーネントを作成します。 形式が正しくないコンポーネント (たとえば、閉じられていないマークアップ タグ) と、未定義の動作があります。

次のペット詳細コンポーネントを検討してください (*PetDetails.razor* Razor コンポーネントです。*PetDetails.cshtml* Blazor で)、別のコンポーネントを手動で構築します。

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

次の例では、ループ、`CreateComponent`メソッドには、Pet の詳細の 3 つのコンポーネントが生成されます。 呼び出すときに`RenderTreeBuilder`コンポーネントを作成する方法 (`OpenComponent`と`AddAttribute`)、シーケンス番号はソース コードの行番号。 コードのない個別の個別の行に対応するシーケンス番号に基づいてアルゴリズムを Razor コンポーネントは、呼び出しを呼び出します。 コンポーネントを作成するときに`RenderTreeBuilder`メソッド、ハードコーディング シーケンス番号の引数。 **計算またはカウンターを使用して、シーケンス番号を生成すると、パフォーマンスの低下につながります。**

コンポーネントの構築 (*BuiltContent.razor* Razor コンポーネントです。*BuiltContent.cshtml* Blazor で)。

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```
