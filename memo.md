# 64. Dockerfileを書く
- docker build --platform linux/amd64 .

# 65. Anacondaのインストール
-　まずはコンテナ内でアナコンダをインストールする。
- docker run -it 50ce00f97e1e bash
- lsでアナコンダのインストーラがあることを確認する。
- sh Anaconda3-2019.10-Linux-x86_64.sh を実行する
- Do you accept the license terms? [yes|no] に対してはyes
- Anaconda3 will now be installed into this location: /root/anaconda3 に対しては/opt/anaconda3
- インストールが終わるまで待つ
- by running conda init? [yes|no] に対してはyes
- Thank you for installing Anaconda3!と表示される
- アナコンダ3にパスを通す
  - パスを通す=環境変数$PATHに、パスを追加することでコンピュータがプログラムをそのパスから探してきてくれるようになる。
  - export PATH=/path/to/somethting:$PATH
- cd anaconda3/bin
- ./pythonと入力するとpythonが起動するはず。/opt/anaconda3/bin
- exit()でpythonを抜ける
- export PATH=/opt/anaconda3/bin:$PATH

今やった操作をDockerにやらせるにはどうしたら良いか？
.sh（シェル）にはインタラクティブややり取りをスキップするオプションがあるsh -x

- sh -x Anaconda3-2019.10-Linux-x86_64.sh を実行する
  - -b: バッチモードにするとインタラクティブな操作をカットできる
  - -p: インストール先を指定
  - ctrl+cでコマンドを抜ける
- オプションを使ってアナコンダをインストールするため、rm -r anaconda3/でフォルダを削除
- sh /opt/Anaconda3-2019.10-Linux-x86_64.sh -b -p /opt/anaconda3
- 今度はインタラクティブや操作がなく、アナコンダがインストールできる

# 66.Dockerfileの続きを書く
- `ENV`で環境変数を設定する
- `CMD ["jupyter", "lab", "--ip=0.0.0.0", "--allow-root", "--LabApp"]`について
  - --ip=0.0.0.0: ローカルホストでjupyter labを動かす
  - --allow-root: jupyter labはデフォルトではroot権限では動かせないので、rootを許可する
  - --LabApp.token='': tokenは指定しない。
- M1 Macの場合、`docker build --platform linux/amd64 .`にする！     
- `docker run -p 8888:8888 <image>`
- ブラウザを開いて、localhost:8888にアクセスする。うまくいっていれば、jupyter labが起動する。