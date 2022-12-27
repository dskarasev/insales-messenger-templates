# Шаблон уведомления для статуса 'В Пути'

В данном шаблоне используется проверка на дату создания заказа. Статус высылается только в том случае, если заказ создан не более 7 дней назад. Это сделано для того, чтобы платформа не рассылала сообщения по старым заказам, в случае изменения их статуса.

За это отвечает следующий код. Это только пример для понимания. Сам шаблон сообщения ниже
```
// Получаем дату создания текушего заказа
{% capture order_date %}
  {{ order.creation_date | date: '%Y%j' }}
{% endcapture %}

// Вычисляем сколько дней прошло с момента заказа
{% capture order_days %}
  {{ 'now' | date: '%Y%j' | minus: order_date }}
{% endcapture %}

// Превращаем string в int для возможности вычисления в условии
{% assign order_days2 = order_days | plus: 0 %}

// Проверяем, что заказ создан не более чем 7 дней назад
{% if order_days2 < 7 %}
... тут код шаблона ...
{% endif %}
```

Замените значение `order.delivery_title == 'Самовывоз из магазина (м. Владыкино)'` на ваше название способа доставки для самовывоза и измените текст с адресом и ссылкой на карту с координатами вашего офиса.

В коде используется проверка и подстановка текста, если заказ доставляется службой Via.Delivery, если она не нужна вы можете убрать эту часть кода.
```
{% if order.delivery_title contains "Via.Delivery" %}
Вы можете отследить посылку по номеру вашего заказа: {{ order.number }} на сайте https://viadelivery.ru/tracking или по телефону +7(495)118-0964
{% endif %}
```


Сообщение в коде представленно с длинными строками, чтобы не было ненужных переносов строк при отправке сообщения клиенту.

## Шаблон сообщения
```
{% capture order_date %}{{ order.creation_date | date: '%Y%j' }}{% endcapture %}{% capture order_days %}{{ 'now' | date: '%Y%j' | minus: order_date }}{% endcapture %}{% assign order_days2 = order_days | plus: 0 %}{% if order_days2 < 7 %}{% if order.delivery_title != "Самовывоз из магазина (м. Владыкино)" %}{% if order.client.name %}{{order.client.name}}, {% endif %}Ваш заказ №{{ order.number }} в интернет-магазине {{account.main_host}} уже в пути! 
		{% if order.delivery_title != "Via.Delivery – пункт выдачи рядом с домом" %}{% if order.delivery_date? %}Ожидаемая дата доставки: {{ order.delivery_date }}{% endif %}
		Способ получения товара: {{order.delivery_title | strip_html}} {{ order.shipping_address.delivery_address }}
		{% endif %}{% if order.delivery_title contains "Via.Delivery" %}
		Вы можете отследить посылку по номеру вашего заказа: {{ order.number }} на сайте https://viadelivery.ru/tracking или по телефону +7(495)118-0964
		{% endif %}{% if order.all_fields['Трекинг номер заказа в службе доставки'].value != '' %}Трекинг-код вашего заказа: {{ order.all_fields['Трекинг номер заказа в службе доставки'].value }}{% endif %}{% endif %}
	Если у вас есть вопросы по заказу или доставке, пожалуйста, позвоните нам по бесплатному номеру 8(800)55-16-163 или напишите в этот чат.
{% endif %}

```
