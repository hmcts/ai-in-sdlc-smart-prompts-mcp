FROM node:24

WORKDIR /home/app

RUN apt-get update 
RUN apt-get install -y vim 

WORKDIR /home/app

COPY . /home/app

# Install UV
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin/:$PATH"

#
# First install the MCP Proxy to allow SSE transport
#

RUN uv tool install git+https://github.com/jonmach/mcp-proxy --reinstall

#
# Now the Smart-Prompts Service
#

RUN git clone https://github.com/jonmach/smart-prompts-mcp
WORKDIR /home/app/smart-prompts-mcp

RUN npm cache clean --force
RUN npm install
RUN npm run build 
RUN ./verify-install.sh
COPY ./mydotenv.txt /home/app/.env

#WORKDIR /home/app

# Expose the proxy port
EXPOSE 8080

#CMD ["tail", "-f", "/dev/null"]

# Start mcp-proxy with your MCP server
CMD ["mcp-proxy", "--pass-environment", "--host=0.0.0.0", "--debug", "--port=8080", "node", "/home/app/smart-prompts-mcp/dist/index.js"]

# Add the following to your Azure App Container Environment Variables

# Required: GitHub Configuration
#GITHUB_OWNER=hmcts                  # Your GitHub username or org
#GITHUB_REPO=repo-name-here          # Repository containing prompts
#GITHUB_BRANCH=                      # Branch to use (default: main)
#GITHUB_PATH=                        # Subdirectory path (optional)
#GITHUB_TOKEN=                       # Your read-only access token in here>

# Optional: Cache Configuration
#CACHE_TTL=300000                    # Cache time-to-live in ms (default: 5 min)
#CACHE_REFRESH_INTERVAL=60000        # Auto-refresh interval in ms (default: 1 min)

# Optional: Feature Flags
#ENABLE_SEMANTIC_SEARCH=true         # Advanced search features
#ENABLE_PROMPT_COMPOSITION=true      # Prompt combination features
#ENABLE_USAGE_TRACKING=true          # Track prompt usage




