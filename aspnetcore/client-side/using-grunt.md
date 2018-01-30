---
title: "ASP.NET Core での Grunt の使用"
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: c23f170b36ac1b9623835337020f2b5ac9514971
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="using-grunt-in-aspnet-core"></a>ASP.NET Core での Grunt の使用 

によって[ノエル ライス](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt は、スクリプトの縮小、TypeScript のコンパイルをコード品質の「柔らかい」ツール、CSS 前プロセッサ、およびクライアントの開発をサポートするために行う必要がある反復的な面倒だけに関するを自動化する JavaScript タスク ランナーです。 Grunt は ASP.NET プロジェクト テンプレートが既定で Gulp を使用しても、完全に Visual Studio で、サポート (を参照してください[Gulp を使用して](using-gulp.md))。

この例では、その開始ポイントとして空の ASP.NET Core プロジェクトを使用して、最初からクライアントのビルド プロセスを自動化する方法を示します。

完了の例では、ターゲット配置ディレクトリをクリーンアップ、結合の JavaScript ファイル、コードの品質を確認、JavaScript ファイルの内容を圧縮し、web アプリケーションのルートに配置します。 次のパッケージが使用します。

* **grunt**:「Grunt タスク ランナー パッケージです。

* **grunt contrib クリーンアップ**: ファイルまたはディレクトリを削除するプラグイン。

* **grunt contrib-jshint**: JavaScript コードの品質をレビューするプラグイン。

* **grunt contrib-concat**: 1 つのファイルのファイルを結合するプラグイン。

* **grunt contrib uglify**: サイズを縮小する JavaScript の縮小をプラグインします。

* **grunt contrib-ウォッチ**: ファイルのアクティビティを監視するプラグイン。

## <a name="preparing-the-application"></a>アプリケーションを準備します。

を開始するには、新しい空の web アプリケーションを設定し、TypeScript ファイルの例を追加します。 TypeScript ファイルは、自動的に既定の Visual Studio の設定を使用して JavaScript にコンパイルされ、Grunt を使用して処理する、原材料となります。

1.  Visual Studio で、新しい`ASP.NET Web Application`です。

2.  **新しい ASP.NET プロジェクト**ダイアログ ボックスで、選択 ASP.NET Core**空**テンプレートし、[ok] ボタンをクリックします。

3.  ソリューション エクスプ ローラーで、プロジェクトの構造を確認します。 `\src`フォルダーに空`wwwroot`と`Dependencies`ノード。

    ![空の web ソリューション](using-grunt/_static/grunt-solution-explorer.png)

4.  という名前の新しいフォルダーを追加`TypeScript`プロジェクト ディレクトリにします。

5.  すべてのファイルを追加する前に、Visual Studio オプションが存在することを確認してください 'コンパイル on save' の TypeScript ファイルをチェックします。 移動**ツール** > **オプション** > **テキスト エディター** > **Typescript**  > **プロジェクト**:

    ![オプションの TypeScript ファイルの自動 compliation の設定](using-grunt/_static/typescript-options.png)

6.  右クリックし、`TypeScript`ディレクトリおよび select**追加 > 新しい項目の**コンテキスト メニュー。 選択、 **JavaScript ファイル**項目し、ファイルの名前*Tastes.ts* (注、 \*.ts 拡張子) です。 TypeScript コードの下の行をファイルにコピー (を保存すると、新しい*Tastes.js* JavaScript ソース ファイルが表示されます)。
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  2 番目のファイルを追加、 **TypeScript**ディレクトリし名前を付けます`Food.ts`です。 ファイルに次のコードをコピーします。

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a>NPM を構成します。

次に、NPM grunt と grunt タスク ダウンロードを構成します。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加 > 新しい項目の**コンテキスト メニュー。 選択、 **NPM 構成ファイル**項目で、既定の名前をそのまま使用*package.json*、 をクリックし、**追加**ボタンをクリックします。

2. *Package.json*ファイル内、`devDependencies`中かっこをオブジェクトに、"grunt"を入力します。 選択`grunt`intellisense を一覧表示し、Enter キーを押します。 Visual Studio が grunt パッケージ名、引用符で囲むし、コロンを追加します。 コロンの右に、Intellisense の一覧の先頭から、パッケージの最新の安定したバージョンを選択します (キーを押して`Ctrl-Space`Intellisense が表示されないかどうか)。

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > NPM を使用して[セマンティック バージョニング](http://semver.org/)の依存関係を整理します。 セマンティック バージョニング、SemVer とも呼ばれる、番号付けスキームでパッケージを識別する<major>.<minor>です。<patch>.Intellisense は、いくつかの一般的な選択肢のみを表示することによって、セマンティック バージョニングを簡略化します。 Intellisense の一覧 (上記の例では 0.4.5) の最上位の項目は、パッケージの最新の安定バージョンと見なされます。 キャレット (^) 記号が最新のメジャー バージョンと一致し、ティルダ (~) には、最新のマイナー バージョンが一致します。 参照してください、 [NPM semver バージョン パーサー参照](https://www.npmjs.com/package/semver)SemVer を提供するすべての表現性をガイドとして。

3. 追加の依存関係の詳細を読み込む grunt-contrib -\*パッケージ化*クリーン*、 *jshint*、 *concat*、 *uglify*、および*ウォッチ*次の例で示すようにします。 バージョンの例に一致する必要はありません。

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. 保存、 *package.json*ファイル。

DevDependencies 項目ごとに、パッケージは、各パッケージが必要なすべてのファイルと共にダウンロードされます。 内のパッケージ ファイルを見つけることができます、`node_modules`ディレクトリを有効にして、 **すべてのファイル**ソリューション エクスプ ローラーでボタンをクリックします。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 右クリックして、ソリューション エクスプ ローラー内の依存関係を手動で復元する必要がある場合、`Dependencies\NPM`を選択して、**パッケージの復元**メニュー オプション。

![パッケージを復元します。](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Grunt を構成します。

Grunt、という名前のマニフェストを使用して構成*Gruntfile.js*を定義、読み込まれ、タスクを手動で実行したりできる Visual Studio でのイベントに基づいて自動的に実行するように構成を登録します。

1.  プロジェクトを右クリックし **追加 > 新しい項目の**します。 選択、 **Grunt 構成ファイル**オプションで、既定の名前、 *Gruntfile.js*、 をクリックし、**追加**ボタンをクリックします。

    最初のコードには、モジュール定義が含まれています。 および`grunt.initConfig()`メソッドです。 `initConfig()`パッケージごとにオプションの設定に使用されると、モジュールの残りの部分がロードされ、タスクを登録します。
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. 内部、`initConfig()`メソッドのオプションを追加、`clean`タスクの例で示すように*Gruntfile.js*下です。 Clean タスクは、ディレクトリの文字列の配列を受け入れます。 このタスクは、wwwroot/lib からファイルを削除し、全体/一時ディレクトリを削除します。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. 下 initConfig() メソッド呼び出しを追加して`grunt.loadNpmTasks()`です。 これにより、タスク実行可能な Visual Studio から。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. 保存*Gruntfile.js*です。 ファイルは次のスクリーン ショットのようになります。

    ![初期の gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. 右クリック*Gruntfile.js*選択**タスク ランナー エクスプ ローラー**コンテキスト メニュー。 タスク ランナー エクスプ ローラー ウィンドウが開きます。

    ![タスク ランナー エクスプ ローラーのメニュー](using-grunt/_static/task-runner-explorer-menu.png)

6. いることを確認`clean`の下に表示**タスク**タスク ランナー エクスプ ローラーでします。

    ![タスク ランナー エクスプ ローラーのタスク一覧](using-grunt/_static/task-runner-explorer-tasks.png)

7. Clean タスクを右クリックし **実行**コンテキスト メニュー。 コマンド ウィンドウには、タスクの進行状況が表示されます。

    ![タスク ランナー エクスプ ローラーの実行のクリーン タスク](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > まだクリーンアップするファイルまたはディレクトリはありません。 必要に応じて、ソリューション エクスプ ローラーでそれらを手動で作成し、テスト用に clean タスクを実行できます。
    
8. InitConfig() メソッドでのエントリを追加`concat`次のコードを使用します。

    `src`プロパティ配列を組み合わせる必要があります順番、結合のファイルを一覧表示します。 `dest`プロパティが生成される結合されたファイルにパスを割り当てます。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all`上記のコードでのプロパティは、ターゲットの名前。 ターゲットは、複数のビルド環境を許可する Grunt タスクの一部で使用されます。 Intellisense を使用して組み込みのターゲットを表示または、自分に割り当てることができます。
    
9. 追加、`jshint`タスクの次のコードを使用します。

    Jshint コード品質のユーティリティは、一時ディレクトリにあるすべての JavaScript ファイルに対して実行されます。
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > オプション"-W069"がエラー時に生成される jshint JavaScript の使用に右山かっこ構文例: ドット付き表記ではなくプロパティを割り当てる`Tastes["Sweet"]`の代わりに`Tastes.Sweet`です。 オプションは、残りのプロセスの続行を許可する警告をオフにします。

10.  追加、`uglify`タスクの次のコードを使用します。

    タスクの縮小、 *combined.js*ファイルが、一時ディレクトリにあるし、標準の命名規則に従った wwwroot/lib で結果ファイルを作成*\<ファイル名\>min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. Grunt contrib クリーンアップをロードする呼び出し grunt.loadNpmTasks()、jshint、concat の同時呼び出しが含まれてし、uglify 次のコードを使用します。
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. 保存*Gruntfile.js*です。 ファイルは次の例のようになります。

    ![完全な grunt ファイルの例](using-grunt/_static/gruntfile-js-complete.png)

13. タスク ランナー エクスプ ローラーのタスク一覧には、通知`clean`、 `concat`、`jshint`と`uglify`タスク。 順序で各タスクを実行し、ソリューション エクスプ ローラーで結果を確認します。 各タスクは、エラーなく実行する必要があります。
    
    ![各タスクの実行タスク ランナー エクスプ ローラー](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Concat タスクが新たに作成*combined.js*ファイルし、temp ディレクトリに配置します。 単に jshint タスクを実行し、出力は生成されません。 新たに作成 uglify タスク*combined.min.js*ファイルし、wwwroot/lib に配置します。 完了時に、ソリューションはようになります次のスクリーン ショット。
    
    ![ソリューション エクスプ ローラーのすべてのタスクします。](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > 各パッケージのオプションの詳細については、次を参照してください。 [https://www.npmjs.com/](https://www.npmjs.com/)とメイン ページで、[検索] ボックスで、パッケージ名を検索します。 たとえば、すべてのパラメーターを説明するドキュメントのリンクを取得する grunt contrib クリーンアップ パッケージを検索できます。

### <a name="all-together-now"></a>まとめ

使用して、Grunt`registerTask()`特定のシーケンスで一連のタスクを実行するメソッド。 たとえば、クリーンな順序で上記の手順 -> 例を実行する concat]-> [jshint]-> [uglify、モジュールに、以下のコードを追加します。 コードは、initConfig 外の loadNpmTasks() 呼び出しと同じレベルに追加する必要があります。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

タスク ランナー エクスプ ローラーの エイリアスのタスクに新しいタスクが表示されます。 右クリックし、その他のタスクと同様に実行することができます。 `all`タスクは実行`clean`、 `concat`、`jshint`と`uglify`の順序で。

![エイリアス grunt タスク](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>変更の監視

A`watch`タスクは、ファイルおよびディレクトリで、監視を保持します。 ウォッチは変更を検出した場合、タスクを自動的にトリガーされます。 変更を監視する initConfig に次のコードを追加\*TypeScript ディレクトリ内の .js ファイル。 JavaScript ファイルが変更された場合、`watch`実行は、`all`タスク。

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

呼び出しを追加して`loadNpmTasks()`を表示する、`watch`タスク ランナー エクスプ ローラーで作業します。

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

タスク ランナー エクスプ ローラーで、監視タスクを右クリックし、コンテキスト メニューの [実行] を選択します。 「待機しています...」を実行中の監視タスクを示しています。 コマンド ウィンドウが表示されます。 メッセージが表示されます。 TypeScript ファイルの 1 つを開き、、スペースを追加し、ファイルを保存します。 監視タスクをトリガーされ順序で実行するその他のタスクをトリガーします。 次のスクリーン ショットは、サンプルの実行を示します。

![タスクの出力を実行しています。](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Visual Studio イベントへのバインド

タスクをバインドするには Visual Studio で作業するたびに、タスクを手動で開始する場合を除き、**する前にビルド**、**後にビルド**、**クリーン**、および**プロジェクトのオープン**イベント。

バインドしてみます`watch`Visual Studio が開くたびに実行されるようにします。 タスク ランナー エクスプ ローラーで、監視タスクを右クリックし、選択**バインド > プロジェクトを開く**コンテキスト メニュー。

![プロジェクトを開くにはタスクをバインドします。](using-grunt/_static/bindings-project-open.png)

アンロードし、プロジェクトの再読み込みします。 プロジェクトが再度読み込まれると、自動的に実行されている、監視タスクが開始されます。

## <a name="summary"></a>まとめ

Grunt は、ほとんどのクライアント ビルド タスクの自動化に使用できる強力なタスク ランナーです。 Grunt は、そのパッケージ、およびツールを Visual Studio との統合機能を提供する NPM を活用します。 Visual Studio のタスク ランナー エクスプ ローラーでは、構成ファイルに対する変更を検出し、タスクの実行、実行中のタスクを表示、および Visual Studio のイベントにタスクを関連付ける便利なインターフェイスを提供します。

## <a name="additional-resources"></a>その他の技術情報

   * [Gulp の使用](using-gulp.md)
