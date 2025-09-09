[CRM Empresas.md](https://github.com/user-attachments/files/22233181/CRM.Empresas.md)
# CRM Empresas

Sistema completo de gerenciamento de relacionamento com empresas clientes, desenvolvido em PHP com MySQL e Apache.

## 📋 Funcionalidades

### 🏢 Gestão de Empresas
- Cadastro completo com validação de CNPJ
- Busca avançada e filtros
- Histórico de relacionamento
- Ranking por atividade

### 📝 Sistema de Ocorrências
- Registro detalhado de interações
- Categorização por tipos
- Workflow de status (Aberta → Em Andamento → Resolvida → Fechada)
- Sistema de anexos
- Controle de prioridades

### 👥 Gestão de Usuários
- Três níveis de acesso (Admin, Operador, Visualizador)
- Autenticação segura
- Controle de sessões
- Logs de auditoria

### 📊 Relatórios e Analytics
- Dashboard com estatísticas em tempo real
- Ranking de empresas mais ativas
- Gráficos interativos
- Exportação para CSV/Excel
- Relatórios personalizáveis

### 💾 Backup e Restauração
- Backup automático agendável
- Backup manual via interface
- Verificação de integridade
- Restauração completa
- Upload de backups externos

### 🎨 Interface Moderna
- Design responsivo (Bootstrap 5)
- Compatível com mobile
- Tema moderno com gradientes
- Navegação intuitiva
- Validação em tempo real

## 🛠️ Requisitos do Sistema

### Servidor
- **SO**: Ubuntu 18.04+ ou Debian 9+ (recomendado)
- **Servidor Web**: Apache 2.4+
- **PHP**: 7.4+ ou 8.0+
- **Banco de Dados**: MySQL 5.7+ ou MariaDB 10.3+

### Extensões PHP Necessárias
- php-mysql
- php-mbstring
- php-xml
- php-curl
- php-zip
- php-gd
- php-json

### Módulos Apache Necessários
- mod_rewrite
- mod_headers
- mod_deflate
- mod_expires
- mod_ssl (para HTTPS)

## 🚀 Instalação Rápida

### 1. Instalação Automática (Recomendada)

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/crm-empresas.git
cd crm-empresas

# Execute o script de instalação
./install.sh
```

O script irá:
- Verificar dependências
- Configurar permissões
- Criar banco de dados
- Configurar Apache
- Configurar backup automático
- Configurar SSL (opcional)

### 2. Instalação Manual

#### Passo 1: Preparar o Ambiente

```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependências
sudo apt install -y apache2 mysql-server php php-mysql php-mbstring php-xml php-curl php-zip php-gd

# Habilitar módulos Apache
sudo a2enmod rewrite headers deflate expires ssl
```

#### Passo 2: Configurar Banco de Dados

```bash
# Acessar MySQL
sudo mysql -u root -p

# Criar banco e usuário
CREATE DATABASE crm_empresas CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'crm_user'@'localhost' IDENTIFIED BY 'sua_senha_segura';
GRANT ALL PRIVILEGES ON crm_empresas.* TO 'crm_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### Passo 3: Configurar Aplicação

```bash
# Copiar arquivos para o servidor
sudo cp -r crm-empresas /var/www/html/

# Configurar permissões
sudo chown -R www-data:www-data /var/www/html/crm-empresas
sudo chmod -R 755 /var/www/html/crm-empresas
sudo chmod -R 777 /var/www/html/crm-empresas/storage
sudo chmod -R 777 /var/www/html/crm-empresas/uploads

# Editar configuração do banco
nano /var/www/html/crm-empresas/config/database.php
```

#### Passo 4: Configurar Apache

```bash
# Copiar configuração do virtual host
sudo cp apache-config.conf /etc/apache2/sites-available/crm-empresas.conf

# Habilitar site
sudo a2ensite crm-empresas
sudo a2dissite 000-default

# Reiniciar Apache
sudo systemctl restart apache2
```

#### Passo 5: Importar Banco de Dados

```bash
# Executar migrations
mysql -u crm_user -p crm_empresas < database/migrations/create_tables.sql
mysql -u crm_user -p crm_empresas < database/migrations/optimize_indexes.sql
mysql -u crm_user -p crm_empresas < database/seeds/initial_data.sql
```

## 🔧 Configuração

### Configuração do Banco de Dados

Edite o arquivo `config/database.php`:

```php
return [
    'host' => 'localhost',
    'database' => 'crm_empresas',
    'username' => 'crm_user',
    'password' => 'sua_senha',
    'charset' => 'utf8mb4',
    'options' => [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        PDO::ATTR_EMULATE_PREPARES => false,
    ]
];
```

### Configuração Geral

Edite o arquivo `config/config.php` para ajustar:

- URL base da aplicação
- Configurações de sessão
- Limites de upload
- Configurações de email
- Timezone

### Backup Automático

Para configurar backup automático via cron:

```bash
# Editar crontab
crontab -e

# Adicionar linha para backup diário às 2:00
0 2 * * * /usr/bin/php /var/www/html/crm-empresas/public/index.php backup/auto >> /var/log/crm-backup.log 2>&1
```

## 👤 Acesso Inicial

Após a instalação, acesse o sistema:

- **URL**: http://seu-dominio.com
- **Usuário**: admin@admin.com
- **Senha**: admin123

**⚠️ IMPORTANTE**: Altere a senha do administrador imediatamente após o primeiro acesso!

## 📱 Uso do Sistema

### Dashboard
- Visão geral com estatísticas
- Gráficos de ocorrências
- Ranking de empresas
- Atividades recentes

### Empresas
- **Cadastrar**: Menu Empresas → Nova Empresa
- **Listar**: Menu Empresas → Ver todas
- **Buscar**: Use a barra de busca ou filtros
- **Editar**: Clique no ícone de edição

### Ocorrências
- **Criar**: Menu Ocorrências → Nova Ocorrência
- **Acompanhar**: Dashboard ou menu Ocorrências
- **Resolver**: Altere o status para "Resolvida"
- **Anexar**: Use o sistema de upload de arquivos

### Relatórios
- **Dashboard**: Estatísticas gerais
- **Ranking**: Menu Relatórios → Ranking de Empresas
- **Detalhado**: Menu Relatórios → Relatório de Ocorrências
- **Exportar**: Botão "Exportar CSV" nos relatórios

### Backup
- **Manual**: Menu Backup → Criar Backup
- **Automático**: Menu Backup → Configurações
- **Restaurar**: Menu Backup → Selecionar backup → Restaurar

## 🔒 Segurança

### Configurações de Produção

1. **HTTPS**: Configure SSL/TLS
2. **Firewall**: Bloqueie portas desnecessárias
3. **Banco**: Use senhas fortes e acesso restrito
4. **Arquivos**: Remova arquivos de desenvolvimento
5. **Logs**: Configure rotação de logs

### Hardening Apache

```apache
# Adicionar ao .htaccess ou configuração do Apache
ServerTokens Prod
ServerSignature Off
Header always set X-Content-Type-Options nosniff
Header always set X-Frame-Options DENY
Header always set X-XSS-Protection "1; mode=block"
```

### Backup de Segurança

```bash
# Backup completo (arquivos + banco)
tar -czf crm-backup-$(date +%Y%m%d).tar.gz /var/www/html/crm-empresas
mysqldump -u crm_user -p crm_empresas > crm-db-$(date +%Y%m%d).sql
```

## 🐛 Solução de Problemas

### Problemas Comuns

#### Erro 500 - Internal Server Error
```bash
# Verificar logs do Apache
sudo tail -f /var/log/apache2/error.log

# Verificar permissões
sudo chown -R www-data:www-data /var/www/html/crm-empresas
sudo chmod -R 755 /var/www/html/crm-empresas
```

#### Erro de Conexão com Banco
```bash
# Verificar se MySQL está rodando
sudo systemctl status mysql

# Testar conexão
mysql -u crm_user -p -h localhost crm_empresas
```

#### Problemas de Upload
```bash
# Verificar permissões da pasta uploads
sudo chmod 777 /var/www/html/crm-empresas/uploads

# Verificar configuração PHP
php -i | grep upload_max_filesize
```

### Logs do Sistema

- **Apache**: `/var/log/apache2/crm-empresas-*.log`
- **PHP**: `/var/log/php_errors.log`
- **Backup**: `/var/log/crm-backup.log`
- **Sistema**: `/var/www/html/crm-empresas/storage/logs/`

## 📈 Performance

### Otimizações Recomendadas

1. **Cache**: Configure OPcache do PHP
2. **Compressão**: Habilite GZIP no Apache
3. **CDN**: Use CDN para assets estáticos
4. **Banco**: Otimize queries e índices
5. **Monitoramento**: Configure monitoramento de recursos

### Configuração OPcache

```ini
; php.ini
opcache.enable=1
opcache.memory_consumption=128
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.revalidate_freq=2
opcache.fast_shutdown=1
```

## 🔄 Atualizações

### Processo de Atualização

1. **Backup**: Sempre faça backup antes de atualizar
2. **Download**: Baixe a nova versão
3. **Merge**: Mescle configurações personalizadas
4. **Migrate**: Execute migrations do banco
5. **Teste**: Verifique funcionamento

```bash
# Exemplo de atualização
cp -r config/database.php /tmp/
git pull origin main
cp /tmp/database.php config/
mysql -u crm_user -p crm_empresas < database/migrations/update_*.sql
```

## 📞 Suporte

### Documentação
- **Manual do Usuário**: `docs/manual-usuario.pdf`
- **API**: `docs/api.md`
- **Desenvolvimento**: `docs/desenvolvimento.md`

### Contato
- **Email**: suporte@crm-empresas.com
- **Issues**: GitHub Issues
- **Wiki**: GitHub Wiki

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📋 Changelog

### v1.0.0 (2024-01-15)
- Lançamento inicial
- Sistema completo de CRM
- Interface responsiva
- Sistema de backup
- Relatórios avançados

---

**Desenvolvido com ❤️ para gestão eficiente de relacionamento com empresas**

