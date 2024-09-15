# Список библиотек

## Стандартная библиотека

### Библиотеки для работы с текстом

- [**STRING** - текстовые константы и шаблоны](standart/string.md)
- [**TEXTWRAP** - форматирование текстовых абзацев](standart/textwrap.md)
- [**RE** - регулярные выражения](standart/re.md)
- [**DIFFLIB** - сравнение последовательностей](standart/difflib.md)

### Структуры данных

- [**ENUM** - перечисление](standart/enum.md)
- [**COLLECTIONS** - сравнение последовательностей](standart/collections.md)
- [**ARRAY** - последовательность данных фиксированного типа](standart/array.md)
- [**QUEUE** - потокобезопасная реализация очереди FIFO](standart/queue.md)
- [**STRUCT** - структуры двоичных данных](standart/struct.md)
- [**WEAKREF** - слабые ссылки на объекты](standart/weakref.md)
- [**COPY** - создание дубликатов объектов](standart/copy.md)
- [**PPRINT** - "красивая печать" структур данных](standart/pprint.md)

### Алгоритмы

- [**FUNCTOOLS** - инструменты для манипулирования функциями](standart/functools.md)
- [**ITERTOOLS** - функции-итераторы](standart/itertools.md)
- [**OPERATOR** - функциональный интерфейс встроенных операторов](standart/operator.md)
- [**COTEXTLIB** - утилиты менеджеров контекста](standart/contextlib.md)

### Дата и время

- [**TIME** - системное время](standart/time.md)
- [**DATETIME** - манипулирование значениями даты и времени](standart/datetime.md)
- [**CALENDAR** - работа с датами](standart/calendar.md)

### Математика

- [**DECIMAL** - математика чисел с фиксированной точностью и чисел с плавающей точкой](standart/decimal.md)
- [**FRACTIONS** - рациональные числа](standart/fractions.md)
- [**RANDOM** - генератор псевдослучайных чисел](standart/random.md)
- [**MATH** - математические функции](standart/math.md)
- [**STATISTICS** - статистические рассчёты](standart/statistics.md)

### Файловая система

- [**OS.PATH** - платформонезависимое манипутирование именами файлов](standart/ospath.md)
- [**PATHLIB** - пути файловой системы как объекты](standart/pathlib.md)
- [**GLOB** - шаблоны имён файлов](standart/glob.md)
- [**FNMATCH** - шаблоны модуля Glob в стиле Unix](standart/fnmatch.md)
- [**LINECACHE** - эффективное чтение файлов](standart/linecache.md)
- [**TEMPFILE** - временные объекты файловой системы](standart/tempfile.md)
- [**SHUTIL** - высокоуровневые файловые операции](standart/shutil.md)
- [**FILELECMP** - сравнение файлов](standart/filelecmp.md)
- [**MMAP** - файлы, отбражаемые в памяти](standart/mmap.md)
- [**CODECS** - кодирование и декодирование строк](standart/codecs.md)
- [**IO** - инструменты для работы с текстовыми, двоичными и "сырыми" потоками ввода-вывода](standart/io.md)

### Постоянное хранение и обмен данными

- [**PICKLE** - сериализация объектов](standart/pickle.md)
- [**SHELVE** - постоянное хранение объектов](standart/shelve.md)
- [**DBM** - базы данных Unix с доступом по ключу](standart/dbm.md)
- [**SQLITE3** - встроенная реляционная база данных](standart/sqlite3.md)
- [**xml.etree.ElementTree** - API для манипулирования XML-элементами](standart/xmltree.md)
- [**CSV** - файлы с данными, разделёнными запятыми](standart/csv.md)

### Сжатие и архивирование данных

- [**ZLIB** - сжатие данных средствами библиотеки GNU zlib](standart/zlib.md)
- [**GZIP** - чтение и запись файлов GNU zip](standart/gzip.md)
- [**BZ2** - формат сжатия bzip2](standart/bz2.md)
- [**TARFILE** - доступ к архивам Tar](standart/tarfile.md)
- [**ZIPFILE** - доступ к ZIP-архивам](standart/zipfile.md)

### Криптография

- [**HASHLIB** - криптографическое хеширование](standart/hashlib.md)
- [**HMAC** - криптографические цифровые подписи и верификация сообщений](standart/hmac.md)

### Параллельные вычисления: процессы, потоки и сопрограммы

- [**SUBPROCESS** - порождение дополнительных процессов](standart/subprocess.md)
- [**SIGNAL** - асинхронные системные события](standart/signal.md)
- [**THREADING** - управление параллельными вычислениями в рамках одного процесса](standart/threading.md)
- [**MULTIPROCESSING** - использование процессов вместо потоков](standart/multiprocessing.md)
- [**ASYNCIO** - асинхронные операции ввода-вывода, цикл событий и инструменты параллелизма](standart/asyncio.md)
- [**CONCURRENT.FUTURES** - управление пулами параллельных задач](standart/concurrent.futures.md)

### Обмен данными по сети

- [**IPADDRESS** - интернет-адреса](standart/ipaddress.md)
- [**SOCKET** - сетевое взаимодействие](standart/socket.md)
- [**SELECTORS** - абстракции мультиплекирования ввода-вывода](standart/selectors.md)
- [**SELECT** - эффективное ожидание завершения ввода-вывода](standart/select.md)
- [**SOCKETSERVER** - создание сетевых серверов](standart/socketserver.md)

### Интернет

- [**URLLIB.PARSE** - разбиение URL-адресов на отдельные элементы](standart/urllib.parse.md)
- [**URLLIB.REQUEST** - доступ к сетевым ресурсам](standart/urllib.request.md)
- [**URLLIB.ROBOPARSER** - управление действиями веб-роботов](standart/urllib.roboparser.md)
- [**BASE64** - кодирование двоичных данных с помощью ASCII](standart/base64.md)
- [**HTTP.SERVER** - базовые классы для реализации веб-серверов](standart/http.server.md)
- [**HTTP.COOKIES** - cookie-файлы HTTP](standart/http.cookies.md)
- [**WEBBROWSER** - отображение веб-страниц](standart/webbrowser.md)
- [**JSON** - JavaScript Object Notation](standart/json.md)
- [**XMLRPC.CLIENT** - клиент XML-RPC](standart/xmlrpc.client.md)
- [**XMLRPC.SERVER** - сервер XML-RPC](standart/xmlrpc.server.md)

### Электронная почта

- [**SMTPLIB** - клиент SMTP](standart/smtplib.md)
- [**SMTPD** - примеры почтовых серверов](standart/smtpd.md)
- [**MAILBOX** - манипулирование архивами электронной почты](standart/smtpd.md)
- [**IMAPLIB** - клиентская библиотека IMAP4](standart/smtpd.md)

### Строительные блоки приложений

- [**ARGPARSE** - анализ параметров и аргументов командной строки](standart/smtpd.md)
- [**GETOPT** - анализ параметров командной строки](standart/getopt.md)
- [**READLINE** - библиотека GNU Readline](standart/readline.md)
- [**GETPASS** - безопасныый ввод пароля](standart/getpass.md)
- [**CMD** - построчные командные процессы](standart/cmd.md)
- [**SHLEX** - лексический анализ синтаксисов в стиле командной оболочки Unix](standart/shlex.md)
- [**CONFIGPARSER** - работа с конфигурационными файлами](standart/configparser.md)
- [**LOGGING** - механизм структурированного журналирования](standart/logging.md)
- [**FILEINPUT** - библиотека фильтров для утилит командной строки](standart/fileinput.md)
- [**ALEXIT** - вызов функций интерпретатором при завершении работы программы](standart/alexit.md)
- [**SCHED** - планирование запуска событий](standart/sched.md)

### Интернационализация и локализация приложений

- [**GETTEXT** - каталоги сообщений](standart/gettext.md)
- [**LOCALE** - API локализации](standart/locale.md)

### Инструментами разработки

- [**PYDOC** - оперативная справка для модулей](standart/pydoc.md)
- [**DOCTEST** - тестирование документации](standart/doctest.md)
- [**UNITTEST** - фреймворк автоматизированного тестирования](standart/unittest.md)
- [**TRACE** - трассировка выполнения программы](standart/trace.md)
- [**TRACEBACK** - исключения и стек вызовов](standart/traceback.md)
- [**CGITB** - подробные отчёты о необработанных исключениях](standart/cgitb.md)
- [**PDB** - интерактивный отладчик](standart/pdb.md)
- [**PROFILE** и **PSTATS** - анализ производительности](standart/profile.md)
- [**TIMEIT** - замер времени выполнения нбольших фрагментов кода Python](standart/timeit.md)
- [**TABNANNY** - проверка отступов](standart/tabnanny.md)
- [**COMPILEALL** - файлы скомпилированного байт-кода](standart/compileall.md)
- [**PYCLBR** - обозреватель классов](standart/pyclbr.md)
- [**VENV** - создание виртуальных окружений](standart/venv.md)
- [**ENSUREPIP** - программа-установщик пакетов Python](standart/ensurepip.md)

### Иструменты среды времени выполнения

- [**SITE** - конфигурирование сайта](standart/site.md)
- [**SYS** - настройка конфигурационных параметров, специфических для системы](standart/sys.md)
- [**OS** - портируемый доступ к средствам, сецифическим для операционных систем](standart/os.md)
- [**PLATFORM** - информация о версии системы](standart/platform.md)
- [**RESOURCE** - управление системными ресурсами](standart/resource.md)
- [**GC** - сборщик мусора](standart/gc.md)
- [**SYSCONFIG** - управление конфигурацией интерпретатороа во время компиляции](standart/sysconfig.md)

### Иструменты языка

- [**WARNINGS** - предупреждения о потенциальых проблемах](standart/warnings.md)
- [**ABC** - абстрактные базовые классы](standart/abc.md)
- [**DIS** - дизассемблирование байт-кода Python](standart/dis.md)
- [**INSPECT** - инспектирование активных объектов](standart/inspect.md)

### Модули и пакеты

- [**IMPORTLIB** - механизм импорта Python](standart/importlib.md)
- [**PKGUTIL** - вспомогательные функции для упаковывания программ](standart/pkguyil.md)
- [**ZIPIMPORT** - загрузка кода Python из ZIP-архивов](standart/zipimport.md)
