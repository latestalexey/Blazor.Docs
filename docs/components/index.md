---
title: Компоненты Blazor
author: guardrex
description: Узнайте, как создавать и использовать компоненты Blazor, основные строительные блоки приложений Blazor, предоставляемые скомпилированными файлами Razor или C#.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/03/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/blazor/components/index
---
# Конпоненты Blazor

От [Luke Latham](https://github.com/guardrex) и [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazor-preview-notice.md)]

[Посмотреть или скачать примеры кода](https://github.com/aspnet/Blazor.Docs/tree/master/docs/common/samples/) ([как скачать](xref:client-side/blazor/index#просмотр-и-загрузка-примеров)). Смотрите также [Начать](xref:client-side/blazor/get-started), для начала работы с Blazor.

Приложения Blazor создаются с использованием *компонентов*. Компонент представляет собой автономный фрагмент пользовательского интерфейса (UI), такой как страница, диалог или форма. Компонент включает как разметку HTML для рендеринга, так и логику обработки, необходимую для ввода данных или реагирования на события пользовательского интерфейса. Компоненты являются гибкими и легкими, и они могут быть вложенными, повторно использованными и совместно использоваться несколькими проектами.

## Классы компонентов

Компоненты Blazor обычно определяются в *\*.cshtml* файлах и используют комбинацию C# и HTML разметки. Внешний вид компонента определяется с помощью использования HTML. Динамическая логика визуализации (например, циклы, условия, выражения) добавляется с использованием встроенного синтаксиса C# [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Когда приложение Blazor скомпилировано, HTML-разметка и логика отображения C# преобразуются в класс компонентов. Имя сгенерированного класса соответствует имени файла.

Члены класса компонента определены в блоке `@functions` (допускается больше одного блока`@functions`). В блоке`@functions`, компоненты состояния (свойства, поля) задаются вместе с методами обработки событий или для определения другой логики компонента.

Члены компонента затем могут использоваться как часть логики рендеринга компонента с использованием выражений C#, которые начинаются с `@`. Например, поле C# определяется префиксом `@` к имени поля. Следующий пример демонстрирует, как это работает:

* `_headingFontStyle` к значению свойства CSS для `font-style`.
* `_headingText` к содержанию элемента `<h1>`.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

После первоначальной визуализации компонента компонент регенерирует дерево рендеринга в ответ на события. Затем Blazor сравнивает новое дерево рендеринга с предыдущим и применяет любые изменения в Document Object Model (DOM) браузера.

## Использование компонентов

Компоненты могут включать другие компоненты, объявляя их с помощью синтаксиса элемента HTML. Разметка для использования компонента выглядит как тег HTML, где имя тега является типом компонента.

Следующая разметка отображает пример `HeadingComponent` (*HeadingComponent.cshtml*):

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/Index.cshtml?start=11&end=11)]

## Параметры компонента

Компоненты могут иметь *параметры компонента*, которые определяются с помощью *непубличных* свойств класса компонентов, помеченный аттрибутом `[Parameter]`. Используйте атрибуты для указания аргументов компонента в разметке.

В следующем примере, на странице `ParentComponent` задаётся значение свойства `Title` компонента `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## Дочерний контент

Компоненты могут устанавливать контент в другом компоненте. Назначающий компонент предоставляет контент между тегами, которые определяет принимающий компонент. Намример, компонент `ParentComponent` может предоставлять контент, который должен быть в компоненте `ChildComponent` путем размещения содержимого внутри тега **\<ChildComponent>**.

*ParentComponent.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/ParentComponent.cshtml?start=1&end=7&highlight=6)]

Дочерний компонент содержит свойство `ChildContent` которое представляет [RenderFragment](/api/Microsoft.AspNetCore.Blazor.RenderFragment.html). Значение `ChildContent` позиционируется в разметке дочернего компонента, где должен отображаться контент. В следующем примере, значение `ChildContent` передаётся из родительского компонента и отображается внутри панели Bootstrap `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> Свойство получающее контент `RenderFragment` должно быть названо `ChildContent` условно.

## Связывание данных

Связывание даннх компонента и элементов DOM происходит с помощью аттрибута `bind`. Следующий пример демонстрирует связывание свойства `ItalicsCheck` и флажок (check box):

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" bind="@_italicsCheck" />
```

Когда флажок установлен или убран, значение свойства меняется на `true` или `false` соответственно.

Флажок обновляется в пользовательском интерфейсе только тогда, когда компонент отображается, а не в ответ на изменение значения свойства. Поскольку компоненты отображают себя после выполнения кода обработчика событий, обновления свойств немедленно отражаются в пользовательском интерфейсе.

Используйте `bind` со свойством `CurrentValue` (`<input bind="@CurrentValue" />`) это будет эквивалентно следующему:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIValueEventArgs __e) => CurrentValue = __e.Value)" />
```

Когда компонент отображает `value` элемента ввода из свойства `CurrentValue`. Когда пользователь вводит данные в текстовое поле запускается `onchange` и свойство `CurrentValue` устанавливается на изменённое значение. На самом деле генерация кода немного сложнее, потому что `bind` имеет дело с несколькими случаями, когда выполняются преобразования типов. В принципе, `bind` связывает текущее значение выражения с атрибутом` value` и обрабатывает изменения с помощью зарегистрированного обработчика.

**Форматирование строк**

Связывание данных работает с [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime) форматом строк (но не для других, таких как формат валют или чисел):

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

Аттрибут `format-value` определяет формат даты для задания `value` элемента `input`. Формат также используется для анализа значения, когда происходит событие `onchange`.

**Параметры компонента**

Связывание также распознает параметры компонента, где `bind-{property}` может связывать значение свойства между компонентами.

Следующий компонет использует компонент `ChildComponent` и связывает параметр `ParentYear` из родительского компонента с параметром `Year` из дочернего компонента:

Родительский компонент:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">Change Year to 1986</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Дочерний компонент:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

Параметр `Year` является связующим, потому что он имеет событие `YearChanged`, которое соответствует типу параметра `Year`.

Загрузка компонента `ParentComponent` производит следующую разметку:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Если значение свойства `ParentYear` изменяется путем выбора кнопки в `ParentComponent`, свойство `Year` компонента `ChildComponent` обновляется. Новое значение `Year` отображается в пользовательском интерфейсе когда `ParentComponent` перезагрузится:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## Обработка событий

Blazor предоставляет функции обработки событий. Имя аттрибута HTML элемента будет вида `on<event>` (например, `onclick`, `onsubmit`) с заданным значением делегата, Blazor рассматривает значение атрибута как обработчик события. Имя атрибута всегда начинается с `on`.

В следующем коде вызывается метод `UpdateHeading` когда нажимается кнопка в пользовательском интерфейсе:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

Следующий код вызывает метод `CheckboxChanged` когда используется флажок(check box) в пользовательском интерфейсе:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

Для некоторых событий поддерживаются типы аргументов событий. Если доступ к одному из этих типов событий не требуется, он не требуется в вызове метода.

Список поддерживаемых агрументов событий:

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

Лямбда-выражения также могут быть использованы:

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

## Захват ссылок на компоненты

Ссылки на компоненты предоставляют способ получить ссылку на экземпляр компонента, чтобы вы могли выдавать команды этому экземпляру, например `Show` или `Reset`. Чтобы зафиксировать ссылку на компонент, добавьте атрибут `ref` к дочернему компоненту, а затем определите поле с тем же именем и тем же типом, что и дочерний компонент.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Когда компонент отображается, поле `loginDialog` заполняется экземпляром дочернего компонента `MyLoginDialog`. Затем вы можете вызвать методы .NET на экземпляре компонента.

> [!IMPORTANT]
> Переменная `loginDialog` заполняется только после рендеринга компонента, и ее вывод включает элемент `MyLoginDialog`, потому что до этого момента не на что ссылаться. Чтобы манипулировать ссылками на компоненты после завершения рендеринга компонента, используйте методы жизненного цикла `OnAfterRenderAsync` или `OnAfterRender`.

При захвате ссылок компонентов используется аналогичный синтаксис для [захват ссылок на элементы](xref:client-side/blazor/javascript-interop#захват-ссылок-на-элементы), это не функционал [JavaScript-взаимодействие](xref:client-side/blazor/javascript-interop). Ссылки на компонент не передаются в код JavaScript; они используются только в коде .NET.

> [!NOTE]
> **Не** используйте ссылки на компоненты для изменения состояния дочерних компонентов. Вместо этого всегда используйте обычные декларативные параметры для передачи данных дочерним компонентам. Это приводит к тому, что дочерние компоненты автоматически возвращаются в правильное время.

## Жизненный цикл методов

`OnInitAsync` и `OnInit` выполняются после того как компонент был инициализирован. Для выполнения асинхронной операции используйте `OnInitAsync` и используется ключевое слово `await`:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Для синхронных операций используйте `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` и `OnParametersSet` вызывается, когда компонент получает параметры от своего родителя, а значения присваиваются свойствам. Эти методы выполняются после `OnInit` во время инициализации компонента.

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` и `OnAfterRender` вызывается каждый раз после завершения обработки компонента. В этот момент заполняются ссылки на элементы и компоненты. Используйте этот этап для выполнения дополнительных шагов инициализации с использованием визуализированного контента, такого как активация сторонних библиотек JavaScript, которые работают с отображаемыми элементами DOM.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` может быть переопределено для выполнения кода до того, как будут установлены параметры:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Если `base.SetParameters` не вызывается, пользовательский код может интерпретировать значение входящих параметров любым способом. Например, входящие параметры не требуются для назначения свойств класса.

`ShouldRender` может быть переопределен для подавления обновления пользовательского интерфейса. Если реализация возвращает `true`, пользовательский интерфейс обновляется. Даже если `ShouldRender` переопределен, компонент всегда отображается.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## Утилизация компонентов с IDisposable

Если компонент реализует [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable), [Dispose метод](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose) вызывается когда компонент удаляется из пользовательского интерфейса. Следующий компонент использует `@implements IDisposable` и метод `Dispose`:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## Маршрутизация

Маршрутизация в Blazor достигается путем предоставления шаблона маршрута каждому доступному компоненту в приложении.

Когда файл *\*.cshtml* с директивой `@page` компилируется, порождённому классу даётся [RouteAttribute](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) задавая шаблон моршрута. Во время выполнения маршрутизатор ищет классы компонентов с атрибутом `RouteAttribute` и отображает тот, какой имеет шаблон маршрута, соответствующий запрашиваемому URL.

К компоненту могут применяться несколько шаблонов маршрутов. Следующий компонент отвечает на запросы `/BlazorRoute` и `/DifferentBlazorRoute`:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

## Параметры маршрута

Компоненты Blazor могут получать параметры из шаблона маршрута, указанного в директиве `@page`. Blazor использует параметры маршрута для заполнения соответствующих параметров компонента.

*RouteParameter.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=9)]

Необязательные параметры не поддерживаются, поэтому в приведенном выше примере применяются две директивы `@page`. Первая разрешает навигацию к компоненту без параметра. Вторая директива `@page` принимает параметр`{text}` и присваивает значение свойству `Text` компонента.

## Наследование базового класса для "Раздельного кода"(code-behind)

Файлы компонентов Blazor (*\*.cshtml*) содержат HTML-разметку и код обработки C# в том же файле. Директива `@inherits` может быть использована для обеспечения Blazor возможностей "Раздельного кода"(code-behind), позволяя отделять разметку компонентов от кода обработки.

[Пример приложения](https://github.com/aspnet/Blazor.Docs/tree/master/docs/common/samples/) показывающего, как компонент может наследовать базовый класс `BlazorRocksBase`, для предоставления свойств и методов компонента.

*BlazorRocks.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Pages/BlazorRocks.cshtml?start=1&end=8)]

*BlazorRocksBase.cs*:

[!code-csharp[](../common/samples/2.x/BlazorSample/Pages/BlazorRocksBase.cs)]

Базовый класс должен быть получен из [BlazorComponent](/api/Microsoft.AspNetCore.Blazor.Components.BlazorComponent.html).

## Поддержка Razor

**Директивы Razor**

Директивы Razor, совместимые с приложениями Blazor, показаны в следующей таблице.

| Директива | Описание |
| --------- | ----------- |
| [@functions](https://docs.microsoft.com/aspnet/core/mvc/views/razor#functions) | Добавляет блок C# кода к компоненту.|
| `@implements` | Реализует интерфейс для класса сгенерированных компонентов. |
| [@inherits](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inherits) | Обеспечивает полный контроль над классом, который наследует компонент. |
| [@inject](https://docs.microsoft.com/aspnet/core/mvc/views/razor#inject) | Включает инъекцию сервиса из [сервисного контейнера](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Для дополнительной информации смотрите [Вложение зависимостей в представления](https://docs.microsoft.com/aspnet/core/mvc/views/dependency-injection). |
| `@layout` | Задает компонент макета. Компоненты макета используются, чтобы избежать дублирования кода и несогласованности. |
| [@page](https://docs.microsoft.com/aspnet/core/mvc/razor-pages#razor-pages) | Указывает, что компонент должен обрабатывать запросы. Директива `@page` может задавать маршрут и дополнительные параметры. В отличие от `Razor Pages` директива `@page` не должна быть первой директивой в верхней части файла. Дополнительную информацию смотрите в разделе [Маршрутизация](xref:client-side/blazor/routing). |
| [@using](https://docs.microsoft.com/aspnet/core/mvc/views/razor#using) | Добавляет C# директиву `using` в класс сгенерированных компонентов. |
| [@addTagHelper](https://docs.microsoft.com/aspnet/core/mvc/views/razor#tag-helpers) | Используйте `@addTagHelper`, чтобы использовать компонент в сборке отличной от сборки приложения. |

**Условные атрибуты**

Blazor условно отображает атрибуты на основе .NET значения. Если значение `false` или `null`, Blazor не будет отображать атрибут. Если значение `true`, атрибут визуализируется с минимальным значением.

В следующем примере, `IsCompleted` определяет будет ли `checked` отображаться в разметке элемента управления.

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Если `IsCompleted` равно `true`, флажок отображается как:

```html
<input type="checkbox" checked />
```

Если `IsCompleted` равно `false`, тогда флажок будет отображаться как:

```html
<input type="checkbox" />
```

**Дополнительная информация о Razor**

Для получения дополнительной информации о Razor смотрите [Ссылка на синтаксис Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Обратите внимание, что не все функции Razor доступны в Blazor в настоящее время.

## Необработанный HTML

Blazor обычно передает строки с использованием текстовых узлов DOM, что означает, что любая их разметка игнорируется и обрабатывается как литерал. Чтобы сделать необработанный HTML, оберните содержимое HTML в значение `MarkupString`, которое затем анализируется как HTML или SVG и вставляется в DOM.

> [!WARNING]
> Предоставление необработанного HTML, из любого ненадежного источника, является **риском безопасности**, и его следует избегать!

В следующем примере показано использование типа `MarkupString` для добавления блока статического HTML в отображаемых данных компонента:

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## Templated components

Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic. Templated components allow you to author higher-level components that are more reusable than regular components. A couple of examples include:

* A table component that allows a user to specify templates for the table's header, rows, and footer.
* A list component that allows a user to specify a template for rendering items in a list.

### Template parameters

A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`. A render fragment represents a segment of UI that is rendered by the component. A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Components/TableTemplate.cshtml)]

When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### Template context parameters

Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element. In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

Alternatively, you can specify the `Context` attribute on the component element. The specified `Context` attribute applies to all specified template parameters. This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element). In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### Generic-typed components

Templated components are often generically typed. For example, a generic ListView component can be used to render `IEnumerable<T>` values. To define a generic component, use the `@typeparam` directive to specify type parameters.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](../common/samples/2.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

When using generic-typed components, the type parameter is inferred if possible:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter. In the following example, `TItem="Pet"` specifies the type:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## Razor templates

Render fragments can be defined using Razor template syntax. Razor templates are a way to define a UI snippet and assume the following format:

```cshtml
@<tag>...<tag>
```

The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.

*RazorTemplates.cshtml*:

```cshtml
@{ 
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly. For example, the previous templates are directly rendered with the following Razor markup:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Rendered output:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
