# Linux Ubuntu

## Golang

1. download go release package

```sh
wget https://golang.google.cn/dl/go1.24.2.linux-amd64.tar.gz #find latest version on the url
tar -xvf go1.24.2.linux-amd64.tar.gz
mv go /usr/local
```

2. setup path

```sh
vim ~/.bashrc

export GOROOT=/usr/local/go
export GOPATH=/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

source ~/.bashrc
go version
```
