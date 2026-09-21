# Slice: append и изменение элементов

## Задача

Что выведет программа в местах, отмеченных комментариями?

Объяснять поведение программы пока не требуется — необходимо определить результат каждого `fmt.Println`.

## Исходный код

```go
package main

import "fmt"

func main() {
	nums := make([]int, 1, 2)

	fmt.Println(nums) // ?

	appendSlice(nums, 1024)
	fmt.Println(nums) // ?

	mutateSlice(nums, 1, 512)
	fmt.Println(nums) // ?
}

func appendSlice(sl []int, val int) {
	sl = append(sl, val)
}

func mutateSlice(sl []int, idx, val int) {
	sl[idx] = val
}
```

## Вопрос

Что произойдёт при выполнении программы и что будет выведено каждым `fmt.Println`?