﻿---
lab:
    title: 'Cognitive Services の概要'
    module: 'モジュール 2 - Cognitive Services を使用した AI アプリの開発'
---

# Cognitive Services の概要

この演習では、Azure サブスクリプションで **Cognitive Services** リソースを作成し、それをクライアント アプリケーションから使用することで、Cognitive Services の使用を開始します。この演習の目標は、特定のサービスに関する専門知識を習得することではなく、開発者として Cognitive Services をプロビジョニングおよび操作するための一般的なパターンに精通することです。

## このコースのリポジトリを複製する

**AI-102-AIEngineer** コードのリポジトリをこのラボで作業している環境にまだ複製していない場合は、次の手順に従って複製してください。それ以外の場合は、複製されたフォルダーを Visual Studio Code で開きます。

1. Visual Studio Code を起動します。
2. パレットを開き (SHIFT+CTRL+P)、**Git: Clone** コマンドを実行して、`https://github.com/MicrosoftLearning/AI-102JA-Designing-and-Implementing-a-Microsoft-Azure-AI-Solution` リポジトリをローカル フォルダーに複製します (どのフォルダーでもかまいません)。
3. リポジトリを複製したら、Visual Studio Code でフォルダーを開きます。
4. リポジトリ内の C# コード プロジェクトをサポートするために追加のファイルがインストールされるまで待ちます。

    > **注**: ビルドとデバッグに必要なアセットを追加するように求められた場合は、**「今はしない」** を選択します。

## Cognitive Services リソースをプロビジョニングする

Azure Cognitive Services は、アプリケーションに組み込むことができる人工知能機能をカプセル化するクラウドベースのサービスです。特定の API (**Language** や **Computer Vision** など) に個別の Cognitive Services リソースをプロビジョニングすることも、単一のエンドポイントとキーを介して複数の Cognitive Services API へのアクセスを提供する一般的な **Cognitive Services** リソースをプロビジョニングすることもできます。この場合、単一の **Cognitive Services** リソースを使用します。

1. `https://portal.azure.com` で Azure portal を開き、Azure サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。
2. **&#65291;「リソースの作成」** ボタンを選択し、*Cognitive Services* を検索して、次の設定で **Cognitive Services** リソースを作成します。
    - **サブスクリプション**: *お使いの Azure サブスクリプション*
    - **リソース グループ**: *リソース グループを選択または作成します (制限付きサブスクリプションを使用している場合は、新しいリソース グループを作成する権限がない可能性があります - 提供されているものを使用してください)*
    - **リージョン**: *利用可能な任意のリージョンを選択します*
    - **名前**: *一意の名前を入力します*
    - **価格レベル**: Standard S0
3. 必要なチェックボックスを選択して、リソースを作成します。
4. デプロイが完了するのを待ってから、デプロイの詳細を表示します。
5. リソースに移動し、その **「キーとエンドポイントページ」** ページを表示します。このページには、リソースに接続して、開発したアプリケーションからリソースを使用するために必要な情報が含まれています。具体的には:
    - クライアント アプリケーションがリクエストを送信できる HTTP *エンドポイント*。
    - 認証に使用できる 2 つの*キー* (クライアント アプリケーションはどちらのキーも認証に使用できます)。
    - リソースがホストされている*場所*。これは、一部の (すべてではない) API へのリクエストに必要です。

## REST インターフェイスの使用

Cognitive Services API は REST ベースであるため、HTTP 経由で JSON リクエストを送信することで API を利用できます。この例では、**Language** REST API を使用して言語検出を実行するコンソール アプリケーションについて説明します。ただし、基本的な原則は、Cognitive Services リソースでサポートされているすべての API で同じです。

> **注**: この演習では、**C#** または **Python** のいずれかから REST API を使用することを選択できます。以下の手順で、希望する言語に適したアクションを実行します。

1. Visual Studio Code の **「エクスプローラー」** ウィンドウで、**01-getting-started** フォルダーを参照し、言語の設定に応じて **C-Sharp** フォルダーまたは **Python** フォルダーを展開します。
2. **rest-client** フォルダーの内容を表示し、構成設定用のファイルが含まれていることに注意してください。
    - **C#**: appsettings.json
    - **Python**: .env

    構成ファイルを開き、含まれている構成値を更新して、Cognitive Services リソースの**エンドポイント**と認証**キー**を反映します。変更を保存します。
4. **rest-client** フォルダーには、クライアント アプリケーションのコード ファイルが含まれていることに注意してください。

    - **C#**: Program.cs
    - **Python**: rest-client.py

    コード ファイルを開き、含まれているコードを確認して、次の詳細に注意してください。
    - HTTP 通信を可能にするために、さまざまな名前空間がインポートされます
    - **Main** 関数のコードは、Cognitive Services リソースのエンドポイントとキーを取得します。これらは、REST リクエストを Text Analytics サービスに送信するために使用されます。
    - プログラムはユーザー入力を受け入れ、**GetLanguage** 関数を使用して、Cognitive Services エンドポイントの Text Analytics 言語検出 REST API を呼び出して、入力されたテキストの言語を検出します。
    - API に送信されるリクエストは、入力データを含む JSON オブジェクトで構成されます。この場合、**ドキュメント** オブジェクトのコレクションであり、それぞれに **ID** と**テキスト**があります。
    - サービスのキーは、クライアント アプリケーションを認証するためのリクエスト ヘッダーに含まれています。
    - サービスからの応答は JSON オブジェクトであり、クライアント アプリケーションはこれを解析できます。
5. **rest-client** フォルダーを右クリックして、統合ターミナルを開きます。次に、次の言語固有のコマンドを入力して、プログラムを実行します。

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python rest-client.py
    ```

6. プロンプトが表示されたら、テキストを入力し、サービスによって検出された言語を確認します。これは、JSON 応答で返されます。たとえば、「Hello」、「Bonjour」、「Hola」と入力してみてください。
7. アプリケーションのテストが終了したら、「quit」と入力してプログラムを停止します。

## SDK の使用

Cognitive Services のREST API を直接使用するコードを記述できますが、Microsoft C#、Python、Node.js など、多くの一般的なプログラミング言語用のソフトウェア開発キット (SDK) があります。SDK を使用すると、Cognitive Services を使用するアプリケーションの開発を大幅に簡素化できます。

1. Visual Studio Code の **「エクスプローラー」** ウィンドウの **01-getting-started** フォルダーで、言語の設定に応じて **C-Sharp** フォルダーまたは **Python** フォルダーを展開します。
2. **sdk-client** フォルダーを右クリックして、統合ターミナルを開きます。次に、言語設定に適合するコマンドを実行して、Text Analytics SDK パッケージをインストールします。

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.0.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.0.0
    ```

3. **sdk-client** フォルダーの内容を表示し、構成設定用のファイルが含まれていることに注意してください。
    - **C#**: appsettings.json
    - **Python**: .env

    構成ファイルを開き、含まれている構成値を更新して、Cognitive Services リソースの**エンドポイント**と認証**キー**を反映します。変更を保存します。
    
4. **sdk-client** フォルダーには、クライアント アプリケーションのコード ファイルが含まれていることに注意してください。

    - **C#**: Program.cs
    - **Python**: sdk-client.py

    コード ファイルを開き、含まれているコードを確認して、次の詳細に注意してください。
    - インストールした SDK の名前空間がインポートされます
    - **Main** 関数のコードは、Cognitive Services リソースのエンドポイントとキーを取得します。これらは SDK で使用され、Text Analytics サービスのクライアントを作成します。
    - **GetLanguage** 関数は、SDK を使用してサービスのクライアントを作成し、クライアントを使用して入力されたテキストの言語を検出します。
5. **sdk-client** フォルダーの統合ターミナルに戻り、次のコマンドを入力してプログラムを実行します。

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python sdk-client.py
    ```

6. プロンプトが表示されたら、テキストを入力し、サービスによって検出された言語を確認します。たとえば、「Goodbye」、「Au revoir」、「Hasta la vista」と入力してみてください。
7. アプリケーションのテストが終了したら、「quit」と入力してプログラムを停止します。

> **注**: Unicode 文字セットを必要とする一部の言語は、この単純なコンソール アプリケーションでは認識されない場合があります。

## 詳細

Azure Cognitive Services の詳細については、[Cognitive Services のドキュメント](https://docs.microsoft.com/azure/cognitive-services/what-are-cognitive-services)を参照してください。
