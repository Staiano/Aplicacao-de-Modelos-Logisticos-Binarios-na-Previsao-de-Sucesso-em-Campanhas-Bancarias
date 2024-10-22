# Aplicação de Modelos Logísticos Binários na Previsão de Sucesso em Campanhas Bancárias

12 de setembro de 2024

**Mais uma etapa nos meus estudos em ciência de dados! Desta vez, exploramos o mundo dos modelos logísticos binários com um estudo de classificação de termos de depósitos de um banco. Nosso objetivo é prever se um cliente vai ou não contratar o termo de depósito.**

No artigo, apresentarei os passos que segui para a criação do modelo, os aprendizados obtidos e a análise final.

Gostaria de expressar minha gratidão ao professor  [Luiz Paulo Fávero](https://www.linkedin.com/in/luiz-paulo-f%C3%A1vero-83a117118/)  pelos ensinamentos que foram fundamentais para a conclusão deste estudo. É importante ressaltar que quaisquer erros presentes neste artigo são exclusivamente meus e decorrentes do meu processo de aprendizagem, sem influência do professor. Todas as influências que ele teve meu artigo possui são positivas.

### Banco de Dados

Nosso banco de dados foi obtido do UCI Machine Learning Repository, no projeto “[Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing)”.

![](https://media.licdn.com/dms/image/v2/D4D12AQGbU3xmvGDkgw/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1726145412558?e=1735171200&v=beta&t=tAlEmg_Unyg5tD_wWa4h_drgpgyjvSwkvqJTKCICml4)

O banco contém uma variável dependente dicotômica e mais 16 variáveis explicativas, das quais 7 são quantitativas e 9 são qualitativas.

  

### Variáveis

**Variável Y:**

-   **“term_deposit”**: Indica se o cliente vai ou não assinar o termo de depósito (1 para sim e 0 para não).

**Variáveis X:**

-   **“job”**: Ramo de trabalho do cliente.
-   **“marital”**: Estado civil do cliente.
-   **“education”**: Nível educacional do cliente.
-   **“default”**: Indica se o cliente está inadimplente ou não.
-   **“balance”**: Saldo médio anual do cliente.
-   **“housing”**: Indica se o cliente financia alguma propriedade.
-   **“loan”**: Indica se o cliente possui algum empréstimo vigente.
-   **“contact”**: Forma de comunicação com o cliente.
-   **“day”**: Último dia de contato com o cliente.
-   **“month”**: Último mês de contato com o cliente.
-   **“duration”**: Duração do último contato com o cliente.
-   **“campaign”**: Número de contatos realizados durante a campanha.
-   **“pdays”**: Quantidade de dias desde o último contato com o cliente.
-   **“previous”**: Número de contatos feitos antes da campanha.
-   **“poutcome”**: Resultado da campanha anterior.

  

### Primeiros Passos

Minha primeira preocupação foi analisar a distribuição dos dados quantitativos. Juntamente com a descrição (.describe()), realizei um boxplot das variáveis quantitativas.

**_Observação:_**  Desconsidere a variável “term_deposit” no boxplot, pois, apesar de ser numérica, seu uso é categórico.

No boxplot, observei que a variável “balance” estava muito desequilibrada, com muitos outliers e valores elevados. Decidi então remover “balance” do modelo.

  

![](https://media.licdn.com/dms/image/v2/D4D12AQHMfXOUKxi10A/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1726145544643?e=1735171200&v=beta&t=aIC2zyj5ONZ9DnQxFWntKVoGUk0VQSVEm1UQbMf_dhI)

Outra variável que retirei foi “day”, que apresentava valores de 1 a 31. Essa variável era muito específica e poderia gerar “ruído” no modelo. Optamos por não incluí-la.

  

### Dummização

O próximo passo foi realizar a dummização (one-hot encoding) das variáveis qualitativas restantes (job, marital, education, default, housing, loan, contact, month, poutcome).

Isso resultou em um novo banco de dados com 42 variáveis, incluindo as dummies.

### Primeiro Modelo

Construí o primeiro modelo com todas as variáveis, resultando em um modelo bom, com um log-likelihood de -10792, um ótimo número para um modelo com 45.000 observações.

O p-value do modelo indicou que pelo menos um beta é significativo e os p-values dos betas mostraram que muitas variáveis tinham potencial para permanecer no modelo, enquanto outras deveriam ser removidas.

![](https://media.licdn.com/dms/image/v2/D4D12AQEnmNr-t1LoVQ/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1726145676077?e=1735171200&v=beta&t=NPFJUVTZu7Y7p2rEcxIgXByTXh5lBAcboRUca1GSKMU)

Utilizei o procedimento stepwise com a função stepwise da biblioteca “”, desenvolvida pelos professores  [Luiz Paulo Fávero](https://www.linkedin.com/in/luiz-paulo-f%C3%A1vero-83a117118/)  e  [Helder Prado Santos](https://www.linkedin.com/in/helderprado/)  do  [MBA USP/Esalq](https://www.linkedin.com/school/mbauspesalq/)  (MBA que estou fazendo e que tem me ajudado a me qualificar para o mercado).

O procedimento stepwise eliminou várias variáveis, incluindo algumas dummies, mas a variável “month” se manteve no modelo e demonstrou grande significância, indicando uma influência importante no modelo.

![](https://media.licdn.com/dms/image/v2/D4D12AQHWQaQaJ6N75g/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1726145693726?e=1735171200&v=beta&t=pl7WyvrvmdQqHdRpeopK0jz1VKpx_vjtFBeTufraVjA)

### Predição e Matriz de Confusão

O próximo passo foi realizar a predição no modelo e criar matrizes de confusão para diferentes cutoffs, a fim de identificar o melhor threshold para o modelo.

Comecei com um cutoff de 0,50, que resultou em uma acurácia de 90%, com uma especificidade quase perfeita de 97%, mas com uma sensitividade baixa de 34%. Busquei um equilíbrio entre sensitividade e especificidade e encontrei um cutoff de 0,12, que proporcionou uma acurácia de 84%, com sensitividade de 82% e especificidade de 84%.

![](https://media.licdn.com/dms/image/v2/D4D12AQHeMyO5NBTouQ/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1726145850316?e=1735171200&v=beta&t=rCNPH7y8fXguQMmKG_O4LGM255v8v22C_XZyEsm-KF8)

### Curva ROC

A curva ROC foi criada para avaliar o desempenho do modelo, independentemente do cutoff. Obtivemos um AUROC de 0,90 (90%), indicando um excelente desempenho do modelo.

![](https://media.licdn.com/dms/image/v2/D4D12AQHzhPc59vmAsA/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1726145861953?e=1735171200&v=beta&t=cVX1ww6B9tWX4zDIzEyk1QQuuF9ZRCxpIkjjNbNDvFI)

### Modelo de Teste

Após finalizar o modelo de treino, apliquei o mesmo processo ao modelo de teste. Removi as variáveis “day” e “balance”, fiz a dummização das variáveis qualitativas e realizei a predição com o cutoff de 0,12. O modelo de teste apresentou um bom equilíbrio entre sensitividade (80%) e especificidade (84%), com uma acurácia de 84%, similar ao modelo de treino.

  

![](https://media.licdn.com/dms/image/v2/D4D12AQGh2WwYpXND3g/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1726145883347?e=1735171200&v=beta&t=z--RrjYNeirRAci8AVRVT4zAL_7Hn9fm2_A3ZnWHZoU)

### Analisando os Dados

![](https://media.licdn.com/dms/image/v2/D4D12AQEqJbxXTz2AyA/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1726146633179?e=1735171200&v=beta&t=kvGpnSl0FF5BIZN76TWAPvFie9BWHrImMCop2x0HoMA)

O modelo se mostrou eficaz tanto no treino quanto no teste. Agora, vamos analisar o que podemos aprender sobre a contratação de termos de depósito pelos clientes analisando os coeficientes dos nossos betas:

-   **“poutcome” - Sucesso na campanha de marketing anterior:**  A variável “poutcome_success” teve o maior impacto positivo na contratação dos termos de depósito. Isso sugere que investir mais tempo em clientes que tiveram sucesso em campanhas anteriores pode ser vantajoso.
-   **“month” - Meses de março, outubro, setembro e dezembro (Influência positiva):**  Esses meses mostraram influência positiva na aquisição dos termos de depósito. Três deles estão no final de um trimestre, o que pode indicar que contatos realizados no final de um trimestre têm um impacto positivo.
-   **“month” - Meses de agosto, julho, novembro e janeiro (Influência negativa):**  Esses meses apresentaram uma alta taxa de recusa. Uma investigação mais aprofundada sobre os motivos pode ser útil, possivelmente por meio de clusterização para entender as semelhanças entre variáveis.

_Errata: Eu comentei em usar a clusterização para entender as semelhanças, mas agora lembrei que a maioria das variaveis que eu gostaria de estudar são qualitativas, então eu usaria uma Anacor para ver a correlação entre as variáveis_

-   **“job” - Estudantes e aposentados:**  Esses grupos apresentaram boa resposta à contratação de termos de depósito. A estabilidade e o baixo risco dos depósitos podem ser atraentes para esses grupos.

  

### Odds e Probabilidade(Adendo!(20/09))

Um elemento que eu poderia ter colocado no meu artigo era a chance(Odds) onde eu faria e^beta ou seja o número de euler elevado a cada um dos betas para entender o quanto maior é a chance de um evento ocorrer determinado pelo impacto da variavel X

Por exemplo o Poutcome_success tem um beta de 2.38 se fizermos o cáculo de odds de ficariamos com 10,82 ou seja 1082%!

Para deixar mais claro, um sucesso na campanha anterior influencia positivamente mais de 1000% comparado às outras informações sobre a campanha anterior

O que nos da uma probabilidade de 91,4% do evento ocorrer se tivermos sucesso na campanha anterior

**Odds**=exp(beta) = 10.82

**P**=Odds/1+Odds ≈ 91.4%

  

### Conclusão

Agradeço a todos que leram este estudo. Foi gratificante aplicar os conhecimentos adquiridos em aula de forma prática. Meu conhecimento sobre o mercado financeiro não é extenso, mas me esforcei para obter conclusões que me pareceram plausíveis.

Se você gostou do artigo, por favor, compartilhe. Isso pode ajudar a abrir novas oportunidades na carreira. Se tiver sugestões ou encontrar algo que precisa ser melhorado, seu feedback é muito bem-vindo, pois estou sempre aprendendo e valorizo muito novas informações.

Muito obrigado a todos e até a próxima!