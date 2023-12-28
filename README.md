# Анализ приложения для знакомств
## Анализ результатов AB-тестирования контрольной и тестовой групп, в которых проверялось внедрение новой рекомендательной системы для приложения знакомств

Команда разработки приложения разработала новый алгоритм для поиска наиболее подходящих анкет. Для проверки работы алгоритма был проведен AБ-тест, в котором все пользователи были разделены на 2 группы (контрольную и тестовую). Пользователи в контрольной группе (группа 0) пользовались приложением со старым алгоритмом, а пользователи тестовой группы (группа 1) - с новым.

Правда ли, что новый алгоритм улучшил качество сервиса?

Давайте узнаем!

## Предварительный анализ данных
Перед проведением анализа необходимо "предварительно" посмотреть результирующий набор данных, чтобы сформировать первое представление о том, с какими вообще данными мы имеем дело.

- Количество строк - **14514**
- Количество столбцов - **4**
- NULL-значения: **отсутствует**
- Типы данных: **Int64**
- Размер контрольной и тестовой групп: **~500 человек**
- Измеряемая метрика: **is_match (бинарная)**

## Интерпретация результатов
### Начнем интерпретацию с определения целевой метрики, которую мы проверяем

В первую очередь, для интерпретации результатов, необходимо понять, каким образом можно оценить изменение качества работы сервиса, т.е какая метрику необходимо измерить до и после введения новой рекомендательной системы

В данном случае наше нововведение - это алгоритм для поиска наиболее подходящих анкет - поиск тех анкет, которые отвечают взаимностью (взаимные лайки). Выделим для себя несколько потенциальных метрик, на которые потенциально может влиять новый алгоритм

- Общее количество взаимных лайков по продукту
- Доля взаимных лайков от общего количества лайков на 1 пользователя

## 1. Общее количество взаимных лайков по продукту 

Можно начать анализ с проверки того, как введение нового алгоритма могло повлиять на общее количество взаимных лайков по продукту. **Можно ли с помощью нового алгоритма увеличить общее количество взаимных лайков?**

Визуализация распределения общего количества лайков показала, что в группе с новым алгоритмом, количество взаимных лайков выросло в сравнении с контрольной группой, которая использует старый алгоритм. Однако, это может быть связано с тем, что у нас в целом выросло количество лайков и относительный показатель количества взаимных лайков мог остаться прежним.

<image src = "/plots/chi_square_matches" alt = "Распределение лайков для пользователей в зависимости от группы">
 
Сформируем таблицу сопряженности для 2-х категориальных переменных (group, is_match) с 2-мя градациями (0, 1) и проведем анализ с помощью X^2-критерия, убедившись перед этим, что у нас соблюдаются все условия для его проведения, и проверим гипотезу о том, что новый алгоритм увеличивает количество взаимных лайков по продукту

- Все наблюдения независимы
- Минимальное количество наблюдений в каждой ячейке больше 5

При расчете статистики X^2 получили следующие результаты 
- X^2-статистика: 618,7
- p_value: 1.44e-136

**Ожидаемая таблица сопряженности:**

|  Группа| Невзаимные лайки | Взаимные лайки |
|:----------|:----------:|:----------:|
| Контрольная    | 3193   | 1599  |
| Тестовая    | 6478  | 3244 |

**Наблюдаемая таблица сопряженности:**

|  Группа| Невзаимные лайки | Взаимные лайки |
|:----------|:----------:|:----------:|
| Контрольная    | 3858   | 934  |
| Тестовая    | 5813  | 3909  |

Видим, что наше p_value сильно меньше заявленного 0.05, из чего можем сделать вывод, что распределение по группам действительно влияет на количество лайков. Но пока что это нам не говорит о том, какие именно показатели сильно отличаются между собой. Чтобы это узнать нужно провести анализ остатков (хотя бы визуальный)

<image src = "/plots/mosaic_matches" alt = "Распределение остатков в наблюдаемом и ожидаемом распределениях">









