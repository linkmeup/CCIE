Репозиторий проекта CCIE за год
http://ccie.linkmeup.ru/


Для синхронизации репозитория с конфигурациями надо выполнить:
```code
mkdir git_test
cd git_test
git init
git remote add ccie https://github.com/linkmeup/CCIE
git pull ccie master
 ```
Затем, для синхронизации изменений, достаточно выполнять:
```code
git pull ccie master
```
Можно также сделать алиас для команды.
Добавить в  ~/.bashrc, например, такую строку:
```code
alias ccie-linkmeup="cd /home/user/ccieinyear/git_test/; git pull ccie master; cd -"
```
Тогда синхронизировать репозиторий можно по команде ccie-linkmeup (автопродолжение для этой команды будет работать).
