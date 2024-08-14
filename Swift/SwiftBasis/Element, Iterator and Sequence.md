for работает под капотом таким образом, Iterator.next() возвращает следующий элемент, если он есть мы идем
[Open: Pasted image 20240509192608.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/e15aca0b8dcff2ad29816411fea400fe_MD5.jpeg)
![](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/e15aca0b8dcff2ad29816411fea400fe_MD5.jpeg)
[Open: Pasted image 20240509192618.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/3af8742c342e29a525de38bbe3ed6b8b_MD5.jpeg)
 
Protocol Iterator и Sequence тесно связаны, Sequence позволяет получать доступ создавая Итератор.
**Тип Element.**
Элемент это тип возвращаемого из итерируемой коллекции, подписанной на Sequence элемента.
Sequence has an **Element** and **Iterator** associated types, declared as:
[Open: Pasted image 20240509193029.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/5b977f3b88a6a669fbf922484d5ef4e2_MD5.jpeg)
![](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/5b977f3b88a6a669fbf922484d5ef4e2_MD5.jpeg)
Но в протоколе Iterator тоже есть элемент, по сути он всю работу и делает. 

### Создание своей map'ы
[Open: Pasted image 20240509193508.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/081e9a3a1395e35b59e7213f718c5621_MD5.jpeg)
![](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/081e9a3a1395e35b59e7213f718c5621_MD5.jpeg)

Делаем extension для протокола Sequence, чтобы можно было использовать на все типы коллекций, подписанные на этот протокол, дальше пишим функцию с дженерик параметром, которая возвращает массив таких дженериков. В параметр функции передаем closure'у , в которой на вход передается Element - это associatedType итератора, то есть типа эоемента в коллекции, а возвращаем мы наш один элемент-дженерик. После создаем переменную результат, ее мы и вернем, в форе заполняем ее с помощью обычного append'a и использования переданной кложуры.
