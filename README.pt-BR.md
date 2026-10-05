<p align="center">
  <img src="assets/logo.svg" alt="Logo do Diário de votos" width="96" height="96">
</p>

<h1 align="center">Diário de votos</h1>

<p align="center">
  <a href="README.md">English</a> | <strong>Português (Brasil)</strong>
</p>

Um diário privado, que roda só no navegador, para os seus votos em eleições brasileiras. Registre cada voto, veja como suas escolhas se distribuem no espectro político, nos partidos, no gênero e na raça ao longo dos anos, e confira quem de fato venceu.

É um app estático em HTML/JS (sem framework, sem build, sem backend, sem conta, sem rastreamento). **Seus dados nunca saem do seu navegador.**

> **Aviso:** a interface está em português do Brasil e o conhecimento embutido é sobre eleições brasileiras. Veja as [Limitações](#limitações).

## Recursos

- **Registro de votos:** ano, turno, cargo, local, número, nome, partido, gênero, e opcionalmente pessoa LGBT e cor/raça autodeclarada.
- **Painel** com relatório que dá para compartilhar:
  - onde você está no espectro político (geral e por ano);
  - orientação dos partidos por ano e tabela de partidos por ano;
  - proporção de votos em mulheres, pessoas LGBT, pessoas negras (pretas e pardas), pessoas indígenas, e a divisão por cor/raça;
  - quem recebeu mais de um voto seu;
  - seus votos contra quem venceu, eleição por eleição;
  - eleições em que você não votou (ausências), fora das estatísticas de votos.
- **Opção** de contar ou ignorar os votos de 2º turno.
- **Aba Eleições:** cadastre vencedores para comparar com seus votos. Já há resultados embutidos (veja abaixo).
- **Aba Partidos:** edite a orientação de cada partido, da esquerda à direita, para o espectro refletir o seu critério.
- **Exporte** o relatório em PDF ou PNG e seus dados em JSON. **Importe** um JSON de volta quando quiser.

## Como usar

Não precisa instalar nada.

1. Baixe ou clone o repositório.
2. Abra o `index.html` em um navegador moderno.

Ou sirva localmente:

```bash
git clone https://github.com/vicleme/voting-diary.git
cd voting-diary
python3 -m http.server 8000
# abra http://localhost:8000
```

Para publicar, ative o **GitHub Pages** (Settings > Pages > Deploy from a branch > `main` / raiz).

## Privacidade

- Tudo fica no `localStorage` do navegador (chave `diario_votos_v1`). Limpar os dados do site apaga tudo, então exporte um JSON de backup de vez em quando.
- O app não faz requisições de rede ao rodar: as bibliotecas e fontes que ele usa estão em [`vendor/`](vendor).
- Os arquivos JSON, PDF e PNG exportados contêm o seu histórico de votos. O `.gitignore` incluído ignora os nomes padrão de exportação, para você não commitá-los sem querer.

## Como o resultado é definido

Para cada voto, o app decide "eleito" ou "não eleito" nesta ordem:

1. Se você marcou **"Foi eleito(a)?"** no voto, essa resposta vale.
2. Senão, ele procura vencedores cadastrados para o mesmo ano, cargo e local (Presidente ignora o local).
3. Se o candidato bate com um vencedor, conta como eleito.
4. Se não, conta como não eleito quando o cargo tem uma única vaga (Presidente, Governador, Prefeito) ou quando os vencedores foram cadastrados como **lista completa** ("Lista completa de eleitos"). Caso contrário, fica como "sem dado".

### Resultados embutidos

Já vêm prontos, e seus cadastros próprios são mesclados com eles sem duplicar:

| Cargo | Local | Anos |
| --- | --- | --- |
| Presidente | Brasil | 1989 a 2022 |
| Governador | SP | 2018, 2022, 2026 |
| Senador | SP | 2018, 2022, 2026 |
| Prefeito | Mongaguá (SP) | 2020, 2024, 2025 (suplementar) |

Confira com a fonte oficial ([TSE](https://www.tse.jus.br/)) antes de confiar neles e abra uma issue se achar algum erro.

## Formato dos dados

Os arquivos exportados têm esta forma (os nomes dos campos estão em português por compatibilidade com arquivos já existentes):

```json
{
  "app": "diario-de-votos",
  "versao": 3,
  "nome": "Seu nome (opcional)",
  "votes": [
    {
      "id": 1, "ano": 2022, "turno": 1, "cargo": "Presidente", "local": "",
      "num": "13", "nome": "Candidato", "partido": "PT", "gen": "M",
      "res": "", "lgbt": false, "negra": false, "raca": ""
    }
  ],
  "winners": [
    { "id": 1, "ano": 2022, "cargo": "Governador", "local": "SP",
      "nome": "Candidato", "partido": "PARTIDO", "full": true }
  ],
  "parties": { "PT": 0 },
  "absences": [
    { "id": 1, "ano": 2025, "turno": 1, "local": "Cidade", "desc": "", "just": "sim" }
  ]
}
```

| Campo | Valores |
| --- | --- |
| `gen` | `F`, `M`, `O` |
| `res` | `""` (automático), `"sim"`, `"nao"` |
| `raca` | `""`, `branca`, `preta`, `parda`, `amarela`, `indigena` |
| `parties` | `0` esquerda, `1` centro-esquerda, `2` centro, `3` centro-direita, `4` direita, `""` sem classificação |
| `just` | `sim`, `nao`, `pendente` |

## Limitações

- **Idioma:** a interface está só em português do Brasil. Traduções são bem-vindas.
- **Escopo:** cargos, partidos e resultados embutidos são específicos do Brasil.
- **O espectro é uma simplificação.** Ele faz a média da orientação que você atribui a cada partido e não diz nada sobre os candidatos individuais. A classificação padrão é só um ponto de partida e foi feita para ser editada.
- **Gênero, LGBT e cor/raça são informados por você.** Use a autodeclaração pública dos candidatos (para cor/raça, o registro de candidatura no TSE).

## Estrutura do projeto

```
.
├── index.html          # o app inteiro (HTML, CSS e JS)
├── assets/             # logo e favicon
├── vendor/             # código de terceiros e fontes embutidos, com licenças
├── LICENSE
├── README.md
└── README.pt-BR.md
```

## Contribuindo

Issues e pull requests são bem-vindos, principalmente para traduções, correções nos resultados embutidos e suporte a mais cargos e locais. Mantenha sem dependências: HTML, CSS e JS puros.

## Licença

[MIT](LICENSE). Os softwares de terceiros incluídos mantêm a própria licença:

- [html2canvas](https://github.com/niklasvh/html2canvas) 1.4.1 (MIT)
- [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 (MIT)
- [Bricolage Grotesque](https://github.com/ateliertriay/bricolage) e [Public Sans](https://github.com/uswds/public-sans) via [Fontsource](https://fontsource.org/) (SIL Open Font License 1.1)

Os textos completos estão em [`vendor/licenses/`](vendor/licenses).
