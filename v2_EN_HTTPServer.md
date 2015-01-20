# SRS Embeded HTTP Server

SRS Embeded a HTTP web server, supports api and simple HTTP file for HLS.

To deploy SRS HTTP server, read [Usage: HTTP](https://github.com/winlinvip/simple-rtmp-server/wiki/v2_EN_SampleHTTP)

The SRS Embeded HTTP server is rewrite refer to go http module, so it's ok to use srs as http server. Read [#277](https://github.com/winlinvip/simple-rtmp-server/issues/277)


## Use Scenario

The SRS Embeded HTTP server is design to provides basic HTTP service, 
like the camera of mobile phone.

SRS should provides HTTP api, which is actually a embeded HTTP server.

Actually, RTMP is more complex than HTTP, so HTTP server on st is absolutely ok.
The HTTP Server in SRS1.0 is expirement, I will enhance it future.

## Config

Config the HTTP port and root.

```bash
# embeded http server in srs.
# the http streaming config, for HLS/HDS/DASH/HTTPProgressive
# global config for http streaming, user must config the http section for each vhost.
# the embed http server used to substitute nginx in ./objs/nginx,
# for example, srs runing in arm, can provides RTMP and HTTP service, only with srs installed.
# user can access the http server pages, generally:
#       curl http://192.168.1.170:80/srs.html
# which will show srs version and welcome to srs.
# @remeark, the http embeded stream need to config the vhost, for instance, the __defaultVhost__
# need to open the feature http of vhost.
http_stream {
    # whether http streaming service is enabled.
    # default: off
    enabled         on;
    # the http streaming port
    # @remark, if use lower port, for instance 80, user must start srs by root.
    # default: 8080
    listen          8080;
    # the default dir for http root.
    # default: ./objs/nginx/html
    dir             ./objs/nginx/html;
}
```

And, each vhost can specifies the dir.

```bash
vhost __defaultVhost__ {
    # http vhost specified config
    http {
        # whether enable the http streaming service for vhost.
        # default: off
        enabled     on;
        # the virtual directory root for this vhost to mount at
        # for example, if mount to /hls, user access by http://server/hls
        # default: /
        mount       /hls;
        # main dir of vhost,
        # to delivery HTTP stream of this vhost.
        # default: ./objs/nginx/html
        dir         ./objs/nginx/html;
    }
}
```

## MIME

Only some MIME is supported:

<table>
<tr><th>File ext name</th><th>Content-Type</th></tr>
<tr><td>.ts</td><td>Content-Type: video/MP2T;charset=utf-8</td>
<tr><td>.m3u8</td><td>Content-Type: application/x-mpegURL;charset=utf-8</td>
<tr><td>.json</td><td>Content-Type: application/json;charset=utf-8</td>
<tr><td>.css</td><td>Content-Type: text/css;charset=utf-8</td>
<tr><td>.swf</td><td>Content-Type: application/x-shockwave-flash;charset=utf-8</td>
<tr><td>.js</td><td>Content-Type: text/javascript;charset=utf-8</td>
<tr><td>.xml</td><td>Content-Type: text/xml;charset=utf-8</td>
<tr><td>Others</td><td>Content-Type: text/html;charset=utf-8</td>
</table>

## Method

Supported HTTP method:
* GET: Query API, or download file.

Winlin 2015.1