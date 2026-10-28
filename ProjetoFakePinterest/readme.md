# FakePinterest

Clone simplificado do Pinterest feito com **Flask**. Os usuários podem criar uma conta, fazer login, enviar fotos para o próprio perfil, ver o perfil de outras pessoas e navegar por um feed com todas as fotos publicadas.

## Funcionalidades

- Cadastro de usuários com validação de formulário (e-mail, senha de 6 a 20 caracteres e confirmação de senha)
- Login e logout com sessão gerenciada pelo Flask-Login
- Senhas armazenadas com hash (Bcrypt)
- Upload de fotos no próprio perfil
- Visualização do perfil de outros usuários (somente leitura)
- Feed com todas as fotos, da mais recente para a mais antiga; clicar em uma foto leva ao perfil de quem postou
- Proteção CSRF nos formulários (Flask-WTF)

## Tecnologias

- Python 3.9+
- [Flask](https://flask.palletsprojects.com/)
- Flask-SQLAlchemy (banco SQLite)
- Flask-Login
- Flask-Bcrypt
- Flask-WTF / WTForms
- email-validator (exigido pelo validador `Email()` do WTForms)
- HTML, CSS e Jinja2

## Estrutura do projeto

```
projeto/
├── main.py                  # Ponto de entrada da aplicação
├── criar_banco.py           # Script que cria as tabelas do banco
└── fakepinterest/
    ├── __init__.py          # Configuração do app, banco, bcrypt e login manager
    ├── models.py            # Modelos Usuario e Foto
    ├── forms.py             # FormLogin, FormCriarConta e FormFoto
    ├── routes.py            # Rotas da aplicação
    ├── templates/
    │   ├── homepage.html    # Página de login (template base)
    │   ├── criarconta.html  # Página de criação de conta
    │   ├── perfil.html      # Perfil do usuário e upload de fotos
    │   ├── feed.html        # Feed de fotos
    │   └── navbar.html      # Barra de navegação para usuários logados
    └── static/
        ├── css/style.css
        ├── fotos_site/      # Imagens do site (ex.: fundo-login.png)
        └── fotos_posts/     # Fotos enviadas pelos usuários
```

> Os arquivos `main.py` e `criar_banco.py` ficam na raiz, fora da pasta `fakepinterest`.

## Como executar

### 1. Clonar o repositório

```bash
git clone <url-do-repositorio>
cd <nome-da-pasta>
```

### 2. Criar e ativar um ambiente virtual (recomendado)

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux / macOS
source venv/bin/activate
```

### 3. Instalar as dependências

```bash
pip install flask flask-sqlalchemy flask-login flask-bcrypt flask-wtf email-validator
```

### 4. Garantir que as pastas de imagens existem

```bash
mkdir -p fakepinterest/static/fotos_posts
mkdir -p fakepinterest/static/fotos_site
```

Coloque a imagem de fundo da tela de login em `fakepinterest/static/fotos_site/fundo-login.png`.

### 5. Criar o banco de dados

```bash
python criar_banco.py
```

Isso cria o arquivo `comunidade.db` (SQLite) com as tabelas `usuario` e `foto`. Dependendo da versão do Flask-SQLAlchemy, ele pode ficar dentro da pasta `instance/`.

### 6. Iniciar o servidor

```bash
python main.py
```

Acesse [http://127.0.0.1:5000](http://127.0.0.1:5000) no navegador.

## Rotas

| Rota | Métodos | Acesso | Descrição |
|------|---------|--------|-----------|
| `/` | GET, POST | Público | Página inicial com formulário de login |
| `/criarconta` | GET, POST | Público | Criação de conta |
| `/perfil/<id_usuario>` | GET, POST | Logado | Perfil do usuário; no próprio perfil é possível enviar fotos |
| `/feed` | GET | Logado | Feed com todas as fotos |
| `/logout` | GET | Logado | Encerra a sessão |

## Modelo de dados

**Usuario**

| Campo | Tipo | Observações |
|-------|------|-------------|
| id | Integer | Chave primária |
| username | String | Obrigatório |
| email | String | Obrigatório e único |
| senha | String | Hash Bcrypt |
| fotos | Relationship | Fotos do usuário |

**Foto**

| Campo | Tipo | Observações |
|-------|------|-------------|
| id | Integer | Chave primária |
| imagem | String | Nome do arquivo (padrão: `default.png`) |
| data_criacao | DateTime | Data de envio |
| id_usuario | Integer | Chave estrangeira para `usuario.id` |

## Melhorias sugeridas

Pontos que valem atenção caso o projeto evolua além de um estudo:

- **Segurança:** mover a `SECRET_KEY` para uma variável de ambiente em vez de deixá-la no código.
- **Validação de e-mail no login:** o método `validate_email` do `FormLogin` verifica se o e-mail já existe e usa `return ValidationError(...)` em vez de `raise`, então ele não tem efeito. Essa validação faz sentido no `FormCriarConta` (com `raise`), não no login.
- **Data de criação:** em `models.py`, use `default=datetime.utcnow` (sem parênteses). Com parênteses, a data é calculada uma única vez, quando o servidor inicia.
- **Upload de arquivos:** validar a extensão (ex.: `FileAllowed` do Flask-WTF) e evitar sobrescrever arquivos com o mesmo nome (por exemplo, adicionando um identificador único ao nome).
- **Logout:** usar POST em vez de GET para encerrar a sessão.
- **Limpeza de código:** remover imports não utilizados (`from sqlalchemy.engine import default` em `models.py` e `Flask` em `routes.py`).

## Licença

Projeto de estudo, sem licença definida. Adicione uma (por exemplo, MIT) se for publicá-lo.
