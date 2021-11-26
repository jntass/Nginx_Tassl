# Nginx_Tassl
 通过tassl支持国密协议的nginx
 注：如使用TASSL-1.1.1k则可使用标准nginx，下载链接https://github.com/jntass/TASSL-1.1.1k
 ## 如何使nginx调用tassl实现国密ssl协议？
具体方法如下：
1.	下载Tassl-1.1.1b_R_0.8.tgz版本进行编译，并安装到/root/lib_r/tassl中，如是其他目录需要在第二步的configure时，替换--with-openssl的目录。
2.	下载江南天安修改的nginx-1.16.0_tassl.tgz支持国密的nginx进行编译。

      ./configure --with-http_ssl_module --with-stream --with-stream_ssl_module --with-openssl=/root/lib_r/tassl --prefix=/root/nginx

      make

      make install

3.	配置nginx。

     配置nginx.conf证书部分：

     ssl_certificate      /root/lib_r/tassl/tassl_demo/cert/certs/SS.crt;    #/*签名证书*/

     ssl_certificate_key  /root/lib_r/tassl/tassl_demo/cert/certs/SS.key;     #/*签名私钥*/

     ssl_enc_certificate      /root/lib_r/tassl/tassl_demo/cert/certs/SE.crt;     #/*加密证书*/

     ssl_enc_certificate_key  /root/lib_r/tassl/tassl_demo/cert/certs/SE.key;     #/*加密私钥*/

      注意：签名证书的证书用途需有数据签名功能，加密证书的证书用途需有数据加密功能。如果功能不正确，会导致握手失败。调用tassl_demo/cert/中的脚本生成的证书已经具备相应功能,可以用来测试。
4.	下载密信浏览器或者360浏览器进行测试国密网站。

      a)	密信浏览器

      下载地址：https://www.mesince.com/zh-cn/browser

      如果第一次连接失败，那么密信浏览器会尝试国际算法，导致以后的连接都会使用国际算法，无法成功。此时需要在浏览器设置中，清楚浏览数据，进行清楚所有的数据，那么浏览器在下一次再进行连接时，会首先尝试国密算法。

      b)	360浏览器

      下载地址：https://browser.360.cn/se/ver/gmzb.html

      360国密浏览器版本目前较多。而且和新版本的Windows 10系统，会偶发性的导致系统重启。
      而且360要求自己添加根证书，如果没有添加会导致SSL握手完成后显示证书不正确。
      添加跟证书方法：将根证书放在ctl.dat文件中，然后把此文件放在%appdata%/360se6\Application\User Data\Default\ctl目录中，
      注意：上面从"User Data\Default\ctl"的目录并不存在，需要手工创建。
      经过多次测试，目前可以添加根证书生效的版本360_mini_installer_sm_7.exe，已经附件到另一个仓库中https://github.com/jntass/GM_BROWERS
      而且我们的测试环境是Windows 7系统。

      更多信息参考：https://bbs.360.cn/forum.php?mod=viewthread&tid=15660975

