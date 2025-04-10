# Combinando PostgreSQL, FastAPI e Streamlit em uma stack completa de análise de dados usando Docker Compose

## 1 Estrutura de diretórios (opcional, pode criar a estrutura manualmente)
```bash
mkdir -p projeto-dados/{api,dashboard}
```

## 2 Criar script de inicialização do banco
```sql
cat > init.sql << 'EOF'
CREATE TABLE vendas (
    id SERIAL PRIMARY KEY,
    data_venda DATE NOT NULL,
    produto VARCHAR(100) NOT NULL,
    categoria VARCHAR(50) NOT NULL,
    valor DECIMAL(10,2) NOT NULL,
    quantidade INT NOT NULL
);

INSERT INTO vendas (data_venda, produto, categoria, valor, quantidade)
VALUES 
  ('2025-01-15', 'Curso Python', 'Educação', 197.00, 45),
  ('2025-01-20', 'Assinatura BI', 'Software', 89.90, 28),
  ('2025-02-05', 'Consultoria', 'Serviços', 1200.00, 3),
  ('2025-02-12', 'Curso SQL', 'Educação', 147.00, 52),
  ('2025-03-01', 'Licença Tableau', 'Software', 999.00, 15);
EOF
```

## 3 Criar Dockerfile para a API
```docker
cat > api/Dockerfile << 'EOF'
FROM python:3.12-slim

WORKDIR /app

# Copiar e instalar dependências
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar código
COPY . .

# Expor porta para a API
EXPOSE 8000

# Iniciar API com Uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
EOF

# Requisitos para a API
cat > api/requirements.txt << 'EOF'
fastapi==0.115.12
uvicorn==0.34.0
pydantic==2.11.1
sqlalchemy==2.0.22
psycopg2-binary==2.9.9
EOF
```

## 4 Criar Dockerfile para o Dashboard
```docker
cat > dashboard/Dockerfile << 'EOF'
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["streamlit", "run", "app.py", "--server.address=0.0.0.0"]
EOF

# Requisitos para o Dashboard
cat > dashboard/requirements.txt << 'EOF'
streamlit==1.44.0
pandas==2.2.3
plotly==5.18.0
requests==2.31.0
EOF
```

## 5 Iniciar todos os serviços
```bash
docker-compose up -d
```

## 6 Verificar logs dos serviços
```bash
docker-compose logs -f
```

## 7 Parar todos os serviços
```bash
docker-compose down
```

## 8 Parar e remover volumes (limpar dados)
```bash
docker-compose down -v
```

# Benefícios neste exemplo

* Stack completa de análise de dados orquestrada com um único comando
* Comunicação entre serviços usando a rede interna do Docker
* Fluxo de dados bidirecional: inserção de dados via dashboard → API → banco de dados
* Atualização em tempo real dos gráficos após inserção de novos dados
* Persistência de dados com volumes Docker
* Pipeline de dados completo com feedback circular

# Fluxo completo de dados:

* Usuário insere dados no dashboard Streamlit
* Dashboard envia dados para a API FastAPI
* API persiste dados no PostgreSQL
* Dashboard consulta API para atualizar visualizações
* API obtém dados do PostgreSQL