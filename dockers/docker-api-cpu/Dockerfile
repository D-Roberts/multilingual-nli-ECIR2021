FROM ubuntu:18.04

RUN apt update && \
    apt install -y bash \
    build-essential \
    git \
    wget \
    vim \
    unzip \
    curl \
    ca-certificates \
    python3 \
    python3-pip && \
    rm -rf /var/lib/apt/lists

ENV LC_ALL=C.UTF-8

RUN python3 -m pip install --no-cache-dir --upgrade pip && \
    python3 -m pip install --no-cache-dir \
    torch==1.5.1

RUN mkdir /mfactcheck
WORKDIR /mfactcheck

COPY requirements.txt .
RUN python3 -m pip install --no-cache-dir -r requirements.txt

RUN python3 -c "import nltk; nltk.download('punkt')"

RUN mkdir -pv src
RUN mkdir -pv server

COPY src src
COPY server server

ENV PYTHONPATH "${PYTHONPATH}:src"

RUN python3 server/preload.py

ENV FLASK_APP app:app

ENTRYPOINT ["./server/gunicorn_starter.sh"]