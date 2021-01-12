# OpenResty+ModSecurityパッケージング解説

## はじめに

本文書はModSecurityを有効にしたOpenRestyのパッケージの導入手順を記述しています。
次の項目を説明します。

* 前提条件について
* 制約事項について
* アーカイブの内容について
* アーカイブを用いたインストール手順
* ModSecurity-nginxの動作確認手順
* 付録 A CentOS 7でのパッケージのビルド手順
* 付録 B CentOS 8でのパッケージのビルド手順
* 付録 C CentOS 8(Stream)でのパッケージのビルド手順
* 付録 D ソースアーカイブ取得方法について
* 付録 F CentOS 8(Stream)への移行手順

## 前提条件について

前提となる動作環境は2021/01時点のアップデートを適用した次の環境です。

* CentOS 7
* CentOS 8
* CentOS 8(Stream)

また対象となるOpenRestyおよびModSecurityのバージョンは次の通りです。

* OpenResty 1.19.3.1
* ModSecurity 3.0.4
* ModSecurity-nginx 1.0.1

動作を確認するためのルールセットは次のバージョンを使用します。

* OWASP ModSecurity Core Rule Set 3.3.0

## 制約事項について

* ModSecurityではCURLが無効です。
  これはCURLを有効にするとopenresty-openssl111と依存関係の衝突により
  ModSecurityをビルドできなくなるためです。
  これにより外部サイトへのアクセスが発生するルールの指定ができません。
  (例: @pmFromFile で参照先に https:// を指定するなど)
  回避策は、外部サイトを参照せず、ローカルファイルを参照することです。
* CentOS 7ではlibmodsecurityのバージョンが3.0.2と古いため、
  ModSecurity-nginxの要求するバージョンを満たせません。これは
  ModSecurity 3.0.3以上が必要なためです。
  問題を解決するために、openresty-modsecurityとしてOpenResty向けにパッケージを追加しています。
* OpenRestyが更新された場合、後述する手順によりRPMをビルドしなおす必要があります。

## アーカイブの内容について

* openresty-packaging-20210114.tar.gz
  * https://github.com/clear-code/openresty-packaging/ のソースコードのスナップショット
* rpmbuild.el7.tar.gz
  * CentOS 7向けのビルド済みRPM/SRPM一式
* rpmbuild.el8.tar.gz
  * CentOS 8向けのビルド済みRPM/SRPM一式
* rpmbuild.el8stream.tar.gz
  * CentOS 8(Stream)向けのビルド済みRPM/SRPM一式

## アーカイブを用いたインストール手順

以下ではアーカイブからのインストール手順を説明します。

### CentOS 7の場合

アーカイブを展開し、パッケージをインストールします。

```sh
$ tar xf rpmbuild.el7.tar.gz
$ sudo rpm -ivh rpmbuild/RPMS/x86_64/lmdb-libs-0.9.22-2.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/ssdeep-libs-2.14.1-1.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/yajl-2.0.4-4.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/GeoIP-1.5.0-14.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/geoipupdate-2.5.0-1.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-openssl111-1.1.1i-1.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-pcre-8.44-1.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-zlib-1.2.11-3.el7.centos.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el7.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-1.19.3.1-2.1.el7.x86_64.rpm
```

### CentOS 8の場合

アーカイブを展開し、パッケージをインストールします。

```sh
$ tar xf rpmbuild.el8.tar.gz
$ sudo rpm -ivh rpmbuild/RPMS/x86_64/lmdb-libs-0.9.24-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/ssdeep-libs-2.14.1-7.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/yajl-2.1.0-10.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/GeoIP-1.6.12-7.el8.x86_64.rpm \
  rpmbuild/RPMS/noarch/GeoIP-GeoLite-data-2018.06-5.el8.noarch.rpm \
  rpmbuild/RPMS/x86_64/openresty-openssl111-1.1.1i-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-pcre-8.44-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-zlib-1.2.11-3.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-1.19.3.1-2.1.el8.x86_64.rpm
```

### CentOS 8(Stream)の場合

アーカイブを展開し、パッケージをインストールします。

```sh
$ tar xf rpmbuild.el8stream.tar.gz
$ sudo rpm -ivh rpmbuild/RPMS/x86_64/GeoIP-1.6.12-7.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/ssdeep-libs-2.14.1-7.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/yajl-2.1.0-10.el8.x86_64.rpm \
  rpmbuild/RPMS/noarch/GeoIP-GeoLite-data-2018.06-5.el8.noarch.rpm \
  rpmbuild/RPMS/x86_64/openresty-openssl111-1.1.1i-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-pcre-8.44-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-zlib-1.2.11-3.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el8.x86_64.rpm \
  rpmbuild/RPMS/x86_64/openresty-1.19.3.1-2.1.el8.x86_64.rpm
```

後述するModSecurity-nginxの動作確認手順を実施し、403エラーが発生すれば期待通りにインストールできています。

## ModSecurity-nginxの動作確認手順

以下ではパッケージをインストール後に、設定ファイルを配置して動作確認する手順を示します。

### 設定ファイルを用意する

アーカイブ展開以降の手順はCentOS 7/CentOS 8/CentOS 8(Stream)で共通です。

* 次のコマンドを実行し、アーカイブに含まれるサンプルのnginx.confをコピーする

```sh
$ sudo cp ~/rpmbuild/SOURCES/nginx.conf.example /usr/local/openresty/nginx/conf/nginx.conf
```

これは、/usr/local/openresty/nginx/conf/nginx.confに以下の項目を追加するのと同じです。

```
load_module "modules/ngx_http_modsecurity_module.so";

modsecurity on;
modsecurity_rules_file /usr/local/openresty/nginx/conf/nginx.modsecurity.conf;
```

* 次のコマンドを実行し、サンプルのnginx.modsecurity.confをコピーする

```sh
$ sudo cp ~/rpmbuild/SOURCES/nginx.modsecurity.conf.example /usr/local/openresty/nginx/conf/nginx.modsecurity.conf
```

これは、/usr/local/openresty/nginx/conf/nginx.modsecurity.confに以下の項目を設定するのと同じです。

```
SecRuleEngine On
Include owasp-modsecurity-crs/crs-setup.conf
Include owasp-modsecurity-crs/rules/REQUEST-901-INITIALIZATION.conf
Include owasp-modsecurity-crs/rules/REQUEST-942-APPLICATION-ATTACK-SQLI.conf
Include owasp-modsecurity-crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
```

Core Rule Setを次の手順でインストールします。

```sh
$ tar xf ~/rpmbuild/SOURCES/v3.3.0.tar.gz
$ sudo cp -r coreruleset-3.3.0 /usr/local/openresty/nginx/conf
$ sudo ln -sf /usr/local/openresty/nginx/conf/coreruleset-3.3.0 /usr/local/openresty/nginx/conf/owasp-modsecurity-crs
$ sudo cp /usr/local/openresty/nginx/conf/owasp-modsecurity-crs/crs-setup.conf.example \
  /usr/local/openresty/nginx/conf/owasp-modsecurity-crs/crs-setup.conf
```

ここまでできたらopenrestyを起動します

```sh
$ sudo systemctl start openresty
```

/usr/local/openresty/nginx/logs/error.logに以下のようなログが出力されたらModSecurityモジュールとルールが読み込まれています。

```
2021/01/05 06:10:02 [notice] 11695#11695: ModSecurity-nginx v1.0.1 (rules loaded inline/local/remote: 0/132/0)
```

以下のいずれかの手段を用いてルールが適用されることを確認します。

* ブラウザから http://localhost/?union+select にアクセスする
* curlで次のコマンドを実行する

```sh
$ curl http://localhost/?union+select
```

403エラーでアクセスできなければModSecurityのルールに従って期待通りに動作(アクセスをブロック)しています。

## 付録A CentOS 7でのパッケージのビルド手順

パッケージを新規にビルドしなおす際に必要な手順を説明します。

* 依存しているパッケージのインストールをする
* openresty-modsecurity develをビルドする
* OpenRestyをModSecurityを有効にしてビルドする

#### 依存しているパッケージのインストールをする

次の手順でRPMのビルドに必要なパッケージをインストールします。

```sh
$ sudo yum install -y wget epel-release
$ wget https://openresty.org/package/centos/openresty.repo
$ sudo mv openresty.repo /etc/yum.repos.d/
$ sudo yum update -y
$ sudo yum install -y git rpm-build redhat-rpm-config rpmdevtools
$ sudo yum install -y pcre-devel gcc make perl \
    perl-Data-Dumper libtool ElectricFence systemtap-sdt-devel valgrind-devel ccache
$ sudo yum install -y openresty-zlib-devel openresty-openssl111-devel openresty-pcre-devel
```

#### openresty-modsecurity develをビルドする

```sh
$ tar xf rpmbuild.el7.tar.gz
$ sudo yum install -y libxml2-devel GeoIP-devel lmdb-devel libcurl-devel gcc-c++ flex bison yajl-devel ssdeep-devel
$ QA_RPATHS=0x0002 rpmbuild -ba ~/rpmbuild/SPECS/openresty-modsecurity.spec
$ sudo rpm -ivh ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el7.x86_64.rpm \
  ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-devel-3.0.4-1.el7.x86_64.rpm
```

### OpenRestyをModSecurityを有効にしてビルドする

```sh
$ rpmbuild -ba ~/rpmbuild/SPECS/openresty.spec
```


上記によりRPMパッケージがビルドされます。

## 付録B CentOS 8でのパッケージのビルド手順

付録A CentOS 7でのパッケージのビルド手順とほぼ同様の手順でビルドできます。

### 依存しているパッケージのインストールをする

次の手順でRPMのビルドに必要なパッケージをインストールします。

```
$ sudo dnf install -y wget epel-release
$ sudo dnf config-manager --set-enabled powertools
$ sudo dnf install -y git rpm-build redhat-rpm-config rpmdevtools
$ sudo dnf install -y pcre-devel gcc make perl perl-Data-Dumper libtool systemtap-sdt-devel valgrind-devel ccache
$ wget https://openresty.org/package/centos/openresty.repo
$ sudo mv openresty.repo /etc/yum.repos.d/
$ sudo dnf update
$ sudo dnf install -y openresty-zlib-devel openresty-openssl111-devel openresty-pcre-devel
```

### openresty-modsecurity-develをビルドする

```
$ tar xf rpmbuild.el8.tar.gz
$ sudo yum install -y libxml2-devel GeoIP-devel lmdb-devel gcc-c++ flex bison yajl-devel ssdeep-devel
$ QA_RPATHS=0x0002 rpmbuild -ba ~/rpmbuild/SPECS/openresty-modsecurity.spec
$ sudo rpm -ivh ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el8.x86_64.rpm \
  ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-devel-3.0.4-1.el8.x86_64.rpm
```

### ModSecurityを有効にしてOpenRestyをビルドする

```
$ rpmbuild -ba ~/rpmbuild/SPECS/openresty.spec
```

上記によりRPMパッケージがビルドされます。

## 付録C CentOS 8(Stream)でのパッケージのビルド手順

付録B CentOS 8でのパッケージのビルド手順とほぼ同様の手順でビルドできます。

### 依存しているパッケージのインストールをする

次の手順でRPMのビルドに必要なパッケージをインストールします。

次にRPMのビルドに必要なパッケージをインストールします。

```
$ sudo dnf config-manager --set-enabled powertools
$ sudo dnf install -y git rpm-build redhat-rpm-config rpmdevtools wget
$ sudo dnf install -y gcc make perl perl-Data-Dumper libtool systemtap-sdt-devel valgrind-devel pcre-devel
$ wget https://openresty.org/package/centos/openresty.repo
$ sudo mv openresty.repo /etc/yum.repos.d/
$ sudo dnf update
$ sudo dnf install -y openresty-zlib-devel openresty-openssl111-devel openresty-pcre-devel
```

### openresty-modsecurity-develをビルドする

```
$ tar xf rpmbuild.el8stream.tar.gz
$ sudo dnf install -y libxml2-devel GeoIP-devel lmdb-devel gcc-c++ flex bison yajl-devel ssdeep-devel ccache
$ QA_RPATHS=0x0002 rpmbuild -ba ~/rpmbuild/SPECS/openresty-modsecurity.spec
$ sudo rpm -ivh ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-3.0.4-1.el8.x86_64.rpm \
  ~/rpmbuild/RPMS/x86_64/openresty-modsecurity-devel-3.0.4-1.el8.x86_64.rpm
```

### OpenRestyをModSecurityを有効にしてビルドする

```
$ rpmbuild -ba ~/rpmbuild/SPECS/openresty.spec
```

上記によりRPMパッケージがビルドされます。

## 付録D ソースアーカイブ取得方法について

パッケージのビルドや動作確認に用いている各種ソースアーカイブの取得方法を次に示します。

* openresty-packaging
* ModSecurity
* ModSecurity-nginx
* OpenResty
* OWASP ModSecurity Core Rule Set

### openresty-packaging

```sh
$ git clone https://github.com/clear-code/openresty-packaging
$ cd openresty-packaging
$ git checkout cc-modsecurity-20210114
```

### ModSecurity

```sh
$ wget -O ~/rpmbuild/SOURCES/modsecurity-v3.0.4.tar.gz https://github.com/SpiderLabs/ModSecurity/releases/download/v3.0.4/modsecurity-v3.0.4.tar.gz
```

### ModSecurity-nginx

```sh
$ wget -O ~/rpmbuild/SOURCES/modsecurity-nginx-v1.0.1.tar.gz https://github.com/SpiderLabs/ModSecurity-nginx/releases/download/v1.0.1/modsecurity-nginx-v1.0.1.tar.gz
```

### OpenResty

```sh
$ wget -O ~/rpmbuild/SOURCES/openresty-1.19.3.1.tar.gz https://openresty.org/download/openresty-1.19.3.1.tar.gz
```

### OWASP ModSecurity Core Rule Set

```sh
$ wget -O ~/rpmbuild/SOURCES/v3.3.0.tar.gz https://github.com/coreruleset/coreruleset/archive/v3.3.0.tar.gz
```

## 付録E CentOS 8(Stream)への移行手順

CentOS 8からCentOS 8(Stream)へ次のようにして移行します。

```sh
$ sudo dnf update -y
$ sudo dnf install -y centos-release-stream epel-release
$ sudo dnf distro-sync -y
```

移行が完了すると redhat-releaseがCentOS Stream release 8となります。

```
$ cat /etc/redhat-release 
CentOS Stream release 8
```
