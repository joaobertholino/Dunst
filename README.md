# Dunst — notificações do Hyprland

Configuração pessoal de notificações para Arch Linux com Hyprland, mantida como submódulo de `joaobertholino/dotfiles` em `.config/dunst`.

## Aparência

- Fundo preto `#000000`, texto claro `#F5F5F5` e borda cinza `#262626` de 1 px.
- Cantos retos e fonte Noto Sans 11.
- Nome do aplicativo em cinza, ícones de 24 px e espaçamento interno ampliado.
- Alertas críticos em vermelho `#F87171`.

## Aplicar alterações

O Hyprland inicia o Dunst em sua configuração de inicialização. Com o serviço em execução:

```sh
dunstctl reload
notify-send --app-name=Sistema "Notificação de teste" "Tema preto e cinza do sistema."
```

A configuração usa o tema de ícones `bloom-dark`, com `Adwaita` como alternativa. Instale a fonte Noto Sans ou ajuste a opção `font` para uma fonte disponível.

## Versionamento

O arquivo `dunstrc` é a configuração ativa. Backups e arquivos temporários permanecem locais e são ignorados pelo Git.

Ao alterar esta configuração, publique primeiro o commit deste repositório e depois atualize a referência do submódulo no repositório `dotfiles`.
