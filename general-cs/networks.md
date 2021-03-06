

## HTTPS

### RSA
非对称加密，产生一对公钥和私钥，可以用于加密和签名。

加密：公钥加密，私钥解密；签名：私钥加密，公钥解密（验证）。

### CA根证书和中间证书
CA (Certificate Authority) 是证书授权中心，负责发放和管理证书。
CA有自己的公钥和私钥(ca.pub, ca.key)。

用户可以用如下方法获得证书：
1. 用户首先产生自己的公钥和私钥(标记为 site.pub, site.key)
2. 将网站公钥(site.pub)和个人身份信息发送给CA
3. CA严格地核实身份后，将给用户发送一个数字证书。该证书是用 CA 的私钥 (ca.key) 对用户的公钥 (site.pub) 等信息进行签名。
4. 用户可以利用获得的证书部署https服务。

事实上，顶层的CA (Root CA) 是不会直接向用户发放证书的，而是多了一层中间证书颁发机构 (Intermediate CA)。Root CA 用自己的私钥给自己签发了一个证书，称为根证书。中间证书颁发机构可以用上面的方法从 Root CA 得到一个中间证书。用户又用类似的方法从中间证书颁发机构获得用户证书。至于为什么需要多这么一层中间证书颁发机构，目的是为了保护根证书，减少根证书被攻击或者说被破解的风险。

中间证书的层数不一定是一层，层数越多越容易起到保护根证书的作用。但不宜过多，不然验证起来很耗资源。

### 证书验证过程

1. 浏览器（客户端）首先把自己支持的SSL/TSL版本发给服务器。
2. 服务器把自己的公钥和证书发给浏览器，浏览器根据CA机构的信息，找到对应的公钥进行验证。如果验证通过，浏览器会进一步验证中间证书带有的上级证书是否可信。最后，验证带有的根证书是否可信。
3. 浏览器内置了一些受信任的Root CA，所以可以用对应的公钥来验证这些根证书。一旦通过，说明整个证书链是可信任的。当然，浏览器一般也预置了一些可信的中间CA，如果验证证书链时遇到了可信的中间CA，就提前结束，不一定到达根证书。

注意到，网站服务器在部署证书时，不仅仅是导入自己的用户证书，同时也要带上整个证书链。也就是包括签发用户证书的中间证书，以及可能存在的上级中间证书，一直到根证书。这是因为手机浏览器不一定内置了所有的中间证书，如果只有用户证书，可能会导致验证过程中断。但PC端的浏览器一般内置了很多中间CA。

此外，还得查看证书是否过期，用户是否在证书作废列表里等。

### 验证完成后

1. 验证完合法性后，在证书里取出服务器的公钥。
2. 浏览器生成对称密钥。
3. 使用服务器公钥对该对称密钥加密，发回给服务器。
4. 服务器使用私钥解密，得到对称密钥。
5. 服务器使用该对称密钥加密后续http数据。使用对称密钥加密是因为比非对称加密高效。

### 参考
[证书链-Digital Certificates (May 2016)](https://www.jianshu.com/p/46e48bc517d0)


## Network library

1. [Why Network library (e.g., Axios) is needed?](https://dev.to/fleepgeek/when-do-you-need-axios-d)
   1. Axios can work in Node.js but `fetch()` can only work in browser.
   2. Axios can use `axios.get()`, `axios.post()`, which is more clear API.
   3. Axios can cancel a request.
   4. Axios can show the progress.
   5. Axios supports token-based support to prevent CSRF.
   
