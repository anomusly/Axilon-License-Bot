# ⚡ Quick Setup Checklist

## ✅ Pre-Setup Requirements

- [ ] **Node.js 18+** installed from https://nodejs.org/
- [ ] **Discord account** with a server you own/admin
- [ ] **MongoDB** (local or cloud from https://cloud.mongodb.com/)
- [ ] **Text editor** (Notepad++, VS Code, etc.)

## 🤖 Discord Bot Setup

- [ ] Go to https://discord.com/developers/applications
- [ ] Create "New Application" → Name it "License Bot"
- [ ] Go to "Bot" section → "Add Bot"
- [ ] **Copy the Token** (save it somewhere safe!)
- [ ] Go to "OAuth2" → "URL Generator"
- [ ] Select: `bot` + `applications.commands`
- [ ] Select permissions: `Send Messages`, `Use Slash Commands`, `Manage Roles`
- [ ] **Copy the URL** and invite bot to your server
- [ ] **Copy your Discord Server ID** (right-click server name → Copy Server ID)

## 🗄️ Database Setup

### Option A: Cloud Database (Recommended)
- [ ] Sign up at https://cloud.mongodb.com/
- [ ] Create free cluster
- [ ] Get connection string
- [ ] Replace `<password>` with your password

### Option B: Local Database
- [ ] Install MongoDB from https://www.mongodb.com/
- [ ] Start MongoDB service
- [ ] Use connection string: `mongodb://localhost:27017/axilon-licenses`

## ⚙️ System Configuration

- [ ] Open `Axilon License/config/config.yml`
- [ ] Fill in these values:

```yaml
Token: "YOUR_DISCORD_BOT_TOKEN"
GuildID: "YOUR_DISCORD_SERVER_ID"
MongoURI: "YOUR_MONGODB_CONNECTION_STRING"
WebServerSettings:
  Port: 3000
  ApiKey: "CREATE_A_STRONG_PASSWORD_HERE"
```

## 🚀 Installation & Startup

- [ ] Open terminal/command prompt
- [ ] Navigate to `Axilon License` folder
- [ ] Run: `npm install`
- [ ] Run: `npm start` (or double-click `start.bat`/`start.sh`)
- [ ] Check for success messages:
  - ✅ "Connected to MongoDB successfully"
  - ✅ "API Server has started on port 3000"
  - ✅ "Bot is ready!"

## 🧪 Testing

- [ ] Go to your Discord server
- [ ] Type `/getlicense` (should respond even if no licenses)
- [ ] Check `logs/logs.txt` file is created
- [ ] Test API with provided Python/C# examples

## 🔧 Integration

- [ ] Copy integration code from `BEGINNER_GUIDE.md`
- [ ] Replace `YOUR_SECRET_API_KEY` with your API key from config
- [ ] Replace `"My Software"` with your actual product name
- [ ] Test license validation in your software

## 🎯 Next Steps

- [ ] Create your first product (you'll need admin commands)
- [ ] Generate test license keys
- [ ] Set up webhook notifications (optional)
- [ ] Configure automatic role assignment
- [ ] Add license expiration dates

## 🚨 Troubleshooting Quick Fixes

### Bot not responding?
- [ ] Check bot token is correct
- [ ] Verify bot has permissions in Discord
- [ ] Make sure Guild ID is your server's ID

### Database errors?
- [ ] Check MongoDB connection string
- [ ] Verify database is running
- [ ] Test internet connection (for cloud DB)

### API not working?
- [ ] Ensure port 3000 is not blocked
- [ ] Check API key matches in config and code
- [ ] Verify license system is running

## 📞 Need Help?

1. **Check logs**: `logs/logs.txt`
2. **Read full guide**: `BEGINNER_GUIDE.md`
3. **Technical docs**: `README.md`
4. **System explanation**: `SYSTEM_EXPLANATION.md`

---

**🎉 Once everything is checked off, your license system is ready to use!**
