# Museum_fund<br />
В рамках соревнования на платформе "Цифровой прорыв".<br/>

# Данные для проекта:<br/> 
train.csv - данные для обучения<br/> 
train_url_only.csv - дополнительная обучающая выборка<br/>
test.csv - данные для инференса<br/>
image.zip - изображения для обучения<br/>

# Цель:<br/>
Обучение искусственного интеллекта на распознавание вида или категории предмета музейного фонда (типологии) при внесении нового предмета в каталог. Для некоторых предметов в тестовом наборе могут быть только изображения, для других - только текстовые описания, для третьих - и то и другое. Разрешается использовать любые открытые данные о музейном фонде для расширения и обогащения обучающей выборки.<br/> 
Подробнее: https://cups.mail.ru/ru/workareas/leadersofdigital2021/623/1087<br/> 
`В условиях не сказано про лимиты время выполнения кода и ограничениях в данных (то есть можно было скачать хоть весь музейный фонд и обучаться месяц), но дисквалификация могла быть по усмотрению администраторов, что немного демативировало :) Но хотелось реализовать свою идею, я использовал только 180К изображений которые предоставлены в доп.файле.`<br/>
# Метрика: <br/>
В качестве метрики выбрана F1-score(macro). Выбор метрики также неочевиден, в тестовом наборе для инференса всего 1200(600/600) изображений и присутсует значительный дисбаланс классов, но в данном случае классы равнозначны между собой и штрафовать за мелкие классы я не вижу смысла.<br/>   

После визуального просмотра изображений, увидел что в изображениях много шума, то есть изображения по типологиям перемешены (файл EDA.ipynb), поэтому перед скачиванием дополнительных данных, решил реализовать следующую идею, обучить модель на 8 фолдах, далее скачать дополнительные данные и очистить от шума, используя кластеризацию, далее обучить одну "жирную" модель на всех оставшихся изображений после кластеризации.<br/>   
Реализация:<br/> 
**Unzipping_initial_processing.ipynb** - разархивация и удаление пустых и битых файлов.<br/> 
**Short_models_for_clustering_images.ipynb** - обучаю модели на 8 фолдах, использую только небольшой датасет от организаторов (без доп. данных) в дальнейшем использую эти модели для кластеризации.
**Downloading_additional_data.ipynb** - скачивание дполнительных изображений для финальной модели.

**Сreating_training_sample_for_images.ipynb** - кластеризация изображений. По началу была идея кластеризовать денс слоем из 300 нейронов, но работало это на 180К изображений размером 512Х512 очень долго, поэтому кластеризовал распределния с софтмакс, по картинкам видно, что кластеризация выделила правильные сущности (побробней Сreating_training_sample_for_images.ipynb).<br/> 

В типологии документы выделены типология редкие книги:<br/> 
![demo](https://github.com/chelmed/Museum_fund/blob/main/doc.png)<br/> 
В типологии печатной продукции выделен шум:<br/>
![demo](https://github.com/chelmed/Museum_fund/blob/main/print_prod.png)<br/> 
В типологии прочие выделена типология нумезматика:<br/>
![demo](https://github.com/chelmed/Museum_fund/blob/main/num.png)<br/>

**Long_model_for_inference_images.ipynb** - Реализация главной модели для инференса. В процессе обучения использован апсэмплинг и аугментация<br/>

Create_model_W2V.ipynb
Models_train_for_text.ipynb - Реализая для текста 



