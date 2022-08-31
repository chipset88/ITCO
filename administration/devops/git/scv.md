# Системы контроля версий

---
## Источники:
+ [Git - Book](https://git-scm.com/book/ru/v2/) - официальный сайт системы

---

То, что GIT - вещь нужная и полезная, расписывать особо не нужно. Данная заметка нужна для того, чтобы структурировать знания по этой системе и закрепить набор устоявшихся в отрасли практик её использования.

## Основное
GIT - это **распределённая система** контроля версий, то есть наличие какого-то удалённого репозитория, ветки *master* (или как сейчас её богомерзки называют *main*) не обозначает, что у нас есть 'сверх-ветка', без которой вся работа над проектом развалится. Централизации стараются придерживаться только по организационнным соображениям. Однако из каждого снимка, который обозначает текущее состояние проекта можно продолжить работу над ним, в любой момент можно легко и непринуждённо сделать ***fork***, то есть обособить под своё владение проект и вертеть его как хочешь. Нет сложностей в том, чтобы потом сделать запрос на слияние и при этом владелец основного репозитория сможет очень просто рассмотреть те изменения, которые были внесены в его детище. Сделать это можно как из CLI так и при помощи функционала, который предоставляют различные платформы: GitHub, Gitlab, Gitea, Bitbucket.

Если развернуть вышеупомянутое утверждение о распределённости системы техническим языком, то вместо номеров ревизий (как это делалось в централизованных СКВ), кажждый коммит идентифицируется при помощи хэш-суммы. Безусловно, коммиты хранят данные о том, кто сделал изменения, когда и какие (иначе зачем они были бы нужны). Но при этом все эти изменения являются равнозначными, что позволяет чётко и ясно составить список конфликтов и наглядно сравнив варианты, принять один из них или разрешить (***resolv***) по своему.

К тому же, и это уже не является уникальным признаком данного вида СКВ, но очень важно в современной работе администратора, вышеперечисленные платформы интегрируются с различными системами, которые позволяют автоматически тестировать изменения кода, сообщать о результатах тестов по электронной почте, автоматически деплоить код на сервера, что позволяет избежать человеческого фактора - один раз настроил систему, а потом следишь за ней централизованно. То есть СКВ сейчас - это основа подхода непрерывной интеграции, доставки и тестирования.

Устанавливается система просто как в Windows так и в Linux - останавливаться на этом не вижу смысла.

Единственное, что нужно будет сделать - это внести данные в глобальные переменные GIT:
```bash
git config --global user.name "{Your GitHub/Gitlab username}"
git config --global user.email ypurmail@example.com
```
В противном случае каждый коммит, а затем пуш, система будет требовать от нас эти данные. Полный список всех настроек можно посмотреть так ```git config --list --show-origin```

### Основные понятия

**Репозиторий** - собственно какталог, в кором содержится код проекта И! каталог ```.git/``` в котором находятся файлы с информацией о коммитах, текущем состоянии, URL удалённого репозитория, различные ключи, секреты и прочее связанное с контролем версий. Именно наличие этой папки делает репозиторий репозиторием. Репозиторий, в котором находится только директория ```.git/``` называется пустым, но, тем не менее, это уже Репозиторий и с ним можно делать всё, что мы дальше распишем.

Инициализируется репозиторий командой ```git init``` либо можно склонировать имеющийся репозиторий с удалённого сервера, но о работе с ним поговорим отдельно.

**Три состояния** - это очень важная вещь в понимании работы системы:
+ изменённые файлы;
+ индексированные файлы;
+ зафиксированные файлы.

Есть ещё файлы, которые могут находиться в проекте и которые мы по соображениям здравого смысла и безопасности не хотим вносить в систему (фотографии, данные пользователей, ключи в открытом виде и т. п.), для чего создаём в корне репозитория файлик ```.gitignore``` и в каждой строчке прописываем относительно корня репозитория пути до этих фалов и каталогов. НО! Если мы в этот файлик внесём путь до этого же файлика, то система перестанет его читать и толку от него не будет. Поэтому состояние игнора мы не рассматриваем как одно из основных.

***Изменённые файлы*** (modified) - это те файлы, которые мы, как ни странно, изменили, но ещё не успели добавить в список для создания снимка (коммита). Соответственно, в IDS-ах или графических программах для работы с GIT такие файлы помечают буквой 'M' (misterion :-) )

![misterion](../../../img/Mysterion_2.webp "M - misterion")

Их список можно посмотреть командой ```git status```

***Индексированные файлы*** (staged) - когда изменённый файл добавлен в список (индекс) для следующего коммита. Происходит это при помощи команды ```git add [здесь список файлов для внесения в индекс]```, но чаще всего вносят все файлы, которые были изменены говоря об этом точкой ```.``` или такой конструкцией ```./*```. В графических программах напротив таких файлов появляется буква ```A```

***Зафиксированные файлы*** (committed) - это те, которые, собственно находятся в системе и ... короче, никуда они уже запросто так не денуться. В графических программах они отображаться вместе с остальными в простом дереве файлов и никак специально не выделяются. Коммит происходит командой ```git commit -m "Краткое описание изменений"```

***Вопрос:*** "Зачем файлы разделены на изменённые и индексированные, если всё равно чаще всего проблему добавления решают командой ```git add .```?" Всё это сделано для того, чтобы в репозиторий попадали только те файлы, которые по нашему мнению действительно нужны проекту. Поэтому, как говорят англоманы, best practic является использование команды ```git status``` перед и добавлением в индекс, и, тем более, коммитом. По списку можно будет понять, что система "увидела" медиафайлы, которые не являются частью разрабатываемого приложения. Или какая-нибудь "мусорная" или конфиденциальная информация как данные IDE или адреса реальных людей. Увидев это всё, мы быстренько внесём изменения в ```.gitignore``` и избежим сложной процедуры удаления нежелательных данных из репозитория.

Собственно, на этом можно считать, что GIT для индивидуального локального пользования освоен и в рамках локальной машины уже можно не волноваться, что проект куда-то денется. Однако, если вспомнить, что этот некультурный финн по гражданству и швед по национальности...
![FuNVIDIA](../../../img/torvalds-nvidia.jpg "Икона OpenSource")
...создавал систему для совместного использования, то она предполагает прежде всего работу в команде. А так как архитектура была продумана хорошо, то размер команды значения не имеет. В следующей статье  определим базовую последовательность и возможные проблемы при работе с удалённым репозиторием.