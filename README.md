# Общие понятия
| Понятие | Описание |
| ------- | -------- |
| Метод/Функция | Выделенный кусочек кода который может принимать переменные *(параметры)* и возвращать некоторые данные. Предназначен для выделения большого кода на кусочки поскромнее. Аналогия - торт. |
| Переменная | Некоторые данные которые могут меняться |
| Объявление | "Паспорт" переменной/метода. Объявление это голова без кода. В нем написано как называется переменная/метод, какой тип и, для методов, какие параметры принимает. Параметры объявляются как обычные переменные |
| Реализация | Тело метода. Тот самый подвал с китайцами которые делают всю работу |
| Инициализация | При объявлении переменной можно сразу задать ей некоторое значение. Это называется инициализацией. В классе, переменные класса должны инициализироваться в конструкторах. Подробнее в разделе **Переменные/Инициализация** |

# Методы
## Объявление
*Тип* **имя**(*параметры*);

| Пункт | Описание |
| ----- | -------- |
| Тип | Любой тип данных который будет *возвращать* метод. Если предпологается что метод ничего не возвращает, следует писать тип **void** *(англ. пустота)* |
| Имя | Должно быть уникальным в своем пространстве. Если метод часть класса, то в классе не должно быть ничего с таким же именем. Ни переменные, ни методы. Если не является частю класса, то имя должно быть уникально во всей программе. Пример - main. Нельзя в программе иметь две функции main.  |
| Параметры | Объявляются так же как и переменные. Через запятую. Допускаются безымянные переменные, но в таком случае ими нельзя воспользоваться. |

Несколько методов могут иметь одинаковое имя только если их параметры отличаются. Пример:  
```
void sum(int i1, int i2);
void sum(float f1, float f2); //Можно
void sum(int i1, float f1); //Можно
void sum(int a, int b); //Нельзя. Имена параметров различаются, но типы совпадают с первой функцией - int, int 
```
## Параметры
Параметры это несколько переменных объявленных через запятую. Методы могут быть без параметров. В таком случае в скобках ничего не пишем   
Смотри **Объявление переменных**
## Возвращение данных
Метод может возвращать данные любого типа. Пусть то строка, число или объект класса.  
Для возврата используется ключевое слово `return` (*англ. возврать, вернуть*). С return обязательно должна идти некоторая переменная которая вернется туда, откуда вызван метод. При вызове return метод остановится и не будет продолжать работу. Поэтому его, обычно, ставят в конец.  
Исключение: тип `void`. В его случае return можно использовать без какой либо переменной. Это будет означать что именно здесь метод прекратит работу.
## Пример
```
int sum(int a, int b){
  return a+b;
}
```
Вот мы создали метод под названием **sum** (*англ. сумма*), который принимает параметры **int a, int b**. Тоесть когда метод начинает работу, ему заведомо известны числа которые записаны в переменные **а** и **b**. Ими надо как то манипулировать. Это зависит от того, что ты напишешь в теле метода.  
Возвращает метод тип **int**. Тоесть перед окончанием работы метода мы **обязательно** должны вернуть число.  
В теле мы **возвращаем** то, что получится после выражения *а+b*. Числа сложатся, и значение *вернется* из метода назад в программу.  
Вот другой пример:  
```
void sayNumber(int number){
  cout << number;
}
```
Тут мы создаем метод с названием **sayNumber** (*англ. Скажи Число*). Он принимает параметр **int number**. Отличие от предыдущего примера в типе метода: **void**. Заметь, дальше в программе мы нигде не используем слово *return*. Все потому, что оно не нужно. void - пустота, значит мы ничего не должны возвращать.  
В теле метода мы запишем число *number* в консоль *cout*. Тут объяснять, надеюсь, не надо.

Теперь смотри внимательно на этот метод.
```
int sayAndReturnSum(int n1, int n2){
  cout << (n1+n2);
  return n1+n2;
}
```
Ответь на вопросы:
* Какие параметры принимает метод?
* Если он что-то возвращает, то что?
* Что делает метод?  
*Ответ напиши мне **ВК***

# Переменные
## Объявление
*тип* **имя**;

| Пункт | Описание |
| ----- | -------- |
| Тип | Тип переменной. Если проводить аналогию *Объявление-паспорт*, то это *гражданство* переменной. |
| Имя | Имя переменной. Уникально в своем блоке (Программа, Класс, Метод, Цикл). Хотя, желательно, конечно, чтобы оно было уникально во всей программе. |

## Инициализация
Можно сразу задать переменной значение:  

| Способ | Описание |
| ------ | -------- |
| `тип имя;` | В принципе, схоже с инициализацей. В данном случае вызывается **Конструктор по умолчанию** для данного *типа* |
| `тип имя(параметры);` | Вот тут уже проходит инициализация с передачей *параметров* в **Конструктор с параметрами** |
| `тип имя = *нечто;` | Тут сначала вызывается **Конструктор по умолчанию**, а потом присваивается некоторое значение. Такой способ нежелательно использовать если *тип* это некий класс. Зато замечательно работает для элементарных типов данных (*int, char, float...*) |

## В параметрах 
При объявлении переменных в параметрах метода, имя не обязательно. Эта особенность редко используется только тогда, когда метод не будет никак использовать параметр. Такие методы создавать нежелательно, но возможно. Например этим можно пользоваться при *объявлении* метода в классе. 
