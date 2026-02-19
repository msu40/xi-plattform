## Modul-Datenfluss

flowchart TD
    subgraph RECRUIT["🎯 Modul III – AI-Recruit"]
        A[Bewerber-Funnel\nöffentlich, kein Login]
        B[KI-Voice-Screening\nWhisper + LLM]
        C[Scoring & Entscheidung\nQualifiziert / Abgelehnt / Manuell]
        D[Onboarding-Dashboard\nAusweis, Bankdaten, AGB]
        A --> B --> C --> D
    end

    subgraph VERWALTUNG["🗂️ Modul I – Verwaltung & Planung"]
        E[Kundenverwaltung\nStammdaten, Onboarding]
        F[Kampagnenerstellung\n+ Dialfire-Sync]
        G[Agent-Verwaltung\nKonditionen, Stammdaten]
        H[Quota-Tracking\n+ Automatisierungen]
        E --> F
        G --> H
    end

    subgraph SCHEDULING["📅 Modul II – Scheduling"]
        I[Resource Planning Board\nKalender-Ansichten]
        J[Automatisches Matching\nSprache, Zeit, Kapazität]
        K[Konflikt-Erkennung\nWarnungen & Alerts]
        I --> J --> K
    end

    D -->|"Ein-Klick-Übergabe\nProfil + Score + Verfügbarkeit"| G
    D -->|"Agent erscheint sofort\nim Kalender"| I
    F -->|"Kampagnendaten\nZeitfenster, Budget, Sprache"| I
    K -->|"Besetzungsstatus\nvollbesetzt / Lücke / kritisch"| F
    G -->|"Stammdaten\nVerfügbarkeit, Skills"| J

    subgraph EXTERN["🌐 Externe Systeme"]
        L[Dialfire API]
        M[OpenAI Whisper + LLM]
        N[n8n Automatisierung]
        O[S3 Speicher\nAudio-Dateien]
        P[Trello API]
    end

    F <-->|"Kampagnen-Sync\nSkripte, Statistiken"| L
    B -->|"Transkription\n+ Analyse"| M
    N -->|"Webhooks\nE-Mails, Warnungen"| VERWALTUNG
    N -->|"KI-Pipeline\nScoring-Workflow"| RECRUIT
    N -->|"Kapazitäts-Alerts"| SCHEDULING
    B --> O
    C -->|"Score ≥ 90\nTop-Talent"| P 


## Tech-Stack 

flowchart TD
    subgraph VM["🖥️ Debian VM"]
        subgraph DOCKER["🐳 Docker"]
            APP[Web-App\nFrontend + Backend]
            N8N[n8n\nAutomatisierung]
            DB[(PostgreSQL\nZentrale Datenbank)]
            S3[MinIO\nS3-kompatibler Speicher]
        end
        GIT[GitHub Arbeitsaccount\nxi-plattform Repo\nFine-grained Token]
    end

    subgraph CLOUD["☁️ Externe APIs"]
        OPENAI[OpenAI\nWhisper + LLM]
        DIALFIRE[Dialfire API\nKampagnen-Management]
        TRELLO[Trello API\nTop-Talent-Karten]
        SMTP[SMTP-Dienst\nE-Mail-Versand]
    end

    subgraph USER["👥 Nutzer"]
        SUPPORT[Support / Admin\nBrowser]
        AGENT[Call-Agent\nBrowser / Mobile]
        BEWERBER[Bewerber\nöffentlicher Funnel]
    end

    SUPPORT --> APP
    AGENT --> APP
    BEWERBER --> APP

    APP <--> DB
    APP <--> S3
    APP --> N8N

    N8N <--> DB
    N8N --> OPENAI
    N8N --> TRELLO
    N8N --> SMTP
    N8N <--> DIALFIRE

    GIT -->|"CI/CD\nFeature Branches + PRs"| APP
