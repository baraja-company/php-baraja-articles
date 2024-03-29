Навіщо і як використовувати фреймворки та бібліотеки
====================================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	uk: naviso-i-yak-vikoristovuvati-frejmvorki-ta-biblioteki
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Є відомий жарт, що програмісти починають використовувати фреймворки тільки тоді, коли пишуть свої і бачать, що це нікуди не годиться. Найсмішніше в цьому те, що це правда. Я сам це відчув на собі. Навіть двічі.

Втім, навіть на головній сторінці <a href="https://nette.org">Nette</a> про це сказано:

> Справжні програмісти **не використовують фреймворки**. Вони пишуть веб-додатки через командний рядок прямо на сервер. Це данина їм. Для решти з нас Nette зробить нашу роботу набагато простішою і приємнішою.

Роль фреймворків у розробці додатків
-----------------------------------

Упевнений, що ви і самі це знаєте:

У вас з'являється ідея великого проекту, і ви починаєте писати код з нуля безпосередньо на чистому PHP... через кілька годин ви виявляєте, що велика частина роботи все ще повторюється, і ви вирішуєте базові системні проблеми. Наприклад, як підключитися до бази даних, як відобразити форму, як перевірити дані, як відправити електронного листа та багато іншого.

Ви, ймовірно, напишете свої власні функції для виклику цих завдань. Якщо ви пишете кілька проектів одночасно, ви почнете ділитися цими функціями між проектами в одному великому безладному файлі, і ви будете постійно налаштовувати їх, як вам потрібно.

І коли функції знаходяться на певному етапі розвитку, що ви починаєте щоразу будувати поверх них новий проект і одразу розвиватися, тоді можна говорити про те, що ви написали свій фреймворк, або, принаймні, бібліотеку.

Що таке фреймворк і для чого він потрібен
-------------------------

Як ми вже показали на практичному прикладі з життя, фреймворк - це набір багатьох функцій (а в ідеалі класів та об'єктів), які економлять роботу програміста. При розробці йому не треба думати, як підключитися до бази даних, а він знає, що це вже десь запрограмовано і "просто працює".

Таким чином, готові фреймворки - це зібраний розум сотень людей, які розробили тисячі додатків протягом тривалого періоду часу, щоб налагодити одне рішення і одну екосистему для вирішення нових проектів.

З фреймворками ви також отримуєте набір **найкращих практик**, які є способами вирішення проблем без необхідності думати про це. Якщо ви зіткнулися з проблемою, можливо, хтось вирішив її до вас, і завжди краще знайти рішення в документації, ніж складно розбиратися самому.

Абстрактне програмування та принцип інкапсуляції
---------------------------------------------

Добре розроблені фреймворки, такі як Nette, дозволяють програмувати на **дуже високому рівні абстракції**.

Таким чином, програмісту (користувачеві фреймворку) не потрібно розбиратися, що відбувається всередині і як саме працюють компоненти всередині, що економить багато часу і зусиль. Він може зосередитися на вирішенні реальних проблем своєї заявки і дуже швидко отримати результат.

Сам Девід Грудл (автор фреймворку Nette) якось сказав, що він розробив Nette для того, щоб мати можливість запрограмувати сайт для клієнта за ніч, коли він пізно повертається з пабу, а вранці повинен запустити сайт. В основному це пов'язано з тим, що розробник повністю відсторонений від технічних питань.

Для того, щоб це працювало і розробник міг просто використовувати готовий фреймворк в цілому, дуже важливо правильно використовувати <a href="/encapsulation">принцип інкапсуляції</a>.
