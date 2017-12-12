---
title: "コンポーネントの表示"
author: rick-anderson
description: "コンポーネントの表示は再利用可能なレンダリング ロジックがある任意の場所ものです。"
keywords: "ASP.NET Core、コンポーネントの表示、部分ビュー"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 2cf82df78c250cdfdd808d49acfc06dc2ea82f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="view-components"></a>コンポーネントの表示

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="introducing-view-components"></a>コンポーネントの表示の概要

新しい ASP.NET Core mvc コンポーネントの表示は部分のビューに似ていますがはるかに強力なです。 コンポーネントの表示はしない、モデル バインディングを使用し、呼び出し時に指定したデータにのみ依存します。 ビュー コンポーネント:

* 応答の全体ではなく、チャンクを表示します。
* 同じ問題の分離やテストの容易性の利点がありますが、コント ローラーとビューの間にあります。
* パラメーターとビジネス ロジックを持つことができます。
* 通常、レイアウト ページから呼び出されます

コンポーネントの表示は、任意の場所などの複雑すぎるため、部分ビューが再利用可能なレンダリング ロジックがある場合。

* 動的なナビゲーション メニュー
* タグのクラウド (データベースのクエリを)
* ログイン パネル
* ショッピング カート
* パブリッシュされたアーティクル最近
* 一般的なブログでサイドバーの内容
* すべてのページにレンダリングされますをログアウトするかによっては、ユーザーの状態にあるログ、ログインに、いずれかのリンクを表示するログイン パネル

ビュー コンポーネントは、2 つの部分で構成されています: クラス (から派生した通常[ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) であり、結果 (通常はビュー) を返します。 コント ローラーと同様に、ビュー コンポーネントが、POCO をすることができますもほとんどの開発者は活用するために使用可能なプロパティとメソッドから派生することによって`ViewComponent`です。

## <a name="creating-a-view-component"></a>ビュー コンポーネントを作成します。

このセクションには、ビュー コンポーネントを作成する高レベルの要件が含まれています。 記事の後半でおうまく各手順の詳細を確認し、ビュー コンポーネントを作成します。

### <a name="the-view-component-class"></a>ビューのコンポーネント クラス

次のいずれかのビュー コンポーネント クラスを作成できます。

* 派生する*ViewComponent*
* 持つクラスを装飾、`[ViewComponent]`属性、またはクラスから派生する、`[ViewComponent]`属性
* 名前が、サフィックスで終わる、クラスを作成する*ViewComponent*

コント ローラーのようにビュー コンポーネントは、パブリックの入れ子にされないインデックスおよび非抽象クラスをする必要があります。 ビューのコンポーネント名は、削除"ViewComponent"サフィックスを持つクラスの名前です。 明示的に指定できますを使用して、`ViewComponentAttribute.Name`プロパティです。

ビューのコンポーネント クラス:

* コンス トラクターを完全にサポート[依存関係の挿入](../../fundamentals/dependency-injection.md)

* コント ローラーのライフ サイクルは、使用できないことを意味の一部を受け取らない[フィルター](../controllers/filters.md)ビュー コンポーネント

### <a name="view-component-methods"></a>ビュー コンポーネント メソッド

表示コンポーネントにそのロジックを定義する、`InvokeAsync`を返すメソッド、`IViewComponentResult`です。 パラメーターは、モデル バインドからではなく、ビューのコンポーネントの呼び出しから直接取得します。 ビュー コンポーネントしない直接要求が処理されます。 通常、ビュー コンポーネントは、モデルを初期化し、呼び出すことによって、ビューに渡されます、`View`メソッドです。 要約すると、コンポーネントのメソッドを参照してください。

* 定義、`InvokeAsync`を返すメソッドを`IViewComponentResult`
* モデルの初期化を呼び出すことによって、ビューに渡します通常、 `ViewComponent` `View`メソッド。
* パラメーターを取得する呼び出し元のメソッドでは、HTTP ではないから、モデルのバインドがありません。
* HTTP エンドポイントとして直接到達可能ではないに呼び出されます (ビュー) では通常、コードからです。 ビューのコンポーネントが、要求を処理しません。
* 詳細を現在の HTTP 要求ではなく、シグネチャではオーバー ロードします。

### <a name="view-search-path"></a>ビューの検索パス

ランタイムは、次のパスにビューを検索します。

   * ビュー/\<controller_name >/Components/\<view_component_name >/\<view_name >
   * コンポーネント/ビュー/共有/\<view_component_name >/\<view_name >

ビューのコンポーネントの既定のビュー名は*既定*、つまり、ビューは、ファイルは通常の名前*Default.cshtml*です。 別のビュー名を指定するには、ビューのコンポーネントの結果を作成するときに、または呼び出すときに、`View`メソッドです。

ファイルの表示の名前を付けることをお勧め*Default.cshtml*を使用して、*コンポーネント/ビュー/共有/\<view_component_name >/\<view_name >*パス。 `PriorityList`このサンプルで使用されるビュー コンポーネントを使用して*Views/Shared/Components/PriorityList/Default.cshtml*ビュー コンポーネントをします。

## <a name="invoking-a-view-component"></a>ビュー コンポーネントを呼び出す

ビュー コンポーネントを使用して、次を呼び出して、ビュー内。

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

パラメーターに渡される、`InvokeAsync`メソッドです。 `PriorityList`アーティクルで開発されたビュー コンポーネントが呼び出されて、 *Views/Todo/Index.cshtml*ファイルを表示します。 次の場合、`InvokeAsync`メソッドが呼び出された 2 つのパラメーター。

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>としてタグ ヘルパーのビュー コンポーネントを呼び出す

ASP.NET Core 1.1 以降、としてビュー コンポーネントを呼び出すことができます、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

タグ ヘルパーのクラスとメソッドを pascal でパラメーターに変換された、 [kebab ケースを下げる](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)です。 ビュー コンポーネントを呼び出すタグ ヘルパーを使用して、`<vc></vc>`要素。 ビューのコンポーネントの指定は次のとおりです。

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

注: タグ ヘルパーとしてビュー コンポーネントを使用するために必要がありますアセンブリを登録するを使用して、ビュー コンポーネントを含む、`@addTagHelper`ディレクティブです。 たとえば、ビュー コンポーネントが"MyWebApp"と呼ばれるアセンブリ内にある場合は、ディレクティブを追加、次に、`_ViewImports.cshtml`ファイル。

```cshtml
@addTagHelper *, MyWebApp
```

ビュー コンポーネントを参照するすべてのファイルにタグ ヘルパーとしてビュー コンポーネントを登録することができます。 参照してください[タグ ヘルパーのスコープを管理する](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)タグ ヘルパーを登録する方法の詳細についてはします。

`InvokeAsync`このチュートリアルで使用されるメソッド。

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

タグ ヘルパーのマークアップ。

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

上記のサンプルでは、`PriorityList`表示コンポーネントになります`priority-list`です。 表示コンポーネントにパラメーターは、小文字 kebab 属性として渡されます。

### <a name="invoking-a-view-component-directly-from-a-controller"></a>コント ローラーから直接ビュー コンポーネントを呼び出す

コンポーネントの表示は通常、ビューから呼び出されますが、それらをコント ローラー メソッドから直接呼び出すことができます。 コンポーネントの表示は、コント ローラーなどのエンドポイントを定義していない、ときに、コント ローラーのアクションの内容を表すオブジェクトを簡単に実装できます、`ViewComponentResult`です。

この例では、表示コンポーネントは、コント ローラーから直接呼び出されます。

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>チュートリアル: 単純なビュー コンポーネントの作成

[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、ビルド、およびスタート コードをテストします。 単純なプロジェクトでは、`Todo`の一覧を表示するコント ローラー *Todo*項目。

![Todo などの一覧](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>ViewComponent クラスを追加します。

作成、 *ViewComponents*フォルダーに次の追加と`PriorityListViewComponent`クラス。

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

コードに関する注意事項

* ビュー クラスをコンポーネントに含まれていることができます**任意**プロジェクト内のフォルダーです。
* クラスの名前を PriorityList のため**ViewComponent**サフィックスで終わる**ViewComponent**、ランタイムから見ると、クラスのコンポーネントを参照するときに文字列"PriorityList"が使用されます。 について説明をさらに詳しく後述します。
* `[ViewComponent]`属性は、ビュー コンポーネントを参照するための名前を変更できます。 たとえば、でしたがという名前を付けてクラス`XYZ`と適用、`ViewComponent`属性。

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]`上の属性名を使用するビューの コンポーネントの選択を指示する`PriorityList`に関連付けられているコンポーネントでは、ビューから、クラスのコンポーネントを参照しているときに、文字列"PriorityList"を使用するビューを探すときにします。 について説明をさらに詳しく後述します。
* コンポーネントを使用して[依存性の注入](../../fundamentals/dependency-injection.md)データ コンテキストを使用できるようにします。
* `InvokeAsync`公開をビューから呼び出すことができるメソッドは、任意の数の引数を受け取ることができます。
* `InvokeAsync`メソッドのセットを返します`ToDo`を満たす項目、`isDone`と`maxPriority`パラメーター。

### <a name="create-the-view-component-razor-view"></a>ビューのコンポーネントの Razor ビューを作成します。

* 作成、*コンポーネント/ビュー/共有*フォルダーです。 このフォルダー**必要があります**という名前を付ける*コンポーネント*です。

* 作成、*ビュー、共有、コンポーネント/PriorityList*フォルダーです。 このフォルダー名がビュー コンポーネント クラスの名前またはサフィックス マイナス クラスの名前に一致する必要があります (規約の後に使用すると、 *ViewComponent*クラス名にサフィックス)。 使用した場合、`ViewComponent`属性、クラス名は、属性の指定に一致する必要があります。

* 作成、 *Views/Shared/Components/PriorityList/Default.cshtml* Razor ビュー。[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Razor ビューの一覧を受け取る`TodoItem`し、それらを表示します。 場合のビュー コンポーネント`InvokeAsync`メソッド (サンプルのように、)、ビューの名前に合格しなかった*既定*規則によって、ビュー名に使用します。 、このチュートリアルで後ほど説明するビューの名前を渡す方法です。 特定のコント ローラーの既定のスタイルを上書きするには、コント ローラーに固有のビュー フォルダーにビューを追加 (たとえば*Views/Todo/Components/PriorityList/Default.cshtml)*です。
    
    ビューのコンポーネントがコント ローラーに固有である場合は、コント ローラー固有のフォルダーに追加できます (*Views/Todo/Components/PriorityList/Default.cshtml*)。

* 追加、`div`の最下位に優先度リスト コンポーネントへの呼び出しを含む、 *Views/Todo/index.cshtml*ファイル。

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

マークアップ`@await Component.InvokeAsync`ビュー コンポーネントを呼び出すための構文を示しています。 最初の引数は、呼び出しを呼び出したりコンポーネントの名前です。 後続のパラメーターは、コンポーネントに渡されます。 `InvokeAsync`任意の数の引数を受け取ることができます。

アプリをテストします。 次の図は、ToDo リストと優先度の項目を示します。

![todo リストと優先順位の項目](view-components/_static/pi.png)

コント ローラーから直接のビュー コンポーネントを呼び出すこともできます。

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC アクションからの優先度のアイテム](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>ビュー名を指定します。

ビューの複雑なコンポーネントは、いくつかの条件下で、既定ではないビューを指定する必要があります。 次のコードから"PVC"ビューを指定する方法を示しています、`InvokeAsync`メソッドです。 更新プログラム、`InvokeAsync`メソッドで、`PriorityListViewComponent`クラスです。

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

コピー、 *Views/Shared/Components/PriorityList/Default.cshtml*ファイルという名前のビューを*Views/Shared/Components/PriorityList/PVC.cshtml*です。 PVC ビューが使用されているを示すために見出しを追加します。

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

更新*Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

アプリを実行して、PVC ビューを確認します。

![優先順位のビュー コンポーネント](view-components/_static/pvc.png)

PVC ビューは表示されず場合、は、優先度が 4 以上のビュー コンポーネントを呼び出していることを確認します。

### <a name="examine-the-view-path"></a>ビューのパスを調べる

* Priority ビューが返されないようには 3 つを以下の優先順位 パラメーターを変更します。
* 名前を一時的に、 *Views/Todo/Components/PriorityList/Default.cshtml*に*1Default.cshtml*です。
* アプリのテストは、次のエラーが表示されます。

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* コピー *Views/Todo/Components/PriorityList/1Default.cshtml*に*Views/Shared/Components/PriorityList/Default.cshtml*です。
* 一部のマークアップを追加、 *Shared* Todo ビュー コンポーネントを表示するビューは、 *Shared*フォルダーです。
* テスト、 **Shared**コンポーネント ビュー。

![共有コンポーネントの表示と ToDo 出力](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>マジック文字列の回避

時間の安全性をコンパイルする場合は、クラス名に、ハード コーディングされたビュー コンポーネント名を置き換えることができます。 "ViewComponent"サフィックスなしのビュー コンポーネントを作成します。

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

追加、 `using` 、Razor にステートメントがファイルを表示し、使用、`nameof`演算子。

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>その他のリソース

* [ビューへの依存性の注入](dependency-injection.md)
