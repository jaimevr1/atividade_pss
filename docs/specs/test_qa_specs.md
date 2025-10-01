# Dimidui - Guia de Testes e QA

**Objetivo:** Garantir qualidade antes de usar com alunos reais  
**Tempo estimado:** 2-3 horas para checklist completo

---

## 1. TESTES FUNCIONAIS (CORE)

### 1.1 Fluxo Completo: Login → Atividade → Conclusão

**Objetivo:** Verificar que o fluxo principal funciona sem erros

**Passos:**
1. [ ] Abrir aplicação em navegador
2. [ ] Clicar em um aluno (ex: "Ana Silva")
3. [ ] Dashboard carrega em <3 segundos
4. [ ] Ver "Atividade desta Semana" com botão "Começar"
5. [ ] Clicar em "Começar"
6. [ ] Atividade carrega e mostra primeiro módulo
7. [ ] Barra de progresso visível (ex: "Módulo 1 de 3")
8. [ ] Completar primeiro módulo (ex: EstadoJeremias)
9. [ ] Transição automática para segundo módulo
10. [ ] Completar segundo módulo (ex: BateriaRápida)
11. [ ] Transição para terceiro módulo
12. [ ] Completar terceiro módulo (ex: AutoAvaliação)
13. [ ] Redirecionamento automático para Dashboard
14. [ ] Dashboard mostra atividade como completada
15. [ ] Estatísticas atualizadas (ex: "Atividades Completas: 1")

**Critérios de Sucesso:**
- ✅ Sem erros no console do navegador
- ✅ Todas as transições são suaves (sem travamentos)
- ✅ Dados salvam corretamente (recarregar página não perde progresso)

---

### 1.2 Módulo M01 - BateriaRápida

**Cenário 1: Responder Corretamente**
1. [ ] Entrar em atividade com BateriaRápida
2. [ ] Ler problema (ex: "5 × 3 = ?")
3. [ ] Digitar resposta correta (15)
4. [ ] Clicar "Verificar"
5. [ ] Ver feedback positivo ("✓ Correto!")
6. [ ] Feedback aparece em <200ms
7. [ ] Barra de progresso avança (ex: 1/25 → 2/25)
8. [ ] Próximo problema carrega automaticamente

**Cenário 2: Responder Incorretamente**
1. [ ] Ler problema (ex: "7 × 4 = ?")
2. [ ] Digitar resposta errada (30)
3. [ ] Clicar "Verificar"
4. [ ] Ver feedback negativo ("✗ Incorreto. A resposta era 28")
5. [ ] Cor vermelha aparece
6. [ ] Próximo problema carrega após 1s

**Cenário 3: Completar Bateria Inteira**
1. [ ] Completar todas as 25 questões
2. [ ] Ver resumo final (ex: "Você acertou 20/25 (80%)")
3. [ ] Módulo marca como completo
4. [ ] Próximo módulo carrega automaticamente

**Critérios de Sucesso:**
- ✅ Input aceita apenas números
- ✅ Botão "Verificar" desabilitado quando input vazio
- ✅ Feedback visual claro (verde/vermelho)
- ✅ Geração aleatória produz problemas diferentes a cada vez

---

### 1.3 Módulo M20 - EstadoJeremias

**Cenário 1: Apresentação do Jeremias**
1. [ ] Módulo carrega
2. [ ] Avatar do Jeremias aparece (emoji grande, 96px)
3. [ ] Nome "Jeremias" visível
4. [ ] Diálogo aparece em caixa com fundo colorido
5. [ ] Fonte do diálogo é Comic Sans
6. [ ] Botão "Continuar" visível e clicável
7. [ ] Clicar "Continuar" avança para próximo módulo

**Cenário 2: Diferentes Moods**
1. [ ] Testar com mood "neutral" → emoji 🙂
2. [ ] Testar com mood "thinking" → emoji 🤔
3. [ ] Testar com mood "confused" → emoji 😕
4. [ ] Testar com mood "eureka" → emoji 😮
5. [ ] Testar com mood "confident" → emoji 😊
6. [ ] Testar com mood "grateful" → emoji 🙏

**Critérios de Sucesso:**
- ✅ Avatar centralizado
- ✅ Texto legível (contraste adequado)
- ✅ Animação sutil no avatar (bounce)

---

### 1.4 Módulo M10 - JeremiasResolver

**Cenário 1: Identificar Erro do Jeremias**
1. [ ] Módulo carrega com problema
2. [ ] Ver raciocínio do Jeremias (4 passos)
3. [ ] Clicar "Em qual passo Jeremias errou?"
4. [ ] Todos os passos viram botões clicáveis
5. [ ] Clicar no passo ERRADO
6. [ ] Ver alerta "Não é nesse passo. Tente outro!"
7. [ ] Tentar novamente
8. [ ] Clicar no passo CORRETO
9. [ ] Avançar para próxima etapa

**Cenário 2: Identificar Tipo de Erro**
1. [ ] Ver opções de tipo de erro (4 botões)
2. [ ] Clicar em tipo ERRADO
3. [ ] Ver alerta "Não é esse tipo de erro"
4. [ ] Clicar em tipo CORRETO
5. [ ] Ver feedback positivo do Jeremias
6. [ ] Avatar muda para "eureka" 😮
7. [ ] Mensagem de agradecimento aparece
8. [ ] Botão "Continuar" disponível

**Cenário 3: Dados Salvam Corretamente**
1. [ ] Completar módulo
2. [ ] Verificar no Supabase (tabela `jeremias_state`):
   - [ ] `corrections_made` incrementou
   - [ ] `times_taught` incrementou
   - [ ] Conceito foi adicionado a `concepts_taught`

**Critérios de Sucesso:**
- ✅ Interface clara (não confusa)
- ✅ Feedback imediato em cada interação
- ✅ Aluno não consegue "pular" etapas

---

### 1.5 Módulo M14 - AutoAvaliação

**Cenário 1: Responder Autoavaliação**
1. [ ] Módulo carrega com 1-3 conceitos
2. [ ] Cada conceito tem 3-4 opções de escala
3. [ ] Clicar em uma opção para cada conceito
4. [ ] Opção selecionada fica destacada (borda roxa)
5. [ ] Botão "Concluir" desabilitado até responder tudo
6. [ ] Após responder tudo, botão habilita
7. [ ] Clicar "Concluir"
8. [ ] Módulo finaliza

**Cenário 2: Alterar Resposta**
1. [ ] Selecionar uma opção
2. [ ] Mudar de ideia e clicar em outra
3. [ ] Nova opção fica selecionada
4. [ ] Anterior desmarca

**Critérios de Sucesso:**
- ✅ Interface intuitiva (claro que é seleção)
- ✅ Não permite enviar incompleto
- ✅ Dados salvam com respostas corretas

---

### 1.6 Sistema de Progresso

**Cenário 1: Barra de Progresso Global**
1. [ ] Iniciar atividade
2. [ ] Ver barra no topo mostrando "Módulo 1 de 3"
3. [ ] Completar primeiro módulo
4. [ ] Barra atualiza para "Módulo 2 de 3"
5. [ ] Animação suave de preenchimento
6. [ ] Ao finalizar, barra em 100%

**Cenário 2: Progresso no Dashboard**
1. [ ] Após completar atividade, voltar ao dashboard
2. [ ] Ver "Atividades Completas" incrementado
3. [ ] Atividade da semana não aparece mais (ou marca como completa)
4. [ ] Histórico mostra atividade recém-completada

**Cenário 3: Progresso do Jeremias**
1. [ ] Dashboard mostra nível do Jeremias (ex: "Nível 3")
2. [ ] Mostra conceitos aprendidos (ex: "5 conceitos")
3. [ ] Após ensinar Jeremias 5 vezes, verificar se nível sobe
4. [ ] Diálogo do Jeremias muda conforme nível

**Critérios de Sucesso:**
- ✅ Dados persistem após fechar navegador
- ✅ Progresso não regride (não volta para trás)
- ✅ Cálculos corretos (percentuais, contadores)

---

## 2. TESTES DE INTERFACE (UI/UX)

### 2.1 Responsividade (Desktop)

**Resoluções para Testar:**
- [ ] 1920×1080 (Full HD) → Layout não deve "nadar"
- [ ] 1366×768 (HD) → Deve ser confortável
- [ ] 1024×768 (mínimo) → Tudo visível, sem scroll horizontal

**Elementos Críticos:**
- [ ] Botões não ficam cortados
- [ ] Textos não quebram em lugares estranhos
- [ ] Cards não ficam largos demais (max-width: 800px)
- [ ] Barra de progresso visível em todas resoluções

### 2.2 Tipografia

**Verificar:**
- [ ] Texto de questões: 20px, legível
- [ ] Texto de corpo: 18px, confortável
- [ ] Diálogos do Jeremias: 18px, Comic Sans
- [ ] Contraste adequado (WCAG AAA)
  - Light mode: texto escuro (#2D3748) em fundo claro (#FFFFFF)
  - Dark mode: texto claro (#E2E8F0) em fundo escuro (#2D3748)

**Testar Toggle de Fonte:**
1. [ ] Clicar em "Settings" (se implementado)
2. [ ] Alternar para OpenDyslexic
3. [ ] Toda a interface muda de fonte
4. [ ] Preferência salva (recarregar mantém escolha)

### 2.3 Cores e Contraste

**Light Mode:**
- [ ] Fundo: Cinza azulado claro (#E8EDF2)
- [ ] Cards: Branco (#FFFFFF)
- [ ] Texto: Preto suave (#2D3748)
- [ ] Botões primários: Laranja (#F6AD55)
- [ ] Correto: Verde (#48BB78)
- [ ] Incorreto: Vermelho (#F56565)

**Dark Mode:**
- [ ] Fundo: Azul escuro (#1A202C)
- [ ] Cards: Cinza médio (#2D3748)
- [ ] Texto: Branco suave (#E2E8F0)
- [ ] Cores mantêm contraste adequado

**Testar Toggle de Tema:**
1. [ ] Clicar em ícone de sol/lua (se implementado)
2. [ ] Interface muda instantaneamente
3. [ ] Todas as cores invertem apropriadamente
4. [ ] Preferência salva

### 2.4 Acessibilidade

**Tamanho de Clique:**
- [ ] Todos os botões têm no mínimo 48×48px
- [ ] Espaçamento entre botões: 16px mínimo
- [ ] Botões não ficam muito próximos (evitar clique acidental)

**Navegação por Teclado:**
- [ ] Tab navega entre elementos interativos
- [ ] Enter/Space ativa botões
- [ ] Foco visível (outline)
- [ ] Ordem lógica de foco

**Feedback Visual:**
- [ ] Hover mostra mudança de cor/sombra
- [ ] Loading states visíveis (spinners, textos "Carregando...")
- [ ] Erros mostram mensagem clara (não só cor)

---

## 3. TESTES DE INTEGRAÇÃO (BACKEND)

### 3.1 Salvar e Carregar Dados

**Cenário 1: Student Progress**
1. [ ] Iniciar atividade
2. [ ] Fechar navegador no meio
3. [ ] Reabrir e fazer login
4. [ ] Verificar se pode continuar de onde parou
5. [ ] OU se precisa começar de novo (comportamento esperado)

**Verificar no Supabase:**
```sql
SELECT * FROM student_progress WHERE student_id = '[UUID do aluno]';
```

Deve mostrar:
- [ ] `status`: 'in_progress' ou 'completed'
- [ ] `started_at`: timestamp correto
- [ ] `interactions`: array com dados

**Cenário 2: Concept Mastery**
1. [ ] Completar atividade de fluência (25 questões)
2. [ ] Verificar atualização no banco:

```sql
SELECT * FROM concept_mastery WHERE student_id = '[UUID]';
```

- [ ] `attempts` = 25
- [ ] `correct` = número de acertos
- [ ] `incorrect` = 25 - acertos
- [ ] `mastery_level` = correct / attempts (ex: 0.80)

**Cenário 3: Jeremias State**
1. [ ] Ensinar Jeremias 5 vezes
2. [ ] Verificar:

```sql
SELECT * FROM jeremias_state WHERE student_id = '[UUID]';
```

- [ ] `corrections_made` = 5
- [ ] `maturity_level` = 2 (evoluiu de 1 para 2)
- [ ] `concepts_taught` contém conceitos corretos

---

### 3.2 Performance de Queries

**Testar Carregamento:**
1. [ ] Abrir DevTools (F12) > Network
2. [ ] Fazer login
3. [ ] Verificar tempo de carregamento do Dashboard:
   - [ ] Queries Supabase completam em <500ms
   - [ ] Dashboard renderiza em <2s total

**Queries Críticas:**
```sql
-- Deve ser RÁPIDA (<100ms)
SELECT * FROM students WHERE id = '[UUID]';

-- Deve ser RÁPIDA (<200ms)
SELECT * FROM teacher_dashboard;

-- Deve ser RÁPIDA (<150ms)
SELECT * FROM activities WHERE published = TRUE ORDER BY week_number DESC LIMIT 1;
```

**Se lento:**
- Verificar se índices existem
- Otimizar queries
- Considerar adicionar mais índices

---

## 4. TESTES DE EDGE CASES

### 4.1 Dados Inválidos

**Cenário 1: Input Vazio**
1. [ ] BateriaRápida: Tentar enviar sem digitar nada
2. [ ] Botão "Verificar" deve estar desabilitado

**Cenário 2: Input Não-Numérico**
1. [ ] BateriaRápida: Digitar "abc"
2. [ ] Input deve aceitar apenas números (type="number")

**Cenário 3: Nenhum Aluno Cadastrado**
1. [ ] Limpar tabela `students` temporariamente
2. [ ] Acessar página de login
3. [ ] Deve mostrar mensagem apropriada (não quebrar)

### 4.2 Conexão Perdida

**Cenário 1: Offline Durante Atividade**
1. [ ] Iniciar atividade
2. [ ] Desconectar internet
3. [ ] Tentar avançar módulo
4. [ ] Deve mostrar erro claro (não travar silenciosamente)

**Cenário 2: Reconexão**
1. [ ] Reconectar internet
2. [ ] Tentar novamente
3. [ ] Deve funcionar normalmente

### 4.3 Múltiplas Abas

**Cenário 1: Mesma Atividade em Duas Abas**
1. [ ] Abrir atividade em aba 1
2. [ ] Abrir mesma atividade em aba 2
3. [ ] Completar em aba 1
4. [ ] Verificar se aba 2 sincroniza (ou mostra aviso)

---

## 5. TESTES COM USUÁRIOS REAIS (UAT)

### 5.1 Preparação

**Antes de Testar com Alunos:**
- [ ] Todos os testes acima passaram
- [ ] Backup do banco de dados feito
- [ ] Ambiente de staging preparado (não produção)
- [ ] Cronômetro pronto (medir tempo real)
- [ ] Caderno para anotações de observações

### 5.2 Protocolo de Teste

**Selecionar 2-3 Alunos:**
- [ ] 1 aluno do 4º ano
- [ ] 1 aluno do 5º ou 6º ano
- [ ] Preferencialmente com níveis diferentes de habilidade

**Instruções:**
1. [ ] Explicar que é um teste (não avaliação deles)
2. [ ] Pedir para "pensar alto" enquanto usam
3. [ ] NÃO ajudar, apenas observar (ver onde travam)
4. [ ] Anotar:
   - Onde hesitam
   - O que tentam fazer e não conseguem
   - Erros que cometem
   - Comentários espontâneos

**Tarefas:**
1. [ ] Fazer login sozinho
2. [ ] Encontrar e começar atividade
3. [ ] Completar atividade inteira
4. [ ] Voltar ao dashboard

**Após Teste, Perguntar:**
- O que foi mais fácil?
- O que foi mais difícil?
- Algo foi confuso?
- Você usaria de novo?
- Sugestões de melhoria?

### 5.3 Métricas para Coletar

**Quantitativas:**
- [ ] Tempo para login: ___ segundos
- [ ] Tempo para completar atividade: ___ minutos
- [ ] Número de erros cometidos: ___
- [ ] Número de vezes que pediu ajuda: ___

**Qualitativas:**
- [ ] Conseguiu completar sozinho? (sim/não)
- [ ] Interface foi clara? (1-5)
- [ ] Se sentiu motivado? (1-5)
- [ ] Acha que aprendeu algo? (sim/não/talvez)

---

## 6. CHECKLIST DE REGRESSÃO (APÓS MUDANÇAS)

Sempre que fizer uma mudança significativa, rodar:

**Checklist Rápida (10 min):**
- [ ] Login funciona
- [ ] Dashboard carrega
- [ ] Consegue iniciar atividade
- [ ] Consegue completar 1 módulo
- [ ] Dados salvam corretamente

**Checklist Completa (1 hora):**
- [ ] Todos os itens da seção 1 (Testes Funcionais)
- [ ] Verificar responsividade básica
- [ ] Rodar queries de validação no banco

---

## 7. AUTOMAÇÃO (FUTURO)

Quando o projeto crescer, considerar:

### 7.1 Testes Unitários (Vitest)

```javascript
// Exemplo: Testar lógica de pontuação
import { calculateScore } from './scoring'

test('calculateScore retorna 0.8 para 8/10 acertos', () => {
  const interactions = [
    { isCorrect: true },
    { isCorrect: true },
    { isCorrect: false },
    { isCorrect: true },
    // ... 10 total
  ]
  expect(calculateScore(interactions)).toBe(0.8)
})
```

### 7.2 Testes E2E (Playwright)

```javascript
// Exemplo: Testar fluxo completo
test('aluno completa atividade', async ({ page }) => {
  await page.goto('http://localhost:5173')
  await page.click('text=Ana Silva')
  await page.click('text=Começar')
  // ... completar fluxo
  await expect(page).toHaveURL(/dashboard/)
})
```

---

## 8. BUG TRACKING

### 8.1 Template de Bug Report

Quando encontrar um bug, documentar:

```markdown
**Título:** [Breve descrição]

**Severidade:** Crítico / Alto / Médio / Baixo

**Passos para Reproduzir:**
1. 
2. 
3. 

**Comportamento Esperado:**
[O que deveria acontecer]

**Comportamento Atual:**
[O que está acontecendo]

**Screenshots/Console:**
[Anexar prints ou logs]

**Ambiente:**
- Navegador: 
- Resolução: 
- URL: 
```

### 8.2 Priorização

**Crítico (Consertar IMEDIATAMENTE):**
- Impede uso da aplicação
- Perda de dados
- Erro de segurança

**Alto (Consertar antes de usar com alunos):**
- Feature principal não funciona
- UX muito ruim
- Dados incorretos

**Médio (Consertar em próxima iteração):**
- Feature secundária com problema
- UX poderia ser melhor
- Performance aceitável mas não ideal

**Baixo (Backlog):**
- Melhoria estética
- Edge case raro
- Nice-to-have

---

## 9. CHECKLIST FINAL PRÉ-LANÇAMENTO

Antes de usar com alunos reais:

### Funcional
- [ ] Todos os testes da seção 1 passaram
- [ ] Testado com 2-3 alunos reais (UAT)
- [ ] Bugs críticos e altos corrigidos
- [ ] Performance aceitável (<3s para carregar)

### Dados
- [ ] Backup do banco feito
- [ ] Seeds de produção inseridos (alunos reais)
- [ ] Atividades da semana publicadas

### Deploy
- [ ] Deploy em produção realizado
- [ ] URL acessível
- [ ] Variáveis de ambiente corretas
- [ ] Logs monitorados

### Documentação
- [ ] Professor sabe como acessar
- [ ] Instruções de uso básicas prontas
- [ ] Plano de contingência (se der problema)

### Contingência
- [ ] Atividade backup em papel (se sistema cair)
- [ ] Contato de suporte (você) disponível durante aula
- [ ] Rollback pronto (se necessário voltar versão anterior)

---

## 10. MÉTRICAS DE SUCESSO (PÓS-LANÇAMENTO)

Após 1 semana de uso real, coletar:

**Adoção:**
- [ ] ___ % de alunos completaram atividade
- [ ] Tempo médio de conclusão: ___ min
- [ ] Taxa de abandono: ___ %

**Qualidade:**
- [ ] Número de bugs reportados: ___
- [ ] Satisfação dos alunos (1-5): ___
- [ ] Satisfação do professor (1-5): ___

**Pedagógico:**
- [ ] Alunos demonstraram compreensão? (observação)
- [ ] Houve melhoria em avaliações externas? (se aplicável)

Se métricas não forem boas, iterar! 🔄

---

**Bons testes! 🧪**