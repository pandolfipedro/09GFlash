# 09G Flash

Ferramenta visual para programação de módulos de câmbio Aisin 09G / AQ250 por interface J2534.

Este repositório contém a distribuição compilada para Windows. Para abrir, execute **[09GFlash.exe](09GFlash.exe)**; Python e os recursos da interface já estão incluídos no executável.

| Distribuição | Informação |
| --- | --- |
| Versão do produto | 1.0.0 |
| Compilação distribuída | 07/11/2026 |
| Organização deste repositório | 09/11/2026 |
| Arquitetura do executável | Windows x86 — 32 bits |
| Tamanho do executável | 16.103.427 bytes, aproximadamente 16,1 MB |
| Comunicação | J2534 / CAN / TP2.0 / KWP2000 |

## Requisitos

- Windows com suporte a aplicativos de 32 bits.
- Adaptador J2534 e o driver PassThru de **32 bits** instalado pelo fabricante. Em Windows de 64 bits, a DLL usada por este executável também deve ser de 32 bits.
- Arquivo de programação compatível com o módulo e a aplicação.
- Conexão com a internet para baixar arquivos pelo catálogo. Arquivos locais podem ser carregados sem internet.

## Recursos

- Identificação do módulo e análise do arquivo antes da gravação.
- Autenticação com o SA2 extraído do próprio SGO, para os arquivos 09G927 e 09G997 aceitos pelo leitor.
- Reconhecimento automático dos layouts de dados associados a H76 e H97, sem habilitação manual na interface.
- Progresso por blocos confirmados, quantidade enviada, velocidade, tempo de envio estimado e tempo total.
- Etapas separadas para transferência, verificação de checksum e encerramento da sessão.
- Catálogo lateral de arquivos Audi e Volkswagen, com filtros, biblioteca local e downloads sob demanda.
- Registro da sessão e acesso à pasta de logs.

A leitura do arquivo e sua presença no catálogo não comprovam compatibilidade com o veículo ou com o hardware. O suporte do leitor a um layout não garante que outro layout possa ser gravado no mesmo módulo.

## Como usar

1. Abra `09GFlash.exe`.
2. Na página **Programação**, selecione o adaptador. Se necessário, abra **Conexão e ajustes** e use **Escolher DLL…** para indicar a biblioteca PassThru de 32 bits.
3. Use **Conectar e identificar** para consultar o módulo quando ele estiver respondendo.
4. Clique em **Selecionar SGO…**, ou escolha um arquivo na página **Arquivos 09G**.
5. Confira o software, o layout, a autenticação e **Detalhes do arquivo**.
6. Com a alimentação estabilizada e a conexão conferida, clique em **Iniciar gravação**. Esse botão inicia a programação real.
7. Aguarde o resultado final, incluindo checksum e encerramento. **100% de transferência não significa que todas as etapas terminaram.** Preserve a alimentação e a conexão durante a operação.
8. Consulte **Registro da sessão** ou **Pasta de logs** para conferir o resultado.

Após a programação, confira a identificação e os códigos de falha com o equipamento de diagnóstico apropriado.

## Arquivos de firmware

Esta distribuição contém apenas a ferramenta compilada e sua documentação. Os SGOs são obtidos separadamente.

Na página **Arquivos 09G**, selecione um item e use **Baixar arquivo** quando houver link disponível. Depois, **Usar arquivo** carrega o SGO para análise; a programação é iniciada na página **Programação**.

O catálogo está incluído no executável e corresponde à consulta de **07/11/2026**. A disponibilidade dos links pode mudar. Pelo botão **vagflashfile.com ↗** e por **Copiar código**, pesquise o número da peça **sem a versão e sem a extensão**, por exemplo `09G997750E`.

- Site de download: [vagflashfile.com](https://vagflashfile.com/).
- Referências do catálogo: [Audi](https://vag-flashinfo.de/site/filelist?brand=a) e [Volkswagen](https://vag-flashinfo.de/site/filelist?brand=v).

Os downloads são validados quanto à estrutura SGO/SA2 e aos metadados disponíveis. Um SHA-256 verifica integridade; não é uma assinatura de autenticidade do fabricante.

O leitor desta compilação trabalha com SGO plain de um único Datenblock. Para carregar BIN, é necessário o SGO correspondente, conforme as verificações do programa.

## Pastas utilizadas

| Conteúdo | Local |
| --- | --- |
| Executável | Pasta deste repositório |
| Firmware baixado | `firmwares` ao lado do EXE |
| Biblioteca alternativa, se a pasta do EXE não permitir gravação | `%LOCALAPPDATA%\09GFlash\firmwares` |
| Logs | `%LOCALAPPDATA%\09GFlash\logs` |
| Configurações locais | `%LOCALAPPDATA%\09GFlash` |

Firmwares, logs e arquivos de trabalho estão excluídos do versionamento pelo `.gitignore`.

## Conexão e progresso

Se nenhum adaptador aparecer, confira a instalação do driver e a arquitetura da DLL. A mensagem de DLL não encontrada pode ser resolvida indicando o caminho em **Escolher DLL…**.

Os perfis `normal`, `rescue` e `bench` ajustam os tempos da comunicação. O perfil `bench` continua usando o protocolo de diagnóstico; não implementa acesso direto ao boot do processador.

O tempo de gravação depende do adaptador, driver, módulo e tamanho dos dados. A estimativa exibida refere-se ao envio; verificação e encerramento podem continuar após essa contagem.

## Integridade do executável

O arquivo `SHA256SUMS.txt` contém o hash desta distribuição. Para conferir no PowerShell, dentro da pasta:

```powershell
Get-FileHash -LiteralPath .\09GFlash.exe -Algorithm SHA256
```

O hash esperado também está em `release.json`. Esta cópia mantém exatamente o executável compilado em 07/11/2026; a organização do repositório não recompilou nem modificou o programa.

## Conteúdo do repositório

```text
09GFlash.exe       Aplicativo compilado
README.md          Instruções de uso
release.json       Metadados e hash da distribuição
SHA256SUMS.txt     Hash SHA-256 do executável
.gitignore         Exclusões de arquivos gerados durante o uso
.gitattributes     Tratamento de texto e binário pelo Git
```

O repositório é local. A branch inicial é `main`, com uma cópia identificada pela tag `v1.0.0`. Para consultar o histórico:

```powershell
git status
git log --oneline
git tag
```
