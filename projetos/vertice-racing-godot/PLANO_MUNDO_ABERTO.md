# Plano mestre de expansão — VÉRTICE RACING

## Topologia desejada

\`\`\`
                    [PASSAGEM BOREAL / neve]
                             |
                        [SERRA VÉRTICE]
                             | \
          [AERO COMPLEXO]*   |  [DESFILADEIRO]
                  \          |
                 [CIDADE PRINCIPAL] ---- [COSTA DAS ILHAS]
                        |                        |
                 [PORTO INDUSTRIAL] ------- [PONTE ALTA]
\`\`\`

\`*\` O Aero Complexo pode ser standalone scene (transição cinematográfica) enquanto as demais estradas são regiões conectadas por streaming.

## Escopo de regiões
| Região | Atmosfera | Exploração | Evento especial |
|---|---|---|---|
| Serra Vértice | floresta/rochas, pontes e túneis | curvas longas, mirantes, acesso urbano | Hill Sprint + Skyline Loop |
| Costa das Ilhas | litorânea ao pôr do sol | ponte, marina, costeira e ilhas | Sunset Coast Sprint |
| Passagem Boreal | neve, gelo, luz fria | alta montanha e lago | Northern Lights Rally |
| Aero Complexo | pista radical original | túneis, bankings, curvas em camadas | Aero Sprint Cup |

## Decisão técnica de mundo
1. **Cidade original preservada**: cena separada e persistente de referência.
2. **Módulos por região**, carregados por \`PackedScene\` sob \`RegionRoot\`; cada região tem um \`RegionAnchor\`, portais rodoviários e pontos de ativação; a importação não deve duplicar \`WorldEnvironment\` sem isolamento.
3. **In-world**: estradas ligadas ao mundo livre, tráfego e checkpoints retornam ao estado original ao fim do evento.
4. **Standalone**: pista cenográfica especial abre em cena dedicada, GameSession preserva posição/estado e devolve o carro ao mundo livre.
5. **Sem costuras**: conectar a região via transforms e nós \`RoadConnectionStart\`/\`RoadConnectionEnd\`; tolerância/alinhamento definidos no Inspector; nunca mover a cidade inteira para encaixar região.
6. **Públicos de performance**: carregamento gradual, MultiMesh para vegetação, LOD, materiais instanciáveis, colisão simplificada, occlusion/ranges, lightmap quando couber; textura PBR consistente.

## Interfaces para integração
\`RegionAnchor\`: Marker3D raiz do novo módulo; \`RoadConnectionStart\`: Marker3D para conectar à cidade; \`RoadConnectionEnd\`: Marker3D para próxima expansão; \`RaceRoutes/<id>\`: Node3D com \`Path3D\`, checkpoints ordenados \`CP_001\`, \`CP_002\` etc; \`EventSpawns/<id>\`: Marker3D de gatilho; \`PlayerSpawn\`: Marker3D para cenas dedicadas; \`RivalSpawns\`: Marker3D de IA.

## Critérios de aceitação para cada zona
- A cena abre no Godot 4 sem referências quebradas.
- O carro existente anda sobre toda a rota, sem buracos, colisores invertidos e escalas erradas.
- O traçado é legível em alta velocidade com largura suficiente para a IA.
- Mudança de região e retorno não perdem perfil ou posição.
- Relatório de FPS real em 1080p em sessão longa, ausência de mensagens críticas no debugger.
- NDA/IP: projeto e cidade proprietários não incluídos nesta área pública.

## Ordem de integração
Sprint 1: Serra -> revisão GPT-6 -> validação pelo proprietário -> Costa -> Boreal -> Aero.