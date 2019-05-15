# Тестовое задание
Реализовать простое REST API с одним единственным методом, который загружает изображения.

Требования:
- Возможность загружать несколько файлов.
- Возможность принимать multipart/form-data запросы.
- Возможность принимать JSON запросы с BASE64 закодированными изображениями.
- Возможность загружать изображения по заданному URL (изображение размещено где-то в интернете).
- Создание квадратного превью изображения размером 100px на 100px.
- Наличие модульных/интеграционных тестов.

Временем и инструментом для выполнение тестового задания Вы не ограничены. Любые другие аспекты реализации, которые не указаны в требованиях, могут быть выполнены на Ваше усмотрение.

Следующее будет плюсом:
- Корректное завершение приложения при получении сигнала ОС (graceful shutdown).
- Dockerfile и docker-compose.yml, которые позволяют поднять приложение единой docker-compose up командой.
- CI интеграция (Travis CI, Circle CI, другие).

Тестовое задание должно быть предоставлено в виде ссылки на публичный репозиторий (GitHub, BitBucket, GitLab), содержащий исходный код приложения и необходимые инструкции по сборке и запуску.
 
# API
Прием multipart/form-data запросов необходимо направлять в POST localhost/upload. При этом требуются следующие параметры:
* name - название сохраняемого файла.
* data - тело файла.

Прием JSON запросов следует направлять в POST localhost/upload. При этом тело запроса должно иметь следующий вид:
{
  "name": "dog.jpg",
  "data": "data in base64 format"
}

Для загрузки файла из Интернета, необходимо направлять в POST localhost/upload. При этом требуются следующие параметры:
* name - название сохраняемого файла.
* url - ссылка на файл.

# Комментарии
Приложение реализовано на языке Golang, поскольку в задании язык явно оговорен не был, а практического опыта в языке Rust у меня на текущий момент недостаточно. 

Приложение реализовано с использованием фреймвока github.com/adverax/echo, написанного мной и являющегося веткой фрейворка github.com/labstack/echo.

Приложение поддерживает технологию graceful shutdown. Данная возможность реализуется внутри фрейворка adverax/echo.

В тестовом задании явно не указано, что требуется загружать несколько файлов в ОДНОМ запросе. Возможность загрузки нескольких файлов реализуется путем нескольких последовательных запросов.

Загружаемые изображения хранятся в каталогах:
* static/images - содержит полные изображения.
* static/thumbnails - содержит миниатюры.

Для проверки работоспособности методов использовано модульное и интеграционное тестирование. Функциональное тестирование в задании не применялось, поскольку такое требование не выдвигалось.

Для упрощения проверки, я поместил в каталог уже скомпилированный бинарный файл. 

# Инструкция по проверке
Для загрузки требуемых библиотек выполните команды:
```bush
go get github.com/adverax/echo
go get github.com/nfnt/resize
go get github.com/oliamb/cutter
```
 
Для компиляции выполните команду
```bush 
go build
```    

Для запуска сервера выполните команду
```bush 
./repo
```
     
Сервер работает по следующему адресу: localhost:8080. Запросы к нему можно слать через wget. Например:
```bush
curl -X POST -d 'name=test.jpg&url=https://www.infoniac.ru/upload/medialibrary/409/4099172ff56fa1e8d0a35b52bf908726.jpg' http://localhost:8080/upload
```