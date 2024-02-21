# Система поисковых подсказок
## Поиск продолжений
Текущий подход включает в себя:
- удаление полных дубликатов запросов
- удаление близких по смыслу с помощью FastText (поиск близости осуществляется с помощью FAISS)
- исправление опечаток с помощью библиотеки autocomplete (хотел сделать как-то более умно, но работало слишком медленно)

Как итог - данный подход обеспечивает вполне хорошую скорость выдачи запросов:

![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/e0b7daf0-1f83-49ac-83a8-8f7f53f612db)
![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/4ede50c1-39a2-42ca-8925-9f6cc6da970e)

# Поиск брендов
Выбор наиболее релевантных товаров устроен следующим образом: для полученного топа продолжений фразы выбираются все товары, которые по этим фразам предлагались. Далее они сортируются сначала по релевантности, затем по популярности фразы.
Скорость поиска представлена ниже:

![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/9270574d-6d6f-4e9d-8032-1a9322222b53)
![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/f87edd5a-fefb-4411-a62e-3b7db1844504)

# Полный поиск
Поиск и продолжений, и брендов тоже выполняется достаточно быстро (исключены повторяющиеся действия)
![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/86501dd9-7096-4a5c-a551-0a7d27655a3a)

# Более подробное описание алгоритма:
- Сначала удаляются полные дубликаты запросов, чтобы получить словарь уникальных токенов
- Далее те запросы, которые очень похожи на более популярные запросы, заменяются ими. Схожесть оценивается как косинусное расстояние, полученное с помощью FastText, натренированного на данном корпусе. Он достаточно маленький, поэтому схожесть определяется достаточно плохо. Тем не менее, около 500 очень похожих запросов удалилось
- Все запросы векторизуются, близость между ними определяется с помощью FAISS (чтобы предобработка шла достаточно быстро)
- В очищенном корпусе выводится топ запросов по популярности: сначала самый популярный, затем второй по популярности и т.д. Перед выводом предложения исправляются с помощью библиотеки autocorrect
- Для получения товаров выбираются наиболее популярные запросы, для них берутся товары, которые по ним предлагались. Эти товары сначала сортируются по релевантности, а затем по популярности запроса

Также особенностью алгоритма является то, что исходный датасет автоматически парсится в момент создания класса. Из-за этого в начала надо подождать около 5 секунд (в jupyter notebook, в других средах может чуть дольше). Но зато можно загрузить любой другой датасет без предварительной обработки + на скорость рекомендаций это не влияет:)
