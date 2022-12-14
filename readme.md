# Конвертер СЭМД (CDA) документов из формата XML в формат JSON
## Цель сервиса
Конвертировать XML представление в формат JSON на основании расширенного JSONschema документа, содержащего 
xpath поле для конечных узлов, которые должны быть заполнены значениями из входного XML документа

### Пример локального запуска

Пример полностью локального запуска
```
node index -s ./local/source -d ./local/dest -e ./local/error -c ./local/schemas
```
- входные XML мониторятся в папаке: ./local/dest
- выходные файлы в папке ./local/dest
- папка с известными схемами: ./local/schemas


Приемник и источник данных - Kafka, схемы - локально 
```
node index  -k localhost:9092 -s tpk -g xml2json -d jtpk.{{templateId}} -e etpk -c ./schemas
```
- кластер брокера: localhost:9092
- группа читателя: xml2json
- входная очередь: tpk
- выходные очереди: по шаблону, например jtpk.1.2.643.5.1.13.13.14.113.9.1
- папка с известными схемами: ./local/schemas


Конвертация должна производиться на основе схемы, описывающей структуру выходного JSON и xPath для выражения извлечения значений.

Пример [схемы](local/schemas/1.2.643.5.1.13.13.14.113.9.1.json)


### Переменные окружения и параметры запуска
Можно получить по команде
```
node index  -h
```

- `- KAFKA_BROKERS=kafka:9092,kafka-1:9092`  - строка с адресами Kafka
- `- KAFKA_SOURCE_TOPIC=semd.raw` - имя топика, из которого получать XML
- `- KAFKA_TARGET_TOPIC=semd.dep_{{templateId}}` - имя топика, в который передавать JSON маска выходного топика. Если имя топика содержит {{templateId}} - писать в разные выходные топики по имени, формируемом с маской из трех последних разрядов templateId СЭМД
- `- KAFKA_DMQ_TOPIC=dmq.cda` - имя топика, для сообщений, при обработке которых возникла ошибка
- `- KAFKA_CONSUMER_GROUP=cda.cnv` - имя kafka producer
- `- CDA_CNV_SRC=./templates/` - адрес каталога с шаблонами конвертации


