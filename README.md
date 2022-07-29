# caddyfile路径分流配置
```
yourdomain {
    log {
        output file /etc/caddy/caddy.log
    }
    tls {
        protocols tls1.2 tls1.3
        ciphers TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256
        curves x25519
    }
    reverse_proxy localhost:5212 #在5212端口搭建伪装站点，例如cloudreve
    @v2ray_websocket {
        path /zhou2
        header Connection Upgrade
        header Upgrade websocket
    }
    reverse_proxy @v2ray_websocket localhost:8488 #v2ray运行端口
}
```
# v2ray vless 配置文件
```
{
  "inbounds": [
    {
      "port": 8488,
      "listen":"127.0.0.1",
      "protocol": "vless",
      "settings": {
      "decryption": "none",
        "clients": [
          {
            "id": "4ba8242b-b72f-4d6f-b05e-c4b794a9c2aa",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/zhou2"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

