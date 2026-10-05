# SLO уведомлений

```yaml
apiVersion: openslo/v1
kind: ServiceLevelObjective
metadata:
  name: notification-delivery
spec:
  service: notifications
  indicator:
    ratioMetric:
      good: notifications_total{result=~"accepted|delivered"}
      total: notifications_total{result!="invalid_contact"}
  budgetingMethod: Occurrences
  objectives:
    - target: 0.995
      timeWindow: 30d
```

## SLI

- Доставка: доля сообщений, принятых хотя бы одним разрешённым каналом.
- Задержка: 95% сообщений о заказе передаются провайдеру за 5 минут.
- Очередь: возраст старейшего сообщения не более 10 минут.

## SLO

- Доставка 99,5% за 30 дней.
- Повтор: 3 попытки с паузами 1, 5 и 20 минут, затем очередь ручной проверки.
- Нарушение SLO уведомлений не останавливает заказ или оплату.

## SLA

Отдельная финансовая гарантия не предоставляется. При задержке более 30 минут покупатель видит актуальный статус в личном кабинете.

Источник: артефакты 2 и 4; NFR-10.
