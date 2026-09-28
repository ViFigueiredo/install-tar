# install-tar

Instalador automático de pacotes `.tar.gz` / `.tgz` / `.zip` com detecção de tipo de build.

## Uso

```bash
install-tar pacote.tar.gz          # arquivo local
install-tar https://exemplo/foo.tgz # URL
install-tar a.tar.gz b.zip         # vários de uma vez
install-tar-gui                    # seletor de arquivo + progresso (KDE)
install-tar-gui --settings         # tela de configurações
```

## Detecção automática (ordem)

| Sinal | Sistema | Instala em |
|---|---|---|
| `Cargo.toml` | cargo (rust) | `~/.local/bin` |
| `go.mod` | go | `~/.local/bin` |
| `pyproject.toml` / `setup.py` | pipx/pip | venv/prefix |
| `meson.build` | meson | prefix |
| `CMakeLists.txt` | cmake | prefix |
| `configure` | autotools | prefix |
| `Makefile` | make | prefix |
| `*.whl` | pip | prefix |
| `package.json` + `bin` | npm | prefix |
| ELF/Mach-O executável | binário pré-compilado | `~/.local/bin` |

Aplica correção de `Exec=` em arquivos `.desktop`, copia ícones e atualiza o menu do KDE.

## Configuração

Arquivo: `~/.config/install-tar/config` (editável pela GUI ou à mão, veja `config.example`).

| Chave | Default | Descrição |
|---|---|---|
| `PREFIX` | `~/.local` | diretório de instalação |
| `JOBS` | nº de núcleos | compilação paralela |
| `CLEANUP` | `1` | remove temporários após instalar |
| `UPDATE_MENU` | `1` | atualiza menu de aplicativos |
| `DOWNLOAD_DIR` | `~/Downloads` | pasta inicial do seletor |

## Instalação

```bash
mkdir -p ~/bin
cp install-tar install-tar-gui ~/bin/
chmod +x ~/bin/install-tar*
cp ~/.local/share/applications/install-tar.desktop ~/.local/share/applications/ 2>/dev/null || true
```

## Requisitos

- Linux com `tar`, `file` (essenciais)
- GUI: `kdialog` (KDE) + `notify-send`
- Por build system: os respectivos toolchains (detectados em runtime)