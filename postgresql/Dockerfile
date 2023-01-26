FROM postgres:alpine

RUN apk update \
    && apk add postgis

COPY ./ext.sql /docker-entrypoint-initdb.d/