# Interview Notes — Giovanni Faria Giraldeli

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md` (v2 — Project Deep-Dive is the primary segment, follow the 3-layer follow-up ladder). When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Résumé Summary (pre-filled from `cv.md` before the interview)

- Analytics Engineer / Data Engineer, Lisbon, currently at Dremio (lakehouse company) since May 2025: cut analytics infra monthly cost 65% via pipeline re-engineering/re-scaling, improved data governance, built ingestion/modeling pipelines. SQL/Python/dbt/Databricks/Snowflake/Spark/Airflow across the board.
- Prior: Analytics Engineer at Upwork (data modeling to raise payments take rate 1–2pp), Senior Data Analyst/Analytics Engineer at QuintoAndar (cut financing costs 30%, improved bill collection 67% by finding data quality gaps, ML-driven NPS and risk/eligibility clustering work), Product/Data Analyst at Itaú Unibanco (churn framework, A/B testing, CRM analytics).
- No rule-based detection, cybersecurity, or fraud/risk-analytics-system experience in the CV — same gap pattern as the previous candidate (Daniel Fogaça). No JavaScript mentioned. (Correction post-interview: his titles are consistently Analytics Engineer / Data Analyst / Data Engineer throughout, not an ML-centric background — ML shows up as one tool he's used within analytics work, e.g. his own description of using a Random Forest as "an investigative tool," not as his core focus. Earlier framing here overstated this.)
- Three strong real-project anchors to push on with the follow-up ladder tomorrow: the QuintoAndar bill-collection/data-quality-gap story (67% improvement), the Dremio 65% infra cost reduction, and the Upwork take-rate finding (currently vague on its own — "uncovered opportunities... through data modeling").
- Civil engineering background, self-taught/bootcamp path into data (not a CS degree), currently mid-Masters in Information Systems Management.
- See `cv.md` for full detail and the complete "Notable for this role" gap analysis.

## Segment 1 — Quick Introduction + Role Context (5 min)

- Candidate Introduction:
  - Comecou no Itau, identificar KPIs, melhorar produtos, em seguida tracava plano baseado nessa melhoria. Comecou a perceber que gostava mais dar parte tecnica, de analise mesmo, e dai foi migrando na direcao de Data Analyist, depois Analytics Engineer e eventualmente Data Engineer. Trabalhou com parte de segmentacao de clientes, comecou a ter contato com ML, clusterizacao de publico usando KMeans.
  - Quando fui pro Quinto Andar eu fui convidado por instrutor meu da Tera, uma escola de tecnologia, ele era diretor la. La eu trabalhava muito como analytics Engineer, fazia modelagem dos dados, transformacoes ate camada gold e a parte semantica pra colocar no looker pros analistas usarem. Analytics Engineer, basicmanete. Fiquei por um tempo por la, e fazia muito isso relacionado a seguro de imoveis, transacoes de imoveis ate que eu decidi mudar pra Portugual
  - Fazia trabalho de Analytics Engineer, previamos receita de cada cliente novo na Upwork e verificava quais traria mais receita. Eventualmente houve um layoff, muita gente saiu inclusive eu, e dai eu fui pra Dremio.
  - A dremio era uma oportunidade pra melhoria tecnica pra mim. A dreamio e um competidor da Snowflake, entao e um Lake as a Service. Tive bastante interface com a parte tecnica de como funciona uma data warehouse. Esta sendo muito bom pra mim, pra aprender assuntos mais profundos de como uma Data Warehouse funciona. 

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

Notes (background, story):

- He had a progressive carrer, began as Analyst at Bank, then Data Analyts, then Analytics Engineer and Data Engineer as well. He have a broad experience and he worked in big brazilian companies (Quinto Andar and Itau)

## Segment 2 — Project Deep-Dive (30–35 min): the primary signal

Project the candidate chose:

- Um dos meus primeiros projetos no quinto andar. Estavamos comecando a fazer o produto de compra e venda de imoveis, e a experiencia era muito pior
do que oustros produtos do quinto andar, como de aluguel. O objetivo era basicamente melhorar o NPS, sem muitas condicoes de contorno alem disso.
- A base transacional existia, mas nenhum modelo analitco pra entender o que estava acotnecendo. Entao eu identificando os touchpoints nos quais os clientes passavem e pra cada touch point eu colocava varias variaveis pra eu conseguir entender o que estava acontecendo. Entao eu precisava entender quais informacoes existiam, pra identificar quais variaveis poderiam identificar um passo especifico, pra eu conseguir identificar os passos. Eu utilizei essas variaveis pra um random forest, apliequei um feature importance pra analisar a importancia das variavies. Acabamos descobrindo que uma das variaveis dizia que clientes que comrpavam casa acima de 800 mil reais eles queriam tratamento premium, entao derrubava o NPS pra esses casos. Depois desse projeto pra tentar aumetnar o NPS. Era uma pergunta muito aberta e era dificil entender e nao havia muitos dados estruturados pra fazer isso. 

**Layer 1 — What exactly did you do?** (specific technique/tool/steps, not a summary)

Candidate's answer:

* Como voce extraiu os dados pra vc fazer o estudo.

- Na epoca usavamos databricks, era o nosso data warehouse, entao eu tinha facil acesso a notebooks pra treinar algoritmos nos notebooks, entao o que eu precisei fazer foi utilizar os dados. Nao precisei adicionar nenhuma feature, entao era mais organizar o que ja existia e todo o OLTP estava copiado pro Databricks, entao eu tinha que fazer nada de extracao ou coisa do tipo, outra equipe disponibilizava. Pra treinar o modelo, eu nao precisei fazer ETL ou nada assim, pq os dados ja estavam limpos e corretos. O que eu precisei tratar eu tratei dentro do notebook mesmo. 
- No notebook, como eu tratava os dados: maior parte das transfomacoes eram em SparkSQL, usando fu8ncionalidades do proprio Databricks. 
- O target do modelo era a resposta do NPS. O grao dos dados era cliente. Entao ele era um regressor e devolvia a nota do NPS. O modelo previa a nota do NSP do cliente, mas a flag que eu avaliava era se o modelo acertava a nota ou nao, entao era como um classificador. O modelo era um Random Forest, nao me lembro exatamente, mas provavelmente era um scikit learn. 
- A importancia dos features era um retorno do random forest, dado que o modelo esta bem treinado, entao eu usei o proprio feature importance do modelo. Dado que modelo estava prevendo a realidade, entao eu poderia confiar nesses resultados. 
- A entrega desse trabalho foi a analise em si, apresentei pro meu manager, explicamos a ideia pra outros stackholders, explicar a ideia para outros, e sugerimos alguns fatores de melhoria, como por exemplo colocar mais atencao no atendimento de imoveis com valor acima de 800k. A partir disso, criaram uma equipe primium pra cuidar desses casos maiores. Finalmente fizemos um dashboard pra acompanhar o resultado.

**Layer 2 — Why that way?** (trade-offs considered, alternatives ruled out)

Candidate's answer:

- Eu comecei considerando modelos que poderiam retornar feature importance, como Random Forest, XGBoost e outras alternativas. Eu escolhi o Random Forest pq o modelo era mais explicavel, entao comunicar a ideia com os stackholders era mais facil. Entao a decisao foi mais pautada nisso. Acabei usando ML como uma ferramenta ivestigavita. 

**Layer 3 — How did you know it worked?** (incl. direct probe: was success defined upfront, or did they have to figure that out themselves — the real-example test for ambiguity tolerance)

Candidate's answer:

- A partir das propostas que fizemos, que geraram mudancas na empresa, e do acompanhamento dos dashboards, a mlehora do NPS era clara. Esse processo nao foi imediato, a nova equipe precisou se construida, mudancas nos habitos e impacto nos clientes, entao levou coisa ai de uns 4 a 6 meses.

**Did the candidate sustain concrete depth through at least 2 of these 3 layers, or did answers stay at resume-bullet level even after follow-up?**

[X] Sustained real depth (2–3 layers)   [ ] Partial — needed heavy prompting   [ ] Did not sustain depth

Notes:

- Ele foi capaz de ir dando detalhes conforme eu perguntava, mas ele escolheu um projeto que ele fez ha mais de 3 anos. Entao nao estava muito fresco na cabeca, mas ainda asism ele conseguiu dar boas explicacoes e eu entendi exatamente o que ele fez e como. Me ajude analisando as respostas dele, mas me parece que ele deu os detalhes relevantes todos. Ele mencionou que essa era a experiencia dele que mais se encaixava na vaga, mas eu dei uma dica pra ele de que para o cliente, seria melhor usar como exemplo um caso mais recente na carreira. Nao precisa ser identica ao que a vaga pede, mas precisa ser complexo o suficiente pra que o cliente te faca perguntas complexas. Acho que ele da conta, mas vai precisar de uma historia mais cabeluda e mais recente pra impressionar o cliente. 

Needed a second project to find real depth? [X] No   [ ] Yes — note why: Nao, mas eu dei detalhes acima.

-

## Segment 3 — Technical Q&A (10–15 min, time-permitting — pick only 3–4 total)

### Python Reasoning

- [X] **Q:** You're looking at a Python function that loops through a list of detection rules, and for each rule, loops through every event in a dataset to check whether it matches. The dataset has millions of events and there are hundreds of rules. What's the performance concern, and how would you address it?
  > Expected: Nested loop (~rules × events) is slow at this scale; reasons toward vectorized/batch evaluation, and/or pre-filtering on key signals before running full rule logic.
  Candidate's answer: Probably it would take long time if I do one-by-one, like checking every row once for every rule with a single thread, on a Python loop. The easiset way I can think to fix that is to create multiple threads to inspect rules, like deploy multiple workers in the loop where each worker handles parts of the maching. That would scale faster. Or, in one call I could call all the events at once and pass the rule for all events in a single call, vectorization. I could filter events, select only what matters for the set of rules I have.  

- [ ] **Q:** If you inherited a Python codebase with no tests and needed to understand what a specific function actually does before changing it, what would you do beyond reading the code top to bottom?
  > Expected: Trace actual call sites, run it against known inputs, check version history/tickets, write characterization tests before changing anything.
  Candidate's answer:

### SQL on Large, Complex Datasets

- [X] **Q:** You have two queries, each returning the set of event IDs flagged by one detection rule. You need to measure the overlap between them — how many events both rules flagged, and how many only one of them flagged — against a table with tens of millions of rows per day. What would you think about when writing this, to avoid it running too slowly?
  > Expected: Filter/aggregate early, use partitioned/indexed columns, compare the two ID lists in a single query (join/INTERSECT) rather than pulling them out and comparing row by row in code, avoid accidental cross joins, check the query plan.
  Candidate's answer: It dependens if I am dealing with transactional or analytical db. If transaction, I could select only the columsn needed for that, create a temp view for that. Materialization is the key word is. Lets say it is transactional, I would filter the event table, so see which filters could be applied to help make more efficient. So I could have a query that returns the ids for each rule, after properly filtering the event table and selecting only necessary filters, that would be a subquery, one for each rule, then I would have IDs for each rule, at the end I can run a full outter join. 
> He struggled to understand that I was talking about two queries. So he was thinkg about creating a temp table for each rule and then solving the issue. But when I told him that it was two queries again, he mentioned he could do that on a single query. However, he did not mentioned index on the tables, avoiding accidental cross joins, checking the query plan, did not mention intersect.

### Rule-Logic Reasoning

- [ ] **Q:** You're reading someone else's code and find an `if` statement with a long condition — several `and`/`or` checks chained together, no parentheses, not nicely formatted. What would you do to make sure you actually understand what it evaluates to before relying on it?
  > Expected: Don't trust a quick left-to-right read — `and` binds tighter than `or` in most languages, so grouping can differ from intuition. Rewrite with explicit parentheses and/or test against concrete inputs, don't assume.
  Candidate's answer:

### Data-Flow & Root-Cause Investigation

- [ ] **Q:** You're told a specific detection rule has been producing more false positives lately, but nobody knows why, and you don't yet know how the data feeding it is produced. How would you start investigating?
  > Expected: Pin down what "false positive" means with examples, trace the rule's signals back to where they're computed, check for recent upstream changes, compare before/after, then form a specific testable hypothesis.
  Candidate's answer:

## Segment 4 — Task (10–15 min, time-permitting, lowest priority)

Used? [X] No — ran out of time   [ ] A — Rule Redundancy & Simplification   [ ] B — Live Root-Cause Investigation

Notes on approach:

-

Overall bar level for this segment (if used): [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:


## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Depth of real technical ownership under follow-up | High | 3           | |
| Ambiguity tolerance & independent ownership | High | 3           | |
| Data-flow / root-cause investigation | High | 3           | |
| Hypothesis-driven analysis | High | 2           | |
| Communication to non-technical stakeholders | High | 3           | |
| Python reasoning | Medium | 2           | |
| SQL on large, complex datasets | Medium | 2           | |
| Rule-logic & redundancy reasoning | Medium |             | Not assessed — the bank question wasn't asked and Segment 4 wasn't reached (ran out of time). |
| JavaScript familiarity | Low | 1           | Not directly asked — scored low by default since his CV never mentions JavaScript, not based on anything he actually said. |
| Snowflake / domain exposure | Low | 3           | |

## Decision Lean

[ ] Strong hire   [X] Borderline   [ ] No hire

Justification:

- He is a hire, but not a strong one. I think he can pass the client interview, but he needs a better story to tell.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
