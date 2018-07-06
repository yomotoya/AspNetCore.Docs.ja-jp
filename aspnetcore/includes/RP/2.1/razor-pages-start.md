既定のテンプレートでは、**[RazorPagesMovie]**、**[ホーム]**、**[バージョン情報]**、および **[連絡先]** のリンクとページが作成されます。 ブラウザー ウィンドウのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。

![ホームまたはインデックス ページ](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

リンクをテストします。 **[RazorPagesMovie]** と **[ホーム]** のリンクをクリックすると、インデックス ページに移動します。 **[バージョン情報]** と **[連絡先]** のリンクをクリックすると、それぞれ `About` と `Contact` のページに移動します。

## <a name="project-files-and-folders"></a>プロジェクトのファイルとフォルダー

次の表に、プロジェクトのファイルとフォルダーをリストします。 このチュートリアルでは、*Startup.cs* ファイルを理解することが最も重要です。 以下に示す各リンクをレビューする必要はありません。 リンクは、プロジェクトのファイルまたはフォルダーについて詳しい情報が必要な場合の参照として提供されているものです。

| ファイルまたはフォルダー | 目的 |
| -------------- | ------- |
| *wwwroot* | 静的資産が含まれています。 [静的ファイル](xref:fundamentals/static-files)に関するページを参照してください。 |
| *ページ* | [Razor ページ](xref:razor-pages/index)のフォルダー。 |
| *appsettings.json* | [構成](xref:fundamentals/configuration/index) |
| *Program.cs* | ASP.NET Core アプリの[ホスト](xref:fundamentals/host/index)を構成します。 |
| *Startup.cs* | サービスと要求パイプラインを構成します。 [スタートアップ](xref:fundamentals/startup)に関するページを参照してください。 |

### <a name="the-pagesshared-folder"></a>ページ/共有フォルダー

*_Layout.cshtml* ファイルは共通の HTML 要素 (スクリプトとスタイルシート リンク) を含み、アプリのレイアウトを設定します。 たとえば、**RazorPagesMovie**、**Home**、**About**、または **Contact** を選択するときには、要素の共通のセットが Web ページに表示されます。 共通の要素には、ウィンドウの下部のヘッダーと上部のナビゲーション メニューが含まれます。 詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。

*_ValidationScriptsPartial.cshtml* ファイルでは、[jQuery](https://jquery.com/) 検証スクリプトへの参照が提供されます。 チュートリアルの後半で `Create` および `Edit` ページを追加するときに、*_ValidationScriptsPartial.cshtml* ファイルが使用されます。

*_CookieConsentPartial.cshtml* ファイルは、ナビゲーション バー、およびプライバシーと cookie 使用ポリシーを要約するコンテンツを提供します。 プロジェクトに含まれる GDPR 資産の詳細については、「[ASP.NET Core でのヨーロッパ一般的なデータ保護規制 (GDPR) のサポート](xref:security/gdpr)」を参照してください。

### <a name="the-pages-folder"></a>Pages フォルダー

*_ViewStart.cshtml* では、*_Layout.cshtml* ファイルを使用するように Razor ページの `Layout` プロパティを設定します。 詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。

*_ViewImports.cshtml* ファイルには、各 Razor ページにインポートされた Razor ディレクティブが含まれています。 詳細については、「[Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)」 (共有ディレクティブのインポート) を参照してください。

`About`、`Contact` および `Index` ページは基本ページで、アプリを起動する場合に使用できます。 エラー情報を表示する場合は `Error` ページを使用します。
