FROM golang:1.13

WORKDIR /api
COPY . .
ENV GO113MODULE=on

RUN go get github.com/pilu/fresh
CMD ["fresh"]
