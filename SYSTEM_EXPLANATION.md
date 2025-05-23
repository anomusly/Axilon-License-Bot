# How the Axilon License System Works

## 🎯 Overview

The Axilon License System is a comprehensive solution for managing software licenses through Discord integration and a RESTful API. It combines a Discord bot for administrative tasks with a web API for license validation from your applications.

## 🏗️ System Components

### 1. Discord Bot (`src/index.js`)
- **Purpose**: Administrative interface for license management
- **Features**: 
  - Slash commands for users and administrators
  - Automatic role assignment/removal
  - Real-time notifications and logging
  - License expiration monitoring

### 2. Web API Server (`src/api/authorize.js`)
- **Purpose**: Programmatic license validation
- **Features**:
  - RESTful API endpoints
  - Rate limiting protection
  - IP and HWID tracking
  - Comprehensive logging

### 3. Database Models (`src/models/`)
- **License Model**: Stores license keys, user data, usage statistics
- **Product Model**: Manages different software products
- **Statistics Model**: Tracks system-wide metrics

### 4. Utility Functions (`src/utils/`)
- **Utils**: Common functions like pagination, validation
- **Product Loader**: Initializes products on startup

## 🔄 How License Validation Works

### Step-by-Step Process:

1. **Client Application Request**
   ```
   Your Software → API Request → Axilon License System
   ```

2. **API Validation Process**
   ```
   ┌─────────────────┐
   │ Receive Request │
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Validate API Key│
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Check Rate Limit│
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Find License    │
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Validate Product│
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Check Expiration│
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Verify IP Cap   │
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Update Usage    │
   └─────────┬───────┘
             │
   ┌─────────▼───────┐
   │ Return Response │
   └─────────────────┘
   ```

3. **Response to Client**
   - Success: License details and user information
   - Failure: Error code and message

## 🔐 Security Features

### API Key Authentication
- Every API request requires a valid API key
- Prevents unauthorized access to the license system

### Rate Limiting
- Configurable request limits per IP address
- Prevents abuse and DDoS attacks
- Automatic blocking of excessive requests

### IP Address Tracking
- Records all IP addresses that use each license
- Configurable IP caps per license
- Prevents license sharing beyond allowed limits

### Hardware ID (HWID) Binding
- Optional hardware fingerprinting
- Binds licenses to specific machines
- Prevents unauthorized license transfers

### Automatic Expiration
- Scheduled checks for expired licenses
- Automatic removal of Discord roles
- Database cleanup of expired entries

## 📊 Data Flow

### License Creation Flow:
```
Discord Admin → Bot Command → Database → Role Assignment → User Notification
```

### License Validation Flow:
```
Your App → API Request → Database Query → Usage Update → Response
```

### Expiration Flow:
```
Scheduler → Check Expired → Remove Roles → Delete License → Log Action
```

## 🎮 Discord Integration

### User Commands:
- `/getlicense` - View personal license keys

### Admin Commands (if implemented):
- `/createlicense` - Generate new license keys
- `/deletelicense` - Remove license keys
- `/createproduct` - Add new products
- `/statistics` - View system metrics

### Automatic Features:
- Role assignment when licenses are created
- Role removal when licenses expire
- Real-time notifications via webhooks
- Comprehensive logging of all activities

## 🔧 Integration with Your Applications

### Python Integration:
```python
# Simple validation
client = AxilonLicenseClient("http://localhost:3000", "YOUR_API_KEY")
is_valid = client.is_license_valid("LICENSE_KEY", "PRODUCT_NAME")

if is_valid:
    # Allow access to your software
    print("Access granted!")
else:
    # Deny access
    print("Invalid license!")
```

### C# Integration:
```csharp
// Async validation
var client = new AxilonLicenseClient("http://localhost:3000", "YOUR_API_KEY");
var result = await client.ValidateLicenseAsync("LICENSE_KEY", "PRODUCT_NAME");

if (result.Success)
{
    // License is valid
    Console.WriteLine("Access granted!");
}
```

### JavaScript Integration:
```javascript
// Promise-based validation
const client = new AxilonLicenseClient("http://localhost:3000", "YOUR_API_KEY");
const result = await client.validateLicense("LICENSE_KEY", "PRODUCT_NAME");

if (result.success) {
    console.log("Access granted!");
}
```

## 📈 Monitoring and Analytics

### Real-time Logging:
- All API requests logged with timestamps
- Success/failure tracking
- IP address monitoring
- Error tracking and debugging

### Statistics Tracking:
- Total licenses created
- API request counts
- System uptime
- User activity metrics

### Webhook Notifications:
- Successful authentications
- Failed validation attempts
- License expiration events
- System errors and alerts

## 🛠️ Configuration Management

### Main Configuration (`config/config.yml`):
```yaml
# Discord settings
Token: "BOT_TOKEN"
GuildID: "DISCORD_SERVER_ID"

# Database
MongoURI: "mongodb://localhost:27017/licenses"

# API settings
WebServerSettings:
  Port: 3000
  ApiKey: "YOUR_SECRET_API_KEY"

# Security
RatelimitSettings:
  Enabled: true
  Time: 1  # minutes
  MaxRequests: 10
```

## 🔄 Typical Usage Workflow

### For Software Developers:
1. **Setup**: Configure the license system with your Discord server
2. **Product Creation**: Create products for your software
3. **License Generation**: Create licenses for customers
4. **Integration**: Add license validation to your software
5. **Monitoring**: Track usage through Discord and logs

### For End Users:
1. **Purchase**: Buy software from developer
2. **License Receipt**: Receive license key via Discord
3. **Software Use**: Enter license key in software
4. **Validation**: Software validates with Axilon system
5. **Access**: Granted or denied based on license status

## 🚨 Error Handling

### Common Error Scenarios:
- **Invalid License**: License doesn't exist or is malformed
- **Expired License**: License has passed expiration date
- **IP Cap Exceeded**: Too many different IPs using the license
- **Rate Limited**: Too many requests from same IP
- **Invalid Product**: Product name doesn't match
- **API Key Invalid**: Wrong or missing API key

### Error Response Format:
```json
{
  "status_msg": "Error description",
  "status_overview": "failed",
  "status_code": 400
}
```

## 🔧 Maintenance Tasks

### Regular Maintenance:
- Monitor log files for errors
- Check database performance
- Update Discord bot permissions
- Review rate limiting settings
- Backup license database

### Automated Tasks:
- License expiration checking (every 30 seconds)
- Role removal for expired licenses
- Statistics updates
- Log file rotation

## 📋 Best Practices

### Security:
- Use strong API keys
- Enable rate limiting
- Monitor webhook logs
- Regular database backups
- Keep software updated

### Performance:
- Monitor API response times
- Optimize database queries
- Use appropriate rate limits
- Scale horizontally if needed

### User Experience:
- Clear error messages
- Fast API responses
- Reliable Discord notifications
- Comprehensive documentation

This system provides a robust, scalable solution for software license management with Discord integration, making it easy to manage customers and validate licenses programmatically.
