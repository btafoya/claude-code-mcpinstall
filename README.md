# Claude Code MCP Servers - Complete Setup Guide

This guide provides comprehensive instructions for setting up various Model Context Protocol (MCP) servers with Claude Code. These tools dramatically enhance Claude Code's capabilities, allowing it to interact with your filesystem, web browsers, and more.

## What are MCP Servers?

MCP (Model Context Protocol) Servers are extensions that give Claude Code new capabilities beyond just generating code. They allow Claude to interact with your computer in powerful ways:

- Reading and writing files
- Controlling web browsers
- Analyzing web pages
- Processing data with structured thinking
- And much more

## Table of Contents

- [Essential MCP Servers](#essential-mcp-servers)
  - [Sequential Thinking](#1-sequential-thinking)
  - [Filesystem Access](#2-filesystem-access)
  - [Puppeteer (Browser Automation)](#3-puppeteer-browser-automation)
  - [Web Fetching](#4-web-fetching)
  - [Firecrawl (Advanced Web Scraping)](#5-firecrawl-advanced-web-scraping)
  - [Browser Tools (Chrome DevTools Integration)](#6-browser-tools-chrome-devtools-integration)
- [Common Command Parameters](#common-command-parameters)
- [Verifying Installation](#verifying-installation)
- [Testing Each Tool](#testing-each-tool)
- [Troubleshooting](#troubleshooting)

## Essential MCP Servers

### 1. Sequential Thinking

**Purpose**: Gives Claude a structured framework for solving complex problems with step-by-step reasoning.

```bash
claude mcp add sequential-thinking -s user -- npx -y @modelcontextprotocol/server-sequential-thinking
```

**Parameters:**
- No specific parameters to modify
- Useful for breaking down complex problems into manageable steps

### 2. Filesystem Access

**Purpose**: Allows Claude to read, write, and manipulate files on your computer within specified directories.

```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Desktop ~/Downloads ~/Projects
```

**Parameters:**
- **Directory Paths**: Replace `~/Documents ~/Desktop ~/Downloads ~/Projects` with specific directories you want to give Claude access to
- You can add as many directories as needed, separated by spaces
- For read-only access with Docker, add `,ro` after directory paths

### 3. Puppeteer (Browser Automation)

**Purpose**: Gives Claude the ability to navigate websites, take screenshots, and interact with web pages.

```bash
claude mcp add puppeteer -s user -- npx -y @modelcontextprotocol/server-puppeteer
```

**Parameters:**
- This version opens a visible browser window that Claude can control
- For headless mode (no visible window), use Docker version instead

### 4. Web Fetching

**Purpose**: Allows Claude to retrieve content from the web.

```bash
claude mcp add fetch -s user -- npx -y @kazuph/mcp-fetch
```

**Parameters:**
- No specific parameters to modify
- Handles fetching web content with automatic processing of text and images

### 5. Firecrawl (Advanced Web Scraping)

**Purpose**: Enables powerful web scraping, crawling and search capabilities.

```bash
# Replace fc-YOUR_API_KEY with your actual Firecrawl API key
claude mcp add firecrawl -s user -- env FIRECRAWL_API_KEY=fc-YOUR_API_KEY npx -y firecrawl-mcp
```

**Parameters:**
- **API Key**: Replace `fc-YOUR_API_KEY` with your actual Firecrawl API key
- **Optional Environment Variables:**
  - `FIRECRAWL_RETRY_MAX_ATTEMPTS=5` (default: 3)
  - `FIRECRAWL_RETRY_INITIAL_DELAY=2000` (in ms, default: 1000)
  - `FIRECRAWL_RETRY_MAX_DELAY=30000` (in ms, default: 10000)
  - `FIRECRAWL_CREDIT_WARNING_THRESHOLD=2000` (default: 1000)

### 6. Browser Tools (Chrome DevTools Integration)

**Purpose**: Gives Claude access to your browser's console logs, network traffic, and the ability to run performance/accessibility audits.

#### Step 1: Install the Chrome extension
Download from [the releases page](https://github.com/AgentDeskAI/browser-tools-mcp/releases) and install manually through Chrome's extension manager

#### Step 2: Start the middleware server (keep this terminal open)
```bash
npx @agentdeskai/browser-tools-server@1.2.1
```

#### Step 3: Add the MCP server to Claude Code (in a separate terminal)
```bash
claude mcp add browser-tools -s user -- npx -y @agentdeskai/browser-tools-mcp@1.2.1
```

#### Step 4: Open Chrome DevTools (F12) and find the BrowserTools tab

**Parameters:**
- Specify version number (e.g., `@1.2.1`) to ensure you're using the latest version
- In the Chrome extension, you can configure:
  - Auto-paste options
  - Token limits
  - Screenshot handling

## Common Command Parameters

All MCP server commands share some common parameters:

- `-s user`: Sets the scope to user-level (global across all directories)
- `-s local`: Sets the scope to local directory only (default if not specified)
- `--`: Separates Claude Code arguments from the command to run
- `npx -y`: Runs an npm package without installing it permanently, auto-confirming

## Verifying Installation

After installing, you can verify everything is working with:

```bash
claude mcp list
```

This should show all your installed MCP servers.

To remove a server:

```bash
claude mcp remove server-name
```

## Testing Each Tool

Here are some quick prompts to test each tool:

- **Sequential Thinking:** 
  ```
  Use sequential thinking to solve the Monty Hall problem
  ```

- **Filesystem:** 
  ```
  List the files in my Documents folder
  ```

- **Puppeteer:** 
  ```
  Navigate to example.com and take a screenshot
  ```

- **Web Fetching:** 
  ```
  Fetch and summarize the content from https://example.com
  ```

- **Firecrawl:** 
  ```
  Search for recent news about artificial intelligence
  ```

- **Browser Tools:** 
  ```
  Show me the console logs from my browser
  Run an accessibility audit on my current webpage
  ```
  (Make sure Chrome DevTools is open with BrowserTools tab)

## Troubleshooting

- **Windows Issues**: If commands don't work on Windows, try adding `cmd /c` before npx commands:
  ```bash
  claude mcp add sequential-thinking -s user -- cmd /c npx -y @modelcontextprotocol/server-sequential-thinking
  ```

- **Timeout Errors**: Try increasing the timeout:
  ```bash
  MCP_TIMEOUT=10000 claude
  ```

- **Browser Tools Issues**: 
  - Ensure both the middleware server and the MCP server are running
  - Make sure Chrome DevTools is open with BrowserTools tab visible
  - Check that the Chrome extension is correctly installed

- **Filesystem Access**: Make sure to use the correct paths for your system and that Claude has appropriate permissions

- **Connection Problems**: Use `/mcp` in Claude Code to check connection status of all MCP servers
