# Dimidui - Guia de Deploy e Configuração

**Plataforma:** Vercel (Frontend) + Supabase (Backend)  
**Tempo estimado:** 15-20 minutos

---

## 1. PRÉ-REQUISITOS

- [ ] Conta no [Vercel](https://vercel.com) (gratuita)
- [ ] Conta no [Supabase](https://supabase.com) (gratuita)
- [ ] Repositório Git (GitHub, GitLab ou Bitbucket)
- [ ] Node.js 18+ instalado localmente
- [ ] Git instalado

---

## 2. SETUP DO BACKEND (SUPABASE)

### 2.1 Criar Projeto

1. Acesse [app.supabase.com](https://app.supabase.com)
2. Clique em **"New Project"**
3. Preencha:
   - **Organization:** Selecione ou crie uma
   - **Name:** `dimidui-production`
   - **Database Password:** [Gere uma senha forte e SALVE]
   - **Region:** `South America (São Paulo)` (mais próximo do Brasil)
   - **Pricing Plan:** Free (suficiente para MVP)
4. Clique em **"Create new project"**
5. Aguarde 2-3 minutos para provisionamento

### 2.2 Executar Scripts SQL

1. No dashboard do Supabase, vá em **SQL Editor** (ícone `</>` na sidebar)
2. Clique em **"New Query"**
3. Copie TODO o conteúdo do arquivo `dimidui_backend_spec.md` seção "2. SCHEMA COMPLETO"
4. Cole no editor
5. Clique em **"Run"** (Ctrl/Cmd + Enter)
6. Repita para as seções:
   - "3. VIEWS ÚTEIS"
   - "4. FUNCTIONS ÚTEIS"
   - "5. ROW LEVEL SECURITY"
   - "6. DADOS INICIAIS (SEEDS)"

### 2.3 Obter Credenciais

1. Vá em **Settings > API** (ícone de engrenagem na sidebar)
2. Copie as seguintes informações:

```
Project URL: https://[seu-projeto].supabase.co
anon (public) key: eyJhbGc...
service_role (secret) key: eyJhbGc... [NÃO COMPARTILHAR]
```

3. **Salve essas credenciais** - você vai precisar delas

### 2.4 Verificar Instalação

Execute no SQL Editor:

```sql
-- Deve retornar 5 alunos
SELECT COUNT(*) FROM students;

-- Deve retornar 2 atividades
SELECT COUNT(*) FROM activities WHERE published = TRUE;

-- Deve retornar dados do dashboard
SELECT * FROM teacher_dashboard;
```

Se todos os comandos funcionarem, backend está pronto! ✅

---

## 3. SETUP DO FRONTEND (LOCAL)

### 3.1 Clonar e Instalar

```bash
# Se ainda não criou o projeto
npm create vite@latest dimidui -- --template react
cd dimidui

# Instalar dependências
npm install @supabase/supabase-js react-router-dom framer-motion lucide-react tone clsx

# Instalar Tailwind
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### 3.2 Configurar Variáveis de Ambiente

Crie arquivo `.env` na raiz do projeto:

```env
VITE_SUPABASE_URL=https://[seu-projeto].supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGc...
```

⚠️ **IMPORTANTE:** 
- Use as credenciais copiadas do passo 2.3
- `.env` deve estar no `.gitignore` (já está por padrão no Vite)
- NUNCA commite a `service_role key` (não é necessária no frontend)

### 3.3 Testar Conexão

Crie arquivo temporário `test-connection.js`:

```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
)

// Testar conexão
const { data, error } = await supabase
  .from('students')
  .select('*')
  .limit(1)

if (error) {
  console.error('❌ Erro de conexão:', error)
} else {
  console.log('✅ Conexão OK! Aluno:', data[0].name)
}
```

Execute:
```bash
node test-connection.js
```

Se aparecer "✅ Conexão OK!", está funcionando!

### 3.4 Desenvolver Localmente

```bash
npm run dev
```

Acesse: `http://localhost:5173`

---

## 4. DEPLOY NO VERCEL

### 4.1 Preparar Repositório Git

```bash
# Inicializar git (se ainda não fez)
git init

# Adicionar arquivos
git add .
git commit -m "Initial commit - Dimidui MVP"

# Criar repositório no GitHub
# (via interface do GitHub)

# Conectar e enviar
git remote add origin https://github.com/[seu-usuario]/dimidui.git
git branch -M main
git push -u origin main
```

### 4.2 Deploy via Vercel Dashboard

1. Acesse [vercel.com](https://vercel.com)
2. Clique em **"Add New Project"**
3. Clique em **"Import Git Repository"**
4. Selecione seu repositório `dimidui`
5. Configure:

**Build Settings:**
- Framework Preset: `Vite`
- Build Command: `npm run build` (já preenchido)
- Output Directory: `dist` (já preenchido)
- Install Command: `npm install` (já preenchido)

**Environment Variables:**
Clique em **"Environment Variables"** e adicione:

```
Name: VITE_SUPABASE_URL
Value: https://[seu-projeto].supabase.co

Name: VITE_SUPABASE_ANON_KEY
Value: eyJhbGc... (sua chave)
```

6. Clique em **"Deploy"**
7. Aguarde 2-3 minutos

### 4.3 Verificar Deploy

Quando o deploy finalizar:
1. Clique no botão **"Visit"** ou copie a URL (ex: `dimidui.vercel.app`)
2. Acesse a URL
3. Deve aparecer a tela de login com os 5 alunos

Se aparecer corretamente, deploy funcionou! 🎉

---

## 5. CONFIGURAÇÕES PÓS-DEPLOY

### 5.1 Configurar Domínio Customizado (Opcional)

No Vercel Dashboard:
1. Vá em **Settings > Domains**
2. Adicione seu domínio (ex: `dimidui.com.br`)
3. Siga instruções de DNS

### 5.2 Habilitar Analytics (Opcional)

No Vercel Dashboard:
1. Vá em **Analytics**
2. Clique em **"Enable"**
3. Gratuito até 100k eventos/mês

### 5.3 Configurar CORS no Supabase (Se Necessário)

Se tiver problemas de CORS:

1. Supabase Dashboard > **Authentication > URL Configuration**
2. Adicione suas URLs:
   - `http://localhost:5173` (desenvolvimento)
   - `https://dimidui.vercel.app` (produção)

---

## 6. CI/CD AUTOMÁTICO

Depois do primeiro deploy, o Vercel automaticamente:
- ✅ Faz rebuild a cada push no branch `main`
- ✅ Cria preview deploys para PRs
- ✅ Roda testes (se configurados)

**Workflow:**
```bash
# Fazer alterações
git add .
git commit -m "feat: adicionar módulo M5"
git push

# Vercel automaticamente detecta e faz deploy em ~2min
```

---

## 7. MONITORAMENTO

### 7.1 Logs do Frontend (Vercel)

1. Vercel Dashboard > Seu projeto > **Deployments**
2. Clique no deployment
3. Aba **"Functions"** > Ver logs em tempo real

### 7.2 Logs do Backend (Supabase)

1. Supabase Dashboard > **Logs**
2. Filtrar por:
   - **API Logs:** Requisições HTTP
   - **Database Logs:** Queries SQL
   - **Auth Logs:** Autenticações

### 7.3 Performance

**Vercel:**
- Dashboard > **Analytics** > Ver métricas de performance

**Supabase:**
- Dashboard > **Database > Query Performance**
- Identificar queries lentas

---

## 8. BACKUPS E ROLLBACK

### 8.1 Backup do Banco (Supabase)

**Automático:**
- Backups diários automáticos (retidos por 7 dias no plano Free)

**Manual:**
1. Dashboard > **Database > Backups**
2. Clique em **"Create Backup"**
3. Backup criado instantaneamente

**Restaurar:**
1. Encontre o backup desejado
2. Clique em **"Restore"**
3. ⚠️ **CUIDADO:** Isso sobrescreve o banco atual!

### 8.2 Rollback do Frontend (Vercel)

1. Dashboard > **Deployments**
2. Encontre o deployment que funcionava
3. Clique nos 3 pontos > **"Promote to Production"**
4. Rollback instantâneo!

---

## 9. AMBIENTES (DEV/STAGING/PROD)

### 9.1 Estrutura Recomendada

```
Supabase:
- dimidui-development (testes)
- dimidui-production (real)

Vercel:
- main branch → dimidui.vercel.app (produção)
- develop branch → dimidui-dev.vercel.app (staging)
- feature branches → preview deploys automáticos
```

### 9.2 Configurar Staging

**No Git:**
```bash
git checkout -b develop
git push -u origin develop
```

**No Vercel:**
1. Settings > Git
2. Adicione **Production Branch:** `main`
3. Preview branches: `develop` e todas as outras

**Variáveis de ambiente por branch:**
1. Settings > Environment Variables
2. Configure diferentes URLs do Supabase por ambiente:
   - Production: usa `dimidui-production`
   - Preview (develop): usa `dimidui-development`

---

## 10. TROUBLESHOOTING COMUM

### Erro: "Failed to load resource"

**Causa:** Variáveis de ambiente não configuradas  
**Solução:**
```bash
# Verificar localmente
cat .env

# No Vercel: Settings > Environment Variables
# Verificar se VITE_SUPABASE_URL e VITE_SUPABASE_ANON_KEY estão definidas
```

### Erro: "Database error" ou 403

**Causa:** RLS bloqueando acesso  
**Solução:** 
1. Supabase Dashboard > **Authentication > Policies**
2. Verificar se policies estão corretas
3. Temporariamente desabilitar RLS para debug:
```sql
ALTER TABLE students DISABLE ROW LEVEL SECURITY;
```

### Build falha no Vercel

**Causa:** Dependências faltando  
**Solução:**
```bash
# Limpar cache e reinstalar
rm -rf node_modules package-lock.json
npm install
npm run build

# Se funcionar localmente, fazer push
git add .
git commit -m "fix: atualizar dependências"
git push
```

### Deploy lento (>5min)

**Causa:** Cache não está sendo usado  
**Solução:**
1. Vercel Dashboard > Settings > General
2. Habilitar **"Enable Deployment Protection"**
3. Configurar **Build Cache**

### Supabase responde lento

**Causa:** Plano Free tem limites  
**Solução:**
1. Dashboard > **Settings > Billing**
2. Verificar limites:
   - 500 MB database
   - 2 GB bandwidth/mês
   - 50k reqs/mês
3. Otimizar queries ou fazer upgrade

---

## 11. SEGURANÇA

### 11.1 Checklist Pré-Produção

- [ ] `.env` no `.gitignore`
- [ ] Nenhuma chave secreta commitada
- [ ] RLS habilitado em todas as tabelas
- [ ] Policies revisadas (não deixar `USING (true)` em prod)
- [ ] CORS configurado corretamente
- [ ] HTTPS habilitado (automático no Vercel)
- [ ] Backups automáticos habilitados
- [ ] Rate limiting configurado

### 11.2 Rotacionar Chaves (Se Necessário)

**Supabase:**
1. Settings > API > **"Generate new API key"**
2. Atualizar no Vercel: Settings > Environment Variables
3. Fazer redeploy

**Nunca compartilhar:**
- ❌ `service_role key` (admin total)
- ❌ Database password
- ✅ `anon key` pode ser exposta (pública, com RLS)

---

## 12. MANUTENÇÃO REGULAR

### 12.1 Checklist Semanal

- [ ] Verificar logs de erro (Vercel + Supabase)
- [ ] Verificar uso de recursos (bandwidth, storage)
- [ ] Backup manual se fez mudanças grandes

### 12.2 Checklist Mensal

- [ ] Revisar queries lentas (Supabase > Query Performance)
- [ ] Limpar dados antigos (se necessário)
- [ ] Atualizar dependências:
```bash
npm outdated
npm update
```

### 12.3 Checklist Semestral

- [ ] Atualizar Node.js
- [ ] Revisar e otimizar índices do banco
- [ ] Analisar métricas de uso (Analytics)

---

## 13. CUSTOS ESTIMADOS

### Free Tier (Adequado para MVP)

**Vercel:**
- 100 GB bandwidth
- 100 deployments/dia
- Ilimitado previews
- **Custo:** $0

**Supabase:**
- 500 MB database
- 2 GB bandwidth/mês
- 50k API requests/mês
- **Custo:** $0

### Quando Fazer Upgrade

**Sinais:**
- >40 alunos ativos simultâneos
- >50k requests/mês
- Database >400 MB
- Precisar de backups por mais de 7 dias

**Planos Pagos:**
- Vercel Pro: $20/mês (team features, analytics avançado)
- Supabase Pro: $25/mês (8 GB database, backups por 30 dias)

---

## 14. COMANDOS RÁPIDOS

```bash
# Desenvolvimento local
npm run dev

# Build local (testar antes de deploy)
npm run build
npm run preview

# Deploy manual (se não usar Git)
npx vercel

# Ver logs do Vercel (CLI)
npx vercel logs [deployment-url]

# Ver status do deploy
npx vercel inspect [deployment-url]

# Promover preview para produção
npx vercel promote [preview-url]
```

---

## 15. CONTATOS E RECURSOS

**Documentação:**
- Vite: https://vitejs.dev
- React: https://react.dev
- Supabase: https://supabase.com/docs
- Vercel: https://vercel.com/docs
- Tailwind: https://tailwindcss.com/docs

**Suporte:**
- Supabase: https://supabase.com/support
- Vercel: https://vercel.com/support

**Status:**
- Supabase Status: https://status.supabase.com
- Vercel Status: https://vercel-status.com

---

## 16. CHECKLIST FINAL DE DEPLOY

### Primeira Vez:
- [ ] Backend (Supabase) criado e configurado
- [ ] Todas as tabelas criadas
- [ ] Seeds inseridos
- [ ] Credenciais copiadas
- [ ] Frontend buildando localmente
- [ ] `.env` configurado
- [ ] Conexão Supabase testada
- [ ] Repositório Git criado
- [ ] Código commitado e enviado
- [ ] Projeto importado no Vercel
- [ ] Variáveis de ambiente configuradas no Vercel
- [ ] Deploy realizado com sucesso
- [ ] Aplicação acessível via URL
- [ ] Login funcionando
- [ ] Dashboard carregando
- [ ] Atividade executando do início ao fim

### Após Mudanças:
- [ ] Testado localmente (`npm run build`)
- [ ] Commitado com mensagem descritiva
- [ ] Push para Git
- [ ] Aguardar deploy automático (~2min)
- [ ] Verificar URL de produção
- [ ] Testar funcionalidade alterada

---

**Deploy completo! 🚀**

**Próximo passo:** Testar com alunos reais e iterar baseado no feedback.