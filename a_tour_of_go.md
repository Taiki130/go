メモしていく
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

## Functions continued
関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略して記述できる

```go
x int, y int
```
```go
x, y int
```

## Multiple results
関数は複数の戻り値を返すことができる

```go
func swap(x, y string) (string, string) {
	return y, x
}
```

## Named return values
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
## Variables
var ステートメントは変数( variable )を宣言
関数の引数リストと同様に、複数の変数の最後に型を書くことで、変数のリストを宣言できる
```go
var c, python, java bool
```

## Variables with initializers
var 宣言では、変数毎に初期化子( initializer )を与えることができる
初期化子が与えられている場合、型を省略できる
その変数は**初期化子が持つ型**になる
```go
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```

## Short variable declarations
**関数の中**では、 var 宣言の代わりに、短い := の代入文を使い、暗黙的な型宣言ができる
```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Basic types
Go言語の基本型(組み込み型)

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 の別名

rune // int32 の別名
     // Unicode のコードポイントを表す

float32 float64

complex64 complex128
```

## Zero values
変数に初期値を与えずに宣言すると、ゼロ値( zero value )が与えられる
```
数値型(int,floatなど): 0
bool型: false
string型: "" (空文字列( empty string ))
```

## Type conversions
変数 v 、型 T があった場合、 T(v) は、変数 v を T 型へ変換する

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
i := 42
f := float64(i)
u := uint(f)
```

## Type inference
右側の変数が型を持っている場合、左側の新しい変数は同じ型になる
```go
var i int
j := i // j is an int
```
右側に型を指定しない数値である場合、左側の新しい変数は右側の定数の精度に基いて int, float64, complex128 の型になる
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## Constants
定数( constant )は、 const キーワードを使って変数と同じように宣言する
定数は、**文字(character)、文字列(string)、boolean、数値(numeric)のみ**で使える
```go
const Pi = 3.14
const World = "世界"
const Truth = true
```

## Numeric Constants
数値の定数は、高精度な 値 ( values )
```go
package main

import "fmt"

const (
	// Create a huge number by shifting a 1 bit left 100 places.
	// In other words, the binary number that is 1 followed by 100 zeroes.
	Big = 1 << 100
	// Shift it right again 99 places, so we end up with 1<<1, or 2.
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}
// 21
// 0.2
// 1.2676506002282295e+29
```
