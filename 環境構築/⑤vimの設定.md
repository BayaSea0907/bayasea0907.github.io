# vimの設定

***

### .vimrcの書き方
※ vim_scriptは、commandの羅列

* let
  - 代入。`let a = 1`で、aに1を代入

* `"`
  - コメント。`"これでコメントになります`

* `source ~/script.vim`
  - vimの実行。`vim -S ~/script.vim`で、shellからも実行できる

* function, function!
  - 
  ```java
    function Sum(v1, v2)
      return a:v1, a:v2  " 「a:」にはスコープを指定する役割阿があるらしい...
    endfunction
   ```
* set, autcmd, syntax, ni, call, plug, augroup, colorscheem, filetype
  - -----------------------------ここから------------------------------

* `map`
  -  キーマッピング。`map <C-e>:NERDTreeToggle<CR>` で、`Ctrl + e`を押した時に、<br>
`:NERDTreeTogle`のvimコマンドを実行するって意味になる
  - '<CR>'は、キャリッジリターンの略。<Return>, <Enter>も同じ効果

***

### command
* <S-...>
  - Shift
  - `<S-Up>`で、`Shift + ↑`
  - `<S-F1>`で、`Shift + F1`
* <C-...>
  - Control
* <M-...>, <A-...>
  - alt, meta(mac)
* <D-...>
  - Macのcommand

***

### 実際に導入していく
* `sudo yum install -y vim vi`を実行
  - `vim`コマンドが聞くようになる。`.vimrc`, `.viminfo`が作成される
* NERDTreeを導入 (ディレクトリツリーから選択できる機能)
  - 下記を参考にして作業を行う
    https://qiita.com/zwirky/items/0209579a635b4f9c95ee
