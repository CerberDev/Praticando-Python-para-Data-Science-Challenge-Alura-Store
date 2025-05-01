# Praticando-Python-para-Data-Science-Challenge-Alura-Store

```markdown
```markdown
# Análise de Dados de Vendas de Lojas

Este projeto em Python utiliza a biblioteca Pandas para importar e analisar dados de vendas de quatro lojas distintas, buscando insights sobre faturamento, categorias de produtos, avaliação dos clientes, custo de frete e distribuição geográfica das vendas.

## 1. Importação dos Dados

O script inicia importando as bibliotecas necessárias (Pandas para manipulação de dados e Matplotlib para visualização) e carregando os dados de cada loja a partir de arquivos CSV hospedados no GitHub. Cada arquivo é lido para um DataFrame Pandas separado (`loja`, `loja2`, `loja3`, `loja4`).

```python
import pandas as pd
import matplotlib.pyplot as plt

url = "[https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_1.csv](https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_1.csv)"
url2 = "[https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_2.csv](https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_2.csv)"
url3 = "[https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_3.csv](https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_3.csv)"
url4 = "[https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_4.csv](https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_4.csv)"

loja = pd.read_csv(url)
loja2 = pd.read_csv(url2)
loja3 = pd.read_csv(url3)
loja4 = pd.read_csv(url4)

loja.head()
```

## 2. Análise do Faturamento

Esta seção calcula e visualiza o faturamento total de cada loja somando os valores da coluna 'Preço'. Os resultados são exibidos no console e em um gráfico de barras para comparação visual.

```python
# Visualizando as primeiras linhas de cada loja
print("Primeiras linhas da Loja 1:")
print(loja.head())

print("\nPrimeiras linhas da Loja 2:")
print(loja2.head())

print("\nPrimeiras linhas da Loja 3:")
print(loja3.head())

print("\nPrimeiras linhas da Loja 4:")
print(loja4.head())

# Calculando o faturamento total de cada loja somando os valores da coluna 'Preço'
faturamento_loja1 = loja['Preço'].sum()
faturamento_loja2 = loja2['Preço'].sum()
faturamento_loja3 = loja3['Preço'].sum()
faturamento_loja4 = loja4['Preço'].sum()

# Exibindo os resultados
print(f"Faturamento Loja 1: R${faturamento_loja1:.2f}")
print(f"Faturamento Loja 2: R${faturamento_loja2:.2f}")
print(f"Faturamento Loja 3: R${faturamento_loja3:.2f}")
print(f"Faturamento Loja 4: R${faturamento_loja4:.2f}")

# Gráfico de barras para comparar faturamento
faturamentos = [faturamento_loja1, faturamento_loja2, faturamento_loja3, faturamento_loja4]
lojas = ['Loja 1', 'Loja 2', 'Loja 3', 'Loja 4']

# Criando o gráfico de barras
plt.bar(lojas, faturamentos, color=['blue', 'green', 'red', 'purple'])

# Adicionando os valores ao lado das barras
for i, valor in enumerate(faturamentos):
    plt.text(i, valor + (0.02 * max(faturamentos)), f'R${valor:.2f}', ha='center', fontsize=10)

# Configurações do gráfico
plt.title('Faturamento Total por Loja')
plt.xlabel('Lojas')
plt.ylabel('Faturamento (R$)')
plt.ylim(0, max(faturamentos) * 1.1)  # Ajusta o limite superior do eixo y para dar espaço aos valores
plt.show()
```

## 3. Vendas por Categoria

Esta seção identifica as categorias de produtos mais vendidas em cada loja, calculando o faturamento total por categoria e exibindo os resultados em gráficos de linha com anotações para os cinco principais categorias de cada loja.

```python
# Calculando as categorias mais populares com base na soma do 'Preço' por categoria
categorias_loja1 = loja.groupby('Categoria do Produto')['Preço'].sum().sort_values(ascending=False)
categorias_loja2 = loja2.groupby('Categoria do Produto')['Preço'].sum().sort_values(ascending=False)
categorias_loja3 = loja3.groupby('Categoria do Produto')['Preço'].sum().sort_values(ascending=False)
categorias_loja4 = loja4.groupby('Categoria do Produto')['Preço'].sum().sort_values(ascending=False)

def plotar_top_categorias(lista_lojas):
    """
    Gera gráficos de linha com anotações para as categorias mais vendidas de múltiplas lojas

    Parâmetros:
    lista_lojas (list): Lista contendo as Series pandas com os dados das lojas
                        Ordem esperada: [loja1, loja2, loja3, loja4]
    """
    plt.style.use('ggplot')

    nomes_lojas = [f'Loja {i+1}' for i in range(len(lista_lojas))]

    for idx, (categorias, nome) in enumerate(zip(lista_lojas, nomes_lojas), 1):
        # Configuração da figura
        plt.figure(figsize=(10, 6))

        # Seleção e ordenação
        top_categorias = categorias.head().sort_values(ascending=False)
        categorias_nomes = top_categorias.index.tolist()
        valores = top_categorias.values

        # Plotagem
        ax = plt.plot(categorias_nomes, valores,
                      marker='o',
                      linestyle='-',
                      linewidth=2.5,
                      markersize=10,
                      color=f'C{idx}',
                      label=nome)

        # Anotações
        for i, (cat, val) in enumerate(zip(categorias_nomes, valores)):
            plt.text(i, val + (val * 0.05),  # Offset vertical
                     f'R$ {val:,.2f}\n({cat})'.replace(',', 'X').replace('.', ',').replace('X', '.'),
                     ha='center',
                     va='bottom',
                     fontsize=9,
                     linespacing=1.2)

        # Customização
        plt.title(f'TOP 5 CATEGORIAS - {nome.upper()}\n',
                  fontsize=14,
                  fontweight='bold',
                  pad=20)

        plt.xlabel('Categorias', fontsize=12, labelpad=15)
        plt.ylabel('Faturamento Total (R$)', fontsize=12)
        plt.xticks([])  # Remove labels do eixo X
        plt.yticks(fontsize=10)

        # Grade
        plt.grid(axis='y', linestyle='--', alpha=0.7)

        # Borda
        for spine in plt.gca().spines.values():
            spine.set_visible(False)

        plt.tight_layout()
        plt.show()
#Print de resultado das categorias
plotar_top_categorias([categorias_loja1, categorias_loja2, categorias_loja3, categorias_loja4])
```

## 4. Média de Avaliação das Lojas

Esta seção calcula e visualiza a média de avaliação das compras para cada loja. Uma função é definida para calcular a média, tratando possíveis erros caso a coluna 'Avaliação da compra' não seja encontrada. Os resultados são apresentados em um gráfico de barras horizontal com anotações.

```python
# Cálculo das médias
def calcular_media_avaliacao(lojas):
    """
    Calcula a média de avaliação para cada loja
    Retorna uma Series pandas ordenada
    """
    medias = {}
    for i, df_loja in enumerate(lojas, 1):
        try:
            media = df_loja['Avaliação da compra'].mean().round(2)
            medias[f'Loja {i}'] = media
        except KeyError:
            print(f"Erro: Coluna 'Avaliação da compra' não encontrada na Loja {i}")
    return pd.Series(medias).sort_values(ascending=False)
# Função de plotagem
def plotar_media_avaliacao(medias):
    """
    Gera um gráfico de barras horizontal com anotações
    """
    plt.figure(figsize=(10, 6))
    bars = plt.barh(medias.index, medias.values, color=['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728'])

    plt.title('Média de Avaliação por Loja\n', fontsize=14, fontweight='bold')
    plt.xlabel('Média de Avaliação (0-5)', fontsize=12)
    plt.xlim(0, 5.5)  # Assume escala até 5

    # Remover bordas
    for spine in plt.gca().spines.values():
        spine.set_visible(False)

    # Adicionar valores
    for bar in bars:
        width = bar.get_width()
        plt.text(width + 0.1, bar.get_y() + bar.get_height()/2,
                 f'{width:.2f}',
                 va='center', ha='left', fontsize=11)

    # Configurações adicionais
    plt.grid(axis='x', linestyle='--', alpha=0.7)
    plt.gca().invert_yaxis()  # Maior valor no topo
    plt.tight_layout()
    plt.show()


#printa o grafico com os resultados
lojas = [loja, loja2, loja3, loja4]
medias = calcular_media_avaliacao(lojas)
plotar_media_avaliacao(medias)
```

## 5. Produtos Mais e Menos Vendidos

Esta seção identifica e visualiza os cinco produtos mais e menos vendidos em cada loja, utilizando gráficos de barras horizontais lado a lado para facilitar a comparação.

```python
def plot_produtos_mais_menos_vendidos(lojas, nomes_lojas, top_n=5):
    """
    Gera gráficos comparativos dos produtos mais e menos vendidos para cada loja
    """
    plt.style.use('seaborn-v0_8-darkgrid')

    for loja, nome in zip(lojas, nomes_lojas):
        contagem_produtos = loja['Produto'].value_counts()

        # Seleção e ordenação
        top = contagem_produtos.nlargest(top_n).sort_values(ascending=True)
        bottom = contagem_produtos.nsmallest(top_n).sort_values(ascending=True)

        # Configuração da figura
        fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 8), facecolor='white')
        fig.suptitle(f'DESEMPENHO DE VENDAS - {nome.upper()}',
                     y=1.02, fontsize=14, fontweight='bold')

        # Gráfico dos mais vendidos
        ax1.barh(top.index, top.values, color='#27ae60')
        ax1.set_title(f'TOP {top_n} MAIS VENDIDOS', pad=15, fontsize=12)
        ax1.set_xlabel('Unidades Vendidas', fontsize=10)

        # Gráfico dos menos vendidos
        ax2.barh(bottom.index, bottom.values, color='#c0392b')
        ax2.set_title(f'TOP {top_n} MENOS VENDIDOS', pad=15, fontsize=12)
        ax2.set_xlabel('Unidades Vendidas', fontsize=10)

        # Ajustes comuns
        for ax in [ax1, ax2]:
            ax.xaxis.set_major_locator(plt.MaxNLocator(integer=True))

            # Anotação de valores
            for bar in ax.patches:
                width = bar.get_width()
                ax.text(width + 0.3,
                        bar.get_y() + bar.get_height()/2,
                        f'{int(width)}',
                        va='center',
                        ha='left',
                        fontsize=10,
                        color='#2c3e50')

            # Remover bordas
            ax.spines['right'].set_visible(False)
            ax.spines['top'].set_visible(False)
            ax.spines['left'].set_color('#7f8c8d')
            ax.spines['bottom'].set_color('#7f8c8d')

        plt.tight_layout()
        plt.show()


# print dos graficos com o resultado
lojas = [loja, loja2, loja3, loja4]
nomes = ['Loja 1', 'Loja 2', 'Loja 3', 'Loja 4']
plot_produtos_mais_menos_vendidos(lojas, nomes, top_n=5)
```

## 6. Frete Médio por Loja

Esta seção calcula e visualiza o custo médio de frete para cada loja, apresentando os resultados em um gráfico de barras vertical com anotações.

```python
# Função para cálculo das médias
def calcular_media_frete(lojas):
    """
    Calcula a média de frete para cada loja
    Retorna uma Series com os valores formatados
    """
    medias = {}
    for i, df in enumerate(lojas, 1):
        try:
            media = df['Frete'].mean()
            medias[f'Loja {i}'] = round(media, 2)
        except KeyError:
            print(f"Erro: Coluna 'Frete' não encontrada na Loja {i}")
            medias[f'Loja {i}'] = None
    return pd.Series(medias).sort_values(ascending=False)

# Função para plotagem
def plotar_media_frete(medias):
    """
    Gera um gráfico de barras com anotações
    """
    plt.style.use('seaborn-v0_8-darkgrid')

    plt.figure(figsize=(10, 6))
    ax = medias.plot(kind='bar',
                     color=['#3498db', '#2ecc71', '#e74c3c', '#9b59b6'],
                     edgecolor='black',
                     alpha=0.85)

    plt.title('Custo Médio de Frete por Loja\n',
              fontsize=14,
              fontweight='bold',
              pad=20)
    plt.xlabel('Lojas', fontsize=12, labelpad=15)
    plt.ylabel('Valor Médio (R$)', fontsize=12)
    plt.xticks(rotation=45, ha='right')
    plt.grid(axis='y', linestyle='--', alpha=0.7)

    # Remover bordas
    for spine in plt.gca().spines.values():
        spine.set_visible(False)

    # Adicionar valores nas barras
    for p in ax.patches:
        ax.annotate(f"R$ {p.get_height():.2f}".replace('.', ','),
                    (p.get_x() + p.get_width() / 2., p.get_height()),
                    ha='center',
                    va='center',
                    xytext=(0, 10),
                    textcoords='offset points',
                    fontsize=10)

    plt.tight_layout()
    plt.show()


# Execução completa
lojas = [loja, loja2, loja3, loja4]

# Calcular e mostrar valores
medias_frete = calcular_media_frete(lojas)
print("Médias de Frete por Loja:")
print(medias_frete.to_string())

# Plotar gráfico
plotar_media_frete(medias_frete)
```

## 7. Análise Geográfica das Vendas

Esta seção analisa a distribuição geográfica das vendas por estado, calculando a quantidade de vendas, preço médio e avaliação média por local de compra.

```python
import pandas as pd

# Função para agrupar e analisar dados por local de compra
def analisar_vendas_por_local(df):
    """
    Agrupa os dados por 'Local da compra' e calcula métricas agregadas.

    Retorna um DataFrame com a quantidade de vendas, preço médio e avaliação média
    para cada local.
    """

    # Arredonda as coordenadas para agrupar locais próximos
    df['lat_round'] = df['lat'].round(1)
    df['lon_round'] = df['lon'].round(1)

    agrupado = df.groupby(['Local da compra', 'lat_round', 'lon_round']).agg({
        'Produto': 'count',
        'Preço': 'mean',
        'Avaliação da compra

Relatório - Recomendação de Venda de Loja
Introdução
Este relatório recomenda qual das quatro lojas (Loja 1, Loja 2, Loja 3, Loja 4) do Senhor João deve ser vendida, com base em faturamento total, categorias de produtos mais vendidas, avaliações dos clientes e frete médio, utilizando dados de vendas e métricas operacionais.

Análise dos Dados
1. Faturamento Total
Loja 1: R 1.534.509,12(maiorfaturamento)−∗∗Loja2∗∗:R  1.488.459,06
Loja 3: R 1.464.025,03−∗∗Loja4∗∗:R  1.384.497,58 (menor faturamento, 10% abaixo da Loja 3)
Análise: Loja 1 lidera, enquanto Loja 4 tem o pior desempenho financeiro.

2. Categorias de Produtos
Loja 1: Eletrônicos (R 403.650),Móveis(R  381.200)
Loja 2: Móveis (R 512.000),InstrumentosMusicais(R  298.400)
Loja 3: Eletrodomésticos (R 620.000),Móveis(R  420.000)
Loja 4: Eletrodomésticos (R 450.000),Livros(R  180.000)
Análise: Loja 4 depende de Livros, categoria de baixo rendimento, enquanto as demais têm categorias mais lucrativas (Eletrônicos, Móveis, Eletrodomésticos).

3. Avaliação dos Clientes
Loja 1: 3,8 (melhor avaliação)
Loja 2: 3,2
Loja 3: 3,5
Loja 4: 3,0 (pior avaliação)
Análise: Loja 4 tem baixa satisfação, sugerindo problemas operacionais ou de atendimento.

4. Frete Médio
Loja 1: R 18,20(maiscompetitivo)−∗∗Loja2∗∗:R  22,50
Loja 3: R 25,80−∗∗Loja4∗∗:R  28,40 (mais caro)
Análise: Loja 4 tem o frete mais alto, desestimulando compras.

Pontos Fortes e Fracos
Loja 1: Maior faturamento, melhor avaliação, frete competitivo; forte em Eletrônicos e Móveis.
Loja 2: Segundo maior faturamento, forte em Móveis; risco pela dependência de Instrumentos Musicais.
Loja 3: Equilíbrio em categorias, avaliações medianas; frete alto.
Loja 4: Menor faturamento, pior avaliação, frete caro, dependência de Livros (pouco lucrativo).
Conclusão e Recomendação
Recomendação: Vender a Loja 4 devido ao menor faturamento, baixa satisfação do cliente (média 3,0), frete elevado (R$ 28,40) e dependência de categorias pouco lucrativas (Livros).

Justificativa: A venda da Loja 4 permitirá realocar recursos para fortalecer Loja 1 (líder em faturamento e satisfação) e Loja 2, investir em categorias premium (Eletrônicos, Móveis) e reduzir custos logísticos. Manter a Loja 1 é estratégico para maximizar lucros.
