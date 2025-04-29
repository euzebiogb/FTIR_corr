# Analisador Interativo de Espectro Infravermelho (IR)

Acesse: https://euzebiogb.github.io/FTIR_corr/

Temos espectros exemplo no repositório.

## Descrição

Esta é uma aplicação web interativa criada com HTML, CSS (Tailwind) e JavaScript puro para analisar e comparar espectros infravermelhos (IR) de misturas químicas. A ferramenta permite aos usuários fazer upload de espectros de substâncias puras e de uma mistura real, processá-los, calcular um espectro teórico baseado em uma razão de mistura (manual ou otimizada) e visualizar a comparação. Além disso, permite analisar a correlação entre os espectros real e teórico em regiões específicas definidas pelo usuário.

## Funcionalidades Principais

* **Upload de Arquivos:** Carregue espectros IR em formato CSV para duas substâncias puras e uma mistura real.
* **Pré-processamento:**
    * Detecção automática de unidades (Absorbância vs. Transmitância) e conversão para Absorbância.
    * Correção opcional de linha de base (método simples de subtração do mínimo na Absorbância).
    * Conversão de volta para Transmitância (%) após correção.
    * Normalização opcional (0-1) da Transmitância final.
* **Alinhamento de Espectros:** Alinha os três espectros processados a uma grade comum de número de onda usando interpolação linear na faixa de sobreposição.
* **Cálculo da Mistura:**
    * **Modo Manual:** Defina a proporção da mistura usando um controle deslizante.
    * **Modo Automático:** Otimiza a proporção da mistura para minimizar o Erro Quadrático Médio (MSE) entre o espectro real e o teórico.
* **Análise de Correlação por Região:**
    * Defina uma ou mais regiões espectrais (faixas de número de onda).
    * Calcula e exibe o coeficiente de correlação de Pearson (R) entre o espectro real processado e o espectro teórico calculado dentro de cada região definida.
* **Visualização Interativa:**
    * Gráfico Plotly.js comparando o espectro da mistura real processada com o espectro teórico calculado.
    * Eixo X ("Número de Onda cm⁻¹") invertido.
    * Opção para exibir o eixo Y como Transmitância Corrigida (%), Transmitância Normalizada (0-1) ou Absorbância Corrigida.
    * Exibição visual das regiões de correlação e seus respectivos valores R no gráfico.
* **Interface Moderna:** Estilizada com Tailwind CSS para um visual limpo e tema escuro.
* **Exportação:**
    * Exporte os dados do espectro teórico calculado (número de onda, valor na unidade selecionada) como um arquivo CSV.
    * Exporte o gráfico atual como uma imagem PNG.

## Tecnologias Utilizadas

* **HTML5**
* **CSS3** (com [Tailwind CSS](https://tailwindcss.com/))
* **JavaScript (ES6+)** (Vanilla JS, sem frameworks)
* **[Plotly.js](https://plotly.com/javascript/)** (para gráficos interativos)

## Como Usar

1.  **Baixe o Arquivo:** Faça o download do arquivo `index.html`.
2.  **Abra no Navegador:** Abra o arquivo `index.html` diretamente em um navegador web moderno (Chrome, Firefox, Edge, etc.). Não é necessário um servidor web.
3.  **Carregue os Arquivos:** Use os botões na barra lateral para carregar os três arquivos CSV necessários.
4.  **Configure as Opções:** Selecione as opções de pré-processamento, modo de cálculo e regiões de correlação desejadas.
5.  **Analise:** Clique no botão "Analisar Espectros".
6.  **Explore e Exporte:** Interaja com o gráfico e use os botões de exportação conforme necessário.

## Formato do CSV de Entrada

A aplicação espera arquivos CSV onde:

* A **primeira coluna** contém os valores de **Número de Onda (cm⁻¹)**.
* A **segunda coluna** contém os valores espectrais (**Absorbância** ou **Transmitância %**).
* O separador pode ser vírgula (`,`), ponto e vírgula (`;`) ou tabulação (`\t`).
* A aplicação tenta detectar e pular automaticamente uma linha de cabeçalho se a segunda coluna da primeira linha não for um número válido.

**Exemplo:**


    NumeroDeOnda,Absorbancia
    4000,0.1
    3998,0.11
    3996,0.105
    ...
ou

Snippet de código

    Wavenumber;Transmittance
    4000;95.5
    3998;94.2
    3996;94.8
    ...
Detalhes da Funcionalidade
Pré-processamento
Conversão de Unidade: A aplicação verifica se os valores médios na segunda coluna são significativamente maiores que ~3 e se todos são positivos para inferir se os dados são Transmitância (%). Se sim, converte para Absorbância ($A = 2 - \log_{10}(T%)$). Caso contrário, assume que já são Absorbância.
Correção de Linha Base: Se habilitada, subtrai o valor mínimo de Absorbância de todo o espectro de Absorbância. Garante que os valores resultantes não sejam negativos.
Normalização: Se habilitada, escala os valores de Transmitância (%) após a correção de linha base (se aplicada) e conversão de volta para T%, para ficarem na faixa de 0 a 1, baseado no mínimo e máximo de cada espectro individualmente ($T_{norm} = (T - min(T)) / (max(T) - min(T))$).
Alinhamento
Os espectros são alinhados usando os pontos de número de onda do espectro da Mistura que se encontram dentro da faixa de sobreposição de todos os três espectros. Os valores para os espectros Puros nesses pontos são calculados por interpolação linear.

Cálculo da Mistura
Teórico: $Espectro_{Teórico} = (Razão \times Espectro_{Puro1_Alinhado}) + ((1 - Razão) \times Espectro_{Puro2_Alinhado})$ (usando os dados finais de Transmitância processada/normalizada).
Otimização: Encontra a Razão (entre 0 e 1) que minimiza o Erro Quadrático Médio (MSE) entre $Espectro_{Teórico}$ e $Espectro_{Mistura_Real_Alinhada}$.
Análise de Correlação
Calcula o coeficiente de correlação de Pearson entre os dados Y do espectro real processado e do espectro teórico calculado, considerando apenas os pontos de dados que caem dentro da faixa de número de onda especificada para cada região e que são válidos (não-NaN) em ambos os espectros.

Exportação
CSV: Exporta os dados do espectro teórico (commonWavenumbers e theoreticalSpectrum.values convertidos para a unidade de exibição atual).
PNG: Exporta a visualização atual do gráfico Plotly.
Screenshot
(Opcional: Insira um screenshot da aplicação aqui)

Contribuições
Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests. 
