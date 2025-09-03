flowchart TD
    Start[Start] --> Auth[Authenticate with Google OAuth]
    Auth --> AuthCheck{Authenticated}
    AuthCheck -->|Yes| Dashboard[User Dashboard]
    AuthCheck -->|No| Auth
    Dashboard --> Directory[View Project Directory]
    Dashboard --> Projects[Manage Projects]
    Dashboard --> Settings[User Settings]
    Directory --> Search[Search Users]
    Search --> UserProfile[Open User Profile]
    UserProfile --> ViewProjects[View User Projects]
    Projects --> ProjectForm[Create or Edit Project]
    ProjectForm --> ProjectDetails[Enter Project Details]
    ProjectDetails --> MediaUpload[Upload Media and Links]
    MediaUpload --> SaveDB[Save to Supabase]
    Dashboard --> Subscription{Subscription Tier}
    Subscription -->|Free| Ads[Show Ads]
    Subscription -->|Paid| PaidFeatures[Unlock Paid Features]
    PaidFeatures --> Customization[Customization Editor]
    PaidFeatures --> DesignCatalog[Design Catalog]
    PaidFeatures --> AIAssistant[AI Design Assistant]
    Dashboard --> SocialAPI[Integrate Social APIs]
    SocialAPI --> FetchMetrics[Fetch Engagement Metrics]
    FetchMetrics --> DisplayMetrics[Store and Display Metrics]
    Dashboard --> MobileApp[Mobile App]
    MobileApp --> MobileAuth[Authenticate via Google OAuth]
    MobileApp --> QRScan[QR Code Scanning]
    MobileAuth --> Dashboard
    QRScan --> UserProfile
    Dashboard --> Notifications[Email Notifications]
    Notifications --> SendGrid[Send Welcome and Updates]