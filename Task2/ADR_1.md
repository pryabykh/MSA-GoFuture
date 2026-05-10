### <a name="_b7urdng99y53"></a>**Название задачи: Проектируем Event-Driven архитектуру**

### <a name="_uanumrh8zrui"></a>**Дата: 8 апр 2026**

### <a name="_3bfxc9a45514"></a>**Контекст**

Приложение должно обрабатывать более 500 тысяч конкурентных поездок и поддерживать динамическое ценообразование.
Существующий монолит не справляется с масштабированием, поэтому необходима событийная архитектура с Event Bus (шина на
базе Kafka)

### <a name="_3bfxc9a45514"></a>**Требования**

- Модель доменных событий: BookingCreated, BookingFinished, DriverLocationUpdated, PriceCalculated, PaymentCompleted,
  FraudDetected.
- Kafka топики с партиционированием по регионам (booking-region, driver-region).
- Saga для долгоживущих транзакций (Booking → Payment → Notification).
- Надёжная доставка: at-least-once, retry, dead-letter queue.
- Мониторинг событий, задержек, пропускной способности и и других метрик.

### <a name="_3bfxc9a45514"></a>**Решение**

- Event Bus (Kafka) для асинхронного взаимодействия.
- Отправка событий от сервисов Booking, Payments, Pricing, Driver Service в Event Bus
- Подписка на события для сервисов Notification, Fraud, Analytics.
- Сбор метрик в Prometheus, визуализация в Grafana, а также сбор логов в Loki для всех сервисов

### <a name="_3bfxc9a45514"></a>**Виды метрик**

- Kafka: 
  - Пропускная способность
  - Размер очереди
  - Error Rate (failed messages / total messages)
  - Размер Dead-letter queue

- Доменные сервисы:
  - Время обработки запроса
  - Количество событий Saga (commit / rollback)

- Observability:
  - Логи всех сервисов собираются в Loki
  - Метрики собираются в Prometheus
  - Визуализация метрик в Grafana
  - Алерты отправляются через Alertmanager
