Sistema de Gestão de Diligências e Pessoas

Sistema web desenvolvido em Python e Django para o gerenciamento centralizado de cadastros de pessoas (físicas e jurídicas) e o controle completo do ciclo de vida de diligências, prazos, responsáveis e comprovantes.

Sumário

Visão Geral e Arquitetura

Stack Tecnológica

Roadmap e Etapas de Desenvolvimento

Como Executar o Projeto Localmente

Padrões e Convenções

Visão Geral e Arquitetura

O projeto segue o padrão arquitetural MVT (Model-View-Template) do Django, com isolamento de regras de negócio complexas em camadas de serviço (services.py), garantindo código limpo, testável e escalável.

├── core/                  # Configurações globais (settings, urls, wsgi/asgi)
├── accounts/              # Autenticação, perfis de usuário e controle de permissões
├── pessoas/               # App de gestão de PF/PJ, contatos, endereços e papéis
├── diligencias/           # App de criação, fluxo, prazos, histórico e anexos
├── templates/             # Templates HTML globais, layouts base e componentes
├── static/                # Arquivos estáticos (CSS, JS, ícones, imagens)
├── media/                 # Uploads de documentos e comprovantes (ignorado no Git)
├── .env.example           # Modelo de variáveis de ambiente
├── .gitignore             # Arquivos ignorados pelo controle de versão
├── manage.py
└── requirements.txt


Stack Tecnológica

Backend: Python 3.11+ & Django 5.x

Banco de Dados: SQLite (Desenvolvimento) / PostgreSQL (Produção)

Frontend: Django Templates + Bootstrap 5 / HTMX

Gerenciamento de Ambiente: venv & python-decouple

Qualidade de Código: PEP 8, Type Hints & Testes Automatizados (pytest / django.test)

Roadmap e Etapas de Desenvolvimento

Acompanhe abaixo o progresso de construção do sistema. Conforme cada entrega for validada, marque a caixa correspondente ([x]).

Fase 1: Setup e Infraestrutura Base

[ ] Inicializar repositório Git e configurar .gitignore padrão para Python/Django

[ ] Criar e ativar ambiente virtual (venv) e instalar o Django

[ ] Gerar estrutura base do projeto (core) e configurar requirements.txt

[ ] Configurar variáveis de ambiente (.env e .env.example) para SECRET_KEY, DEBUG e banco de dados

[ ] Configurar diretórios globais de templates/, static/ e media/ no settings.py

[ ] Estruturar template base (base.html) com barra de navegação, sidebar e mensagens de alerta (django.contrib.messages)

Fase 2: Autenticação e Controle de Acesso (accounts)

[ ] Criar app accounts e configurar modelo de usuário (padrão ou customizado)

[ ] Implementar telas de Login, Logout e Recuperação de Senha

[ ] Configurar grupos de permissão e níveis de acesso (Administrador, Operador Interno, Correspondente Externo)

[ ] Criar dashboard inicial com resumo de indicadores (KPIs) pós-login

Fase 3: Módulo de Gestão de Pessoas (pessoas)

[ ] Modelar entidades principais: Pessoa (suporte a PF e PJ), Endereco, Contato e PapelPessoa

[ ] Criar e aplicar migrações iniciais (makemigrations / migrate)

[ ] Registrar models no Django Admin com filtros, buscas e inlines

[ ] Implementar validação matemática e de formatação para CPF e CNPJ

[ ] Desenvolver CRUD completo (Listagem com paginação/busca, Criação, Edição, Detalhes e Inativação lógica)

[ ] Criar máscaras de input no frontend (CPF, CNPJ, telefone, CEP com busca automática via ViaCEP)

Fase 4: Módulo de Gestão de Diligências (diligencias)

[ ] Modelar entidades principais: Diligencia, TipoDiligencia, HistoricoMovimentacao e AnexoDiligencia

[ ] Estabelecer relacionamentos (ForeignKey / ManyToMany) entre Diligências e Pessoas (Cliente, Solicitante, Responsável/Correspondente, Parte Envolvida)

[ ] Implementar máquina de estados/status (ex: Pendente, Em Andamento, Aguardando Comprovante, Concluída, Cancelada)

[ ] Criar camada de serviço (services.py) para registrar automaticamente logs no HistoricoMovimentacao a cada mudança de status ou responsável

[ ] Desenvolver CRUD completo de Diligências com filtros avançados (por status, prazo, responsável e cliente)

[ ] Implementar upload, visualização e download seguro de anexos e comprovantes de cumprimento

[ ] Adicionar alertas visuais de vencimento (prazos próximos do fim ou em atraso)

Fase 5: Relatórios, Refinamentos e Qualidade

[ ] Otimizar consultas ao banco de dados (select_related e prefetch_related) para eliminar gargalos N+1

[ ] Implementar exportação de relatórios de diligências (CSV / PDF)

[ ] Escrever testes unitários e de integração para regras críticas (validação de documentos, fluxo de status e permissões)

[ ] Realizar auditoria de segurança (CSRF, XSS, validação de upload de arquivos e proteção de rotas)

Fase 6: Deploy e Produção

[ ] Configurar settings.py para ambiente de produção (DEBUG=False, ALLOWED_HOSTS, HTTPS/SSL)

[ ] Migrar banco de dados para PostgreSQL

[ ] Configurar servidor de arquivos estáticos (WhiteNoise ou Nginx/S3)

[ ] Configurar servidor WSGI/ASGI (Gunicorn / Uvicorn) e realizar deploy final

Como Executar o Projeto Localmente

1. Clone o repositório

git clone https://github.com/seu-usuario/nome-do-repositorio.git
cd nome-do-repositorio


2. Crie e ative o ambiente virtual

# Linux / macOS
python3 -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate


3. Instale as dependências

pip install -r requirements.txt


4. Configure as variáveis de ambiente

Copie o arquivo de exemplo e ajuste conforme necessário:

cp .env.example .env


5. Execute as migrações e crie um administrador

python manage.py migrate
python manage.py createsuperuser


6. Inicie o servidor local

python manage.py runserver


Acesse a aplicação em http://127.0.0.1:8000/.

Padrões e Convenções

Código: Segue rigorosamente a PEP 8 com uso de type hints e docstrings.

Commits: Padronizados com Conventional Commits (feat:, fix:, docs:, refactor:, test:, chore:).

Regra de Ouro: Toda alteração estrutural deve manter a sincronia entre models.py, migrations/ e este README.md.
