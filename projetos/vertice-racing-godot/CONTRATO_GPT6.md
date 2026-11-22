# GPT-6 — Integração, núcleo de jogo e validação

## Trabalho de GPT-6
1. Criar/ajustar \`RegionRegistry\`, \`RegionStreamer\`, \`WorldConnection\` sobre cenas carregadas em \`PackedScene\`, com distâncias de ativação e histerese para evitar carregamento oscilante.
2. Integrar com \`GameSession\`/\`EventDirector\`/\`RaceEventData\`, sem reescrever física de \`RacerEntity\`.
3. Definir a API entre região, corrida in-world e cena dedicada; preservar estado de retorno, dinheiro, reputação e carro ativo.
4. Integrar checkpoint, barreiras ativáveis e IA somente após validação do núcleo da Etapa 1.
5. Testes automatizados: referências, nomes de anchors, assets, transforms, materiais, colisão estática quando testável; regressão de shaders e tangentes NIGHTFALL.
6. Perfil RTX 3060/5600X/16 GB: perfil Eco/Equilibrado/Alto, orçamento de memória, visibilidade dinâmica e LOD.
7. Revisar qualquer ZIP do GLM sem baixar assets sem licença ou sobrescrever código do jogo.

## Pontos de acoplamento
- GameSession: localização no mundo livre, carro ativo e progressão.
- EventDirector: gatilhos de corrida e troca de cena.
- RacerEntity: adaptador mínimo para teleporte, velocidade, bloqueio de input e ângulo de drift; NÃO manipular torque/suspensão.
- RegionStreamer: regiões derivadas da cena principal com nós independentes.
- Nivel de origem: Cidade principal (não incluída neste repositório público).

## Fases de integração
A. validar SerraVerticeWorld isolada, escala e colisões;
B. alinhar RoadConnectionStart ao ponto oficial na cidade **no projeto privado**;
C. testar streaming e distância de descarregamento com carro em movimento;
D. instalar RaceEventData e gatilho; conferir retorno à cidade;
E. validar IA, checkpoints, resultado, HUD;
F. captura de FPS real, replay, auditoria visual final.

## Segurança de trabalho
- Versão candidata separada, backup e reversão; não mexer em main/release sem aprovação.
- Não usar GitHub Actions como dependência obrigatória (limite reduzido).
- Sem novas senhas: documentação pública legível por URL; conteúdo privado exige autorização OAuth/colaborador; nunca colar tokens.
- Nunca confundir o repositório Unreal com o projeto Godot.
