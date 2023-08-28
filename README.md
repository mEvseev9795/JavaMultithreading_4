## Задача 1. Программа-анализатор
## Описание

Ваш коллега из первой задачи до сих пор ломает голову над математической статистикой. Благодаря уже известному вам генератору, он создаёт **из символов "abc" 10 000 текстов, длиной 100 000 каждый**.

<details>
  <summary>Генератор текстов.</summary>
  
  ```java
    public static String generateText(String letters, int length) {
        Random random = new Random();
        StringBuilder text = new StringBuilder();
        for (int i = 0; i < length; i++) {
            text.append(letters.charAt(random.nextInt(letters.length())));
        }
        return text.toString();
    }
  ```
</details>
  
Теперь его интересует, как бы выглядел текст, в котором содержится максимальное количество:

* символов **'a'**;
* символов **'b'**;
* символов **'c'**.

Попробуем решить эту задачу многопоточно: чтобы за анализ строк на предмет максимального количества каждого из трёх символов отвечал отдельный поток. 

То есть за поиск строки с самым большим количеством символов **'a'** отвечал бы один поток, за поиск с самым большим количеством **'b'** — второй и за **'c'** — третий.

Но сгенерировать все тексты, сохранить их в массив и затем пройтись по ним неправильно, т. к. суммарно в текстах было бы около 1 млрд. символов, что привело бы к избыточному расходу памяти. Мы можем пойти другим путём и распараллелить этап создания строк и этапы их анализа.

Для этого строки будут генерироваться в отдельном потоке и заполнять блокирующие очереди, максимальный размер которых **ограничим 100 строками**.

Очереди нужно будет сделать по одной для каждого из трёх анализирующих потоков, т. к. строка должна быть обработана каждым таким потоком.

<details>
  <summary>Подсказка.</summary>
  
  Воспользуйтесь `ArrayBlockingQueue`.
</details>

## Реализация

1. Создайте в статических полях три потокобезопасные блокирующие очереди.
2. Создайте поток, который наполнял бы эти очереди текстами.
3. Создайте по потоку для каждого из трёх символов **'a'**, **'b'** и **'c'**, которые разбирали бы свою очередь и выполняли подсчёты.
  
Не изменяйте код программы, описанный в условиях задачи. В качестве результата отправьте на проверку ссылку на репозиторий с решением.
