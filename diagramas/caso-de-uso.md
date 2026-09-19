# Diagrama de Casos de Uso — FloraHub

```mermaid
flowchart LR
    Visitante((Visitante))
    Usuario((Usuário))
    Admin((Admin))

    subgraph Identificacao["Identificação de Plantas"]
        UC1["Identificar planta por imagem"]
        UC2["Ativar localização para recomendação climática"]
        UC3["Visualizar resultado: sucesso ou falha"]
        UC4["Receber recomendações de cuidado"]
    end

    subgraph Conta["Conta e Perfil"]
        UC5["Cadastrar-se"]
        UC6["Fazer login"]
        UC7["Recuperar senha"]
        UC8["Definir ou editar interesses e preferências"]
    end

    subgraph FeedSocial["Feed e Interação Social"]
        UC9["Visualizar feed"]
        UC10["Ver publicação própria gerada por identificação"]
        UC11["Ver publicações de outros usuários com dados bloqueados se deslogado"]
        UC12["Criar publicação manual"]
        UC13["Acessar contato ou endereço de outro usuário"]
        UC14["Enviar mensagem via inbox de contatos"]
    end

    subgraph Administracao["Administração"]
        UC15["Moderar publicações"]
        UC16["Gerenciar usuários"]
    end

    Visitante --> UC1
    Visitante --> UC2
    Visitante --> UC3
    Visitante --> UC4
    Visitante --> UC9
    Visitante --> UC11
    Visitante -.tentativa de acesso.-> UC13
    UC13 -.inclui.-> UC5

    Usuario --> UC1
    Usuario --> UC2
    Usuario --> UC3
    Usuario --> UC4
    Usuario --> UC5
    Usuario --> UC6
    Usuario --> UC7
    Usuario --> UC8
    Usuario --> UC9
    Usuario --> UC10
    Usuario --> UC11
    Usuario --> UC12
    Usuario --> UC13
    Usuario --> UC14

    Admin --> UC15
    Admin --> UC16
    Admin --> UC6

    UC1 -.inclui.-> UC3
    UC2 -.estende.-> UC4
    UC3 -.inclui.-> UC10
```

## Descrição dos principais casos de uso

| Caso de uso | Ator(es) | Descrição |
|---|---|---|
| Identificar planta por imagem | Visitante, Usuário | Envio de imagem para identificação via IA, retornando dados da planta. |
| Ativar localização para recomendação climática | Visitante, Usuário | Consentimento de uso da localização para consulta à API OpenWeather. |
| Visualizar resultado (sucesso/falha) | Visitante, Usuário | Exibição do resultado da identificação, com CTA para explorar a homepage. |
| Receber recomendações de cuidado | Visitante, Usuário | Exibição de recomendações associadas à planta identificada (poda, produtos, clima, animais domésticos). |
| Cadastrar-se | Visitante | Criação de conta, com preenchimento opcional de interesses/preferências. |
| Fazer login / Recuperar senha | Usuário, Admin | Autenticação no sistema. |
| Definir/editar interesses e preferências | Usuário | Configuração de interesses, ambiente e experiência no painel do usuário. |
| Visualizar feed | Visitante, Usuário | Consulta de publicações das últimas 24h, ordenadas por proximidade/interesse. |
| Ver publicações de outros usuários | Visitante, Usuário | Visualização de publicações com dados sensíveis ocultos se deslogado. |
| Acessar contato/endereço de outro usuário | Visitante, Usuário | Ação que exige autenticação; visitante é direcionado ao cadastro. |
| Criar publicação manual | Usuário | Publicação não vinculada diretamente a uma identificação recente. |
| Enviar mensagem (inbox/contatos) | Usuário | Comunicação direta entre usuários conectados. |
| Moderar publicações / Gerenciar usuários | Admin | Ações administrativas de manutenção do sistema. |
