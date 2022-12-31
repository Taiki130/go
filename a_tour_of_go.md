# Packages, variables, and functions.

## Functions

関数は、0個以上の引数を取ることができる。
変数名の 後ろ に型名を書く。
```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}

```
