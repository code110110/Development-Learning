```shell
echo "# Development-Learning" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
#git remote add origin https://github.com/code110110/Development-Learning.git
git remote set-url origin git@github.com:code110110/Development-Learning.git
git push -u origin main
```

## 将修改过后的文件通过git上传文件到GitHub

```shell
git commit
git commit -a #提交所有的修改
#进入修改页面
#按 i 可以进行修改，添加描述文件，必须添加描述文件
#修改完毕之后 按 alt+c  退出输入模式  
#不需要进入修改的模式了
git commit -a -m "这是我修改的文件"
git push #提交到服务器
```

