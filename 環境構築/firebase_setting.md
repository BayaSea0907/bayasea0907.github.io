# firebaseを使う

※ 参考: 
* npmについて
https://www.webprofessional.jp/beginners-guide-node-package-manager/
* 仮想環境からfirebase-toolsをインストールするとき
https://qiita.com/Orangelinux/items/2a8cb318d06e3b78c8cb
* ホスティングを行うまで
https://qiita.com/Ijoru/items/5b27f1c32df2222514fb
* ディレクトリ構成
https://qiita.com/tag1216/items/6901a200997792dd378c
* 入力フォーム & メール送信機能を実装する
https://qiita.com/Hiroyuki1993/items/1ab9266ca6fc422113e3


***

### FireBaseにログインする
* fierbase-tools(FirebaseCLI)をインストール
  - >sudo npm install -g firebase-tools
* インストール出来たら、ログイン
  - >firebase login --no-localhost # --no-localhostをつけると、ターミナルで認証可能
  
### FireBaseのプロジェクトを初期化
* 好きなディレクトリに入って、初期化
  - >firebase init
  ```######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########
    You're about to initialize a Firebase project in this directory:

  ? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices. (Press <space> to select, <a> to toggle all, <i> to invert selection)
  ❯◯ Database: Deploy Firebase Realtime Database Rules
   □ Firestore: Deploy rules and create indexes for Firestore
   □ Functions: Configure and deploy Cloud Functions
   □ Hosting: Configure and deploy Firebase Hosting sites
   □ Storage: Deploy Cloud Storage security rules
   ```
  * hostingは、pagesディレクトリ作って、保存
    - 後々、vueでbuildしたときに、ここにファイルを生成するようにする    
  * funtcionsは、functionsディレクトリ作って保存
    - サーバー側の処理を記述する
    - 言語はjavascript。依存パッケージも一緒にインストールする
    - `firebase serve --only functions,hosting`でデバッグできると思われる。多分オプションなしでも大丈夫
  * firestoreは、ルートディレクトリにファイルを作成する
    - firebase console(web)でDatabaseの項目を選択して、データベースを作成する。ロケーションは「	asia-northeast1(Tokyo)」
    
### FireBaseのデプロイ
  * `nuxt build` # 本番用の webpack を使用してアプリケーションをビルドおよび最適化する
  * `nuxt genarate` # dist/ ディレクトリに静的ページをエクスポートする
  * `firebase deploy` # ファイアベースにデプロイされる
