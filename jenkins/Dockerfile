FROM jenkins/jenkins:lts

USER root

RUN apt-get update && apt-get install -y \
    php-cli php-mbstring php-xml php-tokenizer php-curl composer \
    && rm -rf /var/lib/apt/lists/*

USER jenkins
