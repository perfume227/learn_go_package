
# bytes
### [type Buffer](https://golang.org/src/bytes/buffer.go#L17)

Buffer 是一个有Read和write方法的变长缓存。
Buffer的零值是空缓存。

```golang
type Buffer struct {
	buf       []byte   // contents are the bytes buf[off : len(buf)]
	off       int      // read at &buf[off], write at &buf[len(buf)]
	bootstrap [64]byte // memory to hold first slice; helps small buffers avoid allocation.
	lastRead  readOp   // last read operation, so that Unread* can work correctly.
}
```
> gloria:其中的bootstrap作为缓存的第一个切片，当需要的缓存小于或等于其长度（64）则使用它，否则将申请更多的缓存空间。

- func [NewBuffer](https://golang.org/src/bytes/buffer.go#L414)(buf []byte) *Buffer
- func [NewBufferString](https://golang.org/src/bytes/buffer.go#L422)(s string) *Buffer
- func (b *Buffer) Bytes() []byte
- func (b *Buffer) Cap() int
- func (b *Buffer) [Grow](https://golang.org/src/bytes/buffer.go#L124)(n int)
	- 扩展缓存容量，如果有必要会扩展两个n的容量，在容量不够时才会有新空间申请
- func (b *Buffer) Len() int
- func (b *Buffer) Next(n int) []byte
- func (b *Buffer) Read(p []byte) (n int, err error)
- func (b *Buffer) ReadByte() (byte, error)
- func (b *Buffer) ReadBytes(delim byte) (line []byte, err error)
- func (b *Buffer) ReadFrom(r io.Reader) (n int64, err error)
- func (b *Buffer) ReadRune() (r rune, size int, err error)
- func (b *Buffer) ReadString(delim byte) (line string, err error)
- func (b *Buffer) Reset()
	- 清空缓存
- func (b *Buffer) String() string
- func (b *Buffer) [Truncate](https://golang.org/src/bytes/buffer.go#L70)(n int)
	- 截断缓存，丢弃off+n以后到数据，这里off一般为0，如果n比现有缓存还大，则panic
- func (b *Buffer) UnreadByte() error
- func (b *Buffer) UnreadRune() error
- func (b *Buffer) [Write](https://golang.org/src/bytes/buffer.go#L135)(p []byte) (n int, err error)
	- 追加p切片的内容到缓存，有需要的话扩展缓存容量
- func (b *Buffer) WriteByte(c byte) error
- func (b *Buffer) WriteRune(r rune) (n int, err error)
- func (b *Buffer) WriteString(s string) (n int, err error)
- func (b *Buffer) [WriteTo](https://golang.org/src/bytes/buffer.go#L208)(w io.Writer) (n int64, err error)
	- 将缓存内容写到w中
