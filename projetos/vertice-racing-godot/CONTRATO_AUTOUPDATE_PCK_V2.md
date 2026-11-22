# VÉRTICE RACING — Contrato Auto Updates V2 (Godot 4.7.2)

**Status:** sistema de carregamento preparado; ainda exige uma instalação inicial no projeto e teste no Windows. Não afirmar que está homologado antes de exportar a base e testar.

## Entrega para o jogador
- VERTICE_RACING.exe e VERTICE_RACING.pck juntos; pasta updates/ no mesmo diretório.
- Novos pacotes nomeados 010_serra_vertice.pck, 020_costa_das_ilhas.pck, 030_hotfix_camera.pck.
- Na próxima inicialização, carregar arquivos PCK/ZIP de recursos Godot, por ordem alfabética.
- O usuário não precisa abrir editor nem compilar o EXE para INSTALAR packs compatíveis.

## Contrato técnico
- PackBoot.gd deve ser o primeiro Autoload e executar ProjectSettings.load_resource_pack(path,true) em _init() antes da cena principal.
- Primeira instalação exige exportar o PCK base uma vez; autoloads já inicializados, binários nativos e mudança de configuração não suportam overlay confiável.
- ZIP-fonte do GLM não se torna automaticamente resource pack: equipe deve gerar PCK Godot 4.7.2, usando paths relativos res:// e testando.
- Carregar um PCK apenas monta recursos; mapas novos precisam de RegionRegistry/RegionStreamer na base para registrar/instanciar cenas. SerraRuntime atual exige reparos na ligação com cidade.
- Não duplicar física do carro, project.godot, GameSession ou autoloads centrais em packs.

## Integridade, reversão e segurança
- Sidecar opcional NOME.pck.sha256 com hash hexadecimal; opcional NOME.pck.json com game_id=vertice-racing-godot, pack_protocol=1, sha256.
- Status em updates/ULTIMO_CARREGAMENTO.json. Falha em um PCK não deve substituir o EXE.
- Criar updates/DESATIVAR_ATUALIZACOES.txt para modo seguro. Para desativar um pack, mover/renomear e reiniciar.
- PCK pode executar scripts: SHA detecta corrupção, não prova autoria; use fontes confiáveis.

## Entrega que pedimos ao GLM
1. ZIP-fonte e PCK Godot compatível da Serra, incluindo SHA e dependências.
2. Prévia real e testes reproduzíveis, TEST_REPORT.md, CHANGELOG.md e licenças.
3. Não mesclar na release principal nem editar repositório Unreal sem aprovação.