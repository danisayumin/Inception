# Exemplos de Teste - Inception

Este arquivo contém exemplos de testes para a avaliação do projeto Inception, com base no `roteiro_de_avaliacao.md`.

## Testes Preliminares

### 1. Limpeza do Ambiente Docker

**Comando:**
```bash
docker stop $(docker ps -qa); docker rm $(docker ps -qa); docker rmi -f $(docker images -qa); docker volume rm $(docker volume ls -q); docker network rm $(docker network ls -q) 2>/dev/null
```
**Verificação:**
*   O comando deve ser executado sem erros.
*   **Resultado Esperado:** Um ambiente Docker limpo, sem contêineres, imagens, volumes ou redes ativas.

### 2. Verificação do `docker-compose.yml`

**Comandos:**
```bash
grep "network: host" srcs/docker-compose.yml
grep "links:" srcs/docker-compose.yml
grep "network(s):" srcs/docker-compose.yml
```
**Verificação:**
*   Os dois primeiros comandos não devem retornar nenhuma saída.
*   O terceiro comando deve retornar a configuração de redes.
*   **Resultado Esperado:** O arquivo `docker-compose.yml` não deve conter `network: host` ou `links:`, mas deve conter a seção `networks`.

### 3. Verificação de Scripts e `Dockerfile`

**Comandos:**
```bash
grep -r -- "--link" .
grep -r "tail -f" .
grep -r "sleep infinity" .
```
**Verificação:**
*   Nenhum dos comandos acima deve retornar nenhuma saída.
*   **Resultado Esperado:** Nenhum script ou `Dockerfile` deve conter `--link`, `tail -f`, ou `sleep infinity`.

## Testes de Funcionalidade

### 1. Execução do `Makefile`

**Comando:**
```bash
make
```
**Verificação:**
*   O comando deve ser executado sem erros.
*   Todos os serviços (NGINX, WordPress, MariaDB) devem ser construídos e iniciados corretamente.
*   **Resultado Esperado:** A aplicação deve estar em execução após a conclusão do comando.

### 2. Acesso ao Site

**Ação:**
*   Abra o navegador e acesse `http://<login>.42.fr`.
*   Abra o navegador e acesse `https://<login>.42.fr`.

**Verificação:**
*   O acesso via `http` deve falhar ou ser redirecionado para `https`.
*   O acesso via `https` deve exibir a página inicial do WordPress, sem a tela de instalação.
*   Pode haver um aviso de segurança sobre o certificado autoassinado, o que é esperado.
*   **Resultado Esperado:** O site WordPress deve estar acessível apenas via HTTPS.

### 3. Teste de Persistência de Dados

**Ação:**
1.  Acesse o painel de administração do WordPress em `https://<login>.42.fr/wp-admin`.
2.  Crie um novo post ou edite uma página existente.
3.  Adicione um comentário em um post.
4.  Reinicie a máquina virtual.
5.  Execute `make` novamente.
6.  Acesse o site novamente.

**Verificação:**
*   O post criado ou a página editada deve permanecer.
*   O comentário adicionado deve estar visível.
*   **Resultado Esperado:** Os dados do WordPress e do MariaDB devem ser persistentes após a reinicialização.

### 4. Verificação dos Volumes Docker

**Comandos:**
```bash
docker volume ls
docker volume inspect <nome_do_volume_wordpress>
docker volume inspect <nome_do_volume_mariadb>
```
**Verificação:**
*   O comando `docker volume ls` deve listar os volumes criados para o WordPress e o MariaDB.
*   A inspeção de cada volume deve mostrar o `Mountpoint` apontando para um diretório dentro de `/home/<login>/data/`.
*   **Resultado Esperado:** Os volumes devem estar corretamente configurados e mapeados para os diretórios de dados do usuário.

### 5. Verificação da Rede Docker

**Comando:**
```bash
docker network ls
```
**Verificação:**
*   O comando deve listar a rede criada pelo `docker-compose`.
*   **Resultado Esperado:** Uma rede customizada para a aplicação deve existir.
