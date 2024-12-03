FROM docker.io/golang:1.22.2-alpine3.19 AS build

RUN apk add make git curl

WORKDIR /code

RUN git clone --depth=1 https://github.com/rramiachraf/dumb.git .
COPY go.mod go.sum ./
RUN go mod download

COPY . .
COPY .git .
RUN make build

###############################################################

FROM alpine:3.19

LABEL org.opencontainers.image.source="https://github.com/rramiachraf/dumb"
LABEL org.opencontainers.image.url="https://github.com/rramiachraf/dumb"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.description="Private alternative front-end for Genius."

COPY --from=build /code/dumb .
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

EXPOSE 5555/tcp
HEALTHCHECK  --interval=5m --timeout=3s CMD wget --no-verbose --tries=1 --spider http://localhost:5555 || exit 1

CMD ["./dumb"]
