# Конфігурація

- [Вступ](#introduction)
- [Конфігурація середовища](#environment-configuration)
    - [Отримання конфігурації середовища](#retrieving-environment-configuration)
    - [Визначення поточного середовища](#determining-the-current-environment)
- [Доступ до значень конфігурації](#accessing-configuration-values)
- [Кешування конфігурації](#configuration-caching)
- [Режим технічного обслуговування](#maintenance-mode)

<a name="introduction"></a>
## Вступ

Всі файли конфігурації Laravel зберігаються в каталозі `config`. Кожен параметр документально оформлений, тому не соромтеся переглядати файли та ознайомитися з доступними вам параметрами.

<a name="environment-configuration"></a>
## Конфігурація середовища

Іноді корисно мати різні значення конфігурації на основі середовища, де працює програма. Наприклад, ви можете використати інший драйвер кешу локально, ніж на продакшен-сервері.

Для цього Laravel використовує PHP бібліотеку [DotEnv](https://github.com/vlucas/phpdotenv) створену Vance Lucas. Після свіжої інсталяції Laravel, у вашому головному каталогу ви знайдете файл `.env.example`. Якщо ви встановили Laravel через Composer, цей файл буде автоматично переіменований в `.env`. В іншому випадку, вам необхідно перейменувати файл вручну.

Ваш файл `.env` повинен бути прихований від вашої програми контролю версій, оскільки кожен розробник / сервер може вимагати іншої конфігурації середовища. Крім того, це може бути небезпечно, у випадку якщо зловмисник отримає доступ до вашого сховища програми контролю версій, оскільки будь які конфіденційні дані будуть виявлені.

Якщо ви працюєте в команді, ви можете залишити файл `.env.example` у вашій програмі. Поширюючи плейсхолдери в цьому файлі, інші розробники у вашій команді можуть чітко бачити які змінні середовища необхідні для запуску вашої програми. Ви також можете створити файл `.env.testing`. Цей файл замінить файл `.env` коли будуть запущені тести PHPUnit або запущена Artisan-команда з параметром `--env=testing`.

> {tip} Будь-яка змінна у вашому файлі `.env` може бути перевизначена за допомогою змінних зовнішнього середовища, таких як змінні середовища на рівні сервера або системного рівня.

<a name="retrieving-environment-configuration"></a>
### Отримання конфігурації середовища

Усі змінні, перелічені в цьому файлі, будуть завантажені в PHP супер-глобальну змінну `$_ENV`. Крім цього, ви можете використовувати хелпер `env` для отримання значень змінних у ваших конфігураційних файлах. Справді, якщо ви переглянете файли конфігурації Laravel, ви помітите декілька варіантів, які вже використовують цей хелпер.

    'debug' => env('APP_DEBUG', false),

Другий аргумент, переданий в функцію `env`, є "значення за замовчуванням". Це значення буде використано, якщо для даного ключа не існує змінної середовища.

<a name="determining-the-current-environment"></a>
### Determining The Current Environment

The current application environment is determined via the `APP_ENV` variable from your `.env` file. You may access this value via the `environment` method on the `App` [facade](/docs/{{version}}/facades):

    $environment = App::environment();

You may also pass arguments to the `environment` method to check if the environment matches a given value. The method will return `true` if the environment matches any of the given values:

    if (App::environment('local')) {
        // The environment is local
    }

    if (App::environment(['local', 'staging'])) {
        // The environment is either local OR staging...
    }

> {tip} The current application environment detection can be overridden by a server-level `APP_ENV` environment variable. This can be useful when you need to share the same application for different environment configurations, so you can set up a given host to match a given environment in your server's configurations.

<a name="accessing-configuration-values"></a>
## Accessing Configuration Values

You may easily access your configuration values using the global `config` helper function from anywhere in your application. The configuration values may be accessed using "dot" syntax, which includes the name of the file and option you wish to access. A default value may also be specified and will be returned if the configuration option does not exist:

    $value = config('app.timezone');

To set configuration values at runtime, pass an array to the `config` helper:

    config(['app.timezone' => 'America/Chicago']);

<a name="configuration-caching"></a>
## Configuration Caching

To give your application a speed boost, you should cache all of your configuration files into a single file using the `config:cache` Artisan command. This will combine all of the configuration options for your application into a single file which will be loaded quickly by the framework.

You should typically run the `php artisan config:cache` command as part of your production deployment routine. The command should not be run during local development as configuration options will frequently need to be changed during the course of your application's development.

> {note} If you execute the `config:cache` command during your deployment process, you should be sure that you are only calling the `env` function from within your configuration files.

<a name="maintenance-mode"></a>
## Maintenance Mode

When your application is in maintenance mode, a custom view will be displayed for all requests into your application. This makes it easy to "disable" your application while it is updating or when you are performing maintenance. A maintenance mode check is included in the default middleware stack for your application. If the application is in maintenance mode, a `MaintenanceModeException` will be thrown with a status code of 503.

To enable maintenance mode, simply execute the `down` Artisan command:

    php artisan down

You may also provide `message` and `retry` options to the `down` command. The `message` value may be used to display or log a custom message, while the `retry` value will be set as the `Retry-After` HTTP header's value:

    php artisan down --message="Upgrading Database" --retry=60

To disable maintenance mode, use the `up` command:

    php artisan up

> {tip} You may customize the default maintenance mode template by defining your own template at `resources/views/errors/503.blade.php`.

#### Maintenance Mode & Queues

While your application is in maintenance mode, no [queued jobs](/docs/{{version}}/queues) will be handled. The jobs will continue to be handled as normal once the application is out of maintenance mode.

#### Alternatives To Maintenance Mode

Since maintenance mode requires your application to have several seconds of downtime, consider alternatives like [Envoyer](https://envoyer.io) to accomplish zero-downtime deployment with Laravel.
