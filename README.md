# bbrplus-5.10
Linux BBRplus Kernel 5.10 ported from BBRplus 4.14  
(note that it does NOT based on 5.10 version of BBR, instead just simple ported the 4.14 version of BBRplus)
<br/>
<br/>
<br/>

***based on original version***  
https://github.com/cx9208/bbrplus 
  
<br/>
<br/> 

## some improvements as Nov-2021

###  i)   merged official v4.14 tcp_bbr patches between 2018-2021 into bbrplus  
###  ii)  keep official v5.17 tcp_bbr module, so may use either  
```sh
net.ipv4.tcp_congestion_control = bbrplus
net.core.default_qdisc = fq
```
or
```sh
net.ipv4.tcp_congestion_control = bbr
net.core.default_qdisc = fq
```
in the `/etc/sysctl.conf` file. &nbsp;&nbsp; ( `fq` is the only recommended packet scheduler, do not use `fq_codel` `fq_pie` `cake` etc ) 
<br/>
<br/>
<br/>

## patch & build bbrplus kernel youself
(or you can use releases compiled by me in "Releases" section)   
<br/>
***(build requirement to GCC is >= 4.9, so GCC upgrade is needed if use CentOS 7.x as builder)*** 
<br/>

### 1) get convert patch on this repository, use git or direct download
        (e.g., convert_official_linux-5.10.x_src_to_bbrplus.patch)

<br/>
<br/>

### 2) download officaial linux kernel
        say 5.10.4        
            wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.10.4.tar.gz

<br/>
<br/>

### 3) extract the tarball & cd extracted directory
        tar zxvf linux-5.10.4.tar.gz && cd linux-5.10.4

<br/>
<br/>

### 4) copy convert patch to extracted kernel directory
        something like
            cp ../convert_official_linux-5.10.x_src_to_bbrplus.patch .

<br/>
<br/>

### 5) do the patch job
        patch -p1 < convert_official_linux-5.10.x_src_to_bbrplus.patch

<br/>
<br/>

(if no error or failed on previous step)
### 6) install dependencies for building kernels

<br/>

***CentOS***  
sudo yum groupinstall Development tools  
sudo yum install ncurses-devel bc gcc gcc-c++ ncurses ncurses-devel cmake elfutils-libelf-devel openssl-devel rpm-build redhat-rpm-config asciidoc hmaccalc perl-ExtUtils-Embed xmlto audit-libs-devel binutils-devel elfutils-devel elfutils-libelf-devel newt-devel python-devel zlib-devel

<br/>

press "y" key when asked

<br/>

***Debian/Ubuntu***  
sudo apt install build-essential libncurses5-dev  
sudo apt build-dep linux

<br/>

press "y" key when asked

<br/>
<br/>

### 7) config build parameters based on current kernel settings
        make oldconfig

<br/>

press Enter key when asked (if dont know what is what)


<br/>
<br/>

### 8) disable debug info & module signing
        scripts/config --disable DEBUG_INFO && scripts/config --disable MODULE_SIG


<br/>
<br/>

### 9) build kernel

<br/>

***CentOS***   
make rpm-pkg LOCALVERSION=-bbrplus 2>&1 | tee build.log

<br/>

***Debian/Ubuntu***  
make deb-pkg LOCALVERSION=-bbrplus 2>&1 | tee build.log

<br/>

if anything goes wrong check the "build.log" file

<br/>
<br/>

(if not failed on previous step)
### 10) collect kernel package files, do test on some other Linux machine

<br/>

***CentOS files***   
located in  
/"user home dir"/rpmbuild/RPMS/x86_64/

<br/>

***Debian/Ubuntu files***  
located in  
parent directory  
