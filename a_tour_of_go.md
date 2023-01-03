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

# More types: structs, slices, and maps.
## Pointers
https://zenn.dev/genki86web/articles/a0ae1d57ad1806
- 変数 T のポインタは、 *T 型で、ゼロ値は nil
```go
var p *int
```
- & オペレータは、そのオペランド( operand )へのポインタを引き出す
```go
i := 42
p = &i
```
- * オペレータは、ポインタの指す先の変数を示す
```go
fmt.Println(*p) // ポインタpを通してiから値を読みだす
*p = 21         // ポインタpを通してiへ値を代入する
```
- fmt.Println(*p)すると、ポインタpにある変数の値を引き出せる？

## Structs
https://zenn.dev/marietty/articles/b3e0efd8e92078
- struct (構造体)は、フィールド( field )の集まり
- クラスの代わりのような機能をもつ
```go
type Vertex struct {
	X int
	Y int
}
```

## Struct Fields
- structのフィールドは、ドット( . )を用いてアクセスする
```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## Pointers to structs
```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

## Struct Literals
- structリテラルは、フィールドの値を列挙することで新しいstructの初期値の割り当てを示す
- Name: 構文を使って、フィールドの一部だけを列挙することができる
- & を頭に付けると、新しく割り当てられたstructへのポインタを戻す

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```

## Arrays
- [n]T 型は、型 T の n 個の変数の配列( array )を表す
- 配列の長さは、型の一部分
- 配列のサイズを変えることはできない
```go
func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```

## Slices
- 配列は固定長
- スライスは可変長、より柔軟な配列とみなすことができる
- 型 []T は 型 T のスライスを表す
```go
a[low : high]
```
```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4]
```

## Slices are like references to arrays
- スライスは配列への参照
- スライスの要素を変更すると、その元となる配列の対応する要素が変更される
- 同じ元となる配列を共有している他のスライスは、それらの変更が反映される

```go
func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
// [John Paul George Ringo]
// [John Paul] [Paul George]
// [John XXX] [XXX George]
// [John XXX George Ringo]
```

## Slice literals
- スライスのリテラルは長さのない配列リテラルのようなもの
```go
func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
// [2 3 5 7 11 13]
// [true false true true false true]
// [{2 true} {3 false} {5 true} {7 true} {11 false} {13 true}]
```


## Slice defaults
- スライスするときは、それらの既定値を代わりに使用することで上限または下限を省略できる
- 既定値は下限が 0 で上限はスライスの長さ
- これらのスライス式は等価
```go
a[0:10]
a[:10]
a[0:]
a[:]
```
```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}

	s = s[1:4]
	fmt.Println(s)

	s = s[:2]
	fmt.Println(s)

	s = s[1:]
	fmt.Println(s)
}
```

## Slice length and capacity
- スライスは長さ( length )と容量( capacity )の両方を持っている
- スライスの長さは、それに含まれる要素の数
- スライスの容量は、スライスの最初の要素から数えて、元となる配列の要素数
- スライス s の長さと容量は len(s) と cap(s) という式を使用して得ることができる
- 十分な容量を持って提供されているスライスを再スライスすることによって、スライスの長さを伸ばすことができる
```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
// len=6 cap=6 [2 3 5 7 11 13]
// len=0 cap=6 []
// len=4 cap=6 [2 3 5 7]
// len=2 cap=4 [5 7]
```

## Nil slices
- スライスのゼロ値は nil
## Creating a slice with make
- スライスは、組み込みの make 関数を使用して作成することができる
- make 関数はゼロ化された配列を割り当て、その配列を指すスライスを返す
```go
a := make([]int, 5)  // len(a)=5
```
- make の3番目の引数に、スライスの容量( capacity )を指定できる
- cap(b) で、スライスの容量を返す
```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

## Slices of slices
- スライスは、他のスライスを含む任意の型を含むことができる
```go
func main() {
	// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```

## Appending to a slice
- append への最初のパラメータ s は、追加元となる T 型のスライス
- パラメータ s は、追加元となる T 型のスライスです。 残りの vs は、追加する T 型の変数群
- もし、元の配列 s が、変数群を追加する際に容量が小さい場合は、より大きいサイズの配列を割り当て直す
- その場合、戻り値となるスライスは、新しい割当先を示すようになる
```go
func append(s []T, vs ...T) []T
```

```go
var s []int
// []

s = append(s, 0)
// [0]

s = append(s, 1)
// [0 1]

s = append(s, 2, 3, 4)
// [0 1 2 3 4]
```

## Range
- for ループに利用する range は、スライスや、マップ( map )をひとつずつ反復処理するために使う
- スライスをrangeで繰り返す場合、rangeは反復毎に2つの変数を返す
- 1つ目の変数はインデックス( index )で、2つ目はインデックスの場所の要素のコピー

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
// 2**0 = 1
// 2**1 = 2
// 2**2 = 4
// 2**3 = 8
// 2**4 = 16
// 2**5 = 32
// 2**6 = 64
// 2**7 = 128
```
## Range continued
- インデックスや値は、 " _ "(アンダーバー) へ代入することで捨てることができる
```go
for i, _ := range pow
for _, value := range pow
```
- もしインデックスだけが必要なのであれば、2つ目の値を省略する
```go
for i := range pow
```

## Maps
- マップのゼロ値は nil
- nil マップはキーを持っておらず、またキーを追加することもできない
- make 関数は指定された型のマップを初期化して、使用可能な状態で返す

```go
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
```

## Map literals
```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```

## Map literals continued
- mapに渡すトップレベルの型が単純な型名である場合は、リテラルの要素から推定できるので、その型名を省略することができる
```go
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

## Mutating Maps

- m へ要素(elem)の挿入や更新
```go
m[key] = elem
```
- 要素の取得
```go
elem = m[key]
```
- 要素の削除
```go
delete(m, key)
```
- キーに対する要素が存在するかどうかは、2つの目の値で確認
```go
elem, ok = m[key]
```
- m に key があれば、変数 ok は true となり、存在しなければ、 ok は false となる

## Function values
```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}
```

## Function closures
- Goの関数は クロージャ( closure ) 
- クロージャは、それ自身の外部から変数を参照する関数値

# Methods and interfaces
## Methods
- 型にメソッド( method )を定義できる
- メソッドは、特別なレシーバ( receiver )引数を関数に取る
- レシーバは、 func キーワードとメソッド名の間に自身の引数リストで表現する
```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```
- Abs メソッドは v という名前の Vertex 型のレシーバを持つことを意味している
-> Vertex型にメソッドを定義しているってこと？

## Methods are functions
- 先程のメソッドを通常の関数として記述すると以下
```go
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}
```

## Methods continued
- レシーバを伴うメソッドの宣言は、レシーバ型が同じパッケージにある必要がある
- 他のパッケージに定義している型に対して、レシーバを伴うメソッドを宣言できない

## Pointer receivers
- ポインタレシーバでメソッドを宣言できる
https://tech.notti.link/4810dcb
### 値レシーバ
```go
type Person struct{ Name string }

// Person 型に対してメソッドを定義する
func (p Person) Greet(msg string) {
    fmt.Printf("%s, I'm %s.\n", msg, p.Name)
}

func main() {
    p := Person{Name: "Taro"} // 値型の変数を用意する
    p.Greet("Hi")             //=> Hi, I'm Taro.
}
```
- メソッドによってレシーバが指す変数を上書きできたら困るとき
- Getter として使う場合
- 参照だけであることを明示できる

### ポインタレシーバ
```go
type Person struct{ Name string }

// *Person 型に対してメソッドを定義する
func (pp *Person) Shout(msg string) {
    fmt.Printf("%s!!!\n", msg)
}

func main() {
    pp := &Person{Name: "Taro"} // ポインタ型の変数を用意する
    pp.Shout("OH MY GOD")       //=> OH MY GOD!!!
}
```
- メソッド内でレシーバが指す変数を上書きする必要があるとき
- メソッドを呼び出すたびに変数がコピーされるとまずいとき
- レシーバが巨大な変数である場合に有効

- 基本的にはポインタレシーバを使い、レシーバが指す変数が上書きされないことを保証したいときだけ値レシーバを使うことが多い印象

## Methods and pointer indirection
- メソッドがポインタレシーバである場合、呼び出し時に、変数、または、ポインタのいずれかのレシーバとして取ることができる
- Scale メソッドはポインタレシーバを持つ場合、Goは利便性のため、 v.Scale(5) のステートメントを (&v).Scale(5) として解釈する
```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

- 変数の引数を取る関数は、特定の型の変数を取る必要がある
```go
var v Vertex
fmt.Println(AbsFunc(v))  // OK
fmt.Println(AbsFunc(&v)) // Compile error!
// &vに型はないため？
```
- メソッドが変数レシーバである場合、呼び出し時に、変数、または、ポインタのいずれかのレシーバとして取ることができる
```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```
- この場合、 p.Abs() は (*p).Abs() として解釈される

## Interfaces are implemented implicitly
- Interfaceはメソッドの塊
- Interfaceが期待するメソッド（例ではFuncA,FuncB）をすべて満たした変数には、自動的にInterfaceが実装される
- Interfaceを満たした変数はInterfaceへ代入することができる
```go
type Hoge interface {
	FuncA()
        FuncB()
}

type Foo struct {}

func (f *Foo) FuncA() {}
func (f *Foo) FuncB() {}

func main() {
        var hoge Hoge
        hoge = Foo{..}       // HogeインターフェースにFoo構造体を代入できる
}
```

## The empty interface
- ゼロ個のメソッドを指定されたインターフェース型は、 空のインターフェース と呼ばれる
- 空のインターフェースは、任意の型の値を保持できる
- 空のインターフェースは、未知の型の値を扱うコードで使用される
```go
interface{}
```

```go
func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## Type assertions
- 型アサーション は、インターフェースの値の基になる具体的な値を利用する手段を提供する
- i が T を保持していない場合、この文は panic を引き起こす
```go
t := i.(T)
```
- i が T を保持していれば、 t は基になる値になり、 ok は真(true)になる
- そうでなければ、 ok は偽(false)になり、 t は型 T のゼロ値になり panic は起きない
```go
func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s) // hello

	s, ok := i.(string)
	fmt.Println(s, ok) // hello true

	f, ok := i.(float64)
	fmt.Println(f, ok) // 0 false

	f = i.(float64) // panic
	fmt.Println(f)
}
```
