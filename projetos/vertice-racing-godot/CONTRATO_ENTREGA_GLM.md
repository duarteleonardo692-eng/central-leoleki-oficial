# Contrato técnico de entrega do GLM — módulos Godot 4

## Estrutura exata recomendada
\`\`\`
expansions/serra_vertice/
  SerraVerticeWorld.tscn
  SerraVerticeRace.tscn
  assets/                 # GLB, texturas PBR e materiais somente com licença verificada
  scripts/                # scripts próprios da expansão, sem copiar física existente
  resources/              # configs de eventos/rotas compatíveis com RaceEventData
  README.md
docs/
  ASSET_MANIFEST.csv
  PERFORMANCE.md
  TEST_REPORT.md
  CHANGELOG.md
licenses/
  LICENSES.md
preview/
  serra_overview.png
  serra_road.png
  skyline_loop.png
\`\`\`

## Nós obrigatórios na cena
\`\`\`
SerraVerticeWorld (Node3D)
  TerrainRoot (Node3D)
  RoadsRoot (Node3D)
  CollisionRoot (Node3D)
  PropsRoot (Node3D)
  RegionAnchor (Marker3D)
  RoadConnectionStart (Marker3D)
  RoadConnectionEnd (Marker3D)
  RaceRoutes (Node3D)
    SERRA_SPRINT (Path3D)
      CP_001 (Marker3D)
      CP_002 (Marker3D)
      ...
      FINISH (Marker3D)
  EventSpawns (Node3D)
    SerraSprintTrigger (Marker3D)
  RivalSpawns (Node3D)
    RIVAL_01 (Marker3D)
  DebugCameras (Node3D)
\`\`\`

Na cena dedicada \`SerraVerticeRace.tscn\`, incluir \`PlayerSpawn\`, \`WorldEnvironment\`, \`RaceRoutes\`, \`RivalSpawns\`, pontos de retorno definidos em metadata. Sem nós \`GameSession\` duplicados: o projeto principal gerencia autoloads.

## Regras de conteúdo e escala
- 1 unidade Godot = 1 metro; eixos Y para cima, frente da pista documentada.
- Estrada trafegável contínua, \`StaticBody3D\` e colisão simplificada; superfícies dirigíveis não podem ter buracos.
- \`CollisionShape3D\` para objetos estáticos, sem uso indiscriminado de malha complexa em vegetação e props.
- No máximo 1–2 sets de materiais por segmento lógico quando possível; textura PBR coerente.
- Evitar ausência de UV0, normais, tangentes se normal map requerido; verificar import options.
- Light probes/WorldEnvironment: não alterar ambiente existente; dedicado sim.
- Sem mudanças em \`project.godot\`, \`PlayerCar.gd\`, fisica, HUD, saves, NIGHTFALL ou cenas centrais.
- Não criar modelos/códigos licenciados de terceiros sem licença documental.

## Evidências
- \`TEST_REPORT.md\`: Godot versão, hardware usado, como reproduzir, erros, warnings e cenas efetivamente executadas.
- \`ASSET_MANIFEST.csv\`: path, origem, autor, URL, licença, data, modificação, sha256.
- \`PERFORMANCE.md\`: FPS média, p1 mínimo, VRAM, drawcalls, triângulos, cenário, resolução, renderer; preencher "não medido" se não executou.
- \`CHANGELOG.md\`: arquivos adicionados/modificados e riscos.
- Preview capturado do render real sempre que possível; não vender concept art como imagem de gameplay.

## Critérios de rejeição
Pacote não carrega no editor, usa assets sem licença, substitui carro/física/cidade, só contém mockup, geometria atravessável, layout de marca copiada, retorna log genérico em vez de evidências.

## Transferência
GLM publica ZIP/sources como artefato acessível ao proprietário, não no site público automaticamente. GPT-6 analisa em branch de integração privada somente após receber arquivos e validar direitos.