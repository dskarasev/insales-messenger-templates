# Шаблон уведомления для статуса 'Готов к выдаче на ПВЗ'

В данном шаблоне также используется проверка на дату создания заказа. Статус высылается только в том случае, если заказ создан не более 7 дней назад. Это сделано для того, чтобы платформа не рассылала сообщения по старым заказам, в случае изменения их статуса.

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

Замените название поля в которое вы записывыаете трекинг-код служб доставки `{{ order.all_fields['Трекинг номер заказа в службе доставки'].value }}`

Сообщение в коде представленно с длинными строками, чтобы не было ненужных переносов строк при отправке сообщения клиенту.

## Шаблон сообщения
```
{% capture order_date %}{{ order.creation_date | date: '%Y%j' }}{% endcapture %}{% capture order_days %}{{ 'now' | date: '%Y%j' | minus: order_date }}{% endcapture %}{% assign order_days2 = order_days | plus: 0 %}{% if order_days2 < 12 %}Ваш заказ №{{ order.number}} в интернет-магазине {{ account.main_host }} готов к получению в пункте выдачи заказов{% if order.shipping_address.delivery_address %} по адресу: {{ order.shipping_address.delivery_address}}. {% endif %}{% if order.delivery_info.outlet.description %}{{ order.delivery_info.outlet.description | strip_html }} {% else %}.{% endif %}{% if order.all_fields['Трекинг номер заказа в службе доставки'].value != "" %}
Трекинг-код вашего заказа: {{ order.all_fields['Трекинг номер заказа в службе доставки'].value }}
{% endif %}
{% endif %}

```
