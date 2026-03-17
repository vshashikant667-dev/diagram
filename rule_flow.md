```mermaid
   graph TD
        %% Section 1: Listening to the World
        subgraph "1. LISTENING PHASE"
        K[Kafka 'Radio'] -- "Broadcasts: 'RELIANCE is suspended!'" --> M[main.go 'Secretary']
        end
   
        %% Section 2: Saving the Rules
        subgraph "2. SAVING PHASE (The Chalkboard)"
        M -- "Writes Update" --> R[(Redis Database)]
        R -- "Store Flags" --> S_Data[Script/Stock Status]
        R -- "Store Rules" --> P_Rule[Price Range Rules]
        R -- "Toggle On/Off" --> T_Rules[Enabled Rules List]
        end
   
        %% Section 3: Checking the Order
        subgraph "3. VALIDATION PHASE"
        Order[New Order Request] --> V[Order Validator]
        V -- "Reads Current Rules" --> R
        V -- "Run Logic" --> Check{Are Rules Broken?}
        Check -- "Yes" --> Fail[Return: '1010' + Violation Message]
        Check -- "No" --> Pass[Return: '0000' + Success]
        end
