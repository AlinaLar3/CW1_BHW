# CW1_BHW
# "Учёт финансов"

## Описание
Этот проект представляет собой консольное приложение для управления личными финансами "Учёт финансов". Он позволяет:
 1. Создавать, редактировать и удалять счета, категории, операции (доходов/расходов).
 2. Импортировать и экспортировать данные: 
a. Экспорт всех данных в файлы CSV, JSON. 
b. Импорт данных из этих форматов.
 3. Статистика: 
a. Измерение времени работы отдельных пользовательских сценариев. 

### Принципы SOLID

1. **S - Принцип единственной ответственности**:
   - Каждый класс отвечает за свою конкретную задачу. Например, класс **BankAccount** отвечает только за представление информации о банковском счете, класс **BankAccountService** отвечает только за управление банковскими счетами, класс **CSVDataVisitor** только за экспорт данных в CSV файлы, класс **JsonDataImporter** отвечает только за импорт данных из JSON файлов и так далее
2. **O - Принцип открытости/закрытости**:
   - Классы могут быть расширены через наследование (например, **BankAccount**, **IDataVisitor**, **DataImporter**) без изменения существующего кода. Это позволяет расширять и создавать подклассы банковских счетов (**BankAccount**), добавлять новые форматы экспорта(**IDataVisitor**), добавлять новые форматы импорта (**DataImporter**) и так далее.

3. **L - Принцип подстановки Лисков**:
   - Подклассы (например, **JsonDataImporter** и **CsvDataImporter**) могут использоваться вместо базового класса (тут, вместо **DataImporter**) без нарушения логики приложения. Они реализуют необходимые методы и свойства.

4. **I - Принцип разделения интерфейсов**:
   - Клиенты не должны вынужденно зависеть от методов, которыми не пользуются (например, **IRepository** или **IDataVisitor** может реализовываться только теми классами, которым это действительно необходимо).

5. **D - Принцип инверсии зависимостей**:
   - Приложение использует контейнер внедрения зависимостей (DI-контейнер) для управления зависимостями, такими как **IVeterinaryClinic** в классе **Zoo**

### Принципы GRASP

1. **Low Coupling**:
   - Классы минимально зависят друг от друга. Например, **BankAccountService**, **CategoryService** и **OperationService** - фасадные классы разделяют объекты, управляемые ими, таким образом, что изменения в одном из них не влияют на другие.

2. **High Cohesion**:
   - Каждый класс имеет четко определенные обязанности, что делает их более понятными и простыми для сопровождения. Например, **BankAccountService**, **CategoryService** и **OperationService** зависят не от конкретного класса InMemoryRepository, а от интерфейса IRepository<T>, следовательно, не захватывает лишнее. 

### Парренты

1. **Фасад (Facade)**: BankAccountService, CategoryService и OperationService.
   - Упрощают взаимодействие с подсистемой управления счетами, категориями и операциями.

2. **Команда (Command)**: ICommand, CreateBankAccountCommand, TimeLoggingCommandDecorator и так далее.
   - Проще применить над каждой из команд паттерн Декоратор.

3. **Декоратор (Decorator)**: CommandDecorator и TimeLoggingCommandDecorator.
   - Добавляет функциональность к командам (измерение времени) без изменения их исходного кода.

4. **Шаблонный метод (Template Method)**:  DataImporter.
   - Определяет скелет алгоритма импорта, позволяя подклассам (JsonDataImporter, CsvDataImporter) переопределять шаги чтения и разбора данных.

5. **Посетитель (Visitor)**: IDataElement, IDataVisitor, FinancialData, JsonDataVisitor, CsvDataVisitor.
   - Добавляет новые операции к структуре данных без изменения классов данных. Позволяет обрабатывать различные операции для выгрузки данных.

6. **Фабрика (Factory)**: IRepositoryFactory и InMemoryRepositoryFactory.
   - Абстрагирует процесс создания репозиториев, упрощает замену реализаций репозиториев.

### Инструкции:

1. Запустить приложение.
2. Выбрать НОМЕР команды.
3. Ввести запрашиваемые данные.
   - При выборе счёта, категории, операции для какой-либо команды указывать НОМЕР (не Id или имя)
   - При запросе файла вводить ПОЛНЫЙ ПУТЬ файла без кавычек. И для экспорта, и для импорта должны быть уже существующие в данной директории файлы.
4. Продолжать выборы команд.
5. При желании завершить работу ввести ноль на странице выбора команд.

### Дополнения:
1. Используется DI-контейнер для зависимостей.

### Структура классов
- **BankAccount.cs** - Класс, представляющий банковский счет. Содержит информацию об ID, имени и балансе.
- **Category.cs** - Класс, представляющий категорию. Содержит информацию об ID, типе (доход или расход) и имени категории.
- **Operation.cs** - Класс, представляющий финансовую операцию. Содержит информацию об ID, типе, ID счета, сумме, дате, ID категории и описании.
- **CategoryType.cs** - Содержит перечисления CategoryType (доход или расход) и OperationType (доход или расход).
- **IBankAccountService.cs** - Интерфейс сервиса управления банковскими счетами (создание, предоставление, обновление, удаление, получение списка).
- **ICategoryService.cs** - Интерфейс сервиса управления категориями (создание, предоставление, обновление, удаление, получение списка).
- **IOperationService.cs** - Интерфейс сервиса управления операциями (создание, предоставление, обновление, удаление, получение списка, получение списка операций по счету).
- **BankAccountService.cs** - Реализация IBankAccountService.
- **CategoryService.cs** - Реализация ICategoryService.
- **OperationService.cs** - Реализация IOperationService.
- **IRepository.cs** - Интерфейс репозитория, определяющий методы для доступа к данным (получение, добавление, обновление, удаление, получение списка).
- **InMemoryRepository.cs** - Реализация IRepository<T>, использующая in-memory список для хранения данных.
- **IRepositoryFactory.cs** - Интерфейс для фабрики репозиториев.
- **InMemoryRepositoryFactory.cs** - Реализация фабрики, создающая InMemoryRepository.
- **ICommand.cs** - Интерфейс, определяющий методы Execute для выполнения команд.
- **CommandDecorator.cs** - Абстрактный класс, реализующий декоратор для команд.
- **TimeLoggingCommandDecorator.cs** - Конкретный декоратор, добавляющий время к команде.
- Команды, реализующие интерфейс ICommand для каждого пункта меню:
- **CreateBankAccountCommand.cs**, **CreateCategoryCommand.cs**, **CreateOperationCommand.cs** - команды для создания счета, категории или операции.
- **EditBankAccountCommand.cs**, **EditCategoryCommand.cs**, **EditOperationCommand.cs** - команды для редактирования счета, категории или операции.
- **DeleteBankAccountCommand.cs**, **DeleteCategoryCommand.cs**, **DeleteOperationCommand.cs** - команды для удаления счета, категории или операции.
- **ExportDataCommand.cs**, **ImportDataCommand.cs** - Команды для экспорта и импорта данных
- **ListAccountOperationsCommand.cs**, **ListBankAccountsCommand.cs**, **ListOperationsCommand.cs**, **ListCategoriesCommand.cs** - Команды для вывода списков счетов, операций, категорий
- Классы и интерфейсы для экспорта и импорта:
- **DataImporter.cs** - Абстрактный класс, определяющий шаблон для импорта данных.
- **JsonDataImporter.cs** - Реализация DataImporter для импорта данных из JSON-файлов.
- **CsvDataImporter.cs**- Реализация DataImporter для импорта данных из CSV-файлов.
- **IDataElement.cs** - Интерфейс, определяющий метод Accept для принятия посетителя.
- **FinancialData.cs** - Класс, представляющий данные для экспорта.
- **IDataVisitor.cs** - Интерфейс, определяющий метод Visit для посещения элемента данных.
- **JsonDataVisitor.cs** - Реализация IDataVisitor для экспорта данных в JSON-файл.
- **CsvDataVisitor.cs** - Реализация IDataVisitor для экспорта данных в CSV-файл.

- **Program.cs** - Главный класс программы, содержащий точку входа и настройки DI-контейнера.
