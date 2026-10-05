# SLO каталога

```yaml
apiVersion: openslo/v1
kind: ServiceLevelObjective
metadata:
  name: catalog-read-availability
spec:
  service: catalog
  indicator:
    ratioMetric:
      good: http_requests_total{route="/catalog",status!~"5.."}
      total: http_requests_total{route="/catalog"}
  budgetingMethod: Occurrences
  objectives:
    - target: 0.999
      timeWindow: 30d
```

## SLI

- Доступность: доля ответов каталога без 5xx. Запросы наблюдения и ошибки клиента 4xx не учитываются.
- Задержка: доля успешных ответов с длительностью не более 800 мс по P99.
- Источник - метрики входного шлюза, окно 30 дней.

## SLO

- Доступность 99,9% за 30 дней -> бюджет ошибки около 43,2 минуты.
- P99 не более 800 мс для 99% пятиминутных окон.
- При расходе 50% бюджета за 7 дней выпуск функций останавливается; команда устраняет причину.

## SLA

Бизнесу обещается 99,5% доступности каталога в месяц. При нарушении запуск рекламных рассылок приостанавливается, CEO получает отчёт в течение рабочего дня.

Источник: артефакты 1, 2 и 5; NFR-02, NFR-03.
