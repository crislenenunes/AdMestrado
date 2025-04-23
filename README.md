# AdMestrado - Plataforma de Análise de Editais para Mestrado em Ciências de Dados

🔗 **Acesse o projeto:** [Hugging Face Space](https://huggingface.co/spaces/Crislene/AdMestrado)

## Visão Geral
O **AdMestrado** é uma plataforma inteligente para análise automatizada de editais de programas de mestrado em Ciências de Dados. Utilizando técnicas de Processamento de Linguagem Natural (PNL) e aprendizado de máquina, a plataforma extrai requisitos de editais em PDF e classifica candidatos com base em critérios como experiência, publicações, inglês e conhecimentos específicos.

## Principais Funcionalidades:
- **Extração de requisitos de editais:** Identificação de requisitos como proficiência em inglês, publicações acadêmicas, experiência em programação, entre outros.
- **Análise de currículos:** Pontuação automática com base em experiência acadêmica, conhecimento em programação, inglês e outros critérios.
- **Geração de dados simulados:** Criação de um dataset fictício para treinar e validar o modelo.
- **Recomendações de candidatos:** Classificação de candidatos com base em pontuação final e sugestões de melhorias.

## Modelo de Classificação Utilizado
O modelo de classificação do AdMestrado é baseado no **Random Forest Classifier** do scikit-learn, configurado para otimizar a acurácia e reduzir o overfitting, utilizando os seguintes parâmetros:

```python
RandomForestClassifier(n_estimators=100, random_state=42)
```

### Variáveis Utilizadas no Treinamento
As variáveis de entrada (features) utilizadas para classificar candidatos são:
- **publicacoes_A1:** Número de publicações acadêmicas do candidato.
- **publicacoes_B:** Publicações em conferências ou periódicos de menor impacto.
- **orientacoes:** Quantidade de orientações acadêmicas realizadas.
- **experiencia_docente:** Anos de experiência no ensino superior.
- **nota_entrevista:** Nota atribuída ao candidato em uma possível entrevista de seleção.

A variável alvo é **target**, que indica se o candidato é adequado ao mestrado (1) ou não (0).

## Ajustes Específicos
- O modelo foi treinado com balanceamento de classes para lidar com a distribuição desigual entre candidatos aprovados e reprovados.
- Foi aplicada uma técnica de **extração de palavras-chave** dos currículos em PDF, que influencia a pontuação final dos candidatos, levando em conta as soft skills descritas nos documentos.

## Avaliação do Modelo
O modelo foi avaliado utilizando as métricas padrão de classificação:
- **Matriz de Confusão**
- **Relatório de Classificação (precision, recall, f1-score)**
- **Gráfico de Importância das Características**

A seguir, um exemplo de avaliação do modelo:
```text
precision    recall  f1-score   support
           0       0.85      0.89      0.87        28
           1       0.86      0.82      0.84        22

accuracy                           0.86        50
macro avg       0.86      0.85      0.85        50
weighted avg       0.86      0.86      0.86        50
```

## Fluxograma da Lógica de Aprovação
A lógica de aprovação do candidato é baseada nos requisitos extraídos dos editais, seguido de uma classificação automatizada, utilizando o modelo de Random Forest e a pontuação gerada.

## Arquitetura Técnica
| Componente           | Tecnologia         | Descrição                                                       |
|----------------------|--------------------|---------------------------------------------------------------|
| Backend de Análise   | Python 3.10        | Lógica principal de classificação                              |
| Modelo de ML         | Scikit-learn       | Random Forest Classifier                                       |
| Interface Web        | Gradio             | Interface interativa no navegador                              |
| PNL                  | Regex + análise léxica | Extração de palavras-chave de currículos                     |
| Implantação          | Hugging Face Spaces | Publicação automática e acessível online                      |

## Como Executar

### Pré-requisitos
- Python 3.10+
- Bibliotecas: `pandas`, `scikit-learn`, `gradio`, `matplotlib`, `seaborn`, `plotly`, `pdfminer.six`, entre outras.

### Instalação
1. Clone o repositório:
```bash
git clone https://github.com/seu_usuario/AdMestrado.git
cd AdMestrado
```

2. Instale as dependências:
```bash
pip install -r requirements.txt
```

### Execução Local
Execute a aplicação:
```bash
python app.py
```

## Estrutura de Arquivos
```
AdMestrado/
├── app.py                        # Código principal da aplicação
├── requisitos.txt                # Dependências do projeto
├── modelo_treinado.joblib        # Modelo de ML treinado
├── editais/                      # Diretório para armazenar os editais baixados
├── perfis/                       # Diretório para armazenar os perfis dos candidatos
└── README.md                     # Este arquivo
```

## Destaques Técnicos
1. **Análise de Currículo com PNL**
   - Utiliza expressões regulares para encontrar palavras-chave em currículos, atribuindo pontuação ao candidato.
   
   ```python
   def analisar_curriculo(texto):
       score = 0
       for palavra in palavras_chave:
           if palavra in texto.lower():
               score += 1
       return min(score, 10)
   ```

2. **Sistema de Pontuação Ponderada**
   - Combina pontuações de experiência, conhecimento em CRM, proficiência em inglês e análise do currículo.

   ```python
   pontuacao = (
       experiencia * 0.30 +
       crm * 0.25 +
       ingles * 0.20 +
       curriculo_score * 0.25
   )
   ```

3. **Exemplo de Cálculo de Pontuação Final**
   - Um exemplo de como calcular a pontuação final de um candidato:

   ```python
   candidato = {
       "experiencia": 5,
       "crm": 4,
       "ingles": 3,
       "curriculo_score": 6.2
   }

   pontuacao_final = (5*0.30) + (4*0.25) + (3*0.20) + (6.2*0.25)
   # Resultado: 73.5 (Potencial)
   ```

## Metodologia

### Geração de Dados
Foram gerados 200 candidatos simulados com diferentes perfis, utilizando variáveis como publicações, experiência, e notas de entrevista.

### Engenharia de Funcionalidades
As funcionalidades foram extraídas e transformadas em features numéricas, como o tempo de experiência e a proficiência em inglês.

### Treinamento
O modelo de Random Forest foi treinado e validado com um conjunto de 50 candidatos, utilizando técnicas de balanceamento de classes.

## Critérios de Aprovação

| Status    | Pontuação | Requisitos                                     |
|-----------|-----------|------------------------------------------------|
| Aprovado  | ≥ 75      | Cumprir todos os critérios mínimos            |
| Potencial | 50–74     | Perfil promissor, com necessidade de ajustes  |
| Reprovado | < 50      | Não atenda aos requisitos essenciais          |

## Como Contribuir
1. Faça um fork do projeto.
2. Crie uma nova branch: `git checkout -b feature/nova-feature`.
3. Commit suas alterações: `git commit -m 'Nova feature'`.
4. Envie para o repositório: `git push origin feature/nova-feature`.
5. Crie um Pull Request.

## Licença
Distribuído sob licença MIT. Veja o arquivo LICENSE para mais informações.

---

<div align="center">  
  <p>Desenvolvido com ❤️ por <a href="https://github.com/crislenenunes">Crislene Nunes</a> duarnte o Bootcamp de IA LLM da SoulCode</p>  
  <img src="https://img.shields.io/badge/Python-3.10+-blue?logo=python" alt="Python">  
  <img src="https://img.shields.io/badge/scikit--learn-1.3+-orange?logo=scikit-learn" alt="Scikit-learn">  
  <img src="https://img.shields.io/badge/Gradio-4.28.3-green?logo=gradio" alt="Gradio">  
</div>

---
