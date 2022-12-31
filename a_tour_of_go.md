メモしていく
# Packages, variables, and functions.
## Functions
- 関数は、0個以上の引数を取ることができる
- 変数名の 後ろ に型名を書く
```go
func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

## Functions continued
- 関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略して記述できる

```go
x int, y int
```
```go
x, y int
```

## Multiple results
- 関数は複数の戻り値を返すことができる

```go
func swap(x, y string) (string, string) {
	return y, x
}
```

## Named return values
- Goでの戻り値となる変数に名前をつける( named return value )ことができる
- naked returnステートメントは、短い関数でのみ利用すべき
- 長い関数で使うと可読性が下がる

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return 
}
```
## Variables
- var ステートメントは変数( variable )を宣言
- 関数の引数リストと同様に、複数の変数の最後に型を書くことで、変数のリストを宣言できる
```go
var c, python, java bool
```

## Variables with initializers
- var 宣言では、変数毎に初期化子( initializer )を与えることができる
- 初期化子が与えられている場合、型を省略できる
- その変数は**初期化子が持つ型**になる
```go
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```

## Short variable declarations
- **関数の中**では、 var 宣言の代わりに、短い := の代入文を使い、暗黙的な型宣言ができる
```go
func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Basic types
- Go言語の基本型(組み込み型)

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
- 変数に初期値を与えずに宣言すると、ゼロ値( zero value )が与えられる
```
数値型(int,floatなど): 0
bool型: false
string型: "" (空文字列( empty string ))
```

## Type conversions
- 変数 v 、型 T があった場合、 T(v) は、変数 v を T 型へ変換する

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
i := 42
f := float64(i)
u := uint(f)
```

## Type inference
- 右側の変数が型を持っている場合、左側の新しい変数は同じ型になる
```go
var i int
j := i // j is an int
```
- 右側に型を指定しない数値である場合、左側の新しい変数は右側の定数の精度に基いて int, float64, complex128 の型になる
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## Constants
- 定数( constant )は、 const キーワードを使って変数と同じように宣言する
- 定数は、**文字(character)、文字列(string)、boolean、数値(numeric)のみ**で使える
```go
const Pi = 3.14
const World = "世界"
const Truth = true
```

## Numeric Constants
- 数値の定数は、高精度な 値 ( values )
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

# Flow control statements: for, if, else, switch and defer
## For
- 初期化ステートメント: 最初のイテレーション(繰り返し)の前に初期化が実行される
- 条件式: イテレーション毎に評価される
- 後処理ステートメント: イテレーション毎の最後に実行される
```go
for i := 0; i < 10; i++ {
	sum += i
}
```

## For continued
- 初期化と後処理ステートメントの記述は任意
```go
for ; sum < 1000; {
	sum += sum
}
```

## For is Go's "while"
- セミコロン(;)を省略することもできる
- C言語などにある while は、Goでは for だけを使う
```go
sum := 1
for sum < 1000 {
	sum += sum
}
```

## Forever
- ループ条件を省略すれば、無限ループ( infinite loop )になるので、無限ループをコンパクトに表現できる
```go
func main() {
	for {
	}
}
```

## If
```go
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}
```

## If with a short statement
- if ステートメントは、 for のように、条件の前に、評価するための簡単なステートメントを書くことができる
- ここで宣言された変数は、 if のスコープ内だけで有効
```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}
```

## If and else
```go
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
```

## Switch
- Go では選択された case だけを実行してそれに続く全ての case は実行されない
- これらの言語の各 case の最後に必要な break ステートメントが Go では自動的に提供される
- Go の switch の case は定数である必要はなく、 関係する値は整数である必要はない
```go
switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux.")
default:
	// freebsd, openbsd,
	// plan9, windows...
	fmt.Printf("%s.\n", os)
}
```

## Switch evaluation order
- switch caseは、上から下へcaseを評価する
- caseの条件が一致すれば、そこで停止(自動的にbreak)する

## Switch with no condition
- 条件のないswitchは、 switch true と書くことと同じ

## Defer
- defer ステートメントは、 defer へ渡した関数の実行を、呼び出し元の関数の終わり(returnする)まで遅延させるもの
- defer へ渡した関数の引数は、すぐに評価されるが、その関数自体は呼び出し元の関数がreturnするまで実行されない

```go
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
// hello
// world
```

## Stacking defers
- defer へ渡した関数が複数ある場合、その呼び出しはスタック( stack )される
- 呼び出し元の関数がreturnするとき、 defer へ渡した関数は LIFO(last-in-first-out) の順番で実行される

```go
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
// counting
// done
// 9
// 8
// 7
// 6
// 5
// 4
// 3
// 2
// 1
// 0
```