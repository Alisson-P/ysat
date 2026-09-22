# YSAT

### You Sure About That?

**Uma agent skill que discorda de você, de propósito, com evidência.**

> 🇺🇸 Read this in [English](README.md) · Variante tipada: [YSAT-JEV](https://github.com/Alisson-P/ysat-jev)

Meus caros, a maioria dos assistentes é treinada para concordar.
Isso é exatamente o pior comportamento possível no momento em que você está prestes a fechar uma arquitetura, um fornecedor, um prazo ou
uma migração.

A YSAT lê o seu contexto real (código, infraestrutura, e-mail, chat, documentos compartilhados e agenda) e te devolve o argumento que ninguém fez na reunião:
o que pode quebrar, com que gravidade, quando, e qual o teste mais barato para rodar antes de você se comprometer.

Ela é deliberadamente assimétrica. O ônus da prova fica com a decisão, não com a objeção.

```
Você: "Vou migrar o banco do cliente para serverless no fim do mês, o que acha?"

YSAT: Decisão como eu entendi ...
      O que eu olhei ...
      Onde isso pode quebrar    (risco | por quê | evidência | severidade | quando dói)
      Ponto cego ...
      Melhor argumento a favor ... e o que teria que ser verdade
      Veredito: faria por fases, em ondas, etc
      Mitigações ... / Teste mais barato ...
```

## Determinismo

Assistente que concorda parece útil, e concordar é a forma mais barata de parecer útil (e de não ajudar em nada tbem).

Quanto mais vc interage com seu agente aí, quanto mais longa a conversa, mais o seu viés viram as premissas dele,
até o ponto em que a resposta que você recebe é a sua própria opinião, só que melhor formatada.

Então, meus bons, isso é o determinismo.
Dê a um agente um usuário com uma posição e uma conversa comprida o bastante, e ele converge para essa posição.
Isso vai acontecer com seus agentes, ou já aconteceu.
O que não irá acontecer é o agente levantando um contra-ponto sozinho daquilo que você não queria
ouvir, porque nada no ciclo premia esse comportamento.

E isso importa mais justamente onde o risco é maior.
Difícil não é tomar uma decisão assumindo um risco conhecido; isso é a vida.
Difícil é tomar uma decisão sem saber de um risco que era conhecido, mas ninguém trouxe isso pra mesa.

Pensando nisso, criei essa primeira versão da YSAT.
Pra ela nos questionar, e fazer isso pensando na estrutura, não viés.
Preferência se derruba em um único turno de pressão. Regra não.
Por isso os invariantes ficam em uma seção própria, fora da área configurável:
risco primeiro, concordância precisa ser justificada, e o veredito só se move com evidência nova.
Tom, profundidade, idioma, domínios e formato são seus para ajustar.
A parte que se recusa a ceder, não.

A ideia da YSAT NÃO É te impedir de nada.
É ela colocar na mesa aquilo que você não estava olhando, que é exatamente para isso que existe a linha de ponto cego em toda resposta.
Às vezes você lê, discorda e segue em frente do mesmo jeito.
Esse também é um bom resultado, porque agora a decisão carrega o contra-argumento na mão, em vez de descobrir ele em produção.

## Por que ninguém levanta a mão

"toda proatividade será recompensada com mais trabalho"; já ouviu isso?

Você levanta um ponto e "voilà"; agora vc é o dono disso; vai lá e resolve; traga-nos opções...

Levantou o risco que ninguém tinha visto? Acabou de se voluntariar (lembrei do exército agora).
A investigação agora é sua, e junto vêm a mitigação, o acompanhamento e aquela conversa meio fria com quem propôs o plano
que você questionou.

A pessoa aprende essa lição uma vez só. Da segunda em diante, a observação fica na cabeça dela,
e fica por cálculo racional, não por incompetência.
Tem gente que chega a sentir medo de dar a opinião contrária, porque levantar o ponto que ninguém viu é o caminho mais curto para
virar o dono dele. O silêncio mais caro de um projeto não é desconhecimento, é conta de padeiro. Triste, né?! pois é...

Contudo, uma skill, um agente não tem carreira para proteger.
Ele não herda a frente de trabalho que acabou de criar, não disputa a mesma promoção
e não vai sentar do lado do arquiteto que ele contrariou pelos próximos seis meses.
Essa é a única vantagem estrutural que ele tem sobre todo mundo na sala,
e a YSAT existe para gastar exatamente essa vantagem.

Daqui em diante é texto construído por IA, com as ideias que eu passei. Bora.
Testa aí, e nos ajude com feedbacks, issues, etc. TMJ

## Por que isso não é só "ser crítico"

Duas formas de falhar matam um agente crítico. A skill foi construída contra as duas:

| Falha | Trilho |
|---|---|
| **Puxa-saquismo**: concordar porque concordar parece prestativo | Risco primeiro na saída, concordância precisa ser justificada, veredito só muda com evidência nova |
| **Alarmismo**: inventar risco para parecer rigoroso | Escala de severidade fixa, toda afirmação com fonte, teto rígido no número de riscos |

Os comportamentos sensíveis à pressão ficam em uma seção **travada**, que a configuração não
alcança. Tom, profundidade, idioma, domínios e formato são inteiramente seus. Calibragem não é.

## Instalar

Bora instalar. A YSAT segue a [especificação Agent Skills](https://agentskills.io/specification).

**1. Baixe a pasta**

```bash
git clone https://github.com/Alisson-P/ysat.git
```

Sem git? Vá para a página inicial do repositório e clique no botão verde **Code**, à direita,
logo acima da lista de arquivos. Não é a aba Code do menu de cima. Depois **Download ZIP**.
A pasta vem como `ysat-main`, renomeie para `ysat`.

**2. Mova a pasta `ysat` para a pasta de skills do agente**

| Agente | Destino |
|---|---|
| Microsoft 365 Copilot | `Documents/Cowork/skills/ysat/` |
| Outros runtimes | `~/.agent/skills/ysat/` |

No Copilot ela aparece em uns 35 segundos.

> A pasta tem que se chamar exatamente `ysat`, igual ao `name` do frontmatter.
> Se não bater, a skill não carrega e nenhum erro aparece.

**Sem suporte a skills?** Cole o `SKILL.md` no prompt de sistema.

## Usar

Instalou? Maravilha. Agora é só falar qualquer uma dessas, em português ou inglês:

- "discorda dessa decisão"
- "por que isso pode dar errado"
- "estou pensando em fazer X, o que pode falhar"
- "quais os riscos de mover para X"
- "challenge this decision"
- "play devil's advocate"
- "what could go wrong with this"

Acrescente um override na própria frase, válido só para aquela rodada: *modo rápido*, *vai fundo*,
*só os 3 principais*, *foca em custo*, *sem filtro*, *manda em doc*.

## Personalizar

Meu bom, é aqui que você molda a coisa para o seu jeito de trabalhar. Edite o `config.yaml`, toda
chave é opcional.

```yaml
language: auto          # auto | pt-BR | en-US | qualquer locale
depth: standard         # quick | standard | deep
max_risks: 5            # 3 a 8
bluntness: direct       # plain | direct | blunt
focus_domains: [architecture, security, cost, delivery]
sources: [m365, code, web, calendar]
output_format: chat     # chat | document | card
avoid_dashes: true
house_rules:
  - Dado de cliente saindo do tenant é reportado mesmo em severidade baixa.
custom_domains:
  - name: entrega de campo
    questions:
      - Isso muda o que está escrito no documento de escopo assinado?
```

`house_rules` e `custom_domains` são as chaves em que as pessoas acabam morando: as regras inegociáveis
do seu time e as suas próprias categorias de varredura, aplicadas em toda rodada sem você repetir.

Referência completa, overrides de uma rodada e receitas prontas: [references/customization.md](references/customization.md)
(em inglês).

## O que você não pode desligar

De propósito, e esse é justamente o ponto da skill:

1. Risco primeiro. Sem abertura elogiosa.
2. Concordância precisa ser justificada: "sem motivo forte para discordar" exige nomear as checagens
   que tentaram derrubar a objeção, os riscos residuais e o pré-mortem.
3. O veredito só muda com evidência nova. Repetição, hierarquia e urgência não são evidência.
4. O pré-mortem sempre roda, em qualquer profundidade.
5. Toda afirmação sobre o seu contexto carrega uma fonte. Item sem fonte é rotulado como padrão do domínio.
6. Somente leitura. Ela nunca envia, posta, edita nem executa nada.
7. Nada de avaliar pessoas. Risco de capacidade é tratado como processo e dependência.
8. Quem decide é você. Ela fecha se oferecendo para ajudar a executar, inclusive quando você segue
   contra a recomendação.

Uma `house_rule` pode deixar a skill mais rígida. Uma que tente deixar ela mais mole é ignorada e
reportada.

## O que vem na caixa

```
ysat/
├── SKILL.md                        a skill em si
├── config.yaml                     suas configurações
├── README.md                       versão principal, em inglês
├── README.pt-BR.md                 este arquivo
├── LICENSE                         MIT
├── CHANGELOG.md
└── references/
    ├── risk-checklists.md          9 domínios mais os vieses que sustentam decisão ruim
    ├── customization.md            cada knob, cada override, e o que é travado
    └── examples.md                 três rodadas completas, incluindo uma que resiste à pressão
```

## Quando não usar

- Você quer uma análise equilibrada de prós e contras. Esta aqui defende o contra.
- A decisão já foi executada. Isso é pós-mortem.
- Você quer revisão de código linha a linha, ou o comunicado da decisão escrito para stakeholders.
- Você quer uma opinião sobre uma pessoa. Ela recusa e oferece análise de processo no lugar.

## Contribuindo

Issues e pull requests são bem-vindos, principalmente novos domínios de checklist e novos casos de
resistência a puxa-saquismo. Mantenha travado o que está travado: o único pull request que não será
aceito é o que deixa a skill mais fácil de concordar.

## Licença

MIT. Veja [LICENSE](LICENSE).
