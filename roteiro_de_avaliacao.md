# Roteiro de Avaliação - Inception

Este roteiro de avaliação foi criado com base no arquivo `regua.md` e tem como objetivo guiar a avaliação do projeto Inception.

## Instruções Gerais

1.  **Conhecimento do Avaliado:** Durante todo o processo de avaliação, caso você não saiba como verificar um requisito, o aluno avaliado deverá auxiliá-lo.
2.  **Estrutura de Arquivos:** Garanta que todos os arquivos necessários para configurar a aplicação estejam localizados dentro de uma pasta `srcs`. A pasta `srcs` deve estar na raiz do repositório.
3.  **Makefile:** Garanta que um `Makefile` esteja localizado na raiz do repositório.
4.  **Limpeza do Ambiente:** Antes de iniciar a avaliação, execute o seguinte comando no terminal:
    ```bash
    docker stop $(docker ps -qa); docker rm $(docker ps -qa); docker rmi -f $(docker images -qa); docker volume rm $(docker volume ls -q); docker network rm $(docker network ls -q) 2>/dev/null
    ```
5.  **Verificação do `docker-compose.yml`:**
    *   Leia o arquivo `docker-compose.yml`. Não deve haver `network: host` ou `links:` nele. Caso contrário, a avaliação termina imediatamente.
    *   O arquivo `docker-compose.yml` deve conter `network(s)`. Caso contrário, a avaliação termina imediatamente.
6.  **Verificação de Scripts e Makefile:**
    *   Examine o `Makefile` e todos os scripts em que o Docker é usado. Nenhum deles deve conter `--link`. Caso contrário, a avaliação termina imediatamente.
7.  **Verificação dos Dockerfiles:**
    *   Examine os `Dockerfiles`. Nenhum deles deve conter `tail -f` ou qualquer comando rodando em segundo plano na seção `ENTRYPOINT`. Se houver, a avaliação termina imediatamente. O mesmo se aplica se `bash` ou `sh` forem usados, mas não para executar um script (por exemplo, `nginx & bash` ou `bash`).
    *   Os contêineres devem ser construídos a partir da penúltima versão estável do Alpine ou Debian.
    *   Se o `entrypoint` for um script (ex: `ENTRYPOINT ["sh", "my_entrypoint.sh"]`, `ENTRYPOINT ["bash", "my_entrypoint.sh"]`), garanta que ele não execute nenhum programa em segundo plano (ex: `nginx & bash`).
8.  **Verificação de Loops Infinitos:** Examine todos os scripts no repositório. Garanta que nenhum deles execute um loop infinito. Exemplos de comandos proibidos: `sleep infinity`, `tail -f /dev/null`, `tail -f /dev/random`.
9.  **Execução do Makefile:** Execute o `Makefile`.

## Parte Obrigatória

### Visão Geral do Projeto

O avaliado deve explicar os seguintes pontos em termos simples:

*   Como o Docker e o docker-compose funcionam.
*   A diferença entre uma imagem Docker usada com e sem o docker-compose.
*   O benefício do Docker em comparação com máquinas virtuais (VMs).
*   A relevância da estrutura de diretórios exigida para este projeto.

### Configuração Simples

1.  **Acesso ao NGINX:** Garanta que o NGINX esteja acessível apenas pela porta 443. Após a conclusão, abra a página.
2.  **Certificado SSL/TLS:** Garanta que um certificado SSL/TLS esteja sendo usado.
3.  **Site WordPress:** Garanta que o site WordPress esteja devidamente instalado e configurado; você não deve ver a página de instalação do WordPress. Para acessá-lo, abra `https://login.42.fr` em seu navegador, onde `login` é o nome de usuário do aluno avaliado. Você não deve conseguir acessar o site via `http://login.42.fr`. Se algo não funcionar como esperado, o processo de avaliação termina imediatamente.

### Conceitos Básicos de Docker

1.  **Dockerfiles:** Comece verificando os `Dockerfiles`. Deve haver um `Dockerfile` para cada serviço. Garanta que os `Dockerfiles` não estejam vazios. Se este não for o caso ou se um `Dockerfile` estiver faltando, o processo de avaliação termina imediatamente.
2.  **Imagens Docker:** Garanta que o aluno avaliado tenha escrito seus próprios `Dockerfiles` e construído suas próprias imagens Docker. É proibido o uso de imagens prontas ou de serviços como o DockerHub.
3.  **Versão da Imagem Base:** Garanta que cada contêiner seja construído a partir da penúltima versão estável do Alpine ou Debian. Se um `Dockerfile` não começar com `FROM alpine:X.X.X`, `FROM debian:XXXXX`, ou qualquer outra imagem local, o processo de avaliação termina imediatamente.
4.  **Nomes das Imagens:** As imagens Docker devem ter o mesmo nome do serviço correspondente. Caso contrário, o processo de avaliação termina imediatamente.
5.  **Makefile e Docker Compose:** Garanta que o `Makefile` configure todos os serviços via `docker-compose`. Isso significa que os contêineres devem ser construídos usando `docker-compose` e que não ocorram falhas. Caso contrário, a avaliação termina.

### Rede Docker

1.  **Uso de Rede Docker:** Garanta que a rede Docker seja usada, verificando o arquivo `docker-compose.yml`. Em seguida, execute o comando `docker network ls` para verificar se uma rede está visível.
2.  **Explicação:** O aluno avaliado deve fornecer uma explicação simples sobre a rede Docker. Se algum dos pontos acima não estiver correto, o processo de avaliação termina imediatamente.

### NGINX com SSL/TLS

1.  **Dockerfile:** Garanta que haja um `Dockerfile`.
2.  **Criação do Contêiner:** Usando o comando `docker-compose ps`, garanta que o contêiner seja criado (o uso da flag `-p` é permitido, se necessário).
3.  **Acesso HTTP:** Tente acessar o serviço via HTTP (porta 80) e verifique se você não consegue se conectar.
4.  **Acesso HTTPS:** Abra `https://login.42.fr/` em seu navegador, onde `login` é o login do aluno avaliado. A página exibida deve ser o site WordPress configurado (você não deve ver a página de instalação do WordPress).
5.  **Certificado TLS:** O uso de um certificado TLS v1.2/v1.3 é obrigatório e deve ser demonstrado. O certificado SSL/TLS não precisa ser reconhecido. Um aviso de certificado autoassinado pode aparecer. Se algum dos pontos acima não for claramente explicado e correto, o processo de avaliação termina imediatamente.

### WordPress com php-fpm e seu Volume

1.  **Dockerfile:** Garanta que haja um `Dockerfile`.
2.  **NGINX no Dockerfile:** Garanta que o NGINX não esteja incluído no `Dockerfile`.
3.  **Criação do Contêiner:** Usando o comando `docker-compose ps`, garanta que o contêiner foi criado (o uso da flag `-p` é autorizado, se necessário).
4.  **Volume:** Garanta que um volume esteja presente. Para fazer isso: execute o comando `docker volume ls` e depois `docker volume inspect <nome_do_volume>`. Verifique se a saída padrão contém o caminho `/home/login/data/`, onde `login` é o nome de usuário do aluno avaliado.
5.  **Comentários no WordPress:** Garanta que você pode adicionar um comentário usando a conta WordPress disponível.
6.  **Acesso ao Painel de Administração:** Faça login com a conta de administrador para acessar o painel de administração. O nome de usuário do administrador não deve incluir `admin` ou `Admin` (por exemplo, `admin`, `administrator`, `Admin-login`, `admin-123`, etc.).
7.  **Edição de Página:** No painel de administração, edite uma página. Verifique no site se a página foi atualizada. Se algum dos pontos acima não estiver correto, a avaliação termina agora.

### MariaDB e seu Volume

1.  **Dockerfile:** Garanta que haja um `Dockerfile`.
2.  **NGINX no Dockerfile:** Garanta que não haja NGINX no `Dockerfile`.
3.  **Criação do Contêiner:** Usando o comando `docker-compose ps`, garanta que o contêiner foi criado (o uso da flag `-p` é autorizado, se necessário).
4.  **Volume:** Garanta que haja um Volume. Para fazer isso: execute o comando `docker volume ls` e depois `docker volume inspect <nome_do_volume>`. Verifique se o resultado na saída padrão contém o caminho `/home/login/data/`, onde `login` é o nome de usuário do aluno avaliado.
5.  **Acesso ao Banco de Dados:** O aluno avaliado deve ser capaz de explicar como fazer login no banco de dados. Garanta que o banco de dados não esteja vazio. Se algum dos pontos acima não estiver correto, a avaliação termina agora.

### Persistência!

1.  **Reinicialização:** Você deve reiniciar a máquina virtual.
2.  **Verificação:** Assim que reiniciar, inicie o `docker-compose` novamente. Em seguida, verifique se tudo está funcional e se tanto o WordPress quanto o MariaDB estão configurados. As alterações que você fez anteriormente no site do WordPress ainda devem estar presentes. Se algum dos pontos acima não estiver correto, a avaliação termina agora.
