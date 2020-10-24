# Стили

Стили - это коллеккция значений свойств, которые могут быть применены к переменным.

WPF играет ту же роль что и CSS в HTML.

Стили позволяют определять общий набор характеристик ворматирования и применять их по всему приложению для обеспечения согласованости.

Стили могут работать автоматически, либо предназначаться для элементов конкретного типа, а также каскадироваться через дерево элементов.

Расмотрение стилей:

Пример

```xml
<StackPanel
    Orientation="Horizontal"
    VerticalAligment="Top"
>
    <Button
        Background="DarkBlue"
        Foreground="White"
        FontFamily="Verdana"
        Padding="5"
        Margin="5"
    >Открыть</Button>
    <Button
        Background="DarkBlue"
        Foreground="White"
        FontFamily="Verdana"
        Padding="5"
        Margin="5"
    >Обработать</Button>
    <Button
        Background="DarkBlue"
        Foreground="White"
        FontFamily="Verdana"
        Padding="5"
        Margin="5"
    >Сохранить</Button>
    <Button
        Padding="5"
        Margin="5"
    >Закрыть</Button>
</StackPanel>
```

Здесь мы видем, что для трех элементов управления повторяются атрибуты с одинаковыми значениями. Наличие повторяющего кода является плохим стилем. В данном случае является определение внешнего вида кнопок с помощью стиля, и задание этого стиля для трех кнопок.

Обычно стили как и другие ресурсы приложения определяются в ресурсах окна

```xml
<Windows.Resources>
    ...
    <Style>

    </Style>
    ...
</Windows.Resources>
```

Тем не мение свойства отступ объявлени для класса frameworkElment  Значит ресурсы можно применить практически для любого элемента управления.

```xml
<StackPanel.Resources>
    ...
    <Style>

    </Style>
    ...
</StackPanel.Resources>
```


```xml
<Button.Resources>
    ...
    <Style>

    </Style>
    ...
</Button.Resources>
```

Область действия стиля объявленого когого либо элемента распостраняется на элемент или его дочерний элемент. Имено поэтому в ресурсах окна чаще всего распологаются стили.

Любой стиль WPF это объект класса `Windows.System.Style`

Стиль может использоваться для
- определения значения атрибутов
- обработчика событий
- тригеров
- шаблонов

Ключевые свойства определёные в классе Style

- TargetType - стиль элемента для которого определяется данных стиль
- BaseOn - родительский стиль (позволяет задавать иерархические стили)
- Setters - коллекция объектов Setter или EventSetter, которые предназначены для установления значения свойств и обработчика событий
- Triggers - коллекция тригеров
- Resources - коллекция ресурсов


Основным свойством стиля является коллекция Setters. Свойства содержимого, которой каждый элемент, которое задает значение для некоторого свойства зависимости (стили работают только со свойством зависимости).

Чтобы задать значения свойства зависимости нужно задать имя. Шаблон:

```xml
<Setter
    Property="НАЗВАНИЕ_СВОЙСТВА"
    value="ЗНАЧЕНИЕ"
/>
```

Теперь модифицируем наш пример с использованием стилей


```xml
<Window.Resources>
    <Style TargetType="Button">
        <Style.Setters>
            <Setter Property="Background" Value="DarkBlue" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="Padding" Value="Verdana" />
            <Setter Property="Padding" Value="5" />
            <Setter Property="Margin" Value="5" />
        </Style.Setters>
    </Style>
</Window.Resources>
<StackPanel
    Orientation="Horizontal"
    VerticalAligment="Top"
>
    <Button>Открыть</Button>
    <Button>Обработать</Button>
    <Button>Сохранить</Button>
    <Button
        padding="5"
        Margin="5"
    >Закрыть</Button>
</StackPanel>
```

```xml
<kod>
```

Здесь допускается не использовать элемент Styles setters

В данном примере стиль был применён ко всем кнопкам окна. С помощью атрибуты TargetType был определен тип элемента приправления. Мы написали Button - и он применил ко всем кнопкам

Если для элемента стайл применить x:key с именем стиля, то стиль будет применён к тем кнопкам для которого указано имя стиля. В атрибуте с помощью ...

```xml
<Window.Resources>
    <Style
        TargetType="Button"
        x:Key="DocButton"
    >
        <Style.Setters>
            <Setter Property="Background" Value="DarkBlue" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="Padding" Value="Verdana" />
            <Setter Property="Padding" Value="5" />
            <Setter Property="Margin" Value="5" />
        </Style.Setters>
    </Style>
</Window.Resources>
<StackPanel
    Orientation="Horizontal"
    VerticalAligment="Top"
>
    <Button
        Style="{StaticResources ResourceKey=DocButton}"
    >Открыть</Button>
    <Button
        Style="{StaticResources ResourceKey=DocButton}"
    >Обработать</Button>
    <Button
        Style="{StaticResources ResourceKey=DocButton}"
    >Сохранить</Button>
    <Button
        padding="5"
        Margin="5"
    >Закрыть</Button>
</StackPanel>
```

Свойство BaseOn класса Style позволяет определять иерархические стили. В этом свойстве с помощью расширения разметки указывается родительский стиль.

Дочерний стиль наследует все свойства родительского, а также может их дополнить или переопределить. В примере определяется дочерний стиль DockButton на основе родительского DockButton

```xml
<Window.Resources>
    <Style TargetType="Button" x:Key="DocButton">
        <Style.Setters>
            <Setter Property="Background" Value="DarkBlue" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="Padding" Value="Verdana" />
            <Setter Property="Padding" Value="5" />
            <Setter Property="Margin" Value="5" />
        </Style.Setters>
    </Style>
    <Style 
        BaseOn="{StaticResource DocButton}"
        TargetType="Button"
        x:Key="ActiveDocButton"
    >
        <Setter
            Property="Background"
            Value="DarkBlue"
        />
    </Style>
</Window.Resources>
<StackPanel
    Orientation="Horizontal"
    VerticalAligment="Top"
>
    <Button
        Style="{StaticResources ResourceKey=ActiveDocButton}"
    >Открыть</Button>
    <Button
        Style="{StaticResources ResourceKey=DocButton}"
    >Обработать</Button>
    <Button
        Style="{StaticResources ResourceKey=DocButton}"
    >Сохранить</Button>
    <Button
        padding="5"
        Margin="5"
    >Закрыть</Button>
</StackPanel>
```

Объект Event Setter определяет имя функции обработчика для событий

```xml
<EventSetter
    Event="НАЗВАНИЕ_СОБЫТИЯ"
    Handler="ИМЯ_ФУНКЦИИ"
/>
```


Пример задания обработчика для всех кнопок:

```xml
<Window.Resources>
    <Style TargetType="Button">
        <Style.Setters>
            <Setter
                Property="Margin"
                Value="5"
            />
            <EventSetter
                Event="Click"
                Handler="Button_Click"
            />
        </Style.Setters>
    </Style>
</Window.Resources>
<StackPanel
    Orientation="Horizontal"
    VerticalAligment="Top"
>
    <Button>Открыть</Button>
    <Button>Обработать</Button>
    <Button>Сохранить</Button>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Button is clicked");
}
```

![compile img]()

# Триггеры

При описании стилей часто применяются триггеры. Триггер - это способ описания реакции на изменение. Альтернативны обработчику событий.

Тригеры имею два достоинтсва
- легко задать декларативно
- указывается только условие старта

Условие завершения указывать не требуется. Каждый тригер является результатом класса, который наследуется от класса `System.Windows.TriggerBase`

В WPF доступны пять видов триггеров:

1. Триггер свойства - это экземпляр класса trigger это тригер самого простого типа. Он следит за поялением изменений в свойстве зависимостей.

2. Триггер данных - класс `DataTrigger` раблотает со связыванием данных. Поход на тригер свойство, но следит са появлением изменений не в свойстве зависимостей, а в любых связанных данных.

3. Триггер события - класс `EventTrigger`. Следит за наступлением указанного события. Применяются в анимациях.

4. Мулититригер - класс `MultiTrigger`. Тригер объединяет несколько тригеров свойств. Стартует, если выполняются все условия объединяемых триггеров

5. Мульти триггер данных - класс `MultiDataTrigger` Подобен мульти триггеру, но объединяет несколько триггеров данных.

Для декларации и хранения тригеров имеется коллекция Triggers.

```xml
<Style ...>
    <Style.Triggers>
    <!-- Триггеры-->
    </Style.Triggers>
</Style>
```

Рассмотрим триггер свойст или простого тригера. Это формат простого триггера

```xml
<Trigger
    Property="СВОЙСТВО"
    Value="Значение"
>
    <Trigger.Setters>
        <!-- Коллеция элементо Setter -->
    </Trigger.Setters>
</Trigger>
```

Свойство Setters можно не указывать

```xml
<Trigger
    Property="СВОЙСТВО"
    Value="Значение"
>
    <!-- Коллекция элементов Setter -->
</Trigger>
```

У тригера свойства настраиваются имя свойство значение свойства и коллекция сеттерс.

Простые тригеры следят за значением свойст и в случае их изменения с помощью объекта сеттерс устанавливают значение других свойств.

ПРимер


В этом примере по навидению на кнопку высота шривта устанавливается в 14 пикселей, а цвет шрифта становится крассным.


```xml
<Window ...>
    <Window.Resources>
        <Style.Setters>
            <Setter Property="Button.Background" Value="Black">
            <Setter Property="Button.Foreground" Value="White">
            <Setter Property="Button.FontFamily" Value="Vardana">
            <Setter Property="Button.Margin" Value="10">
        </Style.Setters>
        <Style.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Button.FontSize" Value="14">
                <Setter Property="Button.Foreground" Value="Red">
            </Trigger>
        </Style.Triggers>
    </Window.Resources>
    <StackPanel Background="Black">
        <Button x:Name="button1" Content="Кнопка 1" />
         <Button x:Name="button2" Content="Кнопка 2" />
    </StackPanel>
</Window>
```

Здесь объект тригер применяет два свойства проперти и валью.

Свойство prorerty указывает на отслеживаемое свойство
Свойство value указывает на значение
По достижению которой триггер начинает действовать.
В даном случае триггер отслеживает свойства `isMouseOver` если оно будет равно True - триггер сработает

тригерр имеет вложеную коллекцию Setters, которая реализует его.

# Мульти триггер

```xml
<MultiTrigger>
    <MultiTrigger.Conditions>
        <Condition ...>
            ...
        </Condition ...>
    </MultiTrigger.Conditions>
    <MultiTrigger.Setters>
        <!-- Коллекция элементо Setter -->
    </MultiTrigger.Setters>
</MultiTrigger>
```

Свойство Setters можно не указывать.

Мулти тригер содержит коллекцию элементов кондитион, каждый из которых как и обычный тригер определяет отслеживаемое свойство и его значение.

```xml
<Style.Triggers>
    <MultiTrigger.Condition>
        <Condition Property="IsMouseOver" Value="True" />
        <Condition Property="IsPressed" Value="True" />
    </MultiTrigger.Condition>
    <MultiTrigger.Setters>
        <Setter Property="FontSize" Value="14" />
        <Setter Property="Foreground" Value="Red" />
    </MultiTrigger.Setters>
</Style.Triggers>
```

В данном случае, если оба условия одновременно выполняются IsMouseOver True ШыЗкуыыув  True то срабатывают сеттеры тригера

# MultiDataTrigger

Оно моход на MultiTrigger, но в нем Condition может работать как мулти триггер, а может как свойство другого элемента управления.

```xml
<Condition Property="СВОЙСТВО" Value="ЗНАЧЕНИЕ" />
```

Так и значение связанного свойства другого элемента управления:

```xml
<Condition
    Binding="{Binding ElementName=ИМЯ_СВЯЗАННОГО_ОБЪЕКТА, Path=ИМЯ_СВОЙСТВА}"
    Value="ЗНАЧЕНИЕ"
/>
```
