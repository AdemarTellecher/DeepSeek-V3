--- INÍCIO DO ARQUIVO README_WEIGHTS.md ---

# Documentação do Arquivo de Pesos DeepSeek-V3

## Novos Campos em `config.json`

-   **model_type**: Especifica o tipo de modelo, que é atualizado para `deepseek_v3` nesta versão.
-   **num_nextn_predict_layers**: Indica o número de Módulos de Predição Multi-Token (MTP). Os pesos V3 de código aberto incluem **1 Módulo MTP**.
-   **quantization_config**: Descreve a configuração para quantização FP8.

---

## Visão Geral da Estrutura de Pesos

O arquivo de pesos DeepSeek-V3 consiste em dois componentes principais: **Pesos do Modelo Principal** e **Módulos MTP**.

### 1. Pesos do Modelo Principal

-   **Composição**:
    -   Camadas de incorporação de entrada/saída e um conjunto completo de 61 camadas ocultas do Transformer.
-   **Contagem de Parâmetros**:
    -   Total de parâmetros: **671B**
    -   Parâmetros de ativação: **36,7B** (incluindo 0,9B para Incorporação e 0,9B para o Head de saída).

#### Detalhes Estruturais

-   **Camada de Incorporação**:
    -   `model.embed_tokens.weight`
-   **Camadas Ocultas do Transformer**:
    -   `model.layers.0` a `model.layers.60`, totalizando `num_hidden_layers` camadas.
-   **Camada de Saída**:
    -   `model.norm.weight`
    -   `lm_head.weight`

### 2. Módulos de Predição Multi-Token (MTP)

-   **Composição**:
    -   Módulos MTP adicionais definidos pelo campo `num_nextn_predict_layers`. Neste modelo, o valor é definido como 1.
-   **Contagem de Parâmetros**:
    -   Parâmetros: **11,5B de parâmetros únicos**, excluindo os 0,9B de Incorporação e 0,9B de Head de saída compartilhados).
    -   Parâmetros de ativação: **2,4B** (incluindo os 0,9B de Incorporação e 0,9B de Head de saída compartilhados).

#### Detalhes Estruturais

-   **embed_tokens**: **Compartilha parâmetros** com a camada de Incorporação dos pesos do Modelo Principal.
-   **enorm & hnorm**: Parâmetros RMSNorm necessários para decodificação especulativa.
-   **eh_proj**: Parâmetros para projeção de redução de dimensionalidade nos resultados da norma.
-   **Camada Oculta Adicional do Transformer**:
    -   `model.layers.61.self_attn & mlp` (estrutura idêntica às camadas ocultas do Modelo Principal).
-   **shared_head**: **Compartilha parâmetros** com o Head de saída dos pesos do Modelo Principal.

---

### Regras de Carregamento

-   **Pesos do Modelo Principal**: Carregados via o parâmetro `num_hidden_layers` em `config.json`.
-   **Módulos MTP**: Carregados via o parâmetro `num_nextn_predict_layers`, com IDs de camada anexados imediatamente após as camadas ocultas do Modelo Principal. Por exemplo:
    -   Se `num_hidden_layers = 61` e `num_nextn_predict_layers = 1`, o ID da camada do Módulo MTP é `61`.

---

## Documentação de Pesos FP8

O DeepSeek-V3 oferece suporte nativo ao formato de peso FP8 com escalonamento de bloco 128x128.

### Configuração FP8

O arquivo de peso FP8 introduz um campo `quantization_config` para descrever o método de quantização. Abaixo está um exemplo de configuração:

```json
"quantization_config": {
  "activation_scheme": "dynamic",
  "fmt": "e4m3",
  "quant_method": "fp8",
  "weight_block_size": [128, 128]
}
```

-   **Formato de Quantização**:
    -   Tipo de formato: `fp8` e `e4m3` (correspondente a `torch.float8_e4m3fn`).
    -   Tamanho do bloco de peso: `128x128`.
-   **Esquema de Quantização de Ativação**:
    -   Utiliza quantização de ativação dinâmica (`dynamic`).

### Método de Desquantização

O arquivo de peso FP8 inclui um campo `weight_scale_inv`, que armazena a escala de desquantização para cada bloco de peso.

-   **Formato de Armazenamento**: `Tensor float32`, armazenado junto com os dados de peso.
-   **Fórmula de Desquantização**:
    -   Se o bloco de peso não estiver alinhado a 128, ele é preenchido com zero até 128 antes de calcular a escala. Após a quantização, a parte preenchida é removida.
    -   O processo de desquantização é realizado como: `(bloco de peso 128x128) * weight_scale_inv`.

Por meio da desquantização dos pesos FP8, as operações de tempo de execução permitem a quantização online com uma granularidade de `por-token-por-128-canal`.

---
