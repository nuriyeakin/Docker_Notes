
## 1. Docker compose nedir ? 

- Compose, çoklu Docker uygulamalarını tanımlamak ve çalıştırmak için bir araçtır. 
- Compose ile uygulamanızın hizmetlerini yapılandırmak için bir YAML dosyası kullanırsınız. 
- Ardından, tek bir komutla, tüm hizmetleri yapılandırmanızdan oluşturur ve başlatırsınız.

```
docker-compose  --version
```

## 2. Compose oluşturma

- Docker compose ile çalışmak için Docker-compose.yaml ya da Docker-compose.yml isimli bir dosya yaratmalıyız.
- CLI komutları yani docker-compose komutları sadece dockercompose.yaml dosyasının bulunduğu konumda çalışır.

```
docker-compose up -d
```

```
docker-compose down
```

- yaml dosyasını görmek için:
```
docker-compose config 
```

- Oluşturulan servislerin hangi imajlarla oluşturulduğunu listelemek
```
docker-compose images
```

### 3. Docker compose YAML dosyası

#### Top level veri blogları

- version: docker-compose un hangi versiyonunu kullanacağım.
- services: Oluşturmak istediğim containerları tanımlıyorum.
- volumes:
- networks:

```
version: "3.8"

services:
  veritabani: #Veri tabanı adında bir servis yani container oluşturuyorum.
    image: mysql:5.7 #Hangi imaj dan oluşacağını söylyorum.
    restart: always  #Kapanırsa her zaman çalıştır
    volumes: #veritabı  adında volum tanımladım
      - verilerim:/var/lib/mysql #veri tabanı adında volum tanımladım. var/lib/mysql mount ettim.
                                 #Verilerim isismli volum olmadığı için volumes kısmında tanımlamalıyız.
    environment: # databases in isteklerini yazıyorum
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks: # Olusturdugum servis veya containerların iletişimi için özel network
      - wpnet # Bu isimde bir network olmadığı için aşşağıda oluşturmak zorundayım.

  wordpress: #Worpress adında ikinci bir servis yani container oluşturuyorum.
    image: wordpress:latest #Hangi imaj dan oluşacağını söylyorum.
    depends_on: #Önce veritabanı isimli servis ayağa kalkmalı daha sonra wordpress. Depends on ile bu bağımlılılı sağlıyoruz.
      - veritabani  #Bağımlı kıldığımız servis
    restart: always #Kapanırsa her zaman çalıştır
    ports: # Çalışmasını istediğim port
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: veritabani:3306 
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks: 
      - wpnet

volumes: # Volum oluşturma yeri
  verilerim:

```

```
docker network ls
```

```
docker volume ls
```

```
docker compose ps
```

```
services:
   mysqldb:
     image: ozgurozturknet/webdb 
     environment:
      MYSQL_DATABASE: proje
      MYSQL_USER: projemaster
      MYSQL_PASSWORD: master1234
      MYSQL_ROOT PASSWORD: master1234
     networks:
      - webnet

   websrv:
     build: .         #Bu komut docker imajı bulunduğun dosyadaki docker file dan al demek.
     depends_on:
       - mysqldb
     ports:
       - "80:80"
     restart: always
     networks:
       - webnet
     environment:
       DB_SERVER: mysqldb
       DB_USERNAME: projemaster
       DB_PASS: master1234
       DB_NAME: proje
networks:
  webnet:
    driver: bridge
```

```
FROM php:7.3-apache
RUN apt-get update -y && apt-get install mariadb-client-10.3 -y RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN mkdir /var/www/html/images
RUN chmod 777 /var/www/html/images
COPY -/php/ /var/www/html/
```






