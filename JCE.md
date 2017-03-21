# Java Cryptography Extension (JCE)

JDK自带的加密工具

JDK自带的JCE有限制, 还需要从oracle下载额外的权限文件覆盖本地jdk目录下的权限控制文件  

* 首先得指定一个算法以及key位数，使用KeyGenerator来生成一个SecretKey，然后解析成字节数组，  
最后用BASE64Encoder这个不开源的工具将字节数组转换成字符串保存。  
* 其次需要SecretKeySpec对象，使用之前生成的key和算法名来生成。再根据SecretKeySpec对象初始化Cipher对象，指定Cipher为加密功能。  
用cipher.doFinal来加密字符串
*　解密和加密类似，指定Cipher为解密功能，然后用cipher.doFinal来解密字符串


``` java

  public String genKeyString(String algorithm, int bits) throws Exception {
		
		KeyGenerator keyGen = KeyGenerator.getInstance(algorithm);
		keyGen.init(bits); 
		
		SecretKey securekey = keyGen.generateKey();
		byte[] raws = securekey.getEncoded();
		
		return (new BASE64Encoder()).encode(raws);	
	}
  
  public String encrypt(String str, String key, String algorithm) throws Exception {

      Cipher cipher = Cipher.getInstance(algorithm);
      SecretKeySpec spec = new SecretKeySpec((new BASE64Decoder()).decodeBuffer(key), algorithm);

      cipher.init(Cipher.ENCRYPT_MODE, spec);

      return (new BASE64Encoder()).encode(cipher.doFinal((new BASE64Decoder()).decodeBuffer(str)); 	
	}
  
  public String ｄｅcrypt(String str, String key, String algorithm) throws Exception {

      Cipher cipher = Cipher.getInstance(algorithm);
      SecretKeySpec spec = new SecretKeySpec((new BASE64Decoder()).decodeBuffer(key), algorithm);

      cipher.init(Cipher.DECRYPT_MODE, spec);

      return (new BASE64Encoder()).encode(cipher.doFinal((new BASE64Decoder()).decodeBuffer(str)); 	
	}
```
