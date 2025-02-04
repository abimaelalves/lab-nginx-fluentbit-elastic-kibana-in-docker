# Usa a imagem oficial do NGINX como base
FROM nginx:latest

# Define o diretório de trabalho
WORKDIR /usr/share/nginx/html

# Copia a página HTML para o diretório do NGINX
COPY html/index.html /usr/share/nginx/html/index.html

# Copia o arquivo de configuração do NGINX
COPY nginx.conf /etc/nginx/nginx.conf

# Cria diretórios para os logs
RUN mkdir -p /var/log/nginx && \
    touch /var/log/nginx/access.log /var/log/nginx/error.log

# Expõe a porta padrão do NGINX
EXPOSE 80

# Comando padrão para iniciar o NGINX
CMD ["nginx", "-g", "daemon off;"]
