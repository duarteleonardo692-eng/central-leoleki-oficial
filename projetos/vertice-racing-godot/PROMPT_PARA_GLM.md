# MISSÃO PARA O GLM — VÉRTICE RACING | EXPANSÃO DE MUNDO ORIGINAL

Você é o líder de **World Building + Level Design + Technical Art** de **VÉRTICE RACING (Godot 4, GDScript)**. Trabalhará em colaboração com GPT-6, que assume a arquitetura do mundo aberto, sistema de eventos, integração de cenas, performance e revisão técnica.

## Primeiro leia
- README.md
- PLANO_MUNDO_ABERTO.md
- CONTRATO_ENTREGA_GLM.md
- FONTES_E_LICENCAS.md
- CONTRATO_GPT6.md

**Não estamos criando o jogo do zero.** A cidade principal existe e o carro possui física funcional (RigidBody3D/Raycast). Preserve totalmente física, controles, câmera, HUD, IA, corrida, carros, garagem, ambiente existente e NIGHTFALL. Não implemente física duplicada, não renomeie scripts existentes sem aprovação, não use Unreal Engine e **não adicione polícia**.

## Visão de experiência
Criar um mundo com atrações e estradas naturais além da cidade: cânions e serra sinuosa, litoral e ilhas por pontes e túneis, estradas alpinas com gelo e um centro de espetáculos com pistas radicais elevadas. Experiência de liberdade, cultura automotiva e eventos de corrida com personalidade 100% **VÉRTICE RACING**. Referências de sensação: festivais automotivos e pistas acrobáticas; **não copiar** desenho, branding, cores registradas, mascotes, letreiros, trilhas, carros ou layout identificável de Forza Horizon / Hot Wheels. Não baixar e reempacotar mapas comerciais de outros jogos.

## Prioridade ABSOLUTA — Sprint 1
Produzir **Serra Vértice**, uma região autossuficiente com:
1. Conector rodoviário da borda da cidade, sem deslocar a cidade atual.
2. Estrada de montanha sinuosa e verossímil com 8–12 curvas distintas, acostamento, barreiras e mirantes, uma ponte e um túnel (pode usar cenas dedicadas se os meshes ainda forem limitados).
3. Uma competição point-to-point e uma pista especial *Skyline Loop* com bankings, túnel de luz, rampa e um salto tecnicamente alcançável, tudo original.
4. Ponto de evento discreto (Marker3D ou Area3D documentado), linha de largada e chegada, checkpoints Marker3D e caminho da IA Path3D; **não alterar EventDirector**.
5. Terreno, colisão funcional, vegetação instanciada, materiais PBR, atenuação de draw calls, LOD/HLOD, distâncias de render e versão leve para RTX 3060.
6. Shader/céu existente NÃO alterado; use seu WorldEnvironment separado apenas na cena standalone.
7. Três capturas reais de viewport Godot, um vídeo curto de percurso se possível, e o ZIP do módulo contendo somente arquivos novos.
8. Changelog, atribuição/licenças por asset, manifest SHA256 e resultado dos testes de importação.

## Sprints seguintes, sem começar antes de entregar Sprint 1
- **Costa das Ilhas**: orla, ponte suspensa, estrada litorânea, ilhotas acessíveis, circuito Sunset Coast.
- **Passagem Boreal**: serra nevada, placas de gelo, pinheiros, visual de aurora e pista noturna; NÃO modificar coeficientes de atrito da física base sem uma API de zona aprovada.
- **Aero Complexo**: pistas grandes, peraltadas, interseções em diferentes níveis, curvas espirais, loop e seções em túneis; modo dedicado para cenários exagerados. Garantir contato com colisores contínuos, sem armadilhas onde o carro caia por falta de colisão.

## Arquitetura obrigatória
Entregue prefabs/cenas Godot independentes em \`res://expansions/serra_vertice/\`; raiz Node3D, nós de spawn/ligação/rotas nomeados conforme CONTRATO_ENTREGA_GLM.md. Não instanciar nem carregar a cidade principal na cena de expansão. Use gatilhos/links data-driven e scripts de integração pequenos; GPT-6 integrará no projeto e conectará EventDirector/RaceEventData. Se não tiver o projeto real, **não invente path ou API do carro**. Entregue uma cena Godot mínima de validação, com nós suficientes para que GPT-6 consiga integrar. Não alegue teste de gameplay se não executou a Godot.

## Qualidade visual/performance
Meta inicial 1920×1080 e ~60 FPS na RTX 3060 12 GB + 5600X + 16 GB RAM, a confirmar com medição no PC. Prefira iluminação realista, materiais PBR e neblina leve a materiais excessivamente emissivos. Texturas de 1K/2K em uso comum; 4K apenas para poucos elementos hero justificados. Desenhe a pista de forma legível e divertidamente desafiadora; estrada suficientemente larga para carros e IA; sem interpenetração com guardrails. Nenhum material depende de tangentes se a malha não possui tangentes. Evite milhares de MeshInstance3D individuais e colisores TriangleMesh em objetos móveis.

## Licenças
Buscar somente assets de licenças verificadas para redistribuição em jogos; registrar URL de página de origem, autor, versão, licença exata, data de verificação e modificações. Priorize Poly Haven, ambientCG e Kenney quando licenças CC0 forem comprovadas para o asset específico. Quaternius: algumas páginas usam QAL v1.0; verificar termos e compatibilidade antes de incluir/republicar arquivos. Nunca usar assets extraídos de jogos comerciais ou imagens de marca como material do jogo.

## Como entregar
Produza ZIP \`Vertice_SerraVertice_GLM_Sprint1_CANDIDATA.zip\` contendo \`expansions/serra_vertice/\`, \`docs/\`, \`preview/\`, \`licenses/\`. No README inclua:
- arquivos criados e cenas de entrada;
- pontos de conexão (anchors) com a cidade, unidades/metragem de escala, transforms necessários;
- nomes de caminhos para evento e IA;
- recomendações de LOD, drawcalls, memória, tris por área;
- erros conhecidos e métricas de Godot REALMENTE MEDIDAS;
- instruções de teste e como desativar sem quebrar a cidade.

**Entregue um protótipo visual e navegável do Sprint 1, não apenas um plano ou uma imagem.** Se o modelo não puder criar GLB, escreva claramente a limitação e entregue a geometria com cenas/scripts procedurais legíveis e instruções executáveis. NÃO declarar concluído até que os arquivos existam e sejam verificados. Nenhuma alteração no repositório privado sem autorização explícita.