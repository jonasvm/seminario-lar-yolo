# 🧠 LAR-YOLOv8 – Execução Completa via Docker

Este repositório fornece **um ambiente Docker totalmente configurado** para executar o modelo **LAR-YOLOv8**, garantindo compatibilidade com CUDA, PyTorch e Ultralytics, sem necessidade de ajustes manuais.

O projeto foi preparado para **rodar automaticamente um treino curto e uma inferência**, demonstrando o pipeline completo do modelo com suporte a GPU NVIDIA.

---

## 📘 Visão Geral

O **LAR-YOLOv8** é uma modificação do YOLOv8 desenvolvida por Yihao et al., introduzindo novos módulos de atenção e fusão de recursos para melhorar a eficiência em detecção de objetos.

Devido à rápida evolução das dependências do Ultralytics, este repositório fornece uma **versão Docker compatível e autossuficiente**, que:
- Configura CUDA 11.1, PyTorch 1.8.1 e Ultralytics 8.0.221  
- Clona automaticamente o repositório original  
- Adiciona camadas de compatibilidade (*shims*) e substituições (*fallbacks*) para módulos que não existem nas versões modernas do Ultralytics  
- Executa um experimento de treino e inferência de forma automática, com logs e resultados salvos

---

## 📂 Estrutura do Projeto


---

## ⚙️ O que o Docker faz

O `DockerFile-lar-yolo` executa os seguintes passos:

1. Cria o ambiente base com:
   - **CUDA 11.1 + cuDNN 8**
   - **Python 3.8**
   - **PyTorch 1.8.1+cu111**
   - **Ultralytics 8.0.221**

2. Instala dependências científicas:
   - NumPy, Matplotlib, OpenCV, Pandas, Seaborn, etc.

3. Clona automaticamente o repositório **LAR-YOLOv8**.

4. Cria um script `/usr/local/bin/run_experiment.py` que:
   - Importa e corrige módulos antigos (`ultralytics.yolo.utils → ultralytics.utils`)
   - Define versões simplificadas de blocos ausentes:
     - `C2f_CloAtt` → baseado em `C2f`
     - `Fusion` → fusão tipo *BiFPN*
     - `BiLevelRoutingAttention` → camada identidade (pass-through)
   - Desativa salvamento de checkpoints (`torch.save`) para evitar erros de *pickle*
   - Executa automaticamente:
     - **Treinamento rápido (3 épocas)** no dataset `coco8`
     - **Inferência** na imagem `bus.jpg`
   - Salva os resultados em `/home/jonasvm/lar-yolo/LAR-YOLOv8/runs_auto/predict_coco8`

---

## 🧩 Requisitos

- Sistema operacional Linux (Ubuntu recomendado)  
- Docker instalado  
- GPU NVIDIA configurada  
- NVIDIA Container Toolkit instalado  
  👉 [Guia oficial](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

Teste se o Docker tem acesso à GPU:
```bash
docker run --rm --gpus all nvidia/cuda:11.1.1-cudnn8-devel-ubuntu18.04 nvidia-smi

## 🚀 Rodar

### Build

docker build -f DockerFile-lar-yolo -t lar-yolov8:local .

### Rodar

docker run --rm --gpus all lar-yolov8:local > run.txt

Esse comando vai salvar o output do teste em um arquivo run.txt.

## 📘 Créditos

Código Base: Yihao1998/LAR-YOLOv8
Framework YOLOv8: Ultralytics
Ambiente Docker & Compatibilidade: Adaptado por Jonas VM



