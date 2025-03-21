# **API Django - Taxa Selic**

## *Sumário*
- *📌 Visão Geral*
- *⚙️ Configuração e Instalação*
- *🚀 Executando o Projeto*
- *🔑 Autenticação JWT*
- *📡 Coletar dados da API*
- *🖥️ Rodar o servidor Django*
- *🛠️ Comandos úteis*
- *🧪 Testes da API Taxa Selic*
- *📊 Resultados dos Testes*

## **Visão Geral**
Este projeto é uma API desenvolvida em Django para buscar e armazenar as taxas Selic em um banco de dados MySQL. A aplicação utiliza **Docker** e **Docker Compose** para facilitar a configuração e execução do ambiente, garantindo portabilidade e escalabilidade.

A API permite a recuperação das taxas Selic a partir de uma fonte externa e armazena os dados no banco para consultas futuras.

---

## ⚙️ **Configuração e Instalação**

### **1️⃣ Pré-requisitos**
Antes de iniciar, certifique-se de ter instalado:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/downloads)

---

### **2️⃣ Clonar o repositório**
Para obter o código-fonte do projeto, execute:

```sh
git clone https://github.com/TN-Junior/api-django.git

cd desafio_python # arquivos de configuração do django
cd selic # arquivo coma lógica de extração de dados da api
```

### **3️⃣ Configurar as variáveis de ambiente**
Crie um arquivo .env (se necessário) ou edite o settings.py para garantir que as credenciais do banco de dados estejam corretas.

Exemplo de configuração do banco de dados (settings.py):
```sh
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER': 'django_user',
        'PASSWORD': 'django_password',
        'HOST': 'db',  
        'PORT': '3306',
    }
}
```
## **Executando o Projeto**
### **1️⃣ Subir os containers Docker**
Para iniciar o banco de dados e o Django dentro do ambiente Docker, rode:
```sh
docker-compose up -d
```
Isso iniciará os serviços:

- db: Banco de dados MySQL

- web: Aplicação Django

### **2️⃣ Aplicar migrações do banco**
Após iniciar os containers, aplique as migrações para criar as tabelas no MySQL:
```sh
docker-compose exec web python manage.py migrate
```
Se necessário, crie um superusuário para acessar o Django Admin:
```sh
docker-compose exec web python manage.py createsuperuser
```
## 🔑 Autenticação JWT no Postman
Após criar o superusuário, você pode obter o token de autenticação JWT no Postman para acessar as rotas protegidas da API.

**1️⃣ Acesse a rota de obtenção do token**
- Abra o Postman.
- Selecione o método POST.
- No campo de URL, insira:
```bash
http://127.0.0.1:8000/api/token/
```
- Vá até a aba Body, selecione raw e escolha JSON.
- Insira o seguinte JSON no corpo da requisição:
```bash
{
    "username": "admin",
    "password": "admin123"
}
```
**2️⃣ Enviar a requisição**
- Clique no botão Send.
- Se as credenciais estiverem corretas, você receberá uma resposta semelhante a esta:
```bash
{
    "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```
**3️⃣ Utilizar o token para acessar rotas protegidas**
- Copie o valor de "access" (token de acesso).
- No Postman, vá até a aba Authorization.
- Selecione o tipo Bearer Token.
- Cole o token copiado no campo.
```bash
Authorization: Bearer "token"
```
### **Criando uma nova taxa Selic (POST)**
1. Escolha o método POST.
2. Digite a URL: http://127.0.0.1:8000/api/selic/
3. Acesse a aba "Headers" e adicione:
- Content-Type: application/json
4. Acesse a aba "Body", selecione raw e insira o JSON:
```bash
{
  "data": "2025-03-18",
  "valor": "13.21"
}
```
5. Clique em "Send".

### **Atualizando uma taxa Selic (PUT)**
1. Escolha o método PUT.
2. Digite a URL: http://127.0.0.1:8000/api/selic/9726/
3. Acesse a aba "Headers" e adicione:
- Content-Type: application/json
4. Acesse a aba "Body", selecione raw e insira o JSON:
```bash
{
  "data": "2025-03-18",
  "valor": "13.50"
}
```
5. Clique em "Send".

### **Deletando uma taxa Selic (DELETE)**
1. Escolha o método DELETE.
2. Digite a URL: http://127.0.0.1:8000/api/selic/9726/  

4. Clique em "Send".


### 🔄 Atualizando o Token de Autenticação JWT no Postman
Se o token de acesso (access) expirar, você pode obter um novo sem precisar refazer o login, usando o refresh token.

**🔹 Passo 1: Abrir o Postman**
Abra o Postman e selecione a opção "New Request".

**🔹 Passo 2: Configurar a Requisição**
1. Método: POST
2. URL da API:
```bash
http://127.0.0.1:8000/api/token/refresh/
```
3. Cabeçalhos (Headers):
- Content-Type: application/json

**🔹 Passo 3: Enviar o Refresh Token**
Na aba Body, selecione raw e adicione o seguinte JSON:
```bash
{
    "refresh": "SEU_REFRESH_TOKEN_AQUI"
}
```
Em seguida, clique no botão "Send".

**🔹 Passo 4: Receber um Novo Token**
Se o refresh token for válido, a API retornará uma nova resposta contendo um novo token de acesso:
```bash
{
    "access": "NOVO_ACCESS_TOKEN_AQUI"
}
```
Agora, utilize esse novo token access para continuar autenticado na API.




Esses comandos permitem manipular os registros da API de forma segura, garantindo que apenas usuários autorizados realizem modificações.


## **Coletar dados da API**
Para buscar as taxas Selic e armazená-las no banco de dados, execute:
```sh
docker-compose exec web python manage.py fetch_selic
```

## **Rodar o servidor Django**
Agora, inicie o servidor da API:
```sh
docker-compose exec web python manage.py runserver 0.0.0.0:8000
```
Acesse a API no navegador:
http://localhost:8000


## Comandos úteis
Se precisar parar os containers:
```sh
docker-compose down
```
Para limpar os volumes (⚠️ Isso apagará os dados do banco):
```sh
docker-compose down -v
```
✅ 1. Verifica se o superuser existe no banco de dados, execute:
```bash
docker-compose exec web python manage.py shell
````
E dentro do shell do Django, tente encontrar o usuário:
```bash
from django.contrib.auth import get_user_model
User = get_user_model()
User.objects.all()
```
Se não houver usuários listados, crie um usuário admin:
```bash
User.objects.create_superuser('admin', 'admin@example.com', 'admin123')
```

✅ 2. Verifique se o usuário está ativo
Mesmo que o usuário exista, ele pode estar inativo. Para verificar:
```bash
user = User.objects.get(username="admin")
print(user.is_active)
```
Se False, ative o usuário:
```bash
user.is_active = True
user.save()
```
Agora tente autenticar novamente.

## Testes da API Taxa Selic

Este repositório contém testes automatizados para a API de Taxa Selic, utilizando Django REST Framework.

## Estrutura dos Testes
Os testes estão divididos em três arquivos:
- `test_models.py`: Valida o modelo `SelicRate`.
- `test_permissions.py`: Testa permissões de usuários na API.
- `test_views.py`: Verifica as operações da API (CRUD).

## Como Executar os Testes
```sh
# Executar todos os testes
docker-compose exec web python manage.py test

# Executar testes específicos
docker-compose exec web python manage.py test selic.tests.test_models
docker-compose exec web python manage.py test selic.tests.test_permissions
docker-compose exec web python manage.py test selic.tests.test_views
```

## Resultados dos Testes
### 1. Testes de Modelos (`test_models.py`)
- ✅ Criação de taxa Selic.
- ✅ Restrição de unicidade para datas.
- ✅ Representação em string correta.

### 2. Testes de Permissões (`test_permissions.py`)
- ✅ Usuário comum não pode deletar taxa.
- ✅ Administrador pode deletar taxa.

### 3. Testes de Views (`test_views.py`)
- ✅ Listagem das taxas.
- ✅ Bloqueio de criação sem autenticação.
- ✅ Criação permitida para usuários autenticados.
- ✅ Bloqueio de exclusão por usuários não autenticados.
- ✅ Exclusão permitida para administradores.

## Imagens dos Testes
Resultados capturados:
![Testes de Views](https://github.com/user-attachments/assets/f1cebda5-962c-4b70-957d-ceec0be0713a)

![Testes de Modelos](https://github.com/user-attachments/assets/7b0b7087-8200-4151-bc64-f045fa19d5fd)

![Testes de Permissões](https://github.com/user-attachments/assets/a1b30a28-04d3-4b13-98b0-41911d6489d6)


Os testes foram bem-sucedidos, garantindo que a API funciona conforme esperado.


