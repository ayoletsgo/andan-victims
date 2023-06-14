# Описание проекта

В данном проекте проводится анализ погодных данных для Москвы. Используется информация о температуре, атмосферном давлении, скорости ветра и осадках за определенный период времени. Цель проекта - составить погодный портрет Москвы на основе этих данных.

## Структура проекта

Проект состоит из следующих частей:

### 1. **Подгрузка используемых библиотек**
 В данной части кода происходит импорт необходимых библиотек и установка начальных параметров.
### 2. **Парсинг данных из источника**
 В этой части кода происходит парсинг данных о погоде из двух источников (станций 276120 и 275155) за определенный период времени. Данные собираются в один датафрейм и сохраняются в файл "parsed_data.csv".
### 3. **Предварительный анализ данных**
 В этой части производится предварительный анализ данных, заполнение пропущенных значений и преобразование типов данных. Также делаются некоторые предположения о природе данных.
### 4. **Визуализация данных**:
 В данной части проекта происходит визуализация данных. Строится гистограмма средней температуры и гистограммы для каждого из признаков и динамика средних значений, также вычисляются некоторые статические параметры.

### 5. **Создание новых признаков**
Создание дополнительных колонок в DataFrame: для улучшения анализа и предсказательной способности модели машинного обучения были созданы дополнительные признаки на основе существующих данных о погоде. В данном шаге мы добавили три новые колонки в наш DataFrame:

* **Осадки_b**: Эта колонка является булевой и указывает, были ли осадки в данном дне.

* **Сезон**: В этой колонке указан текущий сезон года на основе значения месяца.

* **Выходной**: В этой колонке указывается, является ли данный день выходным (субботой или воскресеньем). 
### 6. **Проверка гипотез**
Подведем итоги проверки гипотез:

* Гипотеза 1: Температуру можно свести к какому-то из классических распределений.
Результаты:   
Данные не соответствуют нормальному распределению.
Данные не соответствуют распределению Стьюдента.
Данные соответствуют смеси трех нормальных распределений.
Визуально гистограмма реальных данных и предсказанное распределение смеси трех нормальных распределений показывают довольно хорошее совпадение.

* Гипотеза 2: Каждый год средняя температура растет относительно базисного 2009.
Результаты:   
Средняя температура не имеет статистически значимого тренда за период с 2009 по 2021 год.
Коэффициент корреляции Спирмена показывает положительную связь, но статистически значимого тренда не обнаружено.

* Гипотеза 3: Осень самая дождливая.
Результаты:  
Нет статистической значимости в различии между осенью и летом.
Нет статистической значимости в различии между осенью и весной.
Хотя осень можно считать более дождливой, различия не достигают статистической значимости на уровне 5%.

* Гипотеза 4: Выходные в среднем не являются статистически более дождливыми или холодными, чем обычные дни. Это означает, что представление о том, что в выходные "невезет" по погоде, является когнитивной ошибкой, связанной с восприятием.

* Гипотеза 5: Нет статистически значимых различий в средней температуре между осенью и весной. Поэтому предположение о том, что осенью всегда холоднее, чем весной, не подтверждается.

* Гипотеза 6: В осенний, весенний и летний сезоны температура статистически ниже, когда есть осадки, по сравнению с днями без осадков. Однако зимой температура при наличии осадков статистически выше, чем без осадков. Это связано с континентальным климатом Москвы, где осадки зимой способствуют повышению температуры, в то время как в остальные времена года осадки приводят к понижению температуры.

Таким образом, проведенный анализ позволяет лучше понять погодные особенности Москвы и опровергает некоторые распространенные представления о погоде в выходные дни и различиях температуры между осенью и весной. Континентальный климат Москвы проявляется в особенностях влияния осадков на температуру в разные сезоны.

Итак, гипотезы подтверждены либо опровергнуты на основе проведенных статистических тестов.

### 7.Машинное обучение
Мы использовали метод машинного обучения, называемый случайным лесом (Random Forest), для предсказания средней температуры и осадков. Модель была обучена на исторических данных за период с 2009 по 2019 годы. Мы разделили данные на обучающую и тестовую выборки, где в качестве признаков использовали значения температуры и осадков за предыдущие 14 дней. Также мы включили в модель информацию о сезоне.

Для предсказания средней температуры мы использовали модель RandomForestRegressor, которая является мощным алгоритмом машинного обучения, основанным на ансамбле решающих деревьев. Модель показала хорошие результаты на обучающих и тестовых данных, с MAE (средней абсолютной ошибкой) около 1.97 и R-squared (коэффициентом детерминации) около 0.932.

Для визуализации результатов мы построили графики разностей между фактическими и предсказанными значениями, диаграмму рассеяния и график остатков.

Затем мы использовали ту же модель, но для предсказания булевой переменной - наличия осадков. Модель RandomForestClassifier  показала плохие результаты с ROC-AUC около 0.82, Accuracy около 0.797, Precision около 0.585 и Recall около 0.474.

Мы пробовали разные подходы к решению задачи прогнозирования погоды. Сначала мы использовали модель случайного леса и настроили её параметры с помощью GridSearchCV. После множества экспериментов и подбора лучших параметров, мы получили модель, которая дала неплохие результаты.

Затем мы решили попробовать ещё одну модель - логистическую регрессию. Также провели подбор параметров с помощью GridSearchCV и получили лучшие значения. В результате этого эксперимента мы улучшили метрики качества модели и получили хороший ROC-AUC.

Чтобы ещё больше улучшить точность модели, мы подобрали оптимальный порог для классификации. Это позволило нам улучшить метрики и получить ещё более точные предсказания.

ROC-AUC: 0.84
Accuracy: 0.79
Precision: 0.54
Recall: 0.81

В целом, наши модели машинного обучения показали хорошие результаты в предсказании средней температуры и наличия осадков на основе исторических данных.

## Итог
Итак, мы успешно предсказываем погоду с помощью наших моделей! Мы исследовали данные, проводили много экспериментов, подбирали параметры и получили отличные результаты. Это был интересный и увлекательный проект, и мы гордимся своими достижениями!

Спасибо нам за проект! Мы в своём познании настолько преисполнились... 🎉🚀
## Как использовать проект

Для использования проекта необходимо иметь установленные следующие библиотеки:

- pandas
- numpy
- requests
- matplotlib
- scipy
- sklearn
- lxml

Запуск проекта производится последовательным выполнением кода в каждой части проекта. Важно установить значение переменной `reparse` в соответствующей части кода в зависимости от необходимости повторного парсинга данных или использования уже сохраненного файла "parsed_data.csv".

## Важно знать

- Проект использует данные о погоде для Москвы с двух метеорологических станций (276120 и 275155).
- В проекте происходит парсинг данных с сайта "http://pogoda-service.ru".
- Для некоторых пропущенных значений используется метод заполнения предыдущими значениями.
- Визуализация данных выполняется с использованием библиотеки matplotlib.
