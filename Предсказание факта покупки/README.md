## Предсказание факта покупки

[HTML](https://github.com/shatalina/data_science_YP/blob/main/%D0%9F%D1%80%D0%B5%D0%B4%D1%81%D0%BA%D0%B0%D0%B7%D0%B0%D0%BD%D0%B8%D0%B5%20%D1%84%D0%B0%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%BA%D1%83%D0%BF%D0%BA%D0%B8/Master_%D0%AE%D0%BB%D0%B8%D1%8F%20%D0%A8%D0%B0%D1%82%D0%B0%D0%BB%D0%B8%D0%BD%D0%B0_review%20(1).html) [ipynb](https://github.com/shatalina/data_science_YP/blob/main/%D0%9F%D1%80%D0%B5%D0%B4%D1%81%D0%BA%D0%B0%D0%B7%D0%B0%D0%BD%D0%B8%D0%B5%20%D1%84%D0%B0%D0%BA%D1%82%D0%B0%20%D0%BF%D0%BE%D0%BA%D1%83%D0%BF%D0%BA%D0%B8/Master_%D0%AE%D0%BB%D0%B8%D1%8F%20%D0%A8%D0%B0%D1%82%D0%B0%D0%BB%D0%B8%D0%BD%D0%B0_review%20(1).ipynb)

### Описание проекта  
Даны данные по покупкам из трех магазинов разных типов за год. В данных указаны id клиентов, число заказов и стоимость покупок по дням. Необходимо построить модель, которая предскажет факт покупки/его отстутствие в течение последующих 30 дней по каждому клиенту. 


### Навыки и инструменты

- pandas
- matplotlib
- numpy
- shap
- sklearn
- datetime
- catboost

### Выводы

- Были исследованы данные
-  Проверено два способа подготовки данных для обучения моделей 
- Провели обучение моделей RandomForestClassifier, LogisticRegression  и CatBoostClassifier 
- Лучшей моделью оказалась LRC c параметрами {'C': 0.9000000000000001, 'class_weight': 'balanced'} 
- Показатели метрик лучшей модели:
- - 'recall_score' - 0.65
- - 'precision_score' - 0.45
- - 'f1' - 0.53
- - 'roc_auc' - 0.7
