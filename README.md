Как ва уже поняли, это чат бот. 
Мой чат бот работает на простом обучении человеком и имеет свою открытую базу данных. 
Обучение происходит методом добавления данных в базу.
Проект основан на следующем методе работы, я не знаю применяли еге до меня или нет, но я открыто заявляю что, проект разрабатывался лично мной, и никем более. Собственно первые строки проекта устонавливают соединение с базой даннных:

    import sqlite3
    DataBaseSQL3inCode = sqlite3.connect("DataBaseInFiles")
    cursor = DataBaseSQL3inCode.cursor()
    
После устоновки соединения с базой данных, в переменную db(от английского data base) присваивается значение хранимой в коде базы данных. И изменения сохраняются.

    db = cursor.execute("SELECT * FROM AnsvQest").fetchall()
    DataBaseSQL3inCode.commit()
    
Далее начинаются функции программы, мы из пропустим, но потом к ним вернёмся.
Итак, мы на строке 49 (while True:), здесь мы берём у пользователя его сообщение для чат бота, проверяем на ключевые слова для работы программы(например exit).
Далее программа передаёт переменную prompt с сообщением в функцию recp.
в строках ниже запрос обрабатывается. Запрос переделывается в нижний регистр и убираются специальные символы(!,.№"@).

    pers = []
    pr = prompt
    pr = pr.lower()
    pr = re.sub('\W+',' ', pr )

Далее обработанный запрос польнователя проверяется на схожесть с каждым последующим из первого столбца в таблице(db(AnsvQest)).

    for k in range(len(db)):
        list_a = pr
        list_b = re.sub('\W+',' ', db[k][0] )
        result = per_com(list_a, list_b)
        pers.append([result, db[k][1]])
        
Создаётся новая таблица, в которой хранятся полученные результаты с процентам совпадения. И сразу сортируются по убыванию процентов.

    sortPers = sorted(pers)

Ну и если процент равет 0, то программа пишет: "я вас не понял".
иначе, если процент меньше 100, то просим у пользоватея его ответ на свойже вопрос, который добавится в базу данных.

    if result == 0:
        print('я вас не понял')
    else:
        print(sortPers[-1][1])
        if sortPers[-1][0] != 100:
            if input('вам понравился ответ?[да = anything][нет = .] ').lower() == '.':
                otvet_polzovatelya = input("ваш ответ")
                db.append([prompt, otvet_polzovatelya])
                print(db)

 далее программа зацикливается.
