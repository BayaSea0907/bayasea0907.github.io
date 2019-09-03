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
* cinsoleで`node`を入力。
 ```こんな画面が出ればOK
  >.help
  .break Sometimes you get stuck, this gets you out
  .clear Alias for .break
  .exit  Exit the repl
  .help  Show repl options
  .load  Load JS from a file into the REPL session
  .save  Save all evaluated commands in this REPL session to a file
  >.exit
  ```
* fierbase-tools(FirebaseCLI)をインストール
  - >sudo npm install -g firebase-tools
* インストール出来たら、ログイン
  - >firebase login --no-localhost
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
  * hostingは、hostingディレクトリ作って、保存
    - ルーティングの役割を果たす...?
    - 静的ページの配置もここにする
  * funtcionsは、functionsディレクトリ作って保存
    - サーバー側の処理を記述する
    - 言語はjavascript。依存パッケージも一緒にインストールする
    - `firebase serve --only functions,hosting`でデバッグできると思われる。多分オプションなしでも大丈夫

