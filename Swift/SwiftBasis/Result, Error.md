Result - это enum, у которого может быть два кейса, с хранимыми значениями(associated value), для succes'a и failure'а. succes хранит значение для успеха, а failure значение ошибки: подписан на протокол Error
[Open: Pasted image 20240509194704.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/f8bd08e146cde96a376353829c8f7c4b_MD5.jpeg)
![](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/f8bd08e146cde96a376353829c8f7c4b_MD5.jpeg)
Success и Failure - это типы, enum с дженериками
[Open: Pasted image 20240509210624.png](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/9a45cc2deeeb179d672944ae5e09c659_MD5.jpeg)
![](Swift/SwiftBasis/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/9a45cc2deeeb179d672944ae5e09c659_MD5.jpeg)
Создали енум ошибок сети, дальше хендлим их в нашей функции и эскейпинг кложура возвращает ошибки.
