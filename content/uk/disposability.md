## IX. Утилізовуваність
### Підвищуйте надійність за допомогою швидкого запуску і коректного вимкнення

**[Процеси](./processes) застосунку дванадцати факторів є *утилізовуваними*, тобто вони можуть бути запущені або зупинені в будь-який момент.** Це сприяє гнучкому масштабуванню, швидкому розгортанню [коду](./codebase) або змінам [конфігурації](./config) і надійності production-розгортання.

Слід намагатися **мінімізувати час запуску процесів**. В ідеалі час між моментом виконання команди запуску процесу і моментом, коли процес готовий приймати запити чи задачі, має тривати лише пару секунд. Короткий час запуску забезпечує більшу гнучкість для [релізу](./build-release-run) і масштабування. Крім того, це підвищує стійкість, оскільки менеджер процесів може легко переміщувати процеси на нові фізичні машини у разі необхідності.

Процеси мають **коректно завершувати свою роботу, коли вони отримують [SIGTERM](http://en.wikipedia.org/wiki/SIGTERM)** сигнал від диспетчеру процесів. Для веб-процесу коректне завершення роботи досягається завдяки припиненню прослуховування порту, до якого він прив'язаний (що означає відмову від отримання будь-яких нових запитів), завершенню обробки будь-яких поточних запитів та зупинці самого процесу. В цій моделі передбачається, що HTTP-запити короткі (не більше ніж кілька секунд), а у разі довгих запитів (long polling) клієнт має намагатися відновити з'єднання у разі його втрати.

Для процесу, що виконує фонові задачі (worker), коректне завершення роботи досягається за рахунок повернення поточного завдання назад у чергу задач. Наприклад, в [RabbitMQ](http://www.rabbitmq.com/) робочий процес може надіслати команду [`NACK`](http://www.rabbitmq.com/amqp-0-9-1-quickref.html#basic.nack); в [Beanstalkd](http://kr.github.com/beanstalkd/) завдання повертається в чергу автоматично щоразу, коли процес втрачає з'єднання. Системи, що використовують блокування, такі як [Delayed Job](https://github.com/collectiveidea/delayed_job#readme), мають бути повідомлені, щоб звільнити блокування задачі. В цій моделі передбачається, що всі задачі є [повторно вхідними](http://en.wikipedia.org/wiki/Reentrant_%28subroutine%29), що зазвичай досягається шляхом обертання результатів їхньої роботи в транзакції або шляхом використання [ідемпотентних](http://en.wikipedia.org/wiki/Idempotence) операцій.

Процеси також мають бути **стійкі до раптової зупинки** в разі відмови апаратних засобів, на яких вони запущені. Хоча це менш ймовірна подія, ніж коректне завершення роботи за допомогою сигналу `SIGTERM`, вона все ж таки може статися. Рекомендованим підходом є використання надійних черг завдань, таких як Beanstalkd, які повертають завдання в чергу задач, коли клієнти втрачають з'єднання або від'єднуються по тайм-ауту. У будь-якому випадку, застосунок дванадцяти факторів має проектуватися таким чином, щоб обробляти несподівані та некоректні вимкнення. Прикладом вдалої реалізації [архітектури "тільки аварійного вимкнення" (Crash-only design)](http://lwn.net/Articles/191059/) є [СouchDB](http://docs.couchdb.org/en/latest/intro/overview.html).