# rmf-ajustes

Correções da comunidade para os dados de antenas do [Redes Móveis Fixas](https://redesmoveisfixas.com).

Os dados de ERBs vêm do licenciamento oficial da Anatel. Às vezes a Anatel registra uma antena na coordenada errada (no centro da cidade, na sede da operadora, deslocada da torre real). Este repositório é onde a comunidade corrige isso.

Toda correção aqui é revisada e aprovada manualmente antes de entrar no mapa.

## Como funciona

Cada correção é uma entrada num arquivo JSON, organizada por estado (UF). Você abre um Pull Request adicionando ou editando uma entrada, ele é revisado, e na próxima atualização dos dados a correção passa a valer no mapa.

## O que dá pra corrigir hoje

- **Posição (latitude/longitude)**: reposicionar a antena para o local real. É por antena, organizada por estado, em `posicao/<UF>.json`.
- **MIMO**: informar a configuração de antenas de um equipamento (ex: `64x64`, `4x4`). É por equipamento, não por antena, então é global (vale no Brasil inteiro). Em `mimo/homologacao.json` (pela homologação) ou `mimo/nome.json` (pelo nome/modelo).
- **Nome do equipamento**: dar um nome comercial legível a um modelo cru. Em `modelo/alias.json`, pela homologação.

Diferença importante: posição é por antena (chave `erb_id`, separada por UF). MIMO e nome de equipamento são por equipamento (chave = homologação ou nome do modelo), e o mesmo equipamento aparece em todo o país, por isso são catálogos globais, sem divisão por estado.

## Como achar o `erb_id` da antena

O `erb_id` é o **Número da Estação** da Anatel, o identificador oficial da antena. Você encontra ele:

- No detalhe da ERB dentro do app / site do RMF.
- Na consulta pública de licenciamento da Anatel.

É esse número que serve de chave da correção.

## Formato de uma correção de posição

Em `posicao/<UF>.json`, adicione uma entrada com o `erb_id` como chave:

```json
{
  "688130933": {
    "lat": -13.549781,
    "lon": -42.485911,
    "fonte": "Street View",
    "autor": "@seu-usuario",
    "obs": "torre fica na quadra ao lado do registro da Anatel"
  }
}
```

Campos:

| Campo   | Obrigatório | O que é |
|---------|-------------|---------|
| `lat`   | sim         | Latitude real, em graus decimais (negativa no Brasil). |
| `lon`   | sim         | Longitude real, em graus decimais (negativa no Brasil). |
| `fonte` | recomendado | Como você chegou nessa posição (Street View, visita, imagem de satélite). |
| `autor` | opcional    | Seu usuário do GitHub, para crédito. |
| `obs`   | opcional    | Qualquer observação que ajude na revisão. |

Só `lat` e `lon` afetam o mapa. Os outros campos existem para a revisão do PR e para dar rastro de quem corrigiu o quê.

## Como contribuir

1. Ache o `erb_id` da antena.
2. Abra o arquivo do estado dela em `posicao/`, por exemplo `posicao/BA.json`. Se o estado ainda não tem arquivo, crie um com `{}`.
3. Adicione (ou edite) a entrada com a coordenada correta e a `fonte`.
4. Abra um Pull Request explicando de onde tirou a posição.

Dicas para o PR ser aprovado rápido:

- Confira que a coordenada está dentro do Brasil e faz sentido para a cidade da antena.
- Uma `fonte` verificável (link do Street View, por exemplo) acelera muito a revisão.
- Um PR por antena, ou por cidade, facilita a análise.
