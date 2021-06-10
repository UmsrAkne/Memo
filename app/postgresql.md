## 接続を許可するIPを設定する

許可する ipアドレスのリストは pg_hba.conf に記述。  
以下コマンドでファイルを検索。

`$ sudo find /var -name pg_hba.conf`

または

`$ sudo find / -name pg_hba.conf`

/var ディレクトリを検索すればファイルが見つかりそう。  
ネットの資料を見ると他の場所にあるケースもありそうなのでルートを検索したほうが良い？

`$ sudo vim /var/lib/pgsql/data/pg_hba.conf`

アドレスは先のコマンドで表示された結果をそのまま入力する。sudo でないとファイルは開けない。  
開いたファイルに何も記述がなければ開けてないか、ファイルのアドレスが間違っている。  
許可するIPアドレスのリストはファイルの下の方にある。見ればわかるので詳細は省くが、ここに通したいアドレスを追記する。

`sudo systemctl reload postgresql.service`

最後にサービスを再起動する。こっちも sudo が必要。