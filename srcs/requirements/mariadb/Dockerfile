# Usamos a penúltima versão estável do Debian (Bookworm) como imagem base.
FROM debian:bookworm

# Atualizamos a lista de pacotes e instalamos o servidor MariaDB e o cliente.
RUN apt-get update && \
    apt-get install -y mariadb-server mariadb-client && \
    rm -rf /var/lib/apt/lists/*

# Copiamos nosso script de configuração para dentro da imagem.
COPY tools/setup.sql /docker-entrypoint-initdb.d/

# Expomos a porta padrão do MariaDB.
EXPOSE 3306

# O comando padrão para iniciar o servidor MariaDB.
CMD ["mysqld_safe"]
