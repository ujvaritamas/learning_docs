# How to update go version

`sudo rm -rf /usr/local/go`

Download latest go (tar.gz) [link](https://go.dev/dl/)

Extract and sudo mv go /usr/local

`which go`  
`go version`

```
go clean -modcache
go mod tidy
go get -u all
```

Vscode cmd+shift+p  
Go: Install/Update tools  
go clean -cache -modcache -i -r