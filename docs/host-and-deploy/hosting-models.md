---
title: Blazor модели хостинга
author: danroth27
description: Описание клиентских и серверных моделей хостинга Blazor.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/host-and-deploy/hosting-models
---
# Blazor модели хостинга

От [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

Blazor - это веб-инфраструктура на стороне клиента, предназначенная, в первую очередь, для запуска в браузере среды выполнения .NET на основе WebAssembly. Blazor поддерживает несколько моделей хостинга, в том числе и вне процесса хостинга моделей, где логика компонент Blazor работает отдельно от потока пользовательского интерфейса. Независимо от модели хостинга, приложение Blazor и модели компонентов остаются *одинаковыми*. В этой статье обсуждаются доступные модели хостинга для Blazor.

## Клиентская (внутрипроцессная) модель хостинга

Основная модель хостинга для Blazor работает на стороне клиента в браузере. В этой модели приложение Blazor, его зависимости и среда выполнения .NET загружаются в браузер, а приложение выполняется непосредственно в потоке пользовательского интерфейса браузера. Все обновления пользовательского интерфейса и обработка событий происходят в одном процессе. Файлы приложения могут быть развернуты как статические файлы с использованием любого веб-сервера (Смотрите [Хостинг и развертывание](xref:client-side/blazor/host-and-deploy/index)).

![Blazor на стороне клиента](https://user-images.githubusercontent.com/1874516/43042852-998bb680-8d3b-11e8-9d39-adf8d3d77360.png)

Чтобы создать приложение Blazor с использованием модели хостинга на стороне клиента, используйте шаблоны проектов "Blazor" или "Blazor (ASP.NET Core Hosted)" (`blazor` или `blazorhosted` шаблон при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new) в командной строке). Включенны *blazor.webassembly.js* скрипты:

* Загрузка среды выполнения .NET, приложения и ее зависимостей.
* Инициализация среды выполнения для запуска приложения. 

Преимущества клиентской модели хостинга:

* Нет зависимостей на стороне сервера .NET
* Богатый интерактивный интерфейс
* Полное использование клиентских ресурсов и возможностей
* Разгрузка задач с сервера на клиент
* Поддержка автономных сценариев

Недостатком модели хостинга на стороне клиента является:

* Ограничено возможностями браузера
* Требуется более надежное клиентское оборудование и программное обеспечение (например, поддержка WebAssembly)
* Увеличенное время загрузки и время загрузки приложения
* Менее зрелая среда выполнения .NET и инструментальная поддержка (например, ограничения в поддержке и отладке .NET Standard)

> [!NOTE]
> Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template for creating a Blazor app that runs on WebAssembly and is hosted on an ASP.NET Core server. The ASP.NET Core app serves the Blazor app to clients but is otherwise a separate process. The client-side Blazor app can interact with the server over the network using Web API calls or SignalR connections.

> [!IMPORTANT]
> If a client-side Blazor app is served by an ASP.NET Core app hosted as an IIS sub-app, it's important to disable the inherited ASP.NET Core Module handler. Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> Set the app base path in the Blazor app's *index.html* file to the IIS alias used when configuring the sub-app in IIS. For more information, see [App base path](xref:client-side/blazor/host-and-deploy/index#базовый-путь-приложения).

## Серверная хостинг модель

В серверной модели хостинга Blazor выполняется на сервере из приложения ASP.NET Core. Обновления пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через соединение SignalR.

![Blazor на стороне сервера](https://user-images.githubusercontent.com/1874516/43042867-eaa8bb76-8d3b-11e8-8f1d-60768f86f710.png)

Чтобы создать приложение Blazor с использованием серверной модели хостинга, используйте шаблон "Blazor (Server-side on ASP.NET Core)" (`blazorserver` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new) в командной строке). В приложении ASP.NET Core используется серверное приложение Blazor и настраивается конечная точка SignalR, к которой клиенты подключаются. Приложение ASP.NET Core ссылается на класс Blazor `Startup` для добавления серверных служб Blazor и добавления приложения Blazor к конвейеру обработки запросов:

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
        services.AddServerSideBlazor<App.Startup>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
        app.UseServerSideBlazor<App.Startup>();
    }
}
```

Сценарий *blazor.server.js* устанавливает соединение с клиентом. Обязанностью приложения является сохранение и восстановление состояния приложения по мере необходимости (например, в случае потери сетевого подключения).

Преимущества серверной модели хостинга:

* Вы все равно можете написать все свое приложение на .NET и C#, используя модель компонента Blazor.
* Ваше приложение по-прежнему обладает богатым интерактивным восприятием и позволяет избежать ненужных обновлений страницы.
* Размер загружаемого приложения значительно меньше, а начальное время загрузки приложения намного быстрее.
* Логика компонента Blazor может в полной мере использовать возможности сервера, в том числе использование любых совместимых с .NET Core API.
* Поскольку вы работаете на .NET Core на сервере, существующие инструменты .NET, такие как отладка, работают так, как ожидалось.
* Хостинг на стороне сервера работает с тонкими клиентами (например, браузерами, которые не поддерживают WebAssembly и устройствами с ограниченными ресурсами).

Недостатки модели хостинга на стороне сервера:

* Задержка: каждое взаимодействие с пользователем теперь связано с сетевым хостом.
* Нет автономной поддержки: если соединение с клиентом пропадает, приложение перестает работать.
* Масштабируемость: сервер должен управлять несколькими клиентскими соединениями и управлять состоянием клиента.
