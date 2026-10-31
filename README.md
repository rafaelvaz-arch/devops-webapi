## Laboratório Docker
Documentação e guia de execução de comandos para a aplicação devops-webapi.

## Construção
Gere a imagem Docker inicial (versão 1.0.0) a partir da raiz do projeto:

Bash
docker build -t devops-webapi:1.0.0 .
## Execução
Inicie um contêiner individual para testes locais mapeando a porta 8080:

Bash
docker run -d -p 8080:8080 --name webapi devops-webapi:1.0.0

## Staging
Execute o ambiente de staging na porta 8081 com as variáveis de ambiente:   
image_523a37.png

Bash
docker run -d -p 8081:8080 \
  --name devops-api-staging \
  -e APP_ENVIRONMENT=staging \
  -e APP_VERSION="1.1.0" \
  devops-webapi:1.1.0

## Production
Execute o ambiente de produção na porta 8082 com as variáveis de ambiente:   
image_523a37.png

Bash
docker run -d -p 8082:8080 \
  --name devops-api-prod \
  -e APP_ENVIRONMENT=production \
  -e APP_VERSION="1.1.0" \
  devops-webapi:1.1.0

## Health Check
Valide a resposta do endpoint de saúde da aplicação:

Bash
curl -i http://localhost:8080/health
Nova versão
Gere a build de atualização para a versão 1.1.0:

Bash
docker build -t devops-webapi:1.1.0 .

## Falha controlada
Gere e execute a versão 1.2.0 (simulação didática de falha com retorno 500 Internal Server Error no endpoint /health):

Bash
docker build -t devops-webapi:1.2.0 .
docker run -d -p 8080:8080 --name webapi-failed devops-webapi:1.2.0

## Rollback
Pare e remova o contêiner com falha e restabeleça a execução na versão estável 1.1.0:

Bash
docker stop webapi-failed
docker rm webapi-failed
docker run -d -p 8080:8080 --name webapi-stable devops-webapi:1.1.0

## Docker Compose
Gerenciamento unificado dos ambientes (staging e production) via compose.yaml:   

Subir as aplicações em segundo plano:

Bash
docker compose up -d
Listar contêineres em execução:

Bash
docker compose ps
Encerrar e remover os serviços:

Bash
docker compose down