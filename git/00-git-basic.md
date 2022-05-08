

```bash
// 代码仓库本地配置文件
.git/config
// 全局配置文件
~/.gitconfig

// 修改本地配置文件
git config user.name "demo"
git config user.email "demo@demo.com"

// 查看配置文件
git config -l
git config --global -l

// linux
tree .git 

// 查看文件类型
git cat-file -t 8d0e41
// 查看文件内容
git cat-file -p 8d0e41
// 查看文件大小
git cat-file -s 8d0e41

// 列出索引区文件
git ls-files
// 列出索引区文件及信息
git ls-files -s
```



```bash
ls -la

// 创建文件并写入内容
echo "hello git">hello.txt
```



HEAD：理解成指针，永远指向当前工作分支

# Git 对象 (objects)

哈希值存储文件内容，对象类型。不存储文件名。哈希值 = 上一个文件夹名称 + 文件名。

哈希值计算（SHA1）：对象类型 文件内容长度\0文件内容

blob

commit

tree
