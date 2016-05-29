## Репозиторий проекта CCIE за год
http://ccie.linkmeup.ru/


Для синхронизации репозитория с конфигурациями надо выполнить:
```code
git clone https://github.com/linkmeup/CCIE
cd CCIE
git pull origin master
```
Затем, для синхронизации изменений, достаточно выполнять:
```code
git pull origin master
```
Можно также сделать алиас для команды.
Добавить в  ~/.bashrc, например, такую строку:
```code
alias ccie-linkmeup="cd /полный_путь_к_каталогу_CCIE/CCIE/; git pull origin master; cd -"
```
Тогда синхронизировать репозиторий можно по команде ccie-linkmeup (автопродолжение для этой команды будет работать).
