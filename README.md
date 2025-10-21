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

- O contexto de uso é: Uma casa, um ambiente confortável com barulhos tradicionais do cotidiano, a pessoa segue com sua rotina normalmente, porém um barulho foi detecado em outro cômodo da casa. A pessoa recebe em seu celular a notificação - "Barulho em QUARTO detectado - Som de objeto quebrando". Assim, ela corre para ver o que aconteceu e se depara com um vaso quebrado do lado de sua cama e seu gato em cima da escrivaninha. Após perceber que tudo está bem e arrumar a ocorrência, ela segue sua rotina novamente.



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
   - Positivo Casa Inteligente
   - Amazon Alexa
   - Start Life
   - LG Thinq
   - Philips Hue
   - Tuya
   
**2. Colete informações sobre os concorrentes selecionados.**
   - Todos estão envolvidos no contexto de IoT, conectividade com hardware, software e ambiente, maioria possuem as mesmas características padrões do mercado e funcionalidades parecidas para cada tipo de dispositivo.
     
**3. Analise as características e funcionalidades dos concorrentes.**
   - Hardware + cloud: melhor captação e cobertura (sensores dedicados), porém custo e necessidade de configuração/emparelhamento;
   - Apps treináveis: forte personalização (o usuário registra sons), mas geralmente rodam no smartphone → limitações de captura/ruído e consumo de bateria.

**4. Avalie a experiência do usuário (UX).**
   - Avaliações dos usuários do aplicativo Positivo Casa Inteligente: "Prático","Experiência excelente","Lâmpada pisca muito para emparelhar","Aplicativo é lento","Não funciona em paisagem no tablet"

**5. Examine os preços e modelos de negócio.**
   - Variação de R$60,00 até R$300,00 dependendo de qual dispositivo o usuário compra. 

**6. Pesquisa de satisfação do cliente e opiniões.**
   - Avaliações dos usuários do aplicativo Positivo Casa Inteligente: "Prático","Experiência excelente","Lâmpada pisca muito para emparelhar","Aplicativo é lento","Não funciona em paisagem no tablet"

**7. Identifique padrões e tendências no mercado.**
   - Tendência para modelos específicos de "objetos inteligentes", como campainha inteligente, ou fechadura inteligente
   - Navegação no estilo BottomNavigation
   - Tela principal com os dispositivos aparecendo de forma de cards

**8. Elabore relatórios e sumarize os resultados.**
   - Apps puramente móveis: alto poder de personalização, baixa robustez em captação.
   - Hardware dedicado + ML (como a proposta): melhor captação e latência; maior necessidade de configuração inicial.
   - Uso de YAMNet e filtragem por limiar reduz falsos positivos, mas requer validação extensiva em ambientes reais.

**9. Extraia pontos positivos e faça recomendações.**
   - Pontos positivos da proposta (comparados aos concorrentes): processamento local (latência <1 s), uso de YAMNet (ampla gama de classes), integração com Flutter + Firebase (notificações confiáveis), emparelhamento via BLE para facilitar configuração.
   - Personalização fácil; Proteção de privacidade; Robustez a ruído; e Foco em UX de configuração.

**10. Telas dos aplicativos**

   ![Positivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/opcaopositivo.jpg?raw=true)
   
   ![Positivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/cadastropositivo.jpg?raw=true)

   ![Positivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/loginpositivo.jpg?raw=true)
   
   ![SmartLife](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/opcaosmartlife.jpg?raw=true)
   
   ![SmartLife](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/loginsmartlife.jpg?raw=true)

   ![Alexa](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tpalexa.jpg?raw=true)

   ![GoogleHome](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tpgooglehome.jpg?raw=true)
   
   ![PhilipsHue](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tphue.jpg?raw=true)

   ![LGThinq](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tplgthinq.jpg?raw=true)
   
   ![Positivo](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tppositivo.jpg?raw=true)
   
   ![Smartlife](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tpsmartlife.jpg?raw=true)
   
   ![Tuya](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/tptuya.jpg?raw=true) 

   
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

<!--
## Análise de concorrência

- Pesquise serviços ou produtos existentes atualmente que possam realizar o objetivo deste projeto.
- Selecione pelo menos 3 serviços ou produtos diferentes.
- Em relação aos concorrentes, respondam as seguintes perguntas?
  - Existe plataforma similar que atende o mesmo mercado e funcionalidades? Se sim: Quais os pontos positivos? Quais os pontos negativos?
  - Existe plataforma diferente quanto ao serviço, mas que atenda esse mercado? Se sim: Quais os pontos positivos? Quais os pontos negativos?
 -->
 
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

# **Entrega 5**

**\[1 solução por pessoa\]**

Funcionalidade geral:
 - O objetivo dessa funcionalidade é cadastrar um novo hardware (dispositivo que detectará o som ambiente) no aplicativo do usuário.

**Análise de Tarefas**

**1\) HTA**

![HTA](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/HTA.jpg?raw=true)

- Para cadastrar o hardware, o usuário deverá seguir, em ordem, 3 passos:
  
  		-Seguir para a tela de cadastro.
  
  		-Informar os dados do Wi-Fi:
  		Nessa etapa o usuário tem duas opções e deverá seguir com alguma das duas:  
  			- 1: O usuário já informou os dados da rede anteriormente;  
  			- 2: O usuário deve informar wi-fi e senha da rede em qualquer ordem, preferencialmente o nome da rede antes.
    
    	-Escolher o dispositvo.

**2\) GOMS**

![GOMS](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/Goms.jpg?raw=true)

- Para cadastrar o hardware, o usuário deverá seguir 3 goals principais:
  
  		Objetivo 1: Seguir para a tela de cadastro
  			-O usuário deve clicar no ícone de (+).
  
  		Objetivo 2: Informar os dados do Wi-Fi.
    
  		Nessa etapa o usuário tem dois métodos e deverá seguir com algum deles:
  
  			- Método 1: O usuário já informou os dados da rede anteriormente e as informações já estão salvas no sistema;
  				- O usuário deverá clicar no botão "vincular"
  
  			- Método 2: O usuário ainda não informou o Wi-Fi e deve informá-lo para cadastrar o dispositivo.
  				- Método 2.A : Informar o nome da rede, e, em seguida, a senha da rede.
  					-O usuário deve clicar em "nome da rede" e informá-lo;
  					-O usuário deve clicar em "senha" e informá-la;
  					-O usuário deve clicar no botão "vincular".
				- Método 2.A : Informar a senha da rede, e, em seguida, o nome da rede.
					-O usuário deve clicar em "senha" e informá-la;
					-O usuário deve clicar em "nome da rede" e informá-lo;
					-O usuário deve clicar no botão "vincular".
  
		Objetivo 3: Escolher o dispositvo.
    		-O usuário deve clicar no dispositivo encontrado
  			

**3\) CTT**

![CTT](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/CTT.jpg?raw=true)

	- Para cadastrar um novo hardware, o usuário deve acessar a tela de cadastro de hardware e informar os dados da rede Wi-Fi, inserindo o nome e a senha em qualquer ordem.
	Em seguida, o sistema salva as informações da rede e direciona o usuário para a tela de vinculação, onde ele deverá selecionar o dispositivo desejado.
	Após a escolha, o sistema registra o dispositivo e conclui o processo de vinculação.


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

**\[PARTE A: 1 solução completa por pessoa da equipe\]**

**1\)	Identificação de Necessidades dos Usuários e Requisitos de IHC: exercício de perguntas**

* **Que dados coletar?**
  
	-	Infomações pessoais: nome, idade e grau de surdez;
	- 	Dificuldades atuais;
	-   Como elas buscam as soluções atuais;
 	-   Familiaridade com a tecnologia;  
    - 	Coletar feedbacks específicos após explicação da solução: o que mudaria? o que acredita ser uma barreira? ... ;
    - 	Perguntas específicas para cada pessoa, depende da relação dela com a surdez.
    
* **De quem coletar?**
  	- Pessoas surdas;
  	- Pessoas/Instituições responsáveis pelos surdos (pais, escolas, babás).
  
**2\)	Aspectos Éticos**

* **Seu projeto deverá considerar aspectos éticos? Justifique usando os conceitos da aula.**

	- Sim, o projeto envolve aspectos éticos baseado no princípio da justiça e equidade, envolvendo a proteção dos dados e utilização deles apenas para desenvolvimento do projeto.

**\[PARTE B: 1 solução completa por pessoa da equipe \- e com técnicas diferente; questionário deve ser uma das técnicas escolhidas\]**

**3\)	Ferramentas de Coleta de Dados**  
**3.1) nome do instrumento e objetivo de aplicação**  

	- Questionário com o objetivo de coletar as informações necessárias com praticidade e possibilidade de escalabilidade e melhorias.


**3.2) explicar como aplicar (serve para normalizar o processo de aplicação quando pessoas distintas aplicam o instrumento)** 

	- Formular perguntas, separar as melhores e aplicá-las no questionário.


**3.3) instrumento (por exemplo, link do questionário no Google Forms, roteiro de entrevista, roteiro do Grupo Focal, etc)**

[Link do questionário](https://docs.google.com/forms/d/e/1FAIpQLSeGRXnJkaUiu8sKENSzcnSS1H-NjFLpNw5YLdTkTCqo30dPUA/viewform?usp=header)
	
# 

# **Entrega 8**

**CICLO DE VIDA DE ENGENHARIA DE USABILIDADE**

1. **Características da Plataforma**  
   

| Característica | Descrição |
| :---- | :---- |
| Descrição do Software e Hardawre| Hardware capaz de detectar e identificar um som ambiente integrado com um software que notifcará a pessoa do evento ocorrido |
| LISTA DE Capacidades da Plataforma (com explicação) | Possibilidade de usar em qualquer lugar |
| LISTA DE Restrições da Plataforma (com explicação) | Dependência de bateria - Dependência de Internet - Tamanho de tela - Muitas notificações seguidas|

2. **Princípios Gerais do Projeto**  
   
**Pilar de negócio do mercado**
 
| Nome | Descrição | Link |
| :---- | :---- | :---- |
| Descrição do Contexto | .  |  |
| Lei Geral de Proteção de Dados (LGPD) \- Lei n.º 13.709/2018 | A LGPD é a legislação brasileira que regulamenta o tratamento de dados pessoais no Brasil. É importante para o projeto porque estabelece regras sobre como os dados dos usuários devem ser coletados, armazenados, processados e protegidos, garantindo sua privacidade e segurança. | [https://www.planalto.gov.br/ccivil\_03/\_ato2015-2018/2018/lei/l13709.htm](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm) |
| Lei n.º 10.098/2000 \- Lei da Acessibilidade |  Esta lei brasileira estabelece normas gerais e critérios básicos para a promoção da acessibilidade das pessoas com deficiência ou com mobilidade reduzida. É importante para o projeto porque define diretrizes para tornar produtos e serviços, incluindo interfaces de usuário, acessíveis a todos os usuários, independentemente de suas habilidades físicas ou cognitivas. | [https://www.planalto.gov.br/ccivil\_03/leis/l10098.htm](https://www.planalto.gov.br/ccivil_03/leis/l10098.htm) |
| ABNT NBR ISO 9241 Ergonomia da interação humano-sistema |  Esta série de normas brasileiras, baseadas nas normas ISO 9241, fornece diretrizes e orientações para o design centrado no usuário de sistemas interativos, incluindo a concepção de interfaces de usuário. A parte 210 aborda o processo de design centrado no humano, enquanto a parte 11 fornece orientações específicas sobre usabilidade. Essas normas são importantes para o projeto porque estabelecem princípios e métodos para garantir que a interface do usuário atenda às necessidades e expectativas dos usuários. | [https://www.inf.ufsc.br/\~edla.ramos/ine5624/\_Walter/Normas/Parte%2011/iso9241-11F2.pdf](https://www.inf.ufsc.br/~edla.ramos/ine5624/_Walter/Normas/Parte%2011/iso9241-11F2.pdf) |
| Lei Brasileira de Inclusão da Pessoa com Deficiência (Lei nº 13.146/2015) | Define critérios de acessibilidade, comunicação e tecnologia assistiva e especialmente para comprovar que o produto promove autonomia e não cria barreiras de uso. | https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2015/lei/l13146.htm |
| Norma IEC 62368-1 (substituindo 60065 / 60950) | Normativa internacional sobre segurança de equipamentos de tecnologia da informação e áudio/vídeo | https://icertifi.com/iec-62368-certification-replacing-iec-60950-1-and-iec-60065/? |

**Pilar da Interface**
![Patterns](https://github.com/Guilo-s-Community/Interface-Humano-Computador-Guilherme/blob/main/images/imagem_2025-10-12_143807226.png?raw=true)


3. **Metas de Usabilidade**

   1. **Qualitativo**
      
      	- Tela simples
      	- Textos diretos
      	- Feedback constante para o usuário
      	- Ao selecionar um texto, possibilita a acessibilidade para LIBRAS 

   3. **Quantitativo**  

| Metas | Porcentagem | Justificativa |
| ----- | :---- | :---- |
| Facilidade de aprendizado | 10% | Só há a necessidade de aprender se for a primeira vez de uso em um aplicativo de Smart Home, Guilo's Sound seuge a maioria dos padõres de mercado |
| Facilidade de memorização | 10% | Pouca taxa de memorização, apenas em algum passo mais importante, uma dificuldade do usuário ou memorizar a solução de algum erro específico|
| Eficiência | 30% | Importante passo para não exigir muito tempo  e dúvidas do usuário  |
| Eficácia | 30% | Importante passo do para permitir que o usuário siga o fluxo do aplicativo e cumpra todas as tarefas sem erro |
| Satisfação | 20% | Usuário deve se sentir seguro com o aplicativo, não gerar dúvidas e passos incompletos para ter o retorno positivo dele  |
| **Total** | **100%** | Porcentagem |
<!-- TODOs:
- Add exemplos
 -->
