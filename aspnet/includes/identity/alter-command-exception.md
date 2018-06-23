> <span data-ttu-id="0f0fb-101">いくつかのコマンドは、アプリは、その Id のデータ ストアとして SQLite を使用している場合にサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="0f0fb-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="0f0fb-102">データベース エンジンの制限のため`Alter`コマンドは、次の例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0f0fb-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="0f0fb-103">"System.NotSupportedException: SQLite では、この移行操作はサポートされていません"。</span><span class="sxs-lookup"><span data-stu-id="0f0fb-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="0f0fb-104">回避策としては、テーブルを変更するには、データベース、Code First migrations を実行します。</span><span class="sxs-lookup"><span data-stu-id="0f0fb-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
