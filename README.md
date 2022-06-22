# API Gateway SDK для подписи запросов на JavaScript

Для вызова API опубликованных c режимом аутентификации APP, запрос должен быть подписан при помощи APP_KEY и SECRET_KEY.
Для подписи запросов используются API Gateay SDK на одном из поддерживаемых языков программирования.

## Пререквизиты

* NodeJS 16.x и выше;
* npm;
* Модули `moment` и `moment-timezone`.

## Подключение SDK к проекту

1. Установите SDK в проект.
    ```shell
	npm install apigateway-sdk-nodejs
    ```

2. Установите модули `moment` и `moment-timezone`, если они еще не установлены.
	```shell
	npm install moment --save
	npm install moment-timezone --save
    ```

## Пример использования SDK

1. Импортируйте в проект модули `apigateway-sdk-nodejs` и `https`.
	```js
	var signer = require('apigateway-sdk-nodejs')
	var https = require('https')
    ```

2. Создайте новый signer и передайте в него `APP_KEY` и `SECRET_KEY`.
    ```js
	var sig = new signer.Signer()
	sig.Key = "key"
	sig.Secret = "secret"
    ```

3. Создайте объект запроса, в котором укажите метод, URI, заголовки и тело запроса.
    ```js
	var r = new signer.HttpRequest("POST", "https://my-domain.example.com/v1/test");
	r.body = '{"a":1}'
	r.headers = { "x-stage":"RELEASE" }
    ```
  
4. Вызовите функцию для подписывания запроса. Функция автоматически добавит заголовки `X-Sdk-Date` и `Authorization`.
    ```js
	var opt = sig.Sign(r)
    ```

5. Обратитесь к API и просмотрите ответ.
    ```js
	var req = https.request(opt, function(res){
		console.log(res.statusCode)  
		res.on("data",  function(chunk){
			console.log(chunk.toString())
		})
	})
	req.on("error",function(err){
		console.log(err.message)
	})
	req.write(r.body)
	req.end()
    ```
