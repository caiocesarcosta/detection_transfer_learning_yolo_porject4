# Detecção de Objetos em Imagens Usando YOLOv5 e Dataset COCO

Este projeto demonstra a aplicação do framework YOLOv5 para detecção de objetos em imagens, utilizando um conjunto de dados baseado no COCO dataset (Common Objects in Context). Ele automatiza o processo de download das anotações COCO, conversão para o formato YOLO, treinamento do modelo YOLOv5 e detecção de objetos em uma imagem de teste. O código foi organizado utilizando princípios da Programação Orientada a Objetos (POO) em Python, com classes para gerenciamento do dataset e treinamento do modelo.

**Tecnologias Utilizadas:**

*   **Python:** Linguagem de programação principal.
*   **TensorFlow/Keras:** Frameworks de aprendizado de máquina para construir e treinar o modelo CNN.
*   **YOLOv5:** Arquitetura de detecção de objetos em tempo real.
*   **COCO Dataset:** Um grande conjunto de dados para detecção, segmentação e legendas de objetos.
*   **NumPy:** Biblioteca para manipulação de arrays e operações numéricas.
*   **requests:** Para fazer requisições HTTP (baixar arquivos).
*   **PIL (Pillow):** Para manipulação de imagens (abrir, salvar, redimensionar, etc.).
*   **pyyaml:** Para manipulação de arquivos YAML de configuração.

## Estrutura do Código

O código é organizado em duas classes principais:

*   **`YoloImageManagement`:** Responsável por gerenciar o download, a conversão e a organização dos dados de imagem do COCO dataset para o formato YOLO.
*   **`TransferLearningYolo`:** Responsável por clonar o repositório YOLOv5 do GitHub, instalar as dependências, criar o arquivo de configuração YAML e treinar o modelo YOLOv5.

### Classe `YoloImageManagement`

A classe `YoloImageManagement` é responsável por preparar os dados de treinamento para o YOLOv5, realizando os seguintes passos:

**Atributos:**

*   `num_images_to_download` (int): Número de imagens a serem baixadas.
*   `category_names` (list): Lista com os nomes das categorias de objetos a serem detectados (ex: `['person', 'dog', 'cat']`).
*   `data_dir` (str): Diretório onde os dados serão armazenados (`/content/coco_yolo`).
*   `train_dir` (str): Subdiretório para imagens de treinamento (`/content/coco_yolo/images/train`).
*   `labels_dir` (str): Subdiretório para os arquivos de anotação (labels) (`/content/coco_yolo/labels/train`).
*   `img_width` (int): Largura das imagens (em pixels).
*   `coco_data` (dict): Dados carregados do arquivo JSON de anotações do COCO.
*   `category_ids` (list): Lista de IDs das categorias desejadas.
*   `images_data` (list): Lista de metadados das imagens que contêm as categorias desejadas.

**Métodos:**

*   `__init__(self, num_images_to_download: int, category_names: list)`: Construtor da classe. Realiza a montagem do Drive, a criação das pastas e o carregamento das anotações.

*   `mountdriveGoogle(self)`: Monta o Google Drive no ambiente do Colab.

*   `createDiretory(self)`: Cria os diretórios de treinamento e labels, caso não existam.

*   `convertBboxToYolo(self, bbox, img_width, img_height)`: Converte as coordenadas do bounding box do formato COCO para o formato YOLO, normalizando os valores entre 0 e 1.

*   `loadYoloDataset(self)`: Itera sobre as imagens filtradas, baixa as imagens do COCO dataset, extrai as informações de bounding box das anotações, converte para o formato YOLO e salva os arquivos de anotação.

*   `downloadOrOpenAnnotations(self)`: Faz o download do arquivo ZIP de anotações do COCO, extrai o arquivo JSON contendo as anotações e carrega os dados para a variável `coco_data`.

*   `mapImageIDsToCategoryIDs(self)`: Cria um dicionário que mapeia cada ID de imagem para a lista de IDs de categorias presentes nessa imagem.

### Classe `TransferLearningYolo`

A classe `TransferLearningYolo` lida com a configuração, treinamento e execução do modelo YOLOv5.

**Atributos:**

*   `repoYoloGitHub` (str): URL do repositório YOLOv5 no GitHub.
*   `pretrained_weights` (str): Nome do arquivo de pesos pré-treinados a serem usados.
*   `img_width` (int): Largura das imagens (herdada do `coco_dataset`).
*   `batch_size` (int): Tamanho do lote de treinamento.
*   `epochs` (int): Número de épocas de treinamento.
*   `cocoFile` (CocoDatasetHandler): Uma instância da classe CocoDatasetHandler.
*  `data`:  Uma variável de instancia para acesso posterior.
*   `category_names`: Nomes da categoria
*   `num_classes`: quantidade de classes

**Métodos:**

*   `__init__(self, coco_file: YoloImageManagement)`: Construtor da classe. Inicializa a classe.

*   `cloneRepoistoryYoloGitHub(self)`: Clona o repositório YOLOv5 do GitHub.

*   `installDependency(self)`: Instala as dependências do YOLOv5 usando o arquivo `requirements.txt`.

*   `createYAMLConfigurationFileToDataset(self)`: Cria o arquivo `coco.yaml` com as informações sobre o dataset (caminhos, número de classes e nomes das classes).

*   `trainModelYolo(self)`: Executa o script `train.py` do YOLOv5 para treinar o modelo.

*   `detectObjectImage(self)`: Executa o script `detect.py` do YOLOv5 para realizar a detecção de objetos em uma imagem de teste.

*   `train(self)`: Orquestra o processo de treinamento e detecção de objetos.

**Como Usar:**

1.  **Prepare os dados:** Certifique-se de que seus dados de imagem (para treinamento e teste) estejam organizados e acessíveis no Google Drive.
2.  **Ajuste as configurações:** Edite as variáveis de configuração no início do código (como `num_images_to_download` e `category_names`) para corresponder ao seu conjunto de dados e aos objetos que você quer detectar.
3.  **Execute o código:** Execute as células do Colab em sequência.
4.  **Analise os Resultados:** Verifique a saída do treinamento e observe as imagens geradas com as detecções.
