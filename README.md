# Projeto de Interface Humano-Computador

Projeto apresentado ao Centro Universitário [FEI](https://portal.fei.edu.br/), como parte dos requisitos necessários para aprovação na disciplina de Interface Humano-Computador (CC8122) do curso de Ciencia da Computação, orientado pelo Prof. Dr. [Fagner de Assis Moura Pimentel](https://github.com/fagnerpimentel).

Este projeto se baseia no Trabalho de Conclusão de Curso (TCC) entitulado DETECÇÃO E IDENTIFICAÇÃO DE SOM AMBIENTE PARA AUXÍLIO DOMÉSTICO DE PESSOAS COM DEFICIÊNCIA AUDITIVA sob orientação do Professor Dr. Plinio Thomaz Aquino Junior e desenvolvido pelos seguintes alunos:

- Paulo Vinicius Araujo Feitosa 24.122.042-5
- Guilherme Marcato Mendes Justiça 22.225.030-0

**1\) Conhecendo o Problema** 

1.1) Membros de Equipe:
- Paulo Vinicius Araujo Feitosa 24.122.042-5

1.2) Título Original do TCC:

- Detecção e identificação de som ambiente para auxílioo doméstico de pessoas com deficiência auditiva

1.3) Nome do orientador:

- Plinio Thomaz Aquino Junior

1.4) Previsto desenvolver Interface? ( x ) Sim  (  ) Não

1.5) Objetivo do trabalho?

- Desenvolver um hardware capaz de detectar e identificar um som e enviar para um aplicativo, auxiliando de forma direta a independência de pessoas surdas, por meio de uma notificação.

1.6) Qual o produto final? 

- Hardware para detectar e identificar som.
- Software para experiência do usuário.

1.7) Quem é o usuário final deste produto?

- Pessoas surdas com intenção de utilizar a tecnologia ao seu favor.

1.8) O que o usuário recebe de benefício ao usar esse produto? 

- Independência/autonomia e percepcão do ambiente.

1.9) Quais as funcionalidades da ferramenta (visão do usuário)?
	
- Cadastro
- Login 
- Cadastro do dispositivo
- Edição do perfil
- Configuração do dispositivo

1.10) Quais tecnologias e ferramentas computacionais que pretendem usar neste projeto (TCC)?
- Hardware: Raspberry Pi 4 Model B (8 GB).
- Microfone Adafruit I2S MEMS SPH0645LM4H.
- Armazenamento de Dados : Serviços da Firebase.
- Mobile: Flutter (Dart) para o aplicativo multiplataforma.

1.11) Qual é o contexto de uso dessa aplicação?

- Usuário : Pessoas de qualquer idade com qualquer grau de surdez.
- Objetivo : Saber onde e quando ocorreu um barulho.
- Interação : Receber uma notificação e agir conforme vontade.
- Equipamento : Celular.

- O contexto de uso é: Em uma casa aconchegante, onde os sons familiares do cotidiano compunham uma melodia tranquila, a rotina seguia seu curso. De repente, o ritmo foi quebrado não por um som audível, mas por uma vibração no bolso. A tela do celular iluminou-se com um alerta urgente: "Barulho em QUARTO detectado - Som de objeto quebrando". Com o coração acelerado, a pessoa correu em direção ao quarto, imaginando o pior. Ao abrir a porta, a tensão se dissipou instantaneamente. Ao lado da cama, os cacos de um vaso espalhados pelo chão; sobre a escrivaninha, o gato observava a cena com a indiferença típica dos felinos. Após um suspiro de alívio e uma rápida limpeza, a ordem foi restaurada e a vida voltou ao seu ritmo normal.


## Resumo

- Sistema embarcado para aumentar a autonomia de pessoas com deficiência auditiva em ambientes domésticos. Usa uma Raspberry Pi + microfone I2S para captar áudio; pré-processa, classifica sons com o modelo pré-treinado YAMNet e filtra eventos relevantes (ex.: campainha, alarme, bebê chorando). Eventos validados são enviados ao Firebase e notificam o usuário no celular via FCM (alertas visuais e vibratórios). Objetivo operacional: tempo de resposta inferior a 1 s e alta precisão na identificação.

## Introdução
 
### Apresente o propósito do produto ou serviço e quais são os principais benefícios que ele oferece aos usuários.
- Propósito: detectar e identificar sons domésticos relevantes e notificar pessoas com deficiência auditiva para aumentar segurança, independência e qualidade de vida.
- Benefícios: notificações em tempo real (visual + vibração), redução da dependência de terceiros.
### Identifique os problemas ou necessidades que o produto ou serviço resolve ou satisfaz.
- Incapacidade de perceber sinais sonoros essenciais (campainha, alarmes, eletrodomésticos, choro de bebê, etc.).
- Dependência de conviventes ouvintes ou necessidade de múltiplos indicadores visuais pela casa.
- Falta de soluções adaptadas ao ambiente doméstico que sejam robustas a ruído e fáceis de configurar.
  
### Liste as características e funcionalidades do seu produto ou serviço de forma detalhada.
- Captação de áudio com microfone I2S (Adafruit SPH0645LM4H).
- Pré-processamento local: reamostragem para 16 kHz, normalização (float32 entre -1 e 1).
- Extração de espectrograma Mel (janelas de 25 ms, hop de 10 ms) para entrada do classificador.
- Classificação com YAMNet (521 classes pré-treinadas); retorna embeddings, scores e espectrograma (necessários para a classificação do som).
- Filtragem por lista de classes relevantes e limiar de confiança (ex.: 0,70) para evitar falsos positivos.
- Envio do evento (JSON: user_id, tipo de som, timestamp, hardware_id) ao Firebase Realtime Database via biblioteca firebase_admin.
- Notificação ao usuário via Firebase Cloud Messaging (token FCM), com alertas visuais e vibratórios no app Flutter.
- Emparelhamento/configuração inicial via BLE (BlueZ + Bluezero na Raspberry Pi; flutterblueplus no app).
- Histórico de eventos de sons relevantes no aplicativo.
  
### Liste as tecnologias e ferramentas computacionais que pretendem usar neste projeto (TCC).
- Hardware: Raspberry Pi 4 Model B (8 GB) com microfone Adafruit I2S MEMS SPH0645LM4H.
- Bibliotecas e frameworks: sounddevice (captura de áudio), NumPy (arrays), TensorFlow / YAMNet (classificação do som), TensorFlow Lite (se otimizar para embarcado), firebase_admin, Firebase Realtime Database, Firebase Cloud Messaging (FCM).
- Conectividade: Wi-Fi, BLE, flutterblueplus (aplicativo no celular).
- Mobile: Flutter (Dart) para o aplicativo multiplataforma.
- Ferramentas para UX/validação: Google Forms para pesquisa de campo, com deficientes auditivos como entrevistados.
  
### Apresente o contexto de uso dessa aplicação. (“Usuários, tarefas, equipamentos (hardware, software e materiais) e o ambiente físico e social no qual um produto é usado.”)
- Usuários: pessoas com deficiência auditiva (diversos graus), preferencialmente que possuam smartphone Android/iOS. Também cuidadores/familiar e instituições (Exemplo: residências assistidas).
- Tarefas: detectar eventos sonoros importantes, enviar notificações, revisar histórico, configurar/parear dispositivos e indicar sons relevantes.
- Equipamentos: Raspberry Pi + microfone em pontos estratégicos da casa; roteador Wi-Fi doméstico; smartphone com app Flutter; possivelmente múltiplas placas para cobrir ambientes.
-  Ambiente físico/social: residências (salas, cozinhas, quartos); ruído ambiental variável; usuários com diferentes níveis de familiaridade tecnológica. Sistema projetado para operação local com sincronização na nuvem (Firebase) para notificações em tempo real

## Publico Alvo

### Determine qual o grupo específico de pessoas ou organizações para as quais este produto ou serviço é direcionado.
- Pessoas com deficiência auditiva que vivem em ambientes domésticos e buscam maior autonomia e segurança; também cuidadores/familiares.
  
### Descreva as caracteristicas demográficas, comportamentais, psicográficas ou geográficas deste público alvo que o torna mais propenso a se interessar pelo que está sendo oferecido neste projeto ou serviço.
- Demográficas: adultos, adolescentes e idosos com perdas auditivas (qualquer grau), residentes em áreas com acesso a Internet/Wi-Fi (necessário para notificações).
- Comportamentais: usuários que usam smartphone regularmente, valorizam independência, e preferem soluções tecnológicas assistivas.
- Psicográficas: preocupados com segurança doméstica; buscam privacidade e autonomia; aberto a novas tecnologias se simples de usar.
- Geográficas: inicialmente foco em áreas urbanas/suburbanas com infraestrutura de Internet; o projeto é de baixo custo e escalável para outras regiões.

## Análise de concorrência

**1. Identifique os principais concorrentes ou softwares mais utilizados pelo seu público-alvo.**
   - O mercado é liderado por grandes ecossistemas e marcas estabelecidas, nomeadamente: Amazon Alexa, LG ThinQ e Philips Hue.
   
**2. Colete informações sobre os concorrentes selecionados.**
   - Todas as empresas listadas operam dentro do espectro da Internet das Coisas (IoT), integrando hardware, software e conectividade ambiental. Nota-se uma padronização de mercado, onde a maioria compartilha características essenciais e funcionalidades similares para cada categoria de dispositivo.
     
**3. Analise as características e funcionalidades dos concorrentes.**
   - **Soluções de Hardware + Cloud:** Destacam-se pela qualidade superior de captação e cobertura através de sensores dedicados, porém apresentam custos mais elevados e exigem processos de configuração e emparelhamento.;
   - **Aplicativos Treináveis:** O foco é a personalização (o usuário grava os sons), contudo, por dependerem do hardware do smartphone, enfrentam limitações na captação de áudio/ruído e geram alto consumo de bateria.

**4. Avalie a experiência do usuário (UX).**
   - Com base no feedback a positivo, a experiência é polarizada: elogios à praticidade e usabilidade geral contrastam com críticas técnicas, incluindo lentidão do sistema, incompatibilidade do layout paisagem em tablets e o incômodo piscar das lâmpadas durante a conexão.

**5. Examine os preços e modelos de negócio.**
   - Variação de R$60,00 até R$300,00 dependendo de qual dispositivo o usuário compra. 

**6. Pesquisa de satisfação do cliente e opiniões.**
   - As opiniões sobre a solução da Positivo reforçam a percepção de ser um sistema prático, mas com falhas de desempenho (lentidão) e usabilidade (ausência de modo paisagem em tablets e feedback visual excessivo no emparelhamento).

**7. Identifique padrões e tendências no mercado.**
   - Crescente foco em dispositivos de segurança específicos, como campainhas e fechaduras inteligentes.

**8. Elabore relatórios e sumarize os resultados.**
   - Apps puramente móveis: Oferecem alta personalização, mas carecem de robustez na captação de áudio.
   - A implementação do modelo YAMNet com filtragem por limiar é eficaz para reduzir falsos positivos, contudo, sua eficácia depende de validação rigorosa em ambientes reais.

**9. Extraia pontos positivos e faça recomendações.**
   - Pontos positivos da proposta (comparados aos concorrentes): várias classes para classificação de sons (YAMNet) e facilidade de setup via Bluetooth Low Energy (BLE).
   
### Personas
**Persona primaira (Maria, 58 anos):**

![maria](https://github.com/user-attachments/assets/0a47a3b9-9ab8-4e9d-bb02-5d58d8daabfd)


- Perfil: mulher, perda auditiva moderada a severa, mora sozinha em apartamento urbano, aposentada. Usa celular- para mensagens e redes sociais, tem familiar que a visita semanalmente.
- Contexto socioeconômico e cultural: renda média-baixa (aposentadoria), ensino médio completo, prefere soluções de baixo custo e fáceis de usar; valoriza privacidade e autonomia.
- Tecnologia / habilidades: usa smartphone, mas não gosta de configurações técnicas complexas; aceita ajuda de familiares para instalar algo pela primeira vez.
- Necessidades e expectativas: perceber campainha, alarmes, temporizadores de cozinha e choro de bebê/animal (se houver visitas); receber notificações claras (visual e vibratória).


 **Persona secundária (Lucas, 34 anos):** 
  
  ![lucas-novaes-200x300](https://github.com/user-attachments/assets/d708420d-2538-471a-bec6-da7a46fa7a4d)
  
- Perfil: universitário/trabalhador com pouco tempo livre; mora perto e visita/ajuda a mãe; possui smartphone e bom conhecimento técnico.
- Contexto socioeconômico e cultural: renda média, mora em área urbana; sensível a segurança da mãe e disposto a pagar por serviços que aumentem autonomia dela.
- Tecnologia / habilidades: confortável com apps, consegue emparelhar dispositivos via Bluetooth e resolver problemas simples remotamente (p.ex. reiniciar Raspberry Pi).
- Expectativas: receber notificações secundárias (se a mãe não responder) e poder consultar histórico; configurações remotas básicas.

### Quais informações sobre o usuário o serviço/produto deve guardar
- Dados Pessoais e Conta: Nome, telefone, e-mail (para cadastro/auth no Firebase Authentication). 
- Dispositivos: ID(s) da(s) Raspberry Pi, MAC BLE identificador, localização lógica (ex.: “Sala”, “Cozinha”), firmware/version. 
Preferências e configurações: Lista de classes sonoras relevantes (ex.: campainha, alarme incêndio, choro de bebê, micro-ondas), limiar de confiança personalizado, modos “silencioso/sem alertas” por horário, contatos de emergência, vibração/visual padrão. 
- Credenciais / Notificação: Token FCM do dispositivo móvel, permissões, histórico de login (para segurança).
- Histórico de eventos: Eventos detectados (timestamp, tipo de som, score do modelo, dispositivo origem, ocupantes marcados como “respondido”/“ignorado”), gravações ou trechos (se usuário permitir). 
- Dados de ambiente / calibragem: Perfil acústico da residência (nível de ruído médio, janelas com ruído), amostras/enriquecimento de treino local (se o usuário gravar exemplos), preferências de sensibilidade por cômodo.
- Consentimento / privacidade: Consentimento para armazenamento de gravações, política de retenção de dados (p.ex. excluir gravações brutas após X dias), compartilhamento com cuidadores.

### Mapa de empatia

![Mapa de empatia](empatia.png)

- Determine o mapa de empatia[^1] de pelo menos uma persona primária e uma sercundária.
- 
**(Maria, persona primária):**
  - **O que o usuário vê:** Sala de estar com TV (às vezes desligada), cozinhas com micro-ondas, campainha externa visível pela porta, familiares entrando às vezes; smartphone com ícones grandes.
  - **O que o usuário ouve:** (pouco) percebe vibrações do celular se estiver por perto; costuma não ouvir campainha ou alarmes à distância; ruídos ambientes (trânsito, vizinhos) podem confundir sua percepção. (documento destaca ambientes ruidosos como desafio).
  - **O que o usuário diz e faz:** Diz que não quer depender sempre de vizinhos; configura preferências básicas no app (com ajuda); tende a manter app aberto na tela principal quando espera visitas.
  - **O que o usuário pensa e sente:** Deseja segurança e privacidade; teme perder alertas importantes e ser um incômodo para os outros; quer controlar o que é notificado (não quer spam).
  - **Dores:** Falsos positivos/alertas inúteis; configurações difíceis; medo de exposição (gravações indo para nuvem sem controle).
  - **Ganhos:** Receber alerta visual + vibração imediato quando algo crítico ocorre; fácil configuração inicial; histórico acessível para revisar quem bateu à porta; maior autonomia.
  - 
**(Lucas, persona secundária):**
- **O que o usuário vê:** Painel do app com listagem de eventos, notificações não-lidas, status do dispositivo (online/offline).
  - **O que o usuário ouve:** Notificações push do sistema; se perto, sons do ambiente da residência durante visitas; logs de áudio se permitidos.
  - **O que o usuário diz e faz:** Configura limiares, pareia novo dispositivo, verifica logs, aciona contatos em caso de evento crítico; pode definir modos “ausente” ou “em visita”.
  - **O que o usuário pensa e sente:** Quer confiabilidade e mínimo de manutenção; prefere soluções que não gerem “alarme falso” nem muita intervenção manual.
  - **Dores:** Sistema offline ou notificação perdida; configuração inicial complexa; múltiplos dispositivos a gerir em diferentes residências.
  - **Ganhos:** Tranquilidade ao saber que a mãe será alertada; histórico para prova/registro; controle remoto de configurações se necessário.
## Contexto de uso

- **Descreva o ambiente em que o serviço ou poduto deve ser utilizado:** Casas e apartamentos: múltiplos cômodos, diferentes níveis de ruído (cozinha, sala com TV, áreas externas com tráfego).
  
- **Qual/quais o(s) contexto(s) sociais, econômicos e culturais existentes neste ambiente?** No contexto social, usuários podem morar sozinhos, com familiares ouvintes, ou em residências assistidas. Deve suportar multiusuário (primário + contatos de confiança). A sensibilidade cultural exige respeito à privacidade — gravações não devem ser enviadas sem consentimento. No contexto econônico, solução pensada para baixo custo (Raspberry Pi + microfone); possibilidade de versões com/sem assinatura para serviços na nuvem.
  
- **Quais informações sobre o ambiente, o serviço ou poduto deve guardar antes de iniciar a interação?** Local do dispositivo (rótulo: “Cozinha”), perfil acústico inicial (medição de ruído ambiente), preferências de alerta do usuário, contato(s) de emergência, token FCM, consentimento de gravação, lista de classes de som relevantes ativas.
  
- **O que normalmente deve estar acontecendo com o ambiente quando o usuário interagir com o serviço ou poduto?** Dispositivo ligado e conectado (Wi-Fi), microfone em posição fixa, app do usuário com permissão de notificações ativo (ou com permissão para receber alerts via FCM), energia estável (ou bateria/reserva), rotina diária normal (TV, conversas ocasionais). Sistema deve tolerar variações (ruído alto, janelas abertas).

## Jornada do usuário

- Criar uma narrativa para o o seu serviço ou produto com o usuário.
  - Sou uma pessoa com deficiência auditiva e quero ser avisada, no meu celular, quando acontecerem sons importantes em casa (campainha, alarme, bebê chorando, etc.). Compro/recebo a Raspberry Pi com o microfone I2S já preparado. Abro o app móvel, faço meu cadastro e pareio a Raspberry com o app via Bluetooth. O app envia para a placa o Wi-Fi da minha casa e vincula meu usuário. A partir daí, a Raspberry fica “ouvindo” o ambiente  para reconhecer o tipo de som. Quando algo relevante acontece, a placa registra e dispara uma notificação push para o meu celular, com vibração e alerta visual, dizendo o que foi detectado, quando e onde. Se eu quiser, abro o histórico no app e vejo detalhes do evento e do meu hardware.

- Determine o que o usuário realiza desde a primeira até o última interação com o serviço ou produto.
  - Instalo o app e crio a conta no aplicativo, informo nome, telefone e senha. A conta é criada e vinculada ao meu usuário;
  - Ligo a Raspberry Pi e abro o app para conectar;
  - A Raspberry Pi 4 com microfone inicializa o BlueZ e entra em modo de pareamento Bluetooth; o app procura dispositivos próximos;
  - Envio credenciais e vinculação. No app, escolho minha Raspberry; o app envia SSID/senha do Wi-Fi e meu ID de usuário. A Raspberry armazena as credenciais, conecta-se ao Wi-Fi e registra-se no banco, concluindo o vínculo com a minha conta;
  - Habilito notificações push. O app obtém um token e o salva no banco, para que futuras detecções cheguem ao meu celular mesmo com o app fechado.
  - Captação e pré-processamento local. O microfone envia o áudio para a Raspberry.
  - A Raspverry faz o processamento necessário para identificar o áudio captado pelo microfone, e se é um som importante para mim.
  - Registro no banco e disparo da notificação. Ao detectar um evento válido, a Raspberry grava o som, guarda as informações no Banco de Dados e manda para disparar a notificação push imediata.
  - Recebo o alerta no celular. No celular, a notificação vibratória e visual informa tipo de som, horário e local. Toco para abrir o app e posso ver histórico e informações do hardware (telas previstas no protótipo).
  - Ação e confirmação. Depois de avisado, tomo a ação necessária (atender a porta, verificar o bebê, checar o alarme). No app, o evento fica registrado para consulta posterior, apoiando avaliações de usabilidade e confiabilidade do sistema.


 
## Coleta de dados

### 1. Que dados coletar?
   - Idade, gênero, escolaridade;
   - Fluência do usuário com LIBRAS e português;
   - Desde de quando o usuário tem a deficiência auditiva, qual é o nível, quais são as suas dificuldades;
   - Familiariedade com a tecnologia 


### 2. De quem coletar?
   - Pessoas surdas;
   - Pessoas/Instituições responsáveis pelos surdos (pais, escolas, babás).

### 2.1 Quais aspectos éticos?
   - O projeto envolve aspectos éticos baseado no princípio da justiça e equidade, envolvendo a proteção dos dados e utilização deles apenas para desenvolvimento do projeto.

### 3. Ferramentas de Coleta de Dados
   - **Nome do instrumento e objetivo**: Questionário com o objetivo de coletar as informações necessárias com praticidade e possibilidade de escalabilidade e melhorias.

### 3.2 Explicar como aplicar (serve para normalizar o processo de aplicação quando pessoas distintas aplicam o instrumento)
   - Formular perguntas, separar as melhores e aplicá-las no questionário.

### 3.3 Instrumento utilizado 

[Formulário: Tecnologia para Inclusão: Um Aplicativo para Autonomia Auditiva](https://docs.google.com/forms/d/e/1FAIpQLSeGRXnJkaUiu8sKENSzcnSS1H-NjFLpNw5YLdTkTCqo30dPUA/viewform?usp=header).

## Modelo de tarefas

## Design

- Pense nas características de Affordances do seu serviço ou poduto. 
    - Que tipo de acessibilidades devem ser consideradas dentro do seu projeto?
- Discuta o papel das expectativas do usuário no projeto deste serviço ou poduto. Qual a importância e pontos a serem considerados se você quiser vender esse serviço ou poduto?

# **Entrega 4**
## Cenário de Análise/Problema 

Dona Rosa, aposentada que mora sozinha e tem perda auditiva moderada a severa, perde timers do micro-ondas, campainha, alarmes e até o choro de um bebê — resultando em comida queimada, visitas perdidas e sensação contínua de insegurança e dependência da filha que a ajuda semanalmente. Ela usa smartphone para mensagens, quer soluções simples, baratas e que preservem sua privacidade.

Um dispositivo local que identifica sons importantes dentro de casa e envia notificações visuais e por vibração ao celular resolveria esses problemas: evitaria riscos (alarme de incêndio), garantiria que receba entregas e visitas, e devolveria autonomia sem configurações complexas

## Questões de Refinamento
- Evento; ator; ambiente; objetivo

Sem questões

- Planejamento - ação - avaliação

## Refinamento do Cenário Análise/Problema

Dona Rosa, aposentada que mora sozinha e tem perda auditiva moderada a severa, sempre anda com seu celular para recer notificações do aplicativo Hear's Sound, assim não perde timers do micro-ondas, campainha, alarmes e até o choro de um bebê, que a avisa sobre os sons importantes dentro de sua casa.


# **Entrega 5**
## Análise de Tarefas 
 - Cadastro de um Hardware

1) HTA
<img width="291" height="351" alt="HTA" src="https://github.com/user-attachments/assets/2db918b4-8264-4f7c-b673-0afe0e4080c5" />

2) GOMS
Goal 0: Cadastrar novo usuário
	Method 0.1: Já foi cadastrado
		Sel.R: O usuário já existe, o usuário deve inserir as suas informações na tela de login
	Method 1: Usuário novo
		Op 0: Digitar o e-mail no campo "e-mail"
		Op 1: Digital a senha no campo "senha"
		Op 2: Digitar a senha no campo "Digite a senha novamente"
Goal 1: Entrar com o novo cadastro
		Method 1: Digitando as informações
			Op 0: Digitar o e-mail no campo "e-mail"
			Op 1: Digital a senha no campo "senha"

	
   
4) CTT
<img width="691" height="411" alt="CTT" src="https://github.com/user-attachments/assets/69cc876b-9a3d-43cf-a45c-fb537aaebe0e" />


# **Entrega 6**

### Prtotipação em alto nível (React)

![CadOrLogin](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/cadorlogin.jpg?raw=true)

![Cadastro](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/cadastro.jpg?raw=true)

![Login](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/login.jpg?raw=true)

![Home](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/home.jpg?raw=true)

![Perfil](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/perfil.jpg?raw=true)

![Configuração](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/configuracao.jpg?raw=true)

![Ajuda](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/ajuda.jpg?raw=true)

![NovoDispositivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/novodispositivo.jpg?raw=true)

![Vincular](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/vincular.jpg?raw=true)

![ConfigDispostivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/prototype/configdispositivo.jpg?raw=true)



# **Entrega 7**

## O quê coletar: 
- Idade;
- grau de surdez;
- se mora com outra pessoa com deficiência auditiva;
- Dificuldades atuais dentro de casa;
-  Como é feita a solução destes problemas 
## De quem coletar:
-  Pessoas com deficiência auditiva que buscam independência e segurança dentro de casa
## Ferramentas de coleta: 
-  Formulário (![Formulário]([https://doi.org/10.1145/3290605.3300324](https://docs.google.com/forms/d/e/1FAIpQLSeGRXnJkaUiu8sKENSzcnSS1H-NjFLpNw5YLdTkTCqo30dPUA/viewform?usp=header))
-  Questionário de perguntas/dúvidas dos deficientes auditivos 
-  Estudos de casos já existentes sobre o projeto (![Exploring sound awareness in the home for people who are deaf or hard of hearing](https://doi.org/10.1145/3290605.3300324) 


# **Entrega 8**
## Ciclo de vida de engenharia de usabilidade

1. **Características da Plataforma**  
   

| Característica | Descrição |
| :---- | :---- |
| Descrição do sistema | Detecção e identificação de eventos sonoros dentro de casa com notificação par o aplicativo do usuário. |
| LISTA DE Capacidades da Plataforma (com explicação) | Utilização do hardware somente dentro de espaços fechados para a identificação de som |
| LISTA DE Restrições da Plataforma (com explicação) | Dependência de Internet para o seu funcionamento contínuo |

   

2. Princípios gerais do projeto
**Pilar da Interface**
| Nome | Descrição | Link |
| ---- | ---- | ---- |
| Psicologia das cores | Utilza-se a cor roxa por representar magia, da fé, do abstrato, da nobreza e da sabedoria | (https://www.serasaexperian.com.br/conteudos/psicologia-das-cores-no-marketing-digital) |


3. **Metas de Usabilidade**

   1. **Qualitativo**
      
      	- Tela simples
      	- Acessibilidade para surdos (LIBRAS)

   3. **Quantitativo**  

| Metas | Porcentagem | Justificativa |
| ----- | :---- | :---- |
| Facilidade de aprendizado | 10% | Só há a necessidade de aprender se for a primeira vez de uso em um aplicativo de Smart Home, Guilo's Sound segue a maioria dos padrões de mercado |
| Facilidade de memorização | 0% | Nenhuma necessidade de memorização. |
| Eficiência | 40% | Importante para que o usuário possa reagir ao evento de som notificado |
| Eficácia | 20% | Importante passo do para permitir que o usuário siga o fluxo do aplicativo e cumpra todas as tarefas sem erro |
| Satisfação | 30% | Usuário deve se sentir seguro e confiança no aplicativo, tendo certeza que o evento de som notificado foi verídico. |
| **Total** | **100%** | Porcentagem |



# Entrega 9
## Modelo Conceitual 

1. Cenários de Interação
Dona Rosa prepara o almoço e programa o micro-ondas. Ela senta na sala e cochila levemente, dessa vez o dispositivo instalado na cozinha detecta o timer e o **celular de Rosa vibra enquanto uma notificação visual “Timer: Micro-ondas” aparece na tela**; ela levanta e retira a comida antes que queime, sorrindo por ter evitado mais um desperdício.

Mais tarde a auxiliar chega e toca a campainha. **O sistema reconhece o toque da campainha e envia ao telefone de Rosa um alerta visual e vibratório indicando “Campainha — Entrada”**, ela atende sem precisar depender de bater na porta ou da filha. Já à noite, quando uma fritura começa a queimar, o alarme de fumaça dispara e o **dispositivo envia um alerta de emergência persistente e vibração forte ao celular**. Rosa percebe o aviso e sai do apartamento em segurança. Durante a visita da filha e do neto, **o aparelho identifica o choro do bebê no corredor e notifica Rosa imediatamente, permitindo que ela vá ajudar o netinho sem esperar ser chamada.**


2. Design Centrado na Comunicação (DCC)

**Nome do Cenário: Cadastro de Usuário no aplicativo**

| tópico \> subtópico (diálogo) | falas e signos |
| :---- | :---- |
| cadastrar no aplicativo | U: Preciso me cadastrar no aplicativo |
| informar dados do usuário  | D: Qual é o **e-mail** e **senha** de cadastro? U: Quero cadastrar meu **e-mail** principal e minha **senha** padrão|
| restrições dos dados | D: Para realizar o cadastro, você deve informar um **e-mail** válido que aceitamos e uma **senha** que cumpra os requisitos do nosso aplicativo U: Ok, seguirei os padrões do seu aplicativo  |
| cadastrar dados do usuário | D: Vi que você informou uma **senha de confirmação** diferente da anterior, poderia colocar as duas **senhas** iguais? U: Verdade, escrevi errado no **campo de confirmação**!  |
| cadastrar dados do usuário | D: Agora sim, seu cadastro foi realizado. U: Que bom! |
| mensagem | D: Se preferir, você pode entrar no aplicativo agora ou posteriormente. U: Entrarei posteriormente|


# Entrega 10 
## MOLIC
<img width="856" height="497" alt="MOLIC" src="https://github.com/user-attachments/assets/664f2baa-5b37-4635-9b8c-3a90a9940de2" />

# Entrega 11
## FIGMA 
![Link Figma](https://www.figma.com/design/T6PYaEwsPFynxvJS5dRkbb/TesteTcc?node-id=4-27&t=H1TxALvivAInfDig-0)

# Entrega 12
## Planejamento da Avalisação

1. Planejamento de usabilidade

| **Letra** | **Descrição / Preenchimento** |
|----------|-------------------------------|
| **D – Determinar os objetivos da avaliação** | Avaliar se os usuários conseguem se cadastrar sem dificuldades no aplicativo; verificar se a interface é acessível para pessoas surdas; medir se o fluxo evita erros e reduz esforço cognitivo. |
| **E – Explorar perguntas a serem respondidas** | - O usuário entende como deve utilizar o sistema?<br> - Ele compreende os textos presentes no aplicativo?<br> - O usuário percebe as notificações do aplicativo? |
| **C – Escolher métodos de avaliação** | Avaliação heurística, teste de usabilidade com observação, entrevistas pós-teste, análise de tarefas e coleta de métricas dos usuários (tempo, erros, cliques). |
| **I – Identificar usuários** | Usuários primários: pessoas surdas |
| **D – Decidir questões práticas** | Local: teste no próprio celular dos avaliados e dentro de um ambiente controlado. |
| **E – Avaliar, interpretar e apresentar os dados** | A análise deve considerar: facilidade de uso, erros encontrados, expectativas dos usuários e sugestões. Resultados devem ser categorizados em eficiência, eficácia e satisfação. |

2. Lista de instrumentos
    2.1 Contrato de consentimento à pesquisa

	2.2 Questionários

	2.3 Formulário de avalisação heurística

	2.4 Tabela de observações

