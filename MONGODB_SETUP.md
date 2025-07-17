# MongoDB Setup Guide for Local Development

## Option 1: Install MongoDB Locally (Recommended)

### For Windows:
1. **Download MongoDB Community Server**
   - Go to https://www.mongodb.com/try/download/community
   - Download the Windows installer
   - Run the installer and follow the setup wizard

2. **Start MongoDB Service**
   ```bash
   # Open Command Prompt as Administrator
   net start MongoDB
   ```

3. **Verify Installation**
   ```bash
   # Open Command Prompt
   mongosh
   # You should see MongoDB shell prompt
   ```

### For macOS:
1. **Install using Homebrew**
   ```bash
   # Install Homebrew if you don't have it
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # Install MongoDB
   brew tap mongodb/brew
   brew install mongodb-community
   ```

2. **Start MongoDB**
   ```bash
   # Start MongoDB service
   brew services start mongodb/brew/mongodb-community
   ```

3. **Verify Installation**
   ```bash
   mongosh
   ```

### For Linux (Ubuntu/Debian):
1. **Install MongoDB**
   ```bash
   # Import MongoDB public GPG key
   wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
   
   # Create list file for MongoDB
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
   
   # Update package database
   sudo apt-get update
   
   # Install MongoDB
   sudo apt-get install -y mongodb-org
   ```

2. **Start MongoDB**
   ```bash
   sudo systemctl start mongod
   sudo systemctl enable mongod
   ```

3. **Verify Installation**
   ```bash
   mongosh
   ```

## Option 2: Use MongoDB Atlas (Cloud - Free Tier)

### Setup MongoDB Atlas:
1. **Create Account**
   - Go to https://www.mongodb.com/atlas
   - Sign up for free account

2. **Create Cluster**
   - Choose "Build a Database"
   - Select "M0 Sandbox" (Free tier)
   - Choose your preferred region
   - Create cluster

3. **Setup Database Access**
   - Go to "Database Access" in left sidebar
   - Click "Add New Database User"
   - Create username and password
   - Set permissions to "Read and write to any database"

4. **Setup Network Access**
   - Go to "Network Access" in left sidebar
   - Click "Add IP Address"
   - Choose "Allow Access from Anywhere" (0.0.0.0/0) for development
   - Or add your specific IP address

5. **Get Connection String**
   - Go to "Clusters" and click "Connect"
   - Choose "Connect your application"
   - Copy the connection string

## Configure Your Project

### Update server/.env file:
```env
# For Local MongoDB
MONGODB_URI=mongodb://localhost:27017/fitzone

# OR for MongoDB Atlas
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/fitzone?retryWrites=true&w=majority

# Other environment variables
NODE_ENV=development
PORT=5000
FRONTEND_URL=http://localhost:5173
JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_random_12345
```

## VS Code Extensions (Optional but Helpful)

### Install MongoDB Extension:
1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X)
3. Search for "MongoDB for VS Code"
4. Install the official MongoDB extension

### Using MongoDB Extension:
1. Click MongoDB icon in sidebar
2. Click "Add Connection"
3. Enter connection string:
   - Local: `mongodb://localhost:27017`
   - Atlas: Your Atlas connection string
4. You can now browse databases, collections, and documents

## Test Your Connection

### 1. Start your servers:
```bash
# Terminal 1 - Start MongoDB (if local)
mongod

# Terminal 2 - Start backend
cd server
npm run dev

# Terminal 3 - Start frontend
npm run dev
```

### 2. Seed the database:
```bash
npm run seed
```

### 3. Check if data was inserted:
```bash
# Connect to MongoDB shell
mongosh

# Switch to your database
use fitzone

# Check collections
show collections

# Check programs data
db.programs.find().pretty()
```

## Troubleshooting

### Common Issues:

1. **MongoDB not starting**
   ```bash
   # Check if MongoDB is running
   ps aux | grep mongod
   
   # Or check service status (Linux/macOS)
   sudo systemctl status mongod
   brew services list | grep mongodb
   ```

2. **Connection refused error**
   - Make sure MongoDB service is running
   - Check if port 27017 is available
   - Verify MONGODB_URI in .env file

3. **Authentication failed (Atlas)**
   - Double-check username/password in connection string
   - Ensure database user has proper permissions
   - Check network access settings

4. **Database not found**
   - MongoDB creates databases automatically when first document is inserted
   - Run the seed script to create initial data

## Quick Start Commands

```bash
# 1. Make sure MongoDB is running (local)
mongod

# 2. Install dependencies
npm install

# 3. Seed database
npm run seed

# 4. Start development servers
npm run dev:full
```

Your application should now be connected to MongoDB and ready for development!