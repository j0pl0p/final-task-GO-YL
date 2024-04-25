<h1>Оркестратор GO</h1>
Я только бекенд успел сделать. Ну хотя бы апишка работает. Для скачивания проекта воспользуйтесь git clone. Выражения хранятся в СУБД, вместо gRPC я использую RabbitMQ. jwt не доделал( поставьте хотя бы миниум тогда пж (около 30)
<hr><h2>Запуск docker</h2>
Для того чтобы заработал rabbitMQ надо запустить файлик <strong>docker-compose.yml</strong>
<br>Пишем в консольку <strong>docker-compose up -d</strong>
Должна вылезти такая штучка: <img src="doc_images/img.png">
И дальше много текста. Главное, чтобы в конце было чет вроде
<strong>Time to start RabbitMQ: 14678385 us</strong> (Цифры могут меняться)
<h2>Запуск оркестратора</h2>
Откроем новый терминал (все должны находиться в проекте!)
Пишем: <strong>go run main.go</strong> <br>
Если все ок, вылезет:
<img src="doc_images/img_1.png">
Эт значит что очереди в RMQ открыты
<h2>Запуск агента/демона</h2>
Запускаем ТРЕТИЙ терминал в той же папке и пишем <strong>go run daemon.go</strong> <br>
Если хотите больше агентов - запустите больше терминалов.
<img src="doc_images/img_2.png">
Периодически он будет писать successfully sent beat - эт норм.
Если все прошло успешно, можно открывать Postman и тестить APIшечку.
<hr><h2>API</h2>
Для тестирования качаем Postman у кого его нет (можно и другими путями, наверное).
<h4>POST: http://localhost:8080/add-expression</h4>
Добавление выражения. Указываем без пробелов и не кривое!
<img src="doc_images/img_4.png">
На выходе, если все выполнилось правильно, вернется ID выражения, как на картинке
<h4>GET: http://localhost:8080/get-expressions</h4>
Тут ничего указывать не надо, вернется JSON со всеми сохраненными выражениями и их данными:
<img src="doc_images/img_3.png">
<h4>GET: http://localhost:8080/get-value</h4>
Указываем ID выражения, результат которого хотим узнать и получаем результат.
Если выражение еще не посчитано, об этом будет сообщено.
<img src="doc_images/img_5.png">
<h4>POST: http://localhost:8080/set-calc-durations</h4>
Здесь можно указать длительность подсчета каждого действия. Указываем в мс (миллисекундах). По дефолту - 200мс.
Если все хорошо - вернет статус 200.
<img src="doc_images/img_6.png">
<hr>
При перезапуске компонентов система продолжает корректно работать, т.к. данные хранятся в СУБД. (ну вроде))
<br>Мониторинг воркеров работает в терминале (это heartbeat ес чо).
<br> Ну и схемка как это работает:
<img src="doc_images/schema.png">
<img src="doc_images/img_7.png">
