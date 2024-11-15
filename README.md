# Домашнее задание к занятию 17 «Инцидент-менеджмент»

## Задание

Составьте постмортем на основе реального сбоя системы GitHub в 2018 году.

Информацию о сбое можно изучить по ссылкам ниже:

* [краткое описание на русском языке](https://habr.com/ru/post/427301/);
* [развёрнутое описание на английском языке](https://github.blog/2018-10-30-oct21-post-incident-analysis/).

| Пункт | Описание |
|-------|----------|
 | Краткое описание инцидента	|Нарушение связи между региональными узлами, что вызвало цепочку событий, приведшую к длительному снижению качества сервиса.|
| Причина инцидента	| 21 октября 2024 года в 22:52 UTC плановая замена оптического оборудования на восточном побережье США привела к кратковременному (43 секунды) нарушению связи между региональными узлами, что вызвало цепочку событий, приведшую к длительному снижению качества сервиса.|
| Воздействие	| В то время как часть платформы осталась работоспособной, множественные внутренние системы показали несогласованные и устаревшие данные, однако в реальности никакие пользовательские данные не были потеряны. В течение большей части инцидента также были недоступны события вебхуков и сборка сайтов GitHub Pages.|
| Обнаружение	| Внутренние системы показали несогласованные и устаревшие данные.|
| Реакция |	Инцидент был устранен за 24 часа 11 минут|
| Восстановление |	восстановление данных из резервных копий, синхронизацию реплик и возвращение к стабильной топологии. Были подключены дополнительные реплики, что снизило нагрузку и позволило ускорить синхронизацию данных.|
|Таймлайн	| 21 октября 2024, 22:52 UTC Нарушение связи привело к перераспределению MySQL-кластеров через инструмент Orchestrator, что вызвало запись данных в другой регион (западное побережье США), несогласованный с восточным. Это сделало невозможным безопасное возвращение кластеров к прежнему состоянию.|
||22:54 UTC Внутренние системы мониторинга зафиксировали многочисленные ошибки. Инженеры начали реагировать, и к 23:02 UTC выявили несогласованность топологий баз данных.|
|| 23:13 UTC Было принято решение вручную перенастроить топологию баз данных. Однако это оказалось сложным из-за новых записей, сделанных на западном побережье, которые не могли быть синхронизированы с восточным.|
|| 22 октября 2024, 00:05 UTC Инженеры разработали план восстановления, включающий восстановление данных из резервных копий, синхронизацию реплик и возвращение к стабильной топологии.|
|| 06:51 UTC Восстановление некоторых кластеров завершилось, но чтение из устаревших реплик привело к отображению несогласованных данных.|
|| 13:15 UTC Были подключены дополнительные реплики, что снизило нагрузку и позволило ускорить синхронизацию данных.|
|| 16:24 UTC После синхронизации было выполнено переключение на первоначальную топологию. Началась обработка очереди вебхуков и сборок GitHub Pages.|
|| 23:03 UTC Вся очередь была обработана, и статус системы обновлен на ""зеленый""."|
|Последующие действия	| "Уроки и дальнейшие действия|
| 1. Технические инициативы:| Улучшение конфигурации Orchestrator для предотвращения кросс-региональных промоций.|
|| Ускорение внедрения нового механизма отчетности о статусе системы для более точного отображения состояния сервисов.|
|| Активное внедрение дизайна с несколькими активными дата-центрами (active-active-active).|
|| Увеличение инвестиций в тестирование отказоустойчивости и хаос-инжиниринг.|
| 2. Организационные изменения: | Внедрение систематической проверки сценариев отказа для предотвращения аналогичных инцидентов.|
||Усиление обмена знаниями между командами для лучшего понимания существующей инфраструктуры.|
| 3. Коммуникация: |Улучшение процесса предоставления информации во время инцидентов для более точного информирования пользователей."|

