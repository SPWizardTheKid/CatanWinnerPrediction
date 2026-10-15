# CatanWinnerPrediction

## Project Overview
This information system is designed to demonstrate the capabilities of generative AI by predicting the winner of a "Settlers of Catan" board game based on the players' initial moves. The system takes the Kaggle Catan dataset, hides all information except for each player's two starting settlements, and asks an LLM model which player won.

## Repository Structure
* `/data/` – Directory containing the testing data (X, y pairs).
* `/prompts/` – Directory containing the testing prompts submitted to the AI agent.
* `catan_prediction.ipynb` – The source code and working system prototype, adapted for the Google Colab environment.

## UMLs

<details>
<summary>Mermaid UML</summary>

```mermaid

graph TD
    User((User)) <-->|Chats & inputs tiles| UI
    
    subgraph Frontend
        UI[Chat Window UI<br/>Input tiles, view predictions]
    end
    
    UI <-->|API calls JSON| MC
    
    subgraph Backend Application
        MC[Main Controller]
        AI[GenAI Engine<br/>Outcome Predictor]
    end
    
    MC -->|Fetches historical data| DB
    DB -->|Returns context| MC
    
    MC <-->|Sends prompt &<br/>Receives prediction| AI
    
    subgraph Game Data
        DB[(Database<br/>Starting Tiles, Win Stats, History)]
    end
```
</details>

## Sequence Diagram

<details>
<summary>Mermaid Sequence Diagram</summary>

```mermaid
sequenceDiagram
    actor User
    participant UI as Chat Window UI
    participant Backend as Main Controller
    participant DB as Game Data DB
    participant AI as GenAI Engine

    User->>UI: Enters starting tiles & asks for prediction
    activate UI
    
    UI->>Backend: Send prediction request (JSON)
    activate Backend
    
    Backend->>DB: Query game history & win stats
    activate DB
    DB-->>Backend: Return historical context
    deactivate DB
    
    Backend->>AI: Send prompt (User tiles + DB context)
    activate AI
    AI-->>Backend: Return generated prediction
    deactivate AI
    
    Backend-->>UI: Send prediction response
    deactivate Backend
    
    UI-->>User: Display predicted outcome
    deactivate UI
```
</details>

## PlantUML 



<details>

```plantuml
@startuml
!theme plain
skinparam componentStyle rectangle

actor "User" as user

node "Frontend" {
    component "Chat Window UI\n(Input tiles, view predictions)" as chatUI
}

node "Backend Application" {
    component "Main Controller" as app
    component "GenAI Engine\n(Outcome Predictor)" as genAI
}

database "Game Data" as db {
    artifact "Starting Tiles" as tiles
    artifact "Win Statistics" as stats
    artifact "Game History" as history
}

user <--> chatUI : Chats & inputs starting tiles
chatUI <--> app : API calls (JSON)
app --> db : Fetches historical data & stats
db --> app : Returns context
app <--> genAI : Sends prompt (tiles + history)\nReceives generated prediction
@enduml
```
</details>

<img width="799" height="716" alt="plantuml2" src="https://github.com/user-attachments/assets/900f1950-17f9-45f0-995a-24376d0021ea" />


<details>
<summary>PlantUML Sequence</summary>

```plantuml
@startuml
!theme plain
actor User
participant "Chat Window UI" as UI
participant "Main Controller" as Backend
database "Game Data DB" as DB
participant "GenAI Engine" as AI

User -> UI: Enters starting tiles & asks for prediction
activate UI
UI -> Backend: Send prediction request (JSON)
activate Backend
Backend -> DB: Query game history & win stats
activate DB
DB --> Backend: Return historical context
deactivate DB
Backend -> AI: Send prompt (User tiles + DB context)
activate AI
AI --> Backend: Return generated prediction
deactivate AI
Backend --> UI: Send prediction response
deactivate Backend
UI --> User: Display predicted outcome
deactivate UI
@enduml
```
</details>

<img width="974" height="415" alt="plantunml_seq1" src="https://github.com/user-attachments/assets/3ba7e8b5-7571-452c-b143-17859f4313bc" />

## Google Colab Prototype

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SPWizardTheKid/CatanWinnerPrediction/blob/main/Catan_winner_prediction_simple.ipynb)

**System Overview:** This prototype demonstrates a functioning AI system built using the Gemini API. The program reads game data, formats it into text prompts for the language model, and processes the returned winner predictions. Finally, the model's predictions are evaluated against actual winners to provide accuracy statistics.

**How to Run:**
1. Click the "Open In Colab" button above.
2. In Google Colab's left sidebar, open "Secrets" (the 🔑 icon).
3. Add your API key as a secret named `GOOGLE_API_KEY` and ensure "Notebook access" is enabled.
4. From the top menu, click **Runtime** -> **Run all** to execute the notebook and view the AI agent's performance.
