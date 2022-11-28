米国AI開発者がゼロから教えるDocker講座(https://www.udemy.com/course/aidocker/)の学習記録
$docker login
$docker pull <image>
$docker run -it <image> bash imageからコンテナを作成してそのコンテナに入る
$exit
$docker ps -a 
$docker images
$docker restart コンテナをupにする
$docker exec -it <container> bash コンテナを起動
$docker commit <container> <image> containerを指定してimageを作成
$docker tag <source> <target> sourceのimagesをtargetに新しい名前を入れる
$docker push <image> dockerHubにプッシュする(image名とdockerHubのrepository名を一致させる)
$docker rmi <image> imageを削除

$docker run <image> run=crate+ start
$docker create <image>  
$docker start -a <container>設定されたコマンドを出力
$docker run -it ubuntu bash 
-itとは、、、
-i:インプット可能　
-t:表示がきれいになる
コンテナの削除
$docker stop <container>
$docker rm <contianer>
コンテナの全削除
$docker system prune

--------------------------------------------------------------------------------------------------------
Dockerfileの作成
FROM 土台となるosを指定
RUN RUNのあとに書かれたコマンドを(Linuxで実行)、RUNごとにlayerが作成
CMD コンテナのデフォルトのコマンドを指定。
CMD ["excutable","param1","param2"]
*CMDとRUNの違い→layerを作るか作らないか
COPY Dokerfileないにあるファイルをimageにコピー
COPY <src><dest> 
ENTRYPOINT 動作はCMDと同じだが、runコマンド時に上書きが不可。
ENV 環境変数の設定
WORKDIR Docker instructionの実行ディレクトリを変更する
WORKDIR <絶対パス>
(ex) 
RUN mkdir input_dir
COPY test.txt/input_dir 


実行
$docker build <Dokerfileのあるディレクトリ>
$docker run -it <nameを付ける> bash

Dockerimageのlayer数を最小限にする。
→Layerを作成するのはRUn,COPY,ADDの３つ
→コマンドを&&でつなげる
→バックスラッシュ(\)で改行する

Ubuntuではapt-get(apt)というコマンドでpackage管理
$apt-get update
$apt-get install <package>

キャッシュを上手く利用しよう
packageのインストールをlayerごとに全てやっていると時間がかかる。
キャッシュとは、、すでに実行したもののこと。
これを利用して追加分だけ実行するlayerを作成
あらたにRUN を追加するだけ

dockerコンテキストにdockerfileがない場合
$docker build -f <dockerfilename> <build context>

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー

-v <host>:<containerのマウント先> オプションでホストのファイルシステムをコンテナにマウント。コンテナ内のディレクトリは勝手に作成

$docker run -it -v <マウンドしたいディレクトリのパス>:<そのディレクトリを入れるdockerコンテナのディレクトリの絶対パス>　<image> bash
ホスト環境での変更もコンテナに反映される

-u ホストとコンテナのアクセス権限を共有
$docker run -it -u $(id -u) $(id -g) <image> bash #bashを入れるとなぜかlocalhostにアクセスできないので一回なしでやる
$id -u ユーザーidの確認
$id -g グループidの確認

-p ホストのポートをコンテナのポートにつなげる
-p <host_port>:<container_port>

(ex jyupternoteを実行)
$docker run -it -p 8889:8888 --rm jupyter/datasicence-notebook bash
ブラウザからlocalhost:8889にアクセス(番号はなんでもおけ)

--cpus <#ofCPU> --memory <byte> #リソースの設定
$docker inspect <container> | grep -i <知りたい情報(cpusやmemory)>


----------------------------------------------------------------------------------------------
aws
chmod 400 ~/.ssh
ssh -i ~/.ssh/docker_test.pem ubuntu@<パブリックDNS>
sshでインスタンスに入ってsudo apt install docker.io
sudoなしでdockerコマンドを打てるように。。。。
sudo gpasswd -a ubuntu docker
一回exitして入りなおすとsudoなしでdockerコマンドが打てる

EC2にdockerimagesを共有
パターン１　レジストリを介して送る
パターン２　dockerfileを送る
パターン３　Docker imageをtarにして送る
docker save <imageID> > myimage.tar

ファイルをインスタンスに送る
$sftp -i ~/.ssh/docker_test.pem ubuntu@<パブリックDNS>
$put <送りたいファイルのlocalのパス> [リモートのpath]
$get 取得したいファイルのパス 送信先のローカルpath
SFTPのコマンド
put リモートにファイルを送る
get リモートサーバーからファイルを取得

リモートサーバーでtarファイルをDocker imageに変換
$docker load < myimage.tar

ボリュームの増やし方

aws上でユーザーの作成
sudo adduser --uid <id> <name>