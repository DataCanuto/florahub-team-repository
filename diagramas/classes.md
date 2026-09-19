# Diagrama de Classes — FloraHub

```mermaid
classDiagram
    class Usuario {
        +string id
        +string nome
        +string email
        +string senhaHash
        +Localizacao localizacao
        +NivelExperiencia experiencia
        +criarPublicacao(planta, imagem) Publicacao
        +editarPreferencias(interesses, preferencias)
        +ativarLocalizacao(bool)
    }

    class Admin {
        +string id
        +string nome
        +string email
        +moderarPublicacao(publicacaoId)
        +gerenciarUsuario(usuarioId, acao)
    }

    class Interesse {
        +string id
        +string nome
    }

    class Preferencia {
        +string id
        +string nome
        +TipoPreferencia tipo
    }

    class Localizacao {
        +float latitude
        +float longitude
        +string cidade
        +calcularDistancia(Localizacao) float
    }

    class Planta {
        +string id
        +string nomeCientifico
        +string[] nomesPopulares
        +string familia
        +string genero
        +string especie
        +string continenteOrigem
        +bool saudavel
    }

    class Recomendacao {
        +string id
        +TipoRecomendacao tipo
        +string descricao
        +gerarPorClima(dadosClima) Recomendacao
    }

    class Publicacao {
        +string id
        +DateTime dataCriacao
        +string imagemUrl
        +bool publicadaPorVisitante
        +Localizacao localizacao
        +bool contatoBloqueado
        +calcularRelevancia(Usuario visualizador) float
    }

    class ClimaService {
        <<external service>>
        +consultarClima(Localizacao) DadosClima
    }

    class IdentificacaoIAService {
        <<external service>>
        +identificarPlanta(imagem) Planta
        +gerarRecomendacoes(Planta, DadosClima) Recomendacao[]
    }

    class DadosClima {
        +float temperatura
        +float umidade
        +string condicao
    }

    Usuario "1" --> "0..*" Interesse : possui
    Usuario "1" --> "0..*" Preferencia : possui
    Usuario "1" --> "0..1" Localizacao : possui
    Usuario "1" --> "0..*" Publicacao : cria
    Admin --|> Usuario : herda

    Publicacao "1" --> "1" Planta : identifica
    Publicacao "1" --> "0..*" Recomendacao : contém
    Publicacao "0..1" --> "0..1" Localizacao : ocorre em

    IdentificacaoIAService ..> Planta : cria
    IdentificacaoIAService ..> Recomendacao : cria
    ClimaService ..> DadosClima : retorna
    Recomendacao ..> DadosClima : baseia-se em
```

## Notas de modelagem

- `Admin` herda de `Usuario` (permissões adicionais de moderação/gestão).
- `IdentificacaoIAService` e `ClimaService` representam as integrações externas (OpenAI e OpenWeather, respectivamente) — modeladas como serviços externos e não como entidades persistidas.
- `Publicacao.contatoBloqueado` reflete a regra de negócio de ocultar dados sensíveis (endereço/contato) para visualizadores não autenticados.
- `Publicacao.calcularRelevancia` representa o critério de ordenação do feed (combinação de proximidade geográfica e interesses em comum), usado pelo algoritmo de recomendação.
- `TipoRecomendacao` (enum sugerido): `PODA`, `PRODUTO`, `CLIMA`, `ANIMAL_DOMESTICO`, `OUTRO`.
- `TipoPreferencia` (enum sugerido): `AMBIENTE` (jardim, apartamento, terraço), `LUMINOSIDADE` (muita/pouca luz), `DISPONIBILIDADE` (muito/pouco tempo).
