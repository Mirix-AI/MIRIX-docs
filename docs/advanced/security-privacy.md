# Security & Privacy

Mirix is designed with privacy and security as core principles. All sensitive data processing happens locally, with user-controlled privacy settings and enterprise-grade security practices.

## Privacy Architecture

### Data Flow Overview

```mermaid
graph TB
    A[Screen Capture] --> B[Temporary Cloud Storage<br/>Your Google Cloud]
    B --> C[Local Processing<br/>Mirix Agents]
    C --> D[Local Database<br/>PostgreSQL/SQLite]
    
    B --> E[Auto-Delete<br/>After Processing]
    
    F[User Queries] --> C
    C --> G[Local Responses]
    
    H[Backup Data] --> I[Local Storage Only]
    
    style B fill:#ffcccc
    style D fill:#ccffcc
    style I fill:#ccffcc
    
    classDef cloud fill:#ffcccc,stroke:#ff6666
    classDef local fill:#ccffcc,stroke:#66cc66
    
    class B cloud
    class D,I local
```

### Privacy Principles

1. **Local Data Storage**: All long-term user data remains on your local machine
2. **User-Controlled Cloud**: Only your personal Google Cloud account is used for temporary storage
3. **Automatic Cleanup**: Screenshots are automatically deleted after processing
4. **No Third-Party Sharing**: Your data never leaves your control
5. **Transparent Processing**: All data processing is documented and auditable

## Screenshot Handling

### Capture Process

```python
# Mirix screenshot workflow
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

### Privacy Controls

#### Screenshot Exclusions
```python
# Configure applications to exclude from capture
excluded_apps = [
    "Banking App",
    "Password Manager", 
    "Private Browser",
    "Secure Messaging"
]

# Exclude specific window titles
excluded_titles = [
    "*password*",
    "*private*",
    "*incognito*"
]
```

#### Time-Based Controls
```python
# Disable capture during specific hours
privacy_schedule = {
    "disable_hours": "22:00-06:00",  # No capture at night
    "disable_days": ["Saturday", "Sunday"],  # Weekend privacy
    "break_intervals": 30  # 30-minute break every 2 hours
}
```

## Local Data Storage

### Database Security

#### PostgreSQL Security
```sql
-- Enable row-level security
ALTER TABLE memory_table ENABLE ROW LEVEL SECURITY;

-- Create security policies
CREATE POLICY user_memory_policy ON memory_table
    USING (user_id = current_user_id());

-- Encrypt sensitive columns
CREATE EXTENSION IF NOT EXISTS pgcrypto;
```

#### SQLite Security
```python
# Enable encryption for SQLite
import sqlite3
from pysqlcipher3 import dbapi2 as sqlite

# Encrypted database connection
conn = sqlite.connect("mirix.db")
conn.execute("PRAGMA key='your_encryption_key'")
```

### File System Security

```python
# Secure file permissions
import os
import stat

def secure_file_permissions(file_path):
    # Set read/write for owner only
    os.chmod(file_path, stat.S_IRUSR | stat.S_IWUSR)

# Apply to all Mirix files
secure_directories = [
    "~/.mirix/",
    "~/.mirix/backups/",
    "~/.mirix/logs/"
]
```

## Memory Component Security

### Knowledge Vault Security

The Knowledge Vault handles sensitive information with special security measures:

#### Sensitivity Classification
```python
sensitivity_levels = {
    "low": {
        "encryption": False,
        "access_logging": False,
        "examples": ["bookmarks", "public notes"]
    },
    "medium": {
        "encryption": True,
        "access_logging": True,
        "examples": ["contact info", "work documents"]
    },
    "high": {
        "encryption": True,
        "access_logging": True,
        "access_control": True,
        "examples": ["passwords", "API keys", "financial data"]
    }
}
```

#### Secure Storage
```python
# High-sensitivity data handling
def store_sensitive_data(data, sensitivity="high"):
    if sensitivity == "high":
        # Encrypt data
        encrypted_data = encrypt_with_user_key(data)
        
        # Log access
        log_access_attempt(data_type="sensitive", action="store")
        
        # Store with restricted access
        store_with_permissions(encrypted_data, permissions="user_only")
    
    return data_id
```

### Access Control

```python
class SecureMemoryAccess:
    def __init__(self, user_id, session_key):
        self.user_id = user_id
        self.session_key = session_key
        self.access_log = []
    
    def access_memory(self, memory_id, memory_type):
        # Verify user permissions
        if not self.verify_access(memory_id, memory_type):
            raise PermissionError("Access denied")
        
        # Log access
        self.log_access(memory_id, memory_type)
        
        # Return data based on sensitivity
        return self.get_filtered_data(memory_id, memory_type)
```

## Cloud Storage Security

### Google Cloud Configuration

```yaml
# Secure cloud storage configuration
google_cloud:
  project_id: "your-personal-project"
  bucket_name: "mirix-screenshots-private"
  location: "us-central1"
  
  security:
    encryption: "GOOGLE_MANAGED"  # Or customer-managed keys
    access_control: "PRIVATE"
    lifecycle_rules:
      - delete_after_hours: 24
      - archive_after_minutes: 60
```

### Authentication Security

```python
# Secure authentication setup
def setup_secure_auth():
    # Use service account with minimal permissions
    credentials = service_account.Credentials.from_service_account_info({
        "type": "service_account",
        "project_id": "your-project",
        "private_key_id": "key-id",
        "private_key": "-----BEGIN PRIVATE KEY-----\n...",
        "client_email": "mirix@your-project.iam.gserviceaccount.com",
        "client_id": "client-id",
        "auth_uri": "https://accounts.google.com/o/oauth2/auth",
        "token_uri": "https://oauth2.googleapis.com/token"
    })
    
    # Scope to storage only
    scoped_credentials = credentials.with_scopes([
        "https://www.googleapis.com/auth/cloud-platform"
    ])
    
    return scoped_credentials
```

## Network Security

### API Communication

```python
# Secure API calls
import requests
import ssl

def secure_api_call(url, data, api_key):
    # Create secure SSL context
    ssl_context = ssl.create_default_context()
    ssl_context.check_hostname = True
    ssl_context.verify_mode = ssl.CERT_REQUIRED
    
    # Secure headers
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json",
        "User-Agent": "Mirix/1.0"
    }
    
    # Make request with security
    response = requests.post(
        url,
        json=data,
        headers=headers,
        verify=True,  # Verify SSL certificates
        timeout=30
    )
    
    return response
```

### Local Network Protection

```python
# Disable unnecessary network services
def secure_network_config():
    config = {
        "disable_telemetry": True,
        "disable_auto_updates": False,  # Keep security updates
        "local_only_api": True,
        "require_authentication": True,
        "rate_limiting": {
            "max_requests_per_minute": 60,
            "max_requests_per_hour": 1000
        }
    }
    return config
```

## Audit and Monitoring

### Security Logging

```python
import logging
from datetime import datetime

# Security audit logger
security_logger = logging.getLogger('mirix.security')
security_logger.setLevel(logging.INFO)

def log_security_event(event_type, details, severity="INFO"):
    log_entry = {
        "timestamp": datetime.utcnow().isoformat(),
        "event_type": event_type,
        "details": details,
        "severity": severity,
        "user_id": get_current_user_id(),
        "session_id": get_current_session_id()
    }
    
    security_logger.info(f"SECURITY_EVENT: {log_entry}")

# Usage examples
log_security_event("MEMORY_ACCESS", "Accessed Knowledge Vault", "INFO")
log_security_event("FAILED_LOGIN", "Invalid credentials", "WARNING")
log_security_event("DATA_EXPORT", "Backup created", "INFO")
```

### Privacy Monitoring

```python
def privacy_health_check():
    """Monitor privacy compliance"""
    issues = []
    
    # Check screenshot retention
    screenshot_count = count_stored_screenshots()
    if screenshot_count > 600:
        issues.append("Too many screenshots stored")
    
    # Check data encryption
    unencrypted_sensitive = find_unencrypted_sensitive_data()
    if unencrypted_sensitive:
        issues.append(f"Found {len(unencrypted_sensitive)} unencrypted sensitive items")
    
    # Check cloud storage
    cloud_retention = check_cloud_retention_policy()
    if not cloud_retention:
        issues.append("Cloud retention policy not configured")
    
    return issues
```

## Compliance and Standards

### Data Protection Compliance

Mirix follows privacy regulations including:

- **GDPR (General Data Protection Regulation)**
  - Right to be forgotten (data deletion)
  - Data portability (backup/export)
  - Privacy by design (local processing)

- **CCPA (California Consumer Privacy Act)**
  - Transparent data collection
  - User control over data
  - Secure data handling

- **SOC 2 Type II Principles**
  - Security controls
  - Availability monitoring
  - Processing integrity
  - Confidentiality measures

### Security Best Practices

#### Data Minimization
```python
def data_minimization_policy():
    """Implement data minimization"""
    policies = {
        "collect_only_necessary": True,
        "delete_after_processing": True,
        "compress_old_data": True,
        "anonymize_when_possible": True
    }
    return policies
```

#### Encryption Standards
```python
# Use industry-standard encryption
from cryptography.fernet import Fernet
import hashlib

def secure_encryption():
    # Generate key from user password
    def generate_key(password: str) -> bytes:
        return hashlib.sha256(password.encode()).digest()
    
    # Encrypt sensitive data
    def encrypt_data(data: str, key: bytes) -> str:
        f = Fernet(key)
        return f.encrypt(data.encode()).decode()
    
    # Decrypt sensitive data
    def decrypt_data(encrypted_data: str, key: bytes) -> str:
        f = Fernet(key)
        return f.decrypt(encrypted_data.encode()).decode()
```

## User Privacy Controls

### Configuration Options

```yaml
# Privacy configuration in mirix.yaml
privacy:
  screenshot_capture:
    enabled: true
    excluded_apps: 
      - "Banking App"
      - "Password Manager"
    excluded_windows:
      - "*password*"
      - "*private*"
    
  data_retention:
    screenshot_hours: 24
    memory_days: 365
    log_days: 30
    
  cloud_storage:
    enabled: true
    auto_delete: true
    encryption: "google_managed"
    
  memory_security:
    encrypt_sensitive: true
    require_auth: true
    audit_access: true
```

### Privacy Dashboard

```python
def privacy_dashboard():
    """Show privacy status to user"""
    dashboard = {
        "screenshots_stored": count_screenshots(),
        "cloud_storage_usage": get_cloud_usage(),
        "sensitive_data_count": count_sensitive_data(),
        "last_cleanup": get_last_cleanup_time(),
        "encryption_status": check_encryption_status(),
        "privacy_score": calculate_privacy_score()
    }
    return dashboard
```

## Incident Response

### Security Incident Handling

```python
def handle_security_incident(incident_type, details):
    """Handle security incidents"""
    
    # Immediate response
    if incident_type == "UNAUTHORIZED_ACCESS":
        # Lock down sensitive data
        lock_sensitive_data()
        # Notify user
        send_security_alert(details)
        # Log incident
        log_security_incident(incident_type, details)
    
    elif incident_type == "DATA_BREACH":
        # Stop all processing
        stop_all_agents()
        # Secure data
        encrypt_all_data()
        # Create incident report
        create_incident_report(details)
    
    elif incident_type == "CLOUD_COMPROMISE":
        # Disable cloud access
        disable_cloud_access()
        # Switch to local-only mode
        enable_local_only_mode()
        # Notify user
        send_urgent_alert(details)
```

### Recovery Procedures

```python
def security_recovery_checklist():
    """Security recovery checklist"""
    checklist = [
        "Change all API keys and passwords",
        "Rotate encryption keys",
        "Audit all access logs",
        "Verify data integrity",
        "Update security configurations",
        "Test backup systems",
        "Document lessons learned"
    ]
    return checklist
```

## Security Updates

### Automatic Security Updates

```python
def security_update_system():
    """Handle security updates"""
    
    # Check for security updates
    updates = check_security_updates()
    
    for update in updates:
        if update.priority == "CRITICAL":
            # Auto-install critical security updates
            install_update(update)
            log_security_event("SECURITY_UPDATE", f"Installed {update.name}")
        
        elif update.priority == "HIGH":
            # Notify user of important updates
            notify_user_update(update)
```

By following these security and privacy practices, Mirix ensures that your personal data remains secure while providing the intelligent assistance you need.

## What's Next?

Learn about performance optimization and system tuning:

[**Performance →**](performance.md){ .md-button .md-button--primary }

Or explore backup and restore features:

[**Backup & Restore →**](backup-restore.md){ .md-button } 