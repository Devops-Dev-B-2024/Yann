FROM nginx

WORKDIR /usr/share/nginx/html

COPY ./html/index.html ./usr/share/nginx/html/index.html

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
