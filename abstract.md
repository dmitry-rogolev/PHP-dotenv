# Конспект по PHP dotenv

Источник: https://github.com/vlucas/phpdotenv

# Содержание

- [Введение](#введение)
    - [Почему .env?](#почему-env)
    - [Установка](#установка)
- [Использование](#использование)

# Введение

PHP dotenv загружает переменные среды из файла <b>.env</b> в `getenv()`, `$_ENV` и `$_SERVER` автоматически.

## Почему .env?

Сохранение конфигурации в среде является одним из принципов приложения с [12 факторами](https://www.12factor.net/ru/). Все что может изметься в разных средах развертывания, например, учетные данные базы данных или учетные данные для сторонних служб, должно быть извлечено из кода в переменные среды.

`.env` файл &mdash; это простой способ загрузить пользовательские переменные конфигурации, необходимые вашему приложению, без необходимости изменять файлы .htaccess или виртуальные хосты Apache/nginx.

- Нет редактирования виртуальных хостов Apache/nginx;
- Нет добавления `php_value` флагов в файлы .htaccess;
- Простая переносимость и совместное использование требуемых значений ENV;
- Совместим со встроенным веб-сервером PHP и CLI runner.

## Установка

### Composer

    composer require vlucas/phpdotenv

# Использование

Обычно `.env` файл не контролируется системами контроля версий, поскольку он может содержать конфиденциальные ключи API и пароли. Создается отдельный `.env.example` файл со всеми необходимыми переменными среды, определенными, за исключением конфиденциальных, которые либо предоставляются пользователем, либо передаются в другом месте сотрудникам проекта. Затем участники проекта самостоятельно копируют файл `.env.example` в локальный `.env` и проверяют правильность всех настроек для своей локальной среды, заполняя секретные ключи или предоставляя свои собственные значения, когда это необходимо. При таком использовании файл `.env` должен быть добавлен в `.gitignore`, чтобы он никогда не был передан соавторами. Такое использование гарантирует, что никакие конфиденциальные данные никогда не будут в истории контроля версий, поэтому существует меньший риск нарушения безопасности, а производстенные значения никогда не нужно будет передовать всем участникам проекта.

Добавьте конфигурацию вашего проекта в файл `.env` в корне вашего проекта. Убедитесь, что файл `.env` добавлен к вашему `.gitignore`, чтобы он не был отмечен в коде.

    S3_BUCKET="dotenv"
    SECRET_KEY="souper_seekret_key"

Теперь создайте файл `.env.example` и внесите его в проект. Этот файл должен содержать переменные ENV, но значения должны быть либо пустыми, либо заполнены фиктивными данными. Идея состоит в том, чтобы сообщить участникам, какие переменные требуются, но не давать им конкретные значения этих переменных.

    S3_BUCKET="devbucket"
    SECRET_KEY="abc123"

Затем вы можете загрузить `.env` в свое приложение с помощью:

    $dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
    $dotenv->load();

