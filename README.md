# Dunst — notificações do Gentoo com Hyprland

Tema pessoal de notificações, mantido como submódulo de [dotfiles](https://github.com/joaobertholino/dotfiles) em `~/.config/dunst`. Conferido em **25/10/2026**.

O repositório fornece `dunstrc`; o Dunst e o comando `notify-send` são instalados separadamente. A máquina atual usa o Dunst pré-compilado da pilha Nix e o serviço de usuário `dunst.service`.

## Aparência e posicionamento

- Fundo preto `#000000`, texto claro `#F5F5F5` e borda cinza `#262626` de 1 px.
- Cantos retos e fonte Noto Sans 11.
- Posição superior direita, deslocamento `(10, 32)` e largura de 360 px.
- Espaço de 8 px entre notificações; preenchimento vertical de 12 px e horizontal de 16 px.
- Nome do aplicativo em cinza, título em negrito, ícones de 24 px e texto alinhado à esquerda.
- Urgência baixa em cinza; alertas críticos com texto e borda vermelhos `#F87171`.
- Regra específica de formatação para navegadores, incluindo Zen, Firefox e Chromium.

Os ícones procuram `bloom-dark` e depois `Adwaita`. Instale os temas e a fonte ou adapte essas opções ao seu sistema.

## Instalação e aplicação

No clone do repositório principal:

```sh
git submodule update --init --recursive .config/dunst
```

Para instalação independente, faça backup da configuração existente e clone em um destino vazio:

```sh
git clone git@github.com:joaobertholino/Dunst.git "$HOME/.config/dunst"
```

O módulo de inicialização do Hyprland chama o helper local `/usr/local/bin/gentoo-session-start`, que inicia `dunst.service`. O helper e a instalação binária ficam fora deste repositório.

Para aplicar alterações e testar, dentro da sessão gráfica:

```sh
systemctl --user status dunst.service
dunstctl reload
notify-send --app-name=Sistema 'Notificação de teste' 'Tema preto e cinza do sistema.'
```

Em uma instalação sem a unidade systemd local, o daemon pode ser iniciado diretamente com `dunst -conf "$HOME/.config/dunst/dunstrc"`. Mantenha apenas um daemon responsável pelas notificações da sessão.

## Versionamento e recuperação

`dunstrc` é a configuração ativa. O `.gitignore` mantém backups e arquivos temporários fora do Git.

A cópia local utiliza `main` e o remoto `personal`. O `~/.local/bin/auto-push.sh` do repositório principal salva e envia este submódulo antes de atualizar o gitlink no pai; o timer do usuário executa a cada hora. Não há timer adicional neste submódulo.

Para manutenção manual, confira `git status`/`git diff`, publique o commit deste repositório e depois atualize sua referência em `dotfiles`. Para recuperação, restaure uma versão conhecida de `dunstrc` e execute `dunstctl reload`.
