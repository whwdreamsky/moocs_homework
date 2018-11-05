# git 

### 创建git 远程仓库呢







### 遇到的问题

1. 登录Permission denied (publickey)

   ```
   极大多数情况是由于github账号没有设置ssh公钥信息所致。 前往 GitHub 网站的"account settings"依次点击"Setting -> SSH Keys"->"New SSH key" Title处填写“id_rsa.pub”或其他任意信息。 key处原样拷贝下面命令的打印 `~/.ssh/id_rsa.pub` 文件的内容： ``` cat ~/.ssh/id_rsa.pub ```如没有则按下述方法生成：  ssh-keygen -t rsa  一路回车......  最后，输入“ssh -T git@github.com”确认OK即可。再尝试输出就应该有了```cat ~/.ssh/id_rsa.pub```

   作者：ElonChan
   链接：https://www.zhihu.com/question/21402411/answer/95945718
   来源：知乎
   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
   ```

   ​

