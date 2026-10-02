# System of Booking — система бронирования столиков

Консольное приложение на C++ для бронирования столиков в ресторане. Проект учебный, выполнен командой из 3 человек с использованием ООП: наследование, полиморфизм, абстрактные классы, шаблоны, раздельная компиляция и модульные тесты.

## Возможности

- Просмотр столиков и поиск свободных на заданные дату и время
- Создание и отмена бронирований
- Три типа бронирования с разным расчётом депозита:
  - **Standard** — обычная бронь, депозит 0
  - **VIP** — фиксированный депозит
  - **Banquet** — депозит зависит от количества гостей
- Проверка конфликтов (один столик не может быть забронирован дважды на одно время)
- Просмотр броней по дате
- Две роли: клиент (`Customer`) и администратор (`Administrator`)
- Сохранение и загрузка данных из файлов (`data/`)

Подробный список требований: [docs/requirements.md](docs/requirements.md).

## Архитектура

```
Person (abstract)               Booking (abstract)
 ├─ name, phone                  ├─ id, customer, table, date, time, guests
 ├─ virtual getRole() = 0        ├─ virtual calculateDeposit() = 0
 ├─ Customer                     ├─ virtual getDescription()
 └─ Administrator                ├─ StandardBooking  (депозит 0)
                                 ├─ VipBooking       (фикс. депозит)
Table                            └─ BanquetBooking   (депозит за гостя)
 └─ id, seats, isAvailable
                                 Repository<T> → Repository<Table>, Repository<Booking>
BookingService
 └─ createBooking, cancelBooking, findFreeTables,
    hasConflict, getBookingsByDate
```

| Компонент | Назначение |
|---|---|
| `Person`, `Customer`, `Administrator` | Пользователи системы, полиморфный `getRole()` |
| `Table` | Столик: номер, число мест, доступность |
| `Booking` и наследники | Бронирования, полиморфный `calculateDeposit()` |
| `Repository<T>` | Шаблонное хранилище объектов в памяти |
| `BookingService` | Бизнес-логика бронирования |
| `FileStorage` | Чтение и запись данных в файлы |
| `ConsoleUI` | Консольный интерфейс |

Диаграмма классов и детали: [docs/architecture.md](docs/architecture.md).

## Структура проекта

```
system_of_booking/
├── README.md
├── CMakeLists.txt
├── .gitignore
├── docs/                # архитектура, требования, git-правила, протоколы, отчёт
├── include/             # заголовочные файлы (.h)
├── src/                 # реализация (.cpp) и main.cpp
├── tests/               # модульные тесты
└── data/                # файлы данных (tables.txt, bookings.txt)
```

## Требования

- Компилятор с поддержкой C++17 (GCC 9+, Clang 10+, MSVC 2019+)
- CMake 3.14+
- Git

## Сборка и запуск

```bash
# клонирование
git clone <URL репозитория>
cd system_of_booking

# конфигурация и сборка
cmake -S . -B build
cmake --build build

# запуск (из корня проекта, чтобы приложение нашло папку data/)
./build/system_of_booking          # Linux / macOS
build\Debug\system_of_booking.exe  # Windows (MSVC)
```

> Имя исполняемого файла задаётся в `CMakeLists.txt`. Если оно отличается, поправьте команду выше.

## Тесты

```bash
cmake -S . -B build -DBUILD_TESTING=ON
cmake --build build
cd build && ctest --output-on-failure
```

Покрытие тестами:

| Файл | Что проверяется |
|---|---|
| `test_person.cpp` | Классы `Person`, `Customer`, `Administrator` |
| `test_table.cpp` | Класс `Table` |
| `test_booking.cpp` | Расчёт депозита и описание для всех типов броней |
| `test_repository.cpp` | Добавление, поиск, удаление в `Repository<T>` |
| `test_service.cpp` | Логика `BookingService`, конфликты, отмена |
| `test_storage.cpp` | Сохранение и загрузка из файлов |

## Формат данных

Файлы лежат в `data/`.

`tables.txt` — по одному столику в строке:

```
<id> <seats> <isAvailable>
```

`bookings.txt` — по одной брони в строке:

```
<type> <id> <customer> <tableId> <date> <time> <guests>
```

Пример:

```
STANDARD 1 Ivan 3 2026-10-10 19:00 4
```

> Точный формат уточните по реализации `FileStorage` и поправьте здесь при необходимости.

## Команда

| Участник | Зона ответственности |
|---|---|
| Имя Фамилия 1 | _например: модели (`Person`, `Table`, `Booking`) и их тесты_ |
| Имя Фамилия 2 | _например: `Repository`, `BookingService`, `FileStorage`_ |
| Имя Фамилия 3 | _например: `ConsoleUI`, `main.cpp`, документация и отчёт_ |

## Командная работа

- Правила веток, коммитов и Pull Request: [docs/git_workflow.md](docs/git_workflow.md)
- Протоколы встреч и итерации: [docs/meeting_notes.md](docs/meeting_notes.md)
- Отчёт и презентация: [docs/report/](docs/report/)

## Документация

| Документ | Содержание |
|---|---|
| [requirements.md](docs/requirements.md) | Что умеет система |
| [architecture.md](docs/architecture.md) | Классы, UML-диаграмма |
| [git_workflow.md](docs/git_workflow.md) | Git-процесс команды |
| [meeting_notes.md](docs/meeting_notes.md) | Протоколы встреч |
