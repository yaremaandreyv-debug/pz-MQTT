# pz-MQTT
pz-mqtt-yarema⁠
# Практичне заняття "pz-MQTT": Розгортання та налаштування MQTT-брокера


## Виконанні завдання
1. Локально розгорнуто MQTT-брокер **Eclipse Mosquitto** за допомогою Docker Compose.
2. Налаштовано базову конфігурацію брокера (`mosquitto.conf`) з дозволом на анонімні підключення всередині локальної мережі та для підтримки двох протоколів:
   * Стандартний MQTT (TCP, порт 1883)
   * **MQTT over WebSockets** (порт 9001) для можливості підключення прямо з браузера.
3. Ознайомлено з основними принципами MQTT:
   * **Topic**: Канал для розподілу повідомлень (`VITI/Andrii/test`).
   * **Publish**: Відправка повідомлення клієнтом у топік.
   * **Subscribe**: Підписка на топік для отримання повідомлень.
   * **QoS**: Рівень якості обслуговування (використано базовий QoS 0).
4. Здійснено перевірку працездатності брокера через MQTT-інструментарій у Postman.
5. Розроблено два простих Web-клієнти (HTML + JavaScript) з використанням бібліотеки `MQTT.js`:
   * `publisher.html`: Інтерфейс для вводу та відправки повідомлень у топік.
   * `subscriber.html`: Інтерфейс, що слухає топік та миттєво відображає отримані дані на екрані.

## Конфігураційні файли

### Конфігурація Docker Compose (`docker-compose.yml`)
```yaml
version: '3.8'
services:
  mqtt-broker:
    image: eclipse-mosquitto:latest
    container_name: mosquitto-broker
    ports:
      - "1883:1883"
      - "9001:9001"
    volumes:
      - ./mosquitto.conf:/mosquitto/config/mosquitto.conf
    restart: always

  web-client:
    image: nginx:alpine
    container_name: nginx-web-client
    ports:
      - "8081:80"
    volumes:
      - ../client:/usr/share/nginx/html:ro
    restart: always
