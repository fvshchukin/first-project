Создаем README.md
# Изучаем Git и GitHub

## Привязываем SSH-ключ к GitHub

В прошлом уроке вы сгенерировали SSH-ключ, но он пока не привязан к аккаунту на GitHub. Исправим это.

## Инструкция по связыванию SSH-ключа и GitHub-аккаунта

1.После выполнения команды ssh-keygen из предыдущего урока в директории ~/.ssh будет создано два файла — id_ed25519 и id_ed25519.pub (или id_rsa и id_rsa.pub — в зависимости от того, какой алгоритм вы использовали):

- id_ed25519/id_rsa — приватный ключ (файл без .pub в конце). Ни в коем случае не копируйте его и не делитесь им.
- id_ed25519.pub/id_rsa.pub — публичный ключ (на это указывает расширение .pub).
  **Windows**

Cкопировать содержимое ключа в буфер обмена:

`$ clip < ~/.ssh/id_rsa.pub`
`# для ed25519:`
`$ clip < ~/.ssh/id_ed25519.pub`
В качестве альтернативы вы можете распечатать файл на экран с помощью cat ~/.ssh/id*rsa.pub и скопировать его вручную.
Если clip не сработает, выведите содержимое файла с помощью cat ~/.ssh/id_rsa.pub или cat ~/.ssh/id_ed25519.pub и скопируйте вывод в буфер обмена из консоли.
2.Перейдите на GitHub и выберите пункт Settings (англ. «настройки») в меню аккаунта.
![Settings](https://pictures.s3.yandex.net/resources/M2_T4_03_01_1685016747.png)
3.В меню слева нажмите на пункт SSH and GPG keys.
![Settings](https://pictures.s3.yandex.net/resources/M2_T4_03_02_1685016772.png)
4.В открывшейся вкладке выберите New SSH key (англ. «новый SSH-ключ»).
![Settings](https://pictures.s3.yandex.net/resources/M2_T4_03_03_1685016795.png)
5.В поле Title (англ. «заголовок») напишите название ключа. Например, Personal key (англ. «личный ключ»).
6.В поле Key type (англ. «тип ключа») должно быть Authentication Key (англ. «ключ аутентификации»).
7.В поле Key скопируйте ваш ключ из буфера обмена.
![Settings](https://pictures.s3.yandex.net/resources/M2_T4_03_04_1685016816.png)
8.Нажмите на кнопку Add SSH key (англ. «добавить SSH-ключ»).
![Settings](https://pictures.s3.yandex.net/resources/M2_T4_03_05_1685016840.png)
9.Проверьте правильность ключа с помощью следующей команды.
`$ ssh -T git@github.com`
Если это первый раз, когда вы используете Git, чтобы поделиться проектом на GitHub, появится похожее предупреждение.
`The authenticity of host 'github.com (140.82.121.4)' can't be established. ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. This key is not known by any other names. Are you sure you want to continue connecting (yes/no/[fingerprint])?`
Это предупреждение сообщает, что вы никогда не соединялись с сервером GitHub. Поэтому Git не может гарантировать, что сервер является тем, за кого он себя выдаёт.
Для подтверждения подлинности сервер генерирует и публикует ключи SHA256. Вы можете проверить ключи GitHub [по этой ссылке](https://practicum.yandex.ru/trainer/git-basics/lesson/4d662a58-3602-4c5c-9fad-be8cff334f37/). Если ключ в предупреждении совпадает с тем, что вы видите на сайте, значит, сервер является действительным. Введите yes, чтобы продолжить. Вы увидите приветствие на экране.
`Hi %ВАШ*АККАУНТ%! You've successfully authenticated, but GitHub does not provide shell access.`
Если у вас возникли сложности при генерации или привязке SSH-ключей, посмотрите видеоинструкцию, в которой мы показываем всё по порядку.

[![Посмотреть видеоинструкцию по генерации и привязке SSH-ключей]({image-url})]({<video controls src="https://code.s3.yandex.net/git_Basic/SSH_Screencast.mp4" title="video-ur"></video>l} "Link Title")

Ура: теперь ваш ключ привязан к GitHub! Если вы установили кодовую фразу для SSH-ключа, её нужно будет вводить для работы с репозиторием.

## Синхронизируем локальный и удалённый репозитории

Вы зарегистрировались на GitHub, сгенерировали SSH-ключ и привязали локальный репозиторий к удалённому. Самое сложное позади! Теперь разберём, как выкладывать свои правки на удалённый репозиторий. Но сначала немного о ветках.

### Основная ветка

Мы упоминали, что каждый коммит сохраняет актуальное состояние файлов. Сами же коммиты хранятся в ветках (англ. branch).

Если коммит — это снимок состояния файлов, то ветка — временна́я шкала, на которой расположены эти снимки. Ветка всегда начинается от одного из коммитов.

В репозитории может существовать сразу несколько веток — параллельных историй изменений. Также они могут соединяться друг с другом.

![Основная ветка](https://pictures.s3.yandex.net/resources/M2_T4_01-2_1685963656.png)

Самая первая ветка в репозитории появляется автоматически и называется main (англ. «основная») или master. Её имя нужно указывать при отправке коммитов на удалённый репозиторий или при получении их из него.

💡 main или master?

Раньше основная ветка в репозиториях, созданных на GitHub, называлась master, но с 1 октября 2020 года (после волны протестов движения Black Lives Matter) её переименовали в main.

Во всех репозиториях, созданных раньше этой даты, название основной ветки не поменялось. Поэтому в проектах, которые начали именно с master, и в руководствах по работе с Git вы по-прежнему можете встретить имя master.

## Отправить изменения на удалённый репозиторий — git push

Вы уже прошли весь «цикл коммита»: подготовили файлы с помощью git add, закоммитили их с комментарием командой git commit -m. Осталось загрузить содержимое локального репозитория на GitHub. За это отвечает команда git push (от англ. push — «толкать»).

В первый раз эту команду нужно вызвать с флагом -u и параметрами origin (имя удалённого репозитория) и main или master (название текущей ветки). Флаг -u свяжет локальную ветку с одноимённой удалённой. Как вы связывали локальный и удалённый репозитории в предыдущем уроке, так же и здесь нужно дополнительно связать ветки.

`$ git push -u origin main`  
_Если команда приведёт к ошибке, попробуйте заменить main на master_

Появится такой экран

![такой экран](https://pictures.s3.yandex.net/resources/M2_T4_02-2_1685963722.png)

При взаимодействии с удалёнными репозиториями Git выводит в консоль отладочную информацию: количество объектов (файлов), которые отправляются на сервер, информацию о прогрессе сжатия и записи и так далее.

Если вы указывали кодовую фразу при настройке SSH-ключей, её нужно будет ввести.

Зайдите в репозиторий first-project на GitHub. Вы увидите, что в репозитории появились файлы с изменениями.

![файлы с изменениями](https://pictures.s3.yandex.net/resources/M2_T4_03_01-2_1685963748.png)

В дальнейшем при работе с удалённым репозиторием флаг -u можно опустить и писать просто git push

## Работа с графическим интерфейсом GitHub

GitHub предоставляет удобный интерфейс для работы с репозиторием. Например, нажмите на кнопку commit в правой части страницы, чтобы просмотреть все коммиты в репозитории.

![Kнопкa commit в правой части страницы](https://pictures.s3.yandex.net/resources/M2_T4_03_02-2_1685963765.png)

Откроется окно с коммитами и их авторами.

![Oкно с коммитами](https://pictures.s3.yandex.net/resources/M2_T4_03_03-2_1685963782.png)

Сообщение коммита в репозитории тоже является ссылкой.

![Сообщение коммита](https://pictures.s3.yandex.net/resources/M2_T4_03_04-2_1685963797.png)

Перейдите по ссылке, кликните на текст последнего коммита над репозиторием — так вы сможете увидеть все изменения, которые были внесены в репозиторий в этом коммите.

![текст последнего коммита](https://pictures.s3.yandex.net/resources/M2_T4_03_05-2_1685963809.png)

## Задание для самостоятельной работы

1. Откройте проект first-project и создайте в нём файл task.txt с помощью touch.
2. Откройте файл task.txt в текстовом редакторе и внесите любые изменения. Затем сохраните и закройте файл.
3. Сделайте коммит и синхронизируйте изменения с удалённым репозиторием.

`$ git push`

Воспользуйтесь интерфейсом GitHub, чтобы посмотреть ваш последний коммит. Для этого нажмите на имя коммита.

Теперь ваши изменения могут увидеть те, кому вы отправите ссылку на проект!

Копилка ваших знаний о Git постепенно пополняется! Вот о чём мы рассказали:

- Коммиты хранятся в ветках. Начальная ветка создаётся автоматически и называется main или master.
- За отправку изменений на удалённый репозиторий отвечает команда git push.
- Интерфейс GitHub позволяет удобно просмотреть все коммиты в репозитории, а также изменения в этих коммитах.

## Файл README.md

Чтобы другие пользователи, а также потенциальные клиенты или работодатели могли понять, что представляет собой проект, его нужно описать. Такое описание принято указывать в файле README.md (от англ. read — «прочитай» и me — «меня»). В этом уроке вы научитесь оформлять такие файлы.

### Подробнее о том, зачем нужен README.md

Как правило, в README.md проекта можно найти следующую информацию:

1. Название проекта и его краткое описание: кем создан, для чего, какие решает задачи и какие закрывает проблемы.

2. Технологии, которые применяются в проекте. В чём его отличие от аналогичных.

3. Документация проекта — подробная инструкция о том, что представляет собой проект.

4. Планы проекта, если они есть.

Вот пример файла README.md для Git [на GitHub](https://practicum.yandex.ru/trainer/git-basics/lesson/c6b9607c-e8bc-4446-89f9-c74522c3492f/)

### Как создать и оформить README.md

README.md — текстовый файл, который можно создать командой touch, а затем редактировать так же, как и любой другой текстовый документ. Например, в блокноте.

Преимущество README.md в том, что средства командной работы (такие, как GitHub) могут отображать его содержимое в браузере в виде удобной разметки. Для этого нужно не просто залить текст, но и настроить шрифт, заголовки и отступы с помощью markdown. Маркда́ун — это специальный язык разметки. Он позволяет красиво отформатировать текстовый документ.

Разберём базовый синтаксис этого языка. Все правила запоминать не нужно: при оформлении репозитория вы всегда можете вернуться к этому уроку.

### Заголовки, абзацы и перенос

- Заголовки разных уровней создают решётками.

`# H1 — заголовок первого уровня, самый большой`

`## H2 — заголовок второго уровня, поменьше`

`### H3`

`#### H4`

`##### H5`

`###### H6 — заголовок шестого уровня, самый маленький`

# H1 — заголовок первого уровня, самый большой

## H2 — заголовок второго уровня, поменьше

### H3

#### H4

##### H5

###### H6 — заголовок шестого уровня, самый маленький

- Можно добавить черту под заголовком или абзацем.

`#### Заголовок 4`

`Текст над чертой`

---

`Текст под чертой`

#### Заголовок 4

Текст над чертой

---

Текст под чертой

- Чтобы сделать разрыв строки, нужно поставить два пробела (в примере ниже они обозначены точками ⋅⋅) или сочетание символов `<br>`.

Текст до переноса⋅⋅  
Текст после переноса `<br>`
Текст после второго переноса

- Чтобы начать новый параграф, в конце предыдущей строки должно стоять два символа переноса. Для этого нужно нажать Enter два раза.

Чтобы начать новый параграф, в конце предыдущей строки должно стоять два символа переноса.`<br><br>`

 Для этого нужно нажать Enter два раза.

line

another line

### Выделение текста

Чтобы выделить текст курсивом (_текст_), его заключают в звёздочки (астериски) или нижние подчёркивания.

`Курсив — это *звёздочки* или _подчёркивания_.`

- Чтобы выделить текст полужирным шрифтом (**текст**), его окружают двойными звёздочками или двойными нижними подчёркиваниями.

`Полужирный шрифт — двойные **звёздочки** или двойные __подчёркивания__.
Можно совместить выделение **звёздочки и _подчёркивания_**.`

- Чтобы зачеркнуть текст (~~текст~~), его окружают двойными волнистыми линиями — тильдами.

`~~Зачёркнутый текст.~~`

~~Зачёркнутый текст.~~

### Списки

- Для оформления нумерованного списка достаточно поставить в начало строки цифры с точкой.

1. Первый пункт нумерованного списка.
2. Второй пункт.

- Ненумерованный список создаётся звёздочкой с пробелом в начале строки либо дефисом с пробелом.

- первый пункт ненумерованного списка;
- второй пункт ненумерованного списка

- первый пункт ненумерованного списка;
- второй пункт ненумерованного списка

### Ссылки

Чтобы сделать ссылкой часть текста, его заключают в квадратные скобки, а затем указывают нужный адрес в круглых скобках.

`[Яндекс](https://www.yandex.ru)`

[Яндекс](https://www.yandex.ru)

- Также можно добавить ссылке тайтл (от англ title — «название», «заголовок»). Тайтл — это всплывающая подсказка, которая появляется при наведении мыши на ссылку. Тайтл нужно заключить в кавычки и указать внутри скобок после адреса.

`[Яндекс](https://www.yandex.ru "Я Yandex!")`

[Яндекс](https://www.yandex.ru "Я Yandex!")

### Код

Чтобы оформить текст как код, нужно окружить его тройками косых кавычек — грависов. После первой тройки грависов указывают язык программирования, на котором написан код. В маркдауне есть поддержка синтаксиса почти всех популярных языков и инструментов.

`bash
ls - la`  

`html`  

`<h1>А я просто текст</h1>`

```bash
ls - la
```

```html
<h1>А я просто текст</h1>
```

Обратите внимание: вторая тройка тройных кавычек стоит на отдельной строке.

### Пример файла README.md

Если собрать всё вместе, файл README.md может выглядеть так.

`# Шпаргалка markdown`

`## Выделение текста`

Вы можете выделять текст в markdown с помощью символов `_` или `*`. Например:

`Пример _курсива_ и **жирного** текста.`

`## Заголовки`

А вот так этот файл будет встречать гостей репозитория.

![Шпаргалка markdown](https://pictures.s3.yandex.net/resources/M2_T4_03_1685964697.png)

**Задание для самостоятельной работы***

Потренируйтесь использовать маркдаун. Оформите файл README.md для репозитория first-project. Сейчас в нём есть файл readme.txt, который вы создали в начале модуля. Удалите файл и создайте новый с расширением .md.

`$ cd ~/dev/first-project`

`$ rm readme.txt`

`$ touch README.md`

`# затем файл README.md можно редактировать как обычно`

`# с помощью любого текстового редактора (например, блокнота)`

Вы можете применять любые инструменты разметки, которые мы показали в этом уроке, или добавить что-нибудь от себя. Загляните в руководства по маркдауну — например, в [шпаргалку на GitHub](https://practicum.yandex.ru/trainer/git-basics/lesson/c6b9607c-e8bc-4446-89f9-c74522c3492f/) или [в этот гайд](https://practicum.yandex.ru/trainer/git-basics/lesson/c6b9607c-e8bc-4446-89f9-c74522c3492f/).

Отлично: теперь вы умеете оформлять файл README.md — он поможет другим пользователям узнать больше о проекте, который вы разместили в вашем репозитории!

