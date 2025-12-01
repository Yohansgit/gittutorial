```mermaid
flowchart TD
    %% --- Node Styles ---
    classDef start fill:#4CAF50,stroke:#1B5E20,color:#fff;
    classDef data fill:#FF8A80,stroke:#C62828,color:#000;
    classDef process fill:#26C6DA,stroke:#00838F,color:#000;
    classDef model fill:#FF7043,stroke:#E64A19,color:#fff;
    classDef decision fill:#FDD835,stroke:#F9A825,color:#000;
    classDef output fill:#66BB6A,stroke:#2E7D32,color:#fff;
    classDef monitor fill:#26A69A,stroke:#004D40,color:#fff;

    %% --- Horizontal pipeline steps ---
    subgraph TOPROW [ ]
    direction LR
      A1(1. Define Objective):::start
      A2([2. 🔄 Data Acquisition<br>RNA-seq, Clinical, Gencode]):::data
      A3([3. ⚙️ Preprocess Data]):::process
      A4([4. 📊 PCA Analysis]):::process
      A5([5. 🧬 Identify PAM50 & ✨ Feature Selection]):::process
      A6([6. 📝 Select Model<br>(Random Forest)]):::model
    end

    %% --- Vertical flow after model selection ---
    A6 --> B1([7. 🤖 Train Model]):::model
    B1 --> B2([8. Cross Validation]):::model
    B2 --> C1{9. Performance OK?}:::decision

    C1 -- "No" --> D1([10. 🔁 Hyperparameter Tuning]):::model
    D1 -.->|Tune & Retry| A6
    C1 -- "Yes" --> E1{{11. Identify Biomarkers<br>Top 50 Genes}}:::output
    E1 --> F1(12. 🚀 Deployment):::output
    F1 --> G1[/13. Model Monitoring<br>Drift Detection/]:::monitor
    G1 -- "Drift Detected" --> H1([14. 🔁 Retrain Model]):::monitor
    H1 -.->|Cycle| A6

    %% --- Horizontal links for first row ---
    A1 --> A2 --> A3 --> A4 --> A5 --> A6

    %% --- Dotted feedback arrows for loops ---
    linkStyle 12 stroke:#1565C0,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 14 stroke:#1565C0,stroke-width:2px,stroke-dasharray: 5 5
```

