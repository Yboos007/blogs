---
title: openssl升级错误
date: 2022-05-17 16:46:09
tags: Centos
author: Jerry
---
## 此目录为openssh-openssl升级脚本

仅支持Centos7系统

请下载文件后执行 update_ssh.sh

若Centos7.6升级时出现以下问题，可能为Perl版本低

```
make: *** [install_dev] 错误 1
```

请执行下面命令后重新执行 update_ssh.sh

```
sh <(curl -q https://platform.activestate.com/dl/cli/pdli01/install.sh) --activate-default Yboos007/Perl-5.32
```


