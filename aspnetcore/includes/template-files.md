* Startup.cs:[スタートアップ クラス](../fundamentals/startup.md)-クラスは、アプリケーションに加えられたすべての要求を処理する要求パイプラインを構成します。
* Program.cs: [Program クラス](../fundamentals/index.md)アプリケーションのメイン エントリ ポイントを格納しています。
* firstapp.csproj:[プロジェクト ファイル](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj)ASP.NET Core アプリケーションの MSBuild プロジェクト ファイル形式。 プロジェクト間参照を含む NuGet の参照およびその他のプロジェクト関連の項目。
* される appsettings.json/appsettings です。Development.json: 環境ベース アプリの設定の構成ファイルです。 [構成を参照して](xref:fundamentals/configuration)です。
* bower.json: プロジェクトのパッケージの依存関係を Bower です。
* .bowerrc: bower 構成ファイルの Bower 資産をダウンロードするときに、コンポーネントをインストールする場所を定義します。
* bundleconfig.json: バンドルと縮小するフロント エンドの JavaScript と CSS の資産の構成ファイル。
* ビュー:、Razor ビューが含まれています。 ビューは、アプリのユーザー インターフェイス (UI) を表示するコンポーネントです。 一般に、この UI は、モデル データを表示します。
* コント ローラー: MVC コント ローラーを初期状態で含まれる*HomeController.cs*です。 コント ローラーは、ブラウザーの要求を処理するクラスです。
* wwwroot: Web アプリケーションのルート フォルダー。

詳細については、次を参照してください。 [、MVC パターン](xref:mvc/overview)です。
