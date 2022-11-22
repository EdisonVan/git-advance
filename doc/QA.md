# Failed to connect to github.com port 443: Timed out

**方案一：通过 switchHosts 设置两套环境**
**公司 vpn**

```bash
# BEGIN section for OpenVPN Client SSL sites
127.94.0.1 client.openvpn.net
# END section for OpenVPN Client SSL sites
```

**develop**

```bash
199.232.4.133 raw.githubusercontent.com
140.82.112.4 github.com
199.232.69.194 github.global.ssl.fastly.net
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
```

**方案二**

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
sudo vim /etc/hosts

git config http.proxy http://192.168.201.9:8080
git config https.proxy http://192.168.201.9:8080
git config user.email "vanjohnson@foxmail.com"
git config user.name "EdisonVan"
git push -u origin --all

brew cask install switchhosts
ping raw.githubusercontent.com
```

[433 解决方案](https://stackoverflow.com/questions/49345357/fatal-unable-to-access-https-github-com-xxx-openssl-ssl-connect-ssl-error)

- https://www.ipaddress.com/
- https://gitee.com/jyotish/githubhosts/blob/master/README.md
- https://github.com.ipaddress.com
- https://www.youtube.com/watch?v=0lKoXWPoUB0
- https://mp.weixin.qq.com/s/JzSOs10iUcdb48KNHJ6Ihw
- https://blog.csdn.net/qq_43827595/article/details/106736124
- https://blog.csdn.net/developer_zhao/article/details/107785387
- [git 433 解决方案](https://www.jianshu.com/p/c2e829027b0a)
- `sudo vim /etc/hosts`或`Shift+Command+G`查找文件`/etc/hosts`,确认前往
- 避免 DNS 污染，公司 vpn 和 github 环境用 switchosts 区分
- 终端在输以下指令刷新 DNS:`sudo killall -HUP mDNSResponder;say DNS cache has been flushed`
