# VÉRTICE RACING — Sala de colaboração Godot (GPT-6 + GLM)

**Iniciado em 22 de novembro de 2026.** Área pública de instruções para colaboração. Este diretório NÃO é o repositório do jogo e NÃO contém sua cidade privada, modelos proprietários ou credenciais. A branch é separada da main da Central Leoleki; não modifica o site público.

## Objetivo
Criar expansões 3D **originais** para o jogo de corrida e exploração **VÉRTICE RACING** em Godot 4: cidade principal já existente, montanhas, costa, ilhas e região de neve, além de pistas elevadas e cinematográficas. Visual convincente, direção arcade-realista, 60 FPS como meta a medir numa máquina Ryzen 5 5600X, RTX 3060 12 GB, 16 GB RAM.

## Responsabilidade
- **GLM**: level design, terreno, malhas, texturas licenciadas, pistas, colisores e prévias; integrar somente via contratos.
- **GPT-6**: arquitetura de eventos, integração à cidade, loader de regiões, validação estática, revisão de qualidade e otimização.
- **Proprietário**: teste do projeto completo no Godot e aprovação de integração. Nada de publicar release/jogo sem aprovação.

## Contexto preservado
- A física do carro (RigidBody3D / Raycast) já existe e funciona: não reescrever.
- Sistema de eventos Etapa 1 previsto: GameSession, EventDirector, RaceEventData, RacerEntity, EventWorldTrigger.
- Evitar regressão de problemas NIGHTFALL, tangentes, shaders, céu dinâmico, tráfego, HUD e câmera.
- Nunca introduzir polícia nem perseguições.
- O repositório Unreal é independente: NÃO enviar assets/código dessa branch para o projeto Unreal.

## Arquivos neste diretório
1. [PROMPT_PARA_GLM.md](PROMPT_PARA_GLM.md): ordem de missão pronta para copiar.
2. [PLANO_MUNDO_ABERTO.md](PLANO_MUNDO_ABERTO.md): biomas, rotas e integração.
3. [CONTRATO_ENTREGA_GLM.md](CONTRATO_ENTREGA_GLM.md): estrutura, nomes e testes exigidos.
4. [CONTRATO_GPT6.md](CONTRATO_GPT6.md): tarefas de integração e responsabilidade técnica.
5. [FONTES_E_LICENCAS.md](FONTES_E_LICENCAS.md): como encontrar e usar assets externos sem violar direitos.

## Primeiro marco
GLM começa **Serra Vértice**, com estrada de ligação à cidade, **UMA** pista especial e um corredor de exploração. Não produzir quatro mapas gigantes simultaneamente. Cada entrega deve ser jogável isoladamente e com versões de LOD.

## Acesso sem senha nova
Este conteúdo de instruções está em repositório GitHub **público**: o GLM pode ler por URL sem usuário/senha. Para abrir futuramente código/arquivos **privados** do jogo, cada serviço precisa receber acesso autorizado (OAuth ou colaborador); NÃO copiar senha nem token para prompts.