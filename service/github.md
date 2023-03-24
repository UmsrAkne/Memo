
# Github

## push の時に警告が出る
    
    > git push
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    **:**:**:**:**:**:**:**:**:**:**:**:**:**:**:**.
    Please contact your system administrator.
    Add correct host key in /Users/hnw/.ssh/known_hosts to get rid of this message.
    Offending RSA key in /Users/hnw/.ssh/known_hosts:133
    RSA host key for example.com has changed and you have requested strict checking.
    Host key verification failed.

ip が変わったりした時に出るらしい？

    Offending RSA key in /Users/hnw/.ssh/known_hosts:133

のところで問題になっている行を確認できるので、該当行を確認して削除する。

本当に攻撃されてるかもしれない場合は……どうするんだろう。
