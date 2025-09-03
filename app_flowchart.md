flowchart TD
    Start[Visitor] --> MainPage[Main Page with Search]
    MainPage --> SearchPublic[Search Public Users/Projects]
    MainPage --> Auth[Login via Google/GitHub/Twitter OAuth]

    Auth --> AuthCheck{Authenticated}
    AuthCheck -->|Yes| Dashboard[User Dashboard]
    AuthCheck -->|No| MainPage

    Dashboard --> Directory[View/Search Project Directory]
    Dashboard --> Projects[Manage Projects]
    Dashboard --> Settings[User Settings]
    Dashboard --> Subscription{Subscription Tier}
    Dashboard --> SocialAPI[Integrate Social APIs]
    Dashboard --> MobileApp[Mobile App]
    Dashboard --> Notifications[Email Notifications]

    Projects --> ProjectForm[Create or Edit Project]
    ProjectForm --> ProjectDetails[Enter Project Details]
    ProjectDetails --> Visibility[Set Visibility (Public/Private)]
    Visibility --> MediaUpload[Upload Media and Links]
    MediaUpload --> SaveDB[Save to Supabase]

    Subscription -->|Free| Ads[Show Ads + Default Design]
    Subscription -->|Paid| PaidFeatures[Unlock Premium Features]
    PaidFeatures --> Customization[Customization Editor]
    PaidFeatures --> DesignCatalog[Design Catalog]
    PaidFeatures --> AIAssistant[AI Design Assistant (Real-Time)]
    PaidFeatures --> Analytics[Analytics Dashboard]

    Subscription --> Cancel[Cancel Premium/Pro]
    Cancel --> RevertFree[Revert to Free Plan Features]
    RevertFree --> Ads
    RevertFree --> LockCustomization[Customization Locked]
    RevertFree --> DisableAI[AI Assistant Disabled]
    RevertFree --> HideAnalytics[Analytics Hidden]

    SocialAPI --> FetchMetrics[Fetch Engagement Metrics]
    FetchMetrics --> DisplayMetrics[Store & Display Metrics]

    MobileApp --> MobileAuth[Authenticate OAuth]
    MobileApp --> QRScan[QR Code Sharing]
    MobileAuth --> Dashboard
    QRScan --> UserProfile

    Notifications --> SendGrid[Emails via SendGrid]
