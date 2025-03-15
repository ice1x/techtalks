# Сравнение Redis Pub/Sub, RabbitMQ и Kafka

Redis Pub/Sub, RabbitMQ и Kafka — это системы обмена сообщениями (message brokers), но они решают разные задачи и имеют разные архитектурные особенности. 

## 🔑 Основные различия

| Критерий              | Redis Pub/Sub                 | RabbitMQ                             | Kafka                              |
|----------------------|--------------------------------|---------------------------------------|------------------------------------|
| **Тип системы**       | In-memory data store + Pub/Sub  | Message broker (AMQP)                 | Distributed event streaming platform |
| **Архитектура**       | Публикация/подписка              | Очереди с маршрутизацией               | Логи событий (event logs)            |
| **Персистентность**   | Нет                             | Опционально (на диск)                  | Да (долгосрочное хранение на диск)   |
| **Скорость**          | Очень высокая (память)           | Высокая                                | Высокая (диск + кеширование)         |
| **Гарантия доставки** | Нет гарантии                     | Да (подтверждения сообщений)           | Да (хранение в логах)                |
| **Обработка**         | В реальном времени               | Надёжная доставка и маршрутизация       | Масштабируемая потоковая обработка   |
| **Масштабируемость**  | Ограниченная (шардинг)            | Горизонтальное масштабирование         | Высокая (разделы и кластеры)         |
| **Сложность использования** | Простая (легко настраивается) | Средняя (AMQP протокол)                | Сложная (кластеризация, Zookeeper)   |

---



## 🚀 Когда использовать?
### 🔴 Redis Pub/Sub
#### ✅ Подходит для:
  - Чат-приложений в реальном времени.
  - Уведомлений и оповещений.
  - Легковесных потоков данных.
#### ❌ Не подходит для:
  - Гарантированной доставки сообщений.
  - Долгосрочного хранения и повторного воспроизведения данных.

**Пример использования:**
```python
import redis

r = redis.Redis()

# Публикация сообщения
r.publish('channel1', 'Hello, Redis!')

# Подписка на канал
pubsub = r.pubsub()
pubsub.subscribe('channel1')

for message in pubsub.listen():
    print(message)
```
---



# 🐇 RabbitMQ
#### ✅ Подходит для:
- Сложной маршрутизации сообщений (Fanout, Direct, Topic).
- Гарантированной доставки (подтверждения, очереди).
- Интеграции микросервисов.

#### ❌ Не подходит для:
- Очень больших объёмов данных.
- Потоковой обработки в реальном времени.


# 🐇 Пример использования RabbitMQ:
## 📦 Установка библиотеки
Установите библиотеку с помощью pip:
```bash
pip install pika
```
🚀 Пример отправки сообщения
```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Создание очереди
channel.queue_declare(queue='hello')

# Отправка сообщения
channel.basic_publish(exchange='', routing_key='hello', body='Hello, RabbitMQ!')
print(" [x] Sent 'Hello, RabbitMQ!'")
connection.close()
```
📥 Пример получения сообщения
```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Подписка на очередь
channel.queue_declare(queue='hello')

# Callback для обработки сообщений
def callback(ch, method, properties, body):
    print(f" [x] Received {body}")

# Потребитель
channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

### 📝 Описание
- Очередь с именем `hello` создаётся, если её нет.
- Отправка и получение сообщений осуществляется через канал.
- Потребитель подписывается на очередь и обрабатывает сообщения через callback-функцию.

---

### ✅ Запуск
1. Запустите сервер RabbitMQ:
```bash
   rabbitmq-server
```
2. Запустите скрипт отправки сообщения.
2. Запустите скрипт получения сообщения.
---



# 🦁Kafka
#### ✅ Подходит для:
- Потоковой обработки больших объёмов данных.
- Логирования и аудита событий.
- Хранения и обработки данных с возможностью повторного воспроизведения.
#### ❌ Не подходит для:
- Легковесных и короткоживущих сообщений.
- Использования в качестве простой очереди с гарантией доставки.
# 🦁 Пример использования Kafka:
## 📦 Установка библиотеки
Установите библиотеку с помощью pip:
```bash
pip install kafka-python
```
🚀 Пример отправки сообщения (Producer)
```python
from kafka import KafkaProducer

producer = KafkaProducer(bootstrap_servers='localhost:9092')
producer.send('my_topic', b'Hello, Kafka!')
producer.flush()
print(" [x] Sent 'Hello, Kafka!'")
```

📥 Пример получения сообщения (Consumer)
```python
from kafka import KafkaConsumer

consumer = KafkaConsumer(
    'my_topic',
    bootstrap_servers='localhost:9092',
    auto_offset_reset='earliest',
    group_id='my-group'
)

print(" [*] Waiting for messages...")
for message in consumer:
    print(f" [x] Received {message.value.decode('utf-8')}")
```

### 📝 Описание

- Используем библиотеку `kafka-python` для работы с Kafka.
- Сообщения отправляются в топик `my_topic`.
- Потребитель подключается к тому же топику и обрабатывает все сообщения.

---

### ✅ Запуск

1. Запустите сервер Kafka и Zookeeper:
```bash
   zookeeper-server-start.sh /path/to/zookeeper.properties
   kafka-server-start.sh /path/to/server.properties
```
2. Создайте топик:
```bash
kafka-topics.sh --create --topic my_topic --bootstrap-server localhost:9092
```

3. Запустите продюсера и консьюмера.
```bash
kafka-topics.sh --create --topic my_topic --bootstrap-server localhost:9092
```