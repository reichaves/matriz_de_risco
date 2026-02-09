# Matriz de Risco da Abraji (2019–2024)
### Metodologia de Avaliação de Ameaças a Jornalistas no Brasil

> **📄 [Acesse o Estudo Completo (PDF)](https://drive.google.com/file/d/1Cz10GRhqcWxeHFBBSStqsclyJk_dgfE-/view?usp=sharing)**

Este repositório contém o código-fonte e a documentação técnica para a geração da **Matriz de Risco de Segurança para Jornalistas**, desenvolvida pela **Abraji (Associação Brasileira de Jornalismo Investigativo)**.

O projeto utiliza ciência de dados para consolidar, limpar e analisar ocorrências de ataques físicos, digitais e legais contra a imprensa no Brasil, transformando registros históricos em uma ferramenta preditiva para gestão de risco em redações.

---

## 🎯 Objetivo

Construir uma matriz de risco dinâmica que prioriza ações de segurança combinando **Probabilidade (Likelihood)** e **Impacto (Severity)**. O modelo permite identificar quais tipos de agressão são mais críticos em cada região do país, separando os cenários em dois canais principais: **Físico** e **Digital**.

## 📊 Metodologia

O modelo segue padrões internacionais de gestão de risco (**ISO 31000**) e avaliação de risco (**NIST SP 800-30**), adaptados para o contexto de liberdade de imprensa.

### A Fórmula do Risco
$$Risco = Probabilidade \times Impacto$$
*(Escala final de 1 a 25)*

1.  **Probabilidade (1–5):**
    *   Calculada com base na frequência relativa histórica (2021–2024).
    *   Aplica **Suavização de Laplace** ($\alpha=1$) para evitar distorções em regiões com poucos dados.
    *   Mapeamento em **quintis** (20%, 40%, 60%, 80%, 100%).

2.  **Impacto (1–5):**
    *   Baseado em um *codebook* pré-definido (ex: *Assassinato* = 5, *Ameaça online* = 2).
    *   **Ajustes Automáticos (+1):** O script analisa descrições textuais em busca de termos agravantes (ex: "arma", "milícia", "prisão", "bloqueio judicial").
    *   No digital, considera métricas de indisponibilidade e escala (referência: OWASP Risk Rating).

3.  **Classificação:**
    *   🟢 **Baixo:** 1–6
    *   🟡 **Médio:** 8–12
    *   🟠 **Alto:** 15–19
    *   🔴 **Crítico:** 20–25

---

## 🛠️ Stack Tecnológico

O projeto foi desenvolvido em **Python** e estruturado para rodar em ambientes como Google Colab ou Jupyter Notebook local.

*   **Processamento de Dados:** `pandas`, `numpy`
*   **Limpeza e Normalização:** `unidecode`, `re` (Regex)
*   **Matching de Texto (Fuzzy):** `rapidfuzz`
*   **Visualização:** `matplotlib`
*   **IA (Opcional):** `openai` (para desambiguação semântica de categorias complexas)

---

## 📂 Estrutura do Pipeline

O script `analisa_dados_matriz_risco_abraji.py` executa as seguintes etapas:

1.  **Ingestão:** Carrega planilhas anuais (2019–2024), unificando colunas díspares.
2.  **Higienização:** Remove linhas vazias, placeholders (`NaN`, `N/A`) e normaliza datas.
3.  **Taxonomia:**
    *   **Regiões:** Mapeia Cidades/UFs para macrorregiões (Norte, Nordeste, etc.).
    *   **Atores:** Classifica agressores em *Estatal*, *Não Estatal*, *Crime Organizado*, etc.
    *   **Tipos:** Normaliza descrições livres (ex: "agressão física", "soco") para categorias canônicas.
4.  **Cálculo:** Computa a matriz $P \times I$ para cada par `Região` $\times$ `Tipo de Agressão`.
5.  **Output:** Gera tabelas (Excel/CSV) e Heatmaps (PNG) para os canais Físico e Digital.

---

## 🚀 Como Executar

### Pré-requisitos

Instale as dependências necessárias:

```bash
pip install pandas numpy matplotlib rapidfuzz unidecode openai xlsxwriter
```

### Configuração

1.  Coloque sua base de dados no caminho especificado no script ou ajuste a variável `path`:
    ```python
    path = "caminho/para/sua/base_consolidada.xlsx"
    ```
2.  (Opcional) Para usar a normalização via IA, defina sua chave de API:
    ```python
    os.environ["OPENAI_API_KEY"] = "sua-chave-aqui"
    ```

### Execução

Ao rodar o notebook/script, ele irá gerar:
1.  `matriz_risco_regiao_tipo.xlsx`: Tabela completa com os scores.
2.  `matriz_risco_heatmap_digital.png`: Mapa de calor dos riscos digitais.
3.  `matriz_risco_heatmap_fisico.png`: Mapa de calor dos riscos físicos.

---

## 📈 Principais Resultados (Janela 2021–2024)

Conforme detalhado no [estudo completo](https://drive.google.com/file/d/1Cz10GRhqcWxeHFBBSStqsclyJk_dgfE-/view?usp=sharing):

*   **Agressões e Ataques:** Risco **Alto** transversalmente no país.
*   **Assédio Judicial:** Risco **Alto** no Sudeste (combinação de alta frequência e censura prévia).
*   **Digital:** "Discurso estigmatizante" e "Restrições na internet" variam de Médio a Alto, indicando necessidade urgente de protocolos de segurança digital (2FA, Anti-DDoS).

---

## 📝 Créditos e Autoria

*   **Coordenação e Desenvolvimento:** Reinaldo Chaves (Abraji)
*   **Parceiros:** Voces del Sur, Media Defence
*   **Referências:** ISO 31000:2018, NIST SP 800-30 Rev. 1, OWASP Risk Rating.

---

*Este projeto é open-source sob as diretrizes da Abraji. Contribuições para aprimoramento dos dicionários de dados e pesos de impacto são bem-vindas.*
