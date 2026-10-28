 Architecture & System Diagrams


graph TD
    User((☕ Cafe Manager))

    subgraph Elcapitano Cafe System
        UC1[Manage Sales / Record Transaction]
        UC2[Edit / Delete Past Sales]
        UC3[Manage Customer Debts]
        UC4[Manage Item Prices & Menu]
        UC5[View Real-Time Analytics & Peak Hours]
        UC6[Live Cloud Sync]
    end

    User --> UC1
    User --> UC2
    User --> UC3
    User --> UC4
    User --> UC5

    UC1 .->|triggers| UC6
    UC2 .->|triggers| UC6
    UC3 .->|triggers| UC6
    UC4 .->|triggers| UC6

sequenceDiagram
    autonumber
    actor Manager as Cafe Manager (Laptop/Phone)
    participant UI as Web Interface (DOM)
    participant App as App JS Engine
    participant DB as Firebase Firestore
    participant Device2 as Other Synced Device

    Manager->>UI: Selects item, enters quantity, clicks "إتمام البيع"
    UI->>App: Triggers submit event listener
    App->>App: Calculates total price & extracts current hour
    App->>DB: addDoc(salesCol, payload)
    
    rect rgb(20, 30, 45)
        note over DB, Device2: Real-time Listener Push
        DB-->>App: onSnapshot() triggers with updated sales array
        DB-->>Device2: onSnapshot() triggers instantly
    end

    App->>App: Recalculates stats (Daily, Weekly, Monthly)
    App->>App: Recalculates peak/lowest hours
    App->>UI: Re-renders Sales Table & Chart.js graph
    Device2->>Device2: Re-renders Sales Table & Chart.js graph
```
