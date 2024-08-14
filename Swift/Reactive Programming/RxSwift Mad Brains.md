### Subject
это сущность, которая является и Observer, и Observable одновременно. То есть мы можем на эту сущность подписаться и получить какие-то события, либо загнать туда какие-то события такими методами , как .onNext, .onCompleted и т.д.
#### Типы Subject'ов
##### PublishSubject
Подписчики получают только новые события, которые произошли уже после их подписки.
[Open: Pasted image 20240813130717.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/b06960a8b87a5e16fd47cb922e460d14_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/b06960a8b87a5e16fd47cb922e460d14_MD5.jpeg)
Синие стрелки показывают когда мы подписались на событие, поэтому в случае нижний подписки мы увидели только синее событие. А в случае верхней все.

##### BehaviorSubject
Подписчики получают новые события + еще одно последнее событие.
[Open: Pasted image 20240813130941.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/ddd5b2179b977d572d1d8d650577489f_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/ddd5b2179b977d572d1d8d650577489f_MD5.jpeg)
Буфер хранит последнее значение. В случае верхней подписки розовое значие, в случае нижней - зеленое.
##### ReplaySubject
Тоже самое, что и BehaviourSubject, только буфер не на один элемент, а на несколько последних событий.
[Open: Pasted image 20240813131441.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/c5ce25285872a95e9749871c4c800d84_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/c5ce25285872a95e9749871c4c800d84_MD5.jpeg)
##### AsyncSubject
Непонятно зачем нужен. Подписчики получают самое последнее событие после завершения последовательности. после метода .onCompleted()
##### Hot/Cold Observables
Здесь два раза подписались, запрос произошел два раза это холодная последовательность
[Open: Pasted image 20240813133254.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/5012cbf55d8a9d5f87508ea4774ffef1_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/5012cbf55d8a9d5f87508ea4774ffef1_MD5.jpeg)
Добавили .share() и запрос выполнился один раз
[Open: Pasted image 20240813133411.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/0b72e890b72e67afa4dd7511354ea6e0_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/0b72e890b72e67afa4dd7511354ea6e0_MD5.jpeg)
Разница в типах последовательностей.
###### Cold Observable
Холодная последовательность начинает эмитить события только при подписке. При каждой подписке события начинают эмититься заново.
Холодными являются:
1) Любые Observable из коробки
2) Observable созданные через create()
Холодный Observer без подписки не заэмитит события, принтов не будет, как только добавляем подписку - принты появляютс
[Open: Pasted image 20240813165419.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4fbd094074e80ded141ca18c69223fc2_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4fbd094074e80ded141ca18c69223fc2_MD5.jpeg)
[Open: Pasted image 20240813165350.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/76340cfa7034fd39a32d18c9e381fbed_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/76340cfa7034fd39a32d18c9e381fbed_MD5.jpeg)

[Open: Pasted image 20240813130505.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/45b9b7bfccaa0d9593b4e863037341e3_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/45b9b7bfccaa0d9593b4e863037341e3_MD5.jpeg)
Принты:
[Open: Pasted image 20240813165509.png](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/3ec1e5cbebfac42d16c2635348e185f7_MD5.jpeg)
![](Swift/Reactive%20Programming/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/3ec1e5cbebfac42d16c2635348e185f7_MD5.jpeg)

###### Hot Observable
Горячая последовательность эмитит события вне зависимости от того, подписан на нее кто-либо или нет. При новой подписке события не начинают эмититься заново.
Горячими являются:
1) все Subject
2) Connectable Observable
Connectable Observable - observable, который начинает эмитить элементы после того, как к нему был применен метод connect()
После получения Connectable Observable



Функции
.debug() - отображает все логи, которые происходят с подписчиком
.publish() - позволяет получить из обычного Observable горячий. Под капотом он работает через PublishSubject, то есть транслирует всем подписчикам те события, которые происходят после их подписки.
.replay(bufferSize:) - метод позволяет получить из обычного Observable горячий. Под капотом работает через ReplaySubject, то есть транслирует всем подписчикам не только новые события, но еще и некоторое количество последних.>
.replayAll - делает то же самое, что и replay, но имеет бесконечный буфер событий