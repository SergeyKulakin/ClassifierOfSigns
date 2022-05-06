# ClassifierOfSigns
Scientific research seminar project<br/>
авторы: [Сергей Кулакин](https://github.com/SergeyKulakin), [Паль Николай](https://github.com/nicolaspal21), [Регина Хабирова](https://github.com/ReginaKhabirova),[Королёв Максим](), [Бодюль Евгений](https://github.com/broken-kerosene)

[Dataset](https://drive.google.com/drive/folders/1Dy3xREmKdPzN-Lftn-wyrOBWBr8c775z)  - ссылка на собранные данные
<h3>Постановка задачи</h3><br/>
&emsp;Цель исследования - оценка качества классификации изображений дорожных знаков с применением классических методов машинного обучения. 
В качестве метрики будем исппользовать долю верных ответов. Выдвинем гипотезу, что выборка квазисбалансированная.  Самый редкий класс примерно в пять раз встречается реже чем самый популярный.

|    Класс   | %         |
|:----------:|-----------|
| os_predpis | 22.060958 |
| tables     | 21.480406 |
| zapret     | 17.997097 |
| predupr    | 12.191582 |
| info       | 11.756168 |
| predpis    | 5.370102  |
| prior      | 5.079826  |
| service    | 4.063861  |




<h3>Описание процесса сбора данных </h3><br/>
&emsp;Сбор данных проводился при помощи смартфона, фотографии делались в светлое время дня. 
После сбора данных проводилась подготовка изображений: изображение обрезалось (в центре должен находится знак с минимальным кол-во посторонней информации),
изменялся размер изображения до разрешения 224х224 пикселя.
Всего существует 8 типов дорожных знаков:<br/>
    &emsp;&emsp;1 предупреждающие; (в основном треугольные знаки с красной окантовкой и чёрным изображением внутри) 84 изображения<br/>
    &emsp;&emsp;2 знаки приоритета; (прямоугольные, треугольные, круглые знаки с красной и белой окантовкой, возможно заливка жёлтым ) 35 изображения<br/>
	&emsp;&emsp;3 запрещающие; (Круглые знаки с красной окантовкой, встречается жёлтая и синяя заливки по середине знака) 124 изображения<br/>
    &emsp;&emsp;4 предписывающие; (Круглые знаки с синей заливкой и белыми изображениями) 37 изображений<br/>
    &emsp;&emsp;5 особые предписания; (прямоугольные знаки с синей заливкой и чёрно-белыми изображениями) 150 изображений<br/>
    &emsp;&emsp;6 информационные; (Прямоугольные знаки с синей, желтой, зелёной заливками и белыми надписями) 81 изображение<br/>
    &emsp;&emsp;7 сервисные; (прямоугольные знаки с белым окном и чёрным изображением)  28 изображений<br/>
    &emsp;&emsp;8 с дополнительной информацией. (белые прямоугольные знаки с черными изображениями и текстом) 148 изображениями<br/>
&emsp;&emsp;Всего было сборано 689 изображения.

<h3>Описание  моделей</h3><br/>
&emsp;Все изображения цветные и имеют три цветовых канала. Следовательно, имеет смысл составить ансамбль из трёх моделей для повышения качества.<br/>
&emsp;На перечисленных ниже моделях применялись различные подходы при обучении, где-то использовалась серые пиксели в качестве признаков, где-то изображение разбивалось на цветовые каналы. Ниже представленны результаты моделирования<br/>
	
&emsp;&emsp;линейная регрессия score: по каналам с PCA - 0.8454;<br/>
&emsp;&emsp;random forest score: по каналам - 0.807, по каналам c PCA - 0.801;<br/>
&emsp;&emsp;GB (catboost, xgboost, lightgbm) score: catboost(серый) - 0.777,  xgboost(серый) - 0.7536,<br/> &emsp;&emsp;lgbm(серый) - 0.7971;<br/>
&emsp;&emsp;KNN score: серый 0.734, по каналам с PCA - 0.8309;<br/>
&emsp;&emsp;SVC(kernel=‘linear’) score: серый - 0.831; <br/>

&emsp;После обучения были сделаны следующие выводы:<br/>
&emsp;&emsp;хуже всех модельки обучаются на 1 канале (красный цвет);<br/>
&emsp;&emsp;лучше всех на голубом;<br/>
&emsp;&emsp;на SVC и RandomForest суммарная модель показывает лучшие результаты чем на отдельных каналах;<br/>
&emsp;&emsp;лучший результат у SVC (0.84);<br/>
&emsp;&emsp;при снижении размерности в 500 раз качество у KNeighbors и CatBoost выросло;<br/>
&emsp;&emsp;обучение моделей с использованием PCA происходит быстрее, особенно в случае CatBoost.<br/>

<h3>План по улучшению моделей</h3><br/>
&emsp;&emsp;убрать шумовую составляющую фона;<br/>
&emsp;&emsp;уменьшить кол-во признаков у объекта (методы снижения размерности) //частично выполнено;<br/>
&emsp;&emsp;провести оптимизацию гиперпараметров (например в optuna);<br/>
&emsp;&emsp;для поднятия точности можно попробовать перебалансировку (добавление фото в бедные классы)<br/>
&emsp;&emsp;увеличить выборку через аугментацию;[1]<br/>
<br/>

&emsp;&emsp;1. Evaluation of Traffic Sign Recognition Methods Trained on Synthetically Generated Data <br/>