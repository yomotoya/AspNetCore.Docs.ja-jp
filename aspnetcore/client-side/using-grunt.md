---
title: ASP.NET Core で Grunt を使用します。
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: fc912974fb6ed3c65bb46a7d616d9e531587d946
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894639"
---
# <a name="use-grunt-in-aspnet-core"></a>ASP.NET Core で Grunt を使用します。

によって[Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt は、スクリプトの縮小、TypeScript コンパイル、コード品質"lint"ツール、CSS プリプロセッサ クライアント開発をサポートするために行う必要がある繰り返し、作業だけを自動化する JavaScript タスク ランナーです。 ASP.NET プロジェクトのテンプレートでは、Gulp を使用して、既定では、grunt が Visual Studio でサポートされている完全 (を参照してください[Gulp を使用して、](using-gulp.md))。

この例では、開始位置として空の ASP.NET Core プロジェクトを使用して、最初からクライアントのビルド プロセスを自動化する方法について説明します。

完成した例では、ターゲットの配置ディレクトリを消去します、JavaScript ファイルを組み合わせて、コードの品質を確認します、JavaScript ファイルの内容でまとめますおよび web アプリケーションのルートにデプロイします。 次のパッケージを使用します。

* **grunt**:Grunt タスク ランナー パッケージです。

* **grunt-contrib-clean**:ファイルまたはディレクトリを削除するプラグイン。

* **grunt-contrib-jshint**:JavaScript コードの品質をレビューするプラグイン。

* **grunt-contrib-concat**:ファイルを 1 つのファイルに結合するプラグイン。

* **grunt contrib uglify**:サイズを小さく JavaScript の縮小をプラグインします。

* **grunt の contrib-ウォッチ**:ファイルのアクティビティを監視するプラグイン。

## <a name="preparing-the-application"></a>アプリケーションを準備します。

まず、新しい空の web アプリケーションをセットアップし、TypeScript ファイルの例を追加します。 TypeScript ファイルが自動的に既定の Visual Studio の設定を使用して JavaScript にコンパイルし、Grunt を使用して処理する材料になります。

1. Visual Studio で、作成、新しい`ASP.NET Web Application`します。

2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、ASP.NET Core を選択します。**空**テンプレート、[ok] ボタンをクリックします。

3. ソリューション エクスプ ローラーで、プロジェクトの構造を確認します。 `\src`フォルダーには、空`wwwroot`と`Dependencies`ノード。

    ![空の web ソリューション](using-grunt/_static/grunt-solution-explorer.png)

4. という名前の新しいフォルダーを追加`TypeScript`プロジェクト ディレクトリにします。

5. すべてのファイルを追加する前に Visual Studio オプションになっていることを確認します 'コンパイル保存時に' の TypeScript ファイルをチェックします。 移動します**ツール** > **オプション** > **テキスト エディター** > **Typescript**  > **プロジェクト**:

    ![TypeScript ファイルの自動コンパイルの設定のオプション](using-grunt/_static/typescript-options.png)

6. 右クリックし、`TypeScript`ディレクトリを選択します**追加 > 新しい項目の**コンテキスト メニュー。 選択、 **JavaScript ファイル**項目し、ファイルの名前*Tastes.ts* (注、 \*.ts 拡張機能)。 TypeScript コードの下の行をファイルにコピー (を保存すると、新しい*Tastes.js* JavaScript ソース ファイルが表示されます)。

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. 2 番目のファイルを追加、 **TypeScript**ディレクトリと名前を付けます`Food.ts`します。 ファイルに次のコードをコピーします。

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

## <a name="configuring-npm"></a>NPM の構成

次に、grunt、および grunt タスクをダウンロードする NPM を構成します。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加 > 新しい項目の**コンテキスト メニュー。 選択、 **NPM 構成ファイル**項目で、既定の名前をそのまま使用*package.json*、 をクリックし、**追加**ボタンをクリックします。

2. *Package.json*ファイル内、`devDependencies`中かっこをオブジェクトに、"grunt"を入力します。 選択`grunt`Intellisense を一覧から、Enter キーを押します。 Visual Studio は grunt パッケージ名を引用符で囲むし、コロンを追加します。 コロンの右側に、Intellisense リストの先頭から、パッケージの最新の安定バージョンを選択します。 (キーを押して`Ctrl-Space`Intellisense が表示されない場合)。

    ![grunt の Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM を使用して[セマンティック バージョニング](http://semver.org/)依存関係を整理します。 セマンティック バージョニング、SemVer とも呼ばれる、パッケージを識別する番号付けスキームで\<メジャー >.\<マイナー >。\<パッチ >。 Intellisense は、いくつかの一般的な選択肢のみを表示することによって、セマンティック バージョン管理を簡略化します。 Intellisense の一覧 (上記の例では 0.4.5) の一番上の項目は、パッケージの最新の安定バージョンと見なされます。 キャレット (^) 記号が最新のメジャー バージョンと一致して、一致する最新のマイナー バージョンをティルダ (~)。 参照してください、 [NPM semver バージョン パーサー参照](https://www.npmjs.com/package/semver)SemVer を提供する完全な表現へのガイドとして。

3. 依存関係の詳細を読み込む grunt の追加-contrib -\*のパッケージ*クリーン*、 *jshint*、 *concat*、 *uglify*、および*ウォッチ*次の例で示すようにします。 バージョンは、例では、一致する必要はありません。

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

DevDependencies の項目ごとに、パッケージは、各パッケージに必要なすべてのファイルと共にダウンロードされます。 内のパッケージ ファイルを見つけることができます、`node_modules`を有効にするディレクトリ、 **すべてのファイル**ソリューション エクスプ ローラーでボタンをクリックします。

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> 右クリックしてソリューション エクスプ ローラーで依存関係を手動で復元する場合は、`Dependencies\NPM`を選択して、**パッケージの復元**メニュー オプション。

![パッケージを復元します。](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Grunt の構成

Grunt がという名前のマニフェストで構成されている*Gruntfile.js*定義の読み込みし、登録を手動で実行または Visual Studio でのイベントに基づいて自動的に実行するように構成できるタスクです。

1. プロジェクトを右クリックして**追加 > 新しい項目の**します。 選択、 **Grunt 構成ファイル**オプションで、既定の名前をそのまま使用*Gruntfile.js*、 をクリックし、**追加**ボタンをクリックします。

   最初のコードには、モジュールの定義が含まれています。 および`grunt.initConfig()`メソッド。 `initConfig()`パッケージごとにオプションの設定に使用されると、モジュールの残りの部分は読み込みし、タスクを登録します。

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. 内で、`initConfig()`メソッド、オプションを追加、`clean`タスクの例で示すように*Gruntfile.js*以下。 Clean タスクでは、ディレクトリの文字列の配列を受け入れます。 このタスクは、wwwroot/ライブラリからファイルを削除し、全体/一時ディレクトリを削除します。

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. InitConfig() メソッドの呼び出しを追加`grunt.loadNpmTasks()`します。 これにより、タスク実行可能な Visual Studio から。

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. 保存*Gruntfile.js*します。 ファイルの内容は次のスクリーン ショットのようになります。

    ![初期の gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. 右クリック*Gruntfile.js*選択**Task Runner Explorer**コンテキスト メニュー。 Task Runner Explorer ウィンドウが開きます。

    ![タスク ランナー エクスプ ローラーのメニュー](using-grunt/_static/task-runner-explorer-menu.png)

6. いることを確認`clean`下に表示される**タスク**タスク ランナー エクスプ ローラーでします。

    ![タスク ランナー エクスプ ローラーのタスク一覧](using-grunt/_static/task-runner-explorer-tasks.png)

7. Clean タスクを右クリックして**実行**コンテキスト メニュー。 コマンド ウィンドウには、タスクの進行状況が表示されます。

    ![タスク ランナー エクスプ ローラーの実行クリーン タスク](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > まだをクリーンアップするファイルまたはディレクトリはありません。 必要に応じて、ソリューション エクスプ ローラーでそれらを手動で作成し、テストとして clean タスクを実行できます。

8. InitConfig() メソッドでのエントリを追加`concat`次のコードを使用します。

    `src`プロパティ配列を組み合わせる必要がありますの順序で結合します。 ファイルを一覧表示します。 `dest`プロパティが生成される結合されたファイルにパスを割り当てます。

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > `all`上記のコードでプロパティは、ターゲットの名前。 ターゲットは、複数のビルド環境を許可するいくつかの Grunt タスクで使用されます。 Intellisense を使用して組み込みのターゲットを表示または独自に割り当てることができます。

9. 追加、`jshint`タスクの次のコードを使用します。

    一時ディレクトリにあるすべての JavaScript ファイルに対して jshint コード品質のユーティリティを実行するとします。

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > オプション"-W069"がエラー時に生成される jshint JavaScript はつまり、ドット表記ではなくプロパティを割り当てるの構文を角かっこ`Tastes["Sweet"]`の代わりに`Tastes.Sweet`します。 オプションは、他のプロセスの続行を許可する警告をオフにします。

10. 追加、`uglify`タスクの次のコードを使用します。

    タスクの縮小、 *combined.js* ファイルが、一時ディレクトリにあるし、標準の命名規則に従った wwwroot/lib で結果ファイルを作成 *\<ファイル名\>min.js* 。

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. Grunt contrib クリーンアップが読み込まれる呼び出し grunt.loadNpmTasks() では、jshint、concat の同じ呼び出しを含めるし、uglify の次のコードを使用してください。

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. 保存*Gruntfile.js*します。 ファイルの内容は次の例のようになります。

    ![grunt の完全なファイルの例](using-grunt/_static/gruntfile-js-complete.png)

13. タスク ランナー エクスプ ローラーのタスクの一覧を含む通知`clean`、 `concat`、`jshint`と`uglify`タスク。 順序で各タスクを実行し、ソリューション エクスプ ローラーで結果を確認します。 各タスクは、エラーなく実行する必要があります。

    ![タスク ランナー エクスプ ローラーの各タスクの実行](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Concat タスクを作成、新しい*combined.js*ファイルし、temp ディレクトリに配置します。 Jshint タスクは単純に実行され、出力は生成されません。 新しい uglify のタスクを作成*combined.min.js*ファイルし、wwwroot/lib に配置します。 完了時に次のスクリーン ショットのようなもの、ソリューションはなります。

    ![ソリューション エクスプ ローラーのすべてのタスクします。](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > 各パッケージのオプションの詳細については、次を参照してください。 [ https://www.npmjs.com/ ](https://www.npmjs.com/)と参照の検索ボックスに、メイン ページで、パッケージ名。 たとえば、すべてのパラメーターを説明するドキュメント リンクを取得する grunt contrib クリーン パッケージを検索できます。

### <a name="all-together-now"></a>まとめ

Grunt を使用して、`registerTask()`メソッドを特定のシーケンスで一連のタスクを実行します。 たとえば、クリーンの順序で上記の手順 -> の例を実行する concat]-> [jshint]-> [uglify、モジュールに次のコードを追加します。 InitConfig 以外、loadNpmTasks() 呼び出しと同じレベルには、コードを追加する必要があります。

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

エイリアス タスク Task Runner Explorer に新しいタスクが表示されます。 右クリックし、その他のタスクと同様に実行できます。 `all`タスクが実行`clean`、 `concat`、`jshint`と`uglify`の順序で。

![エイリアスの grunt タスク](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>変更の監視

A`watch`タスク保持ファイルとディレクトリを監視します。 ウォッチは変更を検出した場合、タスクを自動的にトリガーされます。 InitConfig への変更を監視するに次のコードを追加\*TypeScript ディレクトリ内の .js ファイル。 JavaScript ファイルが変更された場合`watch`実行は、`all`タスク。

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

Task Runner Explorer で監視タスクを右クリックし、コンテキスト メニューから実行を選択します。 「待機しています...」が実行中の監視タスクを表示するコマンド ウィンドウに表示されます。メッセージ。 TypeScript ファイルのいずれかを開き、スペースを追加し、ファイルを保存します。 監視タスクのトリガーを順番に実行するその他のタスクをトリガーします。 次のスクリーン ショットは、サンプルの実行を示しています。

![タスク出力を実行しています。](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Visual Studio イベントへのバインド

タスクをバインドするには Visual Studio で作業するたびに、タスクを手動で開始する場合を除き、 **Before Build**、**後にビルド**、**クリーン**、および**プロジェクトのオープン**イベント。

バインドしてみます`watch`が Visual Studio を開くたびに実行されるようにします。 タスク ランナー エクスプ ローラーでは、監視タスクを右クリックして**バインド > プロジェクトを開く**コンテキスト メニュー。

![プロジェクトを開くタスクにバインドします。](using-grunt/_static/bindings-project-open.png)

アンロードし、プロジェクトの再読み込みします。 プロジェクトが再度読み込まれると、自動的に実行する監視タスクが開始されます。

## <a name="summary"></a>まとめ

Grunt は、ほとんどのクライアント ビルド タスクの自動化に使用できる強力なタスク ランナーです。 Grunt は、そのパッケージ、およびツールの Visual Studio との統合機能を提供する NPM を活用します。 Visual Studio のタスク ランナー エクスプ ローラーでは、構成ファイルへの変更を検出し、タスクの実行、実行中のタスクを表示、および Visual Studio のイベントにタスクを関連付けるに便利なインターフェイスを提供します。

## <a name="additional-resources"></a>その他の技術情報

* [Gulp の使用](using-gulp.md)
