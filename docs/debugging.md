---
title: Отладка Blazor
author: danroth27
description: Узнайте, как отлаживать приложения Blazor.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/25/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/debugging
---
# Отладка Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

Blazor имеет некоторую *очень раннюю* поддержку для отладки приложений на стороне клиента, работающих на WebAssembly в Chrome. Хотя эта первоначальная поддержка отладки очень ограничена и неполирована, она показывает общую инфраструктуру отладки.

Отладка клиентского приложения Blazor в Chrome:

* Создайте приложение Blazor в конфигурации `Debug` (по умолчанию для неопубликованных приложений).
* Запустите приложение Blazor в Chrome.
* Переведите фокус клавиатуры не Blazor приложение (а не на панель инструментов разработчика, которую вы, вероятно, должны закрыть чтобы не путаться), выберите следующую комбинацию клавиш:
  - `Shift+Alt+D` на Windows/Linux
  - `Shift+Cmd+D` на macOS

Запустите Chrome с удаленной отладкой, чтобы отладить приложение Blazor. Если удаленная отладка отключена, страница Chrome создается с ошибкой. Страница ошибки содержит инструкции по запуску Chrome с открытым отладочным портом, чтобы прокси-сервер отладки Blazor мог подключиться к приложению. *Закройте все экземпляры Chrome* и перезапустите Chrome в соответствии с инструкциями.

![Страница ошибки отладки Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

После того, как Chrome запустится с удаленной отладкой, откроется новая вкладка отладчика. Через мгновение вкладка *Sources* отображает список сборок .NET в приложении. Разверните каждую сборку и найдите исходные файлы *.cs*/*.cshtml*, доступные для отладки. Установите точки останова, вернитесь на вкладку приложения и удалите точки останова. После достижения точки останова, используейте (`F10`) для выполнения по шагам или (` F8`) для возврата работы в обычный режиме.

![Отладка Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor предоставляет прокси-сервер отладки, который реализует [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) и дополняет протокол информацией, специфичной для .NET. Когда нажато сочетание клавиш для отладки, Blazor указывает Chrome DevTools на прокси. Прокси подключается к окну браузера, который вы хотите отлаживать (отсюда необходимость включения удаленной отладки).

Возможно, вам интересно, почему мы не просто используем карты ресурсов браузера. Карты ресурсов позволяют браузеру сопоставлять скомпилированные файлы с исходными файлами. Однако Blazor не сопоставляет C# напрямую с JS/WASM (по крайней мере, пока). Вместо этого Blazor интерпретирует IL в браузере, поэтому карты ресурсов не актуальны.

Обратите внимание, что возможности отладчика **очень ограничены.** Вы можете в настоящее время только:

* Выполнение по шагам в текущем методе (`F10`) или продолжить выполнение в обычном режиме (`F8`).
* На экране *Locals* получать значения любых локальных переменных типа `int`, `string` и `bool`.
* Просматривать стек вызовов, включая цепочки вызовов, идущие от JavaScript в .NET и от .NET в JavaScript.

Вы *не можете*:

* Переходить в дочерние методы (`F11`).
* Получать значения любых переменных, которые не являются `int`, `string` или `bool`.
* Получать значения любых свойств или полей класса.
* Наводить указатель мыши на переменные, чтобы увидеть их значения.
* Вычислять выражения в консоли
* Перейдить по асинхронным вызовам.
* Выполнять большинство других обычных сценариев отладки.

Разработка дальнейших сценариев отладки - это постоянный фокус инженерной команды.
