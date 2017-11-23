# Встановлення

- [Встановлення](#installation)
    - [Вимоги до сервера](#server-requirements)
    - [Встановлення Laravel](#installing-laravel)
    - [Конфігурація](#configuration)
- [Конфігурація веб-сервера](#web-server-configuration)
    - [Красиві URL-и](#pretty-urls)

<a name="installation"></a>
## Встановлення

> {video} Краще вчити візуально? Ларакасти забезпечують [безкоштовний, ретельний вступ до Laravel](http://laravelfromscratch.com) для ночачків, що тільки прийшли в цей фреймворк. Це чудове місце для початку навчання.

<a name="server-requirements"></a>
### Вимоги до сервера

Фреймворк Laravel має кілька вимог. Звичайно, усі ці вимоги задовільняє [Laravel Homestead](/docs/{{version}}/homestead) віртуальна машина, тому дуже рекомендується використовувати Homestead як локальне середовище для розробки на Laravel.

Однак, якщо ви не використовуєте Homestead, вам потрібно буде переконатися, що ваш сервер відповідає наступним вимогам:

<div class="content-list" markdown="1">
- PHP >= 7.0.0
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
</div>

<a name="installing-laravel"></a>
### Встановлення Laravel

Laravel використовує [Composer](https://getcomposer.org) для керування залежностями. Отже, перш ніж використовувати Laravel, переконайтеся, що на вашому комп'ютері встановлено Composer.

#### Через інсталятор Laravel

Спершу, завантажте інсталятор за допомогою Composer:

    composer global require "laravel/installer"

Перевірте, щоб директорія `composer/vendor/bin` була в вашому `$PATH`. Цей каталог існує в різних місцях на основі вашої операційної системи; однак деякі загальні місця включають в себе:

<div class="content-list" markdown="1">
- MacOS: `$HOME/.composer/vendor/bin`
- GNU / Linux Distributions: `$HOME/.config/composer/vendor/bin`
</div>

Після встановлення, команда `laravel new` виконає свіжу інсталяцію Laravel у вказаній вами директорії. Наприклад, "laravel new blog" створить каталог з ім'ям "blog", що містить нову інсталяціюу Laravel з усіма вже встановленими залежностями Laravel:

    laravel new blog

#### Через Composer Create-Project

Крім того, ви також можете встановити Laravel, виконавши команду Composer `create-project` у своєму терміналі:

    composer create-project --prefer-dist laravel/laravel blog

#### Локальний сервер для розробки

Якщо ви маєте встановлений PHP локально і хочете використовувати вбудований сервер PHP для роботи з вашою програмою, ви можете скористатися Artisan-командою `serve`. Ця команда запустить сервер розробки за адресою `http://localhost: 8000`:

    php artisan serve

Звичайно, більш надійні варіанти локальної розробки доступні через [Homestead](/docs/{{version}}/homestead) та [Valet](/docs/{{version}}/valet).

<a name="configuration"></a>
### Конфігурація

#### Публічна директорія

Після встановлення Laravel, ви повинні налаштувати document / web root вашого веб-сервера до `public` каталогу. Файл `index.php` у цьому каталозі служить фронт-контролером для всіх HTTP-запитів, що надходять у вашу програму.

#### Файли конфігурації

Всі файли конфігурації Laravel зберігаються в каталозі `config`. Кожен параметр документально оформлений, тому не соромтеся переглядати файли та ознайомлюватися з доступними вам параметрами.

#### Права доступу до каталогів

Після встановлення Laravel вам може знадобитися налаштувати деякі дозволи. Каталоги `storage` та `bootstrap/cache` повинні мати право запису вашим веб-сервером, інакше Laravel не буде запускатися. Якщо ви використовуєте віртуальну машину [Homestead](/docs/{{version}}/homestead), ці дозвони вже будуть налаштовані.

#### Ключ програми

Наступне, що потрібно зробити після встановлення Laravel - згенерувати ключ програми. Якщо ви встановили Laravel через Composer або програму встановлення Laravel, цей ключ вже був згенерований для вас за допомогою команди `php artisan key: generate`.

Як правило, цей рядок повинен бути довжиною 32 символів. Ключ можна встановити у файлі оточення `.env`. Якщо ви не перейменували файл `.env.example` на `.env`, ви повинні зробити це зараз. **Якщо ключ програми не встановлений, ваші сеанси користувачів та інші зашифровані дані не будуть захищені!**

#### Додаткова конфігурація

Laravel needs almost no other configuration out of the box. You are free to get started developing! However, you may wish to review the `config/app.php` file and its documentation. It contains several options such as `timezone` and `locale` that you may wish to change according to your application.

Для Laravel майже не потрібна інша конфігурація. Ви можете починати розробку! Однак ви можете переглянути файл `config/app.php` та його документацію. Вона містить декілька параметрів, таких як `timezone` і `locale`, які ви можете змінити відповідно до ваших потреб.

Ви також можете налаштувати кілька додаткових компонентів Laravel, таких як:

<div class="content-list" markdown="1">
- [Cache](/docs/{{version}}/cache#configuration)
- [Database](/docs/{{version}}/database#configuration)
- [Session](/docs/{{version}}/session#configuration)
</div>

<a name="web-server-configuration"></a>
## Конфігурація веб-серверу

<a name="pretty-urls"></a>
### Красиві URL-и

#### Apache

Laravel містить файл `public/.htaccess`, який використовується для надання URL-адрес, що не містять `index.php` на шляху. Перед тим, як обслуговувати Laravel з Apache, обов'язково ввімкніть модуль `mod_rewrite`, щоб файл `.htaccess` був виконаний сервером.

Якщо файл `.htaccess`, який поставляється разом із Laravel, не працює з вашою установкою Apache, спробуйте цю альтернативу:

    Options +FollowSymLinks
    RewriteEngine On

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]

#### Nginx

Якщо ви використовуєте Nginx, наступна директива в конфігурації вашого сайту спрямує всі запити до фронт-контролера `index.php`:

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

Звичайно, використовуючи [Homestead](/docs/{{version}}/homestead) чи [Valet](/docs/{{version}}/valet), красиві URL-и будуть автоматично зконфігуровані.
