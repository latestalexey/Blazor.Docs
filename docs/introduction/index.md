---
title: Введение в Blazor
author: guardrex
description: Learn how Blazor runs in the browser to execute C#/Razor code with WebAssembly and the Mono runtime in this introduction.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/introduction/index
---
# Введение в Blazor

От [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27) и [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

Blazor - это экспериментальный .NET фреймворк использующий C#/Razor и HTML, который работает в браузере с помощью WebAssembly. Blazor предоставляет все преимущества создания .NET приложений на клиенте и, при необходимости, на сервере.

## Зачем использовать .NET в браузере?

За последние годы веб-разработка во многих отношениях улучшилась, но создание современных веб-приложений по-прежнему не простая задача. Использование .NET в браузере дает много преимуществ, которые могут помочь сделать веб-разработку проще и продуктивнее:

* **Стабильность и согласованность**: .NET обеспечивает стандартизированные рамки программирования на всех платформах, они являются стабильными, многофункциональными и простыми в использовании.
* **Современные инновационные языки**: Языки .NET постоянно совершенствуются добавляя новые инновационные функции.
* **Лучшие инструменты в отрасли**: Семейство продуктов Visual Studio обеспечивает фантастическую разработку используя .NET на всех платформах в Windows, Linux и MacOS.
* **Скорость и масштабируемость**: .NET имеет сильную историю производительности, надежности и безопасности для разработки приложений. Использование .NET в качестве решения для full-stack разработки упрощает создание быстрых, надежных и безопасных приложений.
* **Full-stack разработка, которая использует существующие навыки и наработки**: Разработчики C#/Razor используют их C#/Razor навыки и наработки для написания клиентских приложений и логики обмена данных с сервером.
* **Широкая поддержка браузеров**: Blazor работает на .NET с использованием открытых веб-стандартов в браузере, без плагинов и без перекодирования кода. Он работает во всех современных веб-браузерах, включая мобильные.

## Как Blazor запускает .NET в браузере

Выполнение .NET-кода внутри веб-браузеров стало возможным благодаря относительно новой технологии, [WebAssembly](http://webassembly.org) (сокращенно *wasm*). WebAssembly является открытым веб-стандартом и поддерживается в веб-браузерах без плагинов. WebAssembly - это компактный формат байт-кода, оптимизированный для быстрой загрузки и максимальной скорости выполнения.

Код WebAssembly позволяет получить доступ ко всем функциям браузера через интерфейс JavaScript. В то же время код WebAssembly работает в той же доверенной изолированной среде, что и JavaScript, для предотвращения вредоносных действий на клиентской машине.

Когда приложение Blazor создается и запускается в браузере:

1. Файлы C# кода и файлы Razor скомпилированы в .NET сборки.
1. Сборки и среда выполнения .NET загружаются в браузер.
1. Blazor использует JavaScript для загрузки среды выполнения .NET и настраивает среду выполнения для загрузки необходимых ссылок на сборку. Обработка объектной модели документа (DOM) и вызовы API браузера обрабатываются исполняемой средой Blazor с помощью JavaScript функционала.

Для поддержки старых браузеров, которые не поддерживают WebAssembly, Blazor использует [asm.js](https://wikipedia.org/wiki/Asm.js) на основе .NET.

## Компоненты Blazor

Приложения Blazor создаются с помощью *компонентов*. Компонент это кусок пользовательского интерфейса (UI), например страница, диалог или форма ввода данных. Компоненты могут быть вложенными, повторно использованными и разделяемыми между несколькими проектами.

В Blazor, компонент это .NET класс. Класс может быть написан непосредственно, как C# класс (*\*.cs*), или чаще в виде страницы разметки Razor (*\*.cshtml*).

[Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor) является синтаксисом для комбинирования разметки HTML с кодом C#. Razor разработан для повышения продуктивности разработчиков, позволяя разработчику переключаться между разметкой и C# кодом в том же файле с помощью поддержки [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense). Следующая разметка является примером базового настраиваемого диалогового компонента в файле Razor (*DialogComponent.cshtml*):

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Когда этот компонент используется в другом месте приложения, IntelliSense ускоряет разработку с синтаксисом и завершением параметров.

Компоненты могут быть:

* Вложенными.
* Созданными с помощью Razor (*\*.cshtml*) или кода C# (*\*.cs*).
* Общими для библиотеки классов.
* Юнит-тестирования без необходимости использования DOM браузера.

## Инфраструктура

Blazor предлагает основные возможности, требуемые большинством приложений, в том числе:

* Макеты (Layouts)
* Маршрутизация (Routing)
* Внедрение зависимости (Dependency injection)

Все эти функции являются необязательными. Когда одна из них не используется в приложении, реализация удаляется из приложения при публикации IL-линкером.

## Совместное использование кода и .NET Standard

Blazor приложения могут использовать существующие [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) библиотеки. .NET Standard является формальной спецификацией .NET API, которые являются общими для реализации .NET. Blazor поддерживает .NET Standard 2.0 или выше. API, которые не применимы в веб-браузере (например, доступ к файловой системе, открытие сокета, потоков и другие функции) выбрасывают [PlatformNotSupportedException](https://docs.microsoft.com/dotnet/api/system.platformnotsupportedexception). .NET Standard библиотеки классов могут быть разделены между кодом сервера и приложением в браузере.

## Взаимодействие с JavaScript

Для приложений, которым требуются сторонние библиотеки JavaScript и вызовы API браузера, WebAssembly предоставляет взаимодействия с JavaScript. Blazor может использовать любую библиотеку или API, которые может использовать JavaScript. Код C# может вызывать код JavaScript, а код JavaScript может вызывать код C#. Для получения дополнительной информации смотрите [Взаимодействие с JavaScript](xref:client-side/blazor/javascript-interop).

## Оптимизация

Для клиентских приложений размер загружаемых приложений является критическим. Blazor оптимизирует размер полезной нагрузки, чтобы сократить время загрузки. Например, неиспользуемые части .NET сборок удаляются во время процесса сборки, HTTP-ответы сжимаются, а среда выполнения и сборки .NET кэшируются в браузере.

## Развёртывание

Используйте Blazor для создания автономного клиентского приложения или full-stack приложения ASP.NET Core, которое содержит как серверные, так и клиентские приложения:

* **Автономное клиентское приложение**, приложение Blazor компилируется в каталог *dist* который содержит только статические файлы. Файлы могут быть размещены в Azure App Service, GitHub Pages, IIS (настроен как статический файловый сервер), серверах Node.js и многих других серверах и службах. В таком виде .NET не требуется на целеном сервере.
* В **full-stack ASP.NET Core приложении**, код можно разделить между серверными и клиентскими приложениями. Полученное в результате серверное приложение ASP.NET Core обслуживает клиентский интерфейс Blazor на стороне клиента и другие конечные точки API на стороне сервера, может быть построено и развернуто на любом облачном или локальном хосте, поддерживаемом ASP.NET Core.
