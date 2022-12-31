# Packages, variables, and functions.

## Functions

関数は、0個以上の引数を取ることができる
変数名の 後ろ に型名を書く
```go
func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略して記述できる

```go
x int, y int
```
```go
x, y int
```

関数は複数の戻り値を返すことができる

```go
func swap(x, y string) (string, string) {
	return y, x
}
```

## variables
Goでの戻り値となる変数に名前をつける( named return value )ことができる
naked returnステートメントは、短い関数でのみ利用すべき
長い関数で使うと可読性が下がる

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return 
}
```
