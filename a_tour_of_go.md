# Packages, variables, and functions.

## Functions

関数は、0個以上の引数を取ることができる。
変数名の 後ろ に型名を書く。
```go
func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略して記述できる。

```go
x int, y int
```
```go
x, y int
```
