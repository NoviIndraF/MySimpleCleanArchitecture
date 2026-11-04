# MySimpleCleanArchitecture

A minimal Android app in **Kotlin** that shows the three layers of **Clean Architecture** with a single feature: building a welcome message. Written while following the Clean Architecture module of a [Dicoding](https://www.dicoding.com/) Android course.

The screen shows one text: `Hello Dicoding! Welcome to Clean Architecture`.

## How it works

```
presentation/   MainActivity  →  MainViewModel  (LiveData<MessageEntity>)
                                     │ uses
domain/         MessageUseCase  ←  MessageInteractor
                                     │ depends on the interface
                IMessageRepository, MessageEntity
                                     ▲ implemented by
data/           MessageRepository  →  IMessageDataSource  ←  MessageDataSource
di/             Injection (manual dependency injection)
```

1. `MainActivity` gets `MainViewModel` from `MainViewModelFactory` and calls `setName("Dicoding")`.
2. `MainViewModel` calls `MessageUseCase.getMessage(name)`.
3. `MessageInteractor` asks `IMessageRepository` for the welcome message.
4. `MessageRepository` reads it from `MessageDataSource`, which returns `MessageEntity("Hello $name! Welcome to Clean Architecture")`.

The domain layer has no Android dependencies and only knows the repository interface, so the data source can change without touching the use case or the UI.

## Tests

`app/src/test/.../domain/MessageUseCaseTest.kt` checks the use case with a Mockito mock of `IMessageRepository`:

```bash
./gradlew test
```

## Tech stack

- Kotlin, AndroidX AppCompat, ViewModel, LiveData, View Binding
- JUnit 4 and Mockito
- Android Gradle Plugin 4.2.2, Gradle 6.7.1 (use JDK 11)
