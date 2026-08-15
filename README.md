# Da Bancada à Clínica

**Instrumento gratuito que estima a posição relativa do desenho de um estudo pré-clínico de terapia gênica frente a um acervo curado de 362 programas históricos, e devolve recomendações acionáveis.**

🔗 **[Abrir a ferramenta](https://SEU-USUARIO.github.io/transla/)** · [Análises de exemplo](#análises-de-exemplo)

---

## O que é

O instrumento recebe a descrição de um projeto pré-clínico de terapia gênica **em planejamento** e devolve
duas coisas: onde aquele desenho se situa em relação a um acervo de 362 programas históricos, e um
conjunto de recomendações sobre as decisões de desenho que mais pesam — e que mais custam para
corrigir depois.

Por trás está um modelo de *gradient boosting* sobre **52 variáveis de desenho experimental**,
com validação cruzada aninhada agrupada por doença.

## Para quem

Pesquisadores desenhando um estudo pré-clínico de terapia gênica; avaliadores de agências de fomento;
programas de pós-graduação. O momento de uso é o **planejamento**, antes de os recursos estarem
comprometidos — é aí que uma recomendação ainda pode mudar o resultado.

## O que **não** é

- **Não estima probabilidade de sucesso.** A base é caso-controle pareada por doença (46,1% de casos
  por construção), portanto não é possível derivar chance absoluta de translação. Qualquer leitura do
  tipo *"este projeto tem X% de chance"* está errada. A saída é **percentil dentro do acervo**.
- **Não é instrumento de decisão.** É de orientação. As recomendações são o produto principal;
  o percentil é contexto.
- **Não substitui** avaliação regulatória, parecer de comitê de ética nem julgamento de especialista.
- **Não pontua fora do domínio.** Projetos fora do escopo de treinamento recebem uma explicação,
  não uma nota. Veja o [caso recusado](casos/mir146b-atc-recusado.html).

## Como usar

1. Abra a ferramenta pelo link acima.
2. Cole a descrição do seu projeto — doença, gene/mecanismo, vetor e a trajetória de validação
   pretendida — ou envie um arquivo `.docx`.
3. Leia as recomendações antes do percentil.

As quatro análises de exemplo rodam sem qualquer configuração e mostram o comportamento do
instrumento, incluindo a recusa.

## Análises de exemplo

| Caso | O que demonstra |
|---|---|
| [Osteoartrite — condrócitos IL-4/TGF-β1, modelo ovino (Auxílio FAPESP)](casos/fapesp-osteoartrite.html) | Projeto em planejamento que acerta as decisões de maior peso: alta prontidão relativa, percentil ~72. |
| [Laceração muscular — circuito lentiviral NF-κB → sTGFβRII-Fc](casos/demambro-laceracao.html) | Condição não monogênica: leitura entregue com confiança declaradamente reduzida. |
| [Doença arterial periférica — minicírculos + CRISPR-Cas13d anti-PHD2](casos/bartolomeo-dap.html) | Prova de conceito ainda in vitro: prontidão baixa no desenho atual, com o caminho para elevá-la. |
| [**Câncer anaplásico de tireoide — CRISPR/Cas9n anti-miR-146b**](casos/mir146b-atc-recusado.html) | **Recusa de pontuação** por estar fora do domínio de validade. O comportamento que distingue este instrumento de um classificador qualquer. |

## Os números do modelo

**Modelo A (referência) — 52 variáveis de desenho, n = 362 programas (167 casos / 195 controles), 94 doenças:**

| Métrica | Valor | IC 95% |
|---|---|---|
| **AUROC** | **0,816** | 0,775 – 0,856 |
| AUPRC | 0,814 | 0,762 – 0,863 |
| Escore de Brier | 0,171 | — |
| Prevalência (por construção) | 0,461 | — |
| LR+ do decil superior | 13,2 | — |
| p (permutação em blocos, 5000) | 0,0002 | — |

**Cenários adicionais:**

| Cenário | n | Variáveis | AUROC | IC 95% |
|---|---|---|---|---|
| B — desenho + ações de maturação | 362 | 55 | 0,823 | 0,782 – 0,863 |
| **C — programas de rótulo não contestado** | 321 | 52 | **0,843** | 0,796 – 0,887 |
| D — apenas os programas contestados | 41 | 52 | 0,526 | 0,357 – 0,710 |
| E — apenas variáveis disponíveis no desenho | 362 | 43 | 0,805 | 0,764 – 0,844 |
| F — apenas indicadores de ausência documental | 362 | 37 | 0,6705 | — |
| Comparador: regressão logística | 362 | 52 | 0,804 | — |

O cenário D — desempenho indistinguível do acaso nos 41 programas cujo rótulo é contestado — é
esperado e informativo: onde o rótulo não é confiável, não há padrão a aprender.

> **Todos esses números são percentis e medidas de discriminação. Nenhum é probabilidade de sucesso.**

## Limitações declaradas

Estas limitações são parte do instrumento, não uma nota de rodapé.

- **Viés de averiguação, medido.** O acervo é enriquecido em casos frente a uma amostra aleatória da
  literatura: numa amostra de n = 100 artigos aleatórios do PubMed, apenas **9,0%** eram casos
  (IC95% 4,2–16,4%), o que corresponde a uma **razão de chances de enriquecimento de 11,5**.
  O acervo não é, e não pretende ser, uma amostra representativa da literatura.
- **Silenciamento gênico: não use a ferramenta.** O desempenho nessa modalidade é indistinguível do
  acaso. O modelo só é confiável em adição/reposição gênica.
- **Ausência de validação prospectiva.** Nenhum projeto foi avaliado antes do desfecho e acompanhado
  até ele. Toda a validação é retrospectiva.
- **Desempenho reduzido em condições não monogênicas:** AUROC **0,752** contra **0,844** nas
  monogênicas. Nesse estrato, leia o percentil como ordem de grandeza.
- **Completude documental é confundidor real.** Um modelo construído **só** com indicadores de
  ausência de informação atinge AUROC 0,6705. Parte do sinal vem de quão bem o estudo foi relatado,
  não apenas de como foi desenhado.
- **Teto imposto pelo ruído de rótulo.** Com ~11% de erro residual estimado na base, o teto de
  desempenho alcançável é ≈0,889 — o modelo atinge boa parte dele, e ganhos adicionais dependem mais
  da qualidade do rótulo do que do algoritmo.
- **Falsos positivos são estudos excelentes que não transladaram.** Em quase toda dimensão de
  qualidade eles superam os casos verdadeiros. Boa ciência é necessária e não suficiente: parte da
  decisão translacional não está no papel.
- **O formulário estruturado de 43 perguntas descrito na monografia está planejado e não
  implementado.** Esta versão pública opera sobre texto livre.

## Conteúdo do repositório

```
index.html                              a ferramenta
casos/                                  as quatro análises de exemplo
dados/
  resultados_manuscrito.json            métricas do modelo, todos os cenários
  resultados_sensibilidade.json         análises de sensibilidade e taxonomia de custo
  associacoes_univariaveis_FDR.csv      as 53 variáveis testadas, OR, IC e q de Benjamini-Hochberg
  importancia_features.csv              importância das variáveis no modelo
  features_incluidas_e_excluidas.json   as 52 do Modelo A e as excluídas, com a justificativa
  Figura1..8, DECISAO_*, ONDE_*         figuras da monografia
```

Todos os HTML são estáticos e trazem CSS embutido. Não há build, servidor, framework nem etapa de
compilação — e isso é decisão de projeto, não limitação.

## Privacidade

O texto que você cola ou o arquivo que envia é lido **no seu navegador**; nada é armazenado por este
site, que é estático e não tem servidor, banco de dados, analytics, cookies nem rastreadores.

⚠️ **Exceção importante — "modo ao vivo":** se você optar por colar uma chave de API para avaliar um
projeto real, o texto do seu projeto é enviado diretamente do seu navegador para a API do provedor
que você escolher (Anthropic ou OpenAI), sujeito à política de privacidade **daquele provedor**.
A chave permanece apenas na memória da aba e não é transmitida a este site. As quatro análises de
exemplo e a leitura do arquivo `.docx` funcionam inteiramente no navegador, sem envio de dados.

## Como citar

> SOBRENOME, N. Da Bancada à Clínica: instrumento de posicionamento relativo do desenho pré-clínico em terapia
> gênica. Monografia submetida ao Prêmio Jovem Cientista 2026. Disponível em:
> `https://SEU-USUARIO.github.io/transla/`

<!-- ↑ preencher com o nome da autora e a URL final antes de publicar -->
<!-- ↑ substituir SEU-USUARIO nos três pontos deste README (topo, citação, e o link da ferramenta) -->


## Licença

- **Código** (`index.html`, `casos/*.html`): [MIT](LICENSE)
- **Dados e figuras** (`dados/`): [CC BY 4.0](LICENSE-DADOS)

---

## English

**Da Bancada à Clínica** is a free instrument that estimates the **relative position** of a planned preclinical
gene therapy study design against a curated corpus of **362 historical development programmes**, and
returns actionable recommendations. It is built on a gradient boosting model over 52 experimental
design variables, with nested cross-validation grouped by disease:
**AUROC 0.816 (95% CI 0.775–0.856)**, and 0.843 on programmes with uncontested labels.

**The output is a percentile within the corpus, not a probability of success.** The underlying dataset
is disease-matched case-control (46.1% cases by construction), so absolute translation probability
cannot be estimated from it. The instrument is advisory, not decision-making, and does not replace
regulatory assessment.

**The tool declines to score projects outside its validated domain**, returning an explanation instead
of a number — see the [refused case](casos/mir146b-atc-recusado.html).

**Declared limitations:** measured ascertainment bias (enrichment odds ratio 11.5 against a random
sample of the literature); performance indistinguishable from chance in gene silencing, a modality
where the tool should not be used; no prospective validation; reduced performance in non-monogenic
conditions (AUROC 0.752 vs 0.844); and documentary completeness as a genuine confounder (an
absence-indicators-only model reaches AUROC 0.6705).

All pages are static, self-contained HTML with inline CSS. Code is MIT licensed; data and figures are
CC BY 4.0.
