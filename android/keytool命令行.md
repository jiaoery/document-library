# 1.什么是JavaKeytool

Keytool 是Java提供的密钥（Key）和证书（Certificate）管理工具，用于管理公钥/私钥对以及相关证书。

该工具为Java自带的工具，只要Java环境存在，就可以正常使用，位于JAVA_HOME(不明白可以去看Java环境变量配置)

# 2.密钥的存储形式

keytool将密钥和证书存储在keystore 或 jks文件下，该文件包含：

* 密钥：如果是非对称加密，则包含密钥和配对公钥；对称加密只包含密钥。
* 可信任的证书实体，只包含公钥；

ps：密钥存储在密钥库中，一个密钥库能够存储多个密钥对，每个密钥都有一个别名。



# 3.keytool命令选项

* 全部命令行集合

```bash
命令:

 -certreq            生成证书请求
 -changealias        更改条目的别名
 -delete             删除条目
 -exportcert         导出证书
 -genkeypair         生成密钥对
 -genseckey          生成密钥
 -gencert            根据证书请求生成证书
 -importcert         导入证书或证书链
 -importpass         导入口令
 -importkeystore     从其他密钥库导入一个或所有条目
 -keypasswd          更改条目的密钥口令
 -list               列出密钥库中的条目
 -printcert          打印证书内容
 -printcertreq       打印证书请求的内容
 -printcrl           打印 CRL 文件的内容
 -storepasswd        更改密钥库的存储口令
```

## 2.1 常用命令行

### 2.1.1 生成公私密钥对

```bash
keytool -genkeypair -alias [别名] -keyalg 【密钥算法一般为RSA】 -keystore [生成文件名]

C:\Users\Jiaoery>keytool -genkeypair -alias myapp -keyalg RSA -keystore myapp.keystore
输入密钥库口令:
再次输入新口令:
您的名字与姓氏是什么?
  [Unknown]:  jiaoery
您的组织单位名称是什么?
  [Unknown]:  jiaoery
您的组织名称是什么?
  [Unknown]:  jiaoery
您所在的城市或区域名称是什么?
  [Unknown]:  chongqing
您所在的省/市/自治区名称是什么?
  [Unknown]:  chongqing
该单位的双字母国家/地区代码是什么?
  [Unknown]:  cn
CN=jiaoery, OU=jiaoery, O=jiaoery, L=chongqing, ST=chongqing, C=cn是否正确?
  [否]:
```

**ps：以上命令会要求输入密码等信息， 证书库密码默认是 `changeit`**

### 2.1.2 将证书导入JDK的证书信任库

需要分为两步完成：

1.导出.cer后缀的签名文件

```bash
keytool -export -trustcacerts -alias [别名] -file [导出文件名.cer后缀] -keystore [需要被导入的文件，jks或keystore后缀] -storepass [storepass 密码]
```

2.导入到Java的证书信任集中

```bash
keytool -import -trustcacerts -alias [别名] -file [导入文件名.cer后缀] -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass [storepass 密码]
```

### 2.1.3 查看当前机器的java证书库中的已有证书

```BASH
keytool -list -v -keystore "C:\Program\Files\Java\jre1.8.0_131\lib\security\cacerts"
```

- `-v` 详细列出

**PS:这里需要输入证书管理密码， 默认是 `changeit`**

### 2.1.4 删除某个证书

```bash
keytool -delete -alias [需要删掉的证书别名] -keystore "C:\Program Files\Java\jre1.8.0_131\lib\security\cacerts"  -storepass [证书密码]
```

### 2.1.5 修改证书密码

```bash
keytool -keypasswd -alias [别名] -keystore [证书库名称] -storepass [证书库密码] -keypass【证书原密码】 -new [证书新密码]
```

### 2.1.6 修改证书密码

```bash
keytool -storepasswd -v -new [证书新密码] -keystore [证书库名称] -storepass [证书库原密码] -storetype [密码库类型，一般为JKS]
```

## 2.2 系统签名打包

这个是在Android系统应用中最需要的签名文件，必须使用`platform.x509.pem `、`platform.pk8`打包成Android 可用的jks or keystore文件

但是这个命令必须在linux（Mac也是可以的）下才可以

```bash
./keytool-importkeypair -k ./[目标文件，以jks或keystore为后缀] -p [密码] -pk8 platform.pk8 -cert platform.x509.pem -alias [别名]
```

