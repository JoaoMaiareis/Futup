DOCUMENTO DE VISÃO — FUTUP

Disciplina: Desenvolvimento Web — Projeto Python + Django
Projeto: Futup
Instituição: CEUB
Curso: Ciência da Computação
Alunos: Pedro Barbosa Souza, Pedro Maia Reis
Professor: Felippe Pires Ferreira
Versão do documento: 1.0
Status: Versão inicial
Data: Outubro de 2026

1. Introdução

O Futup é um sistema web destinado à organização e ao gerenciamento de partidas de futebol recreativo, conhecidas como peladas. A aplicação busca centralizar informações sobre partidas, participantes, quadras, equipes, filas de jogos e condições climáticas, facilitando a organização dos encontros esportivos.

O sistema pretende oferecer aos organizadores ferramentas para cadastrar partidas, distribuir jogadores entre equipes, acompanhar a disponibilidade de vagas e administrar a sequência dos jogos. Para os participantes, a aplicação deverá facilitar a consulta às peladas disponíveis, a entrada nas partidas e o recebimento de avisos relacionados às condições climáticas.

O projeto será desenvolvido utilizando Python e Django, conforme a proposta da disciplina de Desenvolvimento Web.

2. Descrição do problema

A organização de peladas pode envolver diferentes tarefas, como reunir participantes, controlar o número de jogadores, distribuir pessoas entre equipes, administrar a ordem das partidas e informar alterações aos envolvidos.

Quando essas atividades são realizadas de maneira descentralizada, o organizador pode ter dificuldade para acompanhar as vagas disponíveis, manter uma sequência de jogos organizada e comunicar mudanças aos participantes. As condições climáticas também podem interferir na realização das partidas, especialmente quando há previsão de chuva.

O Futup propõe reunir essas atividades em um único sistema web, oferecendo recursos para organizar as partidas e apoiar a tomada de decisões relacionadas à sua realização.

3. Visão do produto

A visão do Futup é disponibilizar uma plataforma web que simplifique a organização de peladas, permitindo que organizadores e participantes acompanhem as informações das partidas em um ambiente centralizado.

A aplicação deverá contemplar diferentes modalidades esportivas, respeitar a capacidade de cada equipe, facilitar a distribuição dos jogadores e organizar a sequência das partidas. Além disso, deverá consultar previsões meteorológicas para identificar condições que possam afetar os jogos e emitir alertas aos usuários envolvidos.

O sistema pretende contribuir para uma organização mais clara e previsível das partidas, reduzindo o esforço manual necessário para administrar participantes, equipes e filas de jogos.

4. Objetivos
4.1. Objetivo geral

Desenvolver uma aplicação web para organizar e gerenciar peladas, integrando o controle de participantes, equipes, quadras, sequência de partidas e alertas meteorológicos.

4.2. Objetivos específicos
Permitir o cadastro e a autenticação de usuários.
Permitir a criação e o gerenciamento de peladas.
Possibilitar a participação dos usuários nas partidas que tenham vagas disponíveis.
Controlar a capacidade das equipes de acordo com a modalidade esportiva.
Auxiliar na distribuição dos participantes entre as equipes.
Organizar a fila de equipes para a realização das partidas.
Permitir o registro dos resultados e a atualização da ordem dos jogos.
Disponibilizar o cadastro e a consulta de quadras esportivas.
Consultar previsões meteorológicas para o período das partidas.
Emitir alertas quando as condições climáticas atenderem às regras estabelecidas pelo sistema.
5. Público-alvo e usuários
5.1. Participantes

São os usuários interessados em encontrar e participar de peladas. Deverão poder consultar as partidas disponíveis, verificar informações relevantes e solicitar participação quando houver vagas.

5.2. Organizadores

São usuários responsáveis pela administração de uma determinada pelada. O organizador poderá criar e cancelar a partida, gerenciar participantes, alterar a distribuição das equipes, registrar resultados e administrar a sequência dos jogos.

Um usuário poderá atuar como organizador em uma pelada e como participante em outra, de acordo com suas permissões em cada partida.

5.3. Usuários autenticados

O sistema será destinado a usuários cadastrados e autenticados. As permissões de gerenciamento serão determinadas conforme a função desempenhada pelo usuário em cada pelada.

6. Escopo do sistema
6.1. Funcionalidades previstas

O escopo inicial do Futup contempla os seguintes grupos de funcionalidades.

a) Cadastro e autenticação
Cadastro de usuários.
Autenticação para acesso ao sistema.
Identificação do usuário responsável por organizar uma pelada.
b) Gerenciamento de peladas
Criação e cancelamento de partidas.
Consulta das peladas disponíveis.
Definição da modalidade, horário, duração e local da partida.
Controle da quantidade de participantes e das vagas disponíveis.
Identificação das partidas que atingiram sua capacidade máxima.
c) Gerenciamento de participantes e equipes
Entrada de participantes em peladas públicas.
Distribuição dos jogadores entre as equipes.
Sorteio de equipes quando essa opção for escolhida pelo organizador.
Alteração manual da distribuição dos participantes pelo organizador.
Controle de substituições durante as partidas.
d) Gerenciamento da fila de jogos
Definição da ordem inicial das equipes.
Identificação das equipes que estão jogando e das que aguardam.
Registro do resultado de cada partida.
Atualização automática da fila conforme as regras definidas.
e) Gerenciamento de quadras
Cadastro e consulta de quadras esportivas.
Registro de endereço, cidade e coordenadas geográficas.
Associação de uma modalidade esportiva a cada quadra.
Verificação de possíveis cadastros duplicados.
f) Integração meteorológica
Consulta de previsões por meio da API Open-Meteo.
Avaliação das condições climáticas durante o período previsto para a partida.
Geração de alertas conforme as regras de probabilidade e intensidade da chuva.
Apresentação de sugestões de remarcação quando aplicável.
g) Notificações
Apresentação de alertas meteorológicos dentro da aplicação aos usuários envolvidos na partida.
6.2. Fora do escopo inicial

Os seguintes recursos não fazem parte do escopo inicial definido até o momento, salvo decisão posterior do grupo:

Pagamentos e cobranças pela participação nas peladas.
Sistema de reservas comerciais de quadras.
Aplicativos móveis nativos independentes da aplicação web.
Integração com serviços pagos de previsão meteorológica.
Garantia de que uma partida será realizada ou cancelada automaticamente com base apenas na previsão do tempo.

Essas delimitações ajudam a manter o foco do projeto nas funcionalidades principais de organização das partidas.

7. Regras gerais do negócio

As funcionalidades do Futup deverão respeitar as regras gerais descritas a seguir.

7.1. Modalidades esportivas

O sistema deverá suportar as seguintes modalidades e capacidades por equipe:

Modalidade	Jogadores por equipe
Futsal	5
Futebol sintético	7
Futebol de campo	11
Futevôlei	2

O limite inicial será de cinco equipes por pelada, com exceção do futevôlei, que poderá ter até sete equipes.

A capacidade total da partida será determinada pela quantidade de equipes permitidas e pelo número de jogadores por equipe.

7.2. Disponibilidade e participação

As peladas serão públicas para consulta e participação de usuários autenticados, respeitando as regras de capacidade e disponibilidade.

A partida será considerada cheia quando todas as vagas previstas estiverem ocupadas. O sistema deverá permitir a organização de partidas com menos de duas equipes completas, embora a realização com pelo menos duas equipes completas seja considerada a situação ideal.

7.3. Distribuição dos jogadores

As equipes serão identificadas por letras em ordem alfabética. O organizador poderá escolher entre a distribuição aleatória e a distribuição livre dos participantes.

Quando o sorteio for utilizado, o sistema deverá considerar apenas as equipes que ainda possuírem vagas. O organizador poderá realizar alterações na distribuição dos jogadores.

7.4. Fila de partidas

A fila inicial seguirá a ordem de criação das equipes. As duas primeiras equipes disputarão a partida inicial.

Após o registro do resultado, a equipe vencedora permanecerá para jogar novamente, enquanto a equipe perdedora será encaminhada ao final da fila. Em caso de empate, as duas equipes serão encaminhadas ao final, seguindo a ordem alfabética definida para o sistema.

O organizador será responsável por registrar o resultado, e o sistema deverá atualizar a fila de acordo com essas regras.

7.5. Substituições durante a partida

Quando um jogador precisar sair durante uma partida, uma pessoa que esteja aguardando poderá substituí-lo temporariamente. Ao término da partida, a pessoa que realizou a substituição deverá retornar à sua equipe original.

O tratamento de situações específicas, como a saída definitiva de um jogador da pelada ou a ausência de pessoas aguardando, deverá ser detalhado nas especificações dos casos de uso e nas regras operacionais.

7.6. Cadastro de quadras

Cada quadra cadastrada deverá estar associada a uma modalidade esportiva e possuir informações de localização.

O sistema deverá verificar possíveis duplicidades considerando a proximidade das coordenadas geográficas e a correspondência do endereço e da cidade.

8. Requisitos de qualidade e restrições
8.1 Usabilidade

 O sistema deverá possuir uma interface simples e fácil de utilizar.
 
8.2 Segurança

 O sistema deverá proteger os dados dos usuários e exigir autenticação para acessar funcionalidades restritas.

8.3 Desempenho

 As páginas principais deverão carregar em até 3 segundos em condições normais de uso.

8.4 Compatibilidade

 O sistema deverá funcionar nos principais navegadores modernos.

8.5 Manutenibilidade

 O código deverá ser organizado para facilitar correções e melhorias.

8.6 Confiabilidade

 O sistema deverá validar os dados informados e tratar erros sem interromper indevidamente seu funcionamento.

8.7 Tecnologias

 O sistema deverá ser desenvolvido utilizando Python e Django.

8.8 Integração

 Se a API meteorológica estiver indisponível, o sistema deverá informar que não foi possível consultar a previsão.

8.9 Banco de dados

 O sistema deverá utilizar SQLite no ambiente inicial de desenvolvimento.

8.10 Proteção de dados

 Senhas, credenciais e arquivos de configuração sensíveis não deverão ser publicados no repositório.



9. Integração com serviços externos
9.1. API meteorológica Open-Meteo

O Futup utilizará a API Open-Meteo para obter dados de previsão meteorológica, sem depender inicialmente de uma chave de API paga.

A aplicação deverá avaliar as previsões correspondentes às horas abrangidas pela duração da partida. Os dados serão utilizados para identificar situações que atendam às regras de alerta e de recomendação de remarcação.

A integração deverá prever a ausência de dados e falhas de comunicação, apresentando uma indicação de indisponibilidade da previsão em vez de gerar conclusões sem suporte nos dados recebidos.

9.2. Regras preliminares para alertas climáticos

As regras registradas na versão atual da documentação do projeto estabelecem:

Emitir um alerta quando a probabilidade de chuva atingir pelo menos 25% em uma das horas avaliadas.
Sugerir remarcação quando a probabilidade atingir pelo menos 50% e a condição de duração mínima de chuva for atendida.
Sugerir remarcação em condições de chuva forte quando a probabilidade atingir pelo menos 40%, mesmo quando a duração mínima não for atingida.

A versão atual do README utiliza o valor de 7,6 mm/h como início da faixa de chuva forte. O grupo deverá confirmar se esse limite é definitivo e garantir que a interpretação da duração da chuva e dos dados horários seja consistente em toda a documentação.

Os alertas deverão ser apresentados na aplicação aos usuários envolvidos na partida. A previsão será utilizada como apoio à decisão, não como garantia de que determinada condição climática ocorrerá.

10. Critérios de sucesso

O projeto será considerado satisfatório quando a aplicação e sua documentação demonstrarem que os objetivos principais foram atendidos. Entre os critérios previstos estão:

Os usuários conseguem se cadastrar e autenticar-se.
Um organizador consegue criar e administrar uma pelada.
O sistema respeita os limites de participantes por modalidade e por equipe.
Os participantes podem entrar em partidas com vagas disponíveis.
O organizador consegue administrar a distribuição das equipes.
O sistema atualiza a fila conforme os resultados registrados.
As quadras podem ser cadastradas com suas informações de localização e modalidade.
A integração meteorológica consegue processar dados disponíveis e apresentar os alertas previstos nas regras.
A indisponibilidade da API meteorológica é tratada sem apresentar uma previsão inexistente como válida.
Os documentos de visão, casos de uso, arquitetura, dados e API apresentam informações coerentes entre si.

11. Riscos e pontos de atenção

Os principais riscos identificados para o desenvolvimento do Futup são:

Regras de negócio incompletas ou interpretadas de maneiras diferentes pelos integrantes.
Dificuldade de representar corretamente a fila de jogos e as substituições.
Indisponibilidade ou ausência de dados na API meteorológica.
Inconsistências entre os modelos de dados e as funcionalidades previstas.
Atrasos decorrentes da divisão de tarefas e da integração dos componentes desenvolvidos pelos integrantes.
Definição tardia de requisitos não funcionais e de critérios de validação.

Esses riscos deverão ser acompanhados durante o desenvolvimento, com revisão das regras e validação conjunta dos documentos.

12. Decisões pendentes

Os seguintes pontos precisam ser confirmados ou detalhados pelo grupo:

Definir o comportamento quando um participante sai definitivamente da pelada durante uma partida.
Definir como o organizador escolhe e registra a substituição, incluindo os casos em que não há jogadores aguardando.
Confirmar o limite definitivo utilizado para classificar chuva forte.
Detalhar como será estimada a duração da chuva a partir dos dados meteorológicos horários.
Definir metas mensuráveis para desempenho e outros requisitos não funcionais.
Confirmar o banco de dados e as configurações do ambiente de produção.
Definir os critérios de validação e os testes de aceitação das principais funcionalidades.

As decisões deverão ser registradas nas especificações correspondentes e refletidas nas demais partes da documentação.

13. Considerações finais

O Futup pretende simplificar a organização de peladas por meio de uma aplicação web que reúna o gerenciamento de participantes, equipes, quadras e filas de jogos, além de informações meteorológicas relevantes.

A definição clara do escopo e das regras de negócio servirá como base para os casos de uso, a arquitetura, o modelo de dados e o contrato inicial da API. A consistência entre esses artefatos será essencial para orientar a implementação e a validação do sistema durante as próximas etapas do projeto.