# AdMestrado – Classificador Inteligente de Candidaturas ao Mestrado

**AdMestrado** é uma ferramenta desenvolvida para automatizar e padronizar a análise de currículos acadêmicos em processos seletivos de mestrado. A aplicação lê o PDF enviado pelo candidato, identifica padrões relevantes e utiliza um modelo de classificação para indicar o potencial de aprovação, com base em critérios objetivos como **produção científica**, **projeto de pesquisa** e **domínio de inglês**.

Este projeto foi desenvolvido por **Crislene Nunes** durante o **Bootcamp de LLM da SoulCode Academy**, com foco em aplicações práticas de IA e interfaces com Gradio, LangChain e Hugging Face Spaces.

---

## Demonstração online

Você pode testar a aplicação diretamente no Hugging Face Spaces:

> https://huggingface.co/spaces/Crislene/AdMestrado

---

## Metodologia

A lógica da aplicação parte da leitura automática do currículo em PDF, seguida pela identificação de padrões-chave e aplicação de um modelo preditivo para estimar a aprovação do candidato.

### Critérios analisados:
- **Publicações científicas** (termos como *artigo*, *publicação*, *evento*)
- **Projeto de pesquisa** (presença da palavra *projeto*)
- **Proficiência em inglês** (mencionando *inglês*, *TOEFL*, *IELTS*)

Esses critérios são transformados em variáveis binárias ou contagem, que alimentam um modelo **Random Forest** treinado com dados simulados. O objetivo é testar a viabilidade de automação no primeiro filtro da triagem de candidatos.

---

## Fluxograma

```mermaid
graph TD
    A[Usuário envia currículo (PDF)] --> B[Extração de texto com pdfminer]
    B --> C[Regex: Publicações, Projeto, Inglês]
    C --> D[Criação de variáveis]
    D --> E[Modelo Random Forest]
    E --> F{Probabilidade > 70%?}
    F -- Sim --> G[Aprovado]
    F -- Não --> H[Reprovado]
    E --> I[Visualização de métricas]
```

---

## O modelo

O modelo usado foi um **RandomForestClassifier** treinado com dados sintéticos simulando perfis reais. Os dados foram balanceados e a acurácia esperada é acima de 85% para esta tarefa de triagem binária.

### Variáveis usadas:
- `publicacoes`: número de menções a artigos/publicações
- `ingles`: presença de termos relacionados a proficiência
- `projeto`: existência de projeto citado no currículo

Essas variáveis compõem um vetor de entrada que determina a probabilidade de aprovação.

---

## Interface da aplicação

A interface Gradio oferece:

- Upload do currículo em PDF
- Botão de análise com retorno instantâneo
- Saída com:
  - Classificação (Aprovado ou Reprovado)
  - Probabilidade (%) de aprovação
  - Gráfico com impacto de cada critério

---

## mkdemo: integração com fluxos de IA

Este Space pode ser usado em fluxos LangChain com o padrão abaixo:

```python
from langchain.tools import tool

@tool
def analisar_candidato(file_path: str) -> dict:
    """Análise automática de um currículo acadêmico para mestrado."""
    # Integração via requests com o Space AdMestrado
```

---

## Estrutura do projeto

```
AdMestrado/
├── app.py                     # Código principal com interface Gradio
├── modelo_mestrado.joblib     # Modelo Random Forest
├── vectorizer.joblib          # Vetorizador (placeholder, se necessário)
├── requirements.txt           # Dependências do projeto
```

---

## Requisitos (requirements.txt)

```txt
gradio==4.28.3
scikit-learn==1.3.0
pdfminer.six==20221105
pandas==2.0.3
joblib==1.3.2
matplotlib==3.7.1
seaborn==0.12.2
numpy==1.24.3
```

---

## Licença

Este projeto está licenciado sob a [MIT License](https://opensource.org/licenses/MIT), permitindo uso, modificação e distribuição livre com atribuição.

---

## Autoria

Desenvolvido por **Crislene Nunes**, participante do **Bootcamp LLM da SoulCode Academy**, como parte de sua trilha prática de projetos com IA e democratização de tecnologias de análise de dados.

---

## Tecnologias utilizadas

`Python` | `Gradio` | `scikit-learn` | `pdfminer.six` | `pandas` | `joblib` | `matplotlib` | `seaborn`

