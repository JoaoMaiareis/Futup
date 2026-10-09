# Contrato Inicial da API — Futup

**Data:** 09/10/2026
**Integrantes:** João Maia, Pedro Maia e Pedro Barbosa
**Versão:** 1.0

## 1. Objetivo

A API do Futup será responsável pela comunicação entre o site e o servidor. Por meio dela, os usuários poderão criar contas, organizar peladas, entrar em times, registrar resultados, cadastrar quadras e consultar a previsão do tempo.

O sistema será desenvolvido em Python com Django. Os endpoints apresentados neste documento são os que pretendemos implementar durante o desenvolvimento.

## 2. Padrão da API

A API utilizará JSON para enviar e receber dados. As rotas terão como endereço inicial `/api/v1/`.

Os principais métodos HTTP serão:

* **GET:** consultar informações.
* **POST:** criar registros ou executar ações.
* **PATCH:** alterar informações.
* **DELETE:** excluir registros ou sair de uma pelada.

As rotas que exigem login receberão um token no cabeçalho da requisição:

```http
Authorization: Bearer <token>
```

O método de autenticação ainda será definido durante a implementação.

## 3. Usuários

| Endpoint               | Método | Função                      | Autenticação |
| ---------------------- | ------ | --------------------------- | ------------ |
| `/api/v1/usuarios/`    | POST   | Criar uma conta             | Não          |
| `/api/v1/auth/login/`  | POST   | Entrar no sistema           | Não          |
| `/api/v1/auth/logout/` | POST   | Sair da conta               | Sim          |
| `/api/v1/usuarios/me/` | GET    | Consultar os próprios dados | Sim          |

### 3.1. Cadastro

**Endpoint:** `POST /api/v1/usuarios/`

O usuário deverá informar nome, e-mail e senha.

Exemplo de requisição:

```json
{
  "nome": "João Maia",
  "email": "joao@email.com",
  "senha": "SenhaForte123"
}
```

Resposta esperada — `201 Created`:

```json
{
  "id": 1,
  "nome": "João Maia",
  "email": "joao@email.com"
}
```

A senha não será enviada na resposta.

### 3.2. Login

**Endpoint:** `POST /api/v1/auth/login/`

O usuário informará seu e-mail e sua senha.

Exemplo de requisição:

```json
{
  "email": "joao@email.com",
  "senha": "SenhaForte123"
}
```

Resposta esperada — `200 OK`:

```json
{
  "token": "exemplo_de_token",
  "usuario": {
    "id": 1,
    "nome": "João Maia"
  }
}
```

O token será utilizado para acessar as rotas que exigem autenticação.

## 4. Peladas

| Endpoint                                 | Método | Função               | Autenticação |
| ---------------------------------------- | ------ | -------------------- | ------------ |
| `/api/v1/peladas/`                       | GET    | Listar peladas       | Não          |
| `/api/v1/peladas/`                       | POST   | Criar uma pelada     | Sim          |
| `/api/v1/peladas/{id}/`                  | GET    | Consultar uma pelada | Não          |
| `/api/v1/peladas/{id}/`                  | PATCH  | Alterar uma pelada   | Organizador  |
| `/api/v1/peladas/{id}/participantes/`    | GET    | Listar participantes | Sim          |
| `/api/v1/peladas/{id}/participantes/`    | POST   | Entrar em uma pelada | Sim          |
| `/api/v1/peladas/{id}/participantes/me/` | DELETE | Sair de uma pelada   | Sim          |
| `/api/v1/peladas/{id}/cancelar/`         | POST   | Cancelar uma pelada  | Organizador  |

### 4.1. Criar uma pelada

**Endpoint:** `POST /api/v1/peladas/`

O usuário deverá informar o nome, a modalidade, a data, o horário, a quadra e o modo de organização dos times.

Exemplo de requisição:

```json
{
  "nome": "Pelada de sábado",
  "modalidade": "futsal",
  "data_hora": "2026-10-10T16:00:00-03:00",
  "quadra_id": 1,
  "tipo_times": "sorteio",
  "max_times": 5
}
```

Resposta esperada — `201 Created`:

```json
{
  "id": 10,
  "nome": "Pelada de sábado",
  "modalidade": "futsal",
  "status": "aberta",
  "organizador_id": 1
}
```

O sistema deverá respeitar os limites de cada modalidade: 5 jogadores por time no futsal, 7 no campo sintético, 11 no futebol de campo e 2 no futevôlei.

Cada pelada poderá ter até 5 times, com exceção do futevôlei, que poderá ter até 7.

### 4.2. Entrar em uma pelada

**Endpoint:** `POST /api/v1/peladas/{id}/participantes/`

O jogador poderá informar o time em que deseja entrar, caso essa opção esteja disponível.

Exemplo de requisição:

```json
{
  "time_id": 3
}
```

O campo `time_id` poderá ser omitido quando o sistema precisar escolher o time automaticamente.

Resposta esperada — `201 Created`:

```json
{
  "mensagem": "Participação confirmada.",
  "pelada_id": 10,
  "usuario_id": 2,
  "time_id": 3
}
```

O sistema deverá verificar se ainda existem vagas antes de confirmar a participação.

## 5. Times

| Endpoint                                         | Método | Função                   | Autenticação |
| ------------------------------------------------ | ------ | ------------------------ | ------------ |
| `/api/v1/peladas/{id}/times/`                    | GET    | Listar os times          | Sim          |
| `/api/v1/peladas/{id}/times/`                    | POST   | Criar um time            | Organizador  |
| `/api/v1/times/{id}/participantes/`              | GET    | Listar jogadores do time | Sim          |
| `/api/v1/times/{id}/participantes/{usuario_id}/` | PATCH  | Mudar um jogador de time | Organizador  |
| `/api/v1/peladas/{id}/times/sortear/`            | POST   | Sortear os jogadores     | Organizador  |

Os times serão identificados por letras, começando pelo A. Na criação da pelada, o organizador poderá escolher entre o sorteio automático e a organização livre.

Quando houver sorteio, o sistema distribuirá os jogadores aleatoriamente, considerando apenas os times que ainda tiverem vagas.

Exemplo de requisição para criar um time:

```json
{
  "nome": "Time A"
}
```

Resposta esperada — `201 Created`:

```json
{
  "id": 3,
  "nome": "Time A",
  "pelada_id": 10,
  "quantidade_jogadores": 0
}
```

## 6. Partidas e resultados

| Endpoint                           | Método | Função                | Autenticação |
| ---------------------------------- | ------ | --------------------- | ------------ |
| `/api/v1/peladas/{id}/fila/`       | GET    | Consultar a fila      | Sim          |
| `/api/v1/peladas/{id}/partidas/`   | GET    | Listar partidas       | Sim          |
| `/api/v1/peladas/{id}/partidas/`   | POST   | Registrar uma partida | Organizador  |
| `/api/v1/partidas/{id}/resultado/` | PATCH  | Registrar o resultado | Organizador  |

A fila começará pela ordem de criação dos times. Os dois primeiros times jogarão, e o organizador informará o resultado.

Exemplo de requisição:

```json
{
  "gols_time_a": 3,
  "gols_time_b": 2
}
```

Resposta esperada — `200 OK`:

```json
{
  "partida_id": 25,
  "time_a": "A",
  "time_b": "B",
  "gols_time_a": 3,
  "gols_time_b": 2,
  "vencedor": "A",
  "status": "finalizada"
}
```

Depois do resultado, o sistema atualizará a fila:

* O vencedor continuará jogando.
* O perdedor irá para o final da fila.
* Em caso de empate, os dois times irão para o final, em ordem alfabética.

## 7. Quadras

| Endpoint                | Método | Função               | Autenticação              |
| ----------------------- | ------ | -------------------- | ------------------------- |
| `/api/v1/quadras/`      | GET    | Listar quadras       | Não                       |
| `/api/v1/quadras/`      | POST   | Cadastrar uma quadra | Sim                       |
| `/api/v1/quadras/{id}/` | GET    | Consultar uma quadra | Não                       |
| `/api/v1/quadras/{id}/` | PATCH  | Alterar uma quadra   | Responsável pelo cadastro |

Para cadastrar uma quadra, serão informados o nome, a cidade, o endereço, as coordenadas e a modalidade.

Exemplo de requisição:

```json
{
  "nome": "Quadra Central",
  "cidade": "Brasília",
  "endereco": "Rua das Flores, 100",
  "latitude": -15.7801,
  "longitude": -47.9292,
  "modalidade": "futsal"
}
```

Resposta esperada — `201 Created`:

```json
{
  "id": 1,
  "nome": "Quadra Central",
  "cidade": "Brasília",
  "modalidade": "futsal"
}
```

Para evitar cadastros repetidos, o sistema verificará o endereço e a proximidade das coordenadas geográficas. A distância utilizada nessa verificação ainda será definida.

## 8. Previsão do tempo

| Endpoint                                        | Método | Função                                | Autenticação |
| ----------------------------------------------- | ------ | ------------------------------------- | ------------ |
| `/api/v1/peladas/{id}/clima/`                   | GET    | Consultar a previsão da pelada        | Sim          |
| `/api/v1/peladas/{id}/sugestoes-reagendamento/` | GET    | Consultar sugestões de novos horários | Sim          |

A previsão será consultada pela API Open-Meteo, utilizando as coordenadas da quadra e o horário da pelada. O backend fará essa consulta e enviará o resultado ao frontend.

Exemplo de resposta:

```json
{
  "pelada_id": 10,
  "chance_chuva": 55,
  "volume_chuva_mm_h": 2.0,
  "alerta": true,
  "sugerir_reagendamento": true,
  "mensagem": "Há possibilidade de chuva. Considere remarcar a pelada."
}
```

Os valores são apenas exemplos.

As regras definidas inicialmente são:

* A partir de 25% de chance de chuva, o sistema mostrará um alerta.
* A partir de 50%, mostrará um alerta e sugerirá remarcar.
* Em caso de chuva forte com pelo menos 40% de chance, também sugerirá remarcar.
* Se não houver previsão, informará que não há previsão definida.

O limite de volume de chuva forte ainda precisa ser confirmado pelo grupo.

## 9. Códigos de resposta HTTP

| Código                      | Significado                     | Exemplo de uso                          |
| --------------------------- | ------------------------------- | --------------------------------------- |
| `200 OK`                    | Operação realizada              | Consulta ou atualização concluída       |
| `201 Created`               | Registro criado                 | Cadastro de usuário ou pelada           |
| `204 No Content`            | Operação concluída sem resposta | Saída de uma pelada                     |
| `400 Bad Request`           | Dados incorretos                | Campos obrigatórios ausentes            |
| `401 Unauthorized`          | Usuário não autenticado         | Token inválido ou ausente               |
| `403 Forbidden`             | Sem permissão                   | Jogador tentando cancelar uma pelada    |
| `404 Not Found`             | Registro não encontrado         | Pelada inexistente                      |
| `409 Conflict`              | Conflito com uma regra          | Tentativa de entrar em uma pelada cheia |
| `500 Internal Server Error` | Erro no servidor                | Falha inesperada                        |

Quando ocorrer um erro, a API retornará uma mensagem em JSON explicando o problema.

Exemplo:

```json
{
  "erro": "Pelada cheia",
  "detalhes": "Não há mais vagas disponíveis."
}
```

## 10. Autenticação e permissões

Algumas operações poderão ser realizadas sem login, como consultar peladas públicas e quadras. Para participar de uma pelada ou alterar informações, o usuário precisará estar autenticado.

O organizador terá permissões adicionais para cancelar peladas, remover participantes, alterar times e registrar resultados. Essas permissões serão verificadas pelo servidor.

## 11. O que falta definir

* Qual método de autenticação será utilizado.
* Como funcionará a escolha dos times.
* Como será feita a substituição de jogadores.
* Qual distância será considerada para identificar quadras duplicadas.
* Qual será o limite de chuva forte.
* Como o sistema reagirá quando a API de clima estiver indisponível.
* Qual banco de dados será utilizado na publicação do sistema.

## 12. Considerações finais

Este contrato apresenta as rotas planejadas para a primeira versão do Futup. Durante o desenvolvimento, alguns endpoints e campos poderão ser ajustados conforme as necessidades encontradas. As mudanças deverão ser registradas para manter a documentação atualizada com o sistema implementado.
