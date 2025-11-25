# Análise Orbital de Satélites Ativos

Este repositório reúne um pipeline completo de processamento, classificação e visualização de dados orbitais utilizando a base `active_satellites.csv`. O código foi desenvolvido para organizar análises exploratórias, gerar métricas de risco, identificar anomalias, agrupar regimes orbitais e produzir visualizações destinadas a relatórios técnicos e artigos.

O projeto está dividido em módulos independentes, que podem ser executados separadamente ou como uma sequência integrada.

---

## Estrutura Geral do Pipeline

### Módulo 1 — Preparação do Ambiente
- Montagem do Google Drive (para uso em Google Colab).
- Importação das bibliotecas principais.
- Configurações de visualização.
- Leitura do arquivo CSV e verificação das colunas essenciais.
- Cálculo de altitude quando necessário.

---

### Módulo 2 — Análises Descritivas
Abrange:
- Classificação por país, tipo de missão e constelação.
- Distribuição por altitude, inclinação, excentricidade, arrasto (BSTAR) e faixa orbital.
- Geração de gráficos e tabelas em formato PNG.

---

### Módulo 3 — Mapa Global por Regiões Latitudinais
- Uso de Cartopy para plotagem global.
- Distribuição dos satélites de acordo com faixas de latitude derivadas da inclinação orbital.
- Representação por cores e pontos simulados.

---

### Módulo 4 — Classificação de Satélites Geossíncronos (GEO)
Aplicação dos critérios tradicionais de GEO:
- Mean Motion próximo de 1 rev/dia,
- Inclinação inferior a 5 graus,
- Excentricidade baixa,
- Altitude próxima de 35.786 km.

Geração de gráficos comparativos (vista geral e zoom) e exporta os satélites classificados.

---

### Módulo 5 — Agrupamento Orbital por K-Means
- Número de clusters definido como k = 4.
- Variáveis utilizadas: inclinação, mean motion, excentricidade e RAAN.
- Identificação do regime predominante em cada cluster.
- Geração de percentuais e linha de saída para LaTeX.

---

### Módulo 6 — Classificação Heurística de Risco de Reentrada
Classificação baseada em:
- altitude do perigeu,
- excentricidade,
- BSTAR,
- REV_AT_EPOCH.

Inclui:
- histogramas das categorias de risco,
- gráfico de excentricidade versus altitude em escala logarítmica,
- exportação de tabela consolidada.

---

### Módulo 7 — Identificação de Anomalias com IsolationForest
- Padronização das variáveis de entrada.
- Ajuste automático da taxa de contaminação.
- Suporte a diferentes cenários atmosféricos (aquecimento, tempestade geomagnética e projeções futuras).
- Geração de gráficos de distribuição dos scores e dispersões.
- Exportação das anomalias detectadas em CSV.

---

### Módulo 8 — Pipeline Unificado de Classificação de Risco (Random Forest)
- Construção de rótulos de risco com ruído controlado.
- Treinamento com validação cruzada.
- Métricas apresentadas: acurácia, F1-weighted, matriz de confusão e importância das variáveis.
- Comparação entre três cenários: atual, aquecimento e tempestade geomagnética.

---

### Módulo 9 — Comparações Gráficas Entre Cenários
O módulo produz:
- gráficos de dispersão comparativa,
- histogramas por categoria de risco,
- curvas acumuladas (CDF) de altitude,
- tabelas-resumo.

---

### Módulo 10 — Mapa de Calor Global
- Conversão das categorias de risco em pesos numéricos.
- Simulação de posições longitudinais.
- Geração de mapa interativo em HTML utilizando Folium.

---

## Células A, B e C — Simulação de Perda de Altitude Durante Tempestades

Essas células complementam o pipeline com um modelo físico simplificado para estimar a queda de altitude causada pelo aumento da densidade atmosférica durante tempestades geomagnéticas.

### Célula A — Parametrização e Funções Auxiliares
Responsável por:
- selecionar satélites em órbita LEO (entre 120 e 1200 km),
- garantir que as colunas necessárias estejam presentes,
- configurar o período de simulação (72 horas, passo de 1 hora),
- gerar um perfil sintético do índice Kp,
- definir funções de densidade atmosférica e fatores de decaimento.

Todo o preparo para a integração numérica é realizado nesta etapa.

### Célula B — Integração Numérica da Altitude (Cenário de Tempestade)
Realiza a simulação completa da perda de altitude:
- integração explícita ao longo das 72 horas,
- uso do perfil Kp para incrementar o arrasto,
- cálculo da altitude final e da perda total por satélite.

Produz:
- estatísticas da perda,
- séries temporais médias por banda de altitude,
- histogramas,
- lista dos satélites mais afetados.

### Célula C — Comparação Entre Tempestade e Condições Calmas
Executa uma segunda integração, desta vez considerando Kp constante e baixo:
- comparação direta entre o cenário de tempestade e o cenário calmo,
- cálculo da diferença entre as altitudes finais,
- geração de gráficos comparativos por banda,
- exportação de tabela consolidada.

---

## Dependências Principais

- Python 3.x  
- NumPy  
- Pandas  
- Matplotlib  
- Seaborn  
- Cartopy  
- scikit-learn  
- Folium  

Instalação típica em Colab:
```bash
pip install cartopy folium

