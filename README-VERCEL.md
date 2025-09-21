# VisDrone Training Analysis - Vercel Deploy

## 🚀 Deploy no Vercel

Este projeto contém uma análise interativa dos dados de treinamento do modelo YOLO no dataset VisDrone.

### 📁 Estrutura do Projeto
```
├── public/
│   └── index.html          # Página principal com análise interativa
├── package.json            # Configurações do projeto
├── vercel.json            # Configurações específicas do Vercel
├── .gitignore             # Arquivos ignorados no Git
└── README.md              # Este arquivo
```

### 🛠️ Como Fazer Deploy

#### Opção 1: Via Vercel CLI
```bash
# Instalar Vercel CLI
npm i -g vercel

# Fazer login
vercel login

# Deploy
vercel --prod
```

#### Opção 2: Via GitHub + Vercel Dashboard
1. Faça push do projeto para um repositório GitHub
2. Acesse [vercel.com](https://vercel.com)
3. Conecte sua conta GitHub
4. Importe o repositório
5. Deploy automático!

#### Opção 3: Drag & Drop
1. Acesse [vercel.com](https://vercel.com)
2. Faça login
3. Arraste a pasta do projeto para a área de deploy
4. Aguarde o deploy automático

### 📊 Funcionalidades
- ✅ Gráficos interativos com Chart.js
- ✅ Análise completa dos dados de treinamento
- ✅ Visualizações responsivas
- ✅ Interpretação científica dos resultados
- ✅ Timeline da jornada de aprendizado

### 🔧 Configurações
- **Framework:** Static HTML
- **Build Command:** Não necessário
- **Output Directory:** Root
- **Install Command:** Não necessário

### 🌐 URLs de Acesso
Após o deploy, seu site estará disponível em:
- `https://seu-projeto.vercel.app`
- `https://seu-projeto.vercel.app/analysis`

### 📈 Dados Analisados
- **50 épocas** de treinamento
- **mAP50:** 25.7% → 92.8%
- **Precisão:** 91.5%
- **Recall:** 88.1%
- **Análise de overfitting:** Ausente
- **Learning rate:** Otimizado com warmup e decay

### 🎯 Resultado
Um detector de objetos YOLO robusto e confiável para o dataset VisDrone!
