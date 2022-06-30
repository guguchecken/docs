# Golang Guidelines

## 前言

本规范在[Effective Go](https://go.dev/doc/effective_go)的基础上，根据Matrix Origin实际情况进行调整和补充。	

每项规范内容根据要求设置不同的等级，详情如下：

- **必须（Mandatory）**：用户必须采用；
- **推荐（Preferable）**：用户理应采用，但如有特殊情况，可以不采用；··

## 1. 代码风格

### 依赖规范

- 【推荐】使用 `go modules` 作为依赖管理的项目不要提交` vendor` 目录。
- 【推荐】使用 `go modules` 管理依赖的项目， `go.sum` 文件必须提交，不要添加到 .gitignore 规则中。

### 命名规范

#### 包命名

- 【推荐】保持`package`的名字与目录名称一致
- 【推荐】包名应该为小写单词，不能使用下划线或者混合大小写，应使用多级目录来进行划分层级
- 【推荐】包名缩写应谨慎使用。若缩写有歧义或者不清晰，不应该使用缩写；当缩写时程序员广泛熟知的词时，可以使用缩写，例如：
  - strconv（string conversion）
  - syscal（system call）
- 【推荐】不使用无意义的包名。包名应该追求清晰而且应该尽量的收敛，使其尽量的符合“单一职责”原则。注意，`xx/util/encryption`这样的包名是允许的。

#### 文件命名

- 【必须】应采用有意义，简短的文件名；若是缩写，应是程序员广泛熟知的。
- 【必须】文件名应该使用小写，并且使用下划线分割各个单词。

#### 结构体命名

- 【必须】采用驼峰命名法，首字母根据访问控制规则采用大写或者小写。

- 【必须】结构体名称应该是名词或者名词短语，如`Customer`、`AddressParser`；其不应该是动词，且应该避免使用 `Data`、`Info` 这类意义太宽泛的结构体名。

- 【必须】结构体的声明以及初始化格式都应该使用多行。

  ~~~go
  //good
  type User struct {
      Name  string
      Email string
  }
  
  u := User{
      Name:  "john",
      Email: "john@example.com",
  }
  ~~~

  

#### 接口命名

- 【推荐】命名规则基本保持和结构体命名规则一致。

- 【推荐】单个函数的接口名以 `er` 作为后缀，例如 `Reader`，`Writer`。

  ~~~go
  //good
  type Reader interface {
      Read(p []byte) (n int, err error)
  }
  ~~~

- 【推荐】两个函数的接口名综合两个函数名。

- 【推荐】三个以上函数的接口名，类似于结构体名。

  ~~~go
  //good
  // Car
  type Car interface {
      // Start ...
      Start([]byte)
      // Stop ...
      Stop() error
      // Recover ...
      Recover()
  }
  ~~~

  

#### 变量命名

- 【必须】变量名必须遵循驼峰式，首字母根据访问控制决定使用大写或小写。

- 【必须】特有名词时，需要遵循以下规则：
  - 如果变量为私有，且特有名词为首个单词，则使用小写，如 `apiClient`；
  - 其他情况都应该使用该名词原有的写法，如 `APIClient`、`repoID`、`UserID`；
  - 错误示例：`UrlArray`，应该写成 `urlArray` 或者 `URLArray`；
  - 详细的专有名词列表可参考[这里](https://github.com/golang/lint/blob/738671d3881b9731cc63024d5d88cf28db875626/lint.go#L770)。

- 【必须】私有全局变量和局部变量规范一致，均以小写字母开头。

- 【必须】代码生成工具自动生成的代码可排除此规则（如 xxx.pb.go 里面的 Id）。

- 【必须】变量名更倾向于选择短命名。特别是对于局部变量。 `c`比`lineCount`要好，`i`比`sliceIndex`要好。基本原则是：变量的使用和声明的位置越远，变量名就需要具备越强的描述性。

#### 常量命名

- 【必须】常量均需遵循驼峰式。

- 私有全局常量和局部变量规范一致，均以小写字母开头。

  ~~~go
  //good
  const appVersion = "1.0.0"
  ~~~

  

#### 函数命名

- 函数名必须遵循驼峰式，首字母根据访问控制决定使用大写或小写。
- 代码生成工具自动生成的代码可排除此规则（如协议生成文件 xxx.pb.go , gotests 自动生成文件 xxx_test.go 里面的下划线）。

#### 泛型命名

- 【推荐】一般泛型命名使用`T`，也可以按照以下命名规范命名：
  - T：通用泛型类型，通常作为第一个泛型类型。
  - S：通用泛型类型，如果需要使用多个泛型类型，可以将S作为第二个泛型类型
  - U：通用泛型类型，如果需要使用多个泛型类型，可以将U作为第三个泛型类型
  - V：通用泛型类型，如果需要使用多个泛型类型，可以将V作为第四个泛型类型
  - E：集合元素 泛型类型，主要用于定义集合泛型类型
  - K：映射-键 泛型类型，主要用于定义映射泛型类型
  - V：映射-值 泛型类型，主要用于定义映射泛型类型
  - N：数值 泛型类型，主要用于定义数值类型的泛型类型
- 【推荐】约束定义时禁止使用~

### 代码格式

#### 代码行

- 【必须】代码必须使用`gofmt`工具进行格式化

- 【必须】一个代码文件不应超过800行，一个函数长度不应超过80行

- 【必须】一行代码不能超过`120`列；若超过，则应使用合理的方式换行。例外场景不受此约束：

  - import模块语句
  - struct tag
  - 工具生成的代码

- 【必须】不添加没有必要的括号

  ~~~go
  //bad
  if foo && (int(bar) > 0) {
      //do something...
  }
  
  //good
  if foo && int(bar) > 0 {
      //do something...
  }
  ~~~

- 【必须】运算符与操作数之间要留空格；作为输入参数或者数组下标时，运算符和操作数之间不需要空格，紧凑展示。（遵循gofmt的逻辑）

  ~~~go
  //good
  var i int = 1 + 2
  v := []float64{1.0, 2.0, 3.0}[i-i]
  fmt.Printf("%f\n", v+1)
  ~~~

- 【必须】采用惰性换行，换行前应尽可能占满当行不留空位

  ~~~go
  // Bad
  fmt.Printf("%v %v %v %v %v %v %v %v %v %v %v %v %v %v\n",
  0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55,89, 144, 233)
  
  // Good
  fmt.Printf("%v %v %v %v %v %v %v %v %v %v %v %v %v %v\n", 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55,
  89, 144, 233)
  ~~~

- 【必须】使用原始字符串文字避免转义

  ~~~go
  // bad
  wantError := "unknown name:\"test\""
  
  // good
  wantError := `unknown error:"test"`
  ~~~

  

#### 空行

- 【必须】函数体的第一行不要换行

  ~~~go
  //bad
  func foo() {
  
      // func body
  }
  
  //good
  func foo() {
      // func body
  }
  ~~~

- 【必须】函数调用和对调用结果的处理，是紧密相连的，不能加空行。

  ~~~go
  //bad
  res, err := foo()
  
  if err != nil || res.Ret != 0 {
      return
  }
  
  //good
  res, err := foo()
  if err != nil || res.Ret != 0 {
      return
  }
  ~~~




### 控制语句

#### if

- 【推荐】应尽量减少` if…else… `语句的使用，若必须要使用，语句应不多于三层

  ~~~go
  //bad
  if condition {
      ...
  }else {
      ...
  }
  
  //good
  if a > 0 {
      ...
      return ...
  }
  //else的代码
  ~~~

- 【推荐】`if`中可以进行初始化语句，则局部变量应该使用以下方式创建：

  ~~~go
  //bad
  err := file.Chmod(0664)
  if err != nil {
      return err
  }
  
  //good
  if err := file.Chmod(0664); err != nil {
      return err
  }
  ~~~

- 【推荐】`if`对两个值进行判断时，应：变量在左，常量在右：

  ~~~go
  //bad
  if nil != err {
      //err handling...
  }
  
  //good
  if err != nil {
      //err handling...
  }
  ~~~

- 【推荐】`if`对两个值进行比较并且返回比较结果时，不应使用`if`，应直接返回

  ~~~go
  //bad
  if a > b {
      return true
  }
  reurn false
  
  //good
  return a > b
  ~~~

- 【推荐】`if`对于`bool`类型的变量，应直接进行判断

  ~~~go
  //bad
  if flag == true {
      //do something...
  }
  
  //good
  if flag {
      //do something...
  }
  ~~~

- 【推荐】`if`中除了常用方法（例如：isPrime），不要在`if`中执行复杂的语句

  ~~~go
  //bad
  if XXXXXXisXXXXXXX(20) == nil {
      //do something...
  }
  
  //good
  if isPrime(10){
      //do something...
  }
  ~~~
  
  

#### for

- 【推荐】使用短声明建立局部变量

  ~~~go
  sum := 0
  for i := 0; i < 10; i++ {
      sum += 1
  }
  ~~~

- 【必须】使用`for...range` 时，若只需要第一项（key），则丢弃第二项

  ~~~go
  for key := range m {
      if key.expired() {
          delete(m, key)
      }
  }
  ~~~

- 【必须】使用`for range`时，若只需要第二项，则把第一项设置为下划线`_`

  ~~~go
  sum := 0
  for _, value := range array {
      sum += value
  }
  ~~~

  

#### switch

- 【必须】`switch`中必须要有`default`语句，并且需要将其放到最后，即使default中没有内容

  ~~~go
  //bad
  switch a {
  case 1 :
      //do something...
  case 2 :
      //do something...
  ...
  case n :
      //do something...
  }
  
  //good
  switch a {
  case 1 :
      //do something...
  case 2 :
      //do something...
  ...
  case n :
      //do something...
  default :
      //do default...
  }
  ~~~

  

#### return

- 【必须】一旦发生错误，应立刻`return`

  ~~~go
  //bad
  f,err := os.Open(file_name)
  defer f.Close()
  
  //do something...
  
  //good
  f,err := os.Open(file_name)
  defer f.Close()
  
  if err != nil {
      return err
  }
  //do something...
  ~~~

  

#### goto

- 【必须】代码中禁止使用`goto`

#### defer

- 【必须】对于文件或者锁，应使用defer进行资源的释放

  ~~~go
  var mu sync.Mutex
  var a int = 10
  //bad
  mu.Lock()
  if a < 10 {
      mu.Unlock()
      return a
  }
  
  a++
  mu.Unlock()
  return a
  
  //good
  mu.Lock()
  defer mu.Unlock()
  
  if a < 10 {
      return a
  }
  
  a++
  return a
  ~~~

- 【推荐】应尽量避免在for循环中使用defer

## 2. 错误处理

- 【必须】应使用`moerr`进行错误处理，详情参见`pkg/common/moerr`

### error

- 【必须】如果error作为函数的返回值，则必须对error进行显式处理或者使用空白标识符忽略。对于`defer xx.Close()`可以不用显式处理。

- 【必须】error 作为函数返回值且有多个返回值的时候，error 必须是最后一个参数。

  ~~~go
  // bad
  func do() (error, int) {
  }
  
  // good
  func do() (int, error) {
  }
  ~~~

- 采用独立的错误流进行错误处理

  ~~~go
  // bad
  if err != nil {
      //handle error...
  }else {
      //normal code...
  }
  
  // good
  if err != nil {
      //handle erroe...
      return //or continue, etc.
  }
  //normal code... 
  ~~~

- 错误判断独立处理，不与其他变量组合判断。

  ~~~go
  // bad
  x, y, err := f()
  if err != nil || y == nil {
      return err
  }
  
  // good
  x, y, err := f()
  if err != nil {
      return err
  }
  if y == nil {
      return fmt.Errorf("some error")
  }
  ~~~

  

### panic

- 【必须】使用断言进行类型转换，若转换失败则会导致`panic`，则应该使用`comma ok`来进行处理

  ~~~go
  //bad
  t := i.(string)
  
  //good
  t,ok := i.(string)
  if !ok {
      //do something...
  }
  ~~~

- 【必须】应尽量避免panic。当error发生时，应该向上游调用者返回error，容许调用者对error进行检测和处理；当错误不能够处理的时候再进行panic
- 【推荐】不应该恢复panic，除非在少数已经定义好的地方
- 【必须】应阻止运行时panic的发生。若运行时发生panic，应将其转换为error

## 3. 并发处理

### Goroutine

- 【必须】业务开发中应根据实际情况限制Goroutine数量

- 【必须】`for range`遍历时，若在循环体中使用并发，需要将`key`或者`value`当作参数传入

  ~~~go
  var arr []int= []int{1,2,3}
  
  //bad
  for _,value := range arr {
      go func(){
          fmt.Println("value is ",value) 
      }()
  }
  
  //good
  for _,value := range arr {
      go func(value){
          fmt.Println("value is ",num) 
      }(num int)
  }
  ~~~

- 【推荐】启动的 goroutine 最好有 recover。因为其他 goroutine 是无法捕当前 goroutine 抛出的异常。如果启动的 goroutine 没有 recover，很容易发生 panic 导致整个进程退出。

### channel

- 【推荐】使用channel时，最好将size设置为1或者使用 unbuffered channel，以减少复杂度。

  ~~~go
  // bad
  // Ought to be enough for anybody!
  c := make(chan int, 64)
  
  // good
  // Size of one
  c := make(chan int, 1) // or
  // Unbuffered channel, size of zero
  c := make(chan int)
  ~~~

  

### Mutex

- 【推荐】零值 sync.Mutex 和 sync.RWMutex 是有效的，所以指向 mutex 的指针基本是不必要的。

  ~~~go
  // Bad
  mu := new(sync.Mutex)
  mu.Lock()
  
  // Good
  var mu sync.Mutex
  mu.Lock()
  ~~~

- 【必须】不应该把Mutex嵌入到另外一个结构体中，即使该结构体不会被导出。

  ~~~go
  // bad
  type sMap struct {
    sync.Mutex
    data map[string]string
  }
  
  // good
  type sMap struct {
    mutex sync.Mutex
    data map[string]string
  }
  ~~~

- 【推荐】优先使用共享锁而不是互斥锁

### Context

- 【推荐】推荐以参数的形式传递Context
- 【推荐】若以Context做i为函数的参数，应该把Context作为第一个参数
- 【推荐】给一个函数方法传递Context的时候，不要传递nil，如果不知道传递什么，就使用context.TODO
- 【推荐】函数调用链必须传播Context，实现完整链路上的控制。



## 4. 单元测试

- 【必须】单元测试文件名命名规范为 example_test.go。

- 【必须】单测文件行数限制是普通文件的 2 倍（1600 行）。单测函数行数限制也是普通函数的2倍（160行）。其他规范细节和普通文件保持一致。

- 【推荐】由于单测文件内的函数都是不对外的，所有可导出函数可以没有注释，但是结构体定义时尽量不要导出。

- 【必须】每个重要的可导出函数都要首先编写测试用例，测试用例和正规代码一起提交方便进行回归测试。

- 【推荐】使用表驱动的方式编写用例，代码看上去会更简洁。

  ~~~go
  // bad
  // func TestSplitHostPort(t *testing.T)
  
  host, port, err := net.SplitHostPort("192.0.2.0:8000")
  require.NoError(t, err)
  assert.Equal(t, "192.0.2.0", host)
  assert.Equal(t, "8000", port)
  
  host, port, err = net.SplitHostPort("192.0.2.0:http")
  require.NoError(t, err)
  assert.Equal(t, "192.0.2.0", host)
  assert.Equal(t, "http", port)
  
  host, port, err = net.SplitHostPort(":8000")
  require.NoError(t, err)
  assert.Equal(t, "", host)
  assert.Equal(t, "8000", port)
  
  host, port, err = net.SplitHostPort("1:8")
  require.NoError(t, err)
  assert.Equal(t, "1", host)
  assert.Equal(t, "8", port)
  
  // good
  // func TestSplitHostPort(t *testing.T)
  
  tests := []struct{
    give     string
    wantHost string
    wantPort string
  }{
    {
      give:     "192.0.2.0:8000",
      wantHost: "192.0.2.0",
      wantPort: "8000",
    },
    {
      give:     "192.0.2.0:http",
      wantHost: "192.0.2.0",
      wantPort: "http",
    },
    {
      give:     ":8000",
      wantHost: "",
      wantPort: "8000",
    },
    {
      give:     "1:8",
      wantHost: "1",
      wantPort: "8",
    },
  }
  
  for _, tt := range tests {
    t.Run(tt.give, func(t *testing.T) {
      host, port, err := net.SplitHostPort(tt.give)
      require.NoError(t, err)
      assert.Equal(t, tt.wantHost, host)
      assert.Equal(t, tt.wantPort, port)
    })
  }
  ~~~

  

## 5. 性能增强

### 内存管理

- 【推荐】尽量在初始化时指定`map`的容量，以减少添加元素时`map`的增长以及内存的分配开销

  ~~~go
  //bad
  m := make(map[string]os.FileInfo)
  
  files, _ := ioutil.ReadDir("./files")
  for _, f := range files {
      m[f.Name()] = f
  }
  
  //good
  files, _ := ioutil.ReadDir("./files")
  
  m := make(map[string]os.FileInfo, len(files))
  for _, f := range files {
      m[f.Name()] = f
  }
  ~~~

- 【推荐】尽量在初始化时指定`slice`的容量(`cap`)，以减少添加元素时`slice`的增长以及内存的分配开销

  ~~~go
  const size = 1000000
  
  //bad
  for n := 0; n < b.N; n++ {
  	data := make([]int, 0)
    	for k := 0; k < size; k++ {
      	data = append(data, k)
    }
  }
  
  //BenchmarkBad-4    219    5202179 ns/op
  
  //good
  for n := 0; n < b.N; n++ {
  	data := make([]int, 0, size)
    	for k := 0; k < size; k++ {
      	data = append(data, k)
    }
  }
  
  //BenchmarkGood-4   706    1528934 ns/op
  ~~~

- 【推荐】使用`map`时，若只需要`key`，不需要`value`或者`value`只用来进行标记,可将value类型设置为空结构体,以减少内存使用

  ~~~go
  //bad
  m := make(map[int]bool,10)
  if _,ok := m[key];ok{
      //do something...
  }
  
  //good
  m := make(map[int]struct{},10)
  if _,ok := m[key];ok {
      //do something...
  }
  ~~~

- 【推荐】struct 布局应考虑内存对齐。应调整struct字段的顺序，将字段宽度从小到大由上到下排列，来减少内存的占用。

  ~~~go
  // bad
  type demo2 struct {
  	a int8
  	c int32
  	b int16
  }
  
  // good
  type demo1 struct {
  	a int8
  	b int16
  	c int32
  }
  ~~~

  

### 字符串转换

- 【推荐】当基本类型与`string`相互转换时，`strconv`要比`fmt`快

  ~~~go
  //bad
  for i := 0; i < b.N; i++ {
    s := fmt.Sprint(rand.Int())
  }
  
  // BenchmarkFmtSprint-4    143 ns/op    2 allocs/op
  
  //good
  for i := 0; i < b.N; i++ {
    s := strconv.Itoa(rand.Int())
  }
  
  // BenchmarkStrconv-4    64.2 ns/op    1 allocs/op
  ~~~

- 【推荐】避免`固定字符串`到`byte切片`的重复转化，应只进行一次转换并捕获结果

  ~~~go
  //bad
  for i := 0; i < b.N; i++ {
    w.Write([]byte("Hello world"))
  }
  
  // BenchmarkBad-4   50000000   22.2 ns/op
  
  //good
  data := []byte("Hello world")
  for i := 0; i < b.N; i++ {
    w.Write(data)
  }
  
  // BenchmarkGood-4  500000000   3.25 ns/op
  ~~~

- 【推荐】行内字符串拼接推荐使用`+`，多行字符串拼接推荐使用`strings.Builder`



## 6. 注释规范

### 包注释

- 【必须】每个包都应该有一个包注释

- 【必须】包如果有多个文件，包注释只需要出现在一个包文件（一般是和包同名的文件）中即可，格式为`//Package 包名 包信息描述`

  ~~~go
  // Package math provides basic constants and mathematical functions.
  package math
  
  // or
  
  /*
  Package template implements data-driven templates for generating textual
  output such as HTML.
  ....
  */
  package template
  ~~~

### type注释

- 【必须】需要`导出`的自定义结构体或者接口都必须要有注释说明对结构体进行简要说明，放在结构体定义的前一行，格式为`//结构体名称 结构体信息描述`

- 【必须】对于结构体内的可导出成员变量，如果是个生僻字或者表意不明确的字，必须要给出注释，放在成员变量的前一行或者同一行的末尾。

  ~~~go
  // User 用户结构定义了用户基础信息
  type User struct {
      Name  string
      Email string
      // Demographic 族群
      Demographic string
  }
  ~~~

- 每个需要导出的类型定义（type definition）和类型别名（type aliases）都必须有注释说明。对类型进行简要介绍，放在定义的前一行。格式为：`// 类型名 类型信息描述`。

  ~~~go
  // StorageClass 存储类型
  type StorageClass string
  
  // FakeTime 标准库时间的类型别名
  type FakeTime = time.Time
  ~~~

  

### 函数注释

- 【必须】需要导出的函数都必须要有注释。注释描述函数的功能，调用方等信息。格式为`//函数名 函数信息描述`。

  ~~~go
  // NewtAttrModel 是属性数据层操作类的工厂方法
  func NewAttrModel(ctx *common.Context) *AttrModel {
      // TODO
  }
  ~~~

  

### 变量，常量注释

- 每个需要导出的常量和变量都必须有注释说明。该注释对常量或变量进行简要介绍，放在常量或者变量定义的前一行。格式为`// 变量名 变量信息描述`。

- 大块常量或变量定义时，可在前面注释一个总的说明，然后每一行常量的末尾详细注释该常量的定义。格式同上

  ~~~go
  // FlagConfigFile 配置文件的命令行参数名
  const FlagConfigFile = "--config"
  
  // 命令行参数
  const (
      FlagConfigFile1 = "--config" // 配置文件的命令行参数名1
      FlagConfigFile2 = "--config" // 配置文件的命令行参数名2
      FlagConfigFile3 = "--config" // 配置文件的命令行参数名3
      FlagConfigFile4 = "--config" // 配置文件的命令行参数名4
  )
  
  // FullName 返回指定用户名的完整名称
  var FullName = func(username string) string {
      return fmt.Sprintf("fake-%s", username)
  }
  ~~~

  

## Appendix

### 开发环境配置

此处初步规划填写VS Code Go开发环境配置

### 常用工具