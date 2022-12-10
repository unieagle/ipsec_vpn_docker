VPN Docker 来源于: https://hub.docker.com/r/hwdsl2/ipsec-vpn-server

# Install VPN Server

```
docker run \
  --name ipsec-vpn-server \
  --restart=always \
  -v ikev2-vpn-data:/etc/ipsec.d \
  -v /lib/modules:/lib/modules:ro \
  -p 500:500/udp \
  -p 4500:4500/udp \
  -d --privileged \
  hwdsl2/ipsec-vpn-server
```

Then, use following command to retrieve login info:

```
docker logs ipsec-vpn-server
```

# For Clients Configuration

## Copy out all the configuration files out

```
docker cp ipsec-vpn-server:/etc/ipsec.d ./
```

Then the files are:
ipsec.d/vpnclient.p12 (for Windows & Linux)
ipsec.d/vpnclient.sswan (for Android)
ipsec.d/vpnclient.mobileconfig (for iOS & macOS)

ipsec.d/vpn-gen.env # env file generated automatically.

## Copy from host to your local

Change the following command as needed:

```
scp -r user@remote.host:/path/to/the/copied/ipsec.d ./
```

Then you will have those files.

## Clients

Distribute out the mentioned .p12, .sswan, .mobileconfig to the clients. Install them on the client platforms.

以Mac为例：

1. 双击 .mobileconfig 文件，会有提示让你去系统设置；
2. 打开系统设置，找到描述文件，会看到有一个等待安装的文件；
3. 进行安装，期间会提示输入密码，密码可以在刚才的log里面看到，或者在刚才下载的`.vpnconfig`文件里面；
4. 现在可以在系统设置的VPN看到刚才配置好的VPN了，并且应该可以连接。
