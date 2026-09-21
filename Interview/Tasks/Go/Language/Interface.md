
## Задача

Дан следующий Go-код.
Необходимо определить, что выведет программа, и объяснить полученный результат.

## Исходный код

```go
package main

import "fmt"

type MyErr struct{}

func (e *MyErr) Error() string {
	return "boom"
}

func do() error {
	var e *MyErr
	return e
}

func main() {
	err := do()

	fmt.Println(err == nil)
}
```
