# 1-lab-nginx

## 📌 Descrição
Este projeto cria um contêiner Docker com uma imagem customizada do **NGINX**. Ele exibe uma página HTML simples e armazena logs de acesso e erro.


## 📂 Estrutura do Projeto
```
1-lab-nginx/
│── Dockerfile
│── nginx.conf
│── html/
│   └── index.html
│── logs/
│   ├── access.log
│   └── error.log
```

## 🚀 Configuração e Execução
1. **Construa a imagem Docker:**
   ```bash
   docker build -t custom-nginx .
   ```
2. **Execute o contêiner:**
   ```bash
   docker run -d --name lab-nginx \
       -p 8080:80 \
       -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
       -v $(pwd)/logs/access.log:/var/log/nginx/access.log \
       -v $(pwd)/logs/error.log:/var/log/nginx/error.log \
       custom-nginx
   ```

3. **Testar a página:**
   ```bash
   curl -i http://localhost:8080
   ```
4. **Verificar logs:**
   ```bash
   tail -f logs/access.log
   ```

---

# 2-lab-nginx-fluentbit

## 📌 Descrição
Este projeto adiciona o **Fluent Bit** para coletar os logs do **NGINX**.

## 📂 Estrutura do Projeto
```
2-lab-nginx-fluentbit/
│── Dockerfile
│── fluent-bit.conf
│── nginx.conf
│── html/
│   └── index.html
│── logs/
│   ├── access.log
│   └── error.log
│── docker-compose.yml
```

## 🚀 Configuração e Execução
1. **Construa a imagem e suba os contêineres:**
   ```bash
   docker-compose up -d --build
   ```
2. **Testar a página:**
   ```bash
   curl -i http://localhost:8081
   ```
3. **Verificar logs do Fluent Bit:**
   ```bash
   docker logs -f fluentbit
   ```

---

# 3-lab-nginx-fluentbit-elasticsearch

## 📌 Descrição
Este projeto adiciona o **Elasticsearch** para armazenar e indexar os logs do **NGINX** coletados pelo **Fluent Bit**.

## 📂 Estrutura do Projeto
```
3-lab-nginx-fluentbit-elasticsearch/
│── Dockerfile
│── fluent-bit.conf
│── nginx.conf
│── html/
│   └── index.html
│── logs/
│   ├── access.log
│   └── error.log
│── docker-compose.yml
│── elasticsearch/
│   ├── elasticsearch.yml
```

## 🚀 Configuração e Execução
1. **Suba os contêineres:**
   ```bash
   docker-compose up -d --build
   ```
2. **Verifique os logs do Fluent Bit:**
   ```bash
   docker logs -f fluentbit
   ```
3. **Verifique os índices no Elasticsearch:**
   ```bash
   curl -X GET "http://localhost:9200/_cat/indices?v" -u elastic:changeme
   ```
4. **Buscar logs indexados:**
   ```bash
   curl -X GET "http://localhost:9200/nginx-logs/_search?pretty" -u elastic:changeme
   ```

---

# 4-lab-nginx-fluentbit-elasticsearch-kibana

## 📌 Descrição
Este projeto adiciona o **Kibana** para visualizar os logs do **NGINX** de forma amigável.

## 📂 Estrutura do Projeto
```
4-lab-nginx-fluentbit-elasticsearch-kibana/
│── Dockerfile
│── fluent-bit.conf
│── nginx.conf
│── html/
│   └── index.html
│── logs/
│   ├── access.log
│   └── error.log
│── docker-compose.yml
│── elasticsearch/
│   ├── elasticsearch.yml
│── kibana/
│   ├── kibana.yml
```

## 🚀 Configuração e Execução
1. **Suba os contêineres:**
   ```bash
   docker-compose up -d --build
   ```
2. **Verifique os logs no Kibana:**
   - Acesse: `http://localhost:5601`
   - Login: `elastic / changeme`
   - Configure um **Index Pattern** para `nginx-logs`.
   - Acesse **Discover** para visualizar os logs.

3. **Gerar novos logs para testar:**
   ```bash
   curl -i http://localhost:8083
   ```

Agora seu ambiente está pronto para observabilidade! 🚀🔥