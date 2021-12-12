
- 配置user.name和user.email
- - global 全局生效
```bash
git config --global user.name 'your_name'
git config --global user.email 'your_email@domain.com'
# maybe --local 放在末尾
git config --local  user.email 'yangwubing@gmail.com' 
# unset 
git config --unset user.email=yangwubing@gmail.com
```

- 配置config
```bash
# local 只对某个仓库有效
git config --local
# global 对当前用户的所有仓库有效
git config --global
# system 对系统所有登录的用户有效
git config --system
```

- 显示config的配置 加 --list
```bash
git config --list --local
git config --list --global
git config --list --system
```

### 建立Git仓库

#### 1. 把已有的项目代码纳入Git管理
```bash
cd proj
git init
```

#### 2. 新建项目
```bash
cd proj
git init proj
cd proj
```

### 