# ClaudeCodeTesting

# Claude Code Setup in GitHub Codespaces

## Prerequisites

Before starting, ensure you have:
- A GitHub account with access to Codespaces
- An Anthropic API key from [console.anthropic.com](https://console.anthropic.com) OR
- A Claude Pro/Max subscription
- Active billing set up for your chosen authentication method

## Step-by-Step Setup Instructions

### Step 1: Create or Open a GitHub Codespace

1. Navigate to your GitHub repository
2. Click the green "Code" button
3. Select "Codespaces" tab
4. Click "Create codespace on main" (or your preferred branch)
5. Wait for the codespace to initialize

### Step 2: Verify System Requirements

Once your codespace loads, verify the environment:

```bash
# Check Node.js version (should be 18+)
node --version

# Check npm version
npm --version

# If Node.js is not installed or version is too old:
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Step 3: Install Claude Code

Install Claude Code globally using npm:

```bash
# Install Claude Code (DO NOT use sudo)
npm install -g @anthropic-ai/claude-code

# Verify installation
claude --version
```

**Important:** Never use `sudo npm install -g` as this can cause permission issues and security risks.

### Step 4: Navigate to Your Project Directory

```bash
# If you're not already in your project root
cd /workspaces/your-repository-name

# Or create a new project directory if needed
mkdir my-project && cd my-project
```

### Step 5: Authentication Setup

Claude Code offers multiple authentication methods. Choose the one that matches your setup:

#### Option A: Using Anthropic API Key (Recommended for GitHub Codespaces)

1. **Set up environment variable:**
   ```bash
   # Set your API key as an environment variable
   export ANTHROPIC_API_KEY="your-api-key-here"
   
   # Make it persistent across sessions
   echo 'export ANTHROPIC_API_KEY="your-api-key-here"' >> ~/.bashrc
   source ~/.bashrc
   ```

2. **Alternative: Create a .env file (if supported by your project):**
   ```bash
   echo "ANTHROPIC_API_KEY=your-api-key-here" > .env
   ```

#### Option B: Using Claude Console OAuth (May have limitations in Codespaces)

```bash
# Start Claude Code and follow OAuth flow
claude

# When prompted, choose "Anthropic Console" authentication
# Copy and paste the provided URL into a new browser tab
# Complete the OAuth flow in your browser
# Return to the terminal and confirm authentication
```

#### Option C: Using Claude Pro/Max Subscription

```bash
# Start Claude Code
claude

# Choose "Claude App (with Pro or Max plan)" when prompted
# Follow the authentication flow using your Claude.ai credentials
```

### Step 6: Initialize Claude Code

1. **Start Claude Code:**
   ```bash
   claude
   ```

2. **Complete the initial setup:**
   - Follow the authentication prompts based on your chosen method
   - Allow Claude Code to analyze your project structure
   - Accept any initialization prompts

3. **Test the installation:**
   ```bash
   # In Claude Code, try a simple command
   /help
   
   # Or ask Claude to describe your project
   What files are in this project?
   ```

### Step 7: Generate Project Documentation (Recommended)

```bash
# Ask Claude to create a project overview
Generate a CLAUDE.md file that explains this project structure and setup
```

## Troubleshooting Common Authentication Issues

### Issue 1: "Authentication Failed" or "API Key Invalid"

**Solutions:**
1. **Verify your API key:**
   ```bash
   # Check if environment variable is set
   echo $ANTHROPIC_API_KEY
   
   # Test API key directly with curl
   curl -H "Authorization: Bearer $ANTHROPIC_API_KEY" \
        -H "Content-Type: application/json" \
        https://api.anthropic.com/v1/messages
   ```

2. **Regenerate your API key:**
   - Go to [console.anthropic.com](https://console.anthropic.com)
   - Navigate to API Keys section
   - Delete the old key and create a new one
   - Update your environment variable

### Issue 2: OAuth Flow Fails in Codespaces

**Solutions:**
1. **Use API key authentication instead:**
   ```bash
   # Switch to API key method
   export ANTHROPIC_API_KEY="your-key-here"
   claude
   ```

2. **Manual OAuth completion:**
   - Copy the OAuth URL from the terminal
   - Open it in a new browser tab (outside of Codespaces)
   - Complete authentication
   - Copy the resulting token/code back to Claude Code

### Issue 3: "Network Connection Issues"

**Solutions:**
1. **Check internet connectivity:**
   ```bash
   # Test connection to Anthropic API
   curl -I https://api.anthropic.com/v1/messages
   ```

2. **Configure proxy if needed:**
   ```bash
   # If your organization uses a proxy
   export HTTPS_PROXY=your-proxy-url
   export HTTP_PROXY=your-proxy-url
   ```

### Issue 4: Permission Errors

**Solutions:**
1. **Fix npm permissions:**
   ```bash
   # Create npm global directory in user space
   mkdir ~/.npm-global
   npm config set prefix '~/.npm-global'
   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   
   # Reinstall Claude Code
   npm install -g @anthropic-ai/claude-code
   ```

### Issue 5: Node.js Version Issues

**Solutions:**
1. **Install correct Node.js version:**
   ```bash
   # Install Node Version Manager
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   source ~/.bashrc
   
   # Install and use Node.js 20
   nvm install 20
   nvm use 20
   nvm alias default 20
   
   # Reinstall Claude Code
   npm install -g @anthropic-ai/claude-code
   ```

## Optimizing Your Codespace Environment

### Terminal Configuration

1. **Configure line breaks for multi-line input:**
   ```bash
   # Use backslash + Enter for line breaks in Claude Code
   # Or configure your terminal for Option+Enter (Meta+Enter)
   ```

2. **Enable notifications:**
   ```bash
   # Enable terminal bell for task completion
   echo "set bell-style audible" >> ~/.inputrc
   ```

### Persistent Configuration

1. **Create a startup script:**
   ```bash
   # Create .codespace/setup.sh
   mkdir -p .devcontainer
   cat > .devcontainer/setup.sh << 'EOF'
   #!/bin/bash
   # Install Claude Code if not present
   if ! command -v claude &> /dev/null; then
       npm install -g @anthropic-ai/claude-code
   fi
   
   # Set up environment variables
   export ANTHROPIC_API_KEY="${ANTHROPIC_API_KEY}"
   EOF
   
   chmod +x .devcontainer/setup.sh
   ```

2. **Add to devcontainer.json (optional):**
   ```json
   {
     "postCreateCommand": ".devcontainer/setup.sh"
   }
   ```

## Best Practices for Codespaces

1. **Store API keys securely:**
   - Use GitHub Codespaces secrets for API keys
   - Go to GitHub Settings → Codespaces → Repository secrets
   - Add `ANTHROPIC_API_KEY` as a secret

2. **Optimize for remote development:**
   - Use Claude Code's file-based workflows for large inputs
   - Avoid copying/pasting large amounts of text directly

3. **Regular maintenance:**
   ```bash
   # Update Claude Code regularly
   npm update -g @anthropic-ai/claude-code
   
   # Check for issues
   claude --version
   claude /doctor # If this command exists
   ```

## Quick Start Commands

Once setup is complete, try these commands:

```bash
# Start Claude Code
claude

# Inside Claude Code:
/help                    # Show available commands
/config                  # Configure settings
What's in this project?  # Analyze project structure
/commit                  # Commit changes with AI-generated messages
```

## Getting Help

If you encounter issues not covered here:

1. Check the [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code)
2. Visit the [GitHub Issues](https://github.com/anthropics/claude-code/issues) page
3. Contact [Anthropic Support](https://support.anthropic.com)

Remember that Claude Code is actively developed, so some authentication methods and features may change over time.