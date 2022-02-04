## Код для исследования
```java
public class JvmComprehension {
    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }
    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```
## Выполнение
Cначала происходит подгрузка классов через ClassLoader. 
В данном коде это будут JvmComprehension, String, Object, Integer, System.

Далее происходит связавыние Linking, а именно:
Verify - проверка валидности
Prepare - подготовка примитивов в статических классах
Resolve - разрешение символьных ссылок

Далее следует инициализация класса Inititalization - выполняются static инициализаторы полей и методов - printAll и main.
Дальше данные попадают в Metaspace, Stack Memory и Heap.

1. При выполения метода main создается Фрейм1 в Stack Memory (SM).
2. Фрейм1 -> int i = 1.
3. Фрейм1 -> в Heap создается объект класса Object, а ссылка o - в SM.
4. Фрейм1 -> в Heap создается объект класса Integer, а ссылка ii - в SM.

5. Создается Фрейм2 в SM для функции printAll.
6. Фрейм2 -> ссылка o, указывающая в Heap на объект класса Object, имеющий ссылку o из Фрейм1.
7. Фрейм2 -> int i = 1.
8. Фрейм2 -> ссылка ii, указывающая в Heap на объект класса Integer, имеющий ссылку ii из Фрейм1.
9. Фрейм2 -> в Heap создается объект класса Integer, а ссылка uselessVar сохраняется в SM.

10. Cоздается Фрейм3 в SM под вызов println. 
11. Фрейм3 -> ссылка String, указывающая на область String Pool (часть Heap), где будет создана строка String. 
12. Фрейм3 -> int i = 1.
13. Фрейм3 -> ссылка ii, указывающая в Heap на объект класса Integer, имеющий ссылки ii из Фрейм1 и ii из Фрейм2.

14. Cоздается фрейм4 в SM под вызов println.
15. Фрейм4 ->  ссылка String, указывающая на область String Pool (часть Heap), где будет создана строка String
    (строковый литерал "finished").

В Java происходит автоматический сбор мусора - объектов которые больше не используются (на которые нет внешних ссылок). 
Такие объекты определяются методом  - обход графа достижимых объектов. Проследить использование памяти и сделать
принудительную очистку мусора можно с помощью утилиты VisualVM.