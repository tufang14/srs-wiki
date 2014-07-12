# 带宽测试

视频很卡，播放不了，缓冲区突然很大，推流上不来，都有可能是带宽过低，SRS支持测试客户端到服务器的带宽。

## SRS配置

SRS配置文件中需要打开带宽测试配置，一般是单独加一个vhost支持测速。SRS的配置`conf/bandwidth.conf`。譬如：

```bash
listen              1935;
vhost __defaultVhost__ {
}

vhost bandcheck.srs.com {
    enabled         on;
    chunk_size      65000;
    bandcheck {
        enabled         on;
        key             "35c9b402c12a7246868752e2878f7e0e";
        interval        30;
        limit_kbps      4000;
    }
}
```

<strong>假设服务器的IP是：192.168.1.170</strong>

## Flash测速工具

启动后用带宽测试客户端就可以查看：[http://winlinvip.github.io/srs.release/trunk/research/players/srs_bwt.html?server=192.168.1.170](http://winlinvip.github.io/srs.release/trunk/research/players/srs_bwt.html?server=192.168.1.170)

备注：请将所有实例的IP地址192.168.1.170都换成部署的服务器IP地址。

检测完毕后会提示带宽，譬如：

```bash
检测结束: 服务器: 192.168.1.107 上行: 2170 kbps 下行: 3955 kbps 测试时间: 7.012 秒
```

## 测速库

我提供了AS和JS的库，可以直接调用来和服务器测速。

AS的库，直接拷贝文件`SrsBandwidth.as`到工程，调用即可（参考注释说明）：
* AS库对象：[SrsBandwidth.as](https://github.com/winlinvip/simple-rtmp-server/blob/master/trunk/research/players/srs_bwt/src/SrsBandwidth.as)
* AS调用对象（主对象）：[srs_bwt.as](https://github.com/winlinvip/simple-rtmp-server/blob/master/trunk/research/players/srs_bwt/src/srs_bwt.as)

JS的库，需要拷贝`srs_bwt.swf`和`srs.bandwidth.js`，调用方法参考js说明：
* JS库对象：[srs.bandwidth.js](https://github.com/winlinvip/simple-rtmp-server/blob/master/trunk/research/players/srs_bwt/src/srs.bandwidth.js)
* JS调用对象（页面）：[srs_bwt.html](https://github.com/winlinvip/simple-rtmp-server/blob/master/trunk/research/players/srs_bwt.html)

备注：JS需要使用swf，可以导入Flex工程自己编译，或者使用已经编译好的[srs_bwt.swf](https://github.com/winlinvip/simple-rtmp-server/blob/master/trunk/research/players/srs_bwt/release/srs_bwt.swf)

## Linux工具测速

另外，SRS还提供了带宽检测命令行工具：

```bash
[winlin@dev6 srs]$ ./objs/bandwidth -i 127.0.0.1 -p 1935 -v bandcheck.srs.com -k 35c9b402c12a7246868752e2878f7e0e
[2014-04-16 16:11:23.335][trace][0][11] result: play 3742 kbps, publish 149 kbps, check time 7.0900 S
```

Winlin