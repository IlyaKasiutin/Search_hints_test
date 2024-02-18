# Система поисковых подсказок
Текущий подход включает в себя:
- удаление полных дубликатов запросов
- удаление близких по смыслу с помощью FastText (поиск близости осуществляется с помощью FAISS)
- исправление опечаток с помощью библиотеки autocomplete (хотел сделать как-то более умно, но работало слишком медленно)

Как итог - данный подход обеспечивает вполне хорошую скорость выдачи запросов:
![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/e0b7daf0-1f83-49ac-83a8-8f7f53f612db)
![image](https://github.com/IlyaKasiutin/Search_hints_test/assets/116337853/3f739f73-67f6-466c-9cd8-a71229753271)
