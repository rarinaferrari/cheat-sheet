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
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}
type Friend interface {
	SayHello()
}
type Dog struct{}

func (d *Dog) SayHello() {
	fmt.Println("Woof woof")
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
	var dog = new(Dog)
	Greet(dog)
}

```

### Управляющие конструкции

```
func main() {
	x := "hello"
	switch x {
	case "foo":
		fmt.Println("Found foo")
	case "bar":
		fmt.Println("Found bar")
	default:
		fmt.Println("Default case")
	}
}


func main() {
	var x = 10

	if x == 10 {
		fmt.Println("x is equal 10")
	} else {
		fmt.Println("x is not equal 10")
	}
}
```

Type switch
```
func foo(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Println("I`m an integer!", v)
	case string:
		fmt.Println("I`m a string!", v)
	default:
		fmt.Println("Unknown type!", v)
	}
}
func main() {
	foo(42)
	foo("hello world")
}

$ go run test.go 
I`m an integer! 42
I`m a string! hello world
```
Type switch выполняет утверждение типов с помощью инструкции `switch`. Здесь представлен специальный синтаксис, `i.(type)` позволяющий извлечь тип переменной интерфейса i.

Циклы

```
func main() {
	for i := 0; i < 10; i++ {
		fmt.Println(i)
	}
}
```
Обратите внимание на `;` в первой строке. В отличие от многих языков, где этот знак используется в качестве разделителя строк, в Go он применяется в различных управляющих конструкциях для выполнения разных, но связанных подзадач в одной строке кода. Первая строка с помощью этого знака разделяет логику инициализации (i:=0), условное выражение (i<10) и оператор увеличения (i++).

```
func main() {
	nums := []int{2, 4, 6, 8}
	for idx, val := range nums {
		fmt.Println(idx, val)
	}
}

$ go run test.go 
0 2
1 4
2 6
3 8
```
Здесь мы инициализируем срез целых чисел `nums`, затем используем в цикле for ключевое слово range для перебора этого среза, range возвращает два значения: текущий индекс и копию текущего значения по этому индексу.

Если вы не собираетсь применять этот индекс, можете в цикле for заменить idx на нижнее подчеркивание, сообщив тем самым Go, что в нем не нуждаетесь.
```
func main() {
	nums := []int{2, 4, 6, 8}
	for _, val := range nums {
		fmt.Println(val)
	}
}

$ go run test.go 
2
4
6
8
```
### Многопоточность
```
func f() {
	fmt.Println("f function")

}

func main() {
	go f()
	time.Sleep(1 * time.Second)
	fmt.Println("main function")
}
```
Для этого используется ключевое слово `go`, размещаемое перед вызовом методов/функций, которые требуется выполнить параллельно.

```
func strlen(s string, c chan int) {
	c <- len(s)
}

func main() {
	c := make(chan int) // Создание канала int
	go strlen("Salutations", c)
	go strlen("World", c)
	x, y := <-c, <-c
	fmt.Println(x, y, x+y)
}

$ go run test.go 
5 11 16
```
Здесь функция `strlen` получает слово в виде строки, а также канал, который будет использоваться для синхронизации данных. Она определяет длину полученной строки и с помощью `<-` помещает результат в канал c.

### Обработка ошибок

```
type error interface {
	Error() string
}
```

В Go используется встроенный тип ошибок, который определяется через объявление interface. Это означает, что вы можете использовать в качестве error любой тип данных, который реализует метод Error(), возвращающий значение string.

```
type MyError string

func (e MyError) Error() string {
	return string(e)
}
```
Здесь мы создаём пользовательский строковый тип MyError и реализуем для этого типа метод Error() string.

```
func foo() error {
	return errors.New("Some error occurred")

}
func main() {
	if err := foo(); err != nil {
		// Обработка ошибок
	}
}
```
Функции и методы, как правило, возвращают не менее одного значения, одним из этих значений практически всегда является error. В Go возвращенная error может иметь значение nil, указывающее, что функция не произвела ошибки. 


