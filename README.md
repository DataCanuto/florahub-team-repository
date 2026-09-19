# FloraHub

Repositório de documentação do Trabalho de Conclusão de Curso (TCC) desenvolvido em equipe no curso de Desenvolvimento de Sistemas do SENAI CIMATEC.

> Este repositório **não contém o código-fonte da aplicação** (que vive em repositório próprio). Aqui são centralizadas as regras de negócio, requisitos, diagramas e artefatos de design (wireframes, mockups, protótipos) produzidos ao longo do desenvolvimento do projeto.

---

## 1. Conceito do projeto

**FloraHub** é um aplicativo mobile que une **identificação de plantas via imagem** com uma **rede social voltada a pessoas que cuidam de plantas**.

O usuário fotografa uma planta e recebe, via inteligência artificial, sua identificação (nome científico, nomes populares, família, gênero, espécie, origem e estado de saúde), além de recomendações de cuidado — incluindo recomendações contextualizadas pelo clima local (integração com a API do OpenWeather) quando o usuário permite o uso da localização. Cada identificação realizada vira automaticamente uma publicação no feed do app, conectando o usuário a outras pessoas com plantas e interesses semelhantes.

### 1.1 O problema (dores da persona)

A persona do FloraHub é alguém que cultiva plantas (do iniciante ao entusiasta) e busca, ao mesmo tempo, **informação confiável** e **conexão com outras pessoas** que compartilham o mesmo hobby. As principais dores identificadas são:

- **Falta de identidade e diagnóstico**: dificuldade em saber o nome, a família e as necessidades reais de uma planta adquirida sem etiqueta ou informação de origem.
- **Cuidado genérico**: recomendações de cuidado que não consideram o clima e a localização real do usuário (luminosidade, umidade, temperatura sazonal).
- **Isolamento social**: a maioria dos aplicativos de identificação de plantas existentes no mercado é puramente utilitária — resolve o "o que é essa planta?", mas não estimula troca de experiência, dúvidas ou proximidade entre pessoas com o mesmo interesse.
- **Falta de pertencimento a uma comunidade local**: dificuldade de encontrar outras pessoas próximas (bairro, cidade) que cultivem plantas parecidas e possam trocar mudas, dicas ou experiências presenciais.
- **Barreira de entrada**: apps que exigem cadastro completo antes de qualquer interação, desestimulando o uso casual/exploratório.

### 1.2 A proposta de valor

O diferencial do FloraHub em relação aos concorrentes (apps de identificação "utilitários") é tratar cada identificação como o **início de uma interação social**, e não como o fim do fluxo:

- Toda identificação vira uma publicação no feed, mesmo para visitantes não cadastrados (com informações sensíveis ocultadas até login).
- O feed é ordenado por **proximidade geográfica** (quando o usuário ativa localização) ou por **interesses em comum** (quando não ativa), aproximando pessoas com contextos de cultivo parecidos.
- O cadastro captura interesses (ex.: orquídeas, suculentas, roseiras, palmeiras), preferências de ambiente (jardim, apartamento, terraço, muita/pouca luz, muito/pouco tempo disponível) e experiência, alimentando tanto recomendações de cuidado quanto o algoritmo de aproximação social.
- O uso é possível sem cadastro (modo visitante/logoff), com CTA natural para criação de conta no momento em que o usuário deseja mais informações (contato, endereço) de outra publicação.

---

## 2. Visão geral do fluxo do app

1. **Telas de boas-vindas** (2 telas, navegação por swipe horizontal) — introdução ao conceito do app.
2. **Tela de identificação de plantas** (primeira tela funcional, sujeita a ajustes futuros de posição no fluxo) — captura/seleção de imagem e opção de ativar localização para recomendações climáticas.
3. **Tela de carregamento** (loading) — processamento da identificação (chamada à API de IA).
4. **Tela de sucesso ou falha da identificação**:
   - Em caso de sucesso: exibe os dados da planta identificada e recomendações; a publicação passa a existir no feed da homepage.
   - Em caso de falha: orienta o usuário a tentar novamente, mantendo o CTA para explorar a homepage.
5. **Homepage (logoff ou logada)**: visualmente semelhante à tela de feed.
   - **Logoff**: publicações aparecem com dados sensíveis (endereço, contato) bloqueados; ao tentar acessá-los, o usuário é conduzido ao fluxo de cadastro/login.
   - **Logada**: feed completo, ordenado por proximidade e/ou interesses.
6. **Feed**: agrega pesquisas/identificações realizadas nas últimas 24h.
   - Ordenação primária: proximidade geográfica com a pesquisa do próprio usuário (se localização ativada) ou por interesses em comum (se localização desativada).
   - Publicações de usuários próximos ao usuário aparecem no topo do feed, logo após a publicação do próprio usuário.
7. **Cadastro/Login**: inclui, opcionalmente (não obrigatório no cadastro inicial), formulário de interesses, preferências e experiência — editável posteriormente no painel do usuário (área do usuário).
8. **Área do usuário**: edição de perfil, preferências/interesses e histórico de publicações.
9. **Inbox / Contatos**: comunicação entre usuários conectados.

---

## 3. Integrações externas

| Serviço | Uso no app |
|---|---|
| **API de IA (OpenAI)** | Identificação da planta a partir da imagem enviada e possivelmente geração de recomendações textuais. |
| **OpenWeather API** | Dados climáticos da localização do usuário, usados para gerar recomendações de cuidado contextualizadas (ex.: rega, exposição solar, proteção contra frio/calor). |
| **Geolocalização** | Usada tanto para recomendações climáticas quanto para ordenação do feed por proximidade e aproximação entre usuários. |

---

## 4. Requisitos

### 4.1 Requisitos Funcionais (RF)

| ID | Descrição |
|---|---|
| RF01 | O sistema deve permitir que o usuário envie uma imagem para identificação de planta. |
| RF02 | O sistema deve retornar, a partir da identificação, os dados da planta: nome científico, nome(s) popular(es), família, gênero, espécie, continente de origem e estado de saúde. |
| RF03 | O sistema deve permitir que o usuário ative o uso de localização durante a identificação. |
| RF04 | Quando a localização estiver ativa, o sistema deve consultar a API do OpenWeather e gerar recomendações de cuidado baseadas no clima local. |
| RF05 | O sistema deve gerar recomendações de cuidado (poda, uso de produtos, presença de animais domésticos, entre outras) associadas à planta identificada. |
| RF06 | O sistema deve exibir uma tela de sucesso com os dados da planta e recomendações, ou uma tela de falha caso a identificação não seja possível. |
| RF07 | Toda identificação realizada (por usuário logado ou visitante) deve gerar uma publicação no feed. |
| RF08 | O sistema deve permitir a navegação para a homepage a partir da tela de sucesso/falha, incentivando a exploração do feed. |
| RF09 | O feed deve exibir publicações das últimas 24 horas. |
| RF10 | O feed deve ordenar publicações por proximidade geográfica quando o usuário tiver a localização ativada, ou por interesses em comum quando não tiver. |
| RF11 | Publicações de usuários próximos devem ser priorizadas no feed, exibidas logo após a publicação do próprio usuário. |
| RF12 | Para usuários não autenticados (logoff), informações sensíveis da publicação (endereço, contato) devem ser ocultadas. |
| RF13 | Ao tentar acessar informações bloqueadas, o usuário deve ser direcionado ao fluxo de cadastro/login. |
| RF14 | O sistema deve permitir cadastro de usuário com informações básicas de conta. |
| RF15 | O sistema deve permitir, de forma opcional (durante ou após o cadastro), o preenchimento de interesses (ex.: orquídeas, suculentas, roseiras, palmeiras), preferências de ambiente (jardim, apartamento, terraço, luminosidade, disponibilidade de tempo) e nível de experiência. |
| RF16 | O sistema deve permitir a edição de interesses e preferências no painel do usuário a qualquer momento. |
| RF17 | O sistema deve utilizar interesses, preferências e localização para recomendar publicações e outros usuários com perfil semelhante. |
| RF18 | O sistema deve permitir login e recuperação de senha (fluxo "esqueci minha senha"). |
| RF19 | O sistema deve permitir criação de novas publicações manuais pelo usuário (além das geradas por identificação). |
| RF20 | O sistema deve fornecer uma área de contatos/inbox para comunicação entre usuários. |
| RF21 | O sistema deve permitir que um administrador gerencie dados do sistema (moderação de conteúdo, gestão de usuários e publicações). |

### 4.2 Requisitos de Sistema (RS / Não-Funcionais)

| ID | Descrição |
|---|---|
| RS01 | O sistema deve se comunicar com serviços externos (IA de identificação e OpenWeather) via requisições HTTP/API, tratando falhas de indisponibilidade com mensagens claras ao usuário (tela de falha). |
| RS02 | O tempo de resposta da identificação deve ser comunicado ao usuário através de uma tela de carregamento (loading). |
| RS03 | O acesso à localização do usuário deve ser opcional e solicitado explicitamente (consentimento). |
| RS04 | Dados sensíveis de contato/endereço de usuários não autenticados no acesso não devem ser expostos a visitantes. |
| RS05 | O sistema deve suportar uso sem autenticação (modo visitante), com persistência ao menos temporária do estado da navegação. |
| RS06 | A arquitetura deve separar claramente frontend (app), backend (regras de negócio e integrações) e serviços externos de terceiros. |
| RS07 | O sistema deve ser responsivo/mobile-first, considerando que o wireframe original ([digital-wireframe.png](assets/figma/wireframes/digital-wireframe.png)) foi desenhado para uso mobile. |
| RS08 | O algoritmo de recomendação (feed e usuários) deve considerar múltiplos critérios combináveis: proximidade geográfica e similaridade de interesses. |

---

## 5. Modelo de domínio (classes principais)

- **Planta**: entidade identificada a partir de uma imagem. Atributos: nome científico, nome(s) popular(es), família, gênero, espécie, continente de origem, saúde (booleano).
- **Recomendação**: entidade associada a uma identificação, representando sugestões de cuidado (poda, uso de produto, recomendação baseada no clima, compatibilidade com animais domésticos, entre outras).
- **Usuário**: pessoa cadastrada no sistema; possui interesses, preferências, nível de experiência, localização e publicações.
- **Admin**: usuário com permissões de gestão/moderação do sistema.
- **Publicação (Feed)**: registro de uma identificação (por usuário ou visitante), associada a uma Planta e a possíveis Recomendações, exibida no feed e ordenada por proximidade/interesse.
- **Interesse / Preferência**: atributos configuráveis do usuário (ex.: tipos de planta de interesse, tipo de ambiente, luminosidade, tempo disponível), usados no algoritmo de recomendação.

> Observação: o modelo acima é conceitual e será refinado nos diagramas de classes formais à medida que o desenvolvimento avança.

---

## 6. Diagramas

Esta seção deve ser atualizada conforme os diagramas forem produzidos e versionados no repositório.

| Diagrama | Descrição | Status |
|---|---|---|
| Diagrama de Casos de Uso | Interações entre Usuário/Visitante/Admin e o sistema (identificar planta, cadastrar-se, visualizar feed, editar preferências, etc.). | A produzir |
| Diagrama de Classes | Estrutura de Planta, Recomendação, Usuário, Admin, Publicação e seus relacionamentos. | A produzir |
| Diagrama de Sequência | Fluxo de identificação de planta (usuário → app → API de IA → OpenWeather → resposta). | A produzir |
| Diagrama de Atividades | Fluxo de navegação do usuário desde as telas de boas-vindas até a publicação no feed. | A produzir |

> Sugestão de organização: salvar os arquivos-fonte (ex.: `.drawio`, `.puml`) e as exportações (`.png`/`.svg`) em uma pasta `diagramas/` na raiz do repositório, versionando ambos.

---

## 7. Estrutura do repositório

```
florahub-team-repository/
├── assets/
│   └── figma/
│       ├── wireframes/          # Wireframes de baixa complexidade (digital-wireframe.png)
│       ├── low-fidelity-prototype/
│       ├── high-fidelity-prototype/
│       └── mockup/
├── backend/                     # Reservado para documentação/artefatos de backend
├── pages/                       # Protótipo estático de referência das telas do app (HTML/CSS/JS)
│   ├── welcome-page/
│   ├── identifying-page/
│   ├── loading-page/
│   ├── identifySucces-page/
│   ├── identifyFail-page/
│   ├── login-homepage/
│   ├── logoff-homepage - Copia/
│   ├── login-page/
│   ├── register-page/
│   ├── forgotPassword-page/
│   ├── userArea-page/
│   ├── inbox-page/
│   ├── contacts-page/
│   └── createNewPost-page/
└── README.md
```

O wireframe de referência está em [assets/figma/wireframes/digital-wireframe.png](assets/figma/wireframes/digital-wireframe.png).

---

## 8. Status do documento

Este README é um documento vivo, atualizado conforme o projeto evolui ao longo das etapas do TCC. Pontos em aberto conhecidos:

- Posição definitiva da tela de identificação de plantas no fluxo inicial (a ser ajustada).
- Tela de formulário de interesses/preferências no cadastro ainda não implementada nas páginas de referência (disponível apenas no painel do usuário).
- Diagramas formais (casos de uso, classes, sequência, atividades) ainda não produzidos — seção 6 a ser preenchida.
