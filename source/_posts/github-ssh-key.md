---
title: 設定GitHub SSH金鑰
date: 2024-11-08 15:42:05
tags:
    - github
    - ssh
    - git
    - tutorial
    - setup
---

## Setup
1. 根據email創建一個新的SSH金鑰
```ssh-keygen -t rsa -b 4096 -C "your_email@example.com"```

2. 開啟ssh-agent，將私鑰加入到ssh-agent\
```
eval "$(ssh-agent -s)"
ssh-add -K ~/.ssh/id_rsa
```

3. 上傳公鑰，並到GitHub上上傳SSH key
```cat ~/.ssh/id_rsa.pub ```
[GitHub Keys Settings](https://github.com/settings/keys)
![image](https://hackmd.io/_uploads/HyiZfBiZkl.png)

4. ssh連接GitHub SSH
```ssh -T git@github.com```

## Resources
- [設定 Github SSH 金鑰 feat. Github SSH、HTTPS 的差異](https://ithelp.ithome.com.tw/articles/10205988)
- [How to clone, push, and pull with git (beginners GitHub tutorial)](https://youtu.be/yxvqLBHZfXk)
