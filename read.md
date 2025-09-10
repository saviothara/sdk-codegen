graph TB
    %% Define styling
    classDef userInterface fill:#FFF2CC,stroke:#D6B656,stroke-width:2px
    classDef authentication fill:#D5E8D4,stroke:#82B366,stroke-width:2px
    classDef coreProcessing fill:#E1D5E7,stroke:#9673A6,stroke-width:2px
    classDef aiServices fill:#DAE8FC,stroke:#6C8EBF,stroke-width:2px
    classDef storage fill:#F8CECC,stroke:#B85450,stroke-width:2px
    classDef infrastructure fill:#FFE6CC,stroke:#D79B00,stroke-width:2px
    classDef cicd fill:#E6F3FF,stroke:#4A90E2,stroke-width:2px
    classDef monitoring fill:#F0F0F0,stroke:#666666,stroke-width:2px

    %% User Interface Layer
    subgraph UI["👥 User Interface"]
        Users[👤 Users]
        GChat[💬 Google Chat<br/>Spaces/DMs]
    end

    %% Authentication Layer
    subgraph AUTH["🔐 Authentication & Authorization"]
        EAPAuth[🏢 EAP Chat Bot<br/>Auth Service<br/>eap-chat-bot-auth-np.cloudapps.telus.com]
        GoogleOAuth[🔑 Google OAuth 2.0]
        OAuthCreds[(🔐 OAuth Client<br/>Credentials)]
    end

    %% Core Processing
    subgraph CORE["⚙️ Core Chat Bot Processing"]
        ChatBot[🤖 Google Chat Bot<br/>Cloud Function]
        OAuthMgr[🔒 OAuth Manager]
        ChatMgr[💬 Chat Manager]
        LLMMgr[🧠 LLM Manager]
        MainHandler[⚡ Main Handler]
        PubSub[📨 Pub/Sub<br/>Message Queue]
        EventProc[🔄 Event Processing]
    end

    %% AI Services
    subgraph AI["🧠 AI & Knowledge Services"]
        VertexSearch[🔍 Vertex AI Search]
        LLMIntegration[🤖 LLM Integration<br/>OpenAI/etc]
        PromptMgmt[📝 Prompt Management]
        DocContext[(📚 Document Context)]
    end

    %% Storage
    subgraph STORAGE["💾 Storage & Secrets"]
        SecretMgr[(🔐 Google Secret Manager)]
        UserSecrets[👤 User Credentials<br/>{secret}-{user_id}]
        OAuthConfig[⚙️ OAuth Client Config]
        AppCreds[🔑 App Credentials]
        GCS[(☁️ Google Cloud Storage)]
        Datastore[(🗄️ Datastore)]
    end

    %% Infrastructure
    subgraph INFRA["🏗️ Infrastructure & Deployment"]
        GKE[☸️ Google Kubernetes Engine<br/>private-na-ne1-001]
        AuthPods[📦 EAP Auth Service<br/>Pods]
        Ingress[🌐 Ingress Controller<br/>nginx]
        LoadBalancer[⚖️ Cloud Load Balancer]
        ArtifactRegistry[📦 Artifact Registry]
    end

    %% CI/CD Pipeline
    subgraph CICD["🚀 CI/CD Pipeline"]
        GitHub[📁 GitHub Repository]
        GitHubActions[⚡ GitHub Actions<br/>Workflow]
        CloudBuild[🔨 Cloud Build<br/>Private Pool]
        CloudDeploy[🚀 Cloud Deploy<br/>Pipeline]
        Skaffold[📋 Skaffold<br/>Build Config]
        Helm[⚙️ Helm Charts]
    end

    %% Monitoring
    subgraph MONITORING["📊 Monitoring & Observability"]
        CloudLogging[📝 Cloud Logging]
        AppLogging[📋 Application Logging]
        ErrorTracking[🚨 Error Tracking]
        HealthMonitoring[❤️ Health Monitoring]
    end

    %% User Flow
    Users --> GChat
    GChat --> PubSub
    PubSub --> ChatBot
    ChatBot --> MainHandler

    %% OAuth Flow
    ChatBot --> OAuthMgr
    OAuthMgr --> SecretMgr
    OAuthMgr --> EAPAuth
    EAPAuth --> GoogleOAuth
    GoogleOAuth --> EAPAuth
    EAPAuth --> SecretMgr

    %% Message Processing Flow
    ChatBot --> ChatMgr
    ChatMgr --> VertexSearch
    VertexSearch --> DocContext
    ChatBot --> LLMMgr
    LLMMgr --> LLMIntegration
    LLMMgr --> PromptMgmt
    ChatBot --> GChat

    %% CI/CD Flow
    GitHub --> GitHubActions
    GitHubActions --> CloudBuild
    CloudBuild --> Skaffold
    Skaffold --> ArtifactRegistry
    CloudBuild --> CloudDeploy
    CloudDeploy --> Helm
    Helm --> GKE

    %% Infrastructure Connections
    LoadBalancer --> Ingress
    Ingress --> AuthPods
    AuthPods --> GKE
    EAPAuth --> SecretMgr
    ChatBot --> GCS
    ChatBot --> Datastore

    %% Secret Manager Internal
    SecretMgr --> UserSecrets
    SecretMgr --> OAuthConfig
    SecretMgr --> AppCreds

    %% Monitoring Connections
    ChatBot --> CloudLogging
    EAPAuth --> AppLogging
    GKE --> HealthMonitoring
    ChatBot --> ErrorTracking

    %% Apply styles
    class Users,GChat userInterface
    class EAPAuth,GoogleOAuth,OAuthCreds authentication
    class ChatBot,OAuthMgr,ChatMgr,LLMMgr,MainHandler,PubSub,EventProc coreProcessing
    class VertexSearch,LLMIntegration,PromptMgmt,DocContext aiServices
    class SecretMgr,UserSecrets,OAuthConfig,AppCreds,GCS,Datastore storage
    class GKE,AuthPods,Ingress,LoadBalancer,ArtifactRegistry infrastructure
    class GitHub,GitHubActions,CloudBuild,CloudDeploy,Skaffold,Helm cicd
    class CloudLogging,AppLogging,ErrorTracking,HealthMonitoring monitoring
