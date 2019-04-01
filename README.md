# Global AI Nights - Beginner Track コンテンツ

## 参加者用ワークショップ

## セッション情報

**タイトル:** 見る、聞く、話す、または理解することができるアプリケーションを、Microsoft Cognitive Services で作成する

**概要:** このワークショップでは、まず、[Microsoft Azure Cognitive Services](https://azure.microsoft.com/ja-jp/services/cognitive-services/) を紹介します。Cognitive Servicesは、スクラッチでコーディングすることなく、インテリジェンスと機械学習をアプリケーションに組み込むために使用できるサービス群です。例えば、[Computer Vision](https://azure.microsoft.com/ja-jp/services/cognitive-services/directory/vision/) や [Text Analytics](https://azure.microsoft.com/ja-jp/services/cognitive-services/directory/lang/) は、事前に学習済みの AI の API で、REST プロトコルによってアクセスできます。

次に、Cognitive Services をオフラインおよびエッジデバイスで実行できるようにするために、Cognitive Services を[コンテナーでホストする](hhttps://docs.microsoft.com/ja-jp/azure/cognitive-services/cognitive-services-container-support)方法を説明します。

次に、転移学習を使用しているカスタム AI - [Custom Vision](https://azure.microsoft.com/ja-jp/services/cognitive-services/custom-vision-service/) を見ていきます。Custom Vision は、画像分類モデルをトレーニングするために独自のデータを少量用意するだけでよくなります。

ワークショップの締めくくりとして、独自に学習した AI を組み込んだアプリケーションを構築します。ここで利用する [Logic Apps](https://azure.microsoft.com/ja-jp/services/logic-apps/) は機械学習についての概念実証に理想的です。

## マシンの前提条件

* ワークショップに必要な画像やコードサンプルを入手するために、'Clone or Download' (緑のボタン) > 'Download ZIP' を選択してダウンロードします。または、このリポジトリをローカルマシンに複製してください: `git clone https://github.com/seosoft/AINightsBegineerTrack-JP.git`  
* [Microsoft Azure サブスクリプション](https://azure.microsoft.com/ja-jp/free/)
* モダンブラウザー(Google Chrome, Microsoft Edge)
* [Postman, API Development Environment - available on Windows, Linux and macOS](https://www.getpostman.com/downloads/)

> すべてのデモとコンテンツは Windows PC 上でテストしていますが、すべてのオプションは macOS と Linux マシンでも実行できるはずです。他のオペレーティングシステムについてフィードバックがある場合は、issue または pull request で情報を提供してください。

**訳注**
Azure サブスクリプションで、Event Grid リソースプロバイダーが登録されていない場合、以下のタスク 4で実行に失敗します。登録状態が不明な場合は、ここで確認してください。  
[サブスクリプション] - [リソースプロバイダー] の **"Microsoft.EventGrid"** が "Registered" になっていれば登録済みです。
![Azure Resource Provider](docs-images/eventgrid_resorceprovider.jpg)

## タスク 1: Microsoft Azure Cognitive Services - Computer Vision

このタスクでは、Webサイトのデモオプションを使用して Cognitive Services を試します。

サイトを開きます: [https://azure.microsoft.com/ja-jp/services/cognitive-services/directory/vision/](https://azure.microsoft.com/ja-jp/services/cognitive-services/directory/vision/)

![Computer Vision website Link highlighted](docs-images/computer-vision-link.JPG)

各セクションで、多くの異なるデモを試せます（Vision、OCR、Face、Emotion Detection、Video Indexer でのシーンとアクティビティの認識など）。

**Computer Vision** の **画像内のシーンおよびアクティビティ認識** の横にある **デモ** リンクを選択します。他のサービスを探索するためのデモリンクもあります。

![Computer Vision Example](docs-images/computer-vision-demo.JPG)

次に、**参照** ボタンを選択して、`sample-images/computer-vision-web-browser/` から cat.jpeg または city.jpeg の画像をアップロードしてみましょう。画像の分析結果を見ることができます。

![Computer Vision Cat Example](docs-images/cat-sample.JPG)

## タスク 2: Microsoft Azure Cognitive Services - REST 経由で Text Analytics を利用

次は、Cognitive Services をアプリケーションに統合するために使われる REST プロトコルを利用します。

まず、[Microsoft Azure](https://azure.microsoft.com/ja-jp/) に接続して、右上にある **ポータル** を選択します。

ポータルで **リソースの作成** を選択します。**Cognitive Services** を検索して選択します。そのあと、ブレードで **作成** を選択します。

![Create Cognitive Services Account](docs-images/cognitive-azure.JPG)

以下を参考に詳細を入力してアカウントを作成します:

* **Name:** サービスに適した任意の名前を入力 (例: `ainightscognitive`)
* **サブスクリプション:** サブスクリプションを選択
* **場所:** 任意の場所を選択（日本リージョンの利用も可能です）
* **Pricing Tier:** S0
* **Resource Group:** '新規選択' を選択して、任意の名前を入力 (例: `ainights`)
* **利用規約ボックスに同意した上で、チェックをオン**
* **'作成' を選択**

![Cognitive Services Details](docs-images/cognitive-details.JPG)

作成されたら、通知領域（画面の右上）の **リソースに移動** を選択します。

![リソースに移動](docs-images/go-to-resource.JPG)

Cognitive Services ページで **Keys** を選択して、**KEY 1** をコピーし、メモ帳などに貼っておきます。

![Copy Key](docs-images/keys.JPG)

左上の *Overview* を選択して、**Endpoint** の値をコピーし、メモ帳などに貼っておきます。
![Copy Endpoint](docs-images/endpoint.JPG)

Postman をローカルマシンにダウンロードして開きます。

> ダウンロード方法は、前述の **マシンの前提条件** を参照

**Request** を選択します。

![Create A Request](docs-images/create-request.JPG)

以下のようにリクエストの詳細を入力し、**新しいコレクションを作成** するオプションを選択して "Text Analytics Samples" という名前を付けます。

![Enter Request Details](docs-images/save-request.JPG)

新しく作成したコレクションを選択して "保存" を選択します。

![Save Request](docs-images/save.JPG)

以下の手順で Text Analytics API を呼び出すリクエストを作成します:

* 左上の GET リクエストから **POST** リクエストに変更
* Cognitive Services の Endpoint URL を入力して、末尾に次の値を追加: `text/analytics/v2.0/sentiment`
* URL テキストボックスの下の **Headers** を選択
* **Key** に `Ocp-Apim-Subscription-Key` を入力、**Value** に KEY1 の値を入力
* **Key** に `Content-Type` を入力、**Value** に `application/json` と入力
* ![Headers and URL](docs-images/url-and-headers.JPG)
* URL テキストボックスの下の **Body** を選択
* ラジオボタンの `raw` を選択
* `sample-code/cognitive-services-api-task/sentiment-analysis-text.json` の JSON のサンプルをテキストボックスに貼り付けます
* **Send** ボタンを選択して、レスポンスを確認しましょう
* ![Body and Submit REST Request](docs-images/rest-body.JPG)

KeyPhrases 関数など、REST API から他のオプションを試すこともできます。 URLの末尾を "sentiment" から "keyPhrases" に変更し、"Send" を選択してサンプルテキストのキーフレーズを表示します。

* ![Key Phrases REST Request](docs-images/keyphrases.JPG)

> [こちら](https://docs.microsoft.com/ja-jp/azure/cognitive-services/text-analytics/language-support)で Text Analytics API の言語サポートをチェックしてください。あなたの言語がサポートされている場合は、テキストを翻訳して上記の API の機能を表示するために JSON ファイルを編集してください。フランス語のJSONファイルの例が ```sample-code/text-analytics-demo/sentiment-analysis-text-fr.json``` にあります。このファイルを適切に編集してください。

> Postman の実行に問題がある場合は、[センチメント分析](https://northeurope.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) および [キーフレーズ抽出](https://northeurope.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) の API ドキュメントで REST API を実行できます。サイトで内で使用するデータセンターを選択し、Postman で使用した JSON のサンプルと Key を入力して実行してみましょう。

## タスク 3: Microsoft Azure Cognitive Services - Custom Vision

Microsoft Azure Custom Vision サービスを使用すると、ごくわずかなコードで独自の画像分類およびオブジェクト検出のアルゴリズムを構築できます。このタスクでは、スタンフォード大学が作成した [ImageNet open dataset]([http://vision.stanford.edu/aditya86/ImageNetDogs/](http://vision.stanford.edu/aditya86/ImageNetDogs/)) の中から、犬の画像を使って犬種分類アルゴリズムを作成します。

前述「マシンの前提条件」でダウンロードしたコンテンツの中で `sample-images/dogs.zip`フォルダー内に、7つの犬種それぞれ30枚づつの画像があります。

* Beagle
* Bernese Mountain Dog
* Chihuahua
* Eskimo Dog (Husky)
* German Shepherd
* Golden Retriever
* Maltese

また、`sample-images/dogs.zip` には、トレーニング用の画像の他にテスト用の画像（dog.zip内の `testset`）もあります。

まず Azure アカウントで Custom Vision インスタンスを作成します。

* [Azure ポータル](https://ms.portal.azure.com) のメインダッシュボードを開く
* 左上の 'リソースの作成' をクリック
* 'Custom Vision' を検索
* Custom Vision の詳細ペインで **作成** をクリック
* 詳細の情報を入力
    * 任意のサービスの名前を入力
    * サブスクリプションを選択
    * **[重要]** 場所は **米国中南部** (SOUTH CENTRAL US) を選択
    * 'Prediction pricing tier' および 'Training pricing tier' で 'S0' を選択
    * TASK2で作成した Resource Group を選択
    * '作成' をクリック
* ![Custom Vision Blade Details](docs-images/custom-vision-azure.JPG)

分類器を作成が完了したら、[https://www.customvision.ai](https://www.customvision.ai) に移動して、Azure 認証情報アカウントでサインインしてください。

> 利用規約の表示が出た場合は、利用規約に同意の上、同意のチェックをオンにして進めてください。

画面が表示されたら、'NEW PROJECT' をクリックして詳細入力ウィンドウを開き、以下を参考に入力します。

* Name: 任意の名前を入力（例: dog classifier）
* Description: 分類器の説明を入力（例: AIアプリ用）
* Resource Group: Custom Vision を作成する Resource Group を選択します（例: ainights[SO]）
* Project Types: Classification
* Classification Types: Multiclass (Single tag per image)
* Domains: General
* ![Create Custom Vision Project](docs-images/create-project.JPG)

'Create Project' を選択すると、以下のように空のワークスペースが表示されます。
![Empty Custom Vision Project](docs-images/start-page.JPG)

ここで、画像の追加とタグの割り当てを開始して画像分類器を作成します。

画面左上の 'Add images' を選択して、[サンプル画像のsample-images/dogs.zipフォルダー内](sample-images/dogs.zip) の最初のフォルダー - Beagle を開き、フォルダー内の 30枚の画像を選択します

**My Tags** に 'beagle' と入力して、'Upload 30 files' で画像をアップロードします 

![Upload images of dogs](docs-images/add-class-images.JPG)

成功すると確認メッセージが表示され、画像がワークスペースで利用可能になります。

![Successful upload](docs-images/upload-successful.JPG)

同じ手順で、フォルダー内の他の 6つの犬種の画像をそれぞれタグ付けしてアップロードします:
* 'Add images' をクリック
* 新しい30枚の犬の画像を選択
* **My Tags** に犬種を入力 (beagle, german-shepherd, maltese などフォルダーの名前を入力)
* 'Upload' を選択
* ワークスペースへのアップロードを確認

すべてのカテゴリがアップロードされ、左側に犬種が表示され、犬の画像の種類に応じてフィルタリングできるようになります。

![All images uploaded and tagged](docs-images/all-categories-uploaded.JPG)

これで、アップロードした犬の画像データについてアルゴリズムを訓練する準備が整いました。右上隅にある緑色の 'Train' ボタンをクリックしてください。

トレーニングプロセスが完了したら、[Performance] タブに移動します。ここで、訓練したモデルの機械学習の評価指標を見ることができます。

![Evaluation Metrics](docs-images/train-metrics.JPG)

これでテストに必要なモデルが完成しました。右上にある（'Train' ボタンの横にあります） 'Quick Test' ボタンを選択してください。ウィンドウが開き、ローカル画像をブラウズしたり URLを入力することができます。

テストフォルダー内（dog.zip内の `testset`）の画像（トレーニングされていない画像）を参照してアップロードします。画像が分析され、モデルがそれが何であると考えているのか（Predictions 内の Tag列）およびモデルの結果に対する信頼性（Predictions 内の Probablity列）の結果が返されます。 

![Quick Test](docs-images/quick-test.JPG)

> モデルの性能を確認するには、テストフォルダー内の他の画像でこの手順を繰り返します。

上部のツールバーの [Predictions] タブをクリックすると、テストした画像が表示されます。このセクションは再トレーニングに使用できます。新しいデータを取得したら、これをモデルに追加してパフォーマンスを向上させることができます。画像は重要な順に並べられます - 正しく分類されていれば、モデルに追加された最も新しい情報が画像が先頭に表示されます。最後の画像はモデルによってすでに学習されている他の画像と非常に似ているかもしれませんが、これは正しい分類の重要性が低いものです。

![Re-training](docs-images/retraining.JPG)

これらの画像をモデルに追加するには、画像を選択してモデルの結果を確認してから、[My Tags] ボックスに正しいタグを入力して [Save and close] をクリックします。

![Add Re-training Tag](docs-images/add-tag.JPG)

この画像は、[Predictions] ワークスペースから消え、[Training Images] ワークスペースに追加されます。新しい画像やタグをいくつか追加したら、モデルを再訓練して改善するかどうかを確認できます。

自身のアプリケーションでこのモデルを使用するには、このAPIにアクセスするための情報が必要です。トップバーの [Performance] タブに移動します。  

[Performance] タブで **[Publish]** をクリックすると犬種分類アプリケーションをクラウドに発行されます。続いて 'Prediction URL' を選択すると、Postman からの API 呼び出しを作成するのに必要な情報が表示されます。

![Prediction URL Location](docs-images/prediction-url-location-v2.jpg)

> #### 訳注 
> - **If you have an image file:** は、ローカルの画像をアップロードするときに利用します。Postman から画像を送信する際、Bodyで **binary** を選択し、画像を直接添付します。
> - **If you have an image URL:** は、URLでアクセスできる画像を参照する際に利用します。画像をインターネットでアクセスできる場所におき、BodyにJSONフォーマットでURLを指定します。
  ![Prediction in Postman](docs-images/postman-custom-vision.JPG)

**おめでとうございます！** Azure Custom Vision サービスを使用して、自分だけの特別な犬の分類モデルを作成することができました。

## タスク 4: カスタム AI をアプリケーションに組み込み - Azure Logic Apps

このタスクでは、Custom Vision で犬種分類アプリケーションを使用するための Azure Logic App を構築します。

最初に、2つの Azure ストレージアカウントを作成します。

Azure ポータルに移動し、左上にある [リソースの作成] をクリックします。Storage セクションを選択し、最初のオプションの 'Storage Account' を選択します。

![Azure Storage Account](docs-images/storage-account.JPG)

2つのストレージアカウントを作成します:
* 一つは画像ファイルを置くのに使います (名前は ainightstor とします)
* もう一つは結果を置くのに使います (名前は resultainights とします)

> 以下の手順を 2回実行して、合計 **2個** のストレージアカウントを作成します

ストレージアカウント作成ページで、設定するためのオプションを入力します:

* **サブスクリプション:** サブスクリプションを選択します
* **リソースグループ:** このワークショップで使っているワークグループを選択します（例: ainights）
* **ストレージアカウント名:** (ユニークな名前である必要があります) 全て小文字でストレージアカウント名を入力します。 *ainightsstor(あなたの名前) や resultsainights(あなたの名前) - ユニークな名前にするために、"かっこをつけずに" 末尾に自分の名前を付けるなどをしてください*
* **場所:** もっとも近いデータセンターを選択します
* **パフォーマンス:** Standard
* **アカウントの種類:** Blob Storage
* **レプリケーション:** ローカル冗長ストレージ (LRS)
* **アクセス層:** ホット

**機能および作成** を選択し、検証されたことを確認してから **作成** を選択します。

![Create Azure Storage Account](docs-images/create-storage-account.JPG)

展開が完了したら、リソースにアクセスしてアカウント設定を確認します。空のBLOBストレージアカウントを確認するには、[**BLOB**] を選択します。

![Select Blob Storage Account](docs-images/select-blobs.JPG)

画像と結果を保存するためにストレージアカウントにコンテナーを追加する必要があります。

**コンテナー** ボタンを選択して、コンテナーの名前を作成します

> **ainightsstor** アカウントには **'images'** コンテナーを作成します

> **resultsainights** アカウントには **'results'** コンテナーを作成します

パブリックアクセスレベルでは、**'コンテナー（コンテナーと BLOB の匿名読み取りアクセス）'** を選択します

![Create a container](docs-images/create-stor-container.JPG)

> 上の設定で、イメージ保存アカウントと結果保存アカウントについて上記の手順を実行します。

いよいよロジックアプリを作成します - 画像保存アカウントをあなたの AI 分類サービスに接続し、結果保存アカウントに結果を入れます。

Azure ポータルのホームページで、[**リソースの作成**] を選択します。Logic App と入力してサービスを選択します。

![Logic App](docs-images/logic-app.JPG)

以下のように設定の詳細を入力してロジックアプリを作成します:

* **名前:** 犬の分類アプリに適した任意の名前を入力
* **サブスクリプション:** 任意のサブスクリプションを選択
* **リソースグループ:** このワークショップで使っているリソースグループを選択
* **場所:** 任意の場所を選択
* **Log Analytics:** Off

**作成** をクリックします。

![Logic App Option](docs-images/create-logic-app.JPG)

作成したら、リソースに移動します。ここから、ロジックプロセスを作成するします。左側のメニューから [**ロジックアプリデザイナー**] を選択し、次に [**Event Grid のリソースイベントが発生するとき**] を選択します。

![Logic App Trigger](docs-images/logic-app-trigger.JPG)

Azure 認証情報を使用してサインインし、Azure Event Grid に接続します。

![Event Grid Sign In](docs-images/event-grid-sign-in.JPG)

接続して緑色のチェックマークが表示されたら、[続行]を選択します。

以下のオプションを選択してください:
* **Subscription:** サブスクリプション
* **Resource Type:** Microsoft.Storage.StorageAccounts
* **Resource Name:** 画像を置く用のストレージアカウントを選択 (例: ainightsstor)
* **Event Type 項目 - 1:** Microsoft.Storage.BlobCreated

![Event Grid Options](docs-images/event-grid-options.JPG)

[**新しいステップ**] を選択します。 **json** と入力して、候補の中から **JSON の解析** を選択します

* **コンテンツ:** テキストボックスを選択すると右側に表示される [動的なコンテンツの追加] で、 **本文** を選択します
* **スキーマ:** テキストボックスを選択し、[logic-app-schema file](sample-code/logic-app-task/logic-app-schema.txt) の JSON スキーマを貼りつけます

![Parse JSON](docs-images/parse-json.JPG)

[**新しいステップ**] を選択します。'http' と入力して、**HTTP** を選択します。

![Select HTTP connector](docs-images/select-http-connector.jpg)

Postman で確認したときと同様に **方法**(POST)、**URI**、**ヘッダー**を入力します。**本文** のところに 前のステップで作成した **JSON の解析** 結果から **url** を選択して埋め込みます。([本文] ボックスにカーソルを合わせると右側に表示される [動的なコンテンツの追加] で 'URL' を検索して選択します。)

![HTTP Options](docs-images/http-connector-options.jpg)

[**新しいステップ**]をクリックします。再び **JSON の解析** を選択します。

- コンテンツ: テキストボックスを選択すると右側に表示される [動的なコンテンツの追加] で、 **HTTP** の結果から **本文** を選択します
- スキーマ: テキストボックスを選択し、[customvision-schema file]() の JSON スキーマを貼りつけます

![Parse JSON 2](docs-images/parse-json2.jpg)

**for each** と入力して、'For each' という灰色のコントロールを選択します。選択したら、 '以前の手順から出力を選択' を選択し、'動的なコンテンツ' から **Predictions** を選択します。

![For each prediction](docs-images/for-each-prediction-add-action.JPG)

**アクションの追加** を選択します。

'制御' と入力して検索して、**制御** アイコンを選択して、その中の **条件** を選択します。

![If Statement](docs-images/if-statement.JPG)

条件ボックスで、'値の選択' をクリックし、[動的なコンテンツの追加] で **JSON の解析2** から **Probability** を選択します。

条件として '次の値以上' を選択し、'値の選択' に **0.7** と入力します。

![For Each Options](docs-images/for-each-options.JPG)

**true の場合** 内の **アクションの追加** を選択します。

'Azure Blob Storage' で検索して、**Azure Blob Storage** アイコンを選択し、**BLOB の作成** を選択します。

接続名に **results** と入力し、'ストレージアカウント' には、先ほど結果を置く用に作成したストレージアカウントを選択します。

![Connect to Result Blob Storage](docs-images/result-blob-connection.JPG)

'フォルダーのパス' で右端にあるフォルダーアイコンを選択し、'results' を選択します。

'BLOB 名' フィールドに '**result-**' と入力し、[動的なコンテンツの追加] で **JSON の解析2** から **Id** を選択します。

'BLOB コンテンツ' を選択し、[動的なコンテンツの追加] から で **JSON の解析2** の 'もっと見る' を選択します。その中で **tagName** を選択しします。その後ろに '**:**' と入力します。続いて[動的なコンテンツの追加]で **probability** を選択します。

![For Each Options](docs-images/for-each-options.JPG)

最後に、Logic Apps のアクションバーで **保存** を選択します。

保存に成功したら、出力をテストしてみましょう。アクションバーで **実行** を選択します。

> **訳注** 実行すると '実行を表示するには、開始操作を実行してください' と表示されます。そのまま次の手順に進んでください。

![Run Logic App to test](docs-images/run-logic-app.JPG)

ここで、作成した画像ストレージアカウント (ainightsstor) に移動します（リソースグループから探すと簡単です）。**BLOB** を選択して、Blobを選択して 'images' コンテナーを選択します。'アップロード' ボタンがあるので、Dogs データテストセットフォルダーから任意の画像をアップロードします。

![Upload Blob](docs-images/upload-blob.JPG)

アップロードできたら、Logic Apps のメインページに戻り、ページ下部の '実行の履歴' を見ます。'成功' した実行の入出力に移動します。

![Run History](docs-images/logic-app-run-history.JPG)

すべてのセクションに緑色のチェックマークが付いているはずです。各セクションを選択して、レイヤー間の入力と出力を表示できます（これは、正しく実行されなかった場合にデバッグするのにも最適な方法です）。

> **訳注**
2019年3月末時点では、上記の Logic App を実行すると**Predict tags from image URL** でエラーが発生しています。  
これは Custom Vision の Prediction URL と Logic App の Predict tags コントロールとのバージョンに差異があるためです。  
今後、Logic App 側が Custom Vision の新しいバージョン (v3.0) に対応することで上記の操作で犬種予測に成功するようになるはずです。  
4月2日の時点では、上記の手順でアプリケーションを作成するという理解までとしてください。


![Logic app run successful](docs-images/explore-logic-app-run.JPG)

最後に、'resultsainight' BLOBストレージアカウントに移動して、BLOB を選択し、'results' コンテナーを開き、そこに作成されたファイルを確認します。ファイルの内容は、犬の画像から得られる、犬の予測クラス、信頼度スコア
です。

![Result](docs-images/result.JPG)
