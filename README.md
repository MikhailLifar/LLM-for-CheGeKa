# LLM-for-CheGeKa

## Features
* Обращение к SOTA моделям через replicate
* Ассинхронная генерация ответов на большое число запросов

## Approaches
Подходы, примененные для улучшения ответов языковых моделей
* Simple Augment - Наложение дополнительных ограничений на промпт - дополнение запроса простыми инстукциями вроде “Пиши на русском”, “Отвечай коротко”
и “Не используй пунктуацию”, а также использование жадной стратегии генерации вывода.
* Chain of Thoughts (CoT) или "Думай пошагово" - добавление в конец промпта модели простой инстукции, побуждающей ее думать пошагово.
* Extract Information - пдоход похожий на CoT с элементами мультиагентности. Первый агент пытается извлечь как можно больше информации об ответе из текста вопроса
(является ли ответ предметом, действием, или свойством, к какой временной эпохе он/оно может относится и так далее). Второй агент на основе информации от первого генерирует окончательный ответ на вопрос.
* Self-Consistency - многократная генерация ответа, в качестве окончательного ответа модели выдается самый популярный ответ среди сгенерированных.
Self Consistency мы использовали в комбинации с Chain of Thoughts. Параметр метода - число генераций ответа.
* Suggester-Discriminator - мультиагентный подход. Идея следующая - первый агент - генератор - генерит ответ. Второй оценивает его по 10-бальной шкале. Если оценка ниже порога,
цикл генерация-оценка повторяется. Параметры метода - порог и максимальное число гененраций.
* SDwCA - Suggester-Discriminator with Critical Analysis. Идея следующая - первый агент - генератор - генерит ответ. Второй оценивает его по 10-бальной шкале. Если оценка слишком низкая, то еще один агент - критик - объясняет почему этот ответ не подошел. Затем генератор генерит новый ответ с учетом всех предыдущих ответов и критик.
* Command-Discussion - мультиагентная стратегия. Стратегия мытается имитировать обсуждение вопроса в команде. Идея следующая - первый агент генерит ответ на вопрос с пояснением. Следующий агент критикует предыдущего и предлагает альтернативный ответ с пояснением. Третий агент критикует первых двух и т.д.. Последний агент оспаривает ответ предпоследнего, не предлагая своего варианта
(это нужно чтобы у последнего варианта не было преимущества отсутствия критики). Потом отдельный агент - капитан - анализирует ответы всех агентов и выдает окончательный ответ. Параметр метода - число генераций ответа.

## Future
* Few shot
* Tree of Thoughts

## Results Table
Responses were generated with Llama3-405B-instruct model
| Approach    | Socres, f1 / exact match |
| -------- | ------- |
| As-is  | 0.444/0.139 |
| Simple Augment | 0.457/0.207 |
| CoT | 0.462/0.171 |
| Extract Information | 0.478/0.231 |
| Self-Consistency-4 | 0.474/0.233 |
| **Suggester-Discriminator-9-7** | **0.49 /0.236** |
| SDwCA-9-7 | 0.48 /0.233 |
| Command-Discussion-4 | 0.377/0.11 |

