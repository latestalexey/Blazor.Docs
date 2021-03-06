---
title: Начало работы с Blazor
author: danroth27
description: Learn how to get started with the Blazor framework.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/get-started
---
# Начало работы с Blazor

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

## Установка

1. Установите [.NET Core 2.1 SDK (2.1.302)](https://go.microsoft.com/fwlink/?linkid=873092).
1. Установите [Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.7 или новее) с *ASP.NET and web development*.
1. Установите последнюю версию [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) из Visual Studio Marketplace.
1. The Blazor templates on the command-line:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

Чтобы создать свой первый проект используя Visual Studio:

1. Выберите **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.
1. Убедитель что **.NET Core** и **ASP.NET Core 2.1** (или новее) выбранны в верху.
1. Выберите шаблон Blazor и нажмите **OK**.

   ![Диалог нового приложения Blazor](https://msdnshared.blob.core.windows.net/media/2018/07/new-blazor-app-dialog-0.5.0.png)
   
1. Нажмите **Ctrl-F5** чтобы запустить приложение *без отладчика*. Работа с отладчиком произойдёт при нажатии  (**F5**) в настоящее время это не поддерживается.

Вы также можете установить и использовать шаблоны Blazor из командной строки:

```console
dotnet new blazor -o BlazorApp1
cd BlazorApp1
dotnet run
```

Поздравляем! Вы запустили ваше первое Blazor приложение!

![Домашняя страница приложения Blazor](https://msdnshared.blob.core.windows.net/media/2018/04/blazor-bootstrap-4.png)

> [!IMPORTANT]
> Файл *global.json* по умолчанию включенный в шаблоны проекта Blazor, может привести к тому, что проект не будет загружен или запущен, если у вас не установлена версия 2.1.3xx .NET Core SDK. Файл *global.json* привязывает проект к 2.1.3xx версии SDK; поэтому, если у вас нет определенного диапазона версий, проект не загружается и не запускается, даже если у вас установлен новый SDK. Обходной путь - удалить файл *global.json* из проекта или установить версию [2.1.302 .NET Core SDK](https://github.com/dotnet/core/blob/master/release-notes/2.1/2.1.2.md).

## Помощь и обратная связь

Обратная связь особенно важна для нас на этой экспериментальной фазе для Blazor. Если у вас возникли проблемы или у вас возникли вопросы, попробовав Blazor, сообщите нам об этом!

* [Проблемы на GitHub](https://github.com/aspnet/blazor/issues) для любых проблем, с которыми вы сталкиваетесь, или для внесения предложений по улучшению.
* Пообщайтесь с нами и сообществом Blazor в [Gitter](https://gitter.im/aspnet/blazor) если вы в затруднении или хотите поделиться тем, как Blazor работает у вас.

После того, как вы попробовали Blazor, сообщите нам, что вы думаете, перейдя на страницу опроса. Просто нажмите ссылку опроса, показанную на домашней странице приложения при запуске одного из шаблонов проекта Blazor:

![Опрос Blazor](https://msdnshared.blob.core.windows.net/media/2018/05/blazor-survey-new.png)

## Что дальше?

<xref:client-side/blazor/tutorials/first-app>
