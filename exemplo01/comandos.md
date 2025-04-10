# Criar o arquivo requirements.txt no git bash
echo "streamlit==1.44.0
pandas==2.2.3
matplotlib==3.10.1" > requirements.txt

# Construir a imagem Docker
docker build -t dashboard-vendas .

# Executar o contêiner
docker run -p 8501:8501 -d --name streamlit-app dashboard-vendas

# Acessar o dashboard
# Abra seu navegador em http://localhost:8501

Benefícios neste exemplo:

Dashboard de visualização de dados pronto para uso em qualquer ambiente
Sem necessidade de instalar Python, Streamlit ou outras dependências no host
Portabilidade para compartilhar facilmente com a equipe
Ambiente isolado com versões específicas das bibliotecas