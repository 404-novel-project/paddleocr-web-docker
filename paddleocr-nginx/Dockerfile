###
### Stage 1: builder
###
FROM node:20 AS builder


WORKDIR /root
RUN wget https://github.com/lilydjwg/paddleocr-web/archive/refs/heads/master.zip -O paddleocr-web.zip && \
    unzip paddleocr-web.zip 
RUN cd paddleocr-web-master && \
    npm install && \
    npm run build

###
### Stage 2: runtime
###
FROM nginx:latest AS runtime

COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=builder /root/paddleocr-web-master/dist /opt/paddleocr-web