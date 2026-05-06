# Практична робота: Побудова поведінкових UML-діаграм
**Тема:** Проєктування системи забезпечення захищеного зв'язку (Secure Communication System)

## Опис проєкту
Система призначена для автоматизації процесів встановлення, моніторингу та відновлення каналів зв'язку. Вона забезпечує стабільне з'єднання між віддаленими абонентами через різні типи каналів із використанням шифрування.

---

## 1. Діаграма варіантів використання (Use Case Diagram)
```mermaid
flowchart LR
    User(["Абонент"])
    Signalman(["Зв'язківець"])
    NetAdmin(["Адмін мережі"])

    subgraph "Система зв'язку"
        UC1("Встановлення з'єднання")
        UC2("Шифрування каналу")
        UC3("Моніторинг якості сигналу")
        UC4("Конфігурація обладнання")
        UC5("Усунення несправностей")
    end

    User --> UC1
    UC1 -.-> UC2
    Signalman --> UC1
    Signalman --> UC3
    Signalman --> UC4
    NetAdmin --> UC4
    NetAdmin --> UC5
```

---

## 2. Діаграма послідовності (Sequence Diagram)

```mermaid
sequenceDiagram
    autonumber
    participant NodeA as Вузол А (Термінал)
    participant Server as Центр комутації
    participant Crypto as Модуль шифрування
    participant NodeB as Вузол Б (Отримувач)

    NodeA->>Server: Запит на встановлення каналу
    Server->>Crypto: Генерація сесійних ключів
    Crypto-->>Server: Ключі готові
    Server->>NodeB: Запит на автентифікацію
    NodeB-->>Server: Підтвердження (Token)
    Server->>NodeA: Канал встановлено (Зашифровано)
    
    Note over NodeA, NodeB: Передача оперативної інформації
    
    NodeA->>NodeB: Пакет даних (Encrypted)
    NodeB-->>NodeA: Підтвердження отримання (ACK)
```

---

## 3. Діаграма діяльності (Activity Diagram)
```mermaid
stateDiagram-v2
    [*] --> LinkMonitoring
    LinkMonitoring --> SignalEvaluation
    
    state "Сигнал в нормі?" as check_signal <<choice>>
    SignalEvaluation --> check_signal
    
    check_signal --> LinkMonitoring : Так
    check_signal --> InterferenceDetected : Ні
    
    state InterferenceDetected {
        [*] --> FrequencyHopping
        FrequencyHopping --> Rerouting
    }
    
    Rerouting --> LinkRestored
    
    state "Зв'язок відновлено?" as check_recovery <<choice>>
    LinkRestored --> check_recovery
    
    check_recovery --> LinkMonitoring : Так
    check_recovery --> EmergencyProtocol : Ні
    
    EmergencyProtocol --> [*]
```

---
**Виконав:** [Ваше Ім'я]
