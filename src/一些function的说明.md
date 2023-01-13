

## os



    os.File类型拥有的都是指针方法，它的指针实现了很多io包中的接口。


    os.OpenFile函数
    这个函数其实是os.Create函数和os.Open函数的底层支持，它最灵活。这个函数有3个参数：
    func OpenFile(name string, flag int, perm FileMode) (*File, error) {
        testlog.Open(name)
        return openFileNolog(name, flag, perm)
    }
    name参数，是文件的路径。
    flag ： 操作模式，限定了操作文件方式
    perm ： 权限模式，控制文件的访问权限


    func (file *File) write(b [ ]byte) (n int, err Error)写入byte类型的信息到文件


    File.Seek(offset, whence)，设置光标的位置
    offset,偏移量
    whence，从哪开始：0从头，1当前，2末尾


    func Remove(name string)Error调用该函数就可以删除文件名为name的文件


    func CreateTemp(dir, pattern string)(*File, error)
    CreateTemp 在目录dir中新建一个临时文件，打开文件进行读写，返回结果文件。
    文件名是通过采用模式并在末尾添加一个随机字符串来生成的。如果模式包含"*"，则随机字符串替换最后的"*"。
    如果 dir 是空字符串，CreateTemp 使用默认目录来存放临时文件，正如 TempDir 返回的那样，多个程序或 goroutines 同时调用 CreateTemp 将不会选择同一个文件。
    调用者可以使用文件的 Name 方法来查找文件的路径名。当不再需要该文件时，调用者有责任将其删除。

```
func ReadAll(r io.Reader)([]byte, error)
ReadAll 从 r 读取直到出现错误或 EOF 并返回它读取的数据。成功的调用返回 err == nil，而不是 err == EOF。因为 ReadAll 被定义为从 src 读取直到 EOF，所以它不会将 Read 中的 EOF 视为要报告的错误。

从 Go 1.16 开始，此函数仅调用 io ReadAll
```

```
Truncate
func (f *File) Truncate(size int64) error
官方注释：Truncate改变文件的大小，它不会改变I/O的当前位置。 如果截断文件，多出的部分就会被丢弃。如果出错，错误底层类型是*PathError。
```



![img](https://blog-1314857283.cos.ap-shanghai.myqcloud.com/images/3c9187f15aebe75708859ff651c60615.png)

![img](https://blog-1314857283.cos.ap-shanghai.myqcloud.com/images/1e6e57e81cb5bd008496bf88fa399fab.png)







```
Go原生的包中有一些核心的interface，其中io.Reader/Writer是比较常用的接口。很多原生的结构都围绕这个系列的接口展开，在实际的开发过程中，你会发现通过这个接口可以在多种不同的io类型之间进行过渡和转化。
关系图表:
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
可见，任何实现了 Read() 函数的对象都可以作为 Reader 来使用。Write()也是同理
```



## test

```
标准库 errors.New 和 fmt.Errorf 函数用于创建实现 error 接口的错误对象。通过判断错误对象实例来确定具体错误类型。

err := errors.New("your first demo error")
errWithCtx = fmt.Errorf("index %d is out of bounds", i)
```



```
log.Fatal 、log.Fatalf
示例：
	log.Fatal("Fatal array ", arr, "\n")
	log.Fatalf("Fatalf array with item [%d,%d]\n", arr[0], arr[1])
对于 log.Fatal 接口，会先将日志内容打印到标准输出，接着调用系统的 os.exit(1) 接口退出程序并返回状态 1 
在实际开发中要慎重，它导致整个系统退出，且不执行defer。
```





```
BenchMark__ 性能测试

Example_ 例子,需要写output注释

testing.T(单元测试)

testing.B(性能测试)

testing.TB接口，顾名思义，是前两个共用的接口。
TB接口通过在接口中定义一个名为private(）的私有方法，保证了即使用户实现了类似的接口，也不会跟testing.TB接口冲突。

t.Run()子测试
```

