# Sử dụng một image chính thức của nginx
FROM nginx:alpine

# Xoá cấu hình Nginx mặc định
RUN rm /etc/nginx/conf.d/default.conf

# Copy file cấu hình Nginx mới vào container
COPY nginx.conf /etc/nginx/conf.d/

# Copy các file của website vào thư mục html của nginx
COPY . /usr/share/nginx/html


# Khởi động nginx khi container bắt đầu
CMD ["nginx", "-g", "daemon off;"]

# Mở port 80
EXPOSE 80


