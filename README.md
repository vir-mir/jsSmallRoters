# jsSmallRoters
Простой Router для **web single page application**

Позволяет обрабатывать  URL и вызывать нужные контролеры в зовисемости от URL

Регулярные вырожения с именованием параметров например
`~^/user/(?P<user_id>\\w+)$~ig` вернеться `{user_id: /* int */}`

**Внимание** - нужно писать `\\w` вместо `\w` так как это строка а не объект **regex**

## Пример использования
```javascript
    RouteCollection = [
			new Route('~^/user/(?P<uesr_id>\\d+)/$~', {
			        header: Header,
			        left: Left,
			        main: MainUserId, // центральный контролер для страницы юзер
			        footer: 'Подвал'
			      }),
		  new Route('~^/user/$~', { // на странице пользователей не нужна левая колонка
			        header: Header,
			        main: MainUser, // центральный контролер для страницы юзеров  
			        footer: Footer
			      }),
		  new Route('~^/posts/(?P<post_ur>[^/]+)/$~', { 
			        header: Header,
			        left: Left,    
			        main: MainPost,    // центральный контролер для страницы поста  
			        right: RightPost, // на странице поста нужен правй блок
			        footer: FooterPost // на странице поста нужен другой подвал
			      }),
		]
		var routers = new Routers(RouteCollection, window.document.location);
		routers.match();
		
		// Ваша фантазия, я использую ReactJs
		// Но здесь для примера обычные объекты
		var header = new routers.getComponent('header');
		var left = new routers.getComponent('left')(/* Если что-то нужно передать в конструктор, например зареган ли Юзер */);
		var main = new routers.getComponent('header');
		var right = new routers.getComponent('header'); // вернет null для всех страниц кроме /posts/post_url/
		var footer = new routers.getComponent('header');
		
```

## Мини  API
```javascript
var routers = new Routers(RouteCollection, window.document.location);

routers.setUrl(url, search, hash); // перезапускает routers.match(); с новым урлом и меняет его в history
// search, hash непереданны, будет пустота на их месте

routers.isRequest() // true или false если нет такого URL

routers.getRequestParamName(name, defaultParam) // параметры пришедшие из URL
// defaultParam непереданны, будет пустота на их месте

routers.getRequestParams() // Все пораметры

routers.getRote() // Вернет текуший {Route} из RouteCollection

```
