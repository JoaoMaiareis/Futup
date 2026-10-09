# Futup

[![Status](https://img.shields.io/badge/status-em_desenvolvimento-yellow)]()
[![Versão](https://img.shields.io/badge/versão-0.2.0-blue)]()
[![Licença](https://img.shields.io/badge/licença-acadêmica-lightgrey)]()

**Instituição:** Ceub  
**Curso:** Ciência da Computação  
**Disciplina:** Desenvolvimento Web  
**Turma / Semestre:** 2026.4  
**Professor(a):** Felippe Pires Ferreira  
**Status do projeto:** Etapa 1 

---

## Sumário

- [1. Descrição do projeto](#1-descrição-do-projeto)
- [2. Funcionalidades](#2-funcionalidades)
- [3. Demonstração](#3-demonstração)
- [4. Tecnologias utilizadas](#4-tecnologias-utilizadas)
- [5. Arquitetura](#5-arquitetura)
- [6. Organização dos diretórios](#6-organização-dos-diretórios)
- [7. Participantes](#7-participantes)
- [8. Como executar](#8-como-executar)
- [9. Configuração](#9-configuração)
- [10. Testes](#10-testes)
- [11. Uso de inteligência artificial](#11-uso-de-inteligência-artificial)
- [12. Contribuição e fluxo de trabalho](#12-contribuição-e-fluxo-de-trabalho)
- [13. Histórico de versões](#13-histórico-de-versões)[]
- [14. Limitações e próximos passos](#14-limitações-e-próximos-passos)
- [15. Licença, referências e contato](#15-licença-referências-e-contato)

---

## 1. Descrição do projeto

O **Futup** é uma aplicação web para gestão de peladas: partidas informais entre amigos, como futsal, futebol society e futebol de campo, além de futevôlei. O sistema reúne em um só lugar a organização da partida, a formação dos times e a escolha da quadra.

O problema central é a combinação de **falta de gente para jogar** com **falta de organização**. Hoje, quem monta a pelada precisa chamar jogadores um a um, controlar quem confirmou, dividir os times e ainda torcer para que o tempo ajude. O Futup centraliza esse processo: o organizador cria a pelada, as pessoas entram pelas vagas disponíveis e o sistema acompanha a previsão do clima, alertando os envolvidos quando há risco de chuva e sugerindo novos horários quando necessário.

Uma mesma pessoa pode **organizar e/ou jogar** peladas; os dois papéis não são necessariamente simultâneos. As peladas são sempre públicas, e as quadras são cadastradas pelos próprios usuários.

### Objetivos

- **Objetivo geral:** desenvolver uma aplicação web que facilite a organização de peladas, ajudando a completar vagas, formar times e lidar com o clima.
- **Objetivos específicos:**
  - permitir cadastro e autenticação de usuários;
  - permitir criar peladas (tipo, data, horário, quadra) e entrar nelas enquanto houver vagas;
  - cadastrar quadras sem duplicidade;
  - formar times por sorteio ou de forma livre e gerenciar a fila de times durante a pelada;
  - consultar a previsão do clima e alertar sobre chuva, sugerindo remarcação quando necessário.

### Público-alvo

- Pessoas que organizam peladas e precisam completar vagas e montar times
- Pessoas que querem encontrar uma pelada para jogar

---

## 2. Funcionalidades

| Funcionalidade | Descrição | Status |
| --- | --- | --- |
| Autenticação | Cadastro, login e logout. Só pessoa logada pode entrar em uma pelada | Planejada |
| Cadastro de quadras | Quadras cadastradas pelos usuários, sem duplicidade; cada quadra serve a um tipo de pelada | Planejada |
| Criação de peladas | O organizador define tipo, data, horário, quadra e se os times são sorteados ou livres | Planejada |
| Entrada e vagas | Pessoas entram enquanto há vaga; a pelada passa a "cheia" quando lota | Planejada |
| Gestão de times | Times identificados por letras (A, B, C...), com sorteio ou formação livre | Planejada |
| Fila de times | Controle de quem joga e quem espera, atualizado a partir do resultado informado | Planejada |
| Reposição de vaga | Substituição de quem sai no meio da partida | Planejada |
| Previsão do clima | Consulta à API Open-Meteo, com alerta de chuva e sugestão de remarcar | Planejada |
| Moderação pelo organizador | O organizador pode remover participantes e cancelar a pelada | Planejada |

### Regras de negócio

**Tipos de pelada e vagas**

| Tipo | Jogadores por time | Máximo de times |
| --- | --- | --- |
| Futsal | 5 | 5 |
| Campo sintético | 7 | 5 |
| Futebol de campo | 11 | 5 |
| Futevôlei | 2 | 7 |

- O limite de vagas é de **pessoas**: capacidade = número máximo de times × jogadores por time do tipo.
- O mínimo ideal é de **2 times completos**. Ter menos que isso não impede a pelada de acontecer.
- A pelada é exibida como **"cheia"** quando todas as vagas são preenchidas.
- O organizador pode remover participantes e cancelar a pelada.
- Todas as peladas são públicas.

**Quadras**

- Uma quadra é considerada duplicada quando tem **coordenadas próximas** (algumas dezenas de metros) **e o mesmo endereço na cidade**.
- Cada quadra serve a um único tipo de pelada.

**Times**

- Os times são identificados por **letras**.
- Na criação da pelada, o organizador define se os times são **sorteados** ou **livres**. A pessoa não escolhe individualmente.
- O sorteio só ocorre na opção "times sorteados" e considera apenas times com vaga livre. Na opção "time livre" não há sorteio.
- O organizador pode alterar os times.

**Fila de times**

- Jogam os **dois primeiros** da fila.
- Quem **perde** vai para o último lugar da fila e entra o próximo; quem **ganha** continua.
- Em caso de **empate**, os dois times saem e vão ao fim da fila em ordem alfabética.
- A ordem inicial segue a ordem de criação dos times.
- O organizador informa o resultado e o sistema reorganiza a fila automaticamente.
- A regra vale para todos os tipos de pelada.

Exemplo com os times A a D: A x B, B perde e vai ao fim, entra C; A ganha de novo, C vai ao fim, entra D.

**Reposição de vaga**

- Se alguém sai no meio da partida, **qualquer pessoa que esteja descansando** pode substituí-lo no momento, sem sair do próprio time.
- Quando a partida acaba, quem entrou volta ao seu time original.
- "Time que não está jogando" é qualquer time a partir da **posição 3** da fila.

**Clima e chuva**

O sistema avalia a previsão para o **período da pelada** (do início ao fim). Peladas que não começam ou terminam em hora cheia são tratadas como se terminassem em `:00`, e todas as horas que ela toca são avaliadas.

| Situação | Critério |
| --- | --- |
| **Alerta de chuva** | Probabilidade de chuva de **25% ou mais** em alguma hora do período, qualquer que seja o volume (inclusive chuva fraca) |
| **Sugestão de remarcar** | Probabilidade de **50% ou mais**, com pelo menos **30 minutos** de chuva durante a pelada |
| **Sugestão de remarcar (chuva forte)** | Chuva forte com probabilidade de **40% ou mais**, mesmo durando menos de 30 minutos |

- A estimativa dos 30 minutos usa o intervalo padrão da API (1 hora). Uma "hora com chuva" é definida pela **chance de chover**, não pelo volume.
- A nova data sugerida pode ser no mesmo dia, 1 hora antes ou depois da chuva.
- O alerta aparece dentro do sistema para todos os envolvidos.
- Quando não há previsão (inclusive se a API estiver fora do ar), o sistema mostra **"sem previsão definida"**.
- Outras condições climáticas são apenas observações.

Escala de intensidade de chuva usada como referência:

| Intensidade | Volume |
| --- | --- |
| Fraca | menos de 2,5 mm/h |
| Moderada | de 2,5 mm/h até menos de 7,6 mm/h |
| Forte | 7,6 mm/h ou mais (sem teto) |

### Requisitos não funcionais

- Usabilidade: O sistema deverá possuir uma interface simples e fácil de utilizar.
- Segurança: O sistema deverá proteger os dados dos usuários e exigir autenticação para acessar funcionalidades restritas.
- Desempenho: As páginas principais deverão carregar em até 3 segundos em condições normais de uso.
- Compatibilidade: O sistema deverá funcionar nos principais navegadores modernos.
- Manutenibilidade: O código deverá ser organizado para facilitar correções e melhorias.
- Confiabilidade: O sistema deverá validar os dados informados e tratar erros sem interromper indevidamente seu funcionamento.
- Tecnologias: O sistema deverá ser desenvolvido utilizando Python e Django.
- Integração: Se a API meteorológica estiver indisponível, o sistema deverá informar que não foi possível consultar a previsão.
- Banco de dados: O sistema deverá utilizar SQLite no ambiente inicial de desenvolvimento.
- RNF10 — Proteção de dados: Senhas, credenciais e arquivos de configuração sensíveis não deverão ser publicados no repositório.


---

## 3. Demonstração

O projeto está na **Etapa 1** (documentação e arquitetura). Capturas de tela e vídeo serão adicionados após a implementação, na Etapa 2.

**Vídeo / protótipo:** [a definir]

---

## 4. Tecnologias utilizadas

| Camada | Tecnologia | Versão |
| --- | --- | --- |
| Linguagem | Python | 3.14 |
| Backend | Django | [a definir] |
| Frontend | HTML, CSS | [a definir] |
| Banco de dados | SQLite | [a definir] |
| API externa | Open-Meteo (previsão do clima) | — |
| Testes | pytest | [a definir] |
| Outras ferramentas | Git, GitHub, VS Code | — |

---

## 5. Arquitetura

[A descrever na Etapa 1: camadas, principais componentes e fluxo entre eles. Incluir o diagrama em `docs/` e descrevê-lo aqui.]

```text
[Usuário] → [Interface / Frontend] → [Django / Backend] → [Banco de dados]
                                            ↓
                                      [API Open-Meteo]
```

**Decisões relevantes:**

- Uso da API **Open-Meteo** para a previsão do clima.
- As regras de chuva, vagas e fila de times são tratadas como regras de negócio do backend (ver seção 2).

### Endpoints principais

[A definir na Etapa 2.]

---

## 6. Organização dos diretórios


## 6. Organização dos diretórios

```text
.
├── README.md
├── decisoes.md
└── docs/
    ├── api/
    │   └── api.md
    ├── modelagem/
    │   ├── arquitetura/
    │   │   └── Arquitetura_do_Sistema.pdf
    │   ├── banco-de-dados/
    │   │   └── Modelo_de_Dados_Futup.pdf
    │   ├── casos-de-uso/
    │   │   └── Casos_de_Uso_Futup_2.pdf
    │   └── classes/
    │       └── diagrama-de-classes.pdf
    ├── planejamento/
    │   └── Planejamento_Futup.pdf
    ├── plano de integração externa/
    │   └── Plano_de_Integracao_Ext.pdf
    ├── prototipo/
    │   └── figma.md
    └── visao/
        └── visao.md
```

| Diretório / arquivo | Função |
| --- | --- |
| "README.md" |Apresentação geral do projeto, objetivos, visão geral e instruções
| "decisoes.md" |Registro de decisões de arquitetura, projeto e escopo
| "docs/" |Pasta raiz de toda a documentação do projeto
| "docs/api/" |Documentação das rotas, endpoints e contratos da API (api.md)
| "docs/modelagem/" |Artefatos de modelagem do sistema em formato PDF
| "docs/modelagem/arquitetura/" |Diagrama e documento da arquitetura do sistema
| "docs/modelagem/banco-de-dados/" |Modelo de dados e modelo lógico/ER do banco de dados
| "docs/modelagem/casos-de-uso/" | Especificações e diagramas dos casos de uso
| "docs/modelagem/classes/" | Diagrama de classes do sistema
| "docs/planejamento/" | Documentação sobre planejamento, cronograma e fases do projeto
| "docs/plano de integração externa/" | Documentação de integrações com serviços externos (ex.: API de clima)
| "docs/prototipo/" | Links e especificações do protótipo no Figma (figma.md)
| "docs/visao/" | Documento de visão do produto (visao.md)
---

## 7. Participantes

| Nome | Matrícula | Função no projeto |
| --- | --- | --- |
| Joao Maia Reis      | 22503526      | Desenvolvedor |
| Pedro Maia Reis     | 22503381      | Desenvolvedor |
| Pedro Barbosa Souza | 22503939      | Desenvolvedor |

**Professor(a) responsável:** Felippe Pires Ferreira

---

## 8. Como executar

> A preencher quando a aplicação estiver implementada (Etapa 2).

### Pré-requisitos

- Git
- Python 3,14
- [a definir o resto ]


### Instalação e execução

```bash
# 1. Clonar o repositório
git clone [URL_DO_REPOSITORIO]
cd futup

# 2. Instalar dependências
[comando de instalação]

# 3. Configurar variáveis de ambiente
cp .env.example .env
# edite o arquivo .env com as credenciais locais

# 4. Executar a aplicação
[comando de execução]
```

**Acesso local:** [a definir]

### Implantação

- **Ambiente:** [a definir na Etapa 2]
- **URL de produção:** [a definir]

---

## 9. Configuração

[Listar as variáveis de ambiente quando forem definidas.]

| Variável | Obrigatória | Descrição | Exemplo |
| --- | --- | --- | --- |
| [a definir] | [Sim/Não] | [descrição] | [exemplo] |

Credenciais reais devem ficar apenas no arquivo `.env` (não versionado).

---

## 10. Testes

[A definir na Etapa 2.]

| Tipo | Ferramenta | O que verifica |
| --- | --- | --- |
| Unitários | [a definir] | Regras de negócio (vagas, fila de times, alertas de chuva) |
| Integração | [a definir] | [a definir] |
| Manuais | [a definir] | Fluxos principais da interface |

**Cobertura atual:** não medida

---

## 11. Uso de inteligência artificial

Este repositório segue a política de uso de IA da disciplina (semáforo pedagógico):

![Política de uso de IA — semáforo](images/semaforo.png)

| Situação | Significado |
| --- | --- |
| **Vermelho — uso proibido** | Atividades de autonomia intelectual (ex.: provas presenciais sem consulta). |
| **Amarelo — uso limitado** | IA pode ser ferramenta auxiliar, desde que haja declaração de uso. |
| **Verde — uso permitido** | Uso livre ao longo da atividade acadêmica. |

### Declaração de uso

- **Houve uso de IA neste projeto?** Sim
- **Ferramentas utilizadas:** Claude (Anthropic), chat gpt e gemini
- **Finalidade:** apoio na discussão e refinamento das regras de negócio e na redação do trabalho
- **O que NÃO foi delegado à IA:** escolha do tema, decisões finais sobre as regras, testes da API Open-Meteo, desisoes 

---

## 12. Contribuição e fluxo de trabalho

### Branches

- `main` — versão estável para avaliação
- `develop` — integração do grupo *(opcional)*
- `feat/[nome]` — nova funcionalidade
- `fix/[nome]` — correção de defeito
- `docs/[nome]` — alterações só de documentação

### Commits

Use mensagens curtas e no imperativo, por exemplo:

- `feat: adiciona cadastro de peladas`
- `fix: corrige validação de data`
- `docs: atualiza instruções de execução`

### Passos sugeridos

1. Criar uma branch a partir de `main`.
2. Implementar e testar localmente.
3. Abrir um *pull request* para revisão do grupo.
4. Só então integrar à branch principal.

**Issues e quadro de tarefas:** [link a definir]

---

## 13. Histórico de versões

| Versão | Data | Descrição |
| --- | --- | --- |
| `0.1.0` | [2026-10-08] | Documentação e arquitetura (Etapa 1) |
| `0.0.1` | [2026-10-04] | Estrutura inicial do repositório |

---

## 14. Limitações e próximos passos

### Problemas conhecidos

- Projeto ainda em fase de documentação; nenhuma funcionalidade implementada.

### Roadmap

- [x] Definição do tema, do problema e das regras de negócio
- [x] Documentação e arquitetura (Etapa 1)
- [ ] Implementação em Python + Django (Etapa 2)
- [ ] Publicação da aplicação
- [ ] Análises SAST/DAST
- [ ] Apresentação final

---

## 15. Licença, referências e contato

**Licença:** uso exclusivamente acadêmico

Este material destina-se a fins educacionais. Verifique com a disciplina se o código pode ser reutilizado fora do curso.

### Documentação complementar

- Índice da pasta `docs/`: [`docs/README.pdf`](docs/README.pdf)
- Casos de uso (diagrama + especificações): [`docs/modelagem/casos-de-uso/especificacoes-casos-de-uso.pdf`](docs/modelagem/casos-de-uso/especificacoes-casos-de-uso.pdf)
- Diagrama de classes: [`docs/modelagem/classes/diagrama-de-classes.pdf`](docs/modelagem/classes/diagrama-de-classes.pdf)
- Modelo conceitual (ER): [`docs/modelagem/banco-de-dados/diagrama-er.pdf`](docs/modelagem/banco-de-dados/diagrama-er.pdf)
- Modelo lógico: [`docs/modelagem/banco-de-dados/modelo-logico.pdf`](docs/modelagem/banco-de-dados/modelo-logico.pdf)
- Apresentação: [`docs/apresentacao.pdf`](docs/)

### Referências

- Open-Meteo. Documentação da API de previsão do tempo.
- Brasil Escola e Wikipedia. Classificação da intensidade da chuva (mm/h), usada como referência para a escala do projeto.

### Contato

Dúvidas sobre o projeto contatar: na sala de aula (grande chances de n olharmos o gmail): pedro.maiaires@sempreceub.com


