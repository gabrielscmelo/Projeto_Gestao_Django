# README.md

# Sistema de Gestão de Diligências e Pessoas

Sistema web (Python/Django) para controle de diligências, prazos, cobranças de resposta e gestão dos colaboradores de um setor.

## Visão geral

- **Diligências:** cadastro, prazos, status (no prazo / em atraso / concluída), anexos e histórico de alterações.
- **Cobranças:** registro manual, prorrogação de prazo, histórico e robô de cobrança automática via SEI.
- **Pessoas:** cadastro de colaboradores (Procurador, Procurador-Chefe, Procurador-Adjunto, Assessor, Estagiário), afastamentos e calendário de férias/carência.

## Arquitetura

```
Projeto_Gestao_Django/
├── config/                 # projeto Django (settings/base.py, dev.py, prod.py, urls.py, wsgi/asgi)
├── apps/
│   ├── core/               # layout base, utilitários, mixins, tema claro/escuro
│   ├── accounts/           # usuário customizado, login, perfis e lotação
│   ├── pessoas/            # colaboradores, afastamentos e calendário
│   ├── diligencias/        # diligências, prazos, anexos e histórico
│   └── cobrancas/          # cobranças, histórico e robô SEI (services + management command)
├── templates/              # templates globais (base.html, parciais)
├── static/                 # CSS, JS, imagens
├── media/                  # uploads (anexos/comprovantes) - não versionado
├── requirements/           # base.txt, dev.txt, prod.txt
├── .env.example
├── manage.py
└── README.md
```

Princípios: regras de negócio fora das views (services, managers e métodos nos models), consultas otimizadas
(`select_related`/`prefetch_related`), `LoginRequiredMixin` em todas as views, validação rigorosa em forms, rastreabilidade
total das diligências.

## Stack

| Camada | Tecnologia |
|---|---|
| Linguagem | Python 3.11+ |
| Framework | Django 5.x |
| Banco | SQLite (dev) / PostgreSQL (prod) |
| Frontend | Django Templates + Bootstrap 5 (tema claro/escuro) + HTMX (busca e filtros sem recarregar a página) |
| Calendário | FullCalendar (via templates) |
| Automação SEI | Playwright (Chrome) |
| Qualidade | ruff, pre-commit, pytest-django, GitHub Actions |

## Roadmap

### Fase 1: Setup e Infraestrutura
- [x] Repositório criado com `README.md` e `.gitignore` na `main`
- [ ] Ambiente virtual, `requirements/` e projeto Django 5.x (`config/`)
- [ ] Settings por ambiente (base/dev/prod) com variáveis em `.env`
- [ ] Criar apps `core`, `accounts`, `pessoas`, `diligencias` e `cobrancas`
- [ ] Templates base (layout, abas de navegação, tema claro/escuro), `static` e `media`
- [ ] SQLite em dev / PostgreSQL em prod
- [ ] Qualidade: ruff, pre-commit e pytest-django
- [ ] CI com GitHub Actions (lint + testes)

### Fase 2: Autenticação e Permissões
- [ ] Usuário customizado (perfil e lotação)
- [ ] Login/logout e troca de senha
- [ ] Perfis e permissões (grupos)
- [ ] Proteção de todas as views (`LoginRequiredMixin`) e limite de tentativas de login
- [ ] Alterar lotação

### Fase 3: Gestão de Pessoas (colaboradores do setor)
- [ ] Cadastro de colaboradores com cargo e situação (ativo/inativo)
- [ ] Listagem com contagem total e por cargo
- [ ] Afastamentos (férias, carência, outros) com período e motivo
- [ ] Validação de sobreposição de períodos
- [ ] Calendário (dia/semana/mês/ano) com filtro Todos/Procurador/Apoio
- [ ] Relatório de afastamentos

### Fase 4: Diligências, Prazos, Anexos e Cobranças
- [ ] Modelo de diligência (SAJ, SEI, nº da diligência, procurador, categoria, secretaria, caminho, Id SEI, envio, prazo)
- [ ] Status calculado e manager de diligências em atraso
- [ ] Histórico de alterações (status, prazo, responsável)
- [ ] Listagem com busca por campo, filtros e contador de ativas
- [ ] Concluir, ver concluídas e exclusão lógica
- [ ] Anexos e comprovantes
- [ ] Cobrança manual com prorrogação e histórico
- [ ] Notificações de prazo
- [ ] Comando de importação dos dados do sistema legado (MySQL)

### Fase 5: Robô SEI, Relatórios e Testes
- [ ] Portar o robô de cobrança para `apps/cobrancas` (service + seletores isolados)
- [ ] Management command `executar_robo_cobranca` (senha informada na execução, nunca persistida)
- [ ] Execução em lote por seleção de diligências, com log de execução
- [ ] Relatórios em PDF (diligências, cobranças, afastamentos)
- [ ] Testes unitários e de integração (robô testado com mocks, sem acessar o SEI real)

### Fase 6: Deploy
- [ ] Gunicorn + Nginx, PostgreSQL, arquivos estáticos e HTTPS
- [ ] Settings de produção e `check --deploy`
- [ ] Backups e logs
- [ ] Pipeline de deploy

## Instalação local

> Os arquivos de ambiente (`requirements/`, `.env.example`, `manage.py`) passam a existir ao final da Fase 1.

```bash
# 1. Clonar
git clone https://github.com/<seu-usuario>/Projeto_Gestao_Django.git
cd Projeto_Gestao_Django

# 2. Ambiente virtual
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
.venv\Scripts\activate           # Windows

# 3. Dependências
pip install -r requirements/dev.txt

# 4. Variáveis de ambiente
cp .env.example .env             # Windows: copy .env.example .env

# 5. Banco e usuário administrador
python manage.py migrate
python manage.py createsuperuser

# 6. Rodar
python manage.py runserver
```

Acesse http://127.0.0.1:8000/

## Fluxo de trabalho (Git)

- `main` contém apenas código funcional e estável; ninguém commita direto nela.
- Cada tarefa nasce em uma branch a partir da `main` e volta por Pull Request.
- Nomes: `feat/<descricao>`, `fix/<descricao>`, `docs/<descricao>`, `chore/<descricao>` (minúsculas, com hífen).
- Commits no padrão: `feat: ...`, `fix: ...`, `docs: ...`, `chore: ...`.
