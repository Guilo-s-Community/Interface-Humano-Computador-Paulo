# Projeto de Interface Humano-Computador

Projeto apresentado ao Centro Universitário [FEI](https://portal.fei.edu.br/), como parte dos requisitos necessários para aprovação na disciplina de Interface Humano-Computador (CC8122) do curso de Ciencia da Computação, orientado pelo Prof. Dr. [Fagner de Assis Moura Pimentel](https://github.com/fagnerpimentel).

Este projeto se baseia no Trabalho de Conclusão de Curso (TCC) entitulado DETECÇÃO E IDENTIFICAÇÃO DE SOM AMBIENTE PARA AUXÍLIO DOMÉSTICO DE PESSOAS COM DEFICIÊNCIA AUDITIVA sob orientação do Professor Dr. Plinio Thomaz Aquino Junior e desenvolvido pelos seguintes alunos:

- Paulo Vinicius Araujo Feitosa 24.122.042-5
- Guilherme Marcato Mendes Justiça 24.122.045-8

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

1. Identifique os principais concorrentes ou softwares mais utilizados pelo seu público-alvo.
2. Colete informações sobre os concorrentes selecionados.
3. Analise as características e funcionalidades dos concorrentes.
4. Avalie a experiência do usuário (UX).
5. Examine os preços e modelos de negócio.
6. Pesquisa de satisfação do cliente e opiniões.
7. Identifique padrões e tendências no mercado.
8. Elabore relatórios e sumarize os resultados.
9. Extraia pontos positivos e faça recomendações.

### Personas
**Persona primaira (Maria, 58 anos):**
- Perfil: mulher, perda auditiva moderada a severa, mora sozinha em apartamento urbano, aposentada. Usa celular- para mensagens e redes sociais, tem familiar que a visita semanalmente.
- Contexto socioeconômico e cultural: renda média-baixa (aposentadoria), ensino médio completo, prefere soluções de baixo custo e fáceis de usar; valoriza privacidade e autonomia.
- Tecnologia / habilidades: usa smartphone, mas não gosta de configurações técnicas complexas; aceita ajuda de familiares para instalar algo pela primeira vez.
- Necessidades e expectativas: perceber campainha, alarmes, temporizadores de cozinha e choro de bebê/animal (se houver visitas); receber notificações claras (visual e vibratória).

 **Persona secundária (Lucas, 34 anos):** 
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

- Determine o mapa de empatia[^1] de pelo menos uma persona primária e uma sercundária. (Maria, persona primária)
  - O que o usuário vê: Sala de estar com TV (às vezes desligada), cozinhas com micro-ondas, campainha externa visível pela porta, familiares entrando às vezes; smartphone com ícones grandes.
  - O que o usuário ouve: (pouco) percebe vibrações do celular se estiver por perto; costuma não ouvir campainha ou alarmes à distância; ruídos ambientes (trânsito, vizinhos) podem confundir sua percepção. (documento destaca ambientes ruidosos como desafio).
  - O que o usuário diz e faz: Diz que não quer depender sempre de vizinhos; configura preferências básicas no app (com ajuda); tende a manter app aberto na tela principal quando espera visitas.
  - O que o usuário pensa e sente: Deseja segurança e privacidade; teme perder alertas importantes e ser um incômodo para os outros; quer controlar o que é notificado (não quer spam).
  - Dores: Falsos positivos/alertas inúteis; configurações difíceis; medo de exposição (gravações indo para nuvem sem controle).
  - Ganhos: Receber alerta visual + vibração imediato quando algo crítico ocorre; fácil configuração inicial; histórico acessível para revisar quem bateu à porta; maior autonomia.

## Contexto de uso

- Descreva o ambiente em que o serviço ou poduto deve ser utilizado.
- Qual/quais o(s) contexto(s) sociais, econômicos e culturais existentes neste ambiente?
- Quais informações sobre o ambiente, o serviço ou poduto deve guardar antes de iniciar a interação?
- O que normalmente deve estar acontecendo com o ambiente quando o usuário interagir com o serviço ou poduto?

## Jornada do usuário

- Criar uma narrativa para o o seu serviço ou poduto com o usuário.
- Determine o que o usuário realiza desde a primeira até o última interação com o serviço ou poduto.
  - Descreva o que acontece ou pode acontecer passo a passo
  - Como a tarefa começa? Como a tarefa se desenvolve? Como a tarefa termina?


<!--
## Análise de concorrência

- Pesquise serviços ou podutos existentes atualmente que possam realizar o objetivo deste projeto.
- Selecione pelo menos 3 serviços ou podutos diferentes.
- Em relação aos concorrentes, respondam as seguintes perguntas?
  - Existe plataforma similar que atende o mesmo mercado e funcionalidades? Se sim: Quais os pontos positivos? Quais os pontos negativos?
  - Existe plataforma diferente quanto ao serviço, mas que atenda esse mercado? Se sim: Quais os pontos positivos? Quais os pontos negativos?
 -->
 
## Coleta de dados

## Modelo de tarefas

## Design

- Pense nas características de Affordances do seu serviço ou poduto. 
    - Que tipo de acessibilidades devem ser consideradas dentro do seu projeto?
- Discuta o papel das expectativas do usuário no projeto deste serviço ou poduto. Qual a importância e pontos a serem considerados se você quiser vender esse serviço ou poduto?

### Prototipação em baixo nível (papel)
#### Avaliação heurística

### Prtotipação em médio nível (Figma)
#### Avaliação heurística

### Prtotipação em alto nível (React)
#### Avaliação heurística

[^1]: Fonte: Adaptado de <https://hazeshift.com.br/mapa-de-empatia/>

<!-- TODOs:
- Add exemplos
 -->
