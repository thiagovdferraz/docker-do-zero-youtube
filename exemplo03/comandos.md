# PostgreSQL no Docker

## Iniciar contêiner PostgreSQL
```bash
docker run --name postgres-dados -e POSTGRES_PASSWORD=segredo -e POSTGRES_USER=analista -e POSTGRES_DB=datawarehouse -p 5432:5432 -d postgres:14.8
```

## Verificar status do contêiner
```bash
docker ps
```
## Executar o cliente psql dentro do contêiner iteraqtivamente
```bash
docker exec -it postgres-dados psql -U analista -d datawarehouse
```

No exemplo isolado, o banco começa vazio

No Docker Compose, será preenchido com dados iniciais e através da API

# Parar o contêiner
```bash
docker stop postgres-dados
```

# Reiniciar o contêiner
```bash
docker start postgres-dados
```

# Benefícios neste exemplo:

* Banco de dados para análise pronto para uso em segundos
* Persistência de dados com volumes Docker
* Isolamento completo do sistema host
* Fácil backup e restauração
* Possibilidade de executar múltiplas versões do PostgreSQL no mesmo sistema

# To Do
## Executar PostgreSQL com volume para persistência
```bash
docker run --name postgres-dados \
  -e POSTGRES_PASSWORD=segredo \
  -e POSTGRES_USER=analista \
  -e POSTGRES_DB=datawarehouse \
  -v postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  -d postgres:14.8
```

Conexão externa

Acessar usando ferramentas externas

Host: localhost

Port: 5432

User: analista

Password: segredo

Database: datawarehouse

# Backup e restauração
## Criar backup do banco de dados
```bash
docker exec -t postgres-dados pg_dump -U analista -d datawarehouse > backup.sql
```

# Restaurar backup (para um contêiner novo)
## 1. Criar novo contêiner
```bash
docker run --name postgres-dados-novo -e POSTGRES_PASSWORD=segredo -e POSTGRES_USER=analista -e POSTGRES_DB=datawarehouse -v postgres_data_novo:/var/lib/postgresql/data -p 5433:5432 -d postgres:14.8
```

## 2. Restaurar dados
```bash
cat backup.sql | docker exec -i postgres-dados-novo psql -U analista -d datawarehouse
```