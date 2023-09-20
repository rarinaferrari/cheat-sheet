## GO BUILD

### Сократить размер бинаря (убрать отладочную информацию)

`go build -ldflags "-w -s"`

### Кроссплатформенная компиляция

```
Thug@ROG-STRIX MINGW64 ~/go
$ GOOS="linux" GOARCH="amd64" go build -ldflags "-w -s" hello.go
Thug@ROG-STRIX MINGW64 ~/go
$ file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, Go BuildID=LbJSOHT4GEQiXgPV66Ol/jE0qarg3WPh5JrCuA_Cc/goDf2MhgnxCGk26y1WhR/M9k1bjTsv-6qxkSujOoC, stripped
```

### MAN

`go doc fmt.Println`

