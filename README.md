# ChatGPT para Arch Linux

**Português** | [English](README.en.md)

Este repositório empacota o aplicativo desktop oficial do ChatGPT, distribuído
pela OpenAI em formato `.deb`, como um pacote nativo do Arch Linux gerenciado
pelo `pacman`.

O projeto é específico para o ChatGPT. Ele não é um conversor genérico de
pacotes Debian.

Durante o empacotamento, os arquivos do aplicativo são preservados, enquanto os
scripts que configurariam um repositório APT no Debian não são executados no
Arch Linux.

## Requisitos

- Arch Linux x86_64
- Grupo `base-devel`, que fornece o `makepkg` e as ferramentas de compilação
- Arquivo oficial `chatgpt_amd64.deb`
- `pacman-contrib` opcional, para usar o comando `updpkgsums`

## Preparar o arquivo de origem

Coloque o `.deb` na raiz deste repositório com o nome:

```text
chatgpt_amd64.deb
```

O arquivo é ignorado pelo Git porque é grande e distribuído separadamente pela
OpenAI.

## Gerar o pacote Arch

Entre na pasta do repositório e execute:

```bash
makepkg -sf
```

O `makepkg` valida o SHA-256 configurado no `PKGBUILD`, verifica as dependências
e gera um arquivo semelhante a:

```text
chatgpt-26.814.41957-1-x86_64.pkg.tar.zst
```

Os diretórios `src/` e `pkg/`, assim como o pacote gerado, são artefatos locais
e estão incluídos no `.gitignore`.

## Instalar ou atualizar

Para instalar o pacote gerado, ou atualizar uma instalação existente:

```bash
sudo pacman -U "$(makepkg --packagelist)"
```

Também é possível compilar e instalar em uma única operação:

```bash
makepkg -sfi
```

Depois, abra **ChatGPT** pelo menu de aplicativos ou execute:

```bash
chatgpt
```

## Atualizar para uma nova versão do ChatGPT

Este é um pacote local, portanto `sudo pacman -Syu` atualiza suas dependências,
mas não baixa automaticamente uma nova versão do ChatGPT.

Quando a OpenAI publicar uma versão nova:

1. Substitua `chatgpt_amd64.deb` pelo novo arquivo.
2. Atualize `pkgver` no `PKGBUILD` com a versão do novo `.deb`.
3. Atualize o SHA-256.
4. Gere e instale o novo pacote.

Com `pacman-contrib` instalado:

```bash
updpkgsums
makepkg -sf
sudo pacman -U "$(makepkg --packagelist)"
```

Sem `updpkgsums`, obtenha o checksum manualmente:

```bash
sha256sum chatgpt_amd64.deb
```

Copie o valor exibido para o campo `sha256sums` do `PKGBUILD` antes de executar
o `makepkg`.

O `pacman` reconhece o pacote já instalado como `chatgpt` e realiza a atualização
preservando as configurações do usuário.

## Remover

```bash
sudo pacman -Rns chatgpt
```

## AppArmor

O perfil `/etc/apparmor.d/chatgpt` fornecido no `.deb` é preservado no pacote,
mas só tem efeito quando o AppArmor está instalado e habilitado no sistema.
