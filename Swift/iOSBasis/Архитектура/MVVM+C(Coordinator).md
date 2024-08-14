Ref: https://codeswift.ru/mvvm-coordinator/
Работа координатора заключается в создании всех зависимостей. Например **создает** **ViewController** и **ViewModel**. **Coordinator** передает **ViewModel** в **ViewController**. Coordinator также ответственен за инициализацию API-Сервиса, или другого сервиса и внедряет(**inject**) в ViewModel или ViewController, если нужно.
Пример: 
	Ты - ViewController, Мама - Coordinator.
	Мама(Coordinator) будит тебя (init ViewController), кладет еду(dependencies/services) и твою домашку(dependencies/services) в твой рюкзак(ViewModel) и отправляет в школу.
Код:
	`func goToLogin() {`
		`let vc = LoginViewController()`
		`let vm = LoginViewModel()`
		`vm.apiClient = authApi`
		`vm.authCoordinator = self`
		`vc.viewModel = vm`
		`navigationController.setViewControllers([vc], animated: true)`
	`}`

[Open: Pasted image 20240625141453.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/996db6a31afb4da87659f4d297cc33a9_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/996db6a31afb4da87659f4d297cc33a9_MD5.jpeg)
## Реализация паттерна.
#### 0. Базовый protocol Coordinator
Создаем **базовый шаблон** для любого координатора в нашем приложении
`protocol Coordinator {`
    `var parentCoordinator: Coordinator? { get set }`
    `var children: [Coordinator] { get set }`
    `var navigationController: UINavigationController { get set }`
    
    `func start()`
`}`
#### **1. Создаем AppCoordinator**
`class AppCoordinator: Coordinator {`
    `var parentCoordinator: Coordinator?`
    `var children: [Coordinator] = []`
    `var navigationController: UINavigationController`
    
    `init(navCon : UINavigationController) {`
        `self.navigationController = navCon`
    `}`
    
    `func start() {`
        `print("App Coordinator start")`
    `}`
`}`
Функция start() содержит начальную операцию или флоу в приложении.
#### **2. Удаление SceneDelegate и настройка AppDelegate

**Scene** можно рассматривать как **раздел**, фичу **или** **use-case** внутри аппки. Небольшой, но внутренне законченный, содержащий часть всего приложения кусок. Насколько большой делать Scene зависит от тебя. 
Обычно это коллекция из 1-4 экранов, которые связанны одной логической целью. Например flow логина, процесс загрузки картинки или воронка продаж.
Удаляем SceneDelegate, переносим логику создания window в AppDelegate.
Удаляем все ненужное из AppDelegate, остается только.
**AppDelegate**
	`var window: UIWindow?`
    `var appCoordinator: AppCoordinator?`

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        window = UIWindow(frame: UIScreen.main.bounds)
        let navigationCon = UINavigationController.init()
        appCoordinator = AppCoordinator(navCon: navigationCon)
        appCoordinator!.start()
        window!.rootViewController = navigationCon
        window!.makeKeyAndVisible()
        return true
    }
[Open: Pasted image 20240625161426.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4aec00951db84f35636ba45dbd82a64c_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4aec00951db84f35636ba45dbd82a64c_MD5.jpeg)
Удаляем сториборд, как обычно из info.plist и таргета + удаляем Application Scene Manifest полностью.
[Open: Pasted image 20240625165752.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/effe093e48e9c15f9b0cccb38ebd454f_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/effe093e48e9c15f9b0cccb38ebd454f_MD5.jpeg)

### Итоговый вид AppDelegate:
[Open: Pasted image 20240625175739.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/2484436a319e83fad864ec42563f4863_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/2484436a319e83fad864ec42563f4863_MD5.jpeg)
### MainViewModel
[Open: Pasted image 20240625175856.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4f7e72845da2e4f138383e58e3e98555_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/4f7e72845da2e4f138383e58e3e98555_MD5.jpeg)
У нас есть сущность координатора, с помощью которой делаются все переходы.
### MainView
[Open: Pasted image 20240625180024.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/202b877102343ab5b186dfd03f03705e_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/202b877102343ab5b186dfd03f03705e_MD5.jpeg)
Хранит модель, которая кладется в Coordinate'оре. Бинд на кнопку с функцию goToNext вызывает метод viewModel'а goToLogin(), которая вызывает функцию координатора.
### Coordinator
[Open: Pasted image 20240625190009.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/d7b0a82640dce9d28bdea3898df804f3_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/d7b0a82640dce9d28bdea3898df804f3_MD5.jpeg)
### LoginViewModel
[Open: Pasted image 20240625190057.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/0bb8551b94ca3f19d1ba15a05de649c8_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/0bb8551b94ca3f19d1ba15a05de649c8_MD5.jpeg)
LoginViewController аналогичен Main'у.
### Улучшение
Добавить протокол, в котором описаны все методы viewModel'и
Есть небольшие изменения, но в целом концепт остался тем же.

Так что я прочитал очень полезный ответ от [Russ Warwick](https://medium.com/@russwarwick), поместить всю кучу функций во внутрь протокола и реализовать их в координаторе лучше чем передавать экземпляр координатора во **ViewModel**.

Так что я добавил протокол **LoginNavigator**. Имея это, **VIewModel** ничего не знает о координаторе. Это лучший путь реализации и координатор может быть легко изменен
[Open: Pasted image 20240625191111.png](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/f231d43cb4f26ddad363589121ea68ad_MD5.jpeg)
![](Swift/iOSBasis/%D0%90%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%D1%8B/f231d43cb4f26ddad363589121ea68ad_MD5.jpeg)