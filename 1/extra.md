#   Docker Practice - Basic Operations


## Sistemdeki tüm container, image ve volumeleri görünüz. 

```bash
docker container ls -a
docker volume ls
docker image ls
```

## Makinenizdeki tüm containerları, imageleri ve volumeleri temizleyin. 

-   Uzun Yol

```bash
docker container rm <id>
docker volume rm <volume_name>
docker image rm <id>
```

-   Kısayol

```bash
docker container prune
docker volume prune
docker image prune -a
```

## Bu imajları çalıştığımız sisteminize çekiniz
## centos, alpine, nginx, hello-world, httpd:alpine, techprodevops/test_app, techprodevops/website_dev  


```bash
docker image pull ...
```

## hello-world isimli imajdan bir container oluşturunuz

```bash
docker container run hello-world
```

## httpd:alpine isimli imajdan detached olarak ve web-server isimli bir container oluşturunuz. Bu container 'ın ismini ve id’sini kontrol ediniz.

```bash
docker container run httpd:alpine
docker container ls
```

## web-server isimli bu container’ın loglarına bakınız.

```bash
docker container logs
```

## Bu container ’ı durdurunuz, ardından yeniden çalıştırınız ve son olarak container’ı sistemden kaldırınız.

```bash
 docker container logs 93b
 docker container stop 93b
 docker rm 93b
```

## httpd:alpine isimli imajdan websunucu adında detached ve “-p 80:80” ile portu publish edilmiş bir container oluşturunuz. Kendi bilgisayarımızın browser 'ından bu web sitesine erişiniz.

```bash
docker container run -d -p 8000:80 --name websunucu httpd:alpine
```


## websunucu adlı bu container’ın içerisine bağlanın ve /usr/local/apache2/htdocs klasöründe echo "Techpro practice page" > index.html komutunu çalıştırınız.

```bash
docker container exec -it websunucu sh
cd /usr/local/apache2/htdocs
echo "Techpro practice page" > index.html
```

## Web tarayıcıya geçerek dosyaya ekleme yapabildiğimizi görmek için sayfayı yenileyiniz. Sonrasında container içerisinden read escape sequence yani ctrl+P ctrl+Q ile çıkalım.

## websunucu isimli container’ı çalışırken siliniz.

```bash
docker container rm-f websunucu
```

## alpine isimli imajdan bir container oluşturunuz ve varsayılan olarak çalışması gereken uygulama yerine “ls” uygulamasının çalışmasını sağlayınız.

```bash
docker container run alpine ls
```

## “practice” isimli bir Volume oluşturunuz. 

```bash
docker volume create practice
```

## alpine isimli imajdan “first” isimli bir container oluşturunuz. Bu container’ı interactive modda oluşturunuz ve bağlanabiliniz. Aynı zamanda “practice” isimli volume’u bu container 'ın “/test” isimli folder ’ına mount ediniz.

```bash
docker container run -it --name first -v practice:/test alpine
```


## “/test” içerisine geçiniz ve “touch abc.txt” komutuyla bir dosya oluşturunuz daha sonra “echo deneme >> abc.txt” komutuyla bu dosyanın içerisine yazı ekleyiniz. 

```bash
docker ...
```

## alpine isimli imajdan “second” isimli ve interactive modda bir container oluşturunuz. “practice” isimli volume’u bu containerın “/test” isimli folder’ına mount ediniz. Bu folder içerisinde “ls” komutuyla dosyaları listeleyiniz ve abc.txt dosyası olduğunu görünüz. “cat abc.txt” ile dosyanın içeriğini kontrol ediniz. 

```bash
docker ...
```

## alpine isimli imajdan “third_container” isimli bir container oluşturunuz. Bu container’ı interactive modda oluşturunuz ve bağlanın.  “practice” isimli volume ’ü bu containerın “/test” isimli folder’ına mount ediniz fakat Read Only olarak mount ediniz. Bu folder içerisine geçiniz ve “touch abc1.txt” komutuyla bir dosya yaratmaya çalışınız ve hatayı görünüz.

```bash
docker ...
```

## Bilgisayarınızda bir klasör oluşturunuz “örneğin c:\deneme” ve bu klasörün içerisinde index.html adlı bir dosya yaratıp bu dosyanın içerisine birkaç yazı ekleyiniz.

```bash
docker ...
```

## techprodevops/website_dev isimli imajdan websunucu1 adında detached ve “-p 80:80” ile portu publish edilmiş bir container oluşturunuz. 

```bash
docker ...
```

## Bilgisayarınızda yarattığımız klasörü container’ın içerisindeki /usr/local/apache2/htdocs klasörüne mount ediniz. Bir tane browser açarak 127.0.0.1 adresine gidiniz ve sitemizi görünüz. Daha sonra bilgisayarınızda yarattığımız klasörün içerisindeki index.html dosyasını edit ediniz ve yeni yazılar ekleyiniz. Web sayfasını refresh ederek bunların geldiğini görünüz.

```bash
docker ...
```

## Tüm çalışan container’ları siliniz. 

```bash
docker ...
```

