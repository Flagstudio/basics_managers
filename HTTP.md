# HTTP

Коды ответа (100, 200, 300, 400, 500)
ЧПУ URL, POST, GET, якоря
Абсолютные и относительные URL
Http/2 и 3

## Структура адреса

```bash
http://blog.example.com:80/catalog/category/knifes?key1=value1&key2=value2#anchor
```

где

- **http://** — протокол
- **blog** — поддомен (субдомен, домен третьего уровня)
- **example** — домен (домен второго уровня)
- **com** — доменная зона (домен первого уровня)
- **80** — порт. для http 80, для https 443
- **/catalog/category/knifes** — ЧПУ-адрес (человеко-понятный URL)
- **key1=value1&key2=value2** — GET параметры запроса
- **anchor** — якорь

## Абсолютные и относительные URL

Абсолютные адреса в проекте - зло. Используйте относительные.

### Абсолютный URL

```bash
https://developer.mozilla.org/ru/docs/Learn
```

### Абсолютный URL, скрыт протокол

```bash
//developer.mozilla.org/ru/docs/Learn
```

### Относительный URL (неправильно) 🚫

```bash
ru/docs/Learn
```

если мы на странице https://domain.ru/ru/about, то преобразуется в https://domain.ru/ru/aboutru/docs/Learn

### Относительный URL (правильно) ✅

```bash
/ru/docs/Learn
```

если мы на странице https://domain.ru/ru/about, то преобразуется в https://domain.ru/ru/docs/Learn


## Якоря (анкоры) ⚓️

Якоря придуманы в HTML, чтобы давать ссылки не просто на HTML страницу, а на определенное место на странице. Позже якоря стали активно использоваться для передачи параметров в JS.

URL с якорем

```bash
https://domain.ru/url#someAnchor
```

HTML-элемент, на который ведет якорь

```html
<h2 name="someAnchor">Заголовок с якорем</h2>
```


## Заголовки HTTP

Заголовки — часть HTTP-запроса и/или HTTP-ответа, невидимые пользователю. Представляют собой пары ключ:значение. Заголовки можно посмотреть в браузере на вкладке Network или получить командой `curl -I domain.ru` (так можно получить только заголовки **ответа** сервера).

### Заголовки запроса

- **Host** — ВАЖНО. домен, по которому мы переходим
- **Cookie** — куки, отдаваемые браузером серверу
- **User-Agent** — браузер, которым делаем запрос
- **Accept-Language** и **Accept-Encoding** — принимаемые браузером язык и кодировка
- **Referer** — предыдущая страниа
- **Authorization** — реквизиты для базовой авторизации (логин пароль)

### Заголовки ответа

- **Cache-Control** — сервер говорит браузеру как кэшировать данные
- **Content-Type** — тип и подтип содержимого, а также кодировка и приложение для открытия содержимого. Примеры:
  - **Content-Type: text/html; charset=UTF-8** — Тип текст, подтип HTML. Кодировка UTF-8
  - **Content-Type: image/gif** — Тип картинка, подтип GIF
  - **Content-Type: application/pdf** — Тип "открываемый внешним приложением", подтип PDF
- **Content-Disposition** — говорит браузеру скачивать или открывать документ как веб-страницу
- **Location** — заголовок для редиректа
- **Set-Cookie** - сервер передает браузеру куку
- **WWW-Authenticate** — выводит окно с базовой аутентификацией
- **Content-Encoding** — сервер говорит браузеру сжата ли страница и в каком виде
- **Server** и **X-Powered-By** — технологии, на которых работает сайт


## Методы HTTP-запросов

Вся разница: GET **не содержит** параметров в теле запроса. POST **содержит** параметры в теле запроса. Причем, все запросы могут иметь **что угодно** в URL, это не имеет значения.

Важно понимать, технически все HTTP запросы работают одинаково. Типы запросов - синтаксический сахар для удобства разработки.

- **GET** — не передает body, получает информацию о ресурсе
- **POST** - передает что-то body, создает ресурс
- **PUT** — передает что-то body, _method=put, заменяет ресурс целиком
- **PATCH** — передает что-то body, _method=patch, редактирует ресурс
- **DELETE** — передает что-то body, _method=delete, удаляет ресурс
- **HEAD** — не передает body. не получает body. только отправляет и получает HTTP-заголовки

## Статус-коды ответа

### По группам

- **100 - 199** Информационные
- **200 - 299** Успешные
- **300 - 399** Редиректы
- **400 - 499** Клиентские ошибки
- **500 - 599** Серверные ошибки

### Популярные коды ответа

- Для редиректов SEO-шники рекомендуют **301** редиректы.
- **401** не авторизован (юзер не представился)
- **403** запрещено (юзер не уполномочен)
- **404** ресурс не найден
- **418** я чайник
- **419** кука протухла (обычно ошибка проверки CSRF)
- **502** Bad Gateway. Обычно веб-сервер

Коды ответа, как и заголовки, можно посмотреть в браузере на вкладке Network или получить командой `curl -I domain.ru`. В отличие от заголовков, коды ответа бывают только в ответе и не используются в запросе.

## HTTP-редиректы

Проверка редиректов очень важна для проверки настроек префиксов домена типа `www` и `http`/`https`.

То есть, у каждого домена обычно есть 4 варианта написания:

- http://domain.ru
- http://www.domain.ru
- https://domain.ru (обычно, основной вариант)
- https://www.domain.ru

И 3 из этих 4 вариантов **обязательно** должны ссылаться на один из них, то есть на главный. Главный вариант определяет заказчик, но обычно это https://domain.ru

Редиректы можно проверить браузером через вкладку Network

![](/img/redirects_devtools.png)


Разработчикам будет удобнее воспользоваться утилитой

```bash
curl -I -L https://domain.ru
```


#### Как сбросить кэш редиректов браузера

На Windows нажать ctrl+H, затем "очистить историю", оставить включенной (отжатой) только галочку напротив "Изображения и другие файлы, сохраненные в кэше". Затем нажать "удалить данные".

---

## SSL

![](/img/ssl.png)

Принцип — передача только зашифрованных данных.

Типы SSL-сертификатов

- Платные (Comodo, Thawte, reg.ru)
  - Срок — 1 год или более
  - Стоимость — от 1500 руб/год
- Let's Encrypt (обычно, мы используем именно LE)
  - Срок — 3 месяца
  - Стоимость — бесплатно
  - Нюанс: Wild Card сертификаты для поддоменов (\*.flagstudio.ru). Доступны в LE только при DNS-аутентификации, то есть добавлении TXT-записи, которую нужно обновлять каждые 3 месяца. Для обновления используется API, которое есть **не у всех DNS-провайдеров**.


### Диагностика SSL


Если на сайте проблемы с SSL, то это одна из двух проблем
- Что-то не так с сертификатом (кончился, выдан не на этот домен или самоподписанный)
- Страница запрашивает что-то по HTTP

На скриншоте видно, как посмотреть подробности о сертификате и наличие ошибок в странице. Конкретно на этом скриншоте показана вторая проблема, так называемая "mixed-content error", то есть часть контента прилетает по HTTP.

![](/img/ssl-mixed-error.png)

Для диагности SSL на вашем сайте вы также можете использовать онлайн-сервис [https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)

Учтите, данные о сертификате, как и HTTP-редиректы, сильно кэшируются браузером. Сбрасываются также, как кэш редиректов (смотрите выше).


## HTTP/2

Суть - мультиплексирование запросов по TCP.

Преимущества перед HTTP/1.1

— Скорость. HTTP/2 — бинарный протокол, а HTTP/1.1 — текстовый
- Скорость. Мультиплексирования множества запросов в одном соединении TCP
- Скорость. Сжатие HTTP-заголовков
- Server Push. Принудительная отправка данных сервером в браузер
- Приоритеты запросов

Минусы

- Из-за "head-of-line blocking" на медленном интернете HTTP/1.1 работает быстрее
- Для работы HTTP/2 нужен SSL

Распространение: По данным W3Techs на 1 ноября 2020 года, 49 % из 10 млн самых популярных интернет-сайтов поддерживают протокол HTTP/2.

Подробая статья с понятными картинками https://habr.com/ru/company/nix/blog/304518/

## HTTP/3

Суть — мультиплексирование по UDP.

Преимущества перед HTTP/2:

- Решена проблема "head-of-line blocking"
- Шифрование и транспорт объединены, пакеты шифруются независимо
- Протокол реализуется на уровне приложений, а не ОС. Поэтому быстро внедряется

Минусы:

- Протокол реализуется на уровне приложений, а не ОС. Поэтому HTTP/3 использует больше CPU

Результаты: По данным Google, страницы загружаются примерно на 5% быстрее, а в потоковом видео на 30% меньше подвисаний по сравнению с TCP.

Распространение: По данным W3Techs на 1 сентября 2020 года, 7 % из 10 млн самых популярных интернет-сайтов поддерживают протокол HTTP/3.

Вот хорошая статья https://habr.com/ru/company/dododev/blog/473930/