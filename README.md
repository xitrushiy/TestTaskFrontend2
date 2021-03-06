# Задание
<pre>
Исходные данные:
На сервере хранится информация о видеороликах двух типов: фильмы и музыка.
Эту информацию можно получать через API, указывая номер страницы с данными.
Одна страница может содержать ролики обоих типов и не более 10 штук.

Пример запроса: http://site.ru/sample-feed/?page=3
Сервер возвращает данные в формате JSON со следующей структурой:
{
	"page": 2,
	"lastPage": 3,
	"content": [
		{
			"id": "1",
			"type": "movie",
			"category": "трейлер",
			"pageUrl": "sample-url",
			"name": "Железный Человек 3 / Iron Man 3"
		},
		{
			"id": "2",
			"type": "music",
			"genre": "rock",
			"pageUrl": "sample-url",
			"name": "Limp Bizkit ­ Behind Blue Eyes"
		},
		...
	]
}

Описание полей страницы:
page ­ [integer] номер страницы
lastPage ­ [integer] номер последней страницы
content ­ [array] массив с данными, которые необходимо отобразить на странице

Описание полей ролика:
id ­ [string] уникальный id ролика
type ­ [string] тип контента (movie/music)
category ­ [string] категория (поле уникально для роликов типа "movie")
genre ­ [string] жанр (поле уникально для роликов типа "music")
pageUrl ­ [string] адрес страницы ролика http://site.ru/[pageUrl]
name ­ [string] название ролика

Задача:
Необходимо сделать страницу, на которой ролики выводятся в виде списка ссылок. Рядом с каждой ссылкой выводится поле category/genre, в зависимости от типа ролика. При загрузке страницы запрашивается и показывается первая порция данных.
Далее по выбору:
1) ЛИБО на странице имеется кнопка "Загрузить еще", по клику на которой загружается следующая страница с данными. Если данные закончились, кнопка "Загрузить еще" должна быть скрыта. Также реализовать возможность фильтрации загруженных данных на клиенте по названию (поле name)
2) ЛИБО прикрутить какой-либо плагин постраничной навигации, в этом случае список должен отображаться постранично (на странице 10 роликов)

Реализовать на mvvm библиотеке по выбору - vue/knockout/react/angular. Для отображения списка НЕ использовать плагины таблиц, а только встроенный шаблонизатор из mvvm решения. Учесть обработку ошибок. Серверные запросы эмулировать через https://github.com/jakerella/jquery-mockjax Для аккуратного вида можно прикрутить bootstrap.
Результат присылать в виде ссылки на github репозиторий с исходным кодом тестового задания и инструкции, как запускать.
</pre>
# Запуск
1. Подтянуть зависимости
2. Для симуляции API REST понадобится https://github.com/typicode/json-server
3. Запустить: >json-server db.json --port 3030 из папки components/DbMockup
(порт может быть другой, но тогда подправьте строчку в компоненте SampleCardList)
4. npm start
5. Наслаждаться этим замечательным функционалом из одной кнопки и строки поиска

# Решение
## Список использованных зависимостей
1. axios - для запросов к бд
2. material-ui - для эстетики
3. json-server - для симуляции API REST из json файлика, в кототором наша "база данных"

### Общий комментарий
Честно говоря, я отклонился от задания местами.
Местами наоборот придал ему некоторого смысла, на мой взгляд.

Чтоб понять логику того, что было создано - представьте, что у нас имеется ~~пиратский~~ лицензионный сервер
с музычкой и фильмами, которые можно скачивать по ссылочке.

На решение ушло 16 часов из которых:
1. 1.5 часа на понимание задачи и анализ mockjax
2. 7 часов непосредственно на код
3. 4 часа на украшательства
4. 1 час на рефакторинг и подчистку
5. 1 час на написание Readme
6. 1.5 часа нашел баги

Верстал grid-ами.

### Отклонения от задания
1. Я посмотрел mockjax, понял идею примерно понял как использовать. Как прикрутить его к React - не понял. Он тут смотрится не органично.
Вместо него для симуляции сервера был использован json-server.
2. Сервер возвращает данные чуть-чуть проще, но смысл тот же. Мою логику можно увидеть в методе SampleCardList -> loadData().
3. Фильтрацию данных сделал не только по имени, но и по типу/жанру (Я как пользователь сервисов иногда музыку и фильмы по жанру ищу)
4. Использовал штуку покруче бутстрапа для красивости. Надеюсь, не осудите :)

### Дополнительные свистульки
1. Оформление
2. Если данные в базе закончатся и выгружать больше нечего - кнопочка с загрузкой исчезает :)

В итоге должно выглядеть примерно так, как в файлике screen.png в корне. Если не так, то тут либо я косяк, либо вы :)