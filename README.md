# srt2rtmp

前言

本次教程采用腾讯云上海免费升级的2C4G8M服务器举例，搭建基于SRS+ffmpeg的环境进行直播流量的转发，便于海外用户直播推流B站。

参考配置


![image](https://user-images.githubusercontent.com/47912037/114272259-d0741480-9a58-11eb-837a-f5030301f8f5.png)
![image](https://user-images.githubusercontent.com/47912037/114272260-d23dd800-9a58-11eb-9809-29de68a7bd52.png)

网速注意

请自行通过iperf3测试本地到国内服务器的udp速度

# 中转教程
## 0. 请注意放行有关端口的UDP流量


## 1.	安装Docker

curl -fsSL https://get.docker.com | bash -s docker

## 2. 去网站上开播（通过malus或者穿梭等免费的chrome插件即可做到），复制出网站上的提供的“服务器”和“直播码”，将两段链接拼接到一起。下载repo中的srt.conf，将拼接好的链接替换到srt.conf第47行的output后。

![0FB5E0C90AE62846FF92E78152F8AF5D](https://user-images.githubusercontent.com/47912037/140515796-0cda5977-a382-46e7-8dad-d9dfc12fbfdb.jpg)

## 3. 在国内服务器的root目录里新建srt2rtmp文件夹，并将修改好的srt.conf放入其中。

## 4.	安装提供的Docker镜像

docker run -d -p 1937:1937/udp --restart=always --name srt2rtmp --network=host -v /root/srt2rtmp/srt.conf:/root/srt.conf haha66666/srt2rtmp

Ps:如果使用的是nat服务器，请使用

docker run -d -p 你的端口:1937/udp --restart=always --name srt2rtmp --network=host -v /root/srt2rtmp/srt.conf:/root/srt.conf haha66666/srt2rtmp

输入docker ps，检查docker镜像是否正常运行
![22141FB94E48248D5893F8F081EE0F5F](https://user-images.githubusercontent.com/47912037/140516656-bb5e5ead-a6ce-4834-96fa-91f97b1ad4d9.jpg)

如显示上图，则为正常运行

## 3.测试直播

去网站上开播

打开obs，将 设置-推流-服务器中的连接改为

srt://你的服务器IP:1937?mode=caller&transtype=live&streamid=#!::h=vhost/ingress/streamkey,m=publish

（例如srt://11.4.5.14:1937?mode=caller&transtype=live&streamid=#!::h=vhost/ingress/streamkey,m=publish），串流密码保持空白
![C0E2E80BC5BE43826B334E5D97BA3B4C](https://user-images.githubusercontent.com/47912037/140516484-33ecefc7-eb96-49ad-8fe7-746813804f30.jpg)

然后开播测试，确保OBS侧正常推流之后，前往直播间查看直播画面是否有花屏，丢帧等问题。
![image](https://user-images.githubusercontent.com/47912037/114272423-7889dd80-9a59-11eb-8107-fbaeb57131d7.png)


请尽情使用吧。

