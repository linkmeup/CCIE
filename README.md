## Репозиторий проекта CCIE за год
http://ccie.linkmeup.ru/

Для того, чтобы синхронизировать конфигурации, по мере того как они будут выкладываться, вам нужно будет установить Git.

Если вы работаете на linux (unix/mac), то для синхронизации репозитория с конфигурациями надо выполнить:
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

Для тех, кто использует Windows.

Можно поставить, например, такой графический клиент Git: https://git-scm.com/download/win

Или один из этих: https://git-scm.com/downloads/guis
