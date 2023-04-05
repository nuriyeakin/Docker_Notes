### 1. Docker Temel Komutlar

#### FROM talimatı  
FROM imaj:tag
- Oluşturulacak imajın hangi imajdan oluşturulacağını belirten talimat. 
- Dockerfile içerisinde geçmesi mecburi tek talimat budur. Mutlaka olmalıdır. 

```
FROM ubuntu:18.04
```
#### RUN komut
- imaj olusturulurken shell'de bir komut çalıştırmak istersek bu talimat kullanilir. 
- Ornegin apt-get install xxx ile xxx isimli uygulamanin bu imaja yuklenmesi saglanabilir.

```
RUN apt-get update
```
#### WORKDIR klasor_lokasyonu
- cd komutuyla ile istedigimiz klasöre geçmek yerine bu talimat kullanilarak istedigimiz klasöre geçer ve oradan çalismaya devam ederiz. 
- Workdirin farkı eğer öyle bir dosya yoksa oluşturulur.

```
WORKDIR /usr/src/app
```

#### COPY kaynak hedef
- imaj icine dosya veya klasör kopyalamak için kullaniriz

```
COPY /source /user/src/app
```

#### EXPOSE port
- Bu imajdan olusturulacak containerlarin hangi portlar üstünden erisilebilecegini yani hangi portlarin yayinlanacagini bu talimatla belirtirsiniz.

```
EXPOSE 80/tcp
```

#### CMD komut
- Bu imajdan container yaratildigi zaman varsayilan olarak çalistirmasini istediginiz komutu bu talimat ile belirlersiniz.

```
CMD java merhaba
```
#### HEALTHCHECK komut
- Bu talimat ile Docker'a bir konteynerin hala çalisip çalismadigini kontrol etmesini soylebiliriz. 
- Docker varsayilan olarak container içerisinde çalisan ilk processi izler ve o caliştığı surece container çalismaya devam eder. 
- Fakat process calişsa bile onun düzgün işlem yapıp yapmadığına bakmaz. HEALTCHECK ile buna bakabilme imkanina kavuşuruz.

```
HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost/ || exit 1 
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------


### 2. Docker İmaje Temeller

#### 2.1 Docker olmadan önce sanal makinada "Linux Sunucuya Java Uygulaması Kurma Adımları"

- Toplamda 6 adımdan oluşuyor.

- 1.Fiziksel ya da Sanal sunucu kur 

- 2.İşletim sistemi kur

- 3.İşletim sistemini güncelle
```
$ sudo apt-get update -y
```

- 4.Java Runtime Environment (JRE) kur
```
$ sudo apt-get install default-jre -y
```

- 5.Uygulamayı sisteme kopyala
```
$ git clone https:
```

- 6.Sistem başlatıldığında uygulamanın da başlaması için ayar yap



#### 2.2 Docker İmage Oluşturma

Yukardaki 6 basamaklı işlemin aynısını yapıyoruz.

```
# 2. adım. İşletim sistemi kur
FROM ubuntu:18.04   

# 3. adım. İşletim sistemini güncelle
RUN apt-get update -y

# 4. adım. Java Runtime Environment (JRE) kur
RUN apt-get install default-jre -y

# 5. adım. Uygulamayı sisteme kopyala
# Merhaba isimli kasöre gececek

WORKDIR /merhaba

# 5.1. adım copyalama işlemini yapsın
COPY /myapp .

# 6. adım. Sistem başlatıldığında uygulamanın da başlaması için ayar yap
CMD [“java”, “merhaba”]

```

"-t" oluşturacağım imaje tag atamak istiyorum.
"." ile bulunduğum konumda işlem yapmasını istiyorum.

```
docker image build -t ozgurozturk/merhaba .
```

Bir docker image nin oluşurken oluşma süreci.
```
docker image history image_name
```

### 3. Docker Image Push 

Tag li bir imajı gönderiyoruz

```
docker image push ozgurozturknet/merhaba
```


### 4. Docker Image Oluşturma

Bir projeye sahibim ve dosya hiyerarşisi aşşagıdaki gibi. "package*.json" ve "server.js" da programlarım var.

```
NODEJS2
├── package*.json
└── server.js
└── Dockerfile
└── requirements.txt
```


```
FROM node:10
WORKDIR /usr/src/app        # Burdaki dizine git 
COPY package*.json ./       # Kopyala
RUN npm install             # Kopyala
COPY server.js .            # Yazdığım programın kodun çalışması için gereken kod
EXPOSE 8080                 # Çalışacak pod
CMD [“node”,”server.js”]    # Çalıştırma komutu.
```

- Requirement dosyalarını mutlaka en üste yazmalıyız !






