#基本常识#
byte-字节：字节是计算机存储容量的基本单位，一个字节由8位二进制数组成  
在计算机内部，一个字节可以表示一个数据，也可以表示一个英文字母，两个字节可以表示一个汉字。
1Byte=8bit  (1B＝8bit) 
![](https://i.imgur.com/PnXhTcU.png)
# 取值范围 #
- **byte**             -127~128(-2的7次方到2的7次方-1)
- **short**  -32768~32767(-2的15次方到2的15次方-1)
- **int** -2147483648~2147483647(-2的31次方到2的31次方-1)
- **long** 	-9223372036854774808~9223372036854774807(-2的63次方到2的63次方-1)
- **float** 3.402823e+38 ~ 1.401298e-45（e+38表示是乘以10的38次方，同样，e-45表示乘以10的负45次方）
- **double** 	1.797693e+308~ 4.9000000e-324
- **char** 	\u0000~\uFFFF
- **boolean**  	false、true

#八大类型详解#
## 从小到大的转换(隐式转换) ##
图中从左向右的转换都是隐式转换，无需再代码中进行强制转换
![](https://i.imgur.com/Z2mwvv3.png)
## 各个类型与byte数组的相互转换##
**byte**: 字节类型 占8位二进制 00000000<br>
**char**: 字符类型 占2个字节 16位二进制 byte[0] byte[1]<br>
**int** : 整数类型 占4个字节 32位二进制 byte[0] byte[1] byte[2] byte[3]<br>
**long**: 长整数类型 占8个字节 64位二进制 byte[0] byte[1] byte[2] byte[3] byte[4] byte[5] byte[6] byte[7]<br>
**float**: 浮点数(小数) 占4个字节 32位二进制 byte[0] byte[1] byte[2] byte[3]<br>
**double**: 双精度浮点数(小数) 占8个字节 64位二进制 byte[0] byte[1] byte[2] byte[3] byte[4] byte[5] byte[6] byte[7]<br>
###int转byte[ ]###
针对声明变量 int i = 3,j = 8;int占4个字节，i = 3 在Java二进制表示：<br/>                 00000000 00000000 00000000 00000011   <br/>j = 8 在Java二进制表示：<br/>00000000 00000000 00000000 00001000<br/>下面进行运算：<br/>1、按位与：每一位进行按位与运算，规则是 1&1 = 1；1&0 = 0；0&1 = 0；0&0 = 0；所以i & j = 0 <br/>2、右位移或者左位移i>>2 = 0向右位移两位，右边使用0补位，变成：<br/>00000000 00000000 00000000 00000000<br/>i<<2 = 12向左位移两位，左边使用0补位，变成:<br/>00000000 00000000 00000000 00001100

    public static  byte[] int_byte(int id){

         byte[] arr=new byte[4]; 
         arr[0]=(byte)((id>>0*8)&0xff);
         arr[1]=(byte)((id>>1*8)&0xff);
         arr[2]=(byte)((id>>2*8)&0xff);
         arr[3]=(byte)((id>>3*8)&0xff);
         return arr;
     }
###byte[ ]转int###
    public static int byte_int(byte[] arr){
        
        int i0=(int)((arr[0]&0xff)<<0*8);
        int i1=(int)((arr[1]&0xff)<<1*8);
        int i2=(int)((arr[2]&0xff)<<2*8);
        int i3=(int)((arr[3]&0xff)<<3*8);     
        return i0+i1+i2+i3;
     }

###short、long、char与int类似###
**注意**byte转long时一定要强转 不然会造成最高32位精度丢失<br/>

    long l = ((long) b[0] << 56) & 0xFF00000000000000L;

###double与float###
    /**
	 * 将一个双精度浮点数转换位字节数组（8个字节），b[0]存储高位字符，大端
	 * 
	 * @param d 双精度浮点数
	 * @return 代表双精度浮点数的字节数组
	 */
	public static byte[] doubleToBytes(double d) {
		return longToBytes(Double.doubleToLongBits(d));
	}
 
	/**
	 * 将一个浮点数转换为字节数组（4个字节），b[0]存储高位字符，大端
	 * 
	 * @param f 浮点数
	 * @return 代表浮点数的字节数组
	 */
	public static byte[] floatToBytes(float f) {
		return intToBytes(Float.floatToIntBits(f));
	}/**
	 * 将一个8位字节数组转换为双精度浮点数
	 * 注意，函数中不会对字节数组长度进行判断，请自行保证传入参数的正确性。
	 * 
	 * @param b 字节数组
	 * @return 双精度浮点数
	 */
	public static double bytesToDouble(byte[] b) {
		return Double.longBitsToDouble(bytesToLong(b));
	}
 
	/**
	 * 将一个4位字节数组转换为浮点数
	 * 注意，函数中不会对字节数组长度进行判断，请自行保证传入参数的正确性。
	 * 
	 * @param b 字节数组
	 * @return 浮点数
	 */
	public static float bytesToFloat(byte[] b) {
		return Float.intBitsToFloat(bytesToInt(b));
	}

##java中byte转换int时为何与0xff进行与运算##
在剖析该问题前请看如下代码

     public static String bytes2HexString(byte[] b) {
    String ret = "";
    for (int i = 0; i < b.length; i++) {
    String hex = Integer.toHexString(b[i] & 0xFF);
    if (hex.length() == 1) {
    hex = '0' + hex;
    }
    ret += hex.toUpperCase();
     }
    return ret;
    }

上面是将byte[]转化十六进制的字符串,注意这里b[i] & 0xFF将一个byte和 0xFF进行了与运算,然后使用Integer.toHexString取得了十六进制字符串,可以看出
b[i] & 0xFF运算后得出的仍然是个int,那么为何要和 0xFF进行与运算呢?直接 Integer.toHexString(b[i]);,将byte强转为int不行吗?答案是不行的.

其原因在于:
1.byte的大小为8bits而int的大小为32bits
2.java的二进制采用的是补码形式

在这里先温习下计算机基础理论

byte是一个字节保存的，有8个位，即8个0、1。
8位的第一个位是符号位， 
也就是说0000 0001代表的是数字1 
1000 0000代表的就是-1 
所以正数最大位0111 1111，也就是数字127 
负数最大为1111 1111，也就是数字-128

上面说的是二进制原码，但是在java中采用的是补码的形式，下面介绍下什么是补码

1、反码：

        一个数如果是正，则它的反码与原码相同；
        一个数如果是负，则符号位为1，其余各位是对原码取反；

2、补码：

       利用溢出，我们可以将减法变成加法
       对于十进制数，从9得到5可用减法：
       9－4＝5    因为4+6＝10，我们可以将6作为4的补数
       改写为加法：
       9+6＝15（去掉高位1，也就是减10）得到5.

       对于十六进制数，从c到5可用减法：
       c－7＝5    因为7+9＝16 将9作为7的补数
       改写为加法：
       c+9＝15（去掉高位1，也就是减16）得到5.

    在计算机中，如果我们用1个字节表示一个数，一个字节有8位，超过8位就进1，在内存中情况为（100000000），进位1被丢弃。

    ⑴一个数为正，则它的原码、反码、补码相同
    ⑵一个数为负，刚符号位为1，其余各位是对原码取反，然后整个数加1
    
 - 1的原码为                10000001
 - 1的反码为                11111110+ 1
 - 1的补码为                11111111
 
 0的原码为                 00000000
 0的反码为                 11111111（正零和负零的反码相同）
                                          +1
 0的补码为               100000000（舍掉打头的1，正零和负零的补码相同）

Integer.toHexString的参数是int，如果不进行&0xff，那么当一个byte会转换成int时，由于int是32位，而byte只有8位这时会进行补位，
例如补码11111111的十进制数为-1转换为int时变为11111111111111111111111111111111好多1啊！即0xffffffff但是这个数是不对的，这种补位就会造成误差。
和0xff相与后，高24比特就会被清0了，结果就对了。

 

 

需要注意几点：

1.计算机中数据按照补码存储

2.byte是8位，int是32位，byte转换为int后是32位，如果不和0xff进行与运算，

例如：byte=-1 那么转为int的补码就是11111111 11111111 11111111 11111111，toHexString()后就是ff ff ff ff 跟原来的相比较多了三个ff。

byte为负数，高3字节就填充1，整数就补0，所以，如果byte是正数那么是否进行&0xff结果都一样；如果是负数就一定需要&0xff。

###补充###
> java 高低位字节，以及转换


**字节序**

1、Big-Endian（大端模式）

Big-Endian就是高位字节排放在内存的低地址端，低位字节排放在内存的高地址端。 

2、Little-Endian （小端模式）

Little-Endian就是低位字节排放在内存的低地址端，高位字节排放在内存的高地址端。 

**大小端模式**：

在操作系统中，x86和一般的OS（如windows，FreeBSD,Linux）使用的是小端模式。但比如Mac OS是大端模式。

1、不同端模式的处理器进行数据传递时必须要考虑端模式的不同(因主机CPU不同，不同的机器会出现高、低两种字节序问题)。

2、在网络上传输数据时，由于数据传输的两端对应不同的硬件平台，采用的存储字节顺序可能不一致。所以在TCP/IP协议规定了在网络上必须采用网络字节顺序，也就是大端模式。对于char型数据只占一个字节，无所谓大端和小端。而对于非char类型数据，必须在数据发送到网络上之前将其转换成大端模式。接收网络数据时按符合接受主机的环境接收。

存储量大于1字节，非char类型，如int，float等，要考虑字节的顺序问题了。

java由于虚拟机的关系,屏蔽了大小端问题。

 

1. 网络字节序是高字节序序
2. C++是主机字节序（高、低都有可能）
3. JAVA是网络字节序、也就是高字节序

 

java端，大小端转换：

其它多余1字节类型可以参考变形写出即可

    public static void shortToByte_LH(short shortVal, byte[] b, int offset) {    
    b[0 + offset] = (byte) (shortVal & 0xff);    
    b[1 + offset] = (byte) (shortVal >> 8 & 0xff);    
    }  
  
    public static short byteToShort_HL(byte[] b, int offset)  
    {  
    short result;  
    result = (short)((((b[offset + 1]) << 8) & 0xff00 | b[offset] & 0x00ff));  
    return result;  
    }  
  
    public static void intToByte_LH(int intVal, byte[] b, int offset) {    
    b[0 + offset] = (byte) (intVal & 0xff);    
    b[1 + offset] = (byte) (intVal >> 8 & 0xff);    
    b[2 + offset] = (byte) (intVal >> 16 & 0xff);    
    b[3 + offset] = (byte) (intVal >> 24 & 0xff);    
    }     
  
    public static int byteToInt_HL(byte[] b, int offset)  
    {  
    int result;  
    result = (((b[3 + offset] & 0x00ff) << 24) & 0xff000000)  
            | (((b[2 + offset] & 0x00ff) << 16) & 0x00ff0000)  
            | (((b[1 + offset] & 0x00ff) << 8) & 0x0000ff00)  
            | ((b[0 + offset] & 0x00ff));  
    return result;  
    }   

 

int 的高低位：

(1) 获取高字节，低字节。

    // 分别取出int的高字节跟低字节
    int value = 515；
    int big = (value & 0xFF00) >> 8;
    int little = value & 0xFF;
（2）根据高字节，低字节，为int指定。

    // 为int设置指定的高字节跟低字节
    int value；
    int a;  int b;
    value= (a<< 8) | (b & 0xFF);


##Java实现uint8_t/uint16_t/uint32_t等##
在Java中，整数可以用byte,short,int和long等类型来表示，并不支持unsigned类型。然而在很多情况下Java也需要处理无符号类型，如翻译C/C++代码，与C/C++进行通讯等，这时就需要用Java来实现uint8_t,uint16_t,uint32_t等类型。
Java实现unsigned类型一般的思路为用更大的存储空间来保存无符号类型，以确保unsigned 最高位不会被Java整型中的符号位所混淆

    public short getUint8(short s){
    return (short) (s & 0x00ff);
    }

    public int getUint16(int i){
    return i & 0x0000ffff;
    }

    public long getUint32(long l){
    return l & 0x00000000ffffffff;
    }
uint64 类型数据太长 在Java里只能转换成字符串

    public String toUint64String(long longValue) {
    final String binaryString = Long.toBinaryString(longValue);
    final UnsignedLong unsignedLong = UnsignedLong.valueOf(binaryString, 2);
    return unsignedLong.toString();
    }
## 特殊的"基本"类型 String ##
String 类代表字符串。Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。<br/> 
字符串是常量；它们的值在创建之后不能更改。<br/>字符串缓冲区支持可变的字符串。因为 String 对象是不可变的，所以可以共享。<br/>
String底层使用一个字符数组来维护的。<br/>成员变量可以知道String类的值是final类型的，不能被改变的，所以只要一个值改变就会生成一个新的String类型对象，存储String数据也不一定从数组的第0个元素开始的，而是从offset所指的元素开始
###创建字符串对象两种方式的区别###
####直接赋值方式创建对象####
直接赋值方式创建对象是在方法区的常量池

    String str="hello";//直接赋值的方式

####通过构造方法创建字符串对象####
通过构造方法创建字符串对象是在堆内存

    String str=new String("hello");//实例化的方式
**总结：两种实例化方式的区别**<br/>


1. 直接赋值（String str = "hello"）：只开辟一块堆内存空间，并且会自动入池，不会产生垃圾。

2. 构造方法（String str=  new String("hello");）:会开辟两块堆内存空间，其中一块堆内存会变成垃圾被系统回收，而且不能够自动入池，需要通过public  String intern();方法进行手工入池。在开发的过程中不会采用构造方法进行字符串的实例化。<br/>


**注意:调用equals比较时应避免空指向**<br/>
用常量去调用方法

`if("hello".equals(str)){
　
　　　　　　}`此时equals会处理null值，可以避免空指向异常
　　　
###byte[ ] 和 String互相转换###
    public static void main(String[] args) 
    {
        //Original String
        String string = "hello world";
         
        //Convert to byte[]
        byte[] bytes = string.getBytes();
         
        //Convert back to String
        String s = new String(bytes);
         
        //Check converted string against original String
        System.out.println("Decoded String : " + s);
    }

###16进制与字符串之间的相互转换###

    /**
    * 字符串转换成为16进制(无需Unicode编码)
    * @param str
    * @return
    */
    public static String str2HexStr(String str) {
    char[] chars = "0123456789ABCDEF".toCharArray();
    StringBuilder sb = new StringBuilder("");
    byte[] bs = str.getBytes();
    int bit;
    for (int i = 0; i < bs.length; i++) {
        bit = (bs[i] & 0x0f0) >> 4;
        sb.append(chars[bit]);
        bit = bs[i] & 0x0f;
        sb.append(chars[bit]);
        // sb.append(' ');
    }
    return sb.toString().trim();
    }

    /**
    * 16进制直接转换成为字符串(无需Unicode解码)
    * @param hexStr
    * @return
    */
    public static String hexStr2Str(String hexStr) {
    String str = "0123456789ABCDEF";
    char[] hexs = hexStr.toCharArray();
    byte[] bytes = new byte[hexStr.length() / 2];
    int n;
    for (int i = 0; i < bytes.length; i++) {
        n = str.indexOf(hexs[2 * i]) * 16;
        n += str.indexOf(hexs[2 * i + 1]);
        bytes[i] = (byte) (n & 0xff);
    }
    return new String(bytes);
    }

##**String字符串编码解码格式**##
String字符串编码解码格式

    String.getBytes()//方法是得到一个操作系统默认的编码格式的字节数组。
    String.getBytes(String decode)//方法会根据指定的decode编码返回某字符串在该编码下的byte数组表示
    new String(byte[] b,String decode)//按照指定的方法编码
正常的编码解码

    byte[] b_gbk = "中".getBytes("GBK"); 
	String s_gbk = new String(b_gbk,"GBK"); 
	System.out.println(s_gbk);//中

编码解码不统一导致乱码

    byte[] b_gbk = "中".getBytes("GBK"); 
	String s_utf8 = new String(b_gbk,"UTF-8"); 
	System.out.println(s_gbk);//��  导致乱码


**进一步理解：**

        byte[] b_gbk = "中".getBytes("GBK"); 
		byte[] b_utf8 = "中".getBytes("UTF-8"); 
		byte[] b_iso88591 = "中".getBytes("ISO8859-1"); 
		System.out.println(b_utf8.length);//3
		System.out.println(b_gbk.length); //2
		System.out.println(b_iso88591.length);//1

解码时字节数组：
将分别返回“中”这个汉字在GBK、UTF-8和ISO8859-1编码下的byte数组表示，此时b_gbk的长度为2，b_utf8的长度为3，b_iso88591的长度为1。

编码后：
而与getBytes相对的，可以通过new String(byte[], decode)的方式来还原这个“中”字时，这个new String(byte[], decode)实际是使用decode指定的编码来将byte[]解析成字符串。

    	String s_gbk = new String(b_gbk,"GBK"); 
		String s_utf8 = new String(b_utf8,"UTF-8"); 
		String s_iso88591 = new String(b_iso88591,"ISO8859-1"); 
		System.out.println(s_gbk);//中
		System.out.println(s_utf8);//中
		System.out.println(s_iso88591);//?

原因：通过打印s_gbk、s_utf8和s_iso88591，会发现，s_gbk和s_utf8都是“中”，而只有s_iso88591是一个不认识的字符，为什么使用ISO8859-1编码再组合之后，无法还原“中”字呢，其实原因很简单，因为ISO8859-1编码的编码表中，根本就没有包含汉字字符，当然也就无法通过"中".getBytes(“ISO8859-1”);来得到正确的“中”字在ISO8859-1中的编码值了，所以再通过new String()来还原就无从谈起了。

需要使用ISO8859-1来操作中文则可以通过中转，既然不可以操作中文字符，那么是否可以通过操作别的编码格式转换后的字节数组，比如utf-8的字节数组来编码成中文，虽然也是乱码但是再通过iso8859-1来还原成utf-8时可还原结果

**String字符串编码解码格式原文来自**：
[https://blog.csdn.net/qq_35241080/article/details/83001149](https://blog.csdn.net/qq_35241080/article/details/83001149 "编码格式") 


----------
2019/3/13 星期三 10:58:38 

##转换工具类##
[https://github.com/UaenaSA/JavaUtils.git](https://github.com/UaenaSA/JavaUtils.git "工具类")
- 

