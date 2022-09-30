Робота з файлами
================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	uk: robota-z-fajlami
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Для роботи з файлами в PHP існує ряд функцій.

- <a href="/fopen">fopen()</a>, низькорівневий доступ до файлів на диску
- <a href="/file-get-contents">file_get_contents()</a>, отримати вміст файлу або URL
- <a href="/file-put-contents">file_put_contents()</a>, зберігаючи рядок в локальний файл.

Функції диска
--------------

- `unlink($path)`, видалити файл
- `copy($from, $to)`, копіює файл з одного місця в інше (можна з URL на локальний диск)
- `rename()`, перейменувати або перемістити файл на диску
- `chmod()`, зміна прав на читання, запис або виконання файлу
