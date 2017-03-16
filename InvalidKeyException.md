# 解决java.security.InvalidKeyException  
今天在使用javax.crypto.Cipher解码一段密码时遇到个异常：  
java.security.InvalidKeyException: Illegal key size or default parameters  
查了下，发现是以下原因
> 因为美国的出口限制，Sun通过权限文件（local_policy.jar、US_export_policy.jar）做了相应限制。
> 因此存在一些问题：
>> * 密钥长度上不能满足需求（如：java.security.InvalidKeyException: Illegal key size or default parameters）；  
>> * 部分算法未能支持，如MD4、SHA-224等算法；  
>> * API使用起来还不是很方便；一些常用的进制转换辅助工具未能提供，如Base64编码转换、十六进制编码转换等工具。  

Oracle在其官方网站上提供了无政策限制权限文件（Unlimited Strength Jurisdiction Policy Files），我们只需要将其部署在JRE环境中，就可以解决限制问题。  
解压缩后覆盖%JDK_Home%\jre\lib\security目录下的local_policy.jar、US_export_policy.jar 文件就可以了。  

