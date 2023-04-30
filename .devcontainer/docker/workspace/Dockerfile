# windows 11
FROM ubuntu:22.04

ARG USER_NAME
ARG USER_GROUP_NAME
ARG USER_UID
ARG USER_GID

LABEL maintainer="mizuki"
LABEL version="2.0"
LABEL description="my-workspace"

# general packages
ARG PKG="vim curl unzip zip sudo tzdata git"

SHELL ["/bin/bash", "-c"]

# ---------------------------------- set up ----------------------------------

# setup timezone https://dev.to/grigorkh/fix-tzdata-hangs-during-docker-image-build-4o9m
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y ${PKG} \
    && groupadd --gid ${USER_GID} ${USER_GROUP_NAME} \
    && useradd --uid ${USER_UID} --shell /bin/bash --gid ${USER_GID} -m ${USER_NAME} \
    && echo %${USER_GROUP_NAME} ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/${USER_GROUP_NAME} \
    && chmod 0440 /etc/sudoers.d/${USER_GROUP_NAME}

USER ${USER_NAME}

ENV HOME=/home/${USER_NAME}

# ---------------------------------- enable git ----------------------------------

# https://github.com/devcontainers/cli/issues/98
# change to main branch https://dev-yakuza.posstree.com/git/default-branch/
RUN git config --global --add safe.directory "${HOME}/workspace" \
    && git config --global user.name "${GIT_USER_NAME}" \
    && git config --global user.email "${GIT_USER_EMAIL}"


# ---------------------------------- sdkman for java ----------------------------------
# https://sdkman.io/install
ARG JAVA_VERSION
ARG GRADLE_VERSION

RUN curl -s "https://get.sdkman.io" | bash \
    && source "${HOME}/.sdkman/bin/sdkman-init.sh" \
    && sdk install java "${JAVA_VERSION}" \
    && sdk install gradle ${GRADLE_VERSION}


# ---------------------------------- pyenv for python ----------------------------------
# dependencies for pyenv
ARG PYENV_DEP="make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget llvm libncurses5-dev \
    libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python3-openssl"

RUN sudo apt-get update -y \
    && sudo apt-get install -y --no-install-recommends ${PYENV_DEP}

# default python version
ARG PYTHON_VERSION
ARG SECOUND_PYTHON_VERSION

ENV PYENV_ROOT=${HOME}/.pyenv

RUN curl https://pyenv.run | bash \
    && echo 'export PYENV_ROOT="${HOME}/.pyenv"' >> ${HOME}/.bashrc \
    && echo 'export PATH="${PYENV_ROOT}/bin:${PATH}"' >> ${HOME}/.bashrc \
    && echo 'eval "$(pyenv init --path)"' >> ${HOME}/.bashrc \
    && export PATH="${PYENV_ROOT}/bin:${PATH}" \
    && eval "$(pyenv init --path)" \
    && pyenv install "${PYTHON_VERSION}" \
    && pyenv global "${PYTHON_VERSION}" \
    && pyenv install "${SECOUND_PYTHON_VERSION}"

# ---------------------------------- nvm for node ----------------------------------
# default node version
ARG NODE_VERSION

ARG NODE_GLOBA_PKG="prettier"

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
    && source ~/.nvm/nvm.sh \
    && nvm install --lts \
    && nvm alias default "${NODE_VERSION}" \
    && nvm use default \
    && npm i --global ${NODE_GLOBA_PKG}

USER root

# ---------------------------------- latest c c++ compiler ----------------------------------
# https://ja.linuxcapable.com/how-to-install-gcc-compiler-on-ubuntu-22-04-lts/
# default gcc g++ version
ARG LATEST_C_VERSION

RUN apt-get -y install gcc-${LATEST_C_VERSION} g++-${LATEST_C_VERSION} \
    && update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-${LATEST_C_VERSION} ${LATEST_C_VERSION} \
    && update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-${LATEST_C_VERSION} ${LATEST_C_VERSION}

# ---------------------------------- set up language ----------------------------------
# setup locale https://stackoverflow.com/a/55077451/13723015
ARG LOCALE

RUN apt-get -y install locales \
    && echo "LC_ALL=${LOCALE}" >> /etc/environment \
    && echo "${LOCALE} UTF-8" >> /etc/locale.gen \
    && echo "LANG=${LOCALE}" > /etc/locale.conf \
    && locale-gen ${LOCALE}

# ---------------------------------- aws cli ----------------------------------

ARG ZIP_FILE=awscliv2.zip

RUN apt-get install -y less \
    && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o ${ZIP_FILE} \
    && unzip ${ZIP_FILE} \
    && ./aws/install \
    && rm -f ${ZIP_FILE}

# ---------------------------------- github cli ----------------------------------

RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
    && chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && apt-get update \
    && apt-get install gh -y \
    && gh --version

# ---------------------------------- addtional packages ----------------------------------

ARG ADDITONAL_PKG="iputils-ping"

RUN apt-get update -y \
    && apt-get install ${ADDITONAL_PKG} -y