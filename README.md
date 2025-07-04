# Arquitetura de IA com Múltiplos Agentes usando n8n

Este projeto implementa uma arquitetura avançada de IA utilizando o n8n, com múltiplos modelos (Ollama/Mistral e Google Gemini) aplicados à análise de relatos médicos e auxílio no diagnóstico de doenças, como dengue.

---

## Objetivo

Criar um sistema automatizado que:

1. Recebe um relato clínico do médico.
2. Identifica a doença mencionada (ex: "dengue").
3. Consulta um banco de dados com casos reais dessa doença.
4. Analisa os sintomas com **duas IAs diferentes**.
5. Usa uma **IA decisora** para comparar as respostas e gerar um diagnóstico final com justificativa e probabilidade estimada.

---

## Dataset

- **Nome:** Symptom2Disease  
- **Fonte:** [Kaggle - symptom2disease](https://www.kaggle.com/datasets/niyarrbarman/symptom2disease)  
- **Uso:** Armazenado em banco **MySQL** na tabela `diagnosticos`, com colunas `label` e `text`.

---

## Arquitetura do Fluxo (n8n)

```plaintext
1. Trigger:
   Recebe o input do médico com relato clínico.

2. Extração da doença (LLM via prompt):
   Gera a query SQL: SELECT * FROM diagnosticos WHERE label LIKE '%Dengue%' LIMIT 5;

3. Consulta MySQL:
   Busca os casos reais confirmados da doença extraída.

4. IA 1 - Ollama (Mistral):
   Analisa sintomas do relato com base nos dados reais.

5. IA 2 - Google Gemini:
   Faz a mesma análise de forma paralela.

6. IA Decisora:
   Compara as duas respostas, escolhe a melhor e gera o parecer clínico final com probabilidade (%).
```
## Ferramentas Utilizadas

- **n8n:** Orquestração dos fluxos automatizados  
- **Google Drive API:** Importação do dataset  
- **MySQL:** Armazenamento dos casos confirmados  
- **Ollama + Mistral:** Modelo LLM local para análise de sintomas  
- **Gemini 1.5 Flash:** Modelo generativo via Google Cloud  
- **LangChain:** Para controle de fluxo e prompts entre modelos  

---

## Resultados

- Respostas com alta clareza e justificativa clínica  
- Probabilidade calculada com base sintomática real  
- Processo 100% automatizado e reprodutível  
- Integração de agentes paralelos + decisão final  

---

## Validação e Métricas

- **Clareza da resposta:** Alta  
- **Coerência com base de dados:** Boa  
- **Evidência baseada em sintomas reais:** Sim  
- **Limitações:** Dependência da qualidade do relato e do dataset  

---

## Melhorias Implementadas

- Prompt do decisor neutro (sem revelar qual IA foi usada)  
- Formato padrão da resposta final: parágrafo + probabilidade  
- Avaliada alternativa com banco vetorial (descartada por simplicidade)  

---

## Dificuldades e Aprendizados

### Dificuldades

- Orquestração entre múltiplas IAs no n8n  
- Controle preciso do output dos modelos  

### Aprendizados

- A colaboração entre IAs com arquitetura distinta gera diagnósticos mais robustos  
- Prompts bem estruturados elevam a confiabilidade das decisões automatizadas  
