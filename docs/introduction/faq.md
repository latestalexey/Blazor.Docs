---
title: Часто задаваемые вопросы (FAQ) о Blazor
author: guardrex
description: Найдите ответы на часто задаваемые вопросы о Blazor.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/introduction/faq
---
# Часто задаваемые вопросы (FAQ) о Blazor

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

## Что такое Blazor?

Blazor - это одностраничная платформа веб-приложений, построенная на .NET, которая работает в браузере с помощью WebAssembly.

## Я новичок в .NET. Что такое .NET?

[.NET](https://www.microsoft.com/net) это бесплатная, кросс-платформенная платформа для разработчиков с открытым исходным кодом для создания множества различных типов приложений (настольных, мобильных, игр, сетевых приложений). .NET включает управляемую среду выполнения, стандартный набор [библиотек](https://docs.microsoft.com/dotnet/api), и поддержку многих современных языков программирования: [C#](https://docs.microsoft.com/dotnet/csharp/), [F#](https://docs.microsoft.com/dotnet/fsharp/) и [VB](https://docs.microsoft.com/dotnet/visual-basic/). Вы можете [начать работать с .NET за 10 минут](https://www.microsoft.com/net/learn/get-started/windows).

## Почему я должен использовать .NET для веб-разработки?

Использование .NET в браузере дает много преимуществ, которые могут помочь сделать веб-разработку проще и продуктивнее:

* **Стабильность и согласованность**: .NET обеспечивает стандартизированные рамки программирования на всех платформах, они являются стабильными, многофункциональными и простыми в использовании.
* **Современные инновационные языки**: Языки .NET такие как [C#](https://docs.microsoft.com/dotnet/csharp/) и [F#](https://docs.microsoft.com/dotnet/fsharp/) позволяют программировать с удовольствием, так как они постоянно улучшаются и поддерживают всё больше необходимого современного функционала.
* **Лучшие инструменты в отрасли**: Семейство продуктов [Visual Studio](https://www.visualstudio.com/) обеспечивает фантастическую разработку используя .NET на всех платформах в Windows, Linux и MacOS.
* **Скорость и масштабируемость**: .NET имеет сильную историю производительности, надежности и безопасности для разработки приложений. Использование .NET в качестве решения для full-stack разработки упрощает создание быстрых, надежных и безопасных приложений.

## Как вы можете запустить .NET в веб-браузере?

Запуск .NET в браузере стал возможным благодаря относительно новой стандартизированной веб-технологии, называемой [WebAssembly](http://webassembly.org/). WebAssembly - это "переносимый, эффективный как по размеру так и по времени загрузки, подходящий для компиляции в веб". Код, скомпилированный в WebAssembly, может запускаться в любом браузере с нативными скоростями. Чтобы запустить исполняемые файлы .NET в веб-браузере, мы используем среду выполнения .NET (конкретно [Mono](http://www.mono-project.com/news/2017/08/09/hello-webassembly/)) которая была скомпилирована для WebAssembly.

## Blazor скомпилирует мое приложение .NET в WebAssembly?

Нет, приложение Blazor состоит из обычных скомпилированных сборок .NET, которые загружаются и запускаются в веб-браузере с использованием среды выполнения .NET на основе WebAssembly. Только сама среда выполнения .NET компилируется в WebAssembly. Тем не менее, поддержка полной [статической досрочной (AoT) компиляции](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/) приложения в WebAssembly может быть добавлена в дальнейшем.

## Разве размер приложения для загрузки не будет огромным, если он также включает среду выполнения .NET?

Не обязательно. Среда выполнения .NET может иметь разные размеры. Ранние прототипы Blazor использовали компактную среду выполнения .NET (включая сборку, сборку мусора, потоковую обработку), которая составляла всего лишь 60 Кбайт WebAssembly. Теперь Blazor работает на Mono, который в настоящее время значительно больше. Тем не менее, возможностей для оптимизации размера достаточно, включая слияние и обрезку исполняемых файлов. Другие потенциальные ограничения размера загрузки могут быть решены через кэширование и использование CDN.

## Какие функции поддерживает Blazor?

Blazor будет поддерживать все функции современного фреймворка для одностраничных приложений:

* Компонентная модель для создания пользовательского интерфейса
* Маршрутизация
* Макеты
* Формы и их валидация
* Внедрение зависимости
* Взаимодействие с JavaScript
* Перезагрузка в браузере во время разработки
* Обработка на стороне сервера
* Полная отладка .NET как в браузерах, так и в среде IDE
* IntelliSense и различные инструменты
* Возможность запуска на старых (не WebAssembly) браузерах через asm.js
* Публикация и настройка размера приложения

## Можно ли использовать Blazor без запуска .NET на сервере?

Да, приложение Blazor можно развернуть как набор статических файлов без необходимости поддержки .NET на сервере.

## Могу ли я использовать Blazor с ASP.NET Core на сервере?

Да! Blazor дополнительно интегрируется с [ASP.NET Core](https://docs.microsoft.com/aspnet/core) чтобы обеспечить бесшовное и последовательное решение для full-stack веб-разработки.

## Является ли Blazor .NET-портом существующего JavaScript фреймворка?

Blazor - это *новый фреймворк*, вдохновлённый существующими современными фреймворками для одностраничных приложений, такими как React, Angular и Vue.

## Как я могу попробовать Blazor?

Чтобы создать свое первое веб-приложение Blazor, ознакомьтесь с нашим [руководством по началу работы](xref:client-side/blazor/get-started).

## Почему Blazor является "экспериментальным" проектом?

Blazor - экспериментальный проект, потому что еще есть много вопросов, ответив на которые можно будет говорить о его жизнеспособности и целесообразности. Целью этой начальной экспериментальной фазы является:

* Проработать технические вопросы.
* Оценить интерес и получить обратную связь.

Мы оптимистично относимся к будущему Blazor; но в настоящее время он не является законченным продуктом и должен считаться пре-альфой.

## Этот снова Silverlight?

Нет, Blazor - это веб-платформа .NET, основанная на HTML и CSS, которая работает в браузере с использованием открытых веб-стандартов. Он не требует плагина, и он работает на мобильных устройствах и старых браузерах.

## Does Blazor use XAML?

No, Blazor is a web framework based on HTML, CSS, and other standard web technologies.

## Поддерживается ли WebAssembly во всех браузерах?

Да, WebAssembly добилась кросс-браузерного консенсуса и [все современные браузеры теперь поддерживают WebAssembly](https://caniuse.com/#search=webassembly).

## Работает ли Blazor в мобильных браузерах?

Да, [современные мобильные браузеры также поддерживают WebAssembly](https://caniuse.com/#search=webassembly).

## Что относительно старых браузеров, которые не поддерживают WebAssembly? Например, работает ли Blazor в IE?

Для старых браузеров, которые не поддерживают WebAssembly, Blazor возвращается к использованию среды выполнения .NET asm.js. Использование asm.js медленнее и имеет больший размер загрузки, но все еще довольно функционально.

## Могу ли я использовать библиотеки .NET Standard с Blazor?

Да, среда выполнения .NET, используемая для Blazor, поддерживает .NET Standard 2.0. API, которые не поддерживаются в браузере, бросают *Not Supported* исключения.

## Вам не нужны функции, такие как сбор мусора и потоки, добавленные в WebAssembly?

Нет, WebAssembly в текущем состоянии достаточно. Среда выполнения .NET обрабатывает собственные проблемы сбора мусора и потоков.

## Могу ли я использовать существующие библиотеки JavaScript в Blazor?

Да, приложения Blazor могут обращаться к JavaScript через API-интерфейсы JavaScript.

## Могу ли я получить доступ к DOM из приложения Blazor?

Вы можете получить доступ к DOM через интерфейс JavaScript из кода .NET. Однако Blazor - это компонентная инфраструктура, которая минимизирует необходимость прямого доступа к DOM.

## Почему Моно? Почему не .NET Core или CoreRT?

[Mono](http://www.mono-project.com/) является спонсируемой Microsoft реализация .NET Framework  с открытым исходным кодом. Моно используется в [Xamarin](https://www.xamarin.com/) для создания клиентских приложений для Android, iOS и macOS. Xamarin также используется [Unity](https://unity3d.com/) для разработки игр. Команда Microsoft Xamarin [объявили о своих планах](http://www.mono-project.com/news/2017/08/09/hello-webassembly/) добавить поддержку для WebAssembly в Mono, и они добились устойчивого прогресса ([Mono и WebAssembly - Обновления на статической компиляции 1/16/2018](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/)). Поскольку Blazor - это веб-интерфейс пользовательского интерфейса на стороне клиента, ориентированный на WebAssembly, Mono - лучший выбор.

[.NET Core](https://www.microsoft.com/net/learn/get-started/windows) в основном используется для серверных приложений и для кросс-платформенных консольных приложений. .NET Core можно использовать для создания бэкэнда для ASP.NET Core приложения Blazor, но не для создания самого клиентского приложения. [CoreRT](https://github.com/dotnet/corert) - это среда .NET Core, оптимизированная для компиляции AoT; и, хотя у него есть придыхание WebAssembly, проект по-прежнему остается незавершенным, его нельзя использовать в `production` проектах.

## Откуда взялось название

Blazor активно использует [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor?view=aspnetcore-2.1), синтаксис разметки для HTML и C#. **Browser + Razor = Blazor!** Когда произносится, это также название шикарной куртки, которую носят хипстеры, которые имеют отличный вкус в моде, стиле и языках программирования.
