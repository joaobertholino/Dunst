# Dunst — notificações do Gentoo com Hyprland

Tema pessoal de notificações, mantido como submódulo de [dotfiles](https://github.com/joaobertholino/dotfiles) em `~/.config/dunst`. Integração com a sessão atualizada em **26/10/2026**.

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

No Gentoo atual, `dunst.service` está habilitado em `hyprland-session.target`. O hook Lua do Hyprland chama `/usr/local/bin/gentoo-session-start`, que importa o ambiente gráfico e ativa esse target; o systemd inicia o Dunst. O serviço acompanha a sessão por um drop-in `~/.config/systemd/user/dunst.service.d/session.conf`, com vínculo também a `graphical-session.target`.

A unidade base está em `/etc/systemd/user/dunst.service`, usa `Type=dbus` e o nome `org.freedesktop.Notifications`, e executa `/opt/hyprland/bin/dunst -conf /home/joaob/.config/dunst/dunstrc`. O monitor do compositor encerra o target e seus serviços ao terminar o Hyprland. O helper, a unidade, o drop-in e o monitor são componentes locais externos a este submódulo; clonar apenas o tema não os instala. Consulte a [integração de sessão no dotfiles](https://github.com/joaobertholino/dotfiles#sessão-notificações-e-calendário).

Para aplicar alterações e testar, dentro da sessão gráfica:

```sh
systemctl --user status dunst.service
journalctl --user -u dunst.service -b
dunstctl reload
notify-send --app-name=Sistema 'Notificação de teste' 'Tema preto e cinza do sistema.'
```

Em uma instalação sem a unidade systemd local, o daemon pode ser iniciado diretamente com `dunst -conf "$HOME/.config/dunst/dunstrc"`. Mantenha apenas um daemon responsável pelas notificações da sessão. Com a unidade local ativa, não acrescente outro `dunst` ao autostart do Hyprland. A integração atual foi verificada com uma única instância, encerramento pelo target e restauração ao reativá-lo.

## Versionamento e recuperação

`dunstrc` é a configuração ativa. O `.gitignore` mantém backups e arquivos temporários fora do Git.

A cópia local utiliza `main` e o remoto `personal`. O `~/.local/bin/auto-push.sh` do repositório principal salva e envia este submódulo antes de atualizar o gitlink no pai; o timer do usuário executa a cada hora. Não há timer adicional neste submódulo.

Para manutenção manual, confira `git status`/`git diff`, publique o commit deste repositório e depois atualize sua referência em `dotfiles`. Para recuperação, restaure uma versão conhecida de `dunstrc` e execute `dunstctl reload`.
