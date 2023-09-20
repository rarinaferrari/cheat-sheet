## GO BUILD

### Сократить размер бинаря (убрать отладочную информацию)

`go build -ldflags "-w -s"`

### Кроссплатформенная компиляция

```
$ GOOS="linux" GOARCH="amd64" go build -ldflags "-w -s" hello.go
$ file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, Go BuildID=LbJSOHT4GEQiXgPV66Ol/jE0qarg3WPh5JrCuA_Cc/goDf2MhgnxCGk26y1WhR/M9k1bjTsv-6qxkSujOoC, stripped
```

### MAN

```
$ go doc fmt.Println
package fmt // import "fmt"

func Println(a ...any) (n int, err error)
    Println formats using the default formats for its operands and writes to
    standard output. Spaces are always added between operands and a newline
    is appended. It returns the number of bytes written and any write error
    encountered.
```

### GO GET
Служит для получения исходного кода пакетов
`go get github.com/stacktitan/ldapauth`

Для избежания конфликтов еще используют `dep` и `mod`

### Стилизация кода
Стилизует код
`go fmt main.go`
Ошибки стиля
`go get -u golang.org/x/lint/golint`
Подозрительные конструкции(вызов Printf() с некорректным форматом строковых типов)
`go vet`

### Примитивные типы данных
`bool, string, int, int8, int16, int32, int64, uint, uint8, uint16, uint32, uint64, uintptr, byte, rune, float32, float64, complex64, complex128`

### Срезы и Карты
Срез - массив, Карта - ассоциативный массив (ключ-значение)
```
func main() {
	var s = make([]string, 0)
	var m = make(map[string]string)
	s = append(s, "some string")
	m["some key"] = "some value"
	fmt.Printf("Slice: %v\nMap: %v", s, m)
}

$ go run types.go 
Slice: [some string]
Map: map[some key:some value]
```
### Указатели, структуры и интерфейсы
Указатели
```
func main() {
	var count = int(42)
	ptr := &count
	fmt.Println(*ptr)
	*ptr = 100
	fmt.Println(count)
}
```
`&` служит для извлечения адреса переменной, `*` для разыменовывания этого адреса
При вызове ссылки `*` - значение фактически заменяется у родителя, а не создаётся копия(объекта к примеру)

Структуры

```
type Person struct {
	Name string
	Age  int
}

func (p *Person) SayHello() {
	fmt.Println("Hello", p.Name)
}
func main() {
	var guy = new(Person)
	guy.Name = "Andrey"
	guy.SayHello()
}

$ go run hello.go 
Hello Andrey
```
Обратите внимание, что структура с заглавной буквы - глобальная и доступа вне пакета, так как со строчной - только в рамках пакета

Интерфейсы
```
type Person struct {
	Name string
	Age  int
}
type Friend interface {
	SayHello()
}

func (p *Person) SayHello() {
	fmt.Println("Hello", p.Name)
}

func Greet(f Friend) {
	f.SayHello()
}
func main() {
	var guy = new(Person)
	guy.Name = "Andrey"
	Greet(guy)
}
```





