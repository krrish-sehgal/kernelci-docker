FROM ubuntu:noble
# # Install dependencies
RUN apt-get update && \
    apt-get install -y git python3 python3-pip python3-dev build-essential wget python3-dateutil python3.12-venv

# Install Node.js / nodemon
RUN apt-get update && apt-get install -y curl && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    nvm install 16 && \
    nvm alias default 16 && \
    npm install -g nodemon

# # Install mongo shell
RUN apt-get update && \
    apt-get install -y gnupg curl && \
    curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
   gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
   --dearmor && \
    echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" > /etc/apt/sources.list.d/mongodb-org-8.0.list && \
    apt-get update && \
    apt-get install -y mongodb-org

# Configuration
RUN mkdir -p /etc/linaro
COPY app/kernelci-backend.cfg /etc/linaro/kernelci-backend.cfg

# Get source code
COPY kernelci-backend /srv/kernelci-backend

RUN python3 -m venv /venv && \
    /venv/bin/pip install --upgrade pip && \
    /venv/bin/pip install -r /srv/kernelci-backend/requirements.txt


EXPOSE 8888

COPY app/entrypoint.sh /srv/entrypoint.sh
CMD ["/srv/entrypoint.sh"]
