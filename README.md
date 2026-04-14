# kuma_pomogator

English version below

Сервис на python с веб-интерфейсом, который позволяет выполнять некоторые операции с KUMA с помощью интерфейса через public REST API

# Требования

Для корректной работы необходимо установить зависимости

```
pip install requirements.txt
```

или

```
python -m pip install requirements.txt
```

# Быстрый старт

Для запуска программы перейдите в папку со скриптом и выполните команду

```
python main.py
```

Веб-интерфейс будет доступен по адресу `http://127.0.0.1:7860`

Для переопредления адреса, порта или протокола консоли используйте переменные окружения. Подробности - в документации [gradio](https://www.gradio.app/guides/environment-variables).

# Работа с программой

Для начала работы в верхнем окне интерфейса укажите адрес, api-порт и токен для ядра KUMA и нажмите кнопку Connect. Если все введенные данные были верны, под кнопкой подключения отобразится Status: Connected и отобразятся основные вкладки работы с программой.

## Вкладка Export

На данной вкладке можно экспортировать алерты, инциденты и правила корреляции в формате CSV.

Поля фильтрации алертов и инцидентов по таймстемпу не являются обязательными.

Необходимые права экспорта алертов:

- GET /alerts

Необходимые права для экспорта инцидентов:

- GET /incidents

Необходимые права для экспорта правил корреляции:

- GET /tenants
- GET /services
- GET /services/:kind/:id
- GET /resources

## Вкладка Assets

На данной вкладке можно импортировать активы в формате CSV. Ожидаемый формат приведен в веб-интерфейсе.

Если то или иное поле отсутствует для актива, оставьте его пустым.

Если то или иное поле содержит запятую внутри, возьмите значение поле в двойные кавычки.

Сервис не проверяет корректность введенных данных (IP, FQDN, MAC). При ошибках в этих полях хотя бы для одного актива не будет импортирован весь список, при этом вернется всплывающая ошибка с указанием на актив.

Также для импорта актива необходимо указать тенант, в который активы будут импортированы.

Необходимые права для импорта активов:

- GET /tenants
- POST /assets/import

## Вкладка Backup/Restore

На данной вкладке можно скачать бэкап архива ядра, а также загрузить бэкап и запустить процедуру восстановления.

Т.к. бэкап при больших объемах базы занимает некоторое время - наберитесь терпения и не закрывайте вкладку (с самой вкладки можно уйти и при возвращении бэкап будет загружен).

Необходимые права для создания бэкапа:

- GET /system/backup

Необходимые права для восстановления бэкапа:

- POST /system/restore

## Вкладка Analyzer

На этой вкладке вы можете проанализировать любой ресурс KUMA в формате JSON.

Обязательно укажите тип ресурса для поиска. Имя ресурса опционально, но его указание сократит время поиска. Также обратите внимание, что имя ресурса - регулярное выражение.

После того как ресурсы будут найдены выберите в выпадающем меню интересующий ресурс и нажмите кнопку ниже - вы увидите ресурс в формате JSON.

Для копирования ресурса нажмите на значок копирования в правом верхнем углу окна с JSON.

Необходимые права для просмотра ресурсов:

- GET /resources

# Известные ограничения

1. Сервис не сохраняет свое состояние - при перезагрузке вкладки все введенные данные нужно будет вводить заново, в т.ч. адрес и токен и выполнять подключение.
2. При выборе большого ресурса для анализа (например, парсер Cisco) могут наблюдаться зависания интерфейса. В таком случае рекомендуется скопировать ресурс и проанализировать его, например, в notepad++.
3. Все файлы для скачивания имеют рандомное название, что связано с использованием tempfile.

---

# English version

# kuma_pomogator

A Python service with a web interface that allows you to perform various operations with KUMA via the public REST API.

# Requirements

Install the required dependencies:

```
pip install requirements.txt
```

or

```
python -m pip install requirements.txt
```

# Quick start

Navigate to the script directory and run:

```
python main-en.py
```

The web interface will be available at `http://127.0.0.1:7860`

To override the address, port, or server protocol, use environment variables. See the [gradio](https://www.gradio.app/guides/environment-variables) documentation for details.

# Usage

To get started, enter the address, API port, and token for the KUMA core in the top section of the interface and click Connect. If the provided credentials are correct, Status: Connected will appear below the button and the main tabs will become visible.

## Export tab

This tab allows you to export alerts, incidents, and correlation rules in CSV format.

Timestamp filter fields for alerts and incidents are optional.

Required permissions for alert export:

- GET /alerts

Required permissions for incident export:

- GET /incidents

Required permissions for correlation rules export:

- GET /tenants
- GET /services
- GET /services/:kind/:id
- GET /resources

## Assets tab

This tab allows you to import assets in CSV format. The expected format is shown in the web interface.

If a field is not applicable for an asset, leave it empty.

If a field value contains a comma, wrap the value in double quotes.

The service does not validate the correctness of the provided data (IP, FQDN, MAC). If any of these fields contain an error for even a single asset, the entire list will not be imported, and a popup error will appear indicating the problematic asset.

You must also specify the tenant into which the assets will be imported.

Required permissions for asset import:

- GET /tenants
- POST /assets/import

## Backup/Restore tab

This tab allows you to download a backup of the core archive, as well as upload a backup and start the restore procedure.

Since creating a backup may take some time for large databases — please be patient and do not close the browser tab (you may navigate away from the tab and the backup will still be available when you return).

Required permissions for backup creation:

- GET /system/backup

Required permissions for backup restoration:

- POST /system/restore

## Analyzer tab

This tab allows you to analyze any KUMA resource in JSON format.

The resource type is required. The resource name is optional, but specifying it will reduce search time. Note that the resource name is treated as a regular expression.

Once the resources are found, select the one you are interested in from the dropdown menu and click the button below — the resource will be displayed in JSON format.

To copy the resource, click the copy icon in the top right corner of the JSON window.

Required permissions for resource viewing:

- GET /resources

# Known limitations

1. The service does not preserve its state — all entered data, including the address, token, and connection, must be re-entered after refreshing the page.
2. When selecting a large resource for analysis (e.g., the Cisco parser), the interface may become unresponsive. In this case, it is recommended to copy the resource and analyze it in an external tool such as Notepad++.
3. All downloadable files have random names due to the use of tempfile.
