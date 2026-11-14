<!-- Cópia de referência do DME oficial no Notion. Não editar decisões aqui sem atualizar a fonte canônica. -->

# DME — GX Orbital

> Fonte oficial e decisões vigentes: [Notion](https://app.notion.com/p/3e5e4cae41fb81db9186f31b68f2407b). Esta cópia foi sincronizada em 14/11/2026. O Notion permanece como fonte oficial; este arquivo é espelho versionado para consulta por ferramentas e agentes de desenvolvimento.

> **Consolidação documental — 14/11/2026.** Esta versão incorpora a revisão técnica/documental preparada em 14/11/2026, preservando decisões aprovadas e mantendo identificadas as propostas, pendências de fonte e decisões ainda dependentes do proprietário. A revisão separada permanece apenas como histórico de trabalho, não como documento oficial.

> **Ficha do documento** — Tipo: DME transversal de experiência (camada de navegação da plataforma) · Estado: contrato funcional com decisões aprovadas pelo proprietário (§14); regras marcadas “a validar no protótipo” em arquitetura estudada; valores da consolidação técnica são referência de engenharia; **nada implementado** · Depende de: Governança (contexto e permissões, §3), DME — Vídeos Curtos (feed, §6.3), DMEs de perfil (destinos por contexto) e GX IA EX (somente se houver ponto de acesso aprovado) · Fonte única de: comportamento, estados, geometria, gestos, critérios de aceite e contrato dos itens do botão orbital GX · Revisão estrutural: 14/11/2026 (proposta; a página original foi editada pela última vez em 31/10/2026) **\[CDS-01\]**

## 1. Finalidade

O **GX Orbital** (“Um Dedo, Um Centro”) é um componente transversal de experiência e navegação contextual do Gospel X: um botão flutuante no centro inferior da tela, acionado com o polegar pela mesma mão que segura o celular, que abre um carrossel orbital contínuo de opções. Ele não é a navegação principal universal nem precisa conter toda a navegação do Gospel X. Seu conteúdo varia conforme tela, contexto, perfil, estado e permissões.

É um recurso transversal do próprio Gospel X, comum aos contextos autorizados. Não pertence exclusivamente a nenhum perfil. Cada contexto fornece a lista de destinos compatíveis com a tela, as permissões e a situação atual.

Ele facilita o uso com uma mão, mas **não substitui** os toques no resto da tela: algumas ações continuarão sendo feitas tocando em outras áreas.

Este documento existe para que o componente seja construído sem margem de interpretação. Tentativas anteriores deram errado porque o comportamento não estava fechado; por isso ele traz regras testáveis (seção 8) e uma lista de erros já conhecidos (seção 9).

## 2. Como funciona

1. **Fechado:** o botão GX fica fixo no centro inferior da tela.
2. **Toque no botão:** abre um anel de opções tipo relógio ao redor do botão. O botão continua no mesmo lugar, no centro do anel, e vira um X.
3. **Girar:** arrastar o dedo em volta do botão gira o anel, no sentido horário ou anti-horário, sem fim. As opções que saem da área visível por um lado reaparecem pelo outro, como os números de um relógio.
4. **Escolher:** um toque parado (sem arrastar) numa opção fecha o anel e executa a opção, seja trocar de contexto/tela ou acionar uma função. Todo item selecionado fecha o orbital antes da ação; não existe `keepOpen` no contrato inicial.
5. **Fechar sem escolher:** tocar no X, tocar no fundo escurecido, tecla Escape (web e teclado) ou botão voltar do sistema.
6. Tudo isso deve ser possível apenas com o polegar.

## 3. O que o anel pode conter

- **Navegação:** ir para outra tela ou módulo disponível no contexto atual.
- **Controles da tela atual:** ações sólidas sobre o que está aberto naquela tela, por exemplo pausar ou retomar uma mídia.
- **Funções globais:** a busca e, se e quando existir ponto de acesso aprovado e autorização, o GX IA EX.
  - A busca do anel é uma porta para a **Busca & Descoberta universal**; o contexto pode influenciar a relevância, mas não restringe a busca nem amplia permissões, e o resultado só mostra o que a pessoa pode descobrir ou visualizar. **\[CDS-03\]**
  - O item de IA não é garantido a todos os perfis: depende de Governança, plano e feature flag, e o ponto de acesso ao GX IA EX ainda não foi decidido no DME GX IA EX §14. **\[CDS-02\]**
- **Ações sociais sobre conteúdo:** curtir, comentar, seguir e compartilhar ficam, por padrão, na própria tela e não entram no anel. A exceção é Vídeos Curtos: ali o anel traz as ações do vídeo em foco (seção 4.7), como porta adicional ao feed, sem substituir os controles da tela. Exemplo: o Perfil Igreja não tem “curtir vídeo” no anel.

A lista é sempre contextual: cada tela e cada perfil oferece só o que faz sentido ali e o que a pessoa pode acessar.

## 4. Contrato de comportamento (vale para qualquer tecnologia)

Nomes de variáveis, propriedades de CSS e eventos citados abaixo são **referência da implementação web**; o que vale para qualquer tecnologia são os comportamentos e os valores (6 px, 180 ms, 44 px, 48 px, ±π). **\[CDS-04\]**

#### 4.1 Posição

- Botão fixo no centro inferior da tela, **acima da área de gestos do sistema** (barra de início do iPhone, barra de gestos ou botões do Android) e das áreas seguras. O valor de 16 px do componente web de referência não basta em celulares com essas áreas.
- Regra proposta, a validar em aparelho real: distância da borda inferior = maior valor entre a área segura inferior e a área de gestos do sistema, mais a margem de 16 px da referência. **\[CTP-01\]**
- O anel é concêntrico com o botão. O botão não se move quando o anel abre.
- Como o botão fica no rodapé, **boa parte da metade inferior do anel fica fora da tela ou sob a área de gestos**. Girar traz essas opções para a área visível. Quanto da metade inferior aparece de fato depende da distância do botão à borda e precisa ser medido em aparelho; o texto anterior dizia que a metade inferior “fica fora da tela”, o que é inconsistente com a atenuação aplicada a itens abaixo do centro (seção 4.3). **\[PF-01\]**

#### 4.2 Itens e rotação

- O carrossel orbital aceita qualquer quantidade total de itens; não existe limite estrutural fixo de opções. A referência inicial de UX é **5 opções visíveis simultaneamente** em Web e Mobile, sem transformar 5 em limite estrutural.
- Raio R dos centros dos itens: 86 px no mobile, 96 px no desktop.
- Texto vigente, geometria com distribuição uniforme pelo total de itens:
  - ângulo base: `baseAngle = Math.PI - index * (2 * Math.PI / totalItens)`;
  - ângulo no frame: `currentAngle = baseAngle + rotationAngle`;
  - coordenadas: `posX = R * Math.cos(currentAngle)` e `posY = -R * Math.sin(currentAngle)`.
- A rotação é contínua e sem limite: os ângulos são sempre tratados de forma circular, sem salto ao passar de 180° para -180°.
- Sem inércia e sem encaixe: o anel gira seguindo o dedo continuamente enquanto ele se move (“inércia angular contínua” no material de referência significa isso, não física de inércia) e para exatamente onde o dedo soltou, sem desaceleração nem encaixe automático no item mais próximo.
- Capacidade simultânea: o alvo de toque mínimo é 44 × 44 px e a distância entre centros deve ser de pelo menos 48 px. Na fórmula `2 · R · sen(π / N) ≥ 48`, **N representa somente a quantidade de itens simultaneamente visíveis no anel**, não o total de opções cadastradas no contexto. O total continua sem limite estrutural fixo; itens podem sair e entrar do campo visível conforme a rotação.

**Conflito identificado, pendente de decisão do proprietário (OBS-30d). \[PD-01\]** O texto vigente distribui os itens uniformemente pelo **total** (360°/N), mas a fórmula de conforto trata N como itens **visíveis**. Contas feitas com a própria fórmula: **\[CTP-02\]**

- com R = 86 px cabem no máximo 11 itens sem sobreposição (2·86·sen(π/11) ≈ 48,4 px); com 12 já não cabem (≈ 44,5 px);
- com R = 96 px cabem no máximo 12 (≈ 49,7 px); com 13 não (≈ 45,9 px);
- com poucos itens a quantidade visível cai abaixo de 5. Com 7 itens há cerca de 51° entre eles, e a metade superior do anel mostra de 3 a 4.

Isso contradiz “sem limite estrutural fixo” e “5 visíveis”. Duas saídas:

- **Opção A, recomendada: passo angular fixo.** O ângulo entre itens consecutivos é fixo (proposta θ₀ = 36°), independente do total, e `baseAngle = Math.PI - index * θ₀`. A cada instante só são desenhados os itens cuja posição cai na janela visível (cerca de 180° com folga); os demais entram ao girar, e a lista é tratada de forma circular (o último é seguido do primeiro), o que preserva “reaparecem pelo outro lado”. Com θ₀ = 36° a distância entre centros é cerca de 53 px (R = 86) e 59 px (R = 96), acima de 48 px, e a metade superior comporta 5 itens, coerente com a referência. O comportamento para 1 a 5 itens (centralização do grupo) precisa ser definido no protótipo. **\[PF-01\]**
- **Opção B: limite total.** Manter a distribuição uniforme e fixar um máximo (por exemplo, 11 itens por contexto). É simples, mas contradiz a decisão fechada de “não há limite estrutural fixo de opções totais” e só vale se o proprietário a reabrir.

#### 4.3 Visibilidade

- **Atenuação da metade inferior:** se `posY > 25 px`, o item usa opacidade 0,45 e escala 0,9. Nos demais, opacidade 1 e escala 1.
- **Regra proposta, a validar no protótipo:** item cujo centro está fora da área visível, ou sobre a área de gestos do sistema, não é desenhado como tocável e fica oculto para leitores de tela até girar de volta. Os critérios de aceite 13 e 8 (segunda frase) dependem desta regra e de 4.5, ainda não validadas. **\[CDS-05\]**

#### 4.4 Arrastar e tocar

- **Arrastar gira a roda. Arrastar nunca seleciona um item**, mesmo que o dedo termine sobre um item.
- O arraste pode começar em qualquer ponto do anel, inclusive sobre um item. O botão central só abre e fecha.
- O ângulo do dedo é calculado em relação ao **centro do botão**: `Math.atan2(-(clientY - centerY), clientX - centerX)`.
- A rotação acumula a **diferença entre dois ângulos consecutivos** do dedo. Cada diferença deve ser normalizada: se for maior que π, subtrai-se 2π; se for menor que -π, soma-se 2π. É isso que evita o salto brusco ao cruzar ±π. Não se deve rotacionar pela distância em pixels percorrida.
- A distância é medida com `Math.hypot(dx, dy)`. Acima de **6 px**, marca-se `hasMoved = true`.
- Ao soltar, se `hasMoved === true`, nenhum clique é aceito por cerca de 180 ms (`preventClickUntilRef = Date.now() + 180`).
- Só um **toque parado** (movimento de até 6 px) seleciona o item, dispara a ação e fecha o anel, **uma única vez**.
- O gesto precisa continuar sendo rastreado mesmo quando o dedo sai da área do botão ou do anel.
- Se o sistema cancelar o gesto (por exemplo, `pointercancel` na web ou interrupção do reconhecedor nativo), o gesto termina com segurança, sem selecionar item e sem deixar o anel em estado inconsistente. **\[CTP-03\]**
- **A área do anel deve travar a rolagem da página durante o arraste** (`touch-action: none` na referência web), para o navegador não tentar rolar a tela enquanto a pessoa gira o anel.

#### 4.5 Fechamento

- Tocar em um item (toque parado), no X ou no fundo fecha o anel. Tocar no fundo **não** deve repassar o toque para a tela de baixo.
- Escape fecha em web e teclado. O botão voltar do sistema fecha o anel antes de navegar (regra proposta, a validar no protótipo). **\[CDS-05\]**
- Ao tocar em um item, o orbital **sempre fecha** e depois executa a ação. Não há `keepOpen` no contrato inicial.
- Com o teclado virtual aberto ou foco em campo editável, o anel aberto fecha para não cobrir o campo nem o teclado (consolidação técnica). **\[CTP-03\]**

#### 4.6 Teclado (web)

- Escape fecha o menu aberto.
- **Seta direita gira no sentido horário e seta esquerda no anti-horário**, em passos de `Math.PI / 8`. (O texto original dizia que as duas giravam do mesmo jeito, o que estava errado.)

#### 4.7 Contexto subjacente, mídia e navegação

- Abrir ou fechar o GX Orbital **não bloqueia nem interrompe automaticamente o contexto atual**. O conteúdo subjacente continua funcionando normalmente, salvo quando a própria ação escolhida exigir outro comportamento.
- A mídia só pausa se o item escolhido for uma ação de pausa ou se o módulo responsável determinar isso.
- Uma seleção pode executar uma ação mantendo o contexto atual ou iniciar uma navegação para outro contexto.
- Quando a seleção inicia uma navegação, o novo contexto passa a determinar o conjunto de opções do GX Orbital. O orbital é recalculado conforme tela, contexto, perfil, estado e permissões.
- **Vídeos Curtos — acionamento do botão do GX Orbital:** o botão do GX Orbital fica disponível na tela de Vídeos Curtos para uso com uma mão. No Mobile, um toque rápido passa para o próximo vídeo; pressionar e segurar por aproximadamente 1–2 segundos abre o GX Orbital. Na Web, um clique simples passa para o próximo vídeo; um duplo clique abre o GX Orbital. O anel aberto oferece as ações do vídeo em foco (curtir, comentar, seguir, compartilhar, salvar, denunciar e ver informações) e pode ser girado para mostrar mais opções. A escolha segue o contrato geral: depois de soltar o dedo, um toque parado no item fecha o anel e executa a ação uma vez; arrastar continua apenas girando o anel. Escolher uma ação que leva a outra tela (por exemplo, comentar) navega para essa tela, e o anel passa a oferecer as opções do novo contexto. Os gestos normais do feed (Vídeos Curtos §6.3) continuam valendo: o Orbital é uma porta adicional e não duplica função, regra ou permissão. Esse comportamento é específico de Vídeos Curtos e não altera o contrato geral do Orbital.
  - Com o anel aberto, o fundo captura todos os gestos: nenhum gesto do feed (swipe, toque simples, toque duplo) chega ao vídeo por baixo (seções 4.5 e critério 18). **\[CDS-05\]**
  - Duas ambiguidades da decisão de 30/10/2026 precisam de definição antes do protótipo: na Web, o primeiro clique de um duplo clique já dispara “próximo vídeo”; no Mobile, “aproximadamente 1–2 segundos” é uma faixa, não um valor, e falta o limite entre “toque rápido” e “pressionar e segurar” e o feedback durante a espera. **\[PD-03\]**
  - A lista de ações do vídeo não inclui “bloquear”, que Vídeos Curtos §8 prevê quando disponível no contexto. **\[PD-04\]**
- Exemplo: no Perfil Igreja, **Oração** pode navegar para o contexto de oração **para quem possui vínculo ativo com aquela igreja**; sem vínculo ativo, o pedido é bloqueado antes do envio (OBS-03; Governança §4). Se o item aparece ou não para quem não tem vínculo é uma decisão em aberto, porque o §11 diz que um perfil nunca vê destino que não pode acessar, enquanto o fluxo de vínculo orienta o usuário a solicitar vínculo. **\[CDS-07\] \[PD-02\]**
- Exemplo (ilustrativo): em Meu Espaço Artístico, no Perfil Artista — Cantor com classificação Profissional / Consolidado, **Análises** poderia navegar para o contexto analítico. **Enquanto o GX Analytics Engine estiver bloqueado para todos os perfis (OBS-25; Analytics §17), esse item não existe.** A expressão “Perfil Cantor Profissional” foi substituída pela terminologia vigente. **\[CDS-06\]**

## 5. Estados

- **Fechado:** só o botão aparece. Toque: abre.
- **Abrindo:** transição curta. Toques ignorados até terminar.
- **Aberto:** anel e fundo visíveis. Toque no botão ou no fundo fecha. Toque parado em item seleciona. Começar a arrastar leva a Arrastando.
- **Arrastando:** o anel gira acompanhando o dedo. Nenhum item é selecionado.
- **Escolhendo:** um item foi tocado. O anel fecha e a ação executa uma vez.
- **Fechando:** transição curta. Depois disso, volta a Fechado.

## 6. Especificação visual

- **Botão central:** 56 a 64 px (`w-14 h-14 sm:w-16 sm:h-16 rounded-full`), borda dourada/âmbar, gradiente escuro profundo, halo suave e pulso luminoso controlado. Acima de tudo (`z-50` na referência web).
- **Fechado:** logo ou monograma GX e indicador de notificação em ping pulsante quando houver.
- **Aberto:** transição suave, giro de cerca de 90° e ícone de fechar (X).
- **Fundo:** cobre a tela, escuro translúcido com desfoque (`fixed inset-0 z-40 bg-slate-950/60` na referência web).
- **Anel:** círculo concêntrico de 250 a 270 px de diâmetro, borda tracejada sutil (`border-dashed border-amber-500/30`), fundo escuro translúcido com desfoque forte.
- **Raio dos itens (R) e diâmetro do anel são coisas diferentes:** R é onde ficam os centros dos itens; o diâmetro é o do círculo de fundo e deve ser maior que 2R para acomodar o tamanho do item.
- **Tamanho dos satélites:** 44 px no Mobile e 48 px em Tablet/Desktop. Esses valores são a referência técnica consolidada para a primeira implementação; a área de toque mínima permanece 44 × 44 px. Ícones: 20 px (consolidação técnica).

## 7. Implementação por plataforma

O contrato da seção 4 é o mesmo em todas. A tecnologia muda:

- **Aplicativo Gospel X (React Native/Expo):** não usar Pointer Events, `setPointerCapture`, CSS `fixed` nem classes Tailwind de web. O rastreamento do gesto deve continuar mesmo com o dedo fora do botão, com reconhecimento de arraste e de toque feito pelo sistema de gestos nativo, e a animação deve rodar na thread de UI. Como sugestão a validar por ADR: react-native-gesture-handler, react-native-reanimated e react-native-safe-area-context. O desfoque de fundo tem custo de desempenho e precisa de alternativa sem desfoque para aparelhos modestos.
- **Painel web (Next.js):** implementação de referência com Pointer Events (`onPointerDown`, `onPointerMove`, `onPointerUp` e `setPointerCapture`), `touch-action: none` na área do anel para a página não rolar junto e cálculo de área segura por CSS. Confirmar a versão do Tailwind, pois os nomes das classes de desfoque mudam entre versões.
- O componente é **um só, compartilhado conceitualmente entre Web e Mobile**. A implementação do mecanismo de entrada é adaptada à plataforma: toque/arraste com dedo no Mobile e clique/arraste com mouse/ponteiro na Web. Não criar duas versões funcionais independentes do GX Orbital.
- O conteúdo do GX Orbital não é fixo por plataforma: Web, celular e tablet respeitam o mesmo contrato conceitual, com adaptações de interação, layout, tamanho da tela e capacidades nativas.
- **O GX Orbital não é o único caminho de navegação nem o único caminho obrigatório para uma ação.** É uma opção contextual complementar; não substitui a navegação convencional nem exige que toda ação esteja dentro dele.
- **Computador/Web:** menus convencionais e controles visíveis da interface continuam disponíveis. O GX Orbital também pode ser oferecido como opção complementar, sem substituir, ocultar ou bloquear a navegação convencional. Ações frequentes, importantes ou contextualmente relevantes podem permanecer diretamente visíveis.
- **Celular:** o GX Orbital pode concentrar uma parcela maior das ações de navegação, controle e operação para preservar espaço e simplicidade visual, sem retirar controles diretos necessários.
- **Tablet:** a interface adapta-se ao tamanho disponível, ao contexto e à forma de interação utilizada (toque, mouse/ponteiro ou teclado). Não se deve impor automaticamente a experiência de celular nem a de computador apenas pelo nome do dispositivo; a apresentação deve acompanhar o espaço e a interação efetivamente disponíveis. Os tamanhos da seção 6 (“Mobile” e “Tablet/Desktop”) são rótulos de referência; a classe de tamanho aplicada deve seguir o espaço e a interação reais, a validar no protótipo. **\[CTP-05\]**
- A duplicação visual de uma ação em mais de um ponto **não duplica sua função, permissão ou processamento**: são apenas diferentes portas de entrada para a mesma ação. O módulo responsável continua sendo a fonte da execução e o servidor valida a autorização quando aplicável.
- O princípio de apresentação é **favorecer reconhecimento visual em vez de exigir memorização ou busca constante por ações**. A escolha entre Orbital, interface direta ou ambos depende do contexto, plataforma, frequência e relevância da ação.
- Estas regras pressupõem um GX Orbital também na Web de usuário final (Vídeos Curtos §4.1 e §12.2), mas o Hub §11 só prevê painéis em Next.js e aplicativo em React Native/Expo. Se existe essa superfície Web de usuário final é uma decisão em aberto. **\[PD-06\]**

## 8. Critérios de aceite

1. Tocar no GX abre o anel e o botão vira X com giro de cerca de 90°.
2. O botão fica no mesmo ponto fechado e aberto.
3. Arrastar em círculo gira nos dois sentidos, sem limite e sem salto ao cruzar ±π.
4. Arrastar nunca seleciona item, mesmo terminando sobre um item.
5. Um toque parado (até 6 px de movimento) num item fecha o anel e executa a ação exatamente uma vez.
6. Depois de um arraste, cliques são ignorados por cerca de 180 ms.
7. Tocar no X ou no fundo fecha sem executar nada, e o toque no fundo não atinge a tela de baixo.
8. Escape fecha em web. O botão voltar do sistema fecha o anel primeiro (depende da regra proposta de 4.5, a validar no protótipo). **\[CDS-05\]**
9. O botão nunca fica sob a barra de gestos ou área segura do iPhone e do Android, nem com navegação por gestos nem por botões.
10. Itens na metade inferior (`posY > 25 px`) aparecem com opacidade 0,45 e escala 0,9; os demais, com opacidade e escala 1.
11. Seta direita gira no sentido horário e seta esquerda no anti-horário, em passos de π/8.
12. Com 1, 2, 8 e 11 itens, os alvos de toque não se sobrepõem. Se a Opção A de 4.2 for adotada, o critério vale também para 12 ou mais itens, e a quantidade visível na metade superior é a mesma para qualquer total. **\[PD-01\]**
13. Itens fora da área visível não são tocáveis nem lidos por leitores de tela (depende da regra proposta de 4.3, a validar no protótipo). **\[CDS-05\]**
14. Nenhum item aparece se a pessoa não pode acessá-lo, e o servidor valida o acesso mesmo assim.
15. Abrir e fechar o anel não interrompe a mídia em reprodução.
16. Com “reduzir movimento” ativo, as animações são reduzidas.
17. As animações e o arraste não travam nos dispositivos de referência definidos para a etapa de validação; o conjunto de aparelhos e versões deverá ser registrado no plano de testes.
18. Arrastar o anel nunca rola a tela por baixo, em nenhuma plataforma.

Critérios acrescentados, a partir de regras que já estavam na consolidação técnica como referência de engenharia: **\[CTP-03\]**

19. Se o sistema cancelar o gesto, ele termina sem selecionar item e sem deixar o anel em estado inconsistente.
20. Com o teclado virtual aberto ou foco em campo editável, o anel aberto fecha e não cobre o campo nem o teclado.
21. Em altura de viewport abaixo de aproximadamente 500 px (paisagem), o raio ou a escala são reduzidos para o anel não colidir com o topo.
22. Em aparelhos modestos, existe alternativa ao desfoque de fundo sem perda de função.

## 9. Erros conhecidos a evitar

- Tratar o arraste como clique e selecionar um item ao soltar.
- Não manter o rastreamento do gesto quando o dedo sai do botão.
- Deixar o anel dar um salto de volta ao cruzar 180° e -180°.
- Girar por distância em pixels em vez de pelo ângulo em relação ao centro do botão.
- Posicionar o botão sob a barra de gestos do celular.
- Copiar o código web para o aplicativo nativo.
- Deixar o toque no fundo passar para a tela de baixo.
- Esconder um item na interface sem validar a permissão no servidor.
- Colocar curtir, comentar, seguir ou compartilhar no anel fora de Vídeos Curtos (a exceção está na seção 4.7).
- Criar uma cópia do componente em cada perfil.
- Redesenhar todos os itens a cada movimento do dedo, ou deixar ouvintes e animações ativos com o anel fechado.
- Girar as duas setas do teclado para o mesmo lado.
- Deixar a página rolar por baixo enquanto o anel está sendo arrastado.

## 10. Contrato dos itens

A interface abaixo é a **referência web** (TypeScript com React). O contrato aprovado são os campos e seus significados; o tipo do ícone e a forma da ação variam com a plataforma, e no aplicativo nativo o ícone recebe as propriedades nativas de tamanho e cor. **\[CDS-09\]**

```ts
export interface RadialMenuItem {
  id: string;
  label: string;
  icon: React.ComponentType<{ className?: string }>;
  badge?: string;
  action: () => void;
  isActive?: boolean;
}
```

- `label` é o nome acessível do item.
- `className` é de Tailwind e vale só na web.
- O contrato é deliberadamente mínimo. O GX Orbital é **burro**: apresenta e dispara a interação; não calcula permissões, não decide governança e não conhece regras de negócio.
- O conjunto autorizado é calculado pelo contexto/Governança; o contexto monta os itens; o módulo responsável executa a ação; o servidor valida novamente quando aplicável.
- Não adicionar ao item campos como `requiredPermission`, `featureFlag` ou `destination` apenas para transferir lógica de negócio para o componente.
- O significado visual de `badge` (formato do texto, onde aparece, como convive com o indicador de notificação do botão fechado) e de `isActive` (como o item ativo é destacado) não está definido. **\[PF-02\]**

## 11. Destinos, contexto e governança

- O mesmo componente é usado nos contextos autorizados de Usuário Final, Igreja, Artista e outros perfis posteriormente aprovados.
- O GX Orbital é um **centro contextual de navegação e ações**, não uma navegação universal fixa.
- A lista de destinos e ações é **contextual** e pode mudar entre perfis, telas, estados e vínculos.
- Segue o modelo de autorização canônico do DME — Governança, seção 3 (fonte única): Pessoa → Conta Gospel X → Perfil → Vínculo → Função/Cargo → Ministério/Departamento → Unidade/Local → Recurso → Permissão → Ação → Dados acessíveis → Escopo → Contexto → Período. Os Destinos/Ações disponíveis no GX Orbital são os Recursos/Módulos que a Permissão libera dentro desse modelo. **\[CDS-03\]**
- O GX Orbital pode executar navegação, ações rápidas, controles da tela, funções globais e ações específicas de perfil, vínculo ou módulo, desde que autorizadas pelo contexto.
- Ele pode apresentar ações destrutivas ou sensíveis quando o contexto e a Governança permitirem. **Ações financeiras só podem aparecer no Painel Web em Next.js, nunca no aplicativo mobile** (Governança §8; Hub §11). A confirmação e o fluxo específico pertencem ao módulo responsável; a autorização final continua sendo validada pelo servidor. **\[CDS-08\]**
- O GX Orbital **não possui hierarquia interna, submenu ou subopções**. Cada contexto possui seu próprio conjunto de opções; ao mudar de contexto, o conjunto é recalculado.
- A busca e qualquer resultado apresentado a partir do botão só mostram o que a pessoa pode descobrir ou visualizar segundo as regras da entidade e da Governança; o contexto não restringe artificialmente a busca. **\[CDS-03\]**
- Um perfil nunca deve enxergar um destino que não possa acessar. Ocultar o item na interface não substitui a autorização server-side do DME de Governança. (Ver a pergunta em aberto sobre “Oração” sem vínculo, PD-02.)
- Badges e indicadores exibidos no Orbital respeitam as mesmas regras de privacidade e isolamento dos dados de origem (Governança, seção sobre navegação global). **\[CDS-03\]**
- O componente não concede permissões, não altera vínculo e não acessa dados sensíveis por conta própria.

## 12. Validação antes de construir

Como é uma navegação inovadora e tentativas anteriores falharam, ela não deve ir direto para produção:

- construir primeiro um protótipo navegável em aparelho real e em Web, validando o uso com uma mão no Mobile e com mouse/ponteiro na Web;
- testar com pessoas reais e registrar acertos, erros de seleção e abandono (metas a definir);
- liberar por feature flag;
- não criar agora um menu linear alternativo específico para substituir o gesto circular. A acessibilidade geral continua obrigatória; uma alternativa específica só será desenhada se surgir uma necessidade real.

Complementos propostos: **\[CTP-04\]**

- o plano de testes registra, no mínimo, estas categorias de aparelho: iPhone com indicador de início e iPhone com ilha dinâmica ou recorte; Android com navegação por gestos; Android com navegação por três botões; Android modesto, para o teste de desempenho sem desfoque; um tablet; e computador com mouse e com teclado;
- cada tentativa registra: aparelho e versão, tarefa pedida, acerto ou erro, item errado selecionado, abandono e tempo para concluir;
- as metas numéricas de acerto, erro e abandono ainda não existem. **\[PF-03\]**

A acessibilidade de leitores de tela no celular (como abrir, percorrer e escolher itens sem girar o anel) não tem mecanismo definido, porque só o teclado da web está descrito. **\[PD-05\]**

## 13. Princípio de experiência

O conceito “Um Dedo, Um Centro” deve reduzir a complexidade percebida sem reduzir a capacidade interna do produto. **Completo por dentro, simples por fora.** O GX Orbital deve oferecer um ponto consistente de acesso contextual, especialmente no celular, sem ser definido como a navegação principal exclusiva ou universal. Na Web, complementa os menus convencionais; no tablet, adapta-se ao espaço, contexto e forma de interação.

## 14. Decisões fechadas e pendências

**Decisões fechadas** (inalteradas):

- O GX Orbital é um centro contextual de navegação e ações, não uma navegação universal fixa.
- Todo item selecionado fecha o orbital antes da ação. Não existe `keepOpen` no contrato inicial.
- O GX Orbital funciona como **Carrossel Orbital Contínuo**: não há limite estrutural fixo de opções totais; a capacidade simultânea é determinada pela geometria. A referência inicial é 5 opções visíveis simultaneamente em Web e Mobile.
- Web e Mobile usam o mesmo componente conceitual e o mesmo contrato; apenas o mecanismo de entrada é adaptado à plataforma.
- No contexto de Vídeos Curtos, o **botão do GX Orbital** possui acionamento específico: Mobile = toque rápido para próximo vídeo e pressionar/segurar por aproximadamente 1–2 segundos para abrir o GX Orbital; Web = clique simples para próximo vídeo e duplo clique para abrir o GX Orbital. O anel aberto traz as ações do vídeo em foco, e a escolha segue o contrato geral (toque parado depois de soltar o dedo; arrastar nunca seleciona). Decisão do proprietário em 30/10/2026.
- O contrato do item permanece mínimo: `id`, `label`, `icon`, `action`, `badge?`, `isActive?`.
- Não será criada agora uma alternativa específica de menu linear para substituir o gesto circular. Acessibilidade geral permanece obrigatória.
- O GX Orbital **não possui hierarquia interna, submenu ou subopções**. Cada contexto possui seu próprio conjunto de opções; ao mudar de contexto, o conjunto é recalculado.
- O GX Orbital pode executar ações contextuais, inclusive sensíveis ou destrutivas, quando autorizadas; ações financeiras apenas no Painel Web (ver seção 11). Confirmação e execução pertencem ao módulo responsável; o servidor valida. **\[CDS-08\]**
- O GX Orbital não é o único caminho de acesso às ações. Ações podem aparecer simultaneamente no Orbital e diretamente na interface quando isso for melhor para descoberta, reconhecimento visual, velocidade de acesso ou carga cognitiva, especialmente na Web. A Web pode usar botões, links, ícones, controles e menu hambúrguer.
- Abrir o GX Orbital não bloqueia nem interrompe automaticamente o contexto atual. Uma ação pode permanecer no contexto atual ou navegar para outro contexto; depois da navegação, o conjunto de opções do orbital é recalculado conforme o novo contexto.

**Pendências remanescentes:** **\[CDS-10\]**

| Nº | Pendência | Situação nesta revisão |
| --- | --- | --- |
| 1 | Lista padrão de destinos e ações por perfil e contexto | Depende dos DMEs de perfil; continua aberta \[PF\] |
| 2 | Escopo da busca e aplicação das permissões | Respondida em grande parte por Busca §2, §5 e §18 (porta universal; contexto não restringe nem amplia permissão); registrada nas seções 3 e 11 |
| 3 | Convivência visual com Vídeos Curtos (posição do botão, cobertura e áreas de gesto), mantendo a regra funcional já fechada | Continua aberta; Vídeos Curtos §16 pede integração “sem cobrir conteúdo ou ações do vídeo” \[PF\] |
| 4 | Animações e transições da mudança de contexto | Continua aberta \[PF\] |
| 5 | Quais controles de tela entram em cada contexto | Continua aberta \[PF\] |
| 6 | GX IA EX no Orbital | Removida como pendência do Orbital; o ponto de acesso é decisão do DME GX IA EX (§14) |
| 7 | Anexar a gravação ou captura do componente de referência | Continua aberta; a gravação não estava acessível \[PF\] |
| 8 | Conflito de geometria entre itens totais e visíveis (OBS-30d) | Nova na revisão: \[PD-01\] |
| 9 | Gestos de Vídeos Curtos na Web e no Mobile; lista de ações sem “bloquear” | Novas: \[PD-03\] e \[PD-04\] |
| 10 | “Oração” sem vínculo; acessibilidade no celular; superfície Web de usuário final | Novas: \[PD-02\], \[PD-05\] e \[PD-06\] |
| 11 | Distância exata do botão à borda; centralização com poucos itens; significado de badge e isActive; metas dos testes | Novas: \[PF-01\], \[PF-02\] e \[PF-03\] |

## Consolidação técnica — referência da primeira implementação

A primeira implementação real do GX Orbital foi consultada exclusivamente para complementar medidas, geometria, limites e parâmetros de engenharia. Ela **não altera as decisões funcionais do DME** e não é fonte de verdade do produto.

#### Parâmetros incorporados como referência técnica

- Área de toque mínima de referência: **44 × 44 px**.
- Botão central: **56 px no Mobile** e **64 px em Tablet/Desktop**.
- Satélites: **44 px no Mobile** e **48 px em Tablet/Desktop**.
- Ícones dos satélites: **20 px** como referência técnica.
- Safe area: posição inferior deve incorporar `env(safe-area-inset-bottom, 0px)` e não ocupar a área de gestos do sistema.
- Landscape crítico: abaixo de aproximadamente **500 px de altura**, reduzir raio/escala para evitar colisão com o topo da viewport (critério 21).
- Teclado virtual: detectar por `visualViewport` e/ou foco em campos editáveis; se o Orbital estiver aberto, fechá-lo para não cobrir campo ou teclado (critério 20).
- Geometria variável: para N itens, distribuição angular uniforme; condição de conforto entre centros pode ser expressa por `2 · R · sen(π / N) ≥ 48 px`. A leitura de N (total ou visíveis) está em revisão em 4.2 \[PD-01\].
- Desempenho: priorizar `transform` e `opacity`, evitando alterações de layout durante o giro; meta de referência de **60 FPS sustentados**.
- Gesto técnico: limiar de aproximadamente **6 px** para distinguir arraste de toque; lockout pós-arraste de aproximadamente **180 ms**; captura de ponteiro ou mecanismo nativo equivalente; `pointercancel` deve encerrar o gesto com segurança (critério 19).
- Camadas Web: referência de backdrop **z40** e Orbital **z50**.
- Acessibilidade técnica: foco, Escape, teclado e semântica acessível devem ser implementados conforme a tecnologia utilizada.

#### Regra importante sobre a primeira implementação

A primeira versão utilizava uma configuração com **8 itens**, mas essa quantidade **não é requisito do DME**. O GX Orbital continua aceitando quantidade variável de opções, sem limite estrutural total. A referência inicial permanece em **5 opções visíveis simultaneamente**.

#### Estado da consolidação

Os parâmetros acima são referências de engenharia para a futura implementação. Eles não reabrem nem substituem as decisões funcionais já fechadas no DME.

## 15. Estado do documento

Esta especificação é uma proposta de UX e interação do **GX Orbital** no Gospel X EX. Na escala do Hub (§14): as decisões fechadas da seção 14 estão em **decisão aprovada**; as regras marcadas “a validar no protótipo” e os parâmetros da consolidação técnica estão em **arquitetura estudada**; **nada está implementado**. Não autoriza implementação automática no Gospel X principal. A implementação deverá ocorrer somente após validação visual, responsiva, acessível e funcional (seção 12). **\[CDS-11\]**
