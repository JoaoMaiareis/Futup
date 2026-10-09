# Registro de Decisões — Futup

###  Tecnologias utilizadas


* **Decisão:** O sistema será desenvolvido em Python com Django. Durante os testes, será utilizado SQLite, e a previsão do tempo será consultada pela API Open-Meteo.
* **Motivo:** Essas tecnologias atendem às necessidades iniciais do projeto e facilitam o desenvolvimento e os testes.
* **Alternativas consideradas:** Outras linguagens, frameworks e bancos de dados.
* **Documentos afetados:** README, arquitetura, requisitos e plano de integração externa.

### Papéis dos usuários


* **Decisão:** O usuário poderá ser organizador em uma pelada e jogador em outra. O organizador poderá remover participantes, cancelar a pelada e mudar jogadores de time.
* **Motivo:** Isso permite que uma mesma pessoa organize algumas peladas e participe de outras.
* **Alternativas consideradas:** Ter um papel fixo para cada conta.
* **Documentos afetados:** Documento de Visão, casos de uso e modelo de dados.

### Tipos de pelada e quantidade de jogadores


* **Decisão:** O sistema terá futsal, campo sintético, futebol de campo e futevôlei. Cada time terá, respectivamente, 5, 7, 11 e 2 jogadores. Serão permitidos até 5 times por pelada, exceto no futevôlei, que poderá ter até 7.
* **Motivo:** As regras permitem organizar diferentes modalidades esportivas.
* **Alternativas consideradas:** Utilizar a mesma quantidade de jogadores e o mesmo limite de times para todas as modalidades.
* **Documentos afetados:** Requisitos, casos de uso, modelo de dados e README.

### Organização da fila de partidas


* **Decisão:** A fila começará pela ordem de criação dos times. Os dois primeiros jogarão. O vencedor continuará na partida seguinte, o perdedor irá para o final da fila e, em caso de empate, os dois times irão para o final em ordem alfabética.
* **Motivo:** A regra facilita a organização das partidas e define como os times devem se alternar.
* **Alternativas consideradas:** Sortear os confrontos a cada partida.
* **Documentos afetados:** Casos de uso, modelo de dados e requisitos funcionais.

### Consulta à previsão do tempo


* **Decisão:** O sistema utilizará a API Open-Meteo para consultar a previsão do tempo. A partir de 25% de chance de chuva, será exibido um alerta. A partir de 50%, será sugerido o reagendamento. Em caso de chuva forte com pelo menos 40% de chance, também será sugerido o reagendamento.
* **Motivo:** A previsão ajuda os participantes a se prepararem e a decidirem se precisam mudar o horário da pelada.
* **Alternativas consideradas:** Não consultar o clima ou utilizar outro serviço de previsão.
* **Documentos afetados:** Requisitos, arquitetura e plano de integração externa.


### Formação dos times


* **Decisão:** Ao criar uma pelada, o organizador poderá escolher entre times sorteados e times definidos livremente. No sorteio, o sistema escolherá aleatoriamente entre os times que ainda tiverem vagas.
* **Motivo:** Isso permite que o grupo escolha como prefere organizar os participantes.
* **Alternativas consideradas:** Utilizar somente sorteio automático ou somente organização manual.
* **Documentos afetados:** Requisitos funcionais, casos de uso e modelo de dados.
* **Pendente:** Definir como o jogador poderá escolher um time específico ou deixar o sistema escolher.

### Reposição de jogadores


* **Decisão:** Um jogador poderá ser substituído durante uma partida. A pessoa que entrar continuará pertencendo ao seu time original e retornará a ele após o término da partida.
* **Motivo:** Isso permite manter a partida em andamento quando alguém precisar sair.
* **Alternativas consideradas:** Interromper a partida até que o jogador retorne ou encerrar o jogo.
* **Documentos afetados:** Requisitos funcionais, casos de uso e modelo de dados.
* **Pendente:** Definir como o substituto será escolhido, de onde virá e como sua participação será registrada no resultado.

### D-009: Cadastro de quadras


* **Decisão:** Os usuários poderão cadastrar quadras com cidade, endereço e coordenadas geográficas. Cada quadra será associada a uma modalidade esportiva.
* **Motivo:** Isso facilita encontrar locais adequados para cada tipo de pelada e evita cadastros repetidos.
* **Alternativas consideradas:** Permitir apenas locais previamente cadastrados pela equipe do sistema.
* **Documentos afetados:** Requisitos, casos de uso e modelo de dados.
* **Pendente:** Definir a distância utilizada para identificar quadras duplicadas.

### Funcionalidades da primeira versão


* **Status:** Provisório
* **Decisão:** A primeira versão será focada na criação de peladas, organização dos jogadores e times, fila de partidas, registro de resultados, cadastro de quadras e consulta à previsão do tempo.
* **Motivo:** Priorizar as funções principais ajuda a manter o projeto dentro do prazo da disciplina.
* **Alternativas consideradas:** Implementar funcionalidades adicionais já na primeira versão.
* **Documentos afetados:** Documento de Visão, requisitos, planejamento e README.
* **Pendente:** Definir quais funcionalidades ficarão explicitamente fora do escopo.


