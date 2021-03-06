#MCLite

MCLite бесплатный это модуль для OpenCart, который предназначен для автоматического сжатия и объединения CSS файлов. 

##Как работает MCLite

Перед тем как послать страницу клиенту он сканирует ее код, находя в нем все CSS файлы, и обрабатывает их в соответствии с настройками, а настроек этих целая куча (как и библиотек для минимизации). После обработки он сохраняет новые (сжатые) CSS файлы во временную папку. Затем подменяет ссылки на минимизированные CSS файлы в коде страницы. Таким образом оригинальные CSS файлы остаются нетронутыми. После этого отправкой файлов стилей для страницы занимается сервер, а не PHP. Сжимаются и передаются только те стили, которые используются на посещаемой странице.

##Возможности MCLite
- Наличие нескольких библиотек для минимизации CSS
- Возможность вставлять в файлы стилей изображения в кодировке base64, вне зависимости от выбранного минимизатора (преимущества)
- Упаковка файлов стилей алгоритмом gzip во время создания файла. Т.е. не будет тратится время на сжатие "на лету"
- Возможность использовать Domain CDN для отдачи CSS и изображений
- Имеется функция оптимизации базы данных
- Несколько режимов объединения файлов
- Возможность исключить определенные файлы из обработки/объединения/минимизации
- Имеется функция сжатия HTML (на лету, без кэширования)
- Наличие нескольких библиотек для сжатия HTML
- Минимум файлов
- Удобный менеджер кэша со статистикой сжатия
- Наличие деинсталлятора (удаляет вместе с файлами и записями модуля)

##Ресурсы

В работе MCLite используются проекты с открытым исходным кодом, такие как
* [CSSMin] Joe Scylla
* [CSSMin Regex] Shashakhmetov Talgat
* [YUI CSS Compressor php port] Tubal Martin
* [CanCSSMini] andi
* [Crunch CSS] Shirley Kaiser
* [Minify HTML] Stephen Clay
* [HTMLMin Regex] Shashakhmetov Talgat
* [Crunch HTML] Shirley Kaiser

##Поддерживаемые версии

Все версии, начиная от 1.5.1 до 1.5.6.4 включительно.

##Установка

###Шаг 1. Копирование файлов

Скопировать содержимое папки "upload" в корневую директорию сайта.


###Шаг 2. Редактирование файлов

В файле "index.php" перед строкой $response-&gt;output(); (в самом конце файла)
добавить:
```php
//MCLite
if (preg_match('/head>/im', $response->output) && !defined('DIR_CATALOG')) {
    $loader->library('mclite/mclite.class');
    $mclite = new mclite($registry, $response->output);
    $response->output = $mclite->output;
}
```
В файле "system/library/response.php" строку 
```php
private $output; 
```
заменить на 
```php
public $output;
```

###Шаг 3. Добавление записей в базу данных

Скопировать в корневую директорию файл "install.php". Выполнить его через адресную строку браузера. Например, http://адрес-сайта/install.php. Он добавит новые значения в базу данных.
После выполнения обязательно удалить файл!!!


###Шаг 4. Настройка привилегий в OpenCart

В административной панели сайта перейти в "Система-&gt;Пользователи-&gt;Группы пользователей". Напротив строки "Главный администратор" нажать "изменить".
В списках "Разрешен просмотр" и "Разрешено внесение изменений" поставить галочку напротив "mcj/setting", либо нажать "Выделить все". Сохранить.


###Шаг 5. Настройка .htaccess

Для использования статического сжатия файлов в файл .htaccess необходимо добавить:
```
AddType text/css .css .cssgz
AddEncoding x-gzip .cssgz
```
[CSSMin]:https://github.com/natxet/CssMin
[CSSMin Regex]:http://halfhope.ru/
[YUI CSS Compressor php port]:https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port
[CanCSSMini]:http://google.com/
[Crunch CSS]:http://websitetips.com/articles/optimization/css/crunch/
[Minify HTML]:https://github.com/mrclay/minify
[HTMLMin Regex]:http://halfhope.ru/
[Crunch HTML]:http://websitetips.com/articles/optimization/html/crunch/
[jQuery]:http://jquery.com
