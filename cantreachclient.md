# Шаблон уведомления для статуса 'Не удалось связаться с покуп.'

В данном коде мы не просто сообщаем, что не удалось связаться с клиентом, а дополнительно показываем, что именно он заказывал. 

Сообщение в коде представленно с длинными строками, чтобы не было ненужных переносов строк при отправке сообщения клиенту. Используются символы форматирования текста WhatsApp

## Шаблон сообщения
```
К сожалению нам не удалось связаться с вами для подтверждения заказа. Вы заказывали у нас:
{% for item in order.items %}- {{ item.title | strip_html }}. {{item.sale_price.full | money}} x {{item.quantity}} {{item.product.unit}}
{% endfor %}
Если Ваш заказ актуален, пожалуйста, напишите в этот чат или позвоните по бесплатному номеру ```8-800-55-16-163```.

*Обратите внимание! Мы не принимаем звонки в WhatsApp!*

Интернет-магазин КАМА (перевязочные материалы) https://kama-med.ru

```
