# VisDrone MOT to YOLO Converter & Training Pipeline

## 📋 Descrição

Este projeto é um pipeline completo para conversão do dataset VisDrone MOT (Multiple Object Tracking) para o formato YOLO e treinamento de modelos de detecção de objetos. O sistema foi otimizado para execução no Google Colab com recursos limitados, processando apenas uma amostra reduzida do dataset para evitar problemas de timeout.

## 🚀 Características Principais

- **Conversão Automática**: Converte anotações VisDrone para formato YOLO
- **Pipeline de Treinamento**: Treinamento completo com YOLOv11
- **Avaliação Integrada**: Sistema de métricas e relatórios automáticos
- **Otimizado para Colab**: Configurado para recursos limitados
- **Visualizações**: Gráficos e matrizes de confusão automáticos
- **Relatórios HTML**: Relatórios detalhados de performance

## 📊 Dataset Suportado

O projeto trabalha com o dataset **VisDrone MOT** que contém:
- **Classes**: pedestrian, people, car, van, truck, bus, motorcycle
- **Formato Original**: Anotações em formato VisDrone
- **Formato Destino**: YOLO (normalizado)

### Mapeamento de Classes
```
VisDrone → YOLO
1 (pedestrian) → 0 (person)
2 (people) → 0 (person)
4 (car) → 1 (car)
5 (van) → 2 (van)
6 (truck) → 3 (truck)
9 (bus) → 4 (bus)
10 (motor) → 5 (motorcycle)
```

## 🛠️ Instalação e Configuração

### Pré-requisitos
```bash
# Instalar dependências principais
pip install ultralytics
pip install polars
pip install numpy
pip install pillow
pip install tqdm
pip install matplotlib
pip install seaborn
pip install pandas
pip install opencv-python
pip install scikit-learn
```

### Configuração do Google Drive
1. Monte seu Google Drive no Colab:
```python
from google.colab import drive
drive.mount('/content/drive')
```

2. Organize seus dados no Drive:
```
/content/drive/MyDrive/visdrone_mot/
├── yolov8_detection/
│   ├── MOT-TRAIN/
│   │   ├── sequences/
│   │   └── annotations/
│   └── MOT-VAL/
│       ├── sequences/
│       └── annotations/
```

## 🎯 Como Usar

### 1. Conversão VisDrone → YOLO

```python
# Executar conversão
python mot4.py
```

**Configurações Padrão:**
- Máximo 60 sequências
- Máximo 100 imagens por sequência
- Split 80% train / 20% val
- Classes mapeadas automaticamente

### 2. Treinamento do Modelo

```python
# Executar pipeline completo de treinamento
from mot4 import run_complete_training_pipeline

success = run_complete_training_pipeline()
```

**Parâmetros de Treinamento:**
- Modelo: YOLOv11n (nano)
- Épocas: 50
- Batch Size: 16
- Tamanho da Imagem: 768px
- Learning Rate: 0.01

### 3. Usar Modelo Treinado

```python
from ultralytics import YOLO

# Carregar modelo treinado
model = YOLO('/content/drive/MyDrive/visdrone_weights/visdrone_best.pt')

# Fazer predições
results = model('sua_imagem.jpg')

# Visualizar resultados
results[0].show()
```

## 📁 Estrutura de Arquivos

```
MOT4/
├── mot4.py                          # Arquivo principal
├── README.md                        # Este arquivo
└── outputs/
    ├── visdrone_yolo/               # Dataset convertido
    │   ├── images/
    │   │   ├── train/
    │   │   └── val/
    │   ├── labels/
    │   │   ├── train/
    │   │   └── val/
    │   └── dataset.yaml
    ├── visdrone_weights/            # Pesos treinados
    │   ├── visdrone_best.pt
    │   └── visdrone_last.pt
    └── visdrone_results/            # Relatórios e métricas
        ├── training_evaluation_report.html
        ├── training_metrics.json
        ├── metrics_overview.png
        └── confusion_matrix.png
```

## 📊 Métricas e Avaliação

O sistema gera automaticamente:

### Métricas Principais
- **mAP@0.5**: Mean Average Precision com IoU 0.5
- **mAP@0.5:0.95**: Mean Average Precision com IoU 0.5-0.95
- **Precision**: Precisão média
- **Recall**: Recall médio
- **F1-Score**: Score F1 balanceado

### Métricas por Classe
- AP@0.5 e AP@0.5:0.95 para cada classe
- Precision e Recall individuais
- Análise de confusão entre classes

### Métricas de Performance
- Tempo de inferência por imagem
- FPS (Frames Per Second)
- Tempo total de treinamento

## 🎨 Visualizações Geradas

1. **Visão Geral das Métricas**: Gráfico com todas as métricas principais
2. **Matriz de Confusão**: Análise de erros de classificação
3. **Métricas por Classe**: Performance individual de cada classe
4. **Precision vs Recall**: Análise de trade-off

## 📄 Relatórios

### Relatório HTML
- Interface web completa
- Métricas organizadas em cards
- Tabelas de performance por classe
- Recomendações baseadas nos resultados
- Visualizações integradas

### Arquivo JSON
- Dados estruturados para análise posterior
- Configurações de treinamento
- Métricas numéricas
- Caminhos dos arquivos gerados

## ⚙️ Configurações Avançadas

### Personalizar Conversão
```python
from mot4 import Config, VisDroneConverter

# Configuração personalizada
config = Config(
    max_sequences=100,           # Mais sequências
    max_images_per_sequence=200, # Mais imagens por sequência
    train_val_split=0.9          # Split diferente
)

converter = VisDroneConverter(config)
converter.run_conversion()
```

### Personalizar Treinamento
```python
from mot4 import TrainingConfig, YOLOTrainer

# Configuração de treinamento personalizada
config = TrainingConfig()
config.epochs = 100              # Mais épocas
config.batch_size = 32           # Batch maior
config.img_size = 1024          # Imagens maiores
config.base_model = "yolo11s.pt" # Modelo maior

trainer = YOLOTrainer(config)
trainer.train()
```

## 🔧 Solução de Problemas

### Problemas Comuns

1. **Erro de memória no Colab**
   - Reduza `max_sequences` e `max_images_per_sequence`
   - Use modelo YOLOv11n (nano)

2. **Dataset não encontrado**
   - Verifique se o Google Drive está montado
   - Confirme a estrutura de diretórios

3. **Timeout no treinamento**
   - Reduza o número de épocas
   - Use batch size menor
   - Salve checkpoints com mais frequência

### Logs e Debug
O sistema gera logs detalhados com emojis para facilitar o acompanhamento:
- 🚀 Início de operações
- ✅ Sucessos
- ❌ Erros
- ⚠️ Avisos
- 📊 Estatísticas

## 📈 Performance Esperada

### Com Dataset Reduzido (60 sequências)
- **Tempo de Conversão**: ~5-10 minutos
- **Tempo de Treinamento**: ~30-60 minutos
- **mAP@0.5 Esperado**: 0.3-0.6 (dependendo da qualidade dos dados)

### Com Dataset Completo
- **Tempo de Conversão**: ~2-4 horas
- **Tempo de Treinamento**: ~4-8 horas
- **mAP@0.5 Esperado**: 0.5-0.8

## 🤝 Contribuição

Para contribuir com o projeto:

1. Fork o repositório
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 👨‍💻 Autor

Desenvolvido com práticas modernas de Python e otimizado para Google Colab.

## 🔗 Links Úteis

- [Dataset VisDrone](https://github.com/VisDrone/VisDrone-Dataset)
- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
- [Google Colab](https://colab.research.google.com/)

---

**Nota**: Este projeto foi otimizado para execução no Google Colab com recursos limitados. Para uso em ambiente local com mais recursos, ajuste as configurações conforme necessário.
