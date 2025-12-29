# Security & Privacy

MIRIX is designed with privacy and security as core principles. All sensitive data processing happens locally, with user-controlled privacy settings and enterprise-grade security practices.

## Privacy Architecture

### Data Flow Overview

#### Memory Updates Workflow

<div class="light-theme-diagram" markdown="1">
```mermaid
graph TB
    A[Screen Capture] --> B[Temporary Cloud Storage<br/>Your Google Cloud]
    B --> C[Local Processing<br/>MIRIX Agents]
    C --> D[Local Database<br/>PostgreSQL/SQLite]
    
    B --> E[Auto-Delete<br/>After Processing]
    
    style A fill:#e6f3ff
    style B fill:#ffcccc
    style C fill:#e6f3ff
    style D fill:#ccffcc
    style E fill:#ccffcc
    
    classDef cloud fill:#ffcccc,stroke:#ff6666
    classDef local fill:#ccffcc,stroke:#66cc66
    classDef process fill:#e6f3ff,stroke:#3399ff
    
    class B cloud
    class D,E local
    class A,C process
```
</div>

<div class="dark-theme-diagram" markdown="1">
```mermaid
graph TB
    A[Screen Capture] --> B[Temporary Cloud Storage<br/>Your Google Cloud]
    B --> C[Local Processing<br/>MIRIX Agents]
    C --> D[Local Database<br/>PostgreSQL/SQLite]
    
    B --> E[Auto-Delete<br/>After Processing]
    
    style A fill:#4a90e2,stroke:#2c5282,color:#ffffff
    style B fill:#e74c3c,stroke:#c0392b,color:#ffffff
    style C fill:#4a90e2,stroke:#2c5282,color:#ffffff
    style D fill:#27ae60,stroke:#1e8449,color:#ffffff
    style E fill:#27ae60,stroke:#1e8449,color:#ffffff
    
    classDef cloud fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef local fill:#27ae60,stroke:#1e8449,color:#ffffff
    classDef process fill:#4a90e2,stroke:#2c5282,color:#ffffff
    
    class B cloud
    class D,E local
    class A,C process
```
</div>

#### Query Response Workflow

<div class="light-theme-diagram" markdown="1">
```mermaid
graph TB
    F[User Query] --> G[Local Memory<br/>Database]
    F --> H[Recent Screenshots<br/>Only Last 20 in RAM]
    G --> I[Gemini Processing<br/>Cloud API]
    H --> I
    I --> J[Local Response<br/>Generation]
    J --> K[Response to User]
    
    L[Older Screenshots] --> M[Automatically Deleted<br/>Not Stored Anywhere]
    
    style F fill:#e6f3ff
    style G fill:#ccffcc
    style H fill:#ffffcc
    style I fill:#ffcccc
    style J fill:#e6f3ff
    style K fill:#ccffcc
    style L fill:#cccccc
    style M fill:#cccccc
    
    classDef cloud fill:#ffcccc,stroke:#ff6666
    classDef local fill:#ccffcc,stroke:#66cc66
    classDef process fill:#e6f3ff,stroke:#3399ff
    classDef minimal fill:#ffffcc,stroke:#ffcc00
    classDef deleted fill:#cccccc,stroke:#999999
    
    class I cloud
    class G,K local
    class F,J process
    class H minimal
    class L,M deleted
```
</div>

<div class="dark-theme-diagram" markdown="1">
```mermaid
graph TB
    F[User Query] --> G[Local Memory<br/>Database]
    F --> H[Recent Screenshots<br/>Only Last 20 in RAM]
    G --> I[Gemini Processing<br/>Cloud API]
    H --> I
    I --> J[Local Response<br/>Generation]
    J --> K[Response to User]
    
    L[Older Screenshots] --> M[Automatically Deleted<br/>Not Stored Anywhere]
    
    style F fill:#4a90e2,stroke:#2c5282,color:#ffffff
    style G fill:#27ae60,stroke:#1e8449,color:#ffffff
    style H fill:#f39c12,stroke:#d68910,color:#ffffff
    style I fill:#e74c3c,stroke:#c0392b,color:#ffffff
    style J fill:#4a90e2,stroke:#2c5282,color:#ffffff
    style K fill:#27ae60,stroke:#1e8449,color:#ffffff
    style L fill:#7f8c8d,stroke:#5d6d7e,color:#ffffff
    style M fill:#7f8c8d,stroke:#5d6d7e,color:#ffffff
    
    classDef cloud fill:#e74c3c,stroke:#c0392b,color:#ffffff
    classDef local fill:#27ae60,stroke:#1e8449,color:#ffffff
    classDef process fill:#4a90e2,stroke:#2c5282,color:#ffffff
    classDef minimal fill:#f39c12,stroke:#d68910,color:#ffffff
    classDef deleted fill:#7f8c8d,stroke:#5d6d7e,color:#ffffff
    
    class I cloud
    class G,K local
    class F,J process
    class H minimal
    class L,M deleted
```
</div>

### Privacy Principles

1. **Local Data Storage**: All long-term user data remains on your local machine
2. **User-Controlled Cloud**: Only your personal Google Cloud account is used for temporary storage
3. **Automatic Cleanup**: Screenshots are automatically deleted after processing
4. **No Third-Party Sharing**: Your data never leaves your control
5. **Transparent Processing**: All data processing is documented and auditable

## Screenshot Handling

### Capture Process

```python
# MIRIX screenshot workflow
def screenshot_workflow():
    # 1. Capture screenshot every second
    screenshot = capture_screen()
    
    # 2. Upload to YOUR Google Cloud storage
    upload_to_user_cloud(screenshot, user_bucket)
    
    # 3. Keep only recent 600 screenshots (~10 minutes)
    maintain_recent_screenshots(limit=600)
    
    # 4. Process screenshots through agents
    process_with_agents(screenshot)
    
    # 5. Delete processed screenshots
    delete_processed_screenshots()
```

## What's Next?

Explore backup and restore features:

[**Backup & Restore →**](backup-restore.md){ .md-button } 