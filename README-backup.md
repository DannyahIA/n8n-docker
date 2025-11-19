# 🚀 Setup de Backup - N8N + Evolution API + Typebot + Nginx

Este é um ambiente completo de backup com todos os serviços integrados em uma única stack Docker.

## 📋 Serviços Incluídos

- **N8N**: Automação e workflows (porta 5679)
- **Evolution API**: API do WhatsApp (porta 8081)
- **Typebot**: Bot builder e viewer (portas 3001/3002)
- **Nginx Proxy Manager**: Gerenciamento de proxy (porta 8181)
- **PostgreSQL**: Banco de dados compartilhado
- **Redis**: Cache para Evolution API

## 🛠️ Como Usar

### 1. Preparar o Ambiente

```bash
# Copie o arquivo de configuração
cp .env-backup .env-backup-local

# Edite as configurações conforme necessário
nano .env-backup-local
```

### 2. Iniciar os Serviços

```bash
# Subir todos os serviços
docker compose -f docker-compose-backup.yml --env-file .env-backup up -d

# Ver logs em tempo real
docker compose -f docker-compose-backup.yml --env-file .env-backup logs -f

# Ver logs de um serviço específico
docker compose -f docker-compose-backup.yml logs -f n8n
```

### 3. Acessar os Serviços

| Serviço | URL | Credenciais |
|---------|-----|-------------|
| N8N | http://localhost:5679 | admin@backup.local / AdminBackup2024! |
| Evolution API | http://localhost:8081 | API Key no .env |
| Typebot Builder | http://localhost:3001 | Configurar no primeiro acesso |
| Typebot Viewer | http://localhost:3002 | - |
| Nginx Proxy Manager | http://localhost:8181 | admin@example.com / changeme |

### 4. Configuração do Nginx Proxy Manager

1. Acesse: http://localhost:8181
2. Faça login com as credenciais padrão
3. Configure os proxy hosts para seus domínios
4. Adicione certificados SSL se necessário

## 📁 Estrutura de Diretórios

```
backup_data/
├── n8n_data/          # Dados do N8N
├── files/             # Arquivos do N8N
├── nginx_data/        # Dados do Nginx Proxy Manager
└── nginx_letsencrypt/ # Certificados SSL
```

## 🔧 Comandos Úteis

### Gerenciamento dos Containers

```bash
# Parar todos os serviços
docker-compose -f docker-compose-backup.yml down

# Reiniciar um serviço específico
docker-compose -f docker-compose-backup.yml restart evolution-api

# Ver status dos containers
docker-compose -f docker-compose-backup.yml ps

# Atualizar imagens
docker-compose -f docker-compose-backup.yml pull
docker-compose -f docker-compose-backup.yml up -d
```

### Backup e Restore

```bash
# Backup do banco PostgreSQL
docker exec postgres_backup pg_dump -U backup_user backup_database > backup.sql

# Restore do banco PostgreSQL
docker exec -i postgres_backup psql -U backup_user backup_database < backup.sql

# Backup dos volumes
docker run --rm -v postgres_data_backup:/data -v $(pwd):/backup alpine tar czf /backup/postgres_backup.tar.gz -C /data .
```

## ⚙️ Configurações Importantes

### Evolution API
- **API Key**: `BackupEvolutionKey429683C4C977415CAAFCCE10F7D57E11`
- **Endpoint**: http://localhost:8081
- **Documentação**: http://localhost:8081/docs

### Typebot Integration
- Configurado para funcionar com Evolution API
- URLs configuradas automaticamente no .env
- Schemas separados no PostgreSQL

### Portas Utilizadas
- **N8N**: 5679
- **Evolution API**: 8081
- **Typebot Builder**: 3001
- **Typebot Viewer**: 3002
- **Nginx HTTP**: 8080
- **Nginx Admin**: 8181
- **Nginx HTTPS**: 4443

## 🔒 Segurança

⚠️ **IMPORTANTE**: Este é um setup de desenvolvimento/backup. Para produção:

1. Altere todas as senhas no arquivo `.env-backup`
2. Configure certificados SSL adequados
3. Restrinja o acesso às portas necessárias
4. Configure firewall adequadamente
5. Use senhas complexas e únicas

## 🐛 Troubleshooting

### Problemas Comuns

1. **Conflito de portas**: Verifique se as portas não estão sendo usadas
2. **Erro de permissão**: Execute com `sudo` se necessário
3. **Banco não conecta**: Aguarde o PostgreSQL inicializar completamente
4. **Evolution API não inicia**: Verifique as configurações do Redis

### Logs Úteis

```bash
# Ver logs de todos os serviços
docker-compose -f docker-compose-backup.yml logs

# Logs específicos do Evolution API
docker logs evolution_api_backup -f

# Logs do PostgreSQL
docker logs postgres_backup -f
```

## 📞 Suporte

Para problemas ou dúvidas:
- Evolution API: [Documentação oficial](https://doc.evolution-api.com/)
- N8N: [Documentação oficial](https://docs.n8n.io/)
- Typebot: [Documentação oficial](https://docs.typebot.io/)

---

Criado com ❤️ para facilitar seu ambiente de backup integrado!