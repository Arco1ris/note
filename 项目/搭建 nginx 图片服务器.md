场景：
前端需要可配置的图片，因此后端需要提供图片地址，本地调试需要搭建一个图片服务器。

过程：
1. 使用 nginx 进行图片服务器的搭建
2. nginx 进行 server 配置，配置内容如下
```
server {
listen 80;  #监听80端口
server_name  images.me.in #随便取的一个域名，需要在 hosts 文件中同时进行配置，IP 域名 （IP 为 127.0.0.1，域名为  images.me.in）可以让项目和此域名保持一致以让图片服务器和项目同域可访问
 location /images/ {
    root /User/XX/Desktop/ ; #root 能够让访问 /images/ 路径时，访问到 /User/XX/Desktop/images，如果将 root 替换为 alias，则访问的是 /User/XX/Desktop/；其中 /User/XX/Desktop/ 是本机中的绝对路径。
autoindex on; #打开目录浏览功能
  }
}
```
3. 启动 nginx
4. 浏览器输入地址 images.me.in ，有看到 nginx 欢迎页说明启动成功，
5. 继续访问  images.me.in/images，提示 403 forbidden，说明文件夹无法被访问到，nginx 用户没有权限。
6. 猜测原因是 nginx 没有权限去访问 images 文件夹，或者文件夹自身问题导致不能被某些用户访问，
7. 通过 ls -l 命令查看文件夹权限，-rwxr--r--，用户能读写执行，其它人也能读，说明不太可能是 images 的问题，一开始为了测试将 images 权限全部打开，`chmod 777 images`，由于是本地暂时调试，所以没考虑到访问安全的问题，不过后来改回 744，以确保不是文件夹的权限问题。
8. 查阅网上资料发现 nginx 默认用户是 nobody，没有访问目录权限，要修改 nginx 的配置文件 nginx.conf ，打开 nginx.conf 第一行修改为 user XX staff，记得要去掉注释 ，XX 为当前用户，staff 为所属 group, 并保存。(这里如果不写staff 会不成功，原因还未查明)
9. 重新启动 nginx
10. 访问 images.me.in/images 发现能看到图片文件夹。配置结束。