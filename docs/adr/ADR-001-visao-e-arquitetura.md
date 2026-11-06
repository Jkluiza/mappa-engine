# ADR 001: Arquitetura Base, Canais de Ingestão e Persistência Vetorial

* **Status:** Aceito
* **Data:** 2026-11-06
* **Decisores:** Jéssica Oliveira (Lead Engineer)

---

## 1. Contexto e Problema
A dispersão da atenção digital gera uma assimetria severa entre a intenção intelectual declarada do indivíduo e o consumo real fragmentado no cotidiano. O **Mappa** nasce como um motor analítico e metacognitivo para auditar o desvio semântico (*Cognitive Drift*) entre tópicos intencionais e pensamentos residuais, fornecendo microdoses socráticas sem atuar como consultoria clínica ou app genérico de produtividade.

## 2. Decisões de Arquitetura

### 2.1. Ingestão por Mensageria (Telegram Bot Webhook)
* **Decisão:** Ingestão primária de texto e áudio curto via Telegram Bot API consumida assincronamente por endpoints em FastAPI.
* **Justificativa:** Elimina a fricção de login e abertura de apps pesados em momentos de saturação mental.

### 2.2. Core Backend em Python Assíncrono (FastAPI)
* **Decisão:** Uso do FastAPI com contratos em Pydantic v2 e orquestração assíncrona (`asyncio`).
* **Justificativa:** Padrão de indústria para microsserviços de IA, alta velocidade de processamento (< 500ms para liberação de webhooks) e validação tipada estrita em tempo de execução.

### 2.3. Persistência Híbrida (PostgreSQL + pgvector)
* **Decisão:** Banco de dados relacional unificado rodando PostgreSQL 16 com extensão `pgvector` em contêiner Docker.
* **Justificativa:** Evita o custo e a sobrecarga operacional de gerenciar um banco vetorial proprietário apartado (Pinecone/Qdrant), permitindo que tabelas transacionais de tarefas e colunas vetoriais de embeddings residam no mesmo ecossistema com suporte à distância de cosseno.

### 2.4. Governança e Guardrails Éticos
* **Decisão:** Inclusão mandatória de interceptador PII (sanitização de dados sensíveis em memória) e roteamento estrito: desabafos de cunho psicológico são arquivados para pauta de terapia sem conselhos gerados por LLM.

## 3. Consequências e Trade-offs
* **Pontos Positivos:** Baixo custo de infraestrutura (FinOps), latência reduzida, stack auditável para bancas técnicas e aderência aos padrões de mercado para Engenharia de IA.
* **Compromissos:** Exige orquestração de migrações estruturadas via Alembic e esteira rigorosa de evals no GitHub Actions para monitorar alucinações.