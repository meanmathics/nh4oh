# NH₄OH: Análise de Dados de Laboratório em Python

Este projeto é uma análise exploratória de dados (EDA) físico-químicos da relação entre **Temperatura** e **Densidade** para determinação da **Concentração** de soluções de Hidróxido de Amônio (NH₄OH).

O objetivo é entender a confiabilidade das tabelas de referência estáticas em excel e aproximações intuitivas em condições diferentes com um modelo de dados visual e preciso, que sirva de base para futuras análises computacionais.

## Metodologia

Foram realizadas 32 análises em duplicata (ou triplicata) a partir de amostras-padrão e diluições das mesmas em laboratório. Os dados foram coletados em 4 temperaturas fixas (10, 15.6, 21.1 e 26.7 °C) para 8 níveis de densidade diferentes.

Os dados brutos estão disponíveis no arquivo `data/dados_amostras.csv`.

## Análise Visual e Conclusões

O script `notebook.ipynb` carrega os 32 pontos de dados e gera o seguinte gráfico de dispersão, que serve como nossa "prova de conceito" visual:

![Gráfico de Dispersão de Dados da Amônia](https://raw.githubusercontent.com/meanmathics/nh4oh/refs/heads/main/amonia_scatter_plot_v1.png)

A partir deste gráfico, podemos deduzir que: **A Relação é Não-Linear:** As 4 "curvas" de temperatura não são perfeitamente paralelas. A distância entre elas (o "gap" de concentração) muda conforme a densidade (e, portanto, a concentração) muda.

## Próximos Passos (v2.0)

Esta análise v1.0 prova a necessidade de uma abordagem mais sofisticada. Os próximos passos para este projeto são:

* **v2.0 (Simulação):** Investigar a "causa" da instabilidade em altas temperaturas, modelando a Pressão de Vapor (Equilíbrio Líquido-Vapor) e, eventualmente, utilizando simulações de Dinâmica Molecular (MD) para entender as interações em nível atômico e comparar com os dados atuais.

## Apêndice: Script para Reprodução no Jupyter_Notebook

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings

warnings.filterwarnings('ignore')

plt.style.use('seaborn-v0_8-whitegrid')
sns.set_palette("husl") 

print("Carregando dados de 'data/dados_amostras.csv'...")
df_amonia = pd.read_csv('data/dados_amostras.csv')

print(f"Dados carregados com sucesso. {len(df_amonia)} pontos de dados.")
print("-" * 30)
print(df_amonia.head())

print("Gerando o gráfico de dispersão...")

plt.figure(figsize=(10, 6))

for temp in sorted(df_amonia['temperatura'].unique()):
    dados_temp = df_amonia[df_amonia['temperatura'] == temp]
    plt.scatter(dados_temp['densidade'], dados_temp['concentracao'],
                   label=f'{temp}°C', alpha=0.8, s=70)

plt.xlabel('Densidade (g/mL)', fontsize=12)
plt.ylabel('Concentração (%)', fontsize=12)
plt.title('Dados Experimentais: Concentração vs. Densidade por Temperatura', fontsize=15)
plt.legend(title='Temperatura')
plt.grid(True, linestyle='--', alpha=0.5)

plt.gca().invert_xaxis() 

plt.tight_layout()
plt.savefig('amonia_scatter_plot_v1.png')
plt.show()

# --- FIM DO SCRIPT ---
