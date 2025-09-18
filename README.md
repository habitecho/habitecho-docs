# Complete n8n Installation and Setup Guide
A beginner-friendly guide to installing n8n (workflow automation tool) on Windows, Linux, and macOS, plus creating your first workflow with Google Drive integration.

# Complete n8n Installation and Setup Guide

A beginner-friendly guide to installing n8n (workflow automation tool) on Windows, Linux, and macOS, plus creating your first workflow with Google Drive integration.

## Table of Contents
- [What is n8n?](#what-is-n8n)
- [Prerequisites](#prerequisites)
- [Installation Methods](#installation-methods)
- [Windows Installation](#windows-installation)
- [Linux Installation](#linux-installation)
- [macOS Installation](#macos-installation)
- [First Workflow Tutorial](#first-workflow-tutorial)
- [Google Drive Integration](#google-drive-integration)
- [Troubleshooting](#troubleshooting)

## What is n8n?

n8n is a powerful workflow automation tool that connects different services together. Think of it as a visual programming tool where you:
- Connect boxes (nodes) together to create workflows
- Each box does something (sends email, creates files, fetches data, etc.)
- Data flows from one box to the next automatically

Perfect for automating repetitive tasks, building AI agents, and integrating multiple tools.

## Prerequisites

Before installing n8n, you need Docker installed on your system:

### Install Docker

**Windows:**
1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/)
2. Run the installer
3. Restart your computer
4. Verify installation: Open Command Prompt and run `docker --version`

**Linux (Ubuntu/Debian):**
```bash
# Update package index
sudo apt update

# Install required packages
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Add user to docker group (optional, avoids using sudo)
sudo usermod -aG docker $USER

# Verify installation
docker --version
```

**macOS:**
1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/)
2. Drag to Applications folder
3. Launch Docker Desktop
4. Verify: Open Terminal and run `docker --version`

## Installation Methods

We'll use Docker for all platforms as it's the most reliable method.

## Windows Installation

### Using Command Prompt/PowerShell:

1. **Create project folder:**
```cmd
mkdir C:\n8n-workspace
cd C:\n8n-workspace
```

2. **Run n8n with Docker:**
```cmd
docker run -it --rm --name n8n -p 5678:5678 -v C:\n8n-workspace:/data docker.n8n.io/n8nio/n8n
```

### Using Git Bash (Recommended):

1. **Create project folder:**
```bash
mkdir /c/n8n-workspace
cd /c/n8n-workspace
```

2. **Run n8n:**
```bash
docker run -it --rm --name n8n -p 5678:5678 -v /c/n8n-workspace:/data docker.n8n.io/n8nio/n8n
```

3. **Open VS Code (optional but recommended):**
```bash
code .
```

## Linux Installation

1. **Create project folder:**
```bash
mkdir ~/n8n-workspace
cd ~/n8n-workspace
```

2. **Run n8n with Docker:**
```bash
docker run -it --rm --name n8n -p 5678:5678 -v ~/n8n-workspace:/data docker.n8n.io/n8nio/n8n
```

3. **For persistent background running:**
```bash
# Create docker-compose.yml file
cat > docker-compose.yml << EOF
version: '3.8'
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    ports:
      - "5678:5678"
    volumes:
      - ./data:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
EOF

# Start n8n
docker-compose up -d
```

## macOS Installation

1. **Create project folder:**
```bash
mkdir ~/n8n-workspace
cd ~/n8n-workspace
```

2. **Run n8n with Docker:**
```bash
docker run -it --rm --name n8n -p 5678:5678 -v ~/n8n-workspace:/data docker.n8n.io/n8nio/n8n
```

3. **Alternative using Docker Compose:**
```bash
# Create docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    ports:
      - "5678:5678"
    volumes:
      - ./data:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
EOF

# Start n8n
docker-compose up -d
```

## Accessing n8n

After running any of the above commands:

1. **Wait for startup message:**
   Look for: `Editor is now accessible via: http://localhost:5678`

2. **Open your browser:**
   Navigate to: `http://localhost:5678`

3. **Complete setup:**
   - Create your account (username/password)
   - You'll see the n8n dashboard

## First Workflow Tutorial

Let's create a simple workflow to test everything works:

### Step 1: Create New Workflow

1. Click "Create Workflow" or "Start from scratch"
2. You'll see a blank canvas

### Step 2: Add Manual Trigger

1. Click the "+" button
2. Search for "Manual Trigger"
3. Click on it
4. A node appears on your canvas

### Step 3: Add Edit Fields Node

1. Click the "+" button after the Manual Trigger
2. Search for "Edit Fields" or "Set"
3. Click on it
4. Click "Add Field" → "String"
5. Set:
   - **Name:** `message`
   - **Value:** `Hello! My first n8n workflow is working!`

### Step 4: Test Basic Workflow

1. Click "Execute workflow"
2. Click on the Edit Fields node
3. Click "Output" tab
4. You should see your message!

## Google Drive Integration

Now let's connect to Google Drive to create files:

### Step 1: Set Up Google Cloud Project

1. **Go to Google Cloud Console:** [console.cloud.google.com](https://console.cloud.google.com)

2. **Create new project:**
   - Click "Select a project" → "New Project"
   - Name: `n8n-integration-project`
   - Click "Create"

3. **Enable Google Drive API:**
   - Go to "APIs & Services" → "Library"
   - Search "Google Drive API"
   - Click "Enable"

4. **Create OAuth2 credentials:**
   - Go to "APIs & Services" → "Credentials"
   - Click "Create Credentials" → "OAuth client ID"
   - If prompted, configure OAuth consent screen first:
     - User Type: External
     - App name: `n8n Google Drive Integration`
     - User support email: Your email
     - Developer contact: Your email
     - Add yourself as test user
   - Application type: Web application
   - Name: `n8n-google-drive-client`
   - Authorized redirect URIs: `http://localhost:5678/rest/oauth2-credential/callback`
   - Click "Create"
   - Copy Client ID and Client Secret

### Step 2: Configure n8n Google Drive

1. **Add Google Drive node:**
   - Click "+" after Edit Fields node
   - Search "Google Drive"
   - Select "Create file from text"

2. **Set up credentials:**
   - Click "Credential to connect with" dropdown
   - "Create New" → "OAuth2 (recommended)"
   - Paste your Client ID and Client Secret
   - Click "Sign in with Google"
   - Grant permissions
   - Click "Save"

3. **Configure the node:**
   - **File Content:** `{{ $json.message }}`
   - **File Name:** `my-first-n8n-file.txt`
   - **Parent Drive:** My Drive
   - **Parent Folder:** / (Root folder)

### Step 3: Test Complete Workflow

1. Click "Execute workflow"
2. Check your Google Drive
3. You should see `my-first-n8n-file.txt` with your message!

## Stopping n8n

**If running with docker run:**
- Press `Ctrl+C` in the terminal

**If running with docker-compose:**
```bash
docker-compose down
```

**To restart later:**
```bash
# For docker run method, run the same docker run command again
# For docker-compose method:
docker-compose up -d
```

## Troubleshooting

### Common Issues:

**Port 5678 already in use:**
```bash
# Kill existing process
docker stop n8n
# Or use different port
docker run -it --rm --name n8n -p 5679:5678 -v ~/n8n-workspace:/data docker.n8n.io/n8nio/n8n
```

**Docker permission denied (Linux):**
```bash
sudo usermod -aG docker $USER
# Logout and login again
```

**Google Drive OAuth callback error:**
- Verify redirect URI exactly matches: `http://localhost:5678/rest/oauth2-credential/callback`
- Add yourself as test user in Google Cloud Console
- Wait 5-10 minutes for Google changes to propagate

**Cannot access localhost:5678:**
- Check if n8n container is running: `docker ps`
- Check Docker is running
- Try accessing `http://127.0.0.1:5678` instead

### Getting Help:

- **n8n Documentation:** [docs.n8n.io](https://docs.n8n.io)
- **Community Forum:** [community.n8n.io](https://community.n8n.io)
- **GitHub Issues:** [github.com/n8n-io/n8n](https://github.com/n8n-io/n8n)

## Next Steps

Now that you have n8n working with Google Drive, you can:

1. **Explore more nodes:** Try HTTP Request, Email, Slack, etc.
2. **Build automation workflows:** Connect multiple services
3. **Schedule workflows:** Set up triggers for automatic execution
4. **Create AI agents:** Combine with AI services for intelligent automation

## Security Notes

- Never commit credentials to version control
- Use environment variables for sensitive data in production
- Consider using service accounts for production Google Drive access
- Implement proper authentication for production deployments

---

**Congratulations!** You now have a fully functional n8n installation and have created your first workflow with external service integration.
