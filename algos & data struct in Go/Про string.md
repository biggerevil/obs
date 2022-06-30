**Сами строки** в Go immutable (неизменяемые). **Переменные со строками** могут меняться (например, принимать другое значение), но сами строки - нет.

Тип string выглядит таким образом:
```go
type _string struct {
	elements *byte // underlying bytes
	len      int   // number of bytes
}
```

То есть строка - это read-only слайс байт.

#### Про Unicode
Unicode сопоставляет каждому символу числовое значение, также называемое Code Point. Это числовое значение в Go представлено типом `rune`, который является alias'ом типа `int32`. Используется `int32`, а не `uint32`, для того, чтобы было легче отслеживать переполнение.

#### Разница между byte и rune в Go
Информация взята [отсюда](https://levelup.gitconnected.com/demystifying-bytes-runes-and-strings-in-go-1f94df215615).

Сразу посмотри примеры:
1. Итерируемся по строке при помощи `fori`, который **итерируется по байтам**:
```go
package main

import "fmt"

func main() {
	// string is a slice of bytes.
	s := "hello你好"

	for i := 0; i < len(s); i++ {
		fmt.Printf("%x ", s[i])
	}
	// result 68 65 6c 6c 6f e4 bd a0 e5 a5 bd
}
```
2. Итерируемся по строке при помощи `forr`, который **декодирует UTF-8 на каждой итерации**. Как мы можем увидеть, вот эти иероглифы занимают по 3 байта:
```go
package main

import "fmt"

func main() {

	// string,
	s := "hello你好"

	// range
	for index, runeValue := range s {
		fmt.Printf("%#U starts at byte position %d\n", runeValue, index)
	}
}


// U+0068 'h' starts at byte position 0
// U+0065 'e' starts at byte position 1
// U+006C 'l' starts at byte position 2
// U+006C 'l' starts at byte position 3
// U+006F 'o' starts at byte position 4
// U+4F60 '你' starts at byte position 5
// U+597D '好' starts at byte position 8
```

#### Создание строк из BYTE *(больше для понимания теории, а не для практики)*:
```go
package main

import (
	"fmt"
)

func main() {

	// converting from a string literal
	t1 := []byte("ABCDE")

	// by a byte slice literal?
	t2 := []byte{'A', 'B', 'C', 'D', 'E'}
	t3 := []byte{65, 66, 67, 68, 69}

	// by copy function
	var t4 = make([]byte, 5)
	copy(t4, "ABCDE")

	fmt.Println(t1) // [65 66 67 68 69]
	fmt.Println(t2) // [65 66 67 68 69]
	fmt.Println(t3) // [65 66 67 68 69]
	fmt.Println(t4) // [65 66 67 68 69]
}
```

#### Создание строк из RUNE *(больше для понимания теории, а не для практики)*: 
```go
package main

import (
	"fmt"
)

func main() {

	var r rune = 27721
	var r1 rune = '汉'
	var r2 rune = '\u6c49'
	var r3 rune = 0b01101100_01001001

	fmt.Println(r)  // 27721
	fmt.Println(r1) // 27721
	fmt.Println(r2) // 27721
	fmt.Println(r3) // 27721

}
```

#### Character - это rune
```go
package main

import (
	"fmt"
)

func main() {

	r := 'Z'
	fmt.Printf("type:%T, value:%v\n", r, r)
	// result: type:int32, value:90
}
```

Строки **всегда** состоят из byte. Но character (что-то в одинарных кавычках) и строки могут превращаться как в rune, так и в byte.

Если мы имеем код `myString = "helloThere"`, то код наподобие `myString[0] = 'b'` не сработает, так как `myString[0]` - **unaddressable** и **immutable**.
