<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<div align="center">
  <img src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/logo.svg?raw=true" width="60%" alt="DeepSeek-V3" />
</div>
<hr>
<div align="center" style="line-height: 1;">
  <a href="https://www.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Homepage" src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/badge.svg?raw=true" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://chat.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Chat" src="https://img.shields.io/badge/🤖%20Chat-DeepSeek%20V3-536af5?color=536af5&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://huggingface.co/deepseek-ai" target="_blank" style="margin: 2px;">
    <img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-DeepSeek%20AI-ffc107?color=ffc107&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://discord.gg/Tc7c45Zzu5" target="_blank" style="margin: 2px;">
    <img alt="Discord" src="https://img.shields.io/badge/Discord-DeepSeek%20AI-7289da?logo=discord&logoColor=white&color=7289da" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/qr.jpeg?raw=true" target="_blank" style="margin: 2px;">
    <img alt="Wechat" src="https://img.shields.io/badge/WeChat-DeepSeek%20AI-brightgreen?logo=wechat&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://twitter.com/deepseek_ai" target="_blank" style="margin: 2px;">
    <img alt="Twitter Follow" src="https://img.shields.io/badge/Twitter-deepseek_ai-white?logo=x&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://github.com/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-CODE" style="margin: 2px;">
    <img alt="Code License" src="https://img.shields.io/badge/Code_License-MIT-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-MODEL" style="margin: 2px;">
    <img alt="Model License" src="https://img.shields.io/badge/Model_License-Model_Agreement-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>


<p align="center">
  <a href="DeepSeek_V3.pdf"><b>link para o Paper do DeepSeek</b>👁️</a>
</p>


## 1. Introdução

Apresentamos o DeepSeek-V3, um modelo de linguagem robusto de Mistura de Especialistas (MoE) com 671 bilhões de parâmetros totais, dos quais 37 bilhões são ativados para cada token.

Para alcançar inferência eficiente e treinamento econômico, o DeepSeek-V3 adota as arquiteturas Multi-head Latent Attention (MLA) e DeepSeekMoE, que foram extensivamente validadas no DeepSeek-V2.

Além disso, o DeepSeek-V3 é pioneiro em uma estratégia livre de perdas auxiliares para balanceamento de carga e define um objetivo de treinamento de predição multi-token para um desempenho mais forte.

Nós pré-treinamos o DeepSeek-V3 em 14,8 trilhões de tokens diversos e de alta qualidade, seguido por etapas de Ajuste Fino Supervisionado e Aprendizado por Reforço para aproveitar ao máximo suas capacidades.

Avaliações abrangentes revelam que o DeepSeek-V3 supera outros modelos de código aberto e alcança um desempenho comparável aos principais modelos de código fechado.

Apesar de seu excelente desempenho, o DeepSeek-V3 requer apenas 2,788 milhões de horas de GPU H800 para seu treinamento completo.

Além disso, seu processo de treinamento é notavelmente estável. Durante todo o processo de treinamento, não experimentamos nenhum pico de perda irrecuperável nem realizamos nenhum rollback.

<p align="center">
  <img width="80%" src="figures/benchmark.png">
</p>

### 2. Resumo do Modelo

---

**Arquitetura: Estratégia Inovadora de Balanceamento de Carga e Objetivo de Treinamento**

- Com base na arquitetura eficiente do DeepSeek-V2, somos pioneiros em uma estratégia livre de perdas auxiliares para balanceamento de carga, que minimiza a degradação de desempenho que surge ao incentivar o balanceamento de carga.
- Investigamos um objetivo de Predição Multi-Token (MTP) e provamos que ele é benéfico para o desempenho do modelo. Ele também pode ser usado para decodificação especulativa para aceleração da inferência.

---

**Pré-Treinamento: Rumo à Eficiência Máxima de Treinamento**

- Projetamos uma estrutura de treinamento de precisão mista FP8 e, pela primeira vez, validamos a viabilidade e a eficácia do treinamento FP8 em um modelo de escala extremamente grande.
- Através do co-design de algoritmos, frameworks e hardware, superamos o gargalo de comunicação no treinamento MoE entre nós, quase alcançando a sobreposição completa de computação-comunicação. Isso aumenta significativamente nossa eficiência de treinamento e reduz os custos de treinamento, permitindo-nos ampliar ainda mais o tamanho do modelo sem sobrecarga adicional.
- Com um custo econômico de apenas 2,664 milhões de horas de GPU H800, concluímos o pré-treinamento do DeepSeek-V3 em 14,8T tokens, produzindo o modelo base de código aberto mais forte atualmente. As etapas de treinamento subsequentes após o pré-treinamento exigem apenas 0,1 milhão de horas de GPU.

---

**Pós-Treinamento: Destilação de Conhecimento do DeepSeek-R1**

- Introduzimos uma metodologia inovadora para destilar as capacidades de raciocínio do modelo de Cadeia Longa de Pensamento (CoT), especificamente de um dos modelos da série DeepSeek R1, em LLMs padrão, particularmente o DeepSeek-V3. Nosso pipeline incorpora elegantemente os padrões de verificação e reflexão do R1 no DeepSeek-V3 e melhora notavelmente seu desempenho de raciocínio. Enquanto isso, também mantemos um controle sobre o estilo e o comprimento da saída do DeepSeek-V3.



## 3. Downloads do Modelo

<div align="center">

| **Modelo**         | **#Total de Parâmetros** | **#Parâmetros Ativados** | **Tamanho do Contexto** | **Download**                                                                   |
| :-----------------: | :---------------------: | :----------------------: | :--------------------: | :-----------------------------------------------------------------------------: |
| DeepSeek-V3-Base    |        671B        |          37B          |          128K         | [🤗 Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3-Base)     |
| DeepSeek-V3         |        671B        |          37B          |          128K         | [🤗 Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3)     |

</div>


> [!NOTE]
> O tamanho total dos modelos DeepSeek-V3 no Hugging Face é de 685B, o que inclui 671B dos pesos do Modelo Principal e 14B dos pesos do Módulo de Predição Multi-Token (MTP).

Para garantir desempenho e flexibilidade ideais, fizemos parceria com comunidades de código aberto e fornecedores de hardware para fornecer várias maneiras de executar o modelo localmente. Para orientação passo a passo, consulte a Seção 6: [Como Executar Localmente](#6-como-executar-localmente).

Para desenvolvedores que desejam se aprofundar, recomendamos explorar [README_WEIGHTS.md](./README_WEIGHTS.md) para detalhes sobre os pesos do Modelo Principal e os Módulos de Predição Multi-Token (MTP). Observe que o suporte a MTP está atualmente em desenvolvimento ativo dentro da comunidade, e agradecemos suas contribuições e feedback.

## 4. Resultados da Avaliação
### Modelo Base
#### Benchmarks Padrão


<div align="center">

|                    | Benchmark (Métrica)   | # Shots | DeepSeek-V2 | Qwen2.5 72B | LLaMA3.1 405B | DeepSeek-V3 |
|--------------------|-----------------------|---------|-------------|-------------|---------------|-------------|
|                    | Arquitetura           | -       | MoE         | Dense       | Dense         | MoE         |
|                    | # Parâmetros Ativados | -       | 21B         | 72B         | 405B          | 37B         |
|                    | # Total de Parâmetros | -       | 236B        | 72B         | 405B          | 671B        |
| Inglês             | Pile-test (BPB)       | -       | 0.606       | 0.638       | **0.542**     | 0.548       |
|                    | BBH (EM)              | 3-shot  | 78.8        | 79.8        | 82.9          | **87.5**     |
|                    | MMLU (Acc.)           | 5-shot  | 78.4        | 85.0        | 84.4          | **87.1**     |
|                    | MMLU-Redux (Acc.)     | 5-shot  | 75.6        | 83.2        | 81.3          | **86.2**     |
|                    | MMLU-Pro (Acc.)       | 5-shot  | 51.4        | 58.3        | 52.8          | **64.4**     |
|                    | DROP (F1)             | 3-shot  | 80.4        | 80.6        | 86.0          | **89.0**     |
|                    | ARC-Easy (Acc.)       | 25-shot | 97.6        | 98.4        | 98.4          | **98.9**     |
|                    | ARC-Challenge (Acc.)  | 25-shot | 92.2        | 94.5        | **95.3**      | **95.3**     |
|                    | HellaSwag (Acc.)      | 10-shot | 87.1        | 84.8        | **89.2**      | 88.9        |
|                    | PIQA (Acc.)           | 0-shot  | 83.9        | 82.6        | **85.9**      | 84.7        |
|                    | WinoGrande (Acc.)     | 5-shot  | **86.3**    | 82.3        | 85.2          | 84.9        |
|                    | RACE-Middle (Acc.)    | 5-shot  | 73.1        | 68.1        | **74.2**      | 67.1        |
|                    | RACE-High (Acc.)      | 5-shot  | 52.6        | 50.3        | **56.8**      | 51.3        |
|                    | TriviaQA (EM)         | 5-shot  | 80.0        | 71.9        | **82.7**      | **82.9**     |
|                    | NaturalQuestions (EM) | 5-shot  | 38.6        | 33.2        | **41.5**      | 40.0        |
|                    | AGIEval (Acc.)        | 0-shot  | 57.5        | 75.8        | 60.6          | **79.6**     |
| Código              | HumanEval (Pass@1)    | 0-shot  | 43.3        | 53.0        | 54.9          | **65.2**     |
|                    | MBPP (Pass@1)         | 3-shot  | 65.0        | 72.6        | 68.4          | **75.4**     |
|                    | LiveCodeBench-Base (Pass@1) | 3-shot  | 11.6        | 12.9        | 15.5          | **19.4**     |
|                    | CRUXEval-I (Acc.)     | 2-shot  | 52.5        | 59.1        | 58.5          | **67.3**     |
|                    | CRUXEval-O (Acc.)     | 2-shot  | 49.8        | 59.9        | 59.9          | **69.8**     |
| Matemática          | GSM8K (EM)            | 8-shot  | 81.6        | 88.3        | 83.5          | **89.3**     |
|                    | MATH (EM)             | 4-shot  | 43.4        | 54.4        | 49.0          | **61.6**     |
|                    | MGSM (EM)             | 8-shot  | 63.6        | 76.2        | 69.9          | **79.8**     |
|                    | CMath (EM)            | 3-shot  | 78.7        | 84.5        | 77.3          | **90.7**     |
| Chinês             | CLUEWSC (EM)          | 5-shot  | 82.0        | 82.5        | **83.0**      | 82.7        |
|                    | C-Eval (Acc.)         | 5-shot  | 81.4        | 89.2        | 72.5          | **90.1**     |
|                    | CMMLU (Acc.)          | 5-shot  | 84.0        | **89.5**    | 73.7          | 88.8        |
|                    | CMRC (EM)             | 1-shot  | **77.4**    | 75.8        | 76.0          | 76.3        |
|                    | C3 (Acc.)             | 0-shot  | 77.4        | 76.7        | **79.7**      | 78.6        |
|                    | CCPM (Acc.)           | 0-shot  | **93.0**    | 88.5        | 78.6          | 92.0        |
| Multilíngue         | MMMLU-não-Inglês (Acc.) | 5-shot  | 64.0        | 74.8        | 73.8          | **79.4**     |

</div>


> [!NOTE]
> Os melhores resultados são mostrados em negrito. Pontuações com uma diferença não superior a 0,3 são consideradas no mesmo nível. O DeepSeek-V3 alcança o melhor desempenho na maioria dos benchmarks, especialmente em tarefas de matemática e código.
> Para mais detalhes sobre a avaliação, consulte nosso artigo.

#### Janela de Contexto
<p align="center">
  <img width="80%" src="figures/niah.png">
</p>

Resultados da avaliação nos testes "Agulha no Palheiro" (NIAH). O DeepSeek-V3 tem um bom desempenho em todos os comprimentos de janela de contexto até **128K**.

### Modelo de Chat
#### Benchmarks Padrão (Modelos maiores que 67B)

<div align="center">

|                    | **Benchmark (Métrica)** | **DeepSeek V2-0506** | **DeepSeek V2.5-0905** | **Qwen2.5 72B-Inst.** | **Llama3.1 405B-Inst.** | **Claude-3.5-Sonnet-1022** | **GPT-4o 0513** | **DeepSeek V3** |
|--------------------|-------------------------|----------------------|-----------------------|----------------------|-----------------------|----------------------------|-----------------|-----------------|
|                    | Arquitetura             | MoE                  | MoE                   | Dense                | Dense                 | -                          | -               | MoE             |
|                    | # Parâmetros Ativados    | 21B                  | 21B                   | 72B                  | 405B                  | -                          | -               | 37B             |
|                    | # Total de Parâmetros   | 236B                 | 236B                  | 72B                  | 405B                  | -                          | -               | 671B            |
| Inglês             | MMLU (EM)               | 78.2                 | 80.6                  | 85.3                 | **88.6**              | **88.3**                    | 87.2            | **88.5**        |
|                    | MMLU-Redux (EM)         | 77.9                 | 80.3                  | 85.6                 | 86.2                  | **88.9**                    | 88.0            | **89.1**        |
|                    | MMLU-Pro (EM)           | 58.5                 | 66.2                  | 71.6                 | 73.3                  | **78.0**                    | 72.6            | 75.9            |
|                    | DROP (3-shot F1)        | 83.0                 | 87.8                  | 76.7                 | 88.7                  | 88.3                        | 83.7            | **91.6**        |
|                    | IF-Eval (Prompt Strict) | 57.7                 | 80.6                  | 84.1                 | 86.0                  | **86.5**                    | 84.3            | 86.1            |
|                    | GPQA-Diamond (Pass@1)  | 35.3                 | 41.3                  | 49.0                 | 51.1                  | **65.0**                    | 49.9            | 59.1            |
|                    | SimpleQA (Correct)      | 9.0                  | 10.2                  | 9.1                  | 17.1                  | 28.4                        | **38.2**        | 24.9            |
|                    | FRAMES (Acc.)           | 66.9                 | 65.4                  | 69.8                 | 70.0                  | 72.5                        | **80.5**        | 73.3            |
|                    | LongBench v2 (Acc.)     | 31.6                 | 35.4                  | 39.4                 | 36.1                  | 41.0                        | 48.1            | **48.7**        |
| Código              | HumanEval-Mul (Pass@1) | 69.3                 | 77.4                  | 77.3                 | 77.2                  | 81.7                        | 80.5            | **82.6**        |
|                    | LiveCodeBench (Pass@1-COT) | 18.8                 | 29.2                  | 31.1                 | 28.4                  | 36.3                        | 33.4            | **40.5**        |
|                    | LiveCodeBench (Pass@1)  | 20.3                 | 28.4                  | 28.7                 | 30.1                  | 32.8                        | 34.2            | **37.6**        |
|                    | Codeforces (Percentile) | 17.5                 | 35.6                  | 24.8                 | 25.3                  | 20.3                        | 23.6            | **51.6**        |
|                    | SWE Verified (Resolved)  | -                    | 22.6                  | 23.8                 | 24.5                  | **50.8**                    | 38.8            | 42.0            |
|                    | Aider-Edit (Acc.)       | 60.3                 | 71.6                  | 65.4                 | 63.9                  | **84.2**                    | 72.9            | 79.7            |
|                    | Aider-Polyglot (Acc.)   | -                    | 18.2                  | 7.6                  | 5.8                   | 45.3                        | 16.0            | **49.6**        |
| Matemática          | AIME 2024 (Pass@1)     | 4.6                  | 16.7                  | 23.3                 | 23.3                  | 16.0                        | 9.3             | **39.2**        |
|                    | MATH-500 (EM)          | 56.3                 | 74.7                  | 80.0                 | 73.8                  | 78.3                        | 74.6            | **90.2**        |
|                    | CNMO 2024 (Pass@1)     | 2.8                  | 10.8                  | 15.9                 | 6.8                   | 13.1                        | 10.8            | **43.2**        |
| Chinês             | CLUEWSC (EM)           | 89.9                 | 90.4                  | **91.4**             | 84.7                  | 85.4                        | 87.9            | 90.9            |
|                    | C-Eval (EM)            | 78.6                 | 79.5                  | 86.1                 | 61.5                  | 76.7                        | 76.0            | **86.5**        |
|                    | C-SimpleQA (Correct)     | 48.5                 | 54.1                  | 48.4                 | 50.4                  | 51.3                        | 59.3            | **64.8**        |

</div>

> [!NOTE]
> Todos os modelos são avaliados em uma configuração que limita o comprimento da saída para 8K. Benchmarks com menos de 1000 amostras são testados várias vezes usando diferentes configurações de temperatura para obter resultados finais robustos. O DeepSeek-V3 se destaca como o modelo de código aberto com melhor desempenho e também apresenta desempenho competitivo contra modelos proprietários de ponta.

#### Avaliação de Geração Aberta

<div align="center">

| Modelo                | Arena-Hard | AlpacaEval 2.0 |
|-----------------------|------------|----------------|
| DeepSeek-V2.5-0905    | 76.2       | 50.5           |
| Qwen2.5-72B-Instruct  | 81.2       | 49.1           |
| LLaMA-3.1 405B        | 69.3       | 40.5           |
| GPT-4o-0513           | 80.4       | 51.1           |
| Claude-Sonnet-3.5-1022| 85.2       | 52.0           |
| DeepSeek-V3           | **85.5**   | **70.0**       |

</div>

> [!NOTE]
> Avaliações de conversação aberta em inglês. Para AlpacaEval 2.0, usamos a taxa de vitória com controle de comprimento como métrica.

## 5. Site de Chat e Plataforma de API
Você pode conversar com o DeepSeek-V3 no site oficial da DeepSeek: [chat.deepseek.com](https://chat.deepseek.com/sign_in)

Também fornecemos API compatível com OpenAI na DeepSeek Platform: [platform.deepseek.com](https://platform.deepseek.com/)

## 6. Como Executar Localmente

O DeepSeek-V3 pode ser implantado localmente usando o seguinte hardware e software de comunidade de código aberto:

1.  **DeepSeek-Infer Demo**: Fornecemos uma demonstração simples e leve para inferência FP8 e BF16.
2.  **SGLang**: Suporta totalmente o modelo DeepSeek-V3 nos modos de inferência BF16 e FP8, com Predição Multi-Token [em breve](https://github.com/sgl-project/sglang/issues/2591).
3.  **LMDeploy**: Permite inferência FP8 e BF16 eficiente para implantação local e na nuvem.
4.  **TensorRT-LLM**: Atualmente suporta inferência BF16 e quantização INT4/8, com suporte a FP8 em breve.
5.  **vLLM**: Suporta o modelo DeepSeek-V3 com modos FP8 e BF16 para paralelismo de tensor e paralelismo de pipeline.
6.  **AMD GPU**: Permite a execução do modelo DeepSeek-V3 em GPUs AMD via SGLang nos modos BF16 e FP8.
7.  **Huawei Ascend NPU**: Suporta a execução do DeepSeek-V3 em dispositivos Huawei Ascend.

Como o treinamento FP8 é adotado nativamente em nossa estrutura, fornecemos apenas pesos FP8. Se você precisar de pesos BF16 para experimentação, pode usar o script de conversão fornecido para realizar a transformação.

Aqui está um exemplo de conversão de pesos FP8 para BF16:

```shell
cd inference
python fp8_cast_bf16.py --input-fp8-hf-path /caminho/para/pesos_fp8 --output-bf16-hf-path /caminho/para/pesos_bf16
```

> [!NOTE]
> Transformers do Hugging Face ainda não tem suporte direto.**

### 6.1 Inferência com a DeepSeek-Infer Demo (apenas exemplo)

#### Requisitos de Sistema

> [!NOTE]
> Linux com Python 3.10 apenas. Mac e Windows não são suportados.

Dependências:
```
torch==2.4.1
triton==3.0.0
transformers==4.46.3
safetensors==0.4.5
```
#### Pesos do Modelo e Preparação do Código de Demonstração

Primeiro, clone nosso repositório DeepSeek-V3 no GitHub:

```shell
git clone https://github.com/deepseek-ai/DeepSeek-V3.git
```

Navegue até a pasta `inference` e instale as dependências listadas em `requirements.txt`. A maneira mais fácil é usar um gerenciador de pacotes como `conda` ou `uv` para criar um novo ambiente virtual e instalar as dependências.

```shell
cd DeepSeek-V3/inference
pip install -r requirements.txt
```

Baixe os pesos do modelo do Hugging Face e coloque-os na pasta `/caminho/para/DeepSeek-V3`.

#### Conversão de Pesos do Modelo

Converta os pesos do modelo Hugging Face para um formato específico:

```shell
python convert.py --hf-ckpt-path /caminho/para/DeepSeek-V3 --save-path /caminho/para/DeepSeek-V3-Demo --n-experts 256 --model-parallel 16
```

#### Executar

Então você pode conversar com DeepSeek-V3:

```shell
torchrun --nnodes 2 --nproc-per-node 8 --node-rank $RANK --master-addr $ADDR generate.py --ckpt-path /caminho/para/DeepSeek-V3-Demo --config configs/config_671B.json --interactive --temperature 0.7 --max-new-tokens 200
```

Ou inferência em lote em um arquivo determinado:

```shell
torchrun --nnodes 2 --nproc-per-node 8 --node-rank $RANK --master-addr $ADDR generate.py --ckpt-path /caminho/para/DeepSeek-V3-Demo --config configs/config_671B.json --input-file $FILE
```

### 6.2 Inferência com SGLang (recomendado)


O [SGLang](https://github.com/sgl-project/sglang) atualmente oferece suporte a [otimizações MLA](https://lmsys.org/blog/2024-09-04-sglang-v0-3/#deepseek-multi-head-latent-attention-mla-throughput-optimizations), [DP Attention](https://lmsys.org/blog/2024-12-04-sglang-v0-4/#data-parallelism-attention-for-deepseek-models), FP8 (W8A8), cache KV FP8 e Torch Compile, oferecendo latência de ponta e desempenho de throughput entre as estruturas de código aberto.

Notavelmente, o [SGLang v0.4.1](https://github.com/sgl-project/sglang/releases/tag/v0.4.1) oferece suporte total à execução do DeepSeek-V3 em **GPUs NVIDIA e AMD**, tornando-o uma solução altamente versátil e robusta.

O SGLang também oferece suporte a [paralelismo de tensor multi-nó](https://github.com/sgl-project/sglang/tree/main/benchmark/deepseek_v3#example-serving-with-2-h208), permitindo que você execute este modelo em várias máquinas conectadas à rede.

A Predição Multi-Token (MTP) está em desenvolvimento e o progresso pode ser acompanhado no [plano de otimização](https://github.com/sgl-project/sglang/issues/2591).

Aqui estão as instruções de lançamento da equipe do SGLang: https://github.com/sgl-project/sglang/tree/main/benchmark/deepseek_v3

### 6.3 Inferência com LMDeploy (recomendado)
O [LMDeploy](https://github.com/InternLM/lmdeploy), uma estrutura de inferência e serviço flexível e de alto desempenho, projetada para grandes modelos de linguagem, agora oferece suporte ao DeepSeek-V3. Ele oferece recursos de processamento de pipeline offline e implantação online, integrando-se perfeitamente com fluxos de trabalho baseados em PyTorch.

Para instruções passo a passo abrangentes sobre como executar o DeepSeek-V3 com LMDeploy, consulte aqui: https://github.com/InternLM/lmdeploy/issues/2960

### 6.4 Inferência com TRT-LLM (recomendado)

O [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM) agora oferece suporte ao modelo DeepSeek-V3, oferecendo opções de precisão como BF16 e INT4/INT8 somente peso. O suporte para FP8 está atualmente em andamento e será lançado em breve. Você pode acessar o branch personalizado do TRTLLM especificamente para suporte ao DeepSeek-V3 através do seguinte link para experimentar os novos recursos diretamente: https://github.com/NVIDIA/TensorRT-LLM/tree/deepseek/examples/deepseek_v3.

### 6.5 Inferência com vLLM (recomendado)

O [vLLM](https://github.com/vllm-project/vllm) v0.6.6 oferece suporte à inferência do DeepSeek-V3 para os modos FP8 e BF16 em GPUs NVIDIA e AMD. Além das técnicas padrão, o vLLM oferece _paralelismo de pipeline_, permitindo que você execute este modelo em várias máquinas conectadas por redes. Para obter orientação detalhada, consulte as [instruções do vLLM](https://docs.vllm.ai/en/latest/serving/distributed_serving.html). Sinta-se à vontade para acompanhar o [plano de melhoria](https://github.com/vllm-project/vllm/issues/11539) também.

### 6.6 Funcionalidade de Inferência Recomendada com GPUs AMD

Em colaboração com a equipe da AMD, alcançamos suporte imediato para GPUs AMD usando SGLang, com total compatibilidade para precisão FP8 e BF16. Para obter orientação detalhada, consulte as [instruções do SGLang](#63-inference-with-lmdeploy-recommended).

### 6.7 Funcionalidade de Inferência Recomendada com NPUs Huawei Ascend
A estrutura [MindIE](https://www.hiascend.com/en/software/mindie) da comunidade Huawei Ascend adaptou com sucesso a versão BF16 do DeepSeek-V3. Para obter orientação passo a passo sobre NPUs Ascend, siga as [instruções aqui](https://modelers.cn/models/MindIE/deepseekv3).

## 7. Licença
Este repositório de código é licenciado sob [a Licença MIT](LICENSE-CODE). O uso dos modelos DeepSeek-V3 Base/Chat está sujeito à [Licença do Modelo](LICENSE-MODEL). A série DeepSeek-V3 (incluindo Base e Chat) oferece suporte ao uso comercial.

## 8. Citação

```
@misc{deepseekai2024deepseekv3technicalreport,
      title={Relatório Técnico DeepSeek-V3},
      author={DeepSeek-AI and Aixin Liu and Bei Feng and Bing Xue and Bingxuan Wang and Bochao Wu and Chengda Lu and Chenggang Zhao and Chengqi Deng and Chenyu Zhang and Chong Ruan and Damai Dai and Daya Guo and Dejian Yang and Deli Chen and Dongjie Ji and Erhang Li and Fangyun Lin and Fucong Dai and Fuli Luo and Guangbo Hao and Guanting Chen and Guowei Li and H. Zhang and Han Bao and Hanwei Xu and Haocheng Wang and Haowei Zhang and Honghui Ding and Huajian Xin and Huazuo Gao and Hui Li and Hui Qu and J. L. Cai and Jian Liang and Jianzhong Guo and Jiaqi Ni and Jiashi Li and Jiawei Wang and Jin Chen and Jingchang Chen and Jingyang Yuan and Junjie Qiu and Junlong Li and Junxiao Song and Kai Dong and Kai Hu and Kaige Gao and Kang Guan and Kexin Huang and Kuai Yu and Lean Wang and Lecong Zhang and Lei Xu and Leyi Xia and Liang Zhao and Litong Wang and Liyue Zhang and Meng Li and Miaojun Wang and Mingchuan Zhang and Minghua Zhang and Minghui Tang and Mingming Li and Ning Tian and Panpan Huang and Peiyi Wang and Peng Zhang and Qiancheng Wang and Qihao Zhu and Qinyu Chen and Qiushi Du and R. J. Chen and R. L. Jin and Ruiqi Ge and Ruisong Zhang and Ruizhe Pan and Runji Wang and Runxin Xu and Ruoyu Zhang and Ruyi Chen and S. S. Li and Shanghao Lu and Shangyan Zhou and Shanhuang Chen and Shaoqing Wu and Shengfeng Ye and Shengfeng Ye and Shirong Ma and Shiyu Wang and Shuang Zhou and Shuiping Yu and Shunfeng Zhou and Shuting Pan and T. Wang and Tao Yun and Tian Pei and Tianyu Sun and W. L. Xiao and Wangding Zeng and Wanjia Zhao and Wei An and Wen Liu and Wenfeng Liang and Wenjun Gao and Wenqin Yu and Wentao Zhang and X. Q. Li and Xiangyue Jin and Xianzu Wang and Xiao Bi and Xiaodong Liu and Xiaohan Wang and Xiaojin Shen and Xiaokang Chen and Xiaokang Zhang and Xiaosha Chen and Xiaotao Nie and Xiaowen Sun and Xiaoxiang Wang and Xin Cheng and Xin Liu and Xin Xie and Xingchao Liu and Xingkai Yu and Xinnan Song and Xinxia Shan and Xinyi Zhou and Xinyu Yang and Xinyuan Li and Xuecheng Su and Xuheng Lin and Y. K. Li and Y. Q. Wang and Y. X. Wei and Y. X. Zhu and Yang Zhang and Yanhong Xu and Yanhong Xu and Yanping Huang and Yao Li and Yao Zhao and Yaofeng Sun and Yaohui Li and Yaohui Wang and Yi Yu and Yi Zheng and Yichao Zhang and Yifan Shi and Yiliang Xiong and Ying He and Ying Tang and Yishi Piao and Yisong Wang and Yixuan Tan and Yiyang Ma and Yiyuan Liu and Yongqiang Guo and Yu Wu and Yuan Ou and Yuchen Zhu and Yuduan Wang and Yue Gong and Yuheng Zou and Yujia He and Yukun Zha and Yunfan Xiong and Yunxian Ma and Yuting Yan and Yuxiang Luo and Yuxiang You and Yuxuan Liu and Yuyang Zhou and Z. F. Wu and Z. Z. Ren and Zehui Ren and Zhangli Sha and Zhe Fu and Zhean Xu and Zhen Huang and Zhen Zhang and Zhenda Xie and Zhengyan Zhang and Zhewen Hao and Zhibin Gou and Zhicheng Ma and Zhigang Yan and Zhihong Shao and Zhipeng Xu and Zhiyu Wu and Zhongyu Zhang and Zhuoshu Li and Zihui Gu and Zijia Zhu and Zijun Liu and Zilin Li and Ziwei Xie and Ziyang Song and Ziyi Gao and Zizheng Pan},
      year={2024},
      eprint={2412.19437},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2412.19437},
}

## 9. Contato
Se você tiver alguma dúvida, abra um problema ou entre em contato conosco em [service@deepseek.com](service@deepseek.com).

