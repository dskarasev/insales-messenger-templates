# Шаблон уведомления для статуса 'Новый'

Скопируйте код и вставьте его в поле сообщения для мессенджеров. Используются символы форматирования текста WhatsApp

```
Заказ на сайте {{ account.title }} {{shop_url | remove_first: "https://"}} - №{{ order.number }} от {{ order.creation_date | date: "%d.%m.%Y" }}
Клиент: {{ order.client.name }} {{ order.client.phone}}

*Состав заказа:*
{% for item in order.items %}- {{ item.title | strip_html }}. {{item.sale_price.full | money}} x {{item.quantity}} {{item.product.unit}}
{% endfor %}- Доставка: {{ order.delivery_price | money }}{% if order.discounts.size > 0 %}{% for discount in order.discounts %}
- Cкидка: {{ discount.amount | money }}{% endfor %}{% endif %}

*Общая сумма заказа{% unless order.delivery_title contains 'Самовывоз' %} с доставкой{% endunless %}: {{ order.total_price | money | remove: '&nbsp;'}}*

{% if order.delivery_title contains 'Самовывоз' %}Наш адрес: г. Москва Алтуфьевское шоссе 1, офис 230. Карта: https://yandex.ru/maps/-/CCUFvCFU0D Режим работы: пн-пт, 10:00-20:00{% else %}Адрес доставки: {{ order.shipping_address.delivery_address}} {% endif %}{% unless order.delivery_title contains 'Самовывоз' %}({{order.delivery_title | strip_html}}){% endunless %}

Выбранный способ оплаты: {{ order.payment_title }}. {% if order.payment_needed? %}*Ссылка для оплаты: {{ order.pay_url }}*{% endif %}

{% if order.delivery_title == 'Заказ в 1 клик' %}Для завершения оформления, пожалуйста, отправьте ваши ФИО, адрес и предпочтительный способ доставки: курьер или пункт выдачи. Мы предложим вам наилучшие варианты.{% else %}Если у вас есть вопросы по заказу{% unless order.delivery_title contains 'Самовывоз' %} или доставке{% endunless %}, пожалуйста, позвоните нам по бесплатному номеру {{account.phone | escape}} или напишите в этот чат.{% endif %}

```
