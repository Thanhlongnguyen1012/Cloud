# syntax=docker/dockerfile:1

FROM nginx:1.14.2

# Xóa tệp cấu hình mặc định của nginx
RUN rm /etc/nginx/conf.d/default.conf

# Copy file cấu hình Nginx mới vào container
COPY nginx.conf /etc/nginx/conf.d/

# Copy các file của website vào thư mục html của nginx
COPY . /usr/share/nginx/html


# Khởi động nginx khi container bắt đầu
CMD ["nginx", "-g", "daemon off;"]

# Mở port 80
EXPOSE 80
