# Nexus Orchestrator — Self-hosted

Rode a plataforma completa (API, Web, filas, execuções) na sua própria infraestrutura.

## Requisitos

- Docker e Docker Compose
- Uma licença Nexus Orchestrator (compre em https://nexusorchestrator.com/self-hosted)

## Instalação

```bash
cp .env.example .env
# edite .env: defina SECRET_KEY, SECRET_ENCRYPTION_KEY, POSTGRES_PASSWORD
# (opcional: já cole sua LICENSE_KEY aqui, ou ative depois pela tela de Billing)

docker compose up -d
```

Acesse:

- Web: http://localhost:3000
- API: http://localhost:8000/docs

No primeiro acesso, crie o workspace inicial (só é permitido criar um; instâncias
self-hosted rodam com um único workspace). Depois disso a tela de cadastro fecha e
fica só o login.

## Ativando a licença

Se você não colocou `LICENSE_KEY` no `.env`, ative pela própria interface:

**Configurações → Billing → Ativar licença** e cole a chave recebida por e-mail.

Sem licença ativa, a instância roda no modo gratuito (limites reduzidos de
usuários, agents, automações e execuções).

## Atualizando

```bash
docker compose pull
docker compose up -d
```

As migrações do banco rodam automaticamente na subida do container `api`.

## Backup

Os dados ficam nos volumes Docker `postgres_data` (banco) e `package_storage`
(pacotes de automação publicados). Faça backup regularmente:

```bash
docker compose exec postgres pg_dump -U rpanexus rpanexus_db > backup.sql
```

## Suporte

- Documentação: https://app.nexusorchestrator.com.br/docs
- Suporte: suporte@nexusorchestrator.com.br
