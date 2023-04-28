负载均衡：


upstream server_name {
       /// ip_hash;
        server  ip:port weight=10;
        server  ip:port weight=2;
}


反向代理:


proxy_pass http://server_name;
