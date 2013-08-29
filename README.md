# sbash

sbashは、対話処理に対して以下2機能を追加したbashです。  
※ベースのbashのバージョンは、4.2です。
* コマンドフィルター機能（特定ユーザに対して、特定コマンドの実行を拒否/許可する）
* コマンドログ機能（実行したコマンドの履歴をリアルタイムにログファイルに出力する）

# 使い方
### バイナリを取得/設置 ###
    # git clone https://github.com/raqda/sbash.git
    # cp -a sbash/bin/sbash /bin/

### ログインシェルの変更 ###
    # vipw 
    root:x:0:0:root:/root:/bin/sbash
    ...

# コマンドフィルター機能
    # touch /etc/sbash.deny
    # touch /etc/sbash.allow

特定ユーザに対し、特定コマンドを実行不可にします。  
/etc/sbash.deny に不可とするユーザ/コマンドを設定します。  
/etc/sbash.allow に設定されているユーザ/コマンドは許可されます。  
※deny, allow の順に評価

### sbash.denyの記述例 ###
    # すべてのユーザに対して、「reboot」コマンドを実行不可とする
    reboot:=:ALL
    # adminユーザに対して、「shutdown」で始まるコマンドを実行不可とする
    shutdown:^:admin
    # rootユーザに対して、「rm」を含むコマンドを実行不可とする
    rm:.:root

* sbash.allowも書式は同じです。設定されたユーザ/コマンドが実行可となります
* ファイルの変更内容はリアルタイムで反映されます
* 設定するコマンドに「:」は使用できません
* 先頭が「#」で始まる行は、コメント扱いとなります

# コマンドログ機能
    touch /var/log/sbash
    chmod 777 /var/log/sbash 

ログファイルを生成すると、ログを吐き出します。  
ファイルが存在しない場合は、ログ機能は無効となります。
