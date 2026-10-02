# system_of_booking
system_of_booking/
├── README.md                 # описание, как собрать и запустить
├── CMakeLists.txt
├── .gitignore                # build/, .vscode/ и т.д.
├── docs/
│   ├── architecture.md       # классы, UML-диаграмма
│   ├── requirements.md       # что умеет система
│   ├── git_workflow.md       # правила веток, PR, коммитов
│   ├── meeting_notes.md      # протоколы встреч, итерации
│   └── report/               # отчёт и презентация
├── include/
│   ├── Person.h
│   ├── Customer.h
│   ├── Administrator.h
│   ├── Table.h
│   ├── Booking.h             # абстрактный класс
│   ├── StandardBooking.h
│   ├── VipBooking.h
│   ├── BanquetBooking.h
│   ├── Repository.h          # шаблонный класс
│   ├── BookingService.h      # логика бронирования
│   ├── FileStorage.h
│   └── ConsoleUI.h
├── src/
│   ├── (.cpp для каждого класса)
│   └── main.cpp
├── tests/
│   ├── CMakeLists.txt
│   ├── test_person.cpp
│   ├── test_table.cpp
│   ├── test_booking.cpp
│   ├── test_repository.cpp
│   ├── test_service.cpp
│   └── test_storage.cpp
└── data/                     # файлы с данными (tables.txt, bookings.txt)


Person (abstract)               Booking (abstract)
 ├─ name, phone                  ├─ id, customer, table, date, time, guests
 ├─ virtual getRole() = 0        ├─ virtual calculateDeposit() = 0
 ├─ Customer                     ├─ virtual getDescription()
 └─ Administrator                ├─ StandardBooking  (депозит 0)
                                 ├─ VipBooking       (фикс. депозит)
Table                            └─ BanquetBooking   (депозит за гостя)
 └─ id, seats, isAvailable
                                 Repository<T>  → Repository<Table>, Repository<Booking>
BookingService
 └─ createBooking, cancelBooking, findFreeTables,
    hasConflict, getBookingsByDate





    
