Система приоритезации задач Apple'а, для того, что система проставила необходимые приоритеты задачам
`enum QualityOfService: Int {`
	`case userInteractive` -- используется для задач взаимодействия с пользователем, анимация или обновления UI
	`case userInitiated` -- задачи требуют немедленного результата в ответ на действия юзера, сохранение документа или выполнение действия по тапу
	`case utility` -- используется для задач, в которых не требуется получить результат немедленно, например загрузка данных
	`case background` -- используется для задач, которые не видны пользователю, синхронизация или бэкап
	`case 'default'` -- значение по умолчанию
`}`
#### pthread qos
`var thread = pthread_t(bitPattern: 0)
`var attr = pthread_attr_t()` 

`pthread_attr_init(&attr)`
`pthread_attr_set_qos_class_np(&attr, QOS_CLASS_USER_INITIATED, 0)` -- установка qos для pthread'а
`pthread_create(&thread, &attr, { pointer in` 
	`print("test")`
	`pthread_attr_set_qos_class_self_np(&attr, QOS_CLASS_BACKGROUND, 0)` -- изменение qos'а pthread'а 
	`return nil`
`}, nil)`

#### Thread qos
`var nsthread = Thread(block: {` -- создание потока, в конструкте в block мы передает кложуру, в данном случае просто print
	`print("test")`
	`print(qos_class_self())` -- текущее значение qos'а этого потока
`})`
`nsthread.qualityOfservice = .userInteractive` -- установка qos 
`nsthread.start()` -- запуск

## НО,  userInteractive qos не может обновлять UI, он используется для обработки данных, которые нужно UI'ю ASAP!
Dispatch_get_global_queue will always give you the global concurrent queue. You cannot update the ui in that queue as in any queue besides the main.

As with User interactive Qos, it is used for work that is critical to the ui and the results are needed as fast as possible. 

A good example is that you have an image that needs processing. You can process the image in global_queue with User-Interactive Qos and when it's done you set the result image in your imageView (this is done in main queue).