# 做苹果推送服务器，很重要的一步，就是生成与苹果APNS连接的证书，一般是`.pem`文件

## 测试

1. 首先在苹果开发者中心 生成 `aps_devlopment.cer` 文件；然后下载；双击导入钥匙串；
2. 打开钥匙串选择--登录--证书--找到 __Apple Development iOS Push Server__ 证书--右键--导出--生成 apns-dev-cert.p12 文件 不要设置密码
3. 然后选择--密钥--找到User下面的 __Apple Development iOS Push Server__ 密钥--右键--导出--生成 apns-dev-key.p12 文件 不要设置密码
4. 打开终端，把上面的 p12 文件生成 .pem 文件
5. `openssl pkcs12 -clcerts -nokeys -out apns-dev-cert.pem -in apns-dev-cert.p12` 生成 apns-dev-cert.pem
6. `openssl pkcs12 -nocerts -out apns-dev-key.pem -in apns-dev-key.p12` 生成 apns-dev-key.pem 这个要输入密码，记住输入的密码；
7. `openssl rsa -in apns-dev-key.pem -out apns-dev-key-noenc.pem` 生成 apns-dev-key-noenc.pem 因为上面的 apns-dev-key.pem 有密码，这一步生成的就是把密码取消的文件；
9. `cat apns-dev-cert.pem apns-dev-key-noenc.pem > apns-dev.pem` 合并 apns-dev-cert.pem apns-dev-key-nonec.pem 把这两个文件合成 apns-dev-cert.pem;
10. 最后在服务器端使用 apns-dev-cert.pem 就可以了；
11. `openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert apns-dev-cert.pem -key apns-dev-key-noenc.pem` 测试证书是否正常使用 该命令执行结事时：可以直接输入任意字符串，回车 出现 closed；这表示成功可用；否之 打印错误信息；

## 正式

1. 首先在苹果开发者中心 生成 `aps.cer` 文件；然后下载；双击导入钥匙串；
2. 打开钥匙串选择--登录--证书--找到 __Apple Push Server__ 证书--右键--导出--生成 apns-cert.p12 文件 不要设置密码
3. 然后选择--密钥--找到User下面的 __Apple Push Server__ 密钥--右键--导出--生成 apns-key.p12 文件 不要设置密码
4. 打开终端，把上面的 p12 文件生成 .pem 文件
5. `openssl pkcs12 -clcerts -nokeys -out apns-cert.pem -in apns-cert.p12` 生成 apns-cert.pem
6. `openssl pkcs12 -nocerts -out apns-key.pem -in apns-key.p12` 生成 apns-key.pem 这个要输入密码，记住输入的密码；
7. `openssl rsa -in apns-key.pem -out apns-key-noenc.pem` 生成 apns-key-noenc.pem 因为上面的 apns-key.pem 有密码，这一步生成的就是把密码取消的文件；
9. `cat apns-cert.pem apns-key-noenc.pem > apns.pem` 合并 apns-cert.pem apns-key-nonec.pem 把这两个文件合成 apns-cert.pem;
10. 最后在服务器端使用 apns-cert.pem 就可以了；
11. `openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert apns-cert.pem -key apns-key-noenc.pem` 测试证书是否正常使用 该命令执行结事时：可以直接输入任意字符串，回车 出现 closed；这表示成功可用；否之 打印错误信息；
