### []byte位操作<<转int数据不正确
*现场还原：设备端通过mqqt向服务器发送自定义数据格式将int转[]byte传输,服务器做解析，原数据为262，解析后是16。*
``` go
# 原代码
taskId = int(((status[3] & 0xff) << 24) | ((status[4] & 0xff) << 16) | ((status[5] & 0xff) << 8) | (status[6] & 0xff))

# 修复
taskId = int(binary.BigEndian.Uint32(status[3:7]))

# 原因是golang的位移操作<<不会自动提升变量的长度，和java相同。
```