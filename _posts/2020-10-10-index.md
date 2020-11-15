Любое свойства зависимости должно быть зарегестрированно перед использованием

Для регистрации необходимо создать статическое поля с типом `DependencyProperty`, которая инициализируется при объявлении или в статическом конструкторе

Экземпляр класса `DependencyProperty` возвращается статическим методом `DependencyProperty.Register`.

Простейшая перегрузка метода Register требует
- задания имени свойства зависимости
- типа свойства зависимости `propertyType`
- типа владельца свойств `ownerType`
- объект метаданных свойст зависимости `typeMetadata`
- функция обратного вызова `calidateValueCallback`


Регистрация свойства зависимости в некотором классе

```csharp
public class User : DependencyObject
{
    public static readonly DependencyProperty NameProperty;
    
    static User()
    {
        NameProperty = DependencyProperty.Register(
            name: "Name",
            propertyType: typeof(string),
            ownerType: typeof(User),
            // метаданные содержат значения свойства по умолчанию
            typeMetadata: new PropertyMetadata("Empty"),
            validataValueCallback: IsNameValid
        );
    }
    
    private static bool IsNameValid(object name)
    {
        return !string.IsNullOrEmpty(name as string);
    }
}
```

Свойство создание экземплярной оболочки

```csharp
public class User : DependencyObject
{
    // Продолжение класса User
    
    public string Name
    {
        get{ return (string) GetValue(NameProperty); }
        set{ SetValue(NameProperty, value); }
    }
}
```

geter и seter ипользуют `getValue` и `SetValue` для чтения установки значения свойства зависимости.
Дополнительную работу в Geterax b seterax не рекомендуется выполнять, потомучто некоторые класса PF могут обращаться миную эту оболочку.

По мимо `getValue` и `SetValue` имеет `ClearValue` сбрасывающий свойства зависимости по умолчанию. А значение по умолчанию устанавливается в `metaData`.

# Привязка данных

Подрузумевает взаимодействие двух объектов источника и приемника.

Объект приемник создает привязкусвязку к определеному объекту источника. В случаи модификации объекта источника объект приемник также будет модифицирован. Для определения привязки используется выражение

```
{Bindin ElementName=Имя_объекта-источника, Path=Свойство_объекта-источника}
```

Bindin обозначает, что будет создан объек класса `System.Windows.Data.Binding`

Path - Имя свойств или путь для исходного объекта

```xml
<Window>
    ...

    <StackPanel>
        <TextBox
            x:Name="myTextBox"
            Height="30"
        />
        <TextBox
            x:Name="myTextBox"
            Text="{Bindin ElementName=myTextBox, Path=Text}"
            Height="30"
        />
    </StackPanel>
</Window>
```

`TextBox` - является источником

Свойства текст элемента

При осуществления ввода в текстовое поле будет происходить изменения в текстовом блоке.

Таким образом привязка данных это отношене которое используется для изъятия данных из источника данных и установки свойства зависимостей в целевом объекте.

![img]()

Целевой объект является элементом управления.
Источником данных может быть произвольный объект dotNet

ПРи определении привязки нужно указать целевое свойство и сточник данных правило извлечения данных из источника.

Кроме этого также можно настроить следующие параметры
1. Направление привязки
    - однонапрвленная (целевое свойство изменяется при изменении данных источника)
    - двунаправленый (изменение в источнике и целевом объекте влияют друг на друга)
    - от цели к источнику (источник данных обновляется при изменении целевого свойства)

2. Условие обновления источника
    - если привязка двунаправленая или от цели к источнику можно настроить момент обновления источника данных.

    Третий конвертер назначений

    привязка данны] может выполнять ... при их перемещении от источника к целеввому объекту и наоборот (преобразовании data к нужному ворматку)

3. Правило проверки данных
    Прязка может включать правило проверки данных на корректность и поведение выполняемое при нарушении правилю


Класс `System.Windows.Data.BindingOperations` предоставляет набор статическим методов для привязки к коду. Частности метод GetBnding позволяет получить привязку для указанного целевого объекта и его свойства зависимости.

```csharp
Binding binding = BindingOperations.GetBinding(myTextBlock, TextBlock.TextProperty);
```

В этом примере получает привязку для свойства зависимости `textProperty` элемента `myTextBlock`

Метод `SetBinding` устанавливает привязку

```csharp
public MainWindow()
{
    InitializeComponent();

    Binding binding = new Binding();

    binding.ElementName = "myTextBox"; //элемент-источник
    binding.Path = new PropertyPath("Text"); //свойство элемента-источника
    myTextBlock.SetBinding(TextBlock.TextProperty, binding); //установка привязки для элемента-приемника
}
```

Метод `ClearBinding` - удаляет привязку

Метод `ClearAllBinding` - удаляет все привязки для данного элемента

Основные свойства `System.Windows.Data.Binding`

- Свойство Converter

![img]()
- `Converter` - конвертер для преобразования данных при привязке
- `ConverterCulter` - культура передаваемая в конвертер
...
- `Path` - путь к информации в источнике данных. Аргумент конструктора класса `Binding`
- `RelativeSource` - Путь к источнику, заддаваемый относительно целевого объекта
- `Source` - источник данных если мы не имеет элементы управления
- `StringFormat` - форматирование для извлекаемых данных строкового типа
- `TargetNullValue` - значение, которое будет использоваться для целевого свойтва, если из источника извлекается `null`
...
- `UpdateSourceTrigger` - Задает момент обновления источника при изменениях и в целевом свойстве (перечисление `UpdateSiurceTrigger`):
    - `PropertyChanged` - немедленно при изменении
    - `LostFocus` - при потере целевым элементов фокуса ввода
    - `Explicit` - при вызове метода `UpdateSource();`
    - `Default` - по значению, заданному в метаданных свойства зависимостей при помощи `DefaultUpdateSourceTrigger`
- `XPath` - Выражение `XPath`. МОжет использоваться, если источник привязки возвращает `XML`

PassBinding позволяет использовать привязку в XAML или в коде.

В ледующем припере при перемещении слайдера автоматически меняется размер шрифта

```xml
<StackPanel>
    <SLider
        x:Name="slider"
        Minimum="1"
        Maximum="40"
    >
    <TextBlock
        x:Name="text"
        Text="Sample Text"
        FontSize="{Binding Element=slider, Path=Value}"
    />
</StackPanel>
```

```csharp
// создаём и настраиваем объект Binding
var binding = new Binding();
binding.ElementName = "slider";
binding.Path = new PropertyPath("value");

//установка привязки
text.SetBinding(TextBlock.FontSizeProperty, binding);
```
