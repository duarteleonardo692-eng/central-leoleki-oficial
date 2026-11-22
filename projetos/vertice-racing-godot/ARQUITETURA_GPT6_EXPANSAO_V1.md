# VÉRTICE RACING — Arquitetura GPT-6 da expansão | Documento de integração V1
Data: 2026-11-22

**ESTADO:** projeto de arquitetura, SEM integração no jogo e SEM auditoria dos binários do GLM. Não confundir este plano com artefato jogável. Versão Desktop UI V1.2 segue em teste separadamente, e **nenhum asset/mapa externo será colocado na build atual**.

## 1. Limites e responsabilidades
- **GLM / arte:** Serra Vértice Sprint 1 (cenário, terreno, estrada, pistas, materiais, colisão, eventos e pontos de entrada), com ZIP + testes + licenças; outros biomas depois.
- **GPT-6 / arquitetura:** RegionRegistry, RegionStreamer, WorldConnection, vínculo com GameSession/EventDirector/RaceEventData e pipeline PCK, auditoria de colisores/UV/tangentes, otimização, QA.
- **NÃO ALTERAR:** física RigidBody3D/Raycast, suspensão, controles, câmera, HUD em estabilização, céu global, tráfego principal, veículos, menus e saves, sem aprovação.
- **Sem polícia**; identidade própria sem copiar marcas e pistas reconhecíveis de jogos existentes.
- Cidade principal persistente: a cena atualmente executada pelo usuário é mundo_v30.tscn (caminho res:// exato será confirmado no projeto real).

## 2. Fluxo do mundo
\`\`\`
          Passagem Boreal [Sprint posterior]
                   |
             Serra Vértice [Sprint 1]
                   | (estrada dirigível / conector)
    Aero Complexo - Cidade principal -------- Costa das Ilhas [posterior]
    (evento cena)       |                       |
                   Porto/Industrial ----- Ponte Litorânea

\`\`\`
Serra, Costa e Boreal são regiões *conectáveis* por estrada e descarregadas/carregadas no entorno do jogador. Aero Complexo pode ser prova de cenário dedicado com retorno ao ponto de origem. A topologia é projeto, não afirma que os mapas existem.

## 3. Árvore recomendada do jogo
\`\`\`
FreeRoamWorld (Node3D)                 # não duplicar nem substituir cena principal
  OriginalCityRoot (existente)
  PlayerVehicle (existente)
    RacerEntity (interface, sem segunda física)
  RegionRoot (Node3D)
  RegionIntegration (Node)
    RegionRegistry (Node)
    RegionStreamer (Node)
    WorldConnection (Node)
    RegionDiagnostics (Node)
  EventTriggers (Node3D)
  RaceRouteRegistry (Node)
  PersistentUI (CanvasLayer)          # HUD/Menu Desktop V1.2, após homologação
\`\`\`

**Autoloads reutilizados**: GameSession (posição/estado/garagem/saves), EventDirector (FREE_ROAM, PREVIEW, TRANSITIONING, COUNTDOWN, RACING, RESULTS). Nada de segundo autoload ou scripts substituindo APIs já existentes.

## 4. Contrato dos módulos GLM
\`\`\`
res://expansions/serra_vertice/
  SerraVerticeWorld.tscn             # Node3D; terreno, estradas e obstáculos
  SerraVerticeRace.tscn              # cena exclusiva Skyline Loop (opcional)
  assets/
  scripts/
  resources/
  README.md
\`\`\`
Nós exigidos em SerraVerticeWorld:
- \`RegionAnchor\` (Marker3D): referência de coordenadas;
- \`RoadConnectionStart\`, \`RoadConnectionEnd\` (Marker3D): conexões geométricas;
- \`TerrainRoot\`, \`RoadsRoot\`, \`CollisionRoot\`, \`PropsRoot\` (Node3D);
- \`RaceRoutes/SERRA_SPRINT\` (Path3D), checkpoints \`CP_001..FINISH\`;
- \`EventSpawns/SerraSprintTrigger\`, \`RivalSpawns/RIVAL_01\`.

Cenas standalone têm \`PlayerSpawn\`, \`WorldEnvironment\` próprio, \`RaceRoutes\`, \`RivalSpawns\`. Não adicionar \`WorldEnvironment\` ativo ao mesmo tempo que o da cidade durante free roam.

## 5. RegionManifest / Registro
Propor resource Godot 4 \`RegionManifest extends Resource\`, com:
\`region_id: StringName\`, \`scene_path: String\`, \`entry_anchor: NodePath\`, \`exit_anchor: NodePath\`, \`visual_load_radius_m: float\`, \`unload_radius_m: float\`, \`max_visible_distance_m: float\`, \`requires_game_version: String\`, \`dependencies: PackedStringArray\`, \`license_manifest_path: String\`.

Nunca aceitar o caminho externo como verdade sem validar: resource deve existir, contagem de dependências precisa estar completa, nomes de âncora corretos, versão compatível. Descarregar só quando suficientemente longe (histerese), sem carros/IA dentro da região, e sem corrida ativa nessa região.

## 6. Streaming sem travamentos
1. Pré-aviso: quando jogador se aproxima da conexão, prefetch dos recursos por \`ResourceLoader.load_threaded_request\`.
2. Quando \`load_threaded_get_status\` indicar prontidão, instanciar \`PackedScene\` sob \`RegionRoot\` na thread principal e ligar colisões antes de tornar o corredor acessível.
3. Renderização e colisão de anéis regionais: distância próxima = colisão + materiais completos; média = LOD + redução de props; longe = impostor/horizon, sem collider individual.
4. Se não carregar, manter entrada bloqueada por barreira ambiental coerente e mensagem discreta; **nunca deixar o carro cair em um buraco de terreno**.
5. Não colocar cena de região inteira numa única MultiMesh gigante quando precisa de streaming setorizado.

## 7. Posição e conexão
- Preservar transform da cidade; alinhar \`RoadConnectionStart\` do módulo ao \`CityRoadExit\` do mapa original usando delta de transforms. Não deslocar cidade nem player arbitrariamente.
- Cada saída deve respeitar continuidade da superfície dirigível (largura, tangência, altura, guardrails) e colisão.
- Trânsito e IA não devem entrar em bloco não carregado; anchors só são públicos após validação da colisão.

## 8. Eventos e cenas especiais
- **In-world**: EventWorldTrigger -> EventDirector -> RaceRoute -> checkpoints e IA; a cidade continua carregada. Barreiras temporárias ativadas apenas na corrida; restauração ao final.
- **Standalone**: salvar via GameSession transform, carro ativo, estado de progressão; fazer fade curto + cena \`SerraVerticeRace\`, depois retornar ao transform original. Troca de ambiente isolada.
- RaceEventData referencia \`event_id\`, \`RaceType\`, \`SceneMode\`, \`route_path\` / \`dedicated_scene_path\`; não instanciar nova física ou copiar scripts de carro.

## 9. Colisão, arte e otimização
- Pista e guardrails precisam de colisão contínua antes da navegação; objetos urbanos não bloqueantes devem ser identificados em relatório, sem declarar 100%.
- \`StaticBody3D\` com shapes simples para estrutura estática; instâncias de árvores podem ter tronco com cilindro/cápsula simplificados; vegetação distante sem colisores.
- Materiais PBR com UV/normais/tangentes válidas quando map normal usado. Validar Godot-import, não apenas glTF JSON.
- Texturas preferencialmente 1–2K, instâncias e LOD em props; 4K somente elementos hero.
- Meta 1080p/60 FPS na RTX 3060 12GB, Ryzen 5 5600X, 16GB RAM **não medida**. Os perfis Eco/Equilibrado/Alto e orçamento de VRAM serão calibrados com build real.

## 10. Distribuição e atualizações
- Artefatos do GLM são **ZIP de fonte** (auditáveis). Após integração, gerar **PCK de expansão** assinado ou acompanhado de manifesto + hash SHA-256.
- Inicializador deve carregar \`updates/*.pck\` cedo, antes de referências aos recursos, via \`ProjectSettings.load_resource_pack\`. Carregar PCK é recurso suportado em Godot 4, mas **nem toda alteração pode ser distribuída sem substituir EXE**.
- Novo mapa autônomo geralmente pode ser conteúdo PCK; mudanças no loader/base/native binaries podem exigir atualização do executável.
- Manter fallback/backups; checksum, versão mínima, ordem de packs, conflitos de path e falhas de importação.
- Não embutir arquivos de cidade proprietária em repositório público.
- **Não gerar/instalar agora**: aguardar teste da build Desktop UI V1.2.

## 11. Gate de aceite da Serra — obrigatório
**Entrega mínima:** \`Vertice_SerraVertice_GLM_Sprint1_CANDIDATA.zip\` com cenas, scripts/malhas reais, \`docs/ASSET_MANIFEST.csv\`, \`docs/TEST_REPORT.md\`, \`docs/PERFORMANCE.md\`, \`docs/CHANGELOG.md\`, \`licenses/LICENSES.md\` e 3 capturas verdadeiras de viewport. Ideal: vídeo curto percorrendo a pista.

**Auditoria GPT-6 no recebimento:**
1. inventário de arquivo/tamanho/hash; 2. inspeção de geometria, materiais/licenças; 3. validação das cenas Godot e anchors; 4. alinhamento estrada/cidade e colisão; 5. performance/profiling; 6. teste contra o sistema de eventos e HUD atuais; 7. homologação exclusiva do proprietário.
**NÃO DECLARAR integração concluída nem FPS medido sem abrir a build real no Godot**.

## 12. Situação desta revisão
No momento de escrever, o GitHub público de colaboração contém apenas documentação produzida por GPT-6; não há ZIP/cenas/GLB/capturas de entrega do GLM nesse diretório. Este documento representa o projeto técnico de integração, não uma revisão dos arquivos finalizados pelo GLM.