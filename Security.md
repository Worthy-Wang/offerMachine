## 目录

- [信息安全的四个特性是什么？](#信息安全的四个特性是什么)
- [简述有哪些加密算法？](#简述有哪些加密算法)
- [HTTPS协议是什么?为什么要使用HTTPS？](#https协议是什么为什么要使用https)

<br>

----------------
## 信息安全的四个特性是什么？
**机密性**：防止信息被窃听，对应的技术有对称加密算法和非对称加密算法。

**完整性**：防止信息被篡改，对应的技术有散列算法，数字签名。

**身份认证性**：防止黑客伪装成发送者，对应的技术有数字签名。

**不可否认性**：防止发送者事后否认自己发送过，对应的技术有数字签名。

<br>

<div align="right">
    <b><a href="#This-is-my-notebook">↥ Back To Top</a></b>
</div>


--------------
## 简述有哪些加密算法？
在如今的信息安全领域，加密算法可以分为以下四类：
* **哈希算法**
* **对称加密算法**
* **非对称加密算法**
* **数字签名算法**


1. **哈希算法**

**哈希算法**也称为**信息摘要算法**（或**散列算法**）。常见的哈希算法有**MD5算法**，**SHA算法**。

*  **MD5算法**
原理是将任何信息（不论大小，格式，数量）经过处理后，用一个定长的散列值表示。

* **SHA算法**
与MD5算法类似，将任何信息处理后以某一个散列值表示。例如**linux系统用户的密码**就是使用SHA-512算法进行加密后放在 `/etc/shadow` 文件中。

<br>


2. **对称加密算法**

对称加密算法也就是**加密和解密采用同一个密钥**。
这种加密方式由于需要传输密钥，所以有很大的安全性问题。常见的对称加密算法有**DES**，**AES**。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110215050950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
<br>

3. **非对称加密算法**

非对称加密算法也就是**加密和解密使用的并不是同一个密钥**。
通常有两个密钥，分别称为**公钥**和**私钥**，公钥可以对外公布，私钥只能持有人自己所拥有不能对外公开。非对称加密算法有效的避免了密钥的传输安全性问题。常见的非对称加密算法有**RSA**。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102150541343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
<br>


4. **数字签名算法**

准确的来说，数字签名算法也属于非对称加密算法中的一种。

* **数字签名**

对于明文信息先进行哈希算法求得信息摘要，后再用自身私钥进行加密后求得数字签名。**数字签名的作用是证明该信息是否为本人所写**。

* **数字证书**

将自身的公钥和一些相关信息，交给`CA（certificate authority 认证机构）`并利用CA的私钥进行加密后得到数字证书。**数字证书的作用是传递公钥**。

* **举例**

发送方A：A对明文进行数字签名，并向CA求得自己的数字证书，将数字证书+数字签名+明文 打包，一同发送给B。

接收方B：B利用CA公钥解开A的数字证书，求得A的公钥；再用A的公钥解开A的数字签名，求得A的明文数字摘要；最后对明文用同样的哈希算法求得数字摘要，并判断两次数字摘要是否相同。

>**注意：CA证书（包含了CA的公钥）一开始就已经存放在了浏览器或者操作系统中，没必要通过网络获取，也就不存在网络劫持的问题。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102152214841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
<br>

<div align="right">
    <b><a href="#This-is-my-notebook">↥ Back To Top</a></b>
</div>


-----------
##  HTTPS协议是什么?为什么要使用HTTPS？
HTTP协议在使用过程中，由于并不满足信息安全的四个特性：**安全性，完整性，身份认证性，不可否认性**。

于是有了HTTPS协议来帮助解决上述问题，HTTPS协议的全称是**HTTP Over SSL**，**SSL协议对浏览器与Web服务器之间的通信进行了加密**，SSL/TLS具体的工作过程如下：

1.	用户向Web服务器发起访问请求；
2.	Web服务器将自己的CA认证的数字证书发给用户；
3.	用户拿到数字证书，并用自己浏览器内置的CA证书解密得到Web服务器的公钥；
4.	用户使用服务器的公钥对于对称加密算法的密钥进行加密，发送给Web服务器；
5.	服务器收到后，用自己的私钥解密，得到对称加密算法的密钥；
6.	双方使用对称加密算法开始通信。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102152738312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)


**总结一下SSL层工作的原理**：
1.	通过CA体系发送公钥（或者说是数字证书）；

2.	通过非对称加密算法确定对称加密的密钥；

3.	通过对称加密算法进行网络通信。

>**补充**：
由于非对称加密算法比对称加密算法复杂很多，同时处理效率也会低很多，所以在实际的SSL/TLS层工作中，非对称加密算法只用于传输对称加密的密钥，真正在通信的过程中使用的是对称加密算法。

<br>

<div align="right">
    <b><a href="#This-is-my-notebook">↥ Back To Top</a></b>
</div>


---------
