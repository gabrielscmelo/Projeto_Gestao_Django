# Sistema de Gestão de Diligências e Pessoas

![Python](https://img.shields.io/badge/Python-3.11%2B-blue?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-5.x-092E20?logo=django&logoColor=white)
![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow)

Sistema web desenvolvido em **Python** e **Django** para o gerenciamento centralizado de cadastros de pessoas (físicas e jurídicas) e o controle completo do ciclo de vida de diligências, prazos, responsáveis e comprovantes.

---

## Sumário
- [Visão Geral e Arquitetura](#visão-geral-e-arquitetura)
- [Stack Tecnológica](#stack-tecnológica)
- [Roadmap e Etapas de Desenvolvimento](#roadmap-e-etapas-de-desenvolvimento)
- [Como Executar o Projeto Localmente](#como-executar-o-projeto-localmente)
- [Padrões e Convenções](#padrões-e-convenções)

---

## Visão Geral e Arquitetura

O projeto segue o padrão arquitetural **MVT (Model-View-Template)** do Django, com isolamento de regras de negócio complexas em camadas de serviço (`services.py`), garantindo código limpo, testável e escalável.

```text
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
