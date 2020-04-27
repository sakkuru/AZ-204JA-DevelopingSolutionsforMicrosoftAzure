﻿---
lab:
    title: 'ラボ: Azure Functions を使用してタスク処理ロジックを実装する'
    module: 'モジュール 02: Azure Functions の実装'
    type: 'Answer Key'
---

# ラボ: Azure Functions を使用してタスク処理ロジックを実装する
# 学生課題解答キー

## Microsoft Azure ユーザー インターフェイス

Microsoft クラウド ツールのダイナミックな特性を考えると、このトレーニング コンテンツの開発後に Azure ユーザー インターフェイスの変更が発生する可能性があります。これらの変更により、ラボの指示と手順が一致しない場合があります。

Microsoft では、コミュニティから必要な変更要望を受けたときにトレーニング コースを更新します。ただし、クラウドの更新は頻繁に行われるため、トレーニング コンテンツの更新前に UI が変更される場合もあります。**その場合は、変更に適宜対応して、ラボで要求されている内容を処理してください。**

## 手順

### 開始する前に

#### ラボの仮想マシンへのログイン

次の認証情報を使用して、Windows 10 仮想マシン (VM) にログインします。
    
-   ユーザー名: **Admin**

-   パスワード: **Pa55w.rd**

> **注意**: 仮想ラボ環境に接続する手順は講師が説明します。

#### インストールされたアプリケーションのレビュー

Windows 10 デスクトップでタスク バーを探します。タスク バーには、この課題で使用するアプリケーションのアイコンが含まれています:
    
-   Microsoft Edge

-   File Explorer

-   Windows Terminal

### 演習 1: Azure のリソースを作成する

#### タスク 1: Azure portal を開く

1.  タスクバーで、**Microsoft Edge** アイコンを選択します。

1.  開いているブラウザー ウィンドウで、「Azure portal」(<https://portal.azure.com>)に移動します。

1.  Microsoft アカウントの電子メール アドレスを入力し、 「**次へ**」 を選択します。

1.  Microsoft アカウントのパスワードを入力し、「**サインイン**」を選択します。

    > **注**: Azure portal に初めてログインする場合は、ポータルのツアーが表示されます。ツアーをスキップしてポータルの使用を開始するには、「**開始する**」 を選択します。

#### タスク 2: Azure Storage アカウントの作成

1.  Azure portal のナビゲーション ペインで、「**すべてのサービス**」 を選択します。

1.  「**すべてのサービス**」 ブレードで、「**ストレージ アカウント**」 を選択します。

1.  「**ストレージア カウント**」 ブレードで、ストレージ アカウントの一覧を表示します。

1.  「**ストレージ アカウント**」 ブレードで、「**追加**」 を選択します。

1.  「**ストレージ アカウントを作成**」 ブレードで、「**基本**」、「**タグ**」、「**レビュー + 作成**」 などのブレード上のタブを確認します。

    > **注**: 各タブは、新しいストレージ アカウントを作成するための、ワークフロー内のステップを表しています。いつでも 「**確認および作成**」 を選択して、残りのタブをスキップできます。

1.  **基本** タブを選択し、タブ領域で次の操作を実行します。
    
    1.  「**サブスクリプション**」 テキストボックスは既定値のままにします。
    
    1.  「**リソース グループ**」 セクションで、「**新規作成**」 を選択して、「**Serverless**」を入力し、「**OK**」 を選択します。
    
    1.  「**ストレージ アカウント名**」 テキストボックスに「**funcstor*[yourname]***」と入力します。
    
    1.  「**場所**」リストで、**(US) 米国東部**リージョンを選択します。
    
    1.  「**パフォーマンス**」 セクションで、「**標準**」 を選択します。
    
    1.  「**アカウントの種類**」 一覧で、「**StorageV2 (general purpose v2)**」 を選択します。
    
    1.  「**レプリケーション**」 リストで、「**ローカル冗長ストレージ (LRS)**」 を選択します。
    
    1.  「**アクセス層 (既定)**」 セクションで、「**ホット**」 が選択されていることを確認します。
    
    1.  「**確認および作成**」 を選択します。

1.  **確認および作成**タブで、以前の手順で指定したオプションを確認します。

1.  「**作成**」を選択し、指定した構成を使用してストレージ アカウントを作成します。

    > **注**: **デプロイ** ブレードで次に進む前に、作成タスクが完了するまで待ちます。

#### タスク 3: Azure Functions アプリを作成する

1.  Azure portal のナビゲーション ペインで、「**リソースを作成**」 リンクを選択します。

1.  **「新規」** ブレードで、「**Marketplace を検索**」 というテキスト ボックスを探します。

1.  検索テキスト ボックスに「**関数**」と入力し、Enter を押します。

1.  **すべて** の検索結果ブレードで、 **関数アプリ**の結果を選択します。

1.  **Function App** ブレードで、「**作成**」 を選択します。

1.  「**関数アプリ**」 ブレードで、「**基本**」 などのタブを検索します。

    > **注**: 各タブは、新しい関数アプリを作成するワークフローのステップを表しています。いつでも 「**確認および作成**」 を選択して、残りのタブをスキップできます。

1.  「**基本**」 タブで、次の操作を実行します。
    
    1.  「**サブスクリプション**」 ボックスは既定値のままにします。
    
    1.  「**リソースグループ**」 セクションで、「**既存のものを使用**」 を選択し、リスト上から 「**サーバーレス**」 を選択します。
    
    1.  「**関数アプリ名**」 ボックスに「**funclogic*[yourname]***」と入力します。

    1.  「**発行**」 セクションで、「**コード**」 を選択します。

    1.  「**ランタイム スタック**」 ドロップダウン リストで、「**NET Core**」を選択します。

    1.  「**リージョン**」 ドロップダウン リストで、「**米国東部**」 リージョンを選択します。
    
    1.  「**次へ: ホスティング**」 を選択します。

1.  「**ホスティング**」 タブで、次の操作を実行します。

    1.  「**ストレージ アカウント**」 ドロップダウンリストで、 この課題で前に作成したストレージ アカウント **funcstor*[yourname]*** を選択します。

    1.  「**オペレーティングシステム**」 セクションで、「**Windows**」 を選択します。

    1.  「**プランタイプ**」 ドロップダウンリストで、「**Consumption**」 オプションを選択します。

    1.  「**次へ: モニタリング**」 を選択する。

1.  「**モニタリング**」 タブで、次の操作を実行します。

    1.  「**Application Insights を有効にする**」 セクションで、「**いいえ**」 を選択します。

    1.  「**レビュー + 作成**」 を選択します。

1.  「**レビュー + 作成**」 タブで、前の手順で選択したオプションをレビューします。

1.  「**作成**」 を選択し、指定した構成を使用して関数アプリを作成します。 

    > **注**: 演習を進める前に、作成タスクが完了するまで待ちます。

#### レビュー

このラボでは、このラボで使用するすべてのリソースを作成しました。

### 演習 2: HTTP 要求によってトリガーされる関数の作成

#### タスク 1: HTTP によってトリガーされる関数を作成する

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した **サーバーレス** リソースグループを検索して選択します。

1.  「**Serverless**」 ブレードで、この課題で前に作成した関数アプリ **funclogic*[yourname]*** を選択します。

1.  「**関数アプリ**」 ブレードで、 「**関数**」 ドロップダウン リストの横にあるプラス記号 (**+**) を選択します。

1.  「**新しい Azure 関数**」 のクイックスタートで、次のアクションを実行します。
    
    1.  「**開発環境の選択**」 ヘッダーで、「**ポータル内**」 を選択し、「**続ける**」 を選択します。
    
    1.  「**関数を作成**」 ヘッダーで、「**その他のテンプレート**」 を選択し、「**終了してテンプレートを表示**」 を選択します。
    
    1.  テンプレートの一覧で、「**HTTP トリガー**」 を選択します。
    
    1.  「**新しい関数**」 ポップアップウィンドウで、「**名前**」 のテキスト ボックスを検索し、「**Echo**」を入力します。
    
    1.  **新機能**ポップアップウィンドウで、**認証レベル**リストを検索し、**匿名**を選択します。
    
    1.  「**新しい関数**」 ポップアップ ウィンドウで、「**作成**」 を選択します。

#### タスク 2: 書き込みコードの記述

1.  関数エディターで、関数スクリプト **run.csx** のサンプルを確認します。

    ```
    #r "Newtonsoft.Json"

    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        name = name ?? data?.name;

        return name != null
            ? (ActionResult)new OkObjectResult($"Hello, {name}")
            : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
    }
    ```

1.  サンプル コードをすべて削除します。

1.  次のコード行を追加して、名前空間 **Microsoft.AspNetCore.Mvc** をインポートします。

    ```
    using Microsoft.AspNetCore.Mvc;
    ```

1.  次のコード行を追加して、名前空間 **System.Net** をインポートします。

    ```
    using System.Net;
    ```

1.  次のコードブロックを追加して「**Run**」という名前の新しい **public static** メソッドを作成します。このメソッドは 、タイプ **IActionResult** の変数を返し、タイプ **HttpRequest** および **ILogger** の変数を、*req* および *log* という名前のパラメーターとして受け取ります。

    ```
    public static IActionResult Run(HttpRequest req, ILogger log)
    {
    }
    ```

1.  **Run** メソッドで次のコード行を入力して、固定メッセージをレンダリングします。

    ```
    log.LogInformation("Received a request");
    ```

1.  次のコード行を入力して、HTTP リクエストのボディを HTTP レスポンスとして出力します。

    ```
    return new OkObjectResult(req.Body);
    ```

1.  「**run.csx**」 ファイルを確認します。 以下が含まれているはずです。

    ```
    using System.Net;
    using Microsoft.AspNetCore.Mvc;

    public static IActionResult Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("Received a request");

        return new OkObjectResult(req.Body);
    }
    ```

1.  **保存** を選択してスクリプトを保存します。

#### タスク 3: ポータルで関数実行をテストする

1.  **ログ** を選択します。

1.  コンパイル結果を確認します。結果には、「コンパイルに成功しました」というメッセージが含まれている必要があります。

1.  「**実行**」 を選択して関数をテストします。

1.  テストの実行結果を確認します。結果は、元の要求本文を正確にエコーする必要があります。

#### タスク 4: 基本関数の URL の取得

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した "**サーバーレス**" リソースグループを検索して選択します。

1.  「**Serverless**」 ブレードで、この課題で前に作成した関数アプリ **funclogic*[yourname]*** を選択します。

1.  「**関数アプリ**」 ブレードで、「**URL**」 テキスト ボックスの値をコピーします。この値は、このラボの後半で使用します。

#### タスク 5: httprepl を使用して関数の実行をテストする

1.  タスク バーで、**Windows Terminal** アイコンを選択します。

1.  コマンド プロンプトで次のコマンドを入力して **httprepl** ツールを起動し、この課題で前にコピーした URL の値をベース URI (Uniform Resource Identifier) として設定します。

    ```
    httprepl <function-app-url>
    ```

    > **注**: 例えば、URL が **https://funclogicstudent.azurewebsites.net** の場合、コマンドは **httprepl https://funclogicstudent.azurewebsites.net** になります。   

1.  ツール プロンプトで次のコマンドを入力し、Enterを選択すると、対応する **API** ディレクトリを参照します。

    ```
    cd api
    ```

1.  次のコマンドを入力し、Enter キーを押して、対応する **echo** ディレクトリを参照します。

    ```
    cd echo
    ```

1.  次のコマンドを入力して **post** コマンドを実行し、**--content** オプションで数値 **3** を設定した HTTP 要求本文を送ります。

    ```
    post --content 3
    ```

1.  次のコマンドを入力して **post** コマンドを実行し、「**--content**」 オプションで数値 「**5**」 を設定した HTTP 要求本文を送ります。

    ```
    post --content 5
    ```

1.  次のコマンドを入力して 「**post**」 コマンドを実行し、「**--content**」 オプションで文字列 「**Hello**」 を設定した HTTP 要求本文を送ります。

    ```
    post --content "Hello"
    ```

1.  次のコマンドを入力して **post** コマンドを実行し、**{"msg": "成功"}** の値を JavaScript Object Notation (JSON) に設定した HTTP 要求本文を送ります。**--content** オプションを使用して:

    ```
    post --content "{"msg": "Successful"}"
    ```

1.  次のコマンドを入力してから、Enter を選択して **httprepl** アプリケーションを終了します。

    ```
    exit
    ```

1.  現在実行中の Windows Terminal アプリケーションを閉じます。

1.	Azure portal が表示されているブラウザー ウインドウに戻ります。

#### レビュー

この演習では、HTTP POST 要求を介して送信されたコンテンツをエコーする基本的な関数を作成しました。

### エクササイズ 3: スケジュールに基づいてトリガーする関数の作成

#### タスク 1: スケジュールでトリガーする関数の作成

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した "**サーバーレス**" リソースグループを検索して選択します。

1.  「**Serverless**」 ブレードで、この課題で前に作成した関数アプリ **funclogic*[yourname]*** を選択します。

1.  「**関数アプリ**」 ブレードで、 「**関数**」 ドロップダウン リストの横にあるプラス記号 (**+**) を選択します。

1.  「**新しい Azure 関数**」 のクイックスタートで、次のアクションを実行します。
        
    1.  テンプレートの一覧で、「**HTTP トリガー**」 を選択します。
    
    1.  「**新しい関数**」 ポップアップウィンドウで、「**名前**」 テキストボックスを確認し「**Recurring**」を入力します。
    
    1.  「**新しい関数**」 ポップアップウィンドウで、「**スケジュール**」 テキスト ボックスを確認し「**0 \* \* \* \* \***」を入力します。
    
    1.  「**新しい関数**」 ポップアップ ウィンドウで、「**作成**」 を選択します。

#### タスク 2: 関数の実行を確認する

1.  関数エディターで 「**保存**」 を選択し、既定の関数として登録します。

1.  **ログ**を選択します。

1.  約 1 分ごとに実行される関数を確認します。各関数の実行は、ログに単純なメッセージを書き込む必要があります。

#### タスク 3: 関数の統合構成を更新する

1.  「**関数アプリ**」 ブレードに戻り、次の操作を実行します。

    1.  この課題で前に作成した **funclogic*[yourname]*** 関数アプリのノードを展開します。

    1.  「**関数**」ノードを展開します。

    1.  その特定の関数の **Recurring** ノードを展開します。

    1.  **統合**ノードを選択します。

1.  「**統合**」 セクションで、次の操作を実行します。

    1.  「**トリガー**」 セクションの 「**タイマー (myTimer)**」 オプションを選択 します。

    1.  「**スケジュール**」 テキストボックスに、値 「**\*/30 \* \* \* \* \***」 を入力します。

    1.  「**保存**」 を選択します。

#### タスク 4: 関数の実行を確認する

1.  「**関数アプリ**」 ブレードに戻り、次の操作を実行します。

    1.  この課題で前に作成した **funclogic*[yourname]*** 関数アプリのノードを展開します。

    1.  「**関数**」ノードを展開します。

    1.  その特定の関数の **Recurring** ノードを選択します。

1.  関数エディターで、「**ログ**」 を選択します。

1.  約 30 秒ごとに実行される関数の実行を監視します。各関数の実行は、ログに単純なメッセージを書き込む必要があります。

#### レビュー

この演習では、固定スケジュールに基づいて自動的に実行される関数を作成しました。

### 演習 4: 他のサービスと統合する関数を作成する

#### タスク 1: HTTP によってトリガーされる関数を作成する

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した "**サーバーレス**" リソースグループを検索して選択します。

1.  「**Serverless**」 ブレードで、この課題で前に作成した関数アプリ **funclogic*[yourname]*** を選択します。

1.  「**関数アプリ**」 ブレードで、 「**関数**」 ドロップダウン リストの横にあるプラス記号 (**+**) を選択します。

1.  「**新しい Azure 関数**」 のクイックスタートで、次のアクションを実行します。
        
    1.  テンプレートの一覧で、「**HTTP トリガー**」 を選択します。
    
    1.  「**新しい関数**」 ポップアップウィンドウで、「**名前**」 テキスト ボックスに「**GetSettingInfo**」と入力します。
    
    1.  「**新しい関数**」 ポップアップウィンドウで、「**認証レベル**」 リストを検索し、「**匿名**」 を選択します。
    
    1.  「**新しい関数**」 ポップアップ ウィンドウで、「**作成**」 を選択します。

#### タスク 2: サンプル コンテンツをアップロードする

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した "**サーバーレス**" リソースグループを検索して選択します。

1.  「**サーバーレス**」 ブレードで、 この課題で前に作成したストレージ アカウント **funcstor*[yourname]*** を選択します。

1.  「**ストレージ アカウント**」 ブレードで、「**Blob サービス**」 セクションにある 「**コンテナー**」 を選択します。

1.  「**コンテナー**」 セクションで、「**+ コンテナー**」 を選択します。

1.  「**新規コンテナー**」 ポップアップ ウィンドウで、次の操作を実行します。
    
    1.  「**名前**」 テキストボックスに、「**content**」を入力します。
    
    1.  「**パブリック アクセス レベル**」 ドロップダウン リストで、「**プライベート (匿名アクセスなし)**」 を選択します。
    
    1.  「**OK**」 を選択します。

1.  「**コンテナ―**」 セクションに戻り、新しく作成した **コンテンツ** コンテナーを選択します。

1.	「**コンテナー**」 ブレードで、「**アップロード**」 を選択します。

1.	「**BLOB をアップロード**」 ウィンドウで、次の操作を実行します。

    1.  「**ファイル**」 セクションで、「**フォルダ**」 アイコンを選択します。

    1.  「**ファイルエクスプローラー**」 ウィンドウで、「**すべてのファイル :\\Allfiles\\Labs\\02\\Starter**」 を参照し、**settings.json** ファイルを選択して 「**開く**」 をクリックします。

    1.  **「ファイルが既に存在する場合は上書きする」**チェックボックスがオンにチェックされていることを確認し、** 「アップロード」** を選択します。 
    
    > **注**: この演習を続行する前に、BLOB がアップロードされるのを待ちます。

#### タスク 3: HTTP トリガー関数を構成する

1.  Azure portal のナビゲーション ペインで、「**リソース グループ**」 リンクを選択します。

1.  「**リソース グループ**」 ブレードで、 この課題で前に作成した "**サーバーレス**" リソースグループを検索して選択します。

1.  「**Serverless**」 ブレードで、この課題で前に作成した関数アプリ **funclogic*[yourname]*** を選択します。

1.  「**関数アプリ**」 ブレードで、次の操作を実行します。

    1.  この課題で前に作成した **funclogic*[yourname]*** 関数アプリのノードを展開します。

    1.  「**関数**」ノードを展開します。

    1.  その特定の関数の **GetSettingInfo** ノードを展開します。

    1.  「**統合**」 ノードを選択します。

1.  「**統合**」セクションで次の操作を実行して、**Azure BLOB ストレージ**タイプの新しい入力を作成します。

    1.  「**新しい入力**」 を選択します。

    1.  「**Azure BLOB ストレージ**」 を選択します。

    1.  「**選択**」 を選択します。

1.  「**Azure BLOB ストレージ入力**」 セクションで、次の操作を実行します。

    1.  「**BLOB パラメーター名**」 ボックスに、値「**json**」を入力します。

    1.  「**パス**」 テキストボックスに、値「**content/settings.json**」を入力します。

    1.  「**ストレージ アカウント接続**」 リストで、「**AzureWebJobsStorage**」 を選択します。

    1.  「**保存**」 を選択します。

1.  「**統合**」 セクションに戻り、既存の **HTTP トリガー**を選択します。

1.  「**HTTP トリガー**」 セクションで、次の操作を実行します。

    1.  「**許可された HTTP メソッド**」 リストで、**選択したメソッド**を選択します。

    1.  「**Request パラメーター名**」 ボックスに、値「**request**」を入力します。

    1.  「**選択した HTTP メソッド**」 チェック ボックス グループで 、「**GET**」 オプションだけが選択されていることを確認します。

    1.  「**保存**」 を選択します。

#### タスク 4: 書き込みコードの記述

1.  「**関数アプリ**」 ブレードで、次の操作を実行します。

    1.  この課題で前に作成した **funclogic*[yourname]*** 関数アプリのノードを展開します。

    1.  「**関数**」ノードを展開します。

    1.  その特定の関数の **GetSettingInfo** ノードを選択します。

1.  関数エディターで、関数スクリプト **run.csx** のサンプルを確認します。

    ```
    #r "Newtonsoft.Json"

    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        name = name ?? data?.name;

        return name != null
            ? (ActionResult)new OkObjectResult($"Hello, {name}")
            : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
    }
    ```

1.  サンプル コードをすべて **削除** します。

1.  次のコード行を追加して、名前空間 **Microsoft.AspNetCore.Mvc** をインポートします。

    ```
    using Microsoft.AspNetCore.Mvc;
    ```

1.  次のコード行を追加して、名前空間 **System.Net** をインポートします。

    ```
    using System.Net;
    ```

1.  次のコードブロックを追加して、**Run** という名前の新しい **静的なパブリック** メソッドを作成します。このメソッドは 、**IActionResult** 型の変数を返し、**HttpRequest** 型や **string** 型の変数を *request* や *json* という名前のパラメーターとして受け取ります。

    ```
    public static IActionResult Run(HttpRequest request, string json)
    {
    }
    ```

1.  「**Run**」 メソッドで、「*json*」 パラメーターの内容を関数の HTTP レスポンスとして返すために次のコード行を入力します。

    ```
    return new OkObjectResult(json);
    ```

1.  「**run.csx**」 ファイルを確認します。 以下が含まれているはずです。

    ```
    using Microsoft.AspNetCore.Mvc;
    using System.Net;

    public static IActionResult Run(HttpRequest request, string json)
    {
        return new OkObjectResult(json);
    }
    ```

1.  **保存** を選択してスクリプトを保存します。

#### タスク 5: 関数実行のテスト

1.  タスク バーで、**Windows Terminal** アイコンを選択します。

1.  開いたコマンドプロンプトで次のコマンドを入力し、Enter キーを選択して、ベース URI をこの課題で前にコピーした URL の値に設定する **httprep** ツールを起動します。

    ```
    httprepl <function-app-url>
    ```

    > **注**: 例えば、URL が **https://funclogicstudent.azurewebsites.net** の場合、コマンドは **httprepl https://funclogicstudent.azurewebsites.net** になります。   

1.  ツールプロンプトで次のコマンドを入力してから、Enter を選択して対応する **api** エンドポイントを参照します。

    ```
    cd api
    ```

1.  次のコマンドを入力して、対応する **getsettinginfo** エンドポイントを参照します。

    ```
    cd getsettinginfo
    ```

1.  次のコマンドを入力し、現在のエンドポイントで **get** コマンドを実行します。

    ```
    get
    ```

1.  関数アプリからのレスポンスに含まれる JSON コンテンツを確認します。関数アプリには次の内容が含まれています。

    ```
    {
        "version": "0.2.4",
        "root": "/usr/libexec/mews_principal/",
        "device": {
            "id": "21e46d2b2b926cba031a23c6919"
        },
        "notifications": {
            "email": "Anais85@outlook.com",
            "phone": "751.757.2014 x4151"
        }
    }
    ```

1.  次のコマンドを入力してから、Enter を選択して **httprepl** アプリケーションを終了します。

    ```
    exit
    ```

1.  現在実行中の Windows Terminal アプリケーションを閉じます。

1.	Azure portal が表示されているブラウザー ウインドウに戻ります。

#### レビュー

このエクササイズでは、ストレージ内の JSON ファイルの内容を返す関数を作成しました。

### エクササイズ 5: サブスクリプションのクリーンアップ 

#### タスク 1: Azure Cloud Shellを開いて、リソース グループを一覧表示します

1.  Azure portal のナビゲーション ウィンドウで、**Cloud Shell** アイコンを選択して新しいシェル インスタンスを開きます。

    > **注**: **Cloud Shell** アイコンは、署名 (\>) アンダースコア文字 (\_) より大きく表されます。

1.  サブスクリプションを使用して Cloud Shell を初めて開く場合は、**Azure Cloud Shell へようこそウィザード** を使って、初めて使用する Cloud Shell を構成できます。ウィザードで次の操作を実行します。
    
    1.  シェルを使用して開始する新しいストレージ アカウントを作成するよう求めるダイアログ ボックスが表示されます。既定の設定を受け入れ、「**ストレージの作成**」 を選択します。 

    > **注**: Cloud Shell が初回のセットアップ手順を完了するのを待ってから、ラボを進めます。Cloud Shellの構成オプションが通知されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。新しいサブスクリプションを使用しているという前提で課題を記述します。

1.  ポータルにある **Cloud Shell** コマンド プロンプトで次のコマンドを入力し、Enter キーを押してサブスクリプション内のすべてのリソース グループを一覧表示します。

    ```
    az group list
    ```

1.  コマンド プロンプトで次のコマンドを入力し、Enter を選択して、リソース グループを削除できるコマンドの一覧を取得します。

    ```
    az group delete --help
    ```

#### タスク 2: リソースグループの削除

1.  コマンド プロンプトで次のコマンドを入力し、リソース グループ "**サーバーレス**" を削除します。

    ```
    az group delete --name Serverless --no-wait --yes
    ```
    
1.  ポータルの Cloud Shell ペインを閉じます。

#### タスク 3: アクティブなアプリケーションを閉じる

1.     現在実行中の Microsoft Edge アプリケーションを閉じます。

#### レビュー

この実習では、この演習で使用したリソース グループを削除してサブスクリプションをクリーンアップしました。