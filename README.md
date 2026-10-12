# Сайт на Quarto с формулами LaTeX

Готовый стартовый проект для GitHub Pages: русская навигация, светлая тема
Cosmo, тёмная Darkly, поиск, оглавление, список заметок, формулы MathJax,
нумерация уравнений и пример статьи по физике.

В автоматической сборке зафиксирован Quarto 1.10.19. Используйте эту же
версию локально, чтобы результат совпадал. Проверка проекта описана
в `VALIDATION.txt`.

## 1. Установка Quarto на Arch Linux

Для HTML-сайта достаточно самого Quarto. R, RStudio, Jupyter и TeX Live
для примеров этого проекта не требуются.

Ниже — установка официального готового бинарного выпуска для Linux x86_64
без прав root. Проверьте архитектуру командой `uname -m`:
для этих команд ответ должен быть `x86_64`.

Сначала скачайте архив в отдельный каталог:

```bash
mkdir -p ~/Downloads/quarto-setup
cd ~/Downloads/quarto-setup
curl -fL https://github.com/quarto-dev/quarto-cli/releases/download/v1.10.19/quarto-1.10.19-linux-amd64.tar.gz -o quarto-1.10.19-linux-amd64.tar.gz
```

Проверьте контрольную сумму:

```bash
sha256sum -c <<'CHECKSUM'
8e38c2b2df6a6dbd369ea16e15acded6894545258d13a02f777f81ad18fd7973  quarto-1.10.19-linux-amd64.tar.gz
CHECKSUM
```

Если проверка успешна (`OK` или `ЦЕЛ`), распакуйте архив и добавьте Quarto
в PATH текущего терминала:

```bash
mkdir -p ~/.local/opt
tar -xzf quarto-1.10.19-linux-amd64.tar.gz -C ~/.local/opt
export PATH="$HOME/.local/opt/quarto-1.10.19/bin:$PATH"
quarto --version
```

Чтобы команда работала и после закрытия терминала, добавьте строку
`export PATH="$HOME/.local/opt/quarto-1.10.19/bin:$PATH"` в `~/.bashrc`
для Bash или в `~/.zshrc` для Zsh. Для других оболочек способ настройки
PATH отличается.

Официальные загрузки для других систем и архитектур:
https://quarto.org/docs/get-started/

Если Quarto уже установлен и `quarto --version` работает, повторная
установка не нужна; при другой версии возможны различия в результате.

## 2. Локальный запуск

Распакуйте `quarto-site.zip` и откройте терминал внутри каталога
`quarto-site` — в нём должен находиться `_quarto.yml`.

```bash
quarto preview
```

Quarto покажет адрес локального сервера и откроет браузер. При сохранении
изменений в исходниках страница будет обновляться. Остановка — `Ctrl+C`.

Для обычной сборки без сервера:

```bash
quarto render
```

Готовый HTML появится в `_site`. Эту папку генерирует Quarto;
вносите изменения в `.qmd`, `_quarto.yml` и `styles.css`.

## 3. Первая публикация на GitHub Pages

1. Создайте на https://github.com/new пустой **Public**-репозиторий
   `my-site`, без README, лицензии и `.gitignore`: необходимые файлы уже
   есть в проекте. Если репозиторий у вас уже существует и содержит файлы,
   сначала клонируйте его и перенесите файлы проекта в его корень;
   не используйте `git init` и `git remote add` повторно.
2. В терминале внутри `quarto-site` выполните команды ниже, заменив
   `USERNAME` своим логином, а `my-site` — выбранным именем репозитория.
   Предполагается, что Git установлен и авторизация на GitHub уже настроена.

```bash
git init -b main
git add .
git commit -m "Create Quarto website"
git remote add origin https://github.com/USERNAME/my-site.git
git push -u origin main
```

Если Git просит имя и email, задайте их для этого репозитория и повторите
`git commit` и следующие команды:

```bash
git config user.name "Ваше имя"
git config user.email "Ваш email для коммитов"
```

Для HTTPS GitHub использует токен или менеджер учётных данных; обычный
пароль аккаунта не подходит. Если у вас настроен SSH, можно использовать
SSH-адрес репозитория вместо HTTPS.

3. В репозитории откройте **Settings → Pages → Build and deployment**
   и выберите **Source: GitHub Actions**.
4. Перейдите в **Actions → Publish Quarto site → Run workflow**,
   выберите ветку `main` и запустите сборку. Если первый автоматический
   запуск после `git push` завершился ошибкой, потому что Pages ещё
   не был включён, повторите его после шага 3.
5. Дождитесь успешного завершения заданий `build` и `deploy`.
   Точный адрес сайта будет в **Settings → Pages** и в результате
   задания `deploy`.

Для репозитория `my-site` адрес обычно такой:
`https://USERNAME.github.io/my-site/`.
Для репозитория с именем `USERNAME.github.io`:
`https://USERNAME.github.io/`.

В этом проекте используется публикация через GitHub Actions. Настраивать
ветку `gh-pages`, каталог `/docs` или выполнять `quarto publish` не нужно.
Токены и секреты в файлы проекта добавлять не требуется.

После публикации можно вписать полный адрес в `website.site-url` в
`_quarto.yml` — пример строки уже есть в комментарии.

## 4. Как добавлять материалы

Создайте `posts/my-note/index.qmd`:

```markdown
---
title: "Моя заметка"
description: "Короткое описание."
date: 2026-10-12
categories: [Физика]
---

## Введение

Формула внутри строки: $E_k=mv^2/2$.

$$
\int_0^{2\pi}\cos^4x\,dx=\frac{3\pi}{4}.
$$ {#eq-integral}

Результат записан в @eq-integral.
```

Поставьте свою дату. Заметка автоматически попадёт в `notes.qmd` при сборке.
Рисунки храните рядом с заметкой и вставляйте как `![Подпись](figure.png)`.
Ссылки на другие страницы делайте относительными, с расширением `.qmd`.

Формулы в браузере отображает MathJax. В этой конфигурации MathJax
загружается с CDN, поэтому для отображения формул нужен доступ в интернет.
Поддержка математической записи LaTeX не означает, что произвольные
пакеты и вёрстка полного `.tex`-документа будут работать в HTML.

Больше примеров — на странице `writing.qmd`. Это служебная памятка автора:
её можно оставить на сайте или удалить вместе с пунктом меню
из `_quarto.yml`.

## 5. Обновление сайта

```bash
quarto preview
```

Проверьте изменения в браузере, остановите предпросмотр и отправьте исходники:

```bash
git add .
git commit -m "Add new note"
git push
```

Workflow соберёт сайт автоматически. Готовый `_site` в Git не добавляется.

## Что находится в проекте

| Файл | Назначение |
| --- | --- |
| `_quarto.yml` | Название, навигация, темы, поиск и настройки формул |
| `index.qmd` | Главная страница |
| `notes.qmd` | Автоматический список заметок |
| `writing.qmd` | Памятка по Markdown и LaTeX |
| `posts/_metadata.yml` | Общие настройки заметок |
| `posts/oscillator/index.qmd` | Пример статьи с формулами |
| `styles.css` | Дополнительные стили поверх готовых тем |
| `.github/workflows/publish.yml` | Автоматическая сборка и публикация |
| `.gitignore` | Исключение генерируемых файлов из Git |

## Источники

Настройки сверены с официальной документацией 12 октября 2026 года:

- Установка: https://quarto.org/docs/get-started/
- Выпуски и контрольные суммы: https://quarto.org/docs/download/release
- Формулы в HTML: https://quarto.org/docs/output-formats/html-basics.html
- Темы: https://quarto.org/docs/output-formats/html-themes.html
- Публикация: https://quarto.org/docs/publishing/github-pages.html
- Настройка Pages workflow: https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages

