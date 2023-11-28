# hacks-ai-BBBB

<div id="badges">
    <a href="https://www.python.org">
        <img src="https://img.shields.io/badge/python-6a6a6a?style=flat&logo=python&logoColor=white" alt="python badge"/>
    </a>
    <a href="https://flask.palletsprojects.com/en/latest/">
        <img src="https://img.shields.io/badge/flask-42aaff?style=flat&logo=flask&logoColor=white" alt="flask badge"/>
    </a>
    <a href="https://scikit-learn.org">
        <img src="https://img.shields.io/badge/sklearn-597b9a?style=flat&logo=sklearn&logoColor=white" alt="sklearn badge"/>
    </a>
    <a href="https://huggingface.co/docs/transformers/index">
        <img src="https://img.shields.io/badge/transformers-ffcf48?style=flat&logo=transformers&logoColor=white" alt="transformers badge"/>
    </a>
    <a href="https://www.nltk.org">
        <img src="https://img.shields.io/badge/nltk-%23042e3c?style=flat&logo=nltk&logoColor=white" alt="nltk badge"/>
    </a>
    <a href="https://pymorphy2.readthedocs.io">
        <img src="https://img.shields.io/badge/pymorphy2-5287ac?style=flat&logo=pymorphy2&logoColor=white" alt="pymorphy2 badge"/>
    </a>
    <a href="https://github.com/natasha/natasha">
        <img src="https://img.shields.io/badge/natasha-3333ff?style=flat&logo=natasha&logoColor=white" alt="natasha badge"/>
    </a>
</div>

___

Модель для определения кредитного рейтинга компании. Она упрощает проверку выпускаемых КРА пресс-релизов о назначении кредитного рейтинга. Продукт обладает графическим интерфейсом, позволяющим проверять пользовательский пресс-релиз. Модель определяет наиболее вероятные кредитные рейтинги согласно пресс-релизу и выделяет ключевые конструкции, повлиявшие на вычисление рейтинга.

Уникальность решения состоит в том, что модель учитывает **отношения между значениями рейтингов**, что дает возможность **минимизировать ошибку** до максимум одного класса. Модель основана на классических простых алгоритмах, что обеспечивает ей **интерпретируемость результатов** (одно из основных требований).

- Screencast веб-сервиса (если веб-сервис не работает): [here](https://drive.google.com/file/d/1I1oETpsmyr9q5SKkV8XULghdCBi7Skfs/view?usp=share_link)
- Ссылка на веб-сервис: [here](31.129.33.12:5000)

**Технический ход решения**:
- предложенную задачу классификации кредитного рейтинга интерпретировали как задачу регрессии, чтобы ввести порядковые отношения между классами
  - разработали шкалу соотвествия числовых значений для классов
  - подобрали функцию scaling-а значений расстояний между классами для их интерпретации как коэффициента "уверенности" (вероятности) в выборе класса
  <p> </p>
  
- сделали аугментацию датасета через перевод с одного языка на другой и возвращение к исходному
- предобработали текстовые данные
  - привели лемматизацию и удалили именнованные сущности через natasha
  - распарсили и избавились от email, номеров телефонов и пунктуации
  - векторизовали TF-IDF по 3-gramms для учета моделью контекста
  <p> </p>

- обучили линейную регрессию
  - для prediction вернули классы из числового диапазона с некоторой погрешностью
  <p> </p>

- вернули словосочетания, влияющие на выбор того или иного класса
  - **1 интерпретируемый способ**: сравнение TF-IDF значений векторов и соотвествующих весов линейной модели по модулю, вытаскивание фичей из сырых текстов через сравнение подстрок после стэмминга
  - **2 экспериментальный способ**: использовали [rubert-tiny2-russian-sentiment](https://huggingface.co/seara/rubert-tiny2-russian-sentiment) для сентимент анализа и находили эти же фразы опять в тексте через [sbert_synonymy](https://huggingface.co/inkoziev/sbert_synonymy), т.е. смотря на косинусное расстояние новых эмбеддингов
  <p> </p>

- обернули все в web-интерфейс на flask с возможностью ознакомиться с постановкой задачи и попробовать определить ближайшие классы кредитного рейтинга, а также двумя способами выделить ключевые формулировки, повлиявшие на решение


В папке `models` хранится модель векторизации текстов и модель регрессии, в папке `datasets` - предобработанные данные, остальные тетради описывают эксперименты/код для тестов и обучения/код для web-приложения. Наилучший выбитый **score** на аугментированных данных: `f1_micro = 0.88` для общих категорий рейтинга и для регрессии `MAE 0.026`, `RMSE 0.039`. Тем не менее модель возможно сильно переобучилась на аугментированных данных, из-за чего на финальном тесте хакатона провалилась

**Contributors**: 
- Кузнецов Андрей: [github](https://github.com/kdduha), [telegram](https://t.me/kdduha)
- Горячев Александр: [github](https://github.com/JesusChristOurGod), [telegram](https://t.me/goryachev_alexander)
- Сеничев Сергей: [github](https://github.com/ssenichev), [telegram](https://t.me/seniichev)
- Привалов Кирилл: [github](https://github.com/hellkirl), [telegram](https://t.me/hellkirl)

