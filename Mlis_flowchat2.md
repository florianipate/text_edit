```mermaid
flowchart TD
    A[Admin configures MLIS_Fee__c] --> B{Today between Start_Date__c and End_Date__c?}
    B -- No --> C[Fee inactive]
    B -- Yes --> D{Amount__c > 0 and Amount__c < 5000?}
    D -- No --> C
    D -- Yes --> E[Active fee amount established]

    E --> F{Journey type?}
    F -- Broker portal --> G[Broker Quote Journey]
    F -- Underwriter Salesforce --> H[Underwriter Quote Journey]
    F -- Quick Quote --> I[Quick Quote Journey]

    G --> J[Step 1 complete]
    H --> J
    J --> K[Proceed to Step 2]
    K --> L[Create MLIS submission Opportunity]
    L --> M[Create Fee__c linked to Submission__c]
    M --> N[Set Fee__c.Category__c = New]
    N --> O[Copy active MLIS_Fee__c.Amount__c]
    O --> P[Quote Results Step 3]
    P --> Q{Fee exists and > 0?}
    Q -- No --> R[Hide fee section]
    Q -- Yes --> S[Display fee per quote]
    S --> T[Create Quote Summary document]
    S --> U[Create Policy Specimen Policy]
    T --> V[Include fee section]
    U --> V
    R --> W[No fee section in document output]

    P --> X[Step 4 complete]
    X --> Y[Step 5 Quote Summary]
    Y --> Z{Fee exists and > 0?}
    Z -- No --> AA[Suppress fee section]
    Z -- Yes --> AB[Show fee section and total including fee]
    AB --> AC[Create Final Draft document]
    AC --> AD[Include fee section]
    AA --> AE[Final Draft without fee section]

    AD --> AF[Proceed to order new policy]
    AE --> AF
    AF --> AG[Create Policy Document]
    AF --> AH[Create Debit Note]
    AG --> AI[Include fee section only when fee > 0]
    AH --> AI

    AI --> AJ[Create InsurancePolicy record]
    AJ --> AK[Populate Transactional_DUAL_Fee__c]
    AK --> AL[Create SFI to FFA Transaction]
    AL --> AM[Populate DUAL Fee - DUAL Share Orig CCY]
    AM --> AN[Create BDX__c records]
    AN --> AO[Populate BDX fee fields]

    AO --> AP{Policy cancelled?}
    AP -- No --> AQ[End]
    AP -- Yes --> AR[Create return Fee__c record]
    AR --> AS[Set Category__c = Return]
    AS --> AT[Reverse amount and propagate return accounting]
    AT --> AQ

    I --> BA[Do not create Fee__c]
    BA --> BB[Evaluate active MLIS_Fee__c]
    BB --> BC{Active fee exists and > 0?}
    BC -- No --> BD[Hide fee section in Step 3 and Quick Quote doc]
    BC -- Yes --> BE[Display fee in Step 3 results]
    BE --> BF[Include fee section in Quick Quote document]

    C -. assumption .-> AQ
