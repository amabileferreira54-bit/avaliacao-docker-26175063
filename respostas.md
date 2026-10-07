# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Amabile Vitoria Albino Ferreira
Matrícula: 26175063
Usuário do GitHub: amabileferreira54-bit
Usuário do Docker Hub: ferreiramabile

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?

   Usei a imagem base `nginx:1.25-alpine`. O tamanho final da imagem gerada (`ferreiramabile/viaserra-portal:1.0-26175063`) é de aproximadamente 42.5 MB.

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.

   O Nginx procura os arquivos na pasta `/usr/share/nginx/html`.
   Comando utilizado para conferir:
   `docker run --rm ferreiramabile/viaserra-portal:1.0-26175063 ls -la /usr/share/nginx/html`

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.

   Nome completo da imagem: `ferreiramabile/viaserra-portal:1.0-26175063`
   Link público no Docker Hub: `https://hub.docker.com/r/ferreiramabile/viaserra-portal`

4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?

   É necessário refazer o build da imagem e enviar a nova versão para o repositório:
   1. `docker build -t ferreiramabile/viaserra-portal:1.0-26175063 ./portal`
   2. `docker push ferreiramabile/viaserra-portal:1.0-26175063`

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | `WORKDIR /usr/share/nginx` | Faltava a pasta `/html` no caminho padrão do Nginx. | Os arquivos seriam salvos fora do diretório padrão do site. | Alterado para `WORKDIR /usr/share/nginx/html`. |
| 2 | `COPY pagina/ .` | A pasta chamava-se `site/` e não `pagina/`. | O build falhou com o erro `ERROR: "/pagina": not found`. | Alterado para `COPY site/ .`. |
| 3 | `CMD ["nginx"]` | Faltava o parâmetro `-g "daemon off;"` para manter o Nginx rodando em primeiro plano. | O container finalizava imediatamente após iniciar. | Alterado para `CMD ["nginx", "-g", "daemon off;"]`. |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

A sintaxe utilizada é `-p <porta_do_host>:<porta_do_container>`.
- `-p 7042:80`: Mapeia a porta 7042 da sua máquina (host) para a porta 80 do container.
- `-p 80:7042`: Mapeia a porta 80 da sua máquina (host) para a porta 7042 do container.

O segundo número (à direita dos dois pontos) é sempre a porta do container. Em `-p 7042:80`, a porta do container é a **80**.

## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.

Comandos equivalentes:
- Portal:
  `docker run -d -p 8063:80 --restart unless-stopped ferreiramabile/viaserra-portal:1.0-26175063`
- Manutenção:
  `docker run -d -p 7063:80 --restart unless-stopped ferreiramabile/viaserra-manutencao:1.0-26175063`

8. Qual comando derruba os dois containers de uma vez?

O comando é:
`docker compose down`

## Verificador

9. Cole aqui o código de verificação gerado pelo script.

`VIASERRA-26175063-5653E282`
