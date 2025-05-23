# 🚀 Complete Beginner's Guide to Axilon License System

## 📖 What is a License System?

A **license system** is like a digital key that controls who can use your software. Think of it like this:

- **Your Software** = A house 🏠
- **License Key** = The key to enter 🔑
- **License System** = The lock that checks if the key is valid 🔒

### Why Do You Need This?
- **Protect Your Software**: Only paying customers can use it
- **Control Access**: You decide who gets access and for how long
- **Track Usage**: See who's using your software and when
- **Automatic Management**: Handles everything automatically through Discord

## 🎯 What Does This System Do?

### For You (The Developer):
1. **Create License Keys**: Generate unique keys for each customer
2. **Manage Customers**: Handle everything through Discord commands
3. **Track Usage**: See who's using your software and from where
4. **Automatic Expiration**: Remove access when licenses expire
5. **Role Management**: Automatically give/remove Discord roles

### For Your Customers:
1. **Get License Key**: Receive their unique key via Discord
2. **Use Your Software**: Enter the key when using your program
3. **Automatic Validation**: System checks if they're allowed to use it
4. **Discord Integration**: Get updates and support through Discord

## 🛠️ What You Need Before Starting

### Required Software:
1. **Node.js** (Version 18 or higher)
   - Download from: https://nodejs.org/
   - This runs the license system

2. **MongoDB** (Database)
   - Option 1: Install locally from https://www.mongodb.com/
   - Option 2: Use free cloud database at https://cloud.mongodb.com/
   - This stores all your license data

3. **Discord Bot**
   - Create at: https://discord.com/developers/applications
   - This manages licenses through Discord

### Required Accounts:
- Discord account (for the bot)
- MongoDB account (if using cloud database)

## 📋 Step-by-Step Setup Guide

### Step 1: Create a Discord Bot

1. **Go to Discord Developer Portal**
   - Visit: https://discord.com/developers/applications
   - Click "New Application"
   - Give it a name like "License Bot"

2. **Create the Bot**
   - Go to "Bot" section
   - Click "Add Bot"
   - Copy the **Token** (you'll need this later)
   - ⚠️ **NEVER share this token with anyone!**

3. **Set Bot Permissions**
   - Go to "OAuth2" → "URL Generator"
   - Select "bot" and "applications.commands"
   - Select these permissions:
     - Send Messages
     - Use Slash Commands
     - Manage Roles
     - Read Message History

4. **Invite Bot to Your Server**
   - Copy the generated URL
   - Open it in browser
   - Select your Discord server
   - Authorize the bot

### Step 2: Set Up Database

#### Option A: Free Cloud Database (Recommended for beginners)
1. **Create MongoDB Atlas Account**
   - Go to: https://cloud.mongodb.com/
   - Sign up for free account

2. **Create a Cluster**
   - Choose "Free" tier
   - Select a region close to you
   - Wait for cluster to be created (2-3 minutes)

3. **Get Connection String**
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database password

#### Option B: Local Database
1. **Download MongoDB**
   - Go to: https://www.mongodb.com/try/download/community
   - Install on your computer

2. **Start MongoDB**
   - Windows: MongoDB should start automatically
   - Mac/Linux: Run `mongod` in terminal

### Step 3: Configure the License System

1. **Open Configuration File**
   - Navigate to: `Axilon License/config/config.yml`
   - Open with any text editor

2. **Fill in Your Settings**
   ```yaml
   # Your Discord bot token (from Step 1)
   Token: "YOUR_BOT_TOKEN_HERE"

   # Your Discord server ID
   GuildID: "YOUR_DISCORD_SERVER_ID"

   # Your database connection (from Step 2)
   MongoURI: "YOUR_MONGODB_CONNECTION_STRING"

   # API settings for your software
   WebServerSettings:
     Port: 3000
     ApiKey: "YOUR_SECRET_API_KEY"  # Make this a strong password

   # Security settings
   RatelimitSettings:
     Enabled: true
     Time: 1  # Allow requests every 1 minute
     MaxRequests: 10  # Maximum 10 requests per minute per IP

   # Colors for Discord messages
   EmbedColors: "#00ff00"  # Green color

   # Enable/disable features
   getLicense:
     Enabled: true
   ```

3. **How to Find Your Discord Server ID**
   - In Discord, right-click your server name
   - Click "Copy Server ID"
   - If you don't see this option, enable Developer Mode:
     - Settings → Advanced → Developer Mode → ON

### Step 4: Install and Start the System

1. **Open Terminal/Command Prompt**
   - Windows: Press `Win + R`, type `cmd`, press Enter
   - Mac: Press `Cmd + Space`, type `terminal`, press Enter
   - Linux: Press `Ctrl + Alt + T`

2. **Navigate to License System Folder**
   ```bash
   cd "path/to/Axilon License"
   ```

3. **Install Dependencies**
   ```bash
   npm install
   ```
   - This downloads all required components
   - Wait for it to complete (1-2 minutes)

4. **Start the System**
   ```bash
   npm start
   ```
   - Or double-click `start.bat` (Windows) or `start.sh` (Mac/Linux)

5. **Check if Everything Works**
   - You should see messages like:
     ```
     Connected to MongoDB successfully.
     [SERVER] API Server has started on port 3000.
     Bot is ready!
     ```

## 🎮 How to Use the System

### Creating Your First Product

A "product" is a category for your software. For example:
- Product: "My Game Hack"
- Product: "Discord Bot"
- Product: "Website Tool"

**To create a product, you'll need to add admin commands to your bot** (not included in basic setup, but here's the concept):

### Managing Licenses Through Discord

Once set up, you can:

1. **View Licenses** (for users):
   - Type `/getlicense` in Discord
   - Shows all licenses for that user

2. **Create Licenses** (admin only):
   - Use admin commands to generate new license keys
   - Assign them to Discord users
   - Set expiration dates

3. **Monitor Usage**:
   - Check logs in `logs/logs.txt`
   - See who's using licenses and when

## 💻 Adding License Checking to Your Software

### Simple Python Example

```python
import requests

def check_license(license_key, product_name):
    """
    Check if a license key is valid

    Args:
        license_key: The key your customer enters
        product_name: Name of your software/product

    Returns:
        True if valid, False if invalid
    """

    # Your license system details
    api_url = "http://localhost:3000/api/client/"
    api_key = "YOUR_SECRET_API_KEY"  # Same as in config.yml

    # Data to send
    data = {
        "licensekey": license_key,
        "product": product_name,
        "Authorization": api_key
    }

    try:
        # Send request to license system
        response = requests.post(api_url, json=data, timeout=10)

        # Check if license is valid
        if response.status_code == 200:
            print("✅ License is valid!")
            return True
        else:
            print("❌ License is invalid!")
            return False

    except Exception as e:
        print(f"❌ Error checking license: {e}")
        return False

# Example usage in your software
def main():
    print("Welcome to My Software!")

    # Ask user for license key
    user_license = input("Enter your license key: ")

    # Check if it's valid
    if check_license(user_license, "My Software"):
        print("Access granted! Starting software...")
        # Your software code here
    else:
        print("Access denied! Please contact support.")
        exit()

if __name__ == "__main__":
    main()
```

### Simple C# Example

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

class Program
{
    private static readonly HttpClient client = new HttpClient();

    static async Task Main(string[] args)
    {
        Console.WriteLine("Welcome to My Software!");

        Console.Write("Enter your license key: ");
        string licenseKey = Console.ReadLine();

        bool isValid = await CheckLicense(licenseKey, "My Software");

        if (isValid)
        {
            Console.WriteLine("✅ Access granted! Starting software...");
            // Your software code here
        }
        else
        {
            Console.WriteLine("❌ Access denied! Please contact support.");
        }
    }

    static async Task<bool> CheckLicense(string licenseKey, string productName)
    {
        try
        {
            var data = new
            {
                licensekey = licenseKey,
                product = productName,
                Authorization = "YOUR_SECRET_API_KEY"  // Same as in config.yml
            };

            string json = JsonSerializer.Serialize(data);
            var content = new StringContent(json, Encoding.UTF8, "application/json");

            var response = await client.PostAsync("http://localhost:3000/api/client/", content);

            if (response.IsSuccessStatusCode)
            {
                Console.WriteLine("✅ License is valid!");
                return true;
            }
            else
            {
                Console.WriteLine("❌ License is invalid!");
                return false;
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"❌ Error checking license: {ex.Message}");
            return false;
        }
    }
}
```

## 🔧 Understanding the System Flow

### What Happens When Someone Uses Your Software:

1. **User starts your software**
2. **Software asks for license key**
3. **User enters their key**
4. **Your software sends key to Axilon License System**
5. **System checks if key is valid**
6. **System responds with YES or NO**
7. **Your software allows or denies access**

### Visual Flow:
```
[Your Software] → [License Check] → [Axilon System] → [Database]
                                         ↓
[Allow/Deny] ← [Response] ← [Valid/Invalid] ← [Check Result]
```

## 🎯 Real-World Example Scenario

Let's say you created a Minecraft hack and want to sell it:

### Step 1: Setup
- You set up Axilon License System
- Create product called "Minecraft Hack"
- Configure Discord bot in your server

### Step 2: Customer Purchase
- Customer joins your Discord server
- Customer pays you for the hack
- You create a license key for them using Discord commands
- Customer automatically gets "Customer" role in Discord

### Step 3: Customer Uses Hack
- Customer downloads your Minecraft hack
- Hack asks for license key when starting
- Customer enters their key
- Hack checks with your Axilon system
- If valid: Hack starts working
- If invalid: Hack shows error message

### Step 4: Automatic Management
- License expires after 30 days (if you set it)
- System automatically removes customer role
- Customer can no longer use the hack
- All tracked in Discord and logs

## 🛡️ Security Features Explained

### API Key Protection
- Like a master password for your system
- Only you know it
- Required for all license checks
- Keep it secret!

### Rate Limiting
- Prevents spam attacks
- Limits how many checks per minute
- Protects your system from overload

### IP Tracking
- Records where licenses are used
- Can limit licenses to specific locations
- Prevents sharing licenses

### Hardware ID (HWID)
- Binds license to specific computer
- Prevents license sharing between computers
- Optional but recommended for security

## 📊 Monitoring Your System

### Log Files
- Location: `Axilon License/logs/logs.txt`
- Shows all license checks
- Records errors and issues
- Check regularly for problems

### Discord Notifications
- Real-time alerts in Discord
- Shows successful/failed license checks
- Notifies about expired licenses
- Configure webhooks for this

### Statistics
- Track total licenses created
- Monitor API usage
- See system performance
- Available through Discord commands

## 🚨 Common Problems and Solutions

### Problem: Bot doesn't respond to commands
**Solution:**
- Check bot token in config.yml
- Make sure bot has permissions in Discord
- Verify Guild ID is correct
- Restart the system

### Problem: License checks always fail
**Solution:**
- Check API key matches in config and your software
- Verify MongoDB connection
- Make sure license system is running
- Check product name matches exactly

### Problem: Database connection errors
**Solution:**
- Verify MongoDB URI in config.yml
- Check internet connection (for cloud database)
- Make sure MongoDB is running (for local database)
- Check database credentials

### Problem: "Cannot find module" errors
**Solution:**
- Run `npm install` in the license system folder
- Make sure Node.js is installed correctly
- Check if you're in the right directory

## 🎓 Next Steps

### Basic Usage:
1. Set up the system following this guide
2. Create your first product
3. Generate test license keys
4. Add license checking to your software
5. Test everything works

### Advanced Features:
1. Add admin commands for license management
2. Set up webhook notifications
3. Implement HWID binding
4. Add license expiration dates
5. Create customer management commands

### Scaling Up:
1. Use cloud hosting for the license system
2. Set up automatic backups
3. Monitor system performance
4. Add multiple products
5. Implement advanced security features

## 💡 Tips for Success

### For Beginners:
- Start simple - test with one product first
- Use the provided examples as templates
- Keep your API key secret and secure
- Regular backup your database
- Monitor logs for issues

### Best Practices:
- Use strong, unique license keys
- Set reasonable expiration dates
- Provide clear instructions to customers
- Have a support system in place
- Keep the system updated

### Customer Experience:
- Make license entry simple in your software
- Provide clear error messages
- Offer good customer support
- Use Discord for community building
- Automate as much as possible

## 📞 Getting Help

### If You're Stuck:
1. **Check the logs** - `logs/logs.txt` shows what's happening
2. **Verify configuration** - Double-check `config/config.yml`
3. **Test step by step** - Isolate the problem
4. **Check examples** - Use provided code templates
5. **Start fresh** - Sometimes easier to reconfigure

### Common Resources:
- **Node.js Documentation**: https://nodejs.org/docs/
- **MongoDB Tutorials**: https://docs.mongodb.com/
- **Discord Bot Guide**: https://discord.js.org/
- **HTTP/API Basics**: https://developer.mozilla.org/en-US/docs/Web/HTTP

## 🎉 Congratulations!

You now have a complete understanding of how license systems work and how to use the Axilon License System!

**Remember:**
- Start simple and build up complexity
- Test everything thoroughly
- Keep security in mind
- Provide good customer support
- Have fun building your software business!

---

**Need more help?** Check the other documentation files:
- `README.md` - Technical documentation
- `SYSTEM_EXPLANATION.md` - Detailed system explanation
- `examples/` folder - More integration examples
