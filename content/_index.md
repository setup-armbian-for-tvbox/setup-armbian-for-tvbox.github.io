---
title: "🌌 Windows Debloat "
date: 2026-07-08
draft: false
---

# Tutorial: Instalando Armbian em TV Box com RK3229

Este guia mostra como configurar sua TV Box com chip RK3229 usando o Armbian, desde a gravação do multitool até o autologin via SSH.

Inclui guia extra para executar ambiente nodejs e java via docker

É possível seguir a documentação de instalação com outros chips (amlogic, allwinner e etc), porem é necessario efetuar o teste com imagens apropriadas do armbian.


![lab](static/images/image.jpg)
![screen](static/images/screen.png)



## ⚠️ Requisitos importantes

- **Adaptador de cartão SD para USB**  
  Certifique-se de possuir um adaptador compatível. Você pode comprá-lo [aqui](https://mercadolivre.com/sec/2trNZa2).

- **Cartão SD com no mínimo 32GB**  
  É necessário um cartão com essa capacidade mínima. Você pode comprá-lo [aqui](https://mercadolivre.com/sec/2mhNMv8).

- **Patch Cord CAT6**
  É necessário um patch cord para conexao estavel. Você pode comprá-lo [aqui](https://mercadolivre.com/sec/23CxYAc).

- **Tv Box**
  É necessário uma tv box com rockchip. Você pode comprá-lo [aqui](https://mercadolivre.com/sec/1mjuZgN).



---

## 🛠️ Etapa 1: Gravar o Multitool no cartão SD

Você pode baixa-lo [aqui](https://drive.google.com/file/d/1UM3QCPTrssl8O-zXluTxHpnQnYGcVb8v/view?usp=drive_link).


```bash
sudo unxz -c /home/elima/Downloads/armbian/multitool_f.img.xz | sudo dd of=/dev/sda bs=4M status=progress conv=fsync
```

Agora mova a imagem do armbian para a pasta images.
Você pode baixa-la [aqui](https://drive.google.com/file/d/1z26f3gcbvdLBqoJSw0vjzBaQc-UQVOQ0/view?usp=drive_link).
#### ⚠️ Se houver erro ao mover a imagem para a pasta images, desmonte o disco e expanda a partição MULTITOOL.

---

## 📺 Etapa 2: Preparar a TV Box com o controle remoto

### Passos para configuração inicial
0. Deixe o **cartão SD** posicionado na bandeja
1. Acesse o menu **Configurações** da TV Box.
2. Selecione a opção **Redefinição de fábrica**.
3. Quando a tela exibir **"Reiniciando"**, insira o **cartão SD** na bandeja .
4. Aguarde o processo de **boot** — pode levar alguns minutos.
5. Utilize o **cabo HDMI original** da TV Box para garantir compatibilidade.

#### ⚠️  Durante essa fase, a imagem pode piscar ou apresentar **pixels verdes**. Isso é **normal** e esperado.

#### 🔌 Conecte um **teclado USB** na porta da TV Box para facilitar a navegação.

---

## 📄 Etapa 3: Navegar no Multitool

### Instruções passo a passo

1. Use a **seta para baixo** até o fim do contrato e pressione **Enter**.

2. No menu de opções, selecione:

   - **3. Erase flash** → pressione **Enter** e aguarde.
   - Após sucesso, pressione **Enter** para continuar.
   - **5. Burn image to flash** → pressione **Enter**.
   - Selecione o **device** → pressione **Enter**.
   - Selecione a **imagem** → pressione **Enter**.
   - Aguarde a **gravação**.
   - Ao final, pressione **Enter** no botão **OK**.

3. Volte ao menu e selecione:

   - **7. Shutdown**


---


## 🔌 Etapa 4: Preparar para o boot do Armbian

### Passos para inicialização

1. **Remova os seguintes itens da TV Box:**
   - Fonte da tomada
   - Teclado USB
   - **Cartão SD**

#### ⚠️⚠️⚠️ Remover o cartao irá manter o multitool e armbian para utilizar em outra tv box, caso nao remova-o, o armbian utilizará como um disco com ponto de montagem. ⚠️⚠️⚠️


2. **Insira o cabo de rede RJ45** na porta Ethernet da TV Box.

3. **Reconecte o teclado** na porta usb.

3. **Reconecte a fonte de energia** para ligar a TV Box.

⏳ **Aguarde o carregamento do Armbian.**

---


## ⌨️ Etapa 5: Configuração inicial no terminal

1. Conecte o **teclado USB** à TV Box.
2. Siga os **passos exibidos na tela**.
3. Ao final, você verá uma **tela de login** semelhante à de sistemas Linux.

---

## 🔐 Etapa 7: Acesso via SSH e ajustes finais

Após reiniciar o dispositivo:

1. Faça login com seu **usuário e senha**.
2. Acesse via **SSH** usando o comando:

```bash
ssh usuario@ip-local
```

---


## 🔓 Etapa 8: Configurar Autologin
Essa parte e necessaria para quando ocorrer reboot do sistema.

### 📁 Criar diretório e editar arquivo de configuração

Execute os comandos abaixo no terminal para configurar o login automático:


```bash
sudo mkdir -p /etc/systemd/system/getty@tty1.service.d
sudo nano /etc/systemd/system/getty@tty1.service.d/autologin.conf
```

### 📄 Conteudo do arquivo 

#### Coloque o nome do seu usuario.

```bash
[Service]
ExecStart=
ExecStart=-/sbin/agetty --autologin seu-usuario --noclear %I $TERM
```


---

## ✅ Etapa 9: Finalização

Agora você pode:

- **Desligar** o dispositivo
- **Movê-lo** para junto do roteador com patch cord de classificação cat6
- **Ligá-lo novamente**

🔁 O **login será automático** e o acesso poderá ser feito via **SSH**.


--- 
## Extra !: Montar HD externo

⚠️ Certifique-se qual o ponto de montagem do seu disco, se necessario altere o path

```bash

sudo apt install -y ntfs-3g

sudo lsblk
sudo mkdir -p /mnt/sda
sudo mount -t ntfs-3g /dev/sda1 /mnt/sda

ls -ld /mnt/sda
sudo chown -R $USER:$USER /mnt/sda

df -h

```

Dica: Voce pode autoatizar a montagem no crontab com a diretiva reboot

---

## 🛡️ Recomendações finais

- 🔒 **Mude a senha** para algo mais forte e seguro.
- 🏷️ **Renomeie o dispositivo** para facilitar sua identificação na rede.


---


❤️ Feito! Sua TV Box com RK3229 está pronta para rodar Armbian com acesso remoto via SSH. 😎



---


## [EXTRA] Configurar Nginx atrás do Tailscale

Securely expose your Nginx web server using the power of [Tailscale](https://tailscale.com) — a zero-config VPN built on WireGuard. This guide walks you through:

- 🔐 Configuring HTTPS with Tailscale-generated certificates  
- 🌐 Mapping host ports to your containerized Nginx instance  
- 🛡️ Restricting access to your private tailnet only  

Whether you're self-hosting a dashboard, API, or static site, this setup ensures encrypted, authenticated access across your trusted devices — without opening public ports.

> 📦 Built for Docker environments.  
> 🧩 Compatible with Alpine-based Nginx images.  
> 🧠 Designed for simplicity and security.


Run these commands to create the folder where Nginx will live.
```
mkdir proxy
cd proxy
```

Run these commands to creates folder structure
```
mkdir /html
mkdir -p /etc/nginx/conf.d
mkdir -p /etc/ssl/certs

```
Go to certs folder
```
cd /etc/ssl/certs
```

Run tailscale command to generate ssl certificate for your domain machine
```
sudo tailscale cert machineName.tailID.ts.net
```

<b>At Below files, you need replace this domain for your domain's : machineName.tailID.ts.net</b>

This is the docker compose file that creates a nginx container, save it inside proxy folder.
/proxy/docker-compose.yml
```
version: "3.9"

networks:
  nginx-net:
    driver: bridge

services:
  nginx:
    image: nginx:stable-alpine3.21
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./html/index.html:/usr/share/nginx/html/index.html
      - ./etc/nginx/conf.d:/etc/nginx/conf.d
      - ./etc/nginx/default.conf:/etc/nginx/default.conf
      - ./etc/ssl/certs:/etc/ssl/certs

```



Put this file at /etc/nginx/default.conf
```
user  nginx;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout 65;

    # Zonas de limite (opcional)
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m rate=5r/s;

    # Tamanho do nome de dominio
    server_names_hash_bucket_size 256;

    # Incluir sites
    include /conf.d/*.conf;

}

```



Put this file at /etc/nginx/conf.d/default.conf
```
server {
    listen 80;
    server_name machineName.tailID.ts.net;
    
    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ =404;
    }

}
server {
    listen 443 ssl;
    server_name machineName.tailID.ts.net;

    # Setup valid tls versions
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'HIGH:!aNULL:!MD5';
    ssl_prefer_server_ciphers off;


    ## Access and error logs.
    access_log /var/log/nginx/access.log;
    error_log  /var/log/nginx/error.log info;

    ## Keep alive timeout set to a greater value for SSL/TLS.
    keepalive_timeout 75 75;


    ssl_certificate     /etc/ssl/certs/machineName.tailID.ts.net.crt;
    ssl_certificate_key /etc/ssl/certs/machineName.tailID.ts.net.key;
    ssl_session_timeout  5m;

    ## Strict Transport Security header for enhanced security. See
    ## http://www.chromium.org/sts. I've set it to 2 hours; set it to
    ## whichever age you want.
    add_header Strict-Transport-Security "max-age=7200";


    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri $uri/ =404;
    }

    
}

```

At proxy folder's
```
sudo tailscale up
docker compose up -d
curl -vk machineName.tailID.ts.net
```

---

# dockerfile-jdk21-for-armv7


# ☕ Aplicação Java com Maven e Docker

Este projeto demonstra como construir e executar uma aplicação **Java (Spring Boot)** em um container Docker utilizando a imagem [`lavender-jre`](https://ghcr.io/retrodaredevil/lavender-jre).  
O processo é dividido em duas fases: **build** (compilação com Maven) e **runtime** (execução do JAR).

---

## 📦 Estrutura do Dockerfile

### 1. Fase de Build (`builder`)
- **Imagem base**: `ghcr.io/retrodaredevil/lavender-jre:21-ubuntu-noble`
- Instala:
  - `openjdk-21-jdk`
  - `maven`
- Define diretório de trabalho:
  ```dockerfile
  WORKDIR /app
  ```
- Copia o `pom.xml` para resolver dependências.
- Copia:
  - `src/`
  - `.env`
  - `logback.xml`
- Compila o projeto:
  ```dockerfile
  mvn clean package -DskipTests -q
  ```
- Resultado: JAR gerado em `/app/target/`.

---

### 2. Fase de Runtime (`runtime`)
- Usa novamente a imagem `lavender-jre`.
- Copia o JAR gerado para `/app/app.jar`.
- Define permissões de execução:
  ```dockerfile
  chmod +x app.jar
  ```

- Configura variáveis de ambiente:
  - **Spring Boot**:
    - `SPRING_PROFILES_ACTIVE=docker`
    - `SPRING_DATASOURCE_URL`
    - `SPRING_DATASOURCE_USERNAME`
    - `SPRING_DATASOURCE_PASSWORD`
  - **Observabilidade (OpenTelemetry)**:
    - `OTEL_SERVICE_NAME=ms-offers-importer`
    - `OTEL_RESOURCE_ATTRIBUTES`
    - Endpoints para logs, métricas e traces (`http://192.168.18.135:4318/...`)
  - **Identificação da instância**:
    - `INSTANCE_ID=$HOSTNAME`

- Exposição da porta:
  ```dockerfile
  EXPOSE 8082
  ```

- Configuração de JVM:
  ```dockerfile
  ENV JAVA_OPTS="-XX:+UseSerialGC -Xms64m -Xmx128m -XX:+UseContainerSupport -XX:MaxRAMPercentage=70 -XX:+AlwaysPreTouch -XX:-UseAdaptiveSizePolicy -XX:+DisableExplicitGC"
  ```

- Comando padrão:
  ```dockerfile
  CMD java $JAVA_OPTS -jar app.jar $LOGS_ENDPOINT $METRICS_ENDPOINT $SPANS_ENDPOINT
  ```

---

## ▶️ Como usar

### 1. Construir a imagem
```bash
docker build -t minha-app-java .
```

### 2. Executar o container
```bash
docker run -p 8082:8082 \
  -e SPRING_DATASOURCE_URL="jdbc:postgresql://host:5432/db" \
  -e SPRING_DATASOURCE_USERNAME="usuario" \
  -e SPRING_DATASOURCE_PASSWORD="senha" \
  minha-app-java
```

### 3. Acessar a aplicação
Abra no navegador:
```
http://localhost:8082
```

---

## 🛠️ Observabilidade

Este container já está configurado para enviar:
- **Logs** → `http://192.168.18.135:4318/v1/logs`
- **Métricas** → `http://192.168.18.135:4318/v1/metrics`
- **Traces** → `http://192.168.18.135:4318/v1/traces`

---

```dockerfile

# Usa a imagem oficial
FROM ghcr.io/retrodaredevil/lavender-jre:21-ubuntu-noble AS builder

RUN apt-get update

RUN apt-get install -y openjdk-21-jdk

RUN apt-get install -y maven

# Define diretório de trabalho
WORKDIR /app

RUN java -version
RUN mvn -version

# Copia apenas o pom.xml primeiro para resolver dependências
COPY pom.xml ./

RUN ls -lah

# Copia arquivos de configuração do Maven
COPY ./src ./src
COPY ./.env ./.env
COPY ./logback.xml ./logback.xml

# Compila e gera o JAR
RUN mvn clean package -DskipTests -q

# Lista o conteudo   # carrega variáveis do arquivo .env
RUN ls -lah target


FROM ghcr.io/retrodaredevil/lavender-jre:21-ubuntu-noble AS runtime
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
RUN chmod +x app.jar

ARG SPRING_DATASOURCE_URL
ARG SPRING_DATASOURCE_USERNAME
ARG SPRING_DATASOURCE_PASSWORD
ENV SPRING_PROFILES_ACTIVE=docker
ENV SPRING_DATASOURCE_URL=$SPRING_DATASOURCE_URL
ENV SPRING_DATASOURCE_USERNAME=$SPRING_DATASOURCE_USERNAME
ENV SPRING_DATASOURCE_PASSWORD=$SPRING_DATASOURCE_PASSWORD
ENV INSTANCE_ID=$HOSTNAME
ENV OTEL_SERVICE_NAME=ms-offers-importer
ENV OTEL_RESOURCE_ATTRIBUTES="service.name=ms-offers-importer,service.instance.id=$(hostname)"
ENV OTEL_EXPORTER_OTLP_ENDPOINT=http://192.168.18.135:4318
ENV OTEL_EXPORTER_OTLP_LOGS_ENDPOINT=http://192.168.18.135:4318/v1/logs
ENV OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=http://192.168.18.135:4318/v1/traces
ENV OTEL_EXPORTER_OTLP_METRICS_ENDPOINT=http://192.168.18.135:4318/v1/metrics
ENV LOGS_ENDPOINT=http://192.168.18.135:4318/v1/logs
ENV METRICS_ENDPOINT=http://192.168.18.135:4318/v1/metrics
ENV SPANS_ENDPOINT=http://192.168.18.135:4318/v1/spans


EXPOSE 8082

# Comando padrão
ENV JAVA_OPTS="-XX:+UseSerialGC -Xms64m -Xmx128m -XX:+UseContainerSupport -XX:MaxRAMPercentage=70 -XX:+AlwaysPreTouch -XX:-UseAdaptiveSizePolicy -XX:+DisableExplicitGC"
CMD java $JAVA_OPTS -jar app.jar $LOGS_ENDPOINT $METRICS_ENDPOINT $SPANS_ENDPOINT

```
---

## 📚 Referências

- [OpenTelemetry](https://opentelemetry.io/)  
- [Maven](https://maven.apache.org/)  
- [Docker](https://docs.docker.com/)  
- Lavender JRE Image [(ghcr.io in Bing)](https://www.bing.com/search?q="https%3A%2F%2Fghcr.io%2Fretrodaredevil%2Flavender-jre")





--- 
# dockerfile-for-nodejs-on-armv7

# 🚀 Aplicação Node.js com Docker (32 bits)

Este projeto demonstra como construir e executar uma aplicação **Node.js** em um container Docker utilizando a imagem oficial [`arm32v7/node`](https://hub.docker.com/_/node).  
A aplicação é servida com o pacote [`serve`](https://www.npmjs.com/package/serve) na porta **5173**.

---

## 📦 Etapas do Dockerfile

1. **Imagem base**
   - Usa `arm32v7/node:22.21.1-bookworm` (compatível com sistemas 32 bits).
   - Alternativa comentada: `node:trixie-slim`.

2. **Diretório de trabalho**
   ```dockerfile
   WORKDIR /app
   ```

3. **Cópia de arquivos de configuração**
   - `package.json`
   - `package-lock.json`
   - `.env`

4. **Instalação de dependências**
   - Instala pacotes do projeto com `npm install`.
   - Limpa cache do npm.
   - Instala globalmente o `serve`.

5. **Verificação de versões**
   - Exibe versões do Node, npm e serve.

6. **Cópia do restante do código**
   - Executa `npm run clean` e `npm run build`.

7. **Exposição da porta**
   ```dockerfile
   EXPOSE 5173
   ```

8. **Comando padrão**
   ```dockerfile
   CMD ["serve", "-s", "dist", "-l", "5173"]
   ```

---


## ▶️ Como usar

### 1. Construir a imagem
```dockerfile
docker build -t minha-app-node .
```

### 2. Executar o container
```dockerfile
docker run -p 5173:5173 minha-app-node
```

### 3. Acessar a aplicação
Abra no navegador:
```
http://localhost:5173
```

---

## 🛠️ Scripts disponíveis

- `npm run clean` → limpa arquivos temporários.
- `npm run build` → gera a versão de produção da aplicação.

---

## 📌 Observações

- Este Dockerfile foi projetado para **arquitetura ARM 32 bits**.  
- Se estiver em ambiente **x86_64**, altere a imagem base para:
  ```dockerfile
  FROM node:trixie-slim
  ```
- O servidor `serve` é usado apenas para servir os arquivos estáticos da pasta `dist`.

---

```dockerfile
# Etapa 1: Build da aplicação
# https://hub.docker.com/_/node
# PARA 32 BITS
FROM arm32v7/node:22.21.1-bookworm
#FROM node:trixie-slim

# Define diretório de trabalho
WORKDIR /app

# Copia arquivos de configuração
COPY package.json package-lock.json .
COPY .env .

# Instala dependências
RUN npm install && npm cache clean --force
RUN npm install -g serve
RUN node -v
RUN npm -v
RUN serve -v

RUN ls -la

# Copia o restante do código
COPY . .
RUN npm run clean
RUN npm run build

EXPOSE 5173

# Porta padrão
CMD ["serve", "-s", "dist", "-l", "5173"]

```


---

## 📚 Referências

- [Node.js Docker Official Images](https://hub.docker.com/_/node)  
- [Serve - npm](https://www.npmjs.com/package/serve)  
- [Documentação do Docker](https://docs.docker.com/)



---

## 🐞 Encontrou um Bug?

Se você encontrou algum erro, comportamento inesperado ou tem sugestões de melhoria, sua contribuição é muito bem-vinda!

### Como reportar um problema

1. Acesse a seção de Issues
2. Clique em **"New Issue"**.
3. Descreva detalhadamente:
   - O que você estava tentando fazer
   - O que aconteceu de errado
   - Passos para reproduzir o problema
   - Prints de tela ou logs, se possível

Agradecemos pelo seu apoio e colaboração 💙

---
- ⚠️ A formatação deste markdown foi feito com apoio do Copilot
- ⚠️ Os links dos produtos sao monetizados
