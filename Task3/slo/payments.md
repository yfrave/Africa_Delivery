# SLO оплаты

```yaml
apiVersion: openslo/v1
kind: ServiceLevelObjective
metadata:
  name: payment-command-availability
spec:
  service: payments
  indicator:
    ratioMetric:
      good: payment_commands_total{result=~"pending|paid|declined"}
      total: payment_commands_total{result!="invalid_request"}
  budgetingMethod: Occurrences
  objectives:
    - target: 0.9995
      timeWindow: 30d
```

## SLI

- Доступность: доля допустимых команд, для которых сохранён однозначный статус `pending`, `paid` или `declined`.
- Задержка: P99 от приёма команды до сохранения `pending` не более 2 с.
- Правильность: повтор с тем же ключом идемпотентности возвращает тот же `payment_id`.

## SLO

- Доступность 99,95% за 30 дней -> бюджет ошибки около 21,6 минуты.
- Не менее 99,9% команд завершаются без ручного поиска статуса.
- При расходе 25% месячного бюджета за час объявляется критический инцидент.

## SLA

Покупателю обещается 99,9% доступности начала оплаты. Сбои M-Pesa и Flutterwave показываются как временная недоступность конкретного способа; заказ сохраняется 30 минут для повтора без двойного списания.

Источник: артефакты 4, 6 и 7; NFR-01, NFR-04, NFR-07.
