# Dimidui - Backend Completo (PostgreSQL Supabase)

**Database:** PostgreSQL 14+ (Supabase)  
**Instruções:** Executar scripts na ordem apresentada no SQL Editor do Supabase

---

## 1. SETUP INICIAL DO PROJETO SUPABASE

### 1.1 Criar Projeto

1. Acesse [supabase.com](https://supabase.com)
2. Clique em "New Project"
3. Preencha:
   - **Name:** dimidui-production (ou nome desejado)
   - **Database Password:** [gere uma senha forte]
   - **Region:** South America (São Paulo) - mais próximo do Brasil
4. Aguarde provisionamento (~2 minutos)

### 1.2 Obter Credenciais

Após criação, vá em **Settings > API**:
- **URL:** `https://[seu-projeto].supabase.co`
- **anon key:** (chave pública)
- **service_role key:** (chave privada - não expor)

Copie estas credenciais para o `.env` do frontend.

---

## 2. SCHEMA COMPLETO (COPIAR E EXECUTAR)

### 2.1 Habilitar UUID Extension

```sql
-- Habilitar extensão UUID (se não estiver habilitada)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

### 2.2 Criar Tabelas

**Executar no SQL Editor (aba SQL Editor > New Query):**

```sql
-- ==============================================
-- TABELA: students
-- Armazena dados dos alunos
-- ==============================================
CREATE TABLE students (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  grade_level INT NOT NULL CHECK (grade_level IN (4, 5, 6)),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  
  -- Preferências de interface
  font_preference TEXT DEFAULT 'atkinson' CHECK (font_preference IN ('atkinson', 'opendyslexic')),
  theme_preference TEXT DEFAULT 'light' CHECK (theme_preference IN ('light', 'dark')),
  
  -- Constraints
  CONSTRAINT students_name_not_empty CHECK (length(trim(name)) > 0)
);

-- Índices para performance
CREATE INDEX idx_students_name ON students(name);
CREATE INDEX idx_students_grade ON students(grade_level);

-- ==============================================
-- TABELA: activities
-- Armazena configurações de atividades
-- ==============================================
CREATE TABLE activities (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  week_number INT,
  type TEXT NOT NULL CHECK (type IN ('fluencia', 'integracao')),
  bncc_codes TEXT[] DEFAULT '{}',
  estimated_duration INT NOT NULL DEFAULT 20, -- minutos
  
  -- Configuração JSON dos módulos
  modules JSONB NOT NULL DEFAULT '[]',
  
  -- Metadados
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  published BOOLEAN DEFAULT FALSE,
  
  -- Constraints
  CONSTRAINT activities_name_not_empty CHECK (length(trim(name)) > 0),
  CONSTRAINT activities_duration_positive CHECK (estimated_duration > 0)
);

-- Índices
CREATE INDEX idx_activities_published ON activities(published) WHERE published = TRUE;
CREATE INDEX idx_activities_week ON activities(week_number);
CREATE INDEX idx_activities_type ON activities(type);

-- ==============================================
-- TABELA: student_progress
-- Rastreia progresso dos alunos nas atividades
-- ==============================================
CREATE TABLE student_progress (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID NOT NULL REFERENCES students(id) ON DELETE CASCADE,
  activity_id UUID NOT NULL REFERENCES activities(id) ON DELETE CASCADE,
  
  -- Status da atividade
  status TEXT NOT NULL DEFAULT 'not_started' 
    CHECK (status IN ('not_started', 'in_progress', 'completed')),
  
  -- Timestamps
  started_at TIMESTAMPTZ,
  completed_at TIMESTAMPTZ,
  time_spent INT DEFAULT 0, -- segundos
  
  -- Dados de desempenho
  interactions JSONB DEFAULT '[]', -- Array de cada interação
  final_score FLOAT CHECK (final_score >= 0 AND final_score <= 1),
  
  -- Auditoria
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  
  -- Constraints
  UNIQUE(student_id, activity_id),
  CONSTRAINT progress_time_positive CHECK (time_spent >= 0),
  CONSTRAINT progress_dates_logical CHECK (
    (completed_at IS NULL) OR (started_at IS NOT NULL AND completed_at >= started_at)
  )
);

-- Índices
CREATE INDEX idx_progress_student ON student_progress(student_id);
CREATE INDEX idx_progress_activity ON student_progress(activity_id);
CREATE INDEX idx_progress_status ON student_progress(status);
CREATE INDEX idx_progress_completed ON student_progress(completed_at) WHERE status = 'completed';

-- ==============================================
-- TABELA: concept_mastery
-- Rastreia domínio de conceitos BNCC por aluno
-- ==============================================
CREATE TABLE concept_mastery (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID NOT NULL REFERENCES students(id) ON DELETE CASCADE,
  bncc_code TEXT NOT NULL,
  
  -- Métricas de desempenho
  attempts INT DEFAULT 0 CHECK (attempts >= 0),
  correct INT DEFAULT 0 CHECK (correct >= 0),
  incorrect INT DEFAULT 0 CHECK (incorrect >= 0),
  mastery_level FLOAT DEFAULT 0.0 CHECK (mastery_level >= 0 AND mastery_level <= 1),
  
  -- Timestamps
  first_practiced TIMESTAMPTZ DEFAULT NOW(),
  last_practiced TIMESTAMPTZ DEFAULT NOW(),
  
  -- Constraints
  UNIQUE(student_id, bncc_code),
  CONSTRAINT mastery_correct_lte_attempts CHECK (correct <= attempts),
  CONSTRAINT mastery_incorrect_lte_attempts CHECK (incorrect <= attempts),
  CONSTRAINT mastery_sum_equals_attempts CHECK (correct + incorrect = attempts)
);

-- Índices
CREATE INDEX idx_mastery_student ON concept_mastery(student_id);
CREATE INDEX idx_mastery_code ON concept_mastery(bncc_code);
CREATE INDEX idx_mastery_level ON concept_mastery(mastery_level);

-- ==============================================
-- TABELA: jeremias_state
-- Estado do personagem Jeremias por aluno
-- ==============================================
CREATE TABLE jeremias_state (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID NOT NULL REFERENCES students(id) ON DELETE CASCADE,
  
  -- Estado de evolução
  maturity_level INT DEFAULT 1 CHECK (maturity_level BETWEEN 1 AND 5),
  concepts_taught TEXT[] DEFAULT '{}',
  
  -- Contadores
  times_taught INT DEFAULT 0 CHECK (times_taught >= 0),
  corrections_made INT DEFAULT 0 CHECK (corrections_made >= 0),
  
  -- Estado narrativo
  last_dialogue TEXT,
  last_mood TEXT CHECK (last_mood IN ('neutral', 'thinking', 'confused', 'eureka', 'confident', 'grateful')),
  last_interaction TIMESTAMPTZ,
  
  -- Timestamps
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  
  -- Constraints
  UNIQUE(student_id)
);

-- Índices
CREATE INDEX idx_jeremias_student ON jeremias_state(student_id);
CREATE INDEX idx_jeremias_level ON jeremias_state(maturity_level);

-- ==============================================
-- TRIGGERS: Atualizar updated_at automaticamente
-- ==============================================
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_activities_updated_at
  BEFORE UPDATE ON activities
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_student_progress_updated_at
  BEFORE UPDATE ON student_progress
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_jeremias_state_updated_at
  BEFORE UPDATE ON jeremias_state
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();
```

---

## 3. VIEWS ÚTEIS

```sql
-- ==============================================
-- VIEW: teacher_dashboard
-- Visão agregada para dashboard do professor
-- ==============================================
CREATE OR REPLACE VIEW teacher_dashboard AS
SELECT 
  s.id AS student_id,
  s.name AS student_name,
  s.grade_level,
  
  -- Estatísticas de atividades
  COUNT(DISTINCT sp.activity_id) FILTER (WHERE sp.status = 'completed') AS activities_completed,
  AVG(sp.final_score) AS avg_score,
  SUM(sp.time_spent) AS total_time_spent,
  
  -- Estado do Jeremias
  COALESCE(j.maturity_level, 1) AS jeremias_level,
  COALESCE(j.corrections_made, 0) AS jeremias_corrections,
  
  -- Conceitos dominados
  COUNT(DISTINCT cm.bncc_code) FILTER (WHERE cm.mastery_level >= 0.75) AS concepts_mastered,
  COUNT(DISTINCT cm.bncc_code) AS concepts_total,
  
  -- Última atividade
  MAX(sp.completed_at) AS last_activity_date
  
FROM students s
LEFT JOIN student_progress sp ON s.id = sp.student_id
LEFT JOIN jeremias_state j ON s.id = j.student_id
LEFT JOIN concept_mastery cm ON s.id = cm.student_id

GROUP BY s.id, s.name, s.grade_level, j.maturity_level, j.corrections_made
ORDER BY s.name;

-- ==============================================
-- VIEW: concept_mastery_summary
-- Resumo de domínio de conceitos por turma
-- ==============================================
CREATE OR REPLACE VIEW concept_mastery_summary AS
SELECT 
  cm.bncc_code,
  COUNT(DISTINCT cm.student_id) AS total_students,
  AVG(cm.mastery_level) AS avg_mastery,
  COUNT(*) FILTER (WHERE cm.mastery_level >= 0.75) AS students_mastered,
  COUNT(*) FILTER (WHERE cm.mastery_level < 0.5) AS students_struggling,
  SUM(cm.attempts) AS total_attempts,
  SUM(cm.correct) AS total_correct,
  CASE 
    WHEN SUM(cm.attempts) > 0 
    THEN ROUND((SUM(cm.correct)::NUMERIC / SUM(cm.attempts)) * 100, 2)
    ELSE 0 
  END AS overall_accuracy
FROM concept_mastery cm
GROUP BY cm.bncc_code
ORDER BY avg_mastery ASC;

-- ==============================================
-- VIEW: recent_activity_log
-- Log recente de atividades completadas
-- ==============================================
CREATE OR REPLACE VIEW recent_activity_log AS
SELECT 
  sp.id,
  s.name AS student_name,
  a.name AS activity_name,
  a.type AS activity_type,
  sp.final_score,
  sp.time_spent,
  sp.completed_at
FROM student_progress sp
JOIN students s ON sp.student_id = s.id
JOIN activities a ON sp.activity_id = a.id
WHERE sp.status = 'completed'
ORDER BY sp.completed_at DESC
LIMIT 50;
```

---

## 4. FUNCTIONS ÚTEIS

```sql
-- ==============================================
-- FUNCTION: Atualizar concept_mastery após atividade
-- ==============================================
CREATE OR REPLACE FUNCTION update_concept_mastery_from_activity(
  p_student_id UUID,
  p_activity_id UUID,
  p_interactions JSONB
)
RETURNS VOID AS $$
DECLARE
  v_bncc_codes TEXT[];
  v_bncc_code TEXT;
  v_correct INT;
  v_total INT;
BEGIN
  -- Buscar códigos BNCC da atividade
  SELECT bncc_codes INTO v_bncc_codes
  FROM activities
  WHERE id = p_activity_id;
  
  -- Para cada código BNCC
  FOREACH v_bncc_code IN ARRAY v_bncc_codes
  LOOP
    -- Calcular acertos e total de tentativas das interações
    -- (Assumindo que p_interactions tem formato: [{ "isCorrect": true/false }])
    SELECT 
      COUNT(*) FILTER (WHERE (value->>'isCorrect')::BOOLEAN = TRUE),
      COUNT(*)
    INTO v_correct, v_total
    FROM jsonb_array_elements(p_interactions);
    
    -- Atualizar ou inserir concept_mastery
    INSERT INTO concept_mastery (student_id, bncc_code, attempts, correct, incorrect, mastery_level, last_practiced)
    VALUES (
      p_student_id,
      v_bncc_code,
      v_total,
      v_correct,
      v_total - v_correct,
      CASE WHEN v_total > 0 THEN v_correct::FLOAT / v_total ELSE 0 END,
      NOW()
    )
    ON CONFLICT (student_id, bncc_code) 
    DO UPDATE SET
      attempts = concept_mastery.attempts + v_total,
      correct = concept_mastery.correct + v_correct,
      incorrect = concept_mastery.incorrect + (v_total - v_correct),
      mastery_level = CASE 
        WHEN (concept_mastery.attempts + v_total) > 0 
        THEN (concept_mastery.correct + v_correct)::FLOAT / (concept_mastery.attempts + v_total)
        ELSE 0 
      END,
      last_practiced = NOW();
  END LOOP;
END;
$$ LANGUAGE plpgsql;

-- ==============================================
-- FUNCTION: Atualizar Jeremias após correção
-- ==============================================
CREATE OR REPLACE FUNCTION update_jeremias_after_teaching(
  p_student_id UUID,
  p_concept_taught TEXT,
  p_was_correct BOOLEAN
)
RETURNS VOID AS $$
DECLARE
  v_current_corrections INT;
  v_current_level INT;
BEGIN
  -- Buscar estado atual
  SELECT corrections_made, maturity_level
  INTO v_current_corrections, v_current_level
  FROM jeremias_state
  WHERE student_id = p_student_id;
  
  -- Se não existe registro, criar
  IF NOT FOUND THEN
    INSERT INTO jeremias_state (student_id, maturity_level, concepts_taught, corrections_made, times_taught)
    VALUES (
      p_student_id,
      1,
      ARRAY[p_concept_taught],
      CASE WHEN p_was_correct THEN 1 ELSE 0 END,
      1
    );
    RETURN;
  END IF;
  
  -- Atualizar registro existente
  UPDATE jeremias_state
  SET
    concepts_taught = array_append(
      CASE WHEN p_concept_taught = ANY(concepts_taught) 
      THEN concepts_taught 
      ELSE array_append(concepts_taught, p_concept_taught) 
      END,
      NULL -- Remove NULL imediatamente
    ),
    times_taught = times_taught + 1,
    corrections_made = CASE WHEN p_was_correct THEN corrections_made + 1 ELSE corrections_made END,
    
    -- Evoluir nível a cada 5 correções
    maturity_level = CASE 
      WHEN p_was_correct AND ((corrections_made + 1) % 5 = 0) AND maturity_level < 5
      THEN maturity_level + 1
      ELSE maturity_level
    END,
    
    last_interaction = NOW()
  WHERE student_id = p_student_id;
  
  -- Limpar NULLs do array
  UPDATE jeremias_state
  SET concepts_taught = array_remove(concepts_taught, NULL)
  WHERE student_id = p_student_id;
END;
$$ LANGUAGE plpgsql;

-- ==============================================
-- FUNCTION: Verificar pré-requisitos de atividade
-- ==============================================
CREATE OR REPLACE FUNCTION check_activity_prerequisites(
  p_student_id UUID,
  p_activity_id UUID
)
RETURNS BOOLEAN AS $$
DECLARE
  v_required_bncc_codes TEXT[];
  v_bncc_code TEXT;
  v_mastery_level FLOAT;
  v_threshold FLOAT := 0.75;
BEGIN
  -- Buscar códigos BNCC necessários da atividade
  SELECT bncc_codes INTO v_required_bncc_codes
  FROM activities
  WHERE id = p_activity_id;
  
  -- Se não tem pré-requisitos, libera
  IF v_required_bncc_codes IS NULL OR array_length(v_required_bncc_codes, 1) = 0 THEN
    RETURN TRUE;
  END IF;
  
  -- Verificar cada pré-requisito
  FOREACH v_bncc_code IN ARRAY v_required_bncc_codes
  LOOP
    SELECT mastery_level INTO v_mastery_level
    FROM concept_mastery
    WHERE student_id = p_student_id AND bncc_code = v_bncc_code;
    
    -- Se não praticou ainda ou está abaixo do threshold
    IF v_mastery_level IS NULL OR v_mastery_level < v_threshold THEN
      RETURN FALSE;
    END IF;
  END LOOP;
  
  -- Todos os pré-requisitos atendidos
  RETURN TRUE;
END;
$$ LANGUAGE plpgsql;
```

---

## 5. ROW LEVEL SECURITY (RLS)

```sql
-- ==============================================
-- Habilitar RLS em todas as tabelas
-- ==============================================
ALTER TABLE students ENABLE ROW LEVEL SECURITY;
ALTER TABLE activities ENABLE ROW LEVEL SECURITY;
ALTER TABLE student_progress ENABLE ROW LEVEL SECURITY;
ALTER TABLE concept_mastery ENABLE ROW LEVEL SECURITY;
ALTER TABLE jeremias_state ENABLE ROW LEVEL SECURITY;

-- ==============================================
-- POLICIES: students
-- Alunos podem ler apenas seus próprios dados
-- ==============================================
CREATE POLICY "students_select_own"
  ON students FOR SELECT
  USING (true); -- Todos podem ver lista para login

CREATE POLICY "students_update_own"
  ON students FOR UPDATE
  USING (auth.uid()::TEXT = id::TEXT); -- Apenas próprio registro (se usar auth)

-- ==============================================
-- POLICIES: activities
-- Alunos podem ler apenas atividades publicadas
-- ==============================================
CREATE POLICY "activities_select_published"
  ON activities FOR SELECT
  USING (published = TRUE);

CREATE POLICY "activities_all_for_admin"
  ON activities FOR ALL
  USING (true); -- TODO: Adicionar verificação de admin

-- ==============================================
-- POLICIES: student_progress
-- Alunos acessam apenas próprio progresso
-- ==============================================
CREATE POLICY "progress_select_own"
  ON student_progress FOR SELECT
  USING (true); -- Temporário: todos podem ver (ajustar depois)

CREATE POLICY "progress_insert_own"
  ON student_progress FOR INSERT
  WITH CHECK (true);

CREATE POLICY "progress_update_own"
  ON student_progress FOR UPDATE
  USING (true);

-- ==============================================
-- POLICIES: concept_mastery
-- ==============================================
CREATE POLICY "mastery_select_own"
  ON concept_mastery FOR SELECT
  USING (true);

CREATE POLICY "mastery_insert_own"
  ON concept_mastery FOR INSERT
  WITH CHECK (true);

CREATE POLICY "mastery_update_own"
  ON concept_mastery FOR UPDATE
  USING (true);

-- ==============================================
-- POLICIES: jeremias_state
-- ==============================================
CREATE POLICY "jeremias_select_own"
  ON jeremias_state FOR SELECT
  USING (true);

CREATE POLICY "jeremias_insert_own"
  ON jeremias_state FOR INSERT
  WITH CHECK (true);

CREATE POLICY "jeremias_update_own"
  ON jeremias_state FOR UPDATE
  USING (true);

-- NOTA: As policies acima são permissivas (true) para MVP.
-- Em produção, ajustar para verificar student_id = auth.uid() após implementar auth real.
```

---

## 6. DADOS INICIAIS (SEEDS)

```sql
-- ==============================================
-- SEED: Alunos de exemplo
-- ==============================================
INSERT INTO students (name, grade_level, font_preference, theme_preference) VALUES
('Ana Silva', 4, 'atkinson', 'light'),
('Bruno Santos', 5, 'atkinson', 'dark'),
('Carlos Oliveira', 6, 'opendyslexic', 'light'),
('Daniela Costa', 4, 'atkinson', 'light'),
('Eduardo Lima', 5, 'atkinson', 'dark');

-- ==============================================
-- SEED: Atividade de exemplo (Fluência)
-- ==============================================
INSERT INTO activities (name, week_number, type, bncc_codes, estimated_duration, modules, published) VALUES
(
  'Multiplicação - Prática Rápida',
  1,
  'fluencia',
  ARRAY['EF04MA07', 'EF04MA06'],
  20,
  '[
    {
      "type": "estado_jeremias",
      "config": {
        "mood": "confident",
        "dialogue": "Hoje vamos praticar multiplicação! Estou pronto!"
      }
    },
    {
      "type": "bateria_rapida",
      "config": {
        "concept": "multiplicação",
        "quantity": 25,
        "operations": {
          "min": [2, 2],
          "max": [9, 9]
        }
      }
    },
    {
      "type": "auto_avaliacao",
      "config": {
        "concepts": ["Tabuada até 9"],
        "scale": ["Preciso praticar mais", "Estou melhorando", "Já domino"]
      }
    }
  ]'::jsonb,
  TRUE
);

-- ==============================================
-- SEED: Atividade de exemplo (Integração)
-- ==============================================
INSERT INTO activities (name, week_number, type, bncc_codes, estimated_duration, modules, published) VALUES
(
  'Jeremias Aprende Divisão',
  2,
  'integracao',
  ARRAY['EF05MA03', 'EF04LP15'],
  20,
  '[
    {
      "type": "estado_jeremias",
      "config": {
        "mood": "thinking",
        "dialogue": "Divisão com resto é complicado, mas vou tentar!"
      }
    },
    {
      "type": "jeremias_resolver",
      "config": {
        "problem": {
          "text": "Maria tem 23 figurinhas para dividir igualmente entre 4 amigos. Quantas cada um recebe? Sobram figurinhas?",
          "correctAnswer": "5 com resto 3"
        },
        "alexAttempt": {
          "reasoning": [
            "Preciso dividir 23 por 4",
            "23 ÷ 4 = 5",
            "Então cada amigo recebe 5 figurinhas"
          ],
          "errorStep": 2,
          "errorType": "ignora_resto"
        },
        "correctionPrompts": [
          "Em qual passo Jeremias errou?",
          "Qual foi o erro?"
        ]
      }
    },
    {
      "type": "auto_avaliacao",
      "config": {
        "concepts": ["Divisão com resto"],
        "scale": ["Ainda confuso", "Entendo o básico", "Sei fazer sozinho"]
      }
    }
  ]'::jsonb,
  TRUE
);

-- ==============================================
-- SEED: Estados iniciais do Jeremias
-- ==============================================
INSERT INTO jeremias_state (student_id, maturity_level, concepts_taught, last_dialogue, last_mood)
SELECT 
  id,
  1,
  ARRAY[]::TEXT[],
  'Olá! Sou o Jeremias. Estou ansioso para aprender com você!',
  'neutral'
FROM students;
```

---

## 7. QUERIES ÚTEIS PARA DEBUGGING

```sql
-- ==============================================
-- Ver todos os alunos e suas estatísticas
-- ==============================================
SELECT * FROM teacher_dashboard;

-- ==============================================
-- Ver domínio de conceitos por código BNCC
-- ==============================================
SELECT * FROM concept_mastery_summary;

-- ==============================================
-- Ver últimas atividades completadas
-- ==============================================
SELECT * FROM recent_activity_log;

-- ==============================================
-- Ver progresso de um aluno específico
-- ==============================================
SELECT 
  s.name,
  a.name AS activity,
  sp.status,
  sp.final_score,
  sp.time_spent,
  sp.completed_at
FROM student_progress sp
JOIN students s ON sp.student_id = s.id
JOIN activities a ON sp.activity_id = a.id
WHERE s.name = 'Ana Silva'
ORDER BY sp.completed_at DESC;

-- ==============================================
-- Ver evolução do Jeremias por aluno
-- ==============================================
SELECT 
  s.name,
  j.maturity_level,
  array_length(j.concepts_taught, 1) AS concepts_count,
  j.corrections_made,
  j.last_interaction
FROM jeremias_state j
JOIN students s ON j.student_id = s.id
ORDER BY j.maturity_level DESC;

-- ==============================================
-- Ver conceitos que precisam de atenção (baixo domínio)
-- ==============================================
SELECT 
  s.name,
  cm.bncc_code,
  cm.mastery_level,
  cm.attempts,
  cm.correct,
  cm.incorrect
FROM concept_mastery cm
JOIN students s ON cm.student_id = s.id
WHERE cm.mastery_level < 0.5
ORDER BY cm.mastery_level ASC;

-- ==============================================
-- Ver distribuição de desempenho por tipo de atividade
-- ==============================================
SELECT 
  a.type,
  COUNT(*) AS total_completed,
  AVG(sp.final_score) AS avg_score,
  AVG(sp.time_spent) AS avg_time_seconds
FROM student_progress sp
JOIN activities a ON sp.activity_id = a.id
WHERE sp.status = 'completed'
GROUP BY a.type;

-- ==============================================
-- Resetar progresso de um aluno (para testes)
-- ==============================================
-- CUIDADO: Isso apaga todos os dados do aluno!
-- DELETE FROM student_progress WHERE student_id = '[UUID do aluno]';
-- DELETE FROM concept_mastery WHERE student_id = '[UUID do aluno]';
-- UPDATE jeremias_state SET 
--   maturity_level = 1, 
--   concepts_taught = '{}', 
--   corrections_made = 0,
--   times_taught = 0
-- WHERE student_id = '[UUID do aluno]';
```

---

## 8. BACKUP E MANUTENÇÃO

### 8.1 Backup Manual

No Supabase Dashboard:
1. **Database > Backups**
2. Clique em "Create Backup"
3. Backups automáticos diários estão habilitados por padrão

### 8.2 Exportar Dados (CSV)

```sql
-- Exportar dados de alunos
COPY (SELECT * FROM students) TO '/tmp/students.csv' WITH CSV HEADER;

-- Exportar progresso
COPY (SELECT * FROM teacher_dashboard) TO '/tmp/dashboard.csv' WITH CSV HEADER;
```

### 8.3 Limpar Dados Antigos (Opcional)

```sql
-- Deletar atividades completadas há mais de 6 meses
DELETE FROM student_progress
WHERE status = 'completed' 
  AND completed_at < NOW() - INTERVAL '6 months';

-- Arquivar ao invés de deletar (melhor prática)
-- CREATE TABLE student_progress_archive (LIKE student_progress INCLUDING ALL);
-- INSERT INTO student_progress_archive SELECT * FROM student_progress WHERE ...;
```

---

## 9. MONITORAMENTO E LOGS

### 9.1 Queries Lentas

No Supabase Dashboard > **Database > Query Performance**

Ou via SQL:
```sql
SELECT 
  query,
  calls,
  total_time,
  mean_time,
  max_time
FROM pg_stat_statements
ORDER BY mean_time DESC
LIMIT 20;
```

### 9.2 Tamanho das Tabelas

```sql
SELECT 
  schemaname,
  tablename,
  pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

---

## 10. SEGURANÇA E BOAS PRÁTICAS

### 10.1 Checklist de Segurança

- [ ] RLS habilitado em todas as tabelas
- [ ] Policies revisadas (não deixar `true` em produção)
- [ ] service_role key nunca exposta no frontend
- [ ] anon key pode ser exposta (pública)
- [ ] Backups automáticos habilitados
- [ ] SSL/TLS habilitado (padrão no Supabase)

### 10.2 Validação de Dados

Sempre validar no backend (constraints SQL) E no frontend:
- Strings não vazias
- Números dentro de ranges válidos
- Arrays não nulos
- JSON bem formado

### 10.3 Rate Limiting

Supabase tem rate limiting padrão:
- Anonymous: 100 requests/min
- Authenticated: 1000 requests/min

Monitore em **Settings > API > Rate Limits**

---

## 11. TROUBLESHOOTING COMUM

### Erro: "relation does not exist"
**Causa:** Tabela não foi criada ou nome errado  
**Solução:** Verificar se executou todos os CREATEs

### Erro: "permission denied for table"
**Causa:** RLS bloqueando acesso  
**Solução:** Revisar policies ou desabilitar RLS temporariamente para debug

### Erro: "duplicate key value violates unique constraint"
**Causa:** Tentando inserir registro duplicado (UNIQUE)  
**Solução:** Verificar se registro já existe antes de INSERT ou usar UPSERT

### Performance lenta em queries
**Causa:** Falta de índices  
**Solução:** Adicionar índices nas colunas usadas em WHERE/JOIN

### JSONB não está indexado
**Causa:** Queries em campos JSONB são lentas sem índice  
**Solução:** Criar índice GIN:
```sql
CREATE INDEX idx_activities_modules ON activities USING GIN (modules);
```

---

## 12. EXEMPLOS DE USO DA API (Frontend)

### 12.1 Buscar Atividade da Semana

```javascript
const { data, error } = await supabase
  .from('activities')
  .select('*')
  .eq('published', true)
  .order('week_number', { ascending: false })
  .limit(1)
  .single()
```

### 12.2 Salvar Progresso

```javascript
const { error } = await supabase
  .from('student_progress')
  .upsert({
    student_id: studentId,
    activity_id: activityId,
    status: 'in_progress',
    started_at: new Date().toISOString(),
    interactions: []
  })
```

### 12.3 Atualizar Concept Mastery

```javascript
const { error } = await supabase
  .rpc('update_concept_mastery_from_activity', {
    p_student_id: studentId,
    p_activity_id: activityId,
    p_interactions: interactions
  })
```

### 12.4 Atualizar Jeremias

```javascript
const { error } = await supabase
  .rpc('update_jeremias_after_teaching', {
    p_student_id: studentId,
    p_concept_taught: 'multiplicação',
    p_was_correct: true
  })
```

### 12.5 Verificar Pré-requisitos

```javascript
const { data, error } = await supabase
  .rpc('check_activity_prerequisites', {
    p_student_id: studentId,
    p_activity_id: activityId
  })

// data será true ou false
if (data) {
  // Aluno pode fazer a atividade
} else {
  // Aluno precisa completar pré-requisitos
}
```

---

## 13. MIGRATIONS (Futuras Alterações)

Quando precisar alterar o schema em produção:

```sql
-- Exemplo: Adicionar nova coluna
ALTER TABLE students 
ADD COLUMN email TEXT;

-- Exemplo: Adicionar constraint
ALTER TABLE students 
ADD CONSTRAINT students_email_unique UNIQUE (email);

-- Exemplo: Criar índice
CREATE INDEX idx_students_email ON students(email);
```

**IMPORTANTE:** Sempre testar migrations em ambiente de desenvolvimento primeiro!

---

## 14. RESTORE (Restaurar de Backup)

No Supabase Dashboard:
1. **Database > Backups**
2. Encontre o backup desejado
3. Clique em "Restore"
4. Confirme (ISSO SOBRESCREVE O BANCO ATUAL!)

---

## 15. CHECKLIST FINAL

Após executar todos os scripts:

- [ ] Tabelas criadas (5 tabelas)
- [ ] Índices criados
- [ ] Triggers funcionando (updated_at atualiza)
- [ ] Views criadas (3 views)
- [ ] Functions criadas (3 functions)
- [ ] RLS habilitado
- [ ] Policies configuradas
- [ ] Seeds inseridos (alunos e atividades de exemplo)
- [ ] Queries de teste executadas com sucesso
- [ ] Credenciais copiadas para .env do frontend

---

**FIM DA ESPECIFICAÇÃO BACKEND**

**Próximo Passo:** Copiar VITE_SUPABASE_URL e VITE_SUPABASE_ANON_KEY para o `.env` do frontend e começar desenvolvimento!