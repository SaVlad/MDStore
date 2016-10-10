# Использование класса
## Синтаксис
```
class foo{
  int field; //Некоторое поле
  foo(); //Конструктор по умолчанию
  void method(); //Некоторый метод
};
```
Объявляет класс с именем **foo**. С этих пор в программе зарегистрирован новый тип данных: "**foo**"  
Этот класс можно использовать как любой другой тип данных.Такие как `int`, `bool` `char*`.
## Обращение к составляющим класса
Возьмем пример  
`foo bar;`  
Здесь мы объявили *(и инициализировали конструктором по умолчанию)* переменную с названием `bar` типа `foo`.  
Теперь, чтобы вызывать методы и получать поля, мы пишем название переменной и то, что хотим из нее получить через точку   
```
foo bar;
bar.method();
cout << bar.field << endl;
```
Тут мы сначала вызвали конструктор по умолчанию (`foo();`), потом метод (`void method();`), 
а затем напечатали в консоль поле (`int field;`)  

Особый подход требуют **указатели** на объект класса. У них свое обращение и свой способ создания новых объектов.  
Рассмотрим пример выше, но теперь с указателями  
```
foo* bar = new foo();
bar->method();
cout << bar->field << endl
```
Так мы создали переменную типа **указатель на объект типа foo** (`foo* bar`) 
и инициализировали его свежевыделенной памятью с вызовом конструктора по умолчанию (`= new foo();`).
Теперь к этому указателю можно обращаться как к обычной переменной, но с некоторыми изменениями синтаксиса. 
Вместо точки используем "стрелочку" `->`. А так все то же самое что и примером ранее.  
Важно помнить, что указатель может быть и не инициализированным. При обращению к "*нулевому*" указателю, будет вылетать ошибка.
## Модификаторы доступа
Не всегда ты захочешь делать части класса доступными всем. Порой некоторые переменные или методы нужно закрыть от чужих глаз.  
Для этого мы вешаем "таблички", модификаторы доступа.

Их три типа:
```
private:
public:
protected:
```
Первый - `private`. Полное закрытие полей и методов от внешнего мира. Только сам класс может ими манипулировать и ничто вне его.  
Второй - `public`. Противоположность `private`. Полностью открыт для любых изменений из любой части программы.  
Третий - `protected`. Защищенный. Для всего мира это `private`, но для "детей" (*наследников этого класса*) это `public`.

## Конструкторы
Первое что всегда вызывается при создании объекта это его конструктор.
Если мы создали переменную объекта, то мы обязаны использовать хоть какой нибудь конструктор.  
Конструкторы бывают 3 типов:
```
class foo{
  foo(); //По умолчанию. Без параметров
  foo(int bar); //С параметрами
  foo(foo&bar); //С особым параметром. Конструктор копирования
}
```
На примере, пожалуй будет проще  
```
foo bar;
foo xyz(123);
```
Сверху вниз. Сначала мы создали переменную  **bar** типа **foo**. 
Вызвался конструктор по умолчанию, так как мы не писали никаких параметров при создании.  
**Важно! Если хотите использовать конструктор по умолчанию, то не пишите скобок! Даже если пустые, не пишите!**  
Второй строкой мы создаем переменную **xyz** типа **foo** используя параметр **123**. Тут вызвался второй конструктор нашего класса.

Но я сказал про три конструктора? Вот в чем штука третьего конструктора, конструктора копирования. Рассмотрим пример  
```
void method(foo bar){ ... }

int main(){
  foo bar;
  method(bar);
  return 0;
}
```
Пример уже посложнее предыдущих, но рассмотрим построчно.  
Я расписал в другом файле как объявляются методы 
и вы должны понимать, что сначала мы объявляем метод **method** который ничего не возвращает (*void*). 
В качестве парамтера принимает **объект типа foo**. Я специально отметил что не указатель, а сам **объект**.  
Что происходит дальше. Мы создаем переменнную **bar** с использованием конструктора по умолчанию, 
а затем отправляем ее в качестве параметра в метод **method**. Метод сделает некоторые, 
не важные для повествования, действия, и закончит работу. Все, вроде, прозрачно, но при чем тут конструктор копирования?  
Вот при чем. Нельзя отправить непосредственно сам объект т.к. он фиксирован в памяти, 
а передача в метод будет означать что объект будет перемещен в стеке. Как быть? Вот тут работает **конструктор копирования**.  
Когда мы пишем переменную некого типа (любого), то сначала создается полная, побитовая копия переменной, 
и уже эта копия передается в метод. Популярны ситуации когда идеально точная копия объекта не имеет смысла, так что нам дается шанс
создать свой способ копирования данных с помощью конструктора копирования. После того как метод закончит работу, на копии будет вызван 
деструктор, и сама копия будет удалена из памяти. Все что произойдет с копией в методе никак не затронет оригинальную переменную.  

Другой разговор при использовании **указателя на объект класса**
```
void method(foo* bar) {}

int main(){
  foo* bar = new foo();
  method(bar);
  return 0;
}
```
Тут происходит тоже самое, но уже не будет вызван конструктор копирования. Так как мы передаем **указатель** на объект, 
нам нет необходимости перемещать объект в памяти и, соответственно, копирование не имеет смысла.  
У данного способа есть определенная особенность которую, впрочем, я не могу отнести ни к плюсу, ни к минусу.
То, что произойдет с объектом в методе, отразится на оригинале. Одним из вариантов может быть, например, изменения полей, 
изменение указателя или вовсе его за**нулл**ение. Это может быть полезно при определенном сценарии, но стоит помнить об этом.

##Деструкторы
Звучит страшно. Чем так отличается деструктор от конструктора? 
Из названия понятно что деструктор выполняет роль противоположную конструктуру.
Конструктор вызывается когда объект создается и призван инициализировать его поля, 
а деструктор вызывается когда объект уничтожается и призван уничтожить все что он создал, подмести за собой мусор.  
С конструктором разобрались, либо просто создаем переменную, либо через `new` если используем указатели, но как вызвать деструктор?
Как дать программе понять что мы болше не хотим иметь этот объект в памяти? В случае без указателей объект уничтожается сразу 
как заканчивается его "блок". Будь то `if`, `for`, `while` или некий метод.  
Если мы работали с указателями, то важно использовать антипод слова `new`, ключевое слово `delete`.  
Роль этого слова в том, что бы вызвать деструктор в классе, на который ссылается указатель, а затем освободить всю эту память.
Освобожденная память сразу и с радостью заполонится мусором, а обращение по указателю будет невозможно.
Синтаксис объявления деструктора очень прост:
```
class foo{
  ~foo();
}
```
Название класса с тильдой `~` в начале. Можно вызвать этот вручную, если хочется, но не обязательно.

# Упражнения

1. Создать простой класс с одной **публичной** переменной типа `int`. 
Создать объект этого класса, присвоить полю некоторое значение и успешно вывести его в консоль
2. Создать класс с конструктом, который принимает параметр типа `int`. Этот параметр сохранить в **публичное** поле.
С помощью этого конструктора создать объект класса с уже установленным полем и вывести это поле в консоль.
* Создать класс с **публичным** полем типа `int` и двумя конструкторами. Один конструктор - по умолчанию. 
Он устанавливает полю значение `0`. Второй деструктор принимает параметр типа `int` и устанавливает полю значение параметра.
Далее создать 2 объекта этого класса (каждый разным конструктором) и вывести значения поля обоих объектов.
* Расширить класс из предыдущего упражнения методом `PRINT`, который будет выводить значение поля в консоль. 
Использовать этот метод для вывода значения в консоль.
* Создать объект типа **указатель** на класс из предыдущего упражнения с использованием конструктора с параметром.
Вызвать из этого объекта метод `PRINT`.
* Создать объект типа **указатель** на класс из предыдущих упражнений с использование конструктора с параметрами.
Вызвать метод `PRINT`. Затем удалить этот объект, создать на его месте новый и снова вызвать метод `PRINT`
* То же самое что и в предыдущем упражнении, но с выводом сообщения в консоль `Object Destroyed` при выполнении деструктора класса.