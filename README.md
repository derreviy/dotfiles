Представь, что ты переустановил CachyOS с Niri, сидишь в терминале **Fish** на чистой системе и хочешь вернуть все свои настройки.

Вот полная пошаговая инструкция с нуля:

---

### Шаг 1. Генерируем SSH-ключ на новом ПК

Новой системе нужно дать доступ к твоему GitHub:

1. Генерируем ключ:
```fish
ssh-keygen -t ed25519 -C "derreviy7@gmail.com"

```


*(Нажимай Enter на всё или укажи пароль)*.
2. Показываем ключ и копируем его:
```fish
cat ~/.ssh/id_ed25519.pub

```


3. Идём на [GitHub → Settings → SSH Keys](https://github.com/settings/keys), жмём **New SSH Key** и вставляем скопированный текст.

---

### Шаг 2. Скачиваем репозиторий с конфигами

Скачиваем твои дотфайлы во внутреннюю папку `.dotfiles`:

```fish
git clone --bare git@github.com:derreviy/dotfiles.git $HOME/.dotfiles

```

---

### Шаг 3. Включаем алиас `config` в Fish

Чтобы терминал понимал команду `config`:

```fish
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
funcsave config

```

---

### Шаг 4. Достаём твои файлы конфигов в `$HOME`

Выполняем «распаковку»:

```fish
mkdir -p .dotfiles-backup; and for f in (config checkout 2>&1 | string match -r '\s+\K\..*'); mkdir -p .dotfiles-backup/(dirname $f); mv $f .dotfiles-backup/$f; end; and config checkout

```

---

### Шаг 5. Финальная настройка

Скрываем личные файлы из домашней директории, чтобы `config status` показывал только твои дотфайлы:

```fish
config config --local status.showUntrackedFiles no

```

**Готово!** Твои настройки Vim, Niri и других утилит снова на месте.

Если решишь докинуть туда еще и автоустановку пакетов из твоего pkglist.txt, в README можно будет просто добавить одну строчку для разворачивания всего софта в CachyOS:

Фрагмент кода

sudo pacman -S --needed - < pkglist.txt

Теперь репозиторий полностью готов и задокументирован.
