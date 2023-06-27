ARG PYTHON_VERSION=3.10

###
### Stage 1: builder
###
FROM python:${PYTHON_VERSION}-bullseye AS builder

WORKDIR /paddleocr-web

RUN apt-get update && apt-get -y install ffmpeg libsm6 libxext6 && rm -rf /var/lib/apt/lists/*
RUN pip3 install --prefix="/install" paddlepaddle paddleocr aiohttp
RUN wget https://github.com/lilydjwg/paddleocr-web/archive/refs/heads/master.zip -O paddleocr-web.zip && \
    unzip paddleocr-web.zip && \
    cd paddleocr-web-master && \
    sed -i -e "s|path='/run/paddleocr/http.sock'|host='0.0.0.0', port='8000'|g" paddleocr-http

###
### Stage 2: runtime
###
FROM python:${PYTHON_VERSION}-bullseye AS runtime

RUN apt-get update && apt-get -y install ffmpeg libsm6 libxext6 && rm -rf /var/lib/apt/lists/*

COPY --from=builder /install /usr/local
COPY --from=builder /paddleocr-web/paddleocr-web-master /paddleocr-web

WORKDIR /paddleocr-web
EXPOSE 8000/tcp
ENTRYPOINT ["python3", "/paddleocr-web/paddleocr-http"]