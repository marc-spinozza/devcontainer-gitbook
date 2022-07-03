# [Choice] Node.js version (use -bullseye variants on local arm64/Apple Silicon): 18-bullseye, 16-bullseye, 14-bullseye, 18-buster, 16-buster, 14-buster
ARG VARIANT=10.24.0-buster
FROM node:${VARIANT}

# [Option] Install zsh
# ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
# ARG UPGRADE_PACKAGES="true"

# Install needed packages, yarn, nvm and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=node
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG NPM_GLOBAL=/usr/local/share/npm-global
ENV NVM_DIR=/usr/local/share/nvm
ENV NVM_SYMLINK_CURRENT=true \ 
    PATH=${NPM_GLOBAL}/bin:${NVM_DIR}/current/bin:${PATH}
COPY library-scripts/*.sh library-scripts/*.env /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    # Remove imagemagick due to https://security-tracker.debian.org/tracker/CVE-2019-10131
    && apt-get purge -y imagemagick imagemagick-6-common \
    # Install common packages, non-root user, update yarn and install nvm
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true"

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install calibre

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# [Optional] Uncomment if you want to install more global node modules
# RUN su node -c "npm install -g <your-package-list-here>""
RUN sudo npm i -g gitbook-cli \
    && cd /usr/local/lib/node_modules/gitbook-cli \
    && sudo npm i graceful-fs@4.1.4 --save \
    && cd /usr/local/lib/node_modules/gitbook-cli/node_modules/npm \
    && sudo npm i graceful-fs@4.1.4 --save

