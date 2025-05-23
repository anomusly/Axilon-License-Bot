# Axilon License System

A comprehensive Discord bot and API system for managing software licenses with MongoDB integration.

## 🚀 Features

- **Discord Bot Integration**: Manage licenses through Discord commands
- **RESTful API**: Validate licenses programmatically from any application
- **MongoDB Database**: Persistent storage for licenses, products, and statistics
- **Rate Limiting**: Prevent API abuse with configurable rate limits
- **IP & HWID Tracking**: Monitor license usage across different machines
- **Automatic Expiration**: Remove expired licenses and Discord roles automatically
- **Webhook Logging**: Real-time notifications for license activities
- **Statistics Tracking**: Monitor system usage and performance

## 📋 Requirements

- **Node.js**: Version 18.19.0 or higher
- **MongoDB**: Database for storing license data
- **Discord Bot**: Bot token and guild permissions
- **Discord Application**: For slash commands

## 🛠️ Installation

1. **Clone or extract the project**
   ```bash
   cd "Axilon License"
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure the system**
   - Edit `config/config.yml` with your settings
   - Set up MongoDB connection string
   - Configure Discord bot token and guild ID
   - Set API keys and webhook URLs

4. **Start the system**
   ```bash
   npm start
   ```

## ⚙️ Configuration

Edit `config/config.yml`:

```yaml
# Discord Bot Configuration
Token: "YOUR_DISCORD_BOT_TOKEN"
GuildID: "YOUR_DISCORD_GUILD_ID"

# MongoDB Configuration
MongoURI: "mongodb://localhost:27017/axilon-licenses"

# Web Server Configuration
WebServerSettings:
  Port: 3000
  ApiKey: "YOUR_API_KEY"

# Rate Limiting
RatelimitSettings:
  Enabled: true
  Time: 1  # minutes
  MaxRequests: 10

# Webhook Logging
WebhookLogsSettings:
  Enabled: true
  AuthenticationWebhookURL: "YOUR_WEBHOOK_URL"

# Embed Colors
EmbedColors: "#00ff00"

# Command Settings
getLicense:
  Enabled: true
```

## 🎮 Discord Commands

### `/getlicense`
- **Description**: View your own license keys
- **Usage**: `/getlicense`
- **Permissions**: Available to all users

### Admin Commands (if implemented)
- `/createlicense` - Create new license keys
- `/deletelicense` - Remove license keys
- `/createproduct` - Add new products
- `/statistics` - View system statistics

## 🔌 API Integration

### Authentication Endpoint

**POST** `/api/client/`

**Headers:**
```json
{
  "Content-Type": "application/json"
}
```

**Request Body:**
```json
{
  "licensekey": "YOUR_LICENSE_KEY",
  "product": "PRODUCT_NAME",
  "hwid": "HARDWARE_ID",
  "Authorization": "YOUR_API_KEY"
}
```

**Success Response (200):**
```json
{
  "status_msg": "SUCCESSFUL_AUTHENTICATION",
  "status_overview": "success",
  "status_code": 200,
  "status": "SUCCESS",
  "discord_id": "123456789012345678",
  "product": "PRODUCT_NAME",
  "license_key": "YOUR_LICENSE_KEY"
}
```

**Error Responses:**
- `400`: Invalid API Key / Invalid Request / Rate Limit Exceeded
- `401`: Invalid License Key / Invalid Product / Max IP Cap Reached

## 🐍 Python Integration Example

```python
import requests
import json

class AxilonLicenseClient:
    def __init__(self, api_url, api_key):
        """
        Initialize the Axilon License Client

        Args:
            api_url (str): Base URL of the license API
            api_key (str): Your API key for authentication
        """
        self.api_url = api_url.rstrip('/')
        self.api_key = api_key
        self.session = requests.Session()

    def validate_license(self, license_key, product_name, hwid=None):
        """
        Validate a license key with the Axilon License System

        Args:
            license_key (str): The license key to validate
            product_name (str): Name of the product
            hwid (str, optional): Hardware ID for additional security

        Returns:
            dict: Response from the license server
        """
        endpoint = f"{self.api_url}/api/client/"

        payload = {
            "licensekey": license_key,
            "product": product_name,
            "Authorization": self.api_key
        }

        if hwid:
            payload["hwid"] = hwid

        try:
            response = self.session.post(
                endpoint,
                json=payload,
                headers={"Content-Type": "application/json"},
                timeout=10
            )

            return {
                "success": response.status_code == 200,
                "status_code": response.status_code,
                "data": response.json()
            }

        except requests.exceptions.RequestException as e:
            return {
                "success": False,
                "status_code": 0,
                "error": str(e)
            }

    def is_license_valid(self, license_key, product_name, hwid=None):
        """
        Simple boolean check for license validity

        Returns:
            bool: True if license is valid, False otherwise
        """
        result = self.validate_license(license_key, product_name, hwid)
        return result.get("success", False)

# Usage Example
if __name__ == "__main__":
    # Initialize the client
    license_client = AxilonLicenseClient(
        api_url="http://localhost:3000",
        api_key="YOUR_API_KEY"
    )

    # Validate a license
    result = license_client.validate_license(
        license_key="SAMPLE-LICENSE-KEY",
        product_name="My Software",
        hwid="unique-hardware-id"
    )

    if result["success"]:
        print("✅ License is valid!")
        print(f"Discord ID: {result['data']['discord_id']}")
    else:
        print("❌ License validation failed!")
        print(f"Error: {result['data']['status_msg']}")
```

## 🔧 C# Integration Example

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

public class AxilonLicenseClient
{
    private readonly HttpClient _httpClient;
    private readonly string _apiUrl;
    private readonly string _apiKey;

    public AxilonLicenseClient(string apiUrl, string apiKey)
    {
        _apiUrl = apiUrl.TrimEnd('/');
        _apiKey = apiKey;
        _httpClient = new HttpClient();
    }

    public async Task<LicenseValidationResult> ValidateLicenseAsync(
        string licenseKey,
        string productName,
        string hwid = null)
    {
        try
        {
            var payload = new
            {
                licensekey = licenseKey,
                product = productName,
                Authorization = _apiKey,
                hwid = hwid
            };

            var json = JsonSerializer.Serialize(payload);
            var content = new StringContent(json, Encoding.UTF8, "application/json");

            var response = await _httpClient.PostAsync($"{_apiUrl}/api/client/", content);
            var responseContent = await response.Content.ReadAsStringAsync();

            return new LicenseValidationResult
            {
                Success = response.IsSuccessStatusCode,
                StatusCode = (int)response.StatusCode,
                Data = JsonSerializer.Deserialize<JsonElement>(responseContent)
            };
        }
        catch (Exception ex)
        {
            return new LicenseValidationResult
            {
                Success = false,
                StatusCode = 0,
                Error = ex.Message
            };
        }
    }

    public async Task<bool> IsLicenseValidAsync(string licenseKey, string productName, string hwid = null)
    {
        var result = await ValidateLicenseAsync(licenseKey, productName, hwid);
        return result.Success;
    }
}
```

## 🌐 JavaScript/Node.js Integration Example

```javascript
const axios = require('axios');

class AxilonLicenseClient {
    constructor(apiUrl, apiKey) {
        this.apiUrl = apiUrl.replace(/\/$/, '');
        this.apiKey = apiKey;
    }

    async validateLicense(licenseKey, productName, hwid = null) {
        try {
            const payload = {
                licensekey: licenseKey,
                product: productName,
                Authorization: this.apiKey
            };

            if (hwid) {
                payload.hwid = hwid;
            }

            const response = await axios.post(`${this.apiUrl}/api/client/`, payload, {
                headers: {
                    'Content-Type': 'application/json'
                },
                timeout: 10000
            });

            return {
                success: true,
                statusCode: response.status,
                data: response.data
            };

        } catch (error) {
            if (error.response) {
                return {
                    success: false,
                    statusCode: error.response.status,
                    data: error.response.data
                };
            } else {
                return {
                    success: false,
                    statusCode: 0,
                    error: error.message
                };
            }
        }
    }

    async isLicenseValid(licenseKey, productName, hwid = null) {
        const result = await this.validateLicense(licenseKey, productName, hwid);
        return result.success;
    }
}

// Usage Example
(async () => {
    const licenseClient = new AxilonLicenseClient(
        'http://localhost:3000',
        'YOUR_API_KEY'
    );

    const result = await licenseClient.validateLicense(
        'SAMPLE-LICENSE-KEY',
        'My Software',
        'unique-hardware-id'
    );

    if (result.success) {
        console.log('✅ License is valid!');
        console.log(`Discord ID: ${result.data.discord_id}`);
    } else {
        console.log('❌ License validation failed!');
        console.log(`Error: ${result.data.status_msg}`);
    }
})();
```

## 📞 Support

For support and questions:
- Check the logs in `./logs/logs.txt`
- Review configuration in `config/config.yml`
- Ensure all dependencies are installed
- Verify MongoDB connection

## 📄 License

This project is licensed under the ISC License.

---

**Created by YouSeeMeRunning | Axilon License System v1.2.1**
