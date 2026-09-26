# Nexus Orchestrator — Self-hosted

Rode a plataforma completa (API, Web, filas, execuções) na sua própria infraestrutura.

## Requisitos

- Docker e Docker Compose
- Uma licença Nexus Orchestrator (compre em https://app.nexusorchestrator.com.br/self-hosted)

## Instalação

```bash
cp .env.example .env
# gere e defina os segredos obrigatórios:
#   openssl rand -hex 32                 # SECRET_KEY
#   openssl rand -base64 32              # SECRET_ENCRYPTION_KEY
# também defina uma senha forte em POSTGRES_PASSWORD
# não configure chaves Stripe: o billing self-hosted é processado pelo servidor central
# (opcional: já cole sua LICENSE_KEY aqui, ou ative depois pela tela de Billing)

docker compose pull
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
usuários, agents, automações e execuções). O download do Agent e as operações
permitidas pelo free tier continuam disponíveis até os limites configurados.

## Configurando e-mail (necessário para convites)

Convidar membros para o workspace depende de envio de e-mail. Configure **uma**
das opções abaixo no `.env` antes de convidar alguém — sem isso, o convite falha:

- **Resend** (mais simples): preencha `RESEND_API_KEY`.
- **SMTP genérico** (Gmail, SendGrid, servidor próprio, etc.): preencha
  `SMTP_HOST`, `SMTP_PORT`, `SMTP_USERNAME`, `SMTP_PASSWORD`, `SMTP_FROM_EMAIL`
  e `SMTP_FROM_NAME`.

Depois de editar o `.env`, reinicie a API e o scheduler:

```bash
docker compose up -d nexus_orchestrator_selfhosted_production_api nexus_orchestrator_selfhosted_production_scheduler
```

## Atualizando

```bash
docker compose pull
docker compose up -d
```

As migrações do banco rodam automaticamente na subida do container `api`. O worker `scheduler` também é iniciado automaticamente para processar agendamentos, filas e execuções.

## Backup

Os dados ficam nos volumes Docker `postgres_data` (banco) e `package_storage`
(pacotes de automação publicados). Faça backup regularmente:

```bash
docker compose exec nexus_orchestrator_db_selfhosted pg_dump -U rpanexus rpanexus_db > backup.sql
```

## Suporte

- Documentação: https://app.nexusorchestrator.com.br/docs
- Suporte: suporte@nexusorchestrator.com.br
