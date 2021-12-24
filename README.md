# HAPROXY
This is a reposiroty for record my notes and configs about haproxy. 
Here is a provided a Docker application in order to test you configurations. This application basically prints out given environment variable for every container in selected page. Basic but useful for working on loadbalancing:
https://github.com/hnasr/javascript_playground/tree/master/docker
Build the Dockerfile and then run the container with given parameters:

```
$sudo docker build .
$sudo docker run -p 2222:9999 -d -e APPID=2222 {{image_id}}
$sudo docker run -p 3333:9999 -d -e APPID=3333 {{image_id}}
$sudo docker run -p 4444:9999 -d -e APPID=4444 {{image_id}}
$sudo docker run -p 5555:9999 -d -e APPID=5555 {{image_id}}
```

Comments provided above basically creates 4 different container that prints out different APPIDs for requests.

## Notes

- Use this comment in order to bind every connections coming from port 443, and assign ssl certificate to it, and also use http2 instead of http/1.1
    ```
    bind *:443 ssl crt /etc/letsencrypt/live/haproxy.pem alpn h2,http/1.1  
    ```

- This comment basically forward url requests with /app1 to app1Back
    ```
    acl app1 path_end -i /app1
    use_backend app1Back if app1
    ```

- This comment blocks url requests ends with /admin
    ```
    http-request deny if { path -i -m beg /admin }
    ```

- Haproxy can be configured both for layer 4(tcp) and layer 7(http), you can use tcp mode if you want to forward your packages depends on ip addresses, but if you prefer something more complicated like creating rules for url names, headers, etc... http mode suppose to be used. You can basically specify the mode with the following command.(Note: Haproxy uses tcp at default, and don't forget to change backend mode if you switch it in frontend, both modes must be the same.)
    ```
    mode http
    ```

## Role Variables

| Variable        | Required | Default       | Choices                   | Comments                               |
| --------------- | -------- | ------------- | ------------------------- | -------------------------------------- |
| APPID           | yes      |               |                           |                                        |

## Requirements
- haproxy
- Docker

## Supported Distributions
- Centos7
- Debian10
- Debian11
- Pardus18
- Debian9
- Fedora30
- Pardus19.0
- Ubuntu16.04
- Ubuntu18.04
- Ubuntu19.04
- Ubuntu19.10
- Ubuntu20.04

## Author Information
Yigithan Saglam - saglamyigithan@gmail.com

## Reference
@Hussein Nasser (HAProxy Crash Course (TLS 1.3, HTTPS, HTTP/2 and more)) : https://www.youtube.com/watch?v=qYnA2DFEELw&t=26s
