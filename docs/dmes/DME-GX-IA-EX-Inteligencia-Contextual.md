<!-- Cópia de referência do DME oficial no Notion. Não editar decisões aqui sem atualizar a fonte canônica. -->

# DME — GX IA EX — Inteligência Contextual

> Fonte oficial e decisões vigentes: [Notion](https://app.notion.com/p/3f3e4cae41fb81769275d7ed5507b30f). Esta cópia foi sincronizada em 14/11/2026. O Notion permanece como fonte oficial; este arquivo é espelho versionado para consulta por ferramentas e agentes de desenvolvimento.

> **Consolidação documental — 14/11/2026.** Esta versão incorpora a revisão técnica/documental preparada em 14/11/2026, preservando decisões aprovadas e mantendo identificadas as propostas, pendências de fonte e decisões ainda dependentes do proprietário. A revisão separada permanece apenas como histórico de trabalho, não como documento oficial.

> **DECISÃO CANÔNICA DO PROPRIETÁRIO — IA:** GX IA EX é a única inteligência central apresentada ao usuário. O motor-ponte interno encaminha a solicitação à inteligência da web autorizada pela plataforma, prepara e preserva o contexto pertinente e devolve a resposta dentro do Gospel X EX. A plataforma administra internamente a conexão, os custos, a disponibilidade, as permissões e a privacidade. O usuário não precisa configurar infraestrutura de inteligência. Esta decisão está aprovada; a documentação, por si só, não comprova implementação técnica.

> **Ficha do documento** — Projeto: **Gospel X EX** · Tipo: DME da inteligência contextual central · Estado: decisão canônica aprovada pelo proprietário (escala do Hub §14: decisão aprovada); implementação técnica não verificada; provedor, custo e integração ainda não autorizados para execução · Depende de: Arquitetura de Motores (§12.1 e §13), Governança (§3 e §10), GX Analytics Engine (§16) e Monetização (custos e cotas comerciais) · Fonte única de: experiência GX IA EX, contexto funcional, limites de atuação e contrato funcional da inteligência · Não é fonte de: contrato técnico da ponte (Motores), autorização, egress, segredos, rotulagem e auditoria (Governança §10), mensuração (Analytics), preços e cotas (Monetização) · Revisão: 14/11/2026 **\[CDS-01\]**

## 1. Finalidade

Este DME define a camada única de inteligência contextual utilizada pelo Gospel X EX.

O objetivo é permitir que diferentes funcionalidades do produto utilizem inteligência sem criar uma integração, contratação ou API de inteligência independente para cada funcionalidade.

> **Uma infraestrutura de inteligência. Vários contextos.**

Cada funcionalidade fornece o contexto necessário para sua própria tarefa. O motor conduz esse contexto pela ponte interna até a inteligência da web autorizada, recebe a resposta e devolve o resultado à funcionalidade.

## 2. Princípio central

**CONTEXTO ENTRA → INTELIGÊNCIA RESPONDE → RESPOSTA SAI**

O produto determina:

- quem é o usuário;
- qual é a situação;
- quais dados podem entrar no contexto;
- quais permissões e limites existem;
- o que pode ser feito com o resultado.

O motor de inteligência organiza e conduz a solicitação. A inteligência interpreta e responde. A funcionalidade do Gospel X EX continua responsável pela apresentação e pela execução de qualquer ação autorizada.

> **SABE ≠ PODE.**
Saber uma informação no contexto não concede autoridade para executar uma ação.

#### 2.1 Regra complementar — o produto funciona sem IA externa

O Gospel X EX deve funcionar sem depender de IA externa para suas funções essenciais. A inteligência é complementar, controlada e economicamente protegida. Primeiro resolver sem IA sempre que possível (regra determinística ou dado interno autorizado). Não consultar a internet para responder o que já está disponível no conhecimento interno autorizado, salvo necessidade explicitamente aprovada. **\[CDS-02\]**

## 3. Fonte única e limites

Este DME é a fonte única de verdade para a experiência GX IA EX, seu contexto funcional, seus limites de atuação e a inteligência central do Gospel X EX. Cada aspecto relacionado tem um único dono: **\[CDS-03\]**

- contrato técnico da ponte interna, retry, DLQ e Circuit Breaker: DME — Arquitetura de Motores, Serviços e Inteligência (§12.1 e §13);
- autorização de solicitações de inteligência, classificação de dados, egress, segredos, identificação das saídas e auditoria: DME — Governança (§10);
- mensuração de uso, desempenho e custo: DME — GX Analytics Engine (§16);
- preços, planos e cotas comerciais: DME — Monetização.

A inteligência interpreta e responde dentro do contexto autorizado. Não ganha acesso autônomo ao banco, não amplia permissões e não executa ações no produto por conta própria.

## 4. Arquitetura funcional

**FUNCIONALIDADE DO GOSPEL X EX** → Contexto do Perfil → Situação Atual → Conversa/Pergunta → **GX IA EX** → Inteligência da web autorizada → **GX IA EX** → Resultado → **FUNCIONALIDADE DO GOSPEL X EX** → Usuário

O usuário percebe a inteligência como parte nativa do Gospel X EX. A inteligência externa utilizada é detalhe interno da infraestrutura, não uma nova experiência ou uma nova funcionalidade contratada pelo usuário.

## 5. Contexto obrigatório

Toda funcionalidade que utilizar inteligência deve fornecer um contexto próprio e suficiente para aquela tarefa.

O contexto pode conter, conforme autorização:

- identidade/contexto do usuário;
- perfil ou papel;
- situação atual;
- módulo ou funcionalidade;
- dados relevantes;
- histórico da interação;
- pergunta ou solicitação atual;
- regras e limites aplicáveis;
- permissões relevantes.

A funcionalidade propõe e declara o contexto necessário para a sua tarefa. O que pode efetivamente ser usado e sair do ecossistema é classificado e autorizado conforme a Governança (§10.1 e §10.2), minimizado pela ponte interna e validado no servidor. Permissão para usar o GX IA EX não significa permissão para enviar todo o contexto à inteligência. **\[CDS-04\]**

O motor não deve descobrir dados adicionais por conta própria.

O histórico da interação é somente o que a funcionalidade fornece para aquela solicitação. Onde, por quanto tempo e para qual finalidade o histórico de conversa é conservado ainda não está definido; enquanto isso, aplica-se a política geral de retenção da Governança (§9.2). **\[PD-01\]**

## 6. Regra para novas funcionalidades

Sempre que um desenvolvedor criar uma funcionalidade do Gospel X EX que necessite de inteligência, ele deverá:

1. identificar a finalidade da inteligência;
2. definir a situação em que ela será utilizada;
3. definir o contexto necessário;
4. definir quais dados podem entrar;
5. definir os limites e permissões;
6. entregar esse contexto ao motor central.

**É proibido criar uma nova integração independente de inteligência apenas porque uma nova funcionalidade precisa de IA.** A nova funcionalidade usa o motor existente.

#### 6.1 Ficha mínima do DME consumidor **\[CTP-01\]**

Proposta de modelo, para que a regra acima seja verificável. O DME de cada funcionalidade consumidora registra:

1. finalidade e situação de uso;
2. contexto necessário e campos que o compõem;
3. classificação de cada campo (público, privado, sensível, de terceiros) e se pode sair da infraestrutura, conforme decisão da Governança (§10.2);
4. perfis, vínculos e permissões que podem acionar;
5. resultado esperado e como é identificado (gerado, sugerido, calculado ou observado);
6. se há pesquisa externa e qual autorização a sustenta (§21);
7. alternativa determinística ou degradação segura;
8. estado na escala do Hub (§14) e feature flag;
9. custo estimado e limite, conforme a ficha de motores (Motores §5 e §12).

## 7. Componentes do motor

Descrição funcional. O contrato técnico da ponte está no DME — Arquitetura de Motores, §13. **\[CDS-03\]**

#### 7.1 Coletor

Recebe o pacote preparado pela funcionalidade: contexto, situação, conversa e pergunta. Não consulta o produto por conta própria.

#### 7.2 Montador

Organiza o pacote para a inteligência. Não cria fatos, permissões ou dados que não tenham sido fornecidos pelo produto.

#### 7.3 Conector

É a ponte central entre o Gospel X EX e a inteligência. Responsabilidades:

- receber a solicitação montada;
- encaminhar a solicitação pela conexão interna autorizada;
- receber a resposta;
- devolver o resultado ao motor.

A ponte utiliza a conexão de inteligência da web autorizada e administrada internamente pela plataforma. A configuração técnica não é uma escolha apresentada ao usuário.

#### 7.4 Retorno/Apresentação

Recebe a resposta e a devolve à funcionalidade consumidora. A funcionalidade decide como apresentar ou utilizar o resultado dentro das regras do Gospel X EX.

A saída deve ser identificada como gerada ou sugerida e, quando aplicável, informar origem, mecanismo, versão e limitações (Governança §10.4; Motores §13.4). **\[CDS-05\]**

## 8. Mecanismo de conexão

O motor mantém um único ponto central de conexão interna com a inteligência da web autorizada. A plataforma administra a conexão e a interface não apresenta alternativas técnicas ao usuário.

> **A funcionalidade não conhece nem contrata diretamente a inteligência. Ela chama o motor central.**

O mecanismo de conexão permanece desacoplado da funcionalidade consumidora. Provedor, modelo e custo-teto ainda não foram definidos. **\[PD-05\]**

## 9. Indisponibilidade e degradação segura

Se a conexão interna autorizada estiver indisponível, retornar erro ou não houver autorização ou classificação suficiente para aquele contexto, o GX IA EX aplica degradação segura: **\[CDS-06\]**

- informa de forma clara que a capacidade não está disponível naquele momento (ou não está autorizada para aquele contexto), sem expor configuração de provedor e sem pedir ao usuário que configure a conexão;
- usa alternativa determinística quando existir;
- registra o evento técnico permitido;
- evita repetição infinita: tentativas controladas, limite padrão de três, backoff e idempotência; esgotado o limite, o evento segue para a Dead-Letter Queue (Motores §12.1);
- quando volume, latência, erro ou custo ultrapassarem os limites cadastrados, o Circuit Breaker (fechado, aberto ou semiaberto) rebaixa ou pausa o recurso e gera alerta;
- não gera custo externo não autorizado.

## 10. Custos e limites

A arquitetura foi definida para evitar o modelo em que cada funcionalidade possui sua própria API de inteligência.

Não haverá:

- uma API de IA por tela;
- uma contratação por funcionalidade;
- uma chave independente para cada recurso;
- consumo pago criado automaticamente por uma funcionalidade;
- cobrança ao usuário apenas porque uma funcionalidade utilizou o motor.

O motor-ponte utiliza internamente a inteligência externa autorizada e configurada para o Gospel X EX, preservando o contexto pertinente. Não existe seletor nem opção para o usuário trocar entre várias inteligências.

Qualquer capacidade paga, contratação ou aumento relevante de custo exige decisão e autorização próprias.

A IA não decide sozinha gerar consumo pago: a geração de custo externo passa por política explícita, autorização, cota e Circuit Breaker (Motores §13.3). Cotas comerciais e planos pertencem à Monetização; este DME não cria cobrança por IA (Analytics §17). **\[CDS-07\]**

Proteções contra abuso (rate limiting, Governança §9.3) e o Circuit Breaker não são cotas comerciais. Se haverá, ou não, limite de uso por usuário para a GX IA EX está em aberto. **\[PD-02\]**

## 11. Isolamento e segurança

O motor trabalha somente com o pacote de contexto recebido naquela solicitação.

Ele não deve:

- acessar livremente o banco do Gospel X EX;
- buscar dados de outros usuários;
- cruzar perfis por iniciativa própria;
- ampliar permissões;
- transformar informação privada em pública;
- executar ações no produto;
- decidir sozinho uma ação sensível.

O produto permanece responsável por autenticação, autorização, isolamento de dados e execução das ações.

As credenciais técnicas da ponte ficam apenas em ambiente server-side seguro, nunca no aplicativo, no navegador, no banco em texto aberto, em logs ou em parâmetros expostos ao cliente (Governança §10.3). O padrão é negar processamento externo quando classificação ou autorização forem insuficientes; RLS, autorização server-side, audience/capability do token e políticas de egress continuam válidos durante preparação, processamento, retorno, armazenamento e exportação (Governança §10.4). **\[CDS-08\]**

## 12. Inteligência como componente, não como autoridade

A inteligência: interpreta; analisa; organiza; sugere; responde.

A inteligência não: concede permissões; publica por conta própria; cria vínculos; altera dados sensíveis; realiza cobrança; decide políticas; substitui a Governança; substitui regras determinísticas; substitui decisão humana quando esta for exigida; aplica punições relevantes, suspende usuários ou remove conteúdo por conta própria. **\[CDS-09\]**

A inteligência pode auxiliar na organização, classificação e sinalização de prioridade de denúncias; a decisão de moderação permanece humana e autorizada (decisão registrada na OBS-27). **\[CDS-09\]**

## 13. Observabilidade, Analytics e auditoria

O uso do motor deve produzir sinais técnicos e operacionais suficientes para o **GX Analytics Engine**, respeitando privacidade e minimização.

Podem ser registrados, conforme necessidade:

- identificação da solicitação;
- funcionalidade consumidora;
- situação/contexto técnico;
- inteligência utilizada;
- sucesso ou falha;
- latência;
- tipo de erro;
- degradação segura aplicada;
- volume de utilização;
- qualidade/resultado quando houver métrica autorizada;
- consumo estimado quando disponível.

Não registrar desnecessariamente o conteúdo privado completo do usuário, contexto sensível ou conversa. Prompts, respostas completas, credenciais e contexto sensível não entram em métricas por padrão (Analytics §16). **\[CDS-10\]**

O bloqueio do acesso funcional dos perfis ao Analytics (Analytics §17; OBS-25) não desativa a observabilidade técnica interna da ponte, desde que respeitadas finalidade, permissões, minimização, retenção e autorização da Governança. **\[CDS-10\]**

Métricas (Analytics) e auditoria (Governança §10.4) são coisas distintas. A Governança exige que prompts, contexto enviado, versão do contrato, decisão de encaminhamento e resultado de autorização sejam auditáveis, sem segredos e sem dados além do necessário; a forma de conciliar isso com a regra de não armazenar prompts em métricas ainda não está definida. **\[PD-03\]**

O objetivo é permitir que o Gospel X EX saiba: onde a inteligência está sendo usada; quais funcionalidades têm maior demanda; onde há falhas; quando uma inteligência está indisponível; quais contextos produzem melhores resultados; onde existem oportunidades de melhoria.

## 14. Experiência do usuário

A inteligência deve funcionar como capacidade nativa do Gospel X EX.

O usuário não precisa: escolher uma API; cadastrar chave; contratar provedor; entender qual mecanismo técnico está sendo utilizado; repetir uma solicitação porque uma inteligência específica ficou indisponível.

A seleção ou eventual substituição do provedor é uma decisão interna de arquitetura e Governança, sujeita a autorização, segurança, privacidade, custo e continuidade do serviço.

O ponto de acesso do usuário ao GX IA EX na interface (por exemplo, um item global do GX Orbital, citado em Orbital §3 como “se e quando existir e for autorizado”) não está definido neste DME. Enquanto não for decidido, nenhuma superfície pode presumir esse acesso, e a disponibilidade para um perfil depende de Governança, plano e feature flag. **\[PD-04\]**

## 15. Relação com os DMEs das funcionalidades

Este DME não substitui os DMEs de perfil ou funcionalidade. Cada DME consumidor define: por que precisa de inteligência; em qual situação; qual contexto fornece; quais resultados espera; quais limites existem (ficha mínima em 6.1).

Este DME define **como o produto disponibiliza a capacidade de inteligência de forma transversal**.

Exemplo: o **GX Busca & Descoberta** (que inclui o Tube Gospel como fonte especializada) pode fornecer um contexto específico para classificação de intenção e organização de descoberta. Outra funcionalidade pode fornecer outro contexto. Ambas utilizam o mesmo motor. **\[CDS-11\]**

Consumidores identificados na documentação atual: **\[CTP-02\]**

| Consumidor | Uso previsto | Fonte da regra | Situação documental |
| --- | --- | --- | --- |
| GX Busca & Descoberta (inclui Tube Gospel) | IA auxilia a classificar intenção; organiza descoberta externa | Busca §6, §9, §21; este DME §21 | Documentado |
| Contexto de Gestão/Governança | Pesquisa externa de apoio à administração | Este DME §21.2 | Finalidades não definidas \[PF-01\] |
| Apoio à triagem de denúncias | Organização, classificação e prioridade, sem decisão | OBS-27 | Decisão aprovada; Governança §11 ainda não alinhada |
| GX Orbital | Não é consumidor; pode oferecer o ponto de acesso | Orbital §3 | Ver \[PD-04\] |

## 16. Fora do escopo

Não fazem parte deste DME: agente autônomo; braços e dedos do GX IA original; automação autônoma de tarefas externas; investigação livre na internet; acesso autônomo ao banco; memória permanente universal; múltiplos agentes; criação automática de novas integrações; uma API diferente para cada funcionalidade; roteamento complexo sem necessidade; infraestrutura distribuída prematura; contratação automática de serviços pagos.

## 17. Contrato simplificado

**ENTRADA:** contexto autorizado + situação + conversa/pergunta.

**PROCESSAMENTO:** coleta → montagem → conexão → inteligência → retorno.

**SAÍDA:** resposta da inteligência → funcionalidade consumidora.

**AUTORIDADE:** permanece no Gospel X EX.

## 18. Critério de conclusão

Este DME estará funcional quando uma funcionalidade do Gospel X EX puder:

1. preparar seu próprio contexto;
2. enviar esse contexto ao motor central;
3. o motor encaminhar a solicitação pela ponte interna autorizada;
4. receber a resposta;
5. devolver a resposta à funcionalidade;
6. permitir apresentação ao usuário;
7. registrar os sinais técnicos necessários;
8. aplicar degradação segura quando a conexão estiver indisponível.

Sem criar uma nova API de inteligência para essa funcionalidade.

Critérios adicionais verificáveis (derivados de Governança §10, Motores §13.6 e Analytics §16): **\[CTP-03\]**

9. com classificação ou autorização insuficientes, o processamento externo é negado e a funcionalidade recebe degradação segura;
10. a saída chega identificada como gerada ou sugerida;
11. nenhuma credencial da ponte existe no cliente, em logs ou no banco em texto aberto;
12. testes demonstram que uma solicitação não recebe dados de outro usuário, igreja ou escopo;
13. nenhum prompt ou resposta completa aparece em métricas.

## 19. Princípios fechados

1. **Uma infraestrutura de inteligência, vários contextos.**
2. **Cada funcionalidade fornece seu próprio contexto.**
3. **Nenhuma funcionalidade contrata sua própria inteligência.**
4. **O motor central é a ponte.**
5. **A conexão técnica é administrada internamente pela plataforma.**
6. **A indisponibilidade deve resultar em degradação segura.**
7. **O usuário não precisa conhecer configurações internas.**
8. **O produto continua sendo a fonte de verdade.**
9. **SABE ≠ PODE.**
10. **A inteligência não ganha autoridade.**
11. **Os braços e dedos do GX IA original não fazem parte deste DME.**
12. **O objetivo é manter o custo operacional controlado sem limitar a experimentação do usuário.** (ver \[PD-02\])
13. **Novas funcionalidades devem reutilizar o motor, nunca reconstruí-lo.**
14. **O produto funciona sem IA externa nas suas funções essenciais; primeiro resolver sem IA.** **\[CDS-12\]**

## 20. Decisão

**APROVADO CONCEITUALMENTE PARA O GOSPEL X EX** (na escala do Hub §14: decisão aprovada; implementação não verificada). **\[CDS-12\]**

Este DME passa a ser a referência para a camada de inteligência contextual do projeto.

Qualquer alteração que introduza nova API por funcionalidade, nova forma de contratação de inteligência, autonomia de execução, ampliação de autoridade ou mudança estrutural relevante deve retornar para decisão antes da implementação.

> **Contexto entra → inteligência responde → resposta sai.**

## 21. Regra de acesso externo por contexto

A inteligência do Gospel X EX **não possui autorização geral para pesquisar fora do ecossistema**.

Para o usuário comum, a regra padrão é: a inteligência trabalha dentro do contexto fornecido pela funcionalidade; não realiza pesquisa externa por iniciativa própria; não amplia o contexto recebido; não transforma a capacidade de conexão do motor em autonomia de navegação.

Uma pesquisa externa somente poderá ocorrer quando o **DME da própria funcionalidade** definir explicitamente essa necessidade e a Governança autorizar o respectivo contexto.

#### 21.1 Exemplo — GX Busca & Descoberta (Tube Gospel)

O **GX Busca & Descoberta**, com o **Tube Gospel** como fonte especializada de YouTube, possui finalidade própria de descoberta e pesquisa externa de conteúdo. A pesquisa externa não representa autonomia da inteligência: é uma **capacidade da funcionalidade**, definida no DME de Busca (§6, §9, §11 e §21) e autorizada pelo seu contexto. O GX IA EX apenas conduz a solicitação e organiza o retorno. Consulta fora do propósito do Gospel X ou de alto risco não é encaminhada para fontes externas (Busca §6.1). **\[CDS-13\]**

#### 21.2 Contexto de Gestão e Governança

O contexto de **Gestão/Governança do Gospel X EX** poderá possuir permissões especiais para utilizar inteligência em pesquisas externas destinadas ao apoio da administração do ecossistema. Essa capacidade: é exclusiva do contexto administrativo autorizado; deve possuir finalidade definida; deve respeitar as permissões do gestor; deve registrar os sinais operacionais necessários; não concede autonomia geral ao motor; não altera a autoridade da Governança; não se aplica automaticamente aos usuários comuns.

Nenhuma finalidade concreta está definida na documentação lida. Até que seja definida, essa capacidade não pode ser implementada. **\[PF-01\]**

#### 21.3 Regra arquitetural

**EXTERNO NÃO É PADRÃO. CONTEXTO E AUTORIZAÇÃO DEFINEM.**

- **Usuário comum:** contexto da funcionalidade → inteligência → resposta.
- **Funcionalidade autorizada a pesquisar externamente:** contexto + autorização específica → GX IA EX → mecanismo externo autorizado → resultado → funcionalidade.
- **Gestão/Governança:** contexto administrativo autorizado → GX IA EX → pesquisa externa permitida para a finalidade definida → resultado → gestão.

A existência de um conector capaz de alcançar uma inteligência ou serviço externo **não significa que todas as funcionalidades ou usuários possam utilizá-lo livremente**. Essa distinção é obrigatória para preservar a experiência nativa, a segurança, o controle de custos e a separação entre inteligência contextual e autonomia externa.

#### 21.4 Resultado externo não vira entidade Gospel X

A IA organiza e apresenta resultados externos identificando origem e data da consulta quando disponível; indica incerteza quando não conseguir confirmar; e nunca apresenta um resultado externo como Perfil Igreja cadastrado, entidade verificada ou vínculo institucional (Busca §9 e §10; OBS-19). **\[CDS-13\]**

## 22. Pendências abertas deste DME

\[PD-01\] retenção do histórico de conversa · \[PD-02\] limite de uso por usuário · \[PD-03\] auditoria de prompts × métricas · \[PD-04\] ponto de acesso na interface · \[PD-05\] provedor, modelo e custo-teto · \[PF-01\] finalidades do contexto de Gestão. Detalhes nas Partes 4 e 5. **\[CDS-12\]**
