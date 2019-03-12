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


- 

