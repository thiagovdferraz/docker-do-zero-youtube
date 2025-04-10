# Criando uma API para acesso a dados usando FastAPI em um contêiner Docker.

## Criar o arquivo requirements.txt
```bash
echo "fastapi==0.115.12
uvicorn==0.34.0
pandas==2.2.3
pydantic==2.11.1" > requirements.txt
```

## Construir a imagem Docker
```bash
docker build -t api-vendas .
```

## Executar o contêiner
```bash
docker run -p 8000:8000 -d --name fastapi-app api-vendas
```

# Testar a API

Acesse http://localhost:8000/docs para ver a documentação interativa

Benefícios neste exemplo:

* API de dados pronta para consumo por outras aplicações
* Documentação interativa automática com Swagger UI
* Isolamento de dependências e versões específicas
* Fácil implantação em qualquer ambiente que tenha Docker