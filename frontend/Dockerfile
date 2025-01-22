FROM bitnami/minideb:bullseye

# Install dependencies
RUN install_packages build-essential git python python3-pip python-dev uwsgi-plugin-python3 curl redis-tools wget

RUN apt-get update && apt-get install -y curl && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    nvm install 16 && \
    nvm alias default 16 && \
    npm install -g nodemon

# Setup application
WORKDIR /srv
COPY kernelci-frontend /srv/kernelci-frontend
RUN pip install -r /srv/kernelci-frontend/requirements.txt  
WORKDIR /srv/kernelci-frontend/app
RUN touch /etc/flask_settings
COPY flask_settings /etc/linaro/kernelci-frontend.cfg

# uWSGI configuration
EXPOSE 5000
COPY uwsgi.ini /srv/uwsgi.ini
COPY entrypoint.sh /srv/entrypoint.sh
CMD ["/srv/entrypoint.sh"]
