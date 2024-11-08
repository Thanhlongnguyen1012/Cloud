# Table of Contents

- [LAB1](#lab1)
  - [Xây dựng các tài nguyên lên cụm Kubernetes](#xây-dựng-các-tài-nguyên-lên-cụm-kubernetes)
    - [1. Tạo file yaml](#1-tạo-file-yaml)
    - [2. Tạo Service và Deployment với câu lệnh](#2-tạo-service-và-deployment-với-câu-lệnh)
  - [Kết quả](#kết-quả)
    - [Sử dụng lệnh để khởi động `service`](#sử-dụng-lệnh-để-khởi-động-service)
- [LAB2](#lab2)
  - [Đóng gói ứng dụng và đưa lên `Docker Hub`](#đóng-gói-ứng-dụng-và-đưa-lên-docker-hub)
    - [1. Đóng gói container](#1-đóng-gói-container)
    - [2. Chia sẻ ứng dụng lên dockerhub](#2-chia-sẻ-ứng-dụng-lên-dockerhub)
  - [Xây dựng tài nguyên lên cụm Kubernetes](#xây-dựng-tài-nguyên-lên-cụm-kubernetes)
    - [1. Tạo file `lab2.yaml` và tạo Deployment và Service bằng câu lệnh](#1-tạo-file-lab2yaml-và-tạo-deployment-và-service-bằng-câu-lệnh)
    - [2. Kiểm tra trạng thái các tài nguyên vừa tạo](#2-kiểm-tra-trạng-thái-các-tài-nguyên-vừa-tạo)
    - [3. Mở ứng dụng vừa tạo](#3-mở-ứng-dụng-vừa-tạo)
- [LAB3](#lab3)
  - [Đóng gói ứng dụng và đưa lên `Docker Hub`](#đóng-gói-ứng-dụng-và-đưa-lên-docker-hub-1)
    - [1. Đóng gói web tĩnh thứ 2](#1-đóng-gói-web-tĩnh-thứ-2)
    - [2. Chia sẻ ứng dụng lên dockerhub](#2-chia-sẻ-ứng-dụng-lên-dockerhub-1)
  - [Triển khai các tài nguyên lên Kubernetes](#triển-khai-các-tài-nguyên-lên-kubernetes)
    - [1. Tạo tài nguyên cho web 2 bằng file `yaml`](#1-tạo-tài-nguyên-cho-web-2-bằng-file-yaml)
    - [2. Tạo tài nguyên cho nginx-proxy](#2-tạo-tài-nguyên-cho-nginx-proxy)
    - [3. Kết quả khi cấu hình xong cụm](#3-kết-quả-khi-cấu-hình-xong-cụm)
- [Tài liệu tham khảo](#tài-liệu-tham-khảo)

# LAB1

## Xây dựng các tài nguyên lên cụm Kubernetes
### 1. Tạo file yaml
Viết file `lab1.yaml` để tạo `Service` và  `Deployment`.
Trước khi tạo `Deployment` ta kiểm tra cụm:

![This is an alt text.](https://i.imgur.com/T4vK9Qu.png "This is a sample image.")
### 2. Tạo Service và Deployment với câu lệnh:
```
kubectl apply -f lab1.yaml

```
Sau khi tạo ta kiểm tra hoạt động của Service và các Pod đã được tạo:

![This is an alt text.](https://i.imgur.com/dB4LC2g.png "This is a sample image.")
## Kết quả
Với cụm có `<Ip-Node>`có thể sử dụng câu lệnh theo `Service` loại `NodePort`: 
```
http://<nodeIP>:nodePort
```
Kiểm tra `<Ip-Node>` hay` EXTERNAL-IP`sau khi thực hiện câu lệnh: 
```
kubectl get nodes -o wide 
```
![This is an alt text.](https://i.imgur.com/y7wt3nI.png "This is a sample image.")

Trong cụm `minikube` không có ` EXTERNAL-IP` nên ta thực hiện mở ứng dụng bằng lệnh `service`.
### Sử dụng lệnh để khởi động `service`
```
minikube service nginx-default-service

```
Minikube tự động mở trình duyệt mặc định:

 ![This is an alt text.](https://i.imgur.com/VMraf3E.png "This is a sample image.")
 ![This is an alt text.](https://i.imgur.com/DV5NrMu.png "This is a sample image.")

```
curl http://127.0.0.1:56632

```

# LAB2

## Đóng gói ứng dụng và đưa lên `Docker Hub`
### 1. Đóng gói container
Download template tại  [antique-cafe](https://www.free-css.com/free-css-templates/page295/antique-cafe).

Dùng lệnh `cd` đến thư mục chứa template đã giải nén và ở trong ví dụ này ta mở bằng thư mục bằng`visual studio code`. Thực hiện tạo `Dockerfile` và `nginx.conf`cùng cấp.
```
cd [thư_mục_đích]

```
#### Khởi động Docker desktop và Sử dụng lệnh `docker build` để tạo image: 
```
docker build -t static-web1-final .

```
![This is an alt text.](https://i.imgur.com/3i2nZQt.png "This is a sample image.")
#### Chạy container bằng lệnh `docker run`:
```
docker run -d -p 127.0.0.1:80:80 static-web1-final

```
![This is an alt text.](https://i.imgur.com/wXRdngg.png "This is a sample image.")
Ta kiểm tra trên docker desktop thấy container ở trạng thái `Running` có nghĩa là đã tạo `image` thành công :

![This is an alt text.](https://i.imgur.com/eYnTLLy.png "This is a sample image.")
Mở trình duyệt (browser) địa chỉ để thấy ứng dụng: 

```
http://127.0.0.1:80

```
![This is an alt text.](https://i.imgur.com/o668CtX.png "This is a sample image.")
Hoặc sử dụng lệnh `curl`:
```
curl http://127.0.0.1:80
```
![This is an alt text.](https://i.imgur.com/N8Md4ej.png  "This is a sample image.")
### 2. Chia sẻ ứng dụng lên dockerhub
#### Gắn `tag` cho ứng dụng bằng lệnh `docker tag`: 
```
docker tag static-web1-final thanhlongnguyen1012/static-web1-final
```
#### Sử dụng lệnh `doker push` để đưa ứng dụng lên `docker hub':
```
docker push thanhlongnguyen1012/static-web1-final
```
![This is an alt text.](https://i.imgur.com/AUO4jxa.png "This is a sample image.")
#### Mở `docker hub` kiểm tra image đã được chia sẻ thành công: 
![This is an alt text.](https://i.imgur.com/G0sx1Zd.png "This is a sample image.")
## Xây dựng tài nguyên lên cụm Kubernetes
### 1. Tạo file `lab2.yaml` và tạo Deployment và Service bằng câu lệnh:
```
kubectl apply -f lab2.yaml
```
### 2. Kiểm tra trạng thái các tài nguyên vừa tạo: 
#### Kiểm tra hoạt động của các `Pod` bằng câu lệnh: 
```
kubectl get pod
```
#### Kiểm tra hoạt động của các `Service` bằng câu lệnh: 
```
kubectl get service
```
![This is an alt text.](https://i.imgur.com/KWDyzWJ.png "This is a sample image.")
### 3. Mở ứng dụng vừa tạo
Với cụm có `<Ip-Node>`có thể sử dụng câu lệnh theo `Service` loại `NodePort`: 
```
http://<nodeIP>:nodePort
```
Kiểm tra `<Ip-Node>` hay` EXTERNAL-IP`sau khi thực hiện câu lệnh: 
```
kubectl get nodes -o wide 
```
![This is an alt text.](https://i.imgur.com/y7wt3nI.png "This is a sample image.")

Trong cụm `minikube` không có ` EXTERNAL-IP` nên ta thực hiện mở ứng dụng bằng lệnh: 
```
minikube service static-web1-service

```
![This is an alt text.](https://i.imgur.com/PXP1Kny.png "This is a sample image.")
Câu lệnh trên mở browser mặc định:

![This is an alt text.](https://i.imgur.com/BVC1auM.png "This is a sample image.")
Có thể thực hiện lệnh `curl`: 
```
curl http://127.0.0.1:53569
```
![This is an alt text.](https://i.imgur.com/v0HEhww.png "This is a sample image.")
# LAB3

## Đóng gói ứng dụng và đưa lên `Docker Hub`
### 1. Đóng gói web tĩnh thứ 2
Cách làm tương tự như ở `lab2`. Trang web được sử dụng ở đây được download tại [makaan template](https://www.free-css.com/free-css-templates/page295/makaan).
#### Khởi động Docker desktop và Sử dụng lệnh `docker build` để tạo image: 
```
docker build -t static-web2-final .

```
#### Chạy container bằng lệnh `docker run`:
```
docker run -d -p 127.0.0.1:80:80 static-web2-final

```
![This is an alt text.](https://i.imgur.com/GEXWFir.png "This is a sample image.")

#### Mở trình duyệt (browser) để thấy ứng dụng với địa chỉ: 
```
http://127.0.0.1:80

```
![This is an alt text.](https://i.imgur.com/NffCef0.png "This is a sample image.")
#### Hoặc sử dụng lệnh `curl`:
```
curl http://127.0.0.1:80
```
![This is an alt text.](https://i.imgur.com/XBWcpIh.png "This is a sample image.")
### 2. Chia sẻ ứng dụng lên dockerhub
#### Gắn `tag` cho ứng dụng bằng lệnh `docker tag`: 
```
docker tag static-web2-final thanhlongnguyen1012/static-web2-final
```
#### Sử dụng lệnh `doker push` để đưa ứng dụng lên `docker hub':
```
docker push thanhlongnguyen1012/static-web2-final
```
![This is an alt text.](https://i.imgur.com/Se5qtet.png "This is a sample image.")
#### Mở `Docker Hub` kiểm tra image đã được chia sẻ thành công: 
![This is an alt text.](https://i.imgur.com/lVVKvOZ.png "This is a sample image.")
## Triển khai các tài nguyên lên Kubernetes
### 1. Tạo tài nguyên cho web 2 bằng file `yaml`:
*chú ý nếu đã tạo tài nguyên bằng file `lab2.yaml` thì không tạo bằng file `lab3-1.yaml` để tránh xung đột vì 2 file giống hệt nhau, đều có mục đích là tạo tài nguyên web1 tĩnh* 
#### Sử dụng file `lab3-2.yaml`câu lệnh sau để tạo `Deployment` và `Service` cho trang web thứ 2: 
```
kubectl apply -f lab3-2.yaml

```
#### Kiểm tra hoạt động của các `Pod` bằng câu lệnh: 
```
kubectl get pod
```
#### Kiểm tra hoạt động của các `Service` bằng câu lệnh: 
```
kubectl get service
```
![This is an alt text.](https://i.imgur.com/yXD3TMU.png "This is a sample image.")

#### Sử dụng lệnh để khởi động `Service`
Với cụm có `<Ip-Node>`có thể sử dụng câu lệnh theo `Service` loại `NodePort`: 
```
http://<nodeIP>:nodePort
```
Kiểm tra `<Ip-Node>` hay` EXTERNAL-IP`sau khi thực hiện câu lệnh: 
```
kubectl get nodes -o wide 
```
![This is an alt text.](https://i.imgur.com/y7wt3nI.png "This is a sample image.")

Trong cụm `minikube` không có ` EXTERNAL-IP` nên ta thực hiện mở ứng dụng bằng lệnh: 
```
minikube service static-web2-service

```
Minikube tự động mở trình duyệt mặc định:

 ![This is an alt text.](https://i.imgur.com/PmiyQ5F.png "This is a sample image.")
 
 ![This is an alt text.](https://i.imgur.com/YwEhffS.png "This is a sample image.")
 
 Có thể thực hiện lệnh `curl`: 
```
curl http://127.0.0.1:55185

```
### 2. Tạo tài nguyên cho nginx-proxy
#### Tạo `configmap` để cấu hình NGINX Reverse Proxy theo yêu cầu:
```
kubectl apply -f lab3-3.yaml

```

#### Sử dụng file `lab3-4.yaml`câu lệnh sau để tạo `Deployment` và `Service` cho trang web thứ 2: 
```
kubectl apply -f lab3-4.yaml

```
#### Kiểm tra hoạt động của các `Pod` bằng câu lệnh: 
```
kubectl get pod
```
#### Kiểm tra hoạt động của các `Service` bằng câu lệnh: 
```
kubectl get service
```
![This is an alt text.](https://i.imgur.com/7s79jp6.png "This is a sample image.")

#### Sử dụng lệnh để khởi động `Service`
Với cụm có `<Ip-Node>`có thể sử dụng câu lệnh theo `Service` loại `NodePort`: 
```
http://<nodeIP>:nodePort
```
Kiểm tra `<Ip-Node>` hay` EXTERNAL-IP`sau khi thực hiện câu lệnh: 
```
kubectl get nodes -o wide 
```
![This is an alt text.](https://i.imgur.com/y7wt3nI.png "This is a sample image.")

Trong cụm `minikube` không có ` EXTERNAL-IP` nên ta thực hiện mở ứng dụng bằng lệnh: 
```
minikube service nginx-proxy

```
Minikube tự động mở trình duyệt mặc định:

 ![This is an alt text.](https://i.imgur.com/EbrYNHT.png "This is a sample image.")
 
 ![This is an alt text.](https://i.imgur.com/IjkHDNF.png "This is a sample image.")
 
 Có thể thực hiện lệnh `curl`: 
```
curl http://127.0.0.1:55231

```
### 3. Kết quả khi cấu hình xong cụm
#### Thực hiện curl path/web1
```
curl http://127.0.0.1:55231/web1

```
Kết quả thu được là: 

![This is an alt text.](https://i.imgur.com/8A4VQYr.png "This is a sample image.")

*Chú ý rằng phải truy cập có thêm `/ `đằng sau nếu muốn xuất hiện giao diện web một cách đầy đủ, vì khi có dấu `/` thì nginx mới hiểu và lấy đủ các file `html`,`htm`... trong source code.*

Ta có thể mở bằng `browser` bằng câu lệnh:
```
http://127.0.0.1:55231/web1

```
Nếu muốn hiển thị đầy đủ giao diện: 
```
http://127.0.0.1:55231/web1/

```

![This is an alt text.](https://i.imgur.com/9SMlO9m.png "This is a sample image.")
#### Thực hiện curl path/web2
```
curl http://127.0.0.1:55231/web2

```
Kết quả thu được là: 

![This is an alt text.](https://i.imgur.com/7ErLcb8.png "This is a sample image.")

Ta có thể mở bằng `browser`:

![This is an alt text.](https://i.imgur.com/P6SNp3w.png "This is a sample image.")

## Tài liệu tham khảo
1.  [Template web](https://www.free-css.com/).
2. [NGINX Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).
3.  [NGINX location](https://nginx.org/en/docs/http/ngx_http_core_module.html?&_ga=2.233688222.1402415623.1730809049-1502593549.1716948748#location).
4.  [https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).
5. [nginx.conf](https://gist.github.com/mockra/9062657).
6. [https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master](https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master).