> # Начало работы с Git
> Git представляет собой систему контроля версий (VCS). Он предназнчен для хранения изменений в программном коде проектов и совместной работе над ними.  
> ### Работа с командной строкой
> Основной инструмент работы с Git - это консоль `bash`. Она характерна для OC Linux. При установке ее на Windows устанавливаются также некоторые прочие компонеты Linux. Вот некоторые наиболее часто используемые консольные команды:  
> 
 - `cd` - смена директории
 - `pwd` - печать текущего пути
 - `ls` - просмотр текущей директории (модификатор `-a` позволяет просмотреть также и скрытые файлы и папки)
 - `touch` - создание файла
 - `mkdir` - создание директории
 - `cp` - копирование файла или папки
 - `mv` - перемещение файла или папки (также их переименование)
 - `rm` - удаление файла (модификатор `-r` позволяет удалить папку и ее содержимое)
 - `rmdir` - удаление пустой папки
 - `cat` - просмотр текстового файла  

Пути в консоли прописываются в виде `/home/user/file.txt`. Запись букв диска Windows выглядит`c:/` или `/c/`. Корневая директория с исполняемый файлом `bash` записывается как `/`. Домашняя директория пользователя обозначается `~`, текущая рабочая директория как `./`, а директория на уровень выше `../`. Если в названии директорий и файлов встречаются пробелы - их записывают в кавычках: например,`"folder name"`.  
Команды в консоли не обязательно вводить полностью - например, имена файлов могут быть автоматически дополнены нажатием `Tab`. Повторный ввод команд также не обязателен - их можно вызвать и буфера клавишами со стрелками `Up\Dn`.
Для редактирования файлов можно использовать можно использовать встроенные редакторы - `nano` или `vi`. Вот пример работы в `vi`:

 1. В консоли ввести `vi file.txt`
 2. Для начала редактирование в редакторе ввести `^I`
 3. По окончании редактирования нажать `Esc`
 4. Для записи изменений и и выхода из редактора ввести `:wq`  

### Создание репозитория Git
Работа с Git начинается с записей в файле конфигурации:
~~~bash
$ git config --global user.name "John Doe"
$ git config --global user.emal "johndoe@yandex.ru"
~~~
Это необходимо при совместной работе, чтобы понимать, кто являлся автором изменений. Адрес электронной почты нужен для связи. Стоит указывать адрес, указанный при регистрации в удаленном репозитории (например, на GitHub). Файл конфигурации `.gitconfig` находится в папке с программой Git и его можно просмотреть с помощью:
~~~bash
$ git config --list
~~~
Для создания локального репозитория необходимо перейти в папку с файлами проекта. Создание репозитория выполняется командой: 
~~~bash
$ git init
~~~
После чего в папке с проектом появится подпапка `.git`со всей служебной информацией. Ее можно принудительно удалить со всеми изменениями:
~~~bash
$ rm -rf .git
~~~
Под каждый новый проект создается свой локальный репозиторий в собственной папке.

### Простейшие действия с Git
У файлов проекта с точки зрения Git может быть несколько состояний:

 1. _untracked_
 2. _staged_
 3. _modified_
 4. _unmodified_  

Файлы типа _untracked_- это не отслеживаемые файлы в рабочей папке. Они могут быть проиндексированы Git и тогда перейдут в категорию _staged_:
~~~bash
$ git add file.txt
~~~
После внесения в них изменений они переходят в состояние _modified_. После записи коммита они становятся _unmodified_  :
~~~bash
$ git commit -m "Текст комментария"  
~~~
Отслеживать изменения в репозитории позволяет команда:
~~~bash
$ git status
~~~
Просмотреть историю коммитов можно с помощью команды:
~~~bash
$ git log
~~~

### Работа с GitHub
Платформа GitHub предоставляет возможность размещения удаленного репозитория Git. После регистрации нового аккаунта необходимо создать новый репозиций на сайте. Название удаленного репозитория стоит сделать как у локальной папки.  
Для работы с ним необходимо установление защищенного канала передачи данных. Делается это с помощью протокола SSH. Осуществить это можно сгенерировав у себя пару ssh-ключей. Так:
~~~bash
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
~~~
или так (модификатор `-t` указывает на тип шифрования, для RSA стоит указать битность `-b`):
~~~bash
$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
~~~
Публичный ключ с расширением `.pub` отправляется на GitHub, а приватный сохраняется в папке пользователя (подпапка `.ssh`). Ключ передается в виде содержимого файла - его можно скопировать в буфер обмена как:
~~~bash
$ clip < ~/.ssh/id_ed25519.pub
~~~
После регистрации ssh-ключа на сайте GitHub можно проверить работу связи:
~~~bash
$ ssh -T git@github.com
~~~
Привязка удаленного репозитория к локальному выполняется из локальной папки с помощью:
~~~bash
$ git remote add origin ssh-url
~~~
Где, ssh-url - это интернет-адрес страницы репозитория в форме SSH (его можно скопировать в GitHub), а origin - дефолтное название удаленного репозитория (при командной работе оказывается, что именно удаленный репозиторий является исходным, а локальный - копией для редактирования) Чтобы убедиться, что репозитории связаны используют:
~~~bash
$ git remote -v
~~~
Наконец, для отправки изменений на удаленный репозиторий используется команда:
~~~bash
$ git push
~~~
А при первом использовании:
~~~bash
$ git push -u origin main
~~~
Где `main` - название основной ветки.