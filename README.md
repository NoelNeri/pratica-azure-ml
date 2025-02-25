## Explore o Machine Learning automatizado no Azure Machine Learning
Neste exercício, você usará o recurso de machine learning automatizado no Azure Machine Learning para treinar e avaliar um modelo de machine learning. Em seguida, você implantará e testará o modelo treinado.

Este exercício deve levar aproximadamente 35 minutos para ser concluído.

Criar um espaço de trabalho do Azure Machine Learning
Para usar o Azure Machine Learning, você precisa provisionar um workspace do Azure Machine Learning na sua assinatura do Azure. Então você poderá usar o Azure Machine Learning Studio para trabalhar com os recursos no seu workspace.

Dica : se você já tiver um espaço de trabalho do Azure Machine Learning, poderá usá-lo e pular para a próxima tarefa.

Entre no portal do Azure usando https://portal.azure.comsuas credenciais da Microsoft.

Selecione + Criar um recurso , pesquise por Machine Learning e crie um novo recurso do Azure Machine Learning com as seguintes configurações:
Assinatura : sua assinatura do Azure .
Grupo de recursos : crie ou selecione um grupo de recursos .
Nome : Insira um nome exclusivo para seu espaço de trabalho .
Região : Leste dos EUA.
Conta de armazenamento : observe a nova conta de armazenamento padrão que será criada para seu espaço de trabalho .
Cofre de chaves : observe o novo cofre de chaves padrão que será criado para seu espaço de trabalho .
Insights do aplicativo : observe o novo recurso padrão de insights do aplicativo que será criado para seu espaço de trabalho .
Registro de contêiner : Nenhum ( um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner ).
Selecione Review + create e, em seguida, selecione Create . Aguarde a criação do seu workspace (pode levar alguns minutos) e, em seguida, vá para o recurso implantado.
Estúdio de lançamento
No seu recurso de espaço de trabalho do Azure Machine Learning, selecione Iniciar estúdio (ou abra uma nova guia do navegador e navegue até https://ml.azure.com e entre no estúdio do Azure Machine Learning usando sua conta da Microsoft). Feche todas as mensagens exibidas.

No Azure Machine Learning Studio, você deve ver seu workspace recém-criado. Caso contrário, selecione All workspaces no menu à esquerda e, em seguida, selecione o workspace que você acabou de criar.

## Use aprendizado de máquina automatizado para treinar um modelo
O machine learning automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguéis de bicicletas que devem ser esperados em um determinado dia, com base em características sazonais e meteorológicas.

Citação : Os dados usados ​​neste exercício são derivados do Capital Bikeshare e são usados ​​de acordo com o contrato de licença de dados publicado .

No Azure Machine Learning Studio , visualize a página ML automatizado (em Criação ).

Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Avançar conforme necessário para avançar pela interface do usuário:

Configurações básicas :

Nome do trabalho : O campo Nome do trabalho já deve estar preenchido previamente com um nome exclusivo. Mantenha-o como está.
Novo nome do experimento :mslearn-bike-rental
Descrição : Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
Tags : nenhuma
Tipo de tarefa e dados :

Selecione o tipo de tarefa : Regressão
Selecionar conjunto de dados : Crie um novo conjunto de dados com as seguintes configurações:
Tipo de dados :
Nome :bike-rentals
Descrição :Historic bike rental data
Tipo : Tabela (mltable)
Fonte de dados :
Selecione De arquivos locais
Tipo de armazenamento de destino :
Tipo de armazenamento de dados : Azure Blob Storage
Nome : workspaceblobstore
Seleção de MLtable :
Pasta de upload : Baixe e descompacte a pasta que contém os dois arquivos que você precisa enviar https://aka.ms/bike-rentals
Selecione Criar . Após a criação do conjunto de dados, selecione o conjunto de dados bike-rentals para continuar a enviar o trabalho de ML automatizado.

Configurações da tarefa :

Tipo de tarefa : Regressão
Conjunto de dados : aluguel de bicicletas
Coluna de destino : aluguéis (inteiro)
Configurações adicionais :
Métrica primária : NormalizedRootMeanSquaredError
Explique o melhor modelo : Não selecionado
Habilitar empilhamento de conjunto : Não selecionado
Usar todos os modelos suportados : Não selecionado. Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
Modelos permitidos : Selecione apenas RandomForest e LightGBM — normalmente você tentaria o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.
Limites : Expandir esta seção
Máximo de ensaios :3
Máximo de ensaios simultâneos :3
Máximo de nós :3
Limite de pontuação métrica : 0.085( para que se um modelo atingir uma pontuação métrica de erro quadrático médio normalizado de 0,085 ou menos, o trabalho termine. )
Tempo limite do experimento :15
Tempo limite de iteração :15
Habilitar rescisão antecipada : Selecionado
Validação e teste :
Tipo de validação : Divisão de validação de trem
Porcentagem de dados de validação : 10
Conjunto de dados de teste : Nenhum
Calcular :

Selecione o tipo de computação : Sem servidor
Tipo de máquina virtual : CPU
Camada de máquina virtual : Dedicada
Tamanho da máquina virtual : Standard_DS3_V2*
Número de instâncias : 1
* Se sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

Envie o trabalho de treinamento. Ele inicia automaticamente.

Espere o trabalho terminar. Pode levar um tempo — agora pode ser uma boa hora para um coffee break!

## Avalie o melhor modelo
Quando o trabalho de aprendizado de máquina automatizado estiver concluído, você poderá revisar o melhor modelo treinado.

Na guia Visão geral do trabalho de aprendizado de máquina automatizado, observe o melhor resumo do modelo. Captura de tela do melhor resumo do modelo do trabalho de aprendizado de máquina automatizado com uma caixa ao redor do nome do algoritmo.

Selecione o texto em Nome do algoritmo para o melhor modelo para visualizar seus detalhes.

Selecione a guia Métricas e selecione os gráficos residuais e prediction_true , caso ainda não estejam selecionados.

Revise os gráficos que mostram o desempenho do modelo. O gráfico de resíduos mostra os resíduos (as diferenças entre valores previstos e reais) como um histograma. O gráfico predict_true compara os valores previstos com os valores verdadeiros.

## Implantar e testar o modelo
Na guia Modelo para o melhor modelo treinado pelo seu trabalho de aprendizado de máquina automatizado, selecione Implantar e use a opção Ponto de extremidade em tempo real para implantar o modelo com as seguintes configurações:
Máquina virtual : Standard_DS3_v2
Contagem de instâncias : 3
Ponto final : Novo
Nome do ponto de extremidade : deixe o padrão ou certifique-se de que seja globalmente exclusivo
Nome da implantação : Deixe o padrão
Inferência de coleta de dados : Desativado
Modelo de pacote : Desativado
Observação: se você receber uma mensagem de que não há cota suficiente para selecionar a máquina virtual Standard_DS3_v2 , selecione uma diferente.

Aguarde o início da implantação - isso pode levar alguns segundos. O status de Implantação para o endpoint predict-rentals será indicado na parte principal da página como Running .
Aguarde até que o status Deploy mude para Succeeded . Isso pode levar de 5 a 10 minutos.


## Teste o serviço implantado
Agora você pode testar seu serviço implantado.

No Azure Machine Learning Studio, no menu à esquerda, selecione Endpoints e abra o endpoint em tempo real predict-rentals .

Na página do endpoint em tempo real do predict-rentals, visualize a guia Teste .

No painel Dados de entrada para testar o ponto de extremidade , substitua o JSON do modelo pelos seguintes dados de entrada:

código
   {
  "input_data": {
    "columns": [
      "day",
      "mnth",
      "year",
      "season",
      "holiday",
      "weekday",
      "workingday",
      "weathersit",
      "temp",
      "atemp",
      "hum",
      "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
 }

Clique no botão Testar .

Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhante a este:

código
 [
   352.3564674945718
 ]
O painel de teste pegou os dados de entrada e usou o modelo que você treinou para retornar o número previsto de aluguéis.

Vamos rever o que você fez. Você usou um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de aluguéis de bicicletas esperados em um determinado dia, com base em características sazonais e meteorológicas .

## Limpar
O serviço web que você criou está hospedado em uma Azure Container Instance . Se você não pretende experimentá-lo mais, você deve excluir o endpoint para evitar acumular uso desnecessário do Azure.

No Azure Machine Learning Studio , na guia Endpoints , selecione o endpoint predict-rentals . Em seguida, selecione Delete e confirme que deseja excluir o endpoint.

Excluir sua computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, você será cobrado por um pequeno valor pelo armazenamento de dados, desde que o espaço de trabalho do Azure Machine Learning exista em sua assinatura. Se você tiver terminado de explorar o Azure Machine Learning, poderá excluir o espaço de trabalho do Azure Machine Learning e os recursos associados.

Para excluir seu espaço de trabalho:

No portal do Azure , na página Grupos de recursos , abra o grupo de recursos que você especificou ao criar seu espaço de trabalho do Azure Machine Learning.
Clique em Excluir grupo de recursos , digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione Excluir .
