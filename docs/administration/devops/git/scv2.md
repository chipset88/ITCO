# Взаимодействие локального и удалённого репозиториев

[Следующая статья](scv3.md)

---
## Команды из статьи
+ ```ssh-keygen -t rsa -b 4096 -C "yourmail@example.com"```
+ ```eval "$(ssh-agent -s)"```
+ ```ssh-add ~/.ssh/id_rsa```
+ ```git remote add origin git@github.com:boyko1989/testGit.git```
+ ```git remote -v```
+ ```git push -u origin master```
+ ```git push```
+ ```git pull```
---

Договоримся, что в качестве удалённого сервера будет использоваться GitHub. Gitlab обладает гораздо более интересным функционалом: можно сделать свой git-сервер установив его на своём оборудовании или в облаке, есть API, который позволяет взимодействовать с сервером через CLI (создавать на сервере репозитории, раннеры, пайплайны и прочее) и много чего ещё, что обязательно рассмотрим в отдельной статье.

## SSH-ключи

В первой статье не упоминалось, что нужно отправить SSH-ключи в используемый хостинг при установке системы GIT, но по идее, это можно сделать, когда мы прописываем данные в ```git config```. Уточним как это делается.

Сгенерируем ключи:
```bash
ssh-keygen -t rsa -b 4096 -C "yourmail@example.com"
```
Можно прожать ```enter``` оставляя всё по-умолчанию. Что касается email, указанного здесь - это тот же, что мы ввели, при первоначальной настройке GIT. В Linux, по идее, можно обойтись и простой командой ```ssh-keygen``` - всё вполне неплохо будет работать.

Путь для сохранения ключей в случае пользования Windows - это ```C:/Users/<user>/.ssh/id_rsa```. Это нужно знать, так как дальше мы добавим ключи в SSH-агент. Убедимся, что он работает командой ```eval "$(ssh-agent -s)"```, на что мы должны получить его PID. Ну и, собственно, добавляем ключ в него: ```ssh-add ~/.ssh/id_rsa```, если мы ничего не меняли в процессе генерации. В итоге должны получить победное:
```bash
Identity added: /c/Users/ВАШЕ_ИМЯ/.ssh/id_rsa (/c/Users/ВАШЕ_ИМЯ/.ssh/id_rsa)
```
Осталось только добавить ключи на GitHub. Для этого виндузятники открывают файл ```C:/Users/ВАШЕ_ИМЯ/.ssh/id_rsa.pub``` (в Linux ```~/.ssh/id_rsa.pub```), и копируют его содержимое в аккаунте GitHub → (Правое верхнее меню) "*Settings*" → "*SSH and GPG keys*" → "*New SSH key*" → Поле "*Key*". Придумываем осмысленный *Title* и можем работать с удалённым репозиторием.

## Ещё раз о создании репозитория

В прошлой статье был рассмотрен вариант создания локального репозитория. В случае, если мы хотим его увидеть на удалённом git-хостинге, то нужно будет сделать следующее: во-первых, нужно создать пустой репозиторий на GitHub с именем, соответствующим имени локального репозитория. К сожалению, я не нашёл способ создать удалённый реп из терминала. Допустим, мы создаём репозиторий **testGit** у пользователя **boyko1989**. В таком случае нам нужно, чтобы в этом репозитории все файлы были зафиксированы. 

Следующий шаг - это добавление URL удалённого репозитория. Его можно взять из кнопки 'Code' на странице созданного пустого репозитория на GitHub. Следующая команда свяжет их:

```bash
git remote add origin git@github.com:boyko1989/testGit.git
git remote -v # Для проверки

origin  git@github.com:boyko1989/testGit.git (fetch)
origin  git@github.com:boyko1989/testGit.git (push)
```
Ну а дальше отправляем наши данные на сервер:
```bash
git push -u origin master
```
Ну и всё: после этого мы можем видеть как содержимое нашего локального репозитория оказалось на сервере.

### Что такое origin

Если просто, то на этом месте в команде определяется **имя подключения** к репозиторию. Конкретно origin - это основное подключение. Хотя мы можем в файлике ```.git/config``` (секцию remote, которого мы редактируем командой ```git remote ... ```) обозвать это подключение как нам будет угодно. 

Подключений может быть несколько, например мы вполне можем решить создать такой же репозиторий и на Gitlab. Для этого создадим соответствующее подключение:

```bash 
$ git remote add gitlab git@gitlab.com:boyko1989/testGit.git

$ git remote -v
gitlab  git@gitlab.com:boyko1989/testGit.git (fetch)
gitlab  git@gitlab.com:boyko1989/testGit.git (push)
origin  git@github.com:boyko1989/testGit.git (fetch)
origin  git@github.com:boyko1989/testGit.git (push)
```
Здесь очень важное отличие между GitHub и Gitlab - после того как я прописал в URL для последнего в своём *пространстве имён* адрес ***будущего*** репозитория, мне не нужно создавать репозиторий вручную. Вместо этого я просто пишу:

```bash
$ git push -u gitlab master
# И получаю следующий вывод, который говорит о том, что репозиторий создан
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 660 bytes | 330.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote:
remote: The private project boyko1989/testGit was successfully created.
remote:
remote: To configure the remote, run:
remote:   git remote add origin git@gitlab.com:boyko1989/testGit.git
remote:
remote: To view the project, visit:
remote:   https://gitlab.com/boyko1989/testGit
remote:
remote:
remote:
To gitlab.com:boyko1989/testGit.git
 * [new branch]      master -> master
branch 'master' set up to track 'gitlab/master'.
```
Правда теперь я должен при пуше всякий раз прописывать имя подключения для GitHub, потому что подключение origin оказалось ниже новых записей и отправка кода осуществляется туда. А так как это происходит на один адрес за раз, то и после отправки на Gitlab кода командой ```git push```, я должен прописать ```git push origin```

```bash 
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To gitlab.com:boyko1989/testGit.git
   11503cc..25d1be6  master -> master

$ git push origin
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:boyko1989/testGit.git
   11503cc..25d1be6  master -> master

```

> !!!! Но  Но делать так крайне нежелательно !!!! Это всё чревато дополнительным запутыванием процесса. Никто не запрещает переезжать с одного хостинга на другой - так лучше и делать.

### Второй вариант создания репозитория

Это создание репозитория клонированием уже имеющегося удалённого. Технически - это не то чтобы прямо таки создание, но как частный случай мы можем его рассматривать. Делается просто: создаём репозиторий на GitHub, берём из кнопки 'Code' на странице репозитория URL (сейчас нужно брать ТОЛЬКО SSH) и на локальной машине в нужном месте открываем терминал GIT и вводим команду:

```bash
git clone git@github.com:boyko1989/testGit.git
```
Репозиторий в нашем распоряжении! Технически - оба варианта равнозначны. Какой нравится, или какой удобен, таким можно и пользоваться.

## Собственно, само взаимодействие

Есть два понятия PUSH и PULL, у которых перевод: "толкай" и "тяни", соответственно. Эта простая концепция, будет встречаться постоянно при построении взаимодействующих систем. Например, при использовании программы ```rsync```. 

Нужно понять, что есть два параметра - это **SRC** (source - "ресурс" или "источник") и **DST** (destination - "назначение") Кино такое было "Final destination" или ... Так вот, всё завист от того откуда куда идут байтики. И, как ни странно, на этом люди попадаются нередко - элементарно путают. Когда локальная машина - это **SRC**, значит происходит **PUSH**: мы байты ТОЛКАЕМ, соответственно, когда наоборот - мы их тянем.

Понятно, что необходимости в PUSHе каждый коммит нет. Польза, вред и идеальная частота определяется теми процессами, которые имеются в команде. Существет понятие **WorkFlow** или рабочий поток. Имеются отработанные процессы, которые наиболее применимы для работы. О них будет [следующая статья](scv3.md).

Также хорошей практикой перед началом работы всегда делать ```git pull```, так как всегда имеется вероятность, что кто-то что-то написал и чтобы не тратить время на решение конфликтов, лучше работать с самой последней версией проекта или ветки.

## Возникающие проблемы

### git push

**Первая проблема**, с которой сталкиваются люди, которые только начинают работать с системой - это невозможность пушить или клонировать репозитории из-за отсутствия правильных ключей. Сообщения об ошибках выглядят примерно так:
```bash
Please make sure you have the correct access rights
and the repository exists.
---------------------------------------------------------
ERROR: You're using an RSA key with SHA-1, which is no longer allowed. Please use a newer client or a different key type.
Please see https://github.blog/2021-09-01-improving-git-protocol-security-github/ for more information.

fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists. 
---------------------------------------------------------
Permission denied (publickey).fatal: 
The remote end hung up unexpectedly
```

Эта проблема решается или генерацией или перегенерацией и записью ключей на GitHub как описано выше.

**Ещё одна** проблема состоит в том, что репозиторий, к которому мы обращаемся как будто и отсутствует ...

```bash
ERROR: Repository not found.
fatal: Could not read from remote repository.
---------------------------------------------------------
error: src refspec master does not match any
error: failed to push some refs to 'github.com:boyko1989/testGit.git'
---------------------------------------------------------
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.
```

Возникнуть это может если мы пытаемся отправить код в действительно несуществущий репозиторий. Либо когда мы прописывали (если прописывали) URL ошиблись в нём. Либо при использовании команды ```git push -u origin master``` нужно проверить название подключения (origin ли) либо название ветки - сейчас по-умолчанию репозитории на GitHub создаются с ветками ```main```, потому что кто-то там не нашёл более подходящего для себя дела как только оскорбляться на слова, ибо ```master``` - "родимое пятно" рабовладельческих традиций (по-русски "хозяин"), а это нынче на "свободном" западе... Короче, это другая история.

Также одна из причин данной проблемы может быть в неправильной учётной записи. Стоит переопределить ```git config```, а в Windows залесть в *Панель управления* → *Учётные записи пользователей* → *Диспетчер учётных записей* и там попереудалять всё, что связано с github.com - возможно по какой-то причине что-то могло сохраниться неправильно.

**Также** существует проблема **конфликтов**. В таком случае в выводе будет что-то вроде:
```bash 
CONFLICT (content): Merge conflict in roses.txt
Automatic merge failed; fix conflicts and then commit the result.
```
Об их разрешении будет информация уже в [следующей](scv3.md) статье.