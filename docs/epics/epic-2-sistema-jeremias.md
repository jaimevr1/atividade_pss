# Epic 2: Sistema do Personagem Jeremias

**ID:** EPIC-002
**Status:** Planejado
**Prioridade:** Must Have (MVP)
**Estimativa:** 2 semanas

## Objetivo

Implementar o personagem virtual Jeremias que comete erros conceituais para ativação da técnica Feynman, permitindo que alunos ensinem e fortaleçam sua própria compreensão.

## Valor de Negócio

Diferencial pedagógico único que ensina através do ensino. Baseado na técnica Feynman, transforma erros em oportunidades de aprendizado e desenvolve metacognição nos estudantes.

## User Stories Incluídas

- **US-004:** M10 - JeremiasResolver (ensino do Jeremias)
- **US-005:** M20 - EstadoJeremias (gestão emocional/narrativa)

## Critérios de Aceitação

- [ ] Sistema de estados emocionais (6 moods: neutral, thinking, confused, eureka, confident, grateful)
- [ ] Catálogo de erros conceituais organizados por nível de maturidade
- [ ] Sistema de evolução/maturidade (5 níveis: Iniciante → Mestre)
- [ ] Integração com banco de dados para persistência de estado
- [ ] Sistema de diálogos adaptativos baseados no nível
- [ ] Animações de transição de mood (500ms bounce effect)
- [ ] Avatar emoji 96x96px + nome "Jeremias"
- [ ] Fonte Comic Sans MS para diálogos do personagem

## Catálogo de Erros (Exemplos)

### Matemática
- **Nível 1:** Confunde operações básicas (+ vs ×)
- **Nível 2:** Esquece reagrupamento em multiplicação
- **Nível 3:** Ordem incorreta em problemas verbais
- **Nível 4:** Erros sutis em casos especiais
- **Nível 5:** Não comete erros (modo "ensino reverso")

### Português
- **Nível 1:** Troca ss/s/ç
- **Nível 2:** Confunde mais/mas
- **Nível 3:** Interpretação literal demais

## Estados de Maturidade

```
Nível 1: "Estou começando a aprender isso..."
Nível 2: "Já melhorei, mas ainda confundo algumas coisas"
Nível 3: "Estou ficando bom nisso!"
Nível 4: "Raramente erro agora. Valeu pela ajuda!"
Nível 5: "Agora eu ensino você! Cria um problema para EU resolver?"
```

## Dependências

- **Pré-requisitos:** Epic 1 (módulos base), sistema de dados
- **Bloqueia:** Atividades de integração avançadas

## Definição de Pronto

- [ ] Catálogo completo de erros para habilidades MVP
- [ ] Sistema de progressão funcional (5 correções = +1 nível)
- [ ] Persistência de estado no Supabase
- [ ] Testes de narrativa com usuários 9-12 anos
- [ ] Animações suaves e não-distrativas
- [ ] Integração com módulos M1, M5, M14

## Riscos

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Jeremias é irritante/infantil | Média | Alto | Teste com usuários, linguagem respeitosa |
| Erros muito fáceis/difíceis | Média | Médio | Calibração com professores |
| Complexidade do sistema de estados | Baixa | Médio | Prototipagem incremental |

## Métricas de Sucesso

- **Engagement:** >80% completam interação com Jeremias
- **Pedagógica:** Alunos identificam erro correto >70% das vezes
- **Emocional:** <5% feedback negativo sobre o personagem
- **Técnica:** Transições de estado <200ms