# Проект по Анализу данных на Python #

### Тема проекта: Изучение статистики трендовых видео сервиса YouTube ###

Авторы проекта: Сабиров Сардор и Тихонова Алена

## Структура репозитория ##

### 1. [Парсинг](https://github.com/sardor014/project_andan_2023/blob/main/scraper.ipynb) ###

> In: [api_key.txt](https://github.com/sardor014/project_andan_2023/blob/main/api_key.txt)

> In: [country_codes.txt](https://github.com/sardor014/project_andan_2023/blob/main/country_codes.txt) 

> Out: [dataset.csv](https://github.com/sardor014/project_andan_2023/blob/main/dataset.csv)


### 2. [EDA](https://github.com/sardor014/project_andan_2023/blob/main/EDA.ipynb) ###

> In: [dataset.csv](https://github.com/sardor014/project_andan_2023/blob/main/dataset.csv)

> Out: [dataset_after_EDA.csv](https://github.com/sardor014/project_andan_2023/blob/main/dataset_after_EDA.csv)

1. Предварительныя обработка
2. Визуализация
3. Создание новых признаков

### 3. [Гипотезы](https://github.com/sardor014/project_andan_2023/blob/main/hypothesis.ipynb) ###

> In: [dataset_after_EDA.csv](https://github.com/sardor014/project_andan_2023/blob/main/dataset_after_EDA.csv)


### 4. [Машинное обучение](https://github.com/sardor014/project_andan_2023/blob/main/ML.ipynb)

> In: [dataset_after_EDA.csv](https://github.com/sardor014/project_andan_2023/blob/main/dataset_after_EDA.csv)


## Этап 1 - Выбор темы и сбор данных

Парсер взят из открытого репозитория GitHub. После этого мы добавили несколько дополнительных функций и комментариев.

В нашем датасете представлены видео из раздела тренды платформы YouTube для 35 стран на момент крайнего запуска парсера - 14 мая 2023 года.

Список стран - Австралия, Австрия, Беларусь, Бельгия, Болгария, Канада, Хорватия, Чехия, Дания, Эстония, Финляндия, Франция, Грузия, Германия, Греция, Венгрия, Исландия, Ирландия, Италия, Латвия, Лихтенштейн, Литва, Молдова, Новая Зеландия, Норвегия, Польша, Португалия, Румыния, Сербия, Южная Корея, Испания, Швеция, Украина, Великобритания, Соединенные Штаты.

После парсинга наш датасет составлен из 6545 строк и 18 столбцов, после обработки путем удаления неиспользуемых признаков - 6545 строк и 14 столбцов

## Этап 2 - Предварительная обработка

После парсинга с использованием api-ключа, удаления некоторых столбцов имеем дело со следующими показателями:

*   `title` - название видео
*   `publishedAt` - время публикации видео, принцип кодирования: "DAY TIME"
    *   DAY - календарный день выпуска видео в формате YYYY-MM-DD
    *   TIME - время в формате HH:MM:SS


*   `channelTitle` - название канала, на котором видео было опубликовано
*   `category` - категория видео

*   `trending_date` - дата, в которую видео попало в список трендов

*   `tags` - теги, прикрепленные автором к видео

*   `view_count` - количество просмотров
*   `likes` - количество лайков
*   `comment_count` - количество комментариев под видео
*   `comments_disabled` - отключены ли под видео комментарии
*   `ratings_disabled` - отключена ли возможность просматривать количество лайков под видео
* `description` - описание видео
* `country` - страна, в чьи тренды попало видео
* `subscriber_count` - количество подписчиков канала

Дополнительно нами были созданы и проанализированы два показателя:

* `daytime` - время суток, в которое было опубликовано видео
    *   0 - видео опубликовано ночью (с 18:00 до 6:00)
    *   1 - видео опубликовано днем (с 6:00 до 18:00)
* `times_appeared` - количество стран, в которых видео попало в тренды

## Этап 3 - Гипотезы ##

Итак, нами были проверены 4 гипотезы:

1.   **Гипотеза о схожем распределении показателей `likes` и `subscriber_count`.** 

> После построения графиков распределения двух переменных: гистограмм, функций плотности, кумулятивных функций распределения и скрипичных диаграмм, мы приходим к выводу о визуальной схожести двух переменных. три различных тестирования - т-тест, U-критерий Манна — Уитни и тест Колмогорова - Смирнова, получаем абсолютно одинаковый результат - несмотря на внешнюю схожесть, переменные `likes` и `subscriber_count` подчиняются разным распределениям.

2. **Гипотеза о различии среднего количества просмотров между разными категориями видео.**

> В силу большого количества выборок и их разницы в размерах, мы прибегли к помощи критерия Краскела-Уоллиса, непараметрической альтернативы одномерному дисперсионному анализу, проверили категории `Comedy`, `Entertainment` и `Sports` на равенство количества просмотров. В результате нулевая гипотеза подтвердилась - рассматриваемые категории имеют одинаковое среднее количество просмотров

3. **Гипотеза о различии среднего количества лайков между видео с включенными и выключенными комментариями.** 

> Данную гипотезу мы тестировали через U-критерий Манна — Уитни, который сравнивает среднее двух распределений. Мы предпочли именно этот метод т-тесту, так как в отличие от т-теста, U-критерий Манна — Уитни не зависит от выбросов и концентрируется на центре распределения. По результатам тестирования получаем, что нулевая гипотеза об отсутствии различия среднего количества лайков между видео с включенными и выключенными комментариями отвергается.

4. **Гипотеза о влиянии времени публикации видео на количество просмотров.**

> Данную гипотезу мы проверили с помощью регрессионного анализа. Для этого нами была обучена модель линейной регрессии: в качестве целевой переменной использован столбец `view_count`, в качестве вспомогательной - `daytime`. Дальше мы взгялнули на полученный коэффициент регрессии обученной модели, он свидетельствует о сильной зависимости количества просмотров от времени публикации видео - ролики, опубликованные в промежутке между 18:00 и 6:00, набирают больше просмотров. Значит, нулевая гипотеза, в котрой утверждается, что время публикации видео не влияет на количество просмотров, не отвергается.

## Этап 4 - Машинное обучение ##

В ходе исследования нами были использованы две модели машинного обучения:

* Линейная регрессия - предсказывание количества лайков под видео. Нами была обучена простейшая модель линейной регрессии. В качестве целевой переменной использовался показатель `likes`, в качестве вспомогательных признаков - `category`, `view_count`, `comment_count`, `country`, `subscriber_count`, `times_appeared`.

* Случайный лес - предсказывание отключения комментариев под видео. Для того, чтобы осуществить предсказание бинарной целевой переменной, мы обучили модель случайного леса (RandomForestClassifier) из библиотеки sklearn. В качестве целевой переменной использовали столбец `comments_disabled`, в качестве вспомогательных признаков - `category`, `view_count`, `likes`, `subscriber_count`, `ratings_disabled`.

Итак, мы обучили две модели: базовую линейную регрессию и случайный лес, получили предсказания первого класса, показатели метрик. Далее нами были предприняты попытки улучшить качество обучаемых моделей - в обеих моделях мы применили перебор гиперпараметров по сетке, в случае с линейной регрессией была предпринята попытка улучшить показатели метрик путем нормализации данных в отрезок от 0 до 1, а также изменения структуры вспомогательных показателей. 

В результате, обученная нами линейная регрессия дала чуть более хорошие значения оцениваемых метрик, однако все еще наблюдается высокое значение отклонения предсказанных значений от фактических, делаем предположение о не самой хорошей структуре используемых выборок.

После перебора гиперпараметров в модели случайного леса значение метрики ROC AUC и вовсе сократилось, однако обученная модель все еще предсказывает лучше случайного классификатора. Отдельно хотелось бы отметить найденные нами интересные взаимосвязи между фактом отключения комментариев и другими показателями, такими как `likes`, `subscriber_count`, `view_count` и принадлежностью видеоролика к категориям `News & Politics` или `Entertainment`. Занимательно то, что данные взаимосвязи не были найдены на этапе корреляционного анализа - делаем вывод о том, как точно вариации в целевой переменной могут быть объяснены или предсказаны с использованием обученной нами модели.


