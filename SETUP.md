# Первичная настройка репозитория

## 1. Создайте пустой репозиторий

Откройте https://github.com/new. Имя, например, matan-notes. Видимость Public.
Не добавляйте README, .gitignore или лицензию на этом экране: исходные файлы уже в наборе.
Для общего проекта позже можно перенести репозиторий в GitHub Organization.

## 2. Подготовьте файлы

Распакуйте архив. В .github/CODEOWNERS замените @YOUR_GITHUB_LOGIN своим логином,
а @MODERATOR_LOGIN - логином второго модератора (или пока удалите второго).
В README.md замените YOUR_GITHUB_LOGIN своим логином, а matan-notes - именем вашей репы,
если выбрали другое. Это адреса кнопок открытия и скачивания PDF.
Замените название на титульном листе в main.tex и примеры билетов своими.
Скрытая папка .github обязательно должна попасть в репозиторий.

## 3. Загрузите проект

Из папки matan-notes-starter выполните (замените YOUR_LOGIN и имя репозитория):

```bash
git init -b main
git add .
git commit -m "Add tickets and automatic PDF build"
git remote add origin https://github.com/YOUR_LOGIN/matan-notes.git
git push -u origin main
```

Если Git просит имя/почту, задайте их для этого репозитория через
`git config user.name "Имя"` и `git config user.email "Почта из GitHub"`, затем повторите коммит.
В Windows команды можно выполнить в Git Bash.
Для входа используйте Git Credential Manager с входом через браузер.
Пароль от сайта GitHub не является паролем для git push.
Токены не добавляйте в файлы и не присылайте в чаты.

## 4. Проверьте Actions

Settings → Actions → General. Actions должны быть включены; разрешите используемые
официальные actions/checkout, actions/upload-artifact, actions/download-artifact.
В шаблоне права заданы явно: build имеет contents: read, publish - contents: write.
Для личного репозитория без дополнительных политик менять глобальные права на
Read and write permissions не нужно. Личные токены и Secrets не нужны.
Если политика организации запрещает выдачу contents: write, её должен изменить владелец организации.

Actions → Build and publish tickets. Дождитесь Build PDF и Publish PDF.
Если первый запуск не произошёл, запустите Run workflow для ветки main.
Ветка pdf создаётся автоматически, создавать её вручную не нужно.
Главная ветка проекта остаётся main. GitHub Pages для этой схемы не нужен.

Откройте ссылку на PDF в README. Проверьте титул, оглавление, кириллицу и формулы.
Если сборка упала, откройте красный шаг Compile tickets и найдите первую ошибку.

## 5. Добавьте модераторов

Settings → Collaborators (или Collaborators and teams) → Add people.
Пригласите второго модератора с правами записи (Write).
В личном репозитории роль collaborator даёт права записи; в организации выбирается роль.
Модератор должен принять приглашение. Его логин должен присутствовать в CODEOWNERS.
Обычным участникам права записи не нужны: они работают через fork и PR.

Автор не может одобрить собственный PR. Если правила обязательны и для владельца,
его PR тоже должен проверить другой модератор. С одним модератором собственные PR
при обязательном одобрении и запрете обхода будут заблокированы.

## 6. Защитите main после первой успешной сборки

Settings → Branches → Add classic branch protection rule.
Branch name pattern: main (именно main, не *).

Включите:

- Require a pull request before merging.
- Require approvals: 1.
- Dismiss stale pull request approvals when new commits are pushed.
- Require review from Code Owners.
- Require status checks to pass before merging: выберите Build PDF.
- Require branches to be up to date before merging.
- Require conversation resolution before merging.
- Do not allow bypassing the above settings, когда второй модератор уже подключён.

Force pushes и deletions оставьте запрещёнными. Сохраните правило.
Проверка Publish PDF не должна быть обязательной: в PR она пропускается специально.
Если Build PDF не находится в поиске, сначала выполните сборку/тестовый PR.
Если её нет даже после запуска main, создайте тестовый PR и вернитесь к настройке проверки.
Это доступно бесплатно в публичном репозитории.

Правило обязательных PR не применяйте к ветке pdf: туда пишет автоматика.
Возможность записи по-прежнему есть только у доверенных участников и GitHub Actions.

## 7. Проверьте цикл исправления

Через сайт откройте билет, нажмите Edit и создайте новую ветку, например fix-ticket-01.
Внесите небольшую правку и откройте PR в main. Второй модератор проверяет:
Build PDF зелёный, формулы верны, при необходимости просмотрен артефакт tickets-pdf.
После Approve нажмите Squash and merge (или обычный Merge).
Дождитесь публикации, откройте прежнюю ссылку PDF и убедитесь, что правка появилась.
В ветке pdf файл SOURCE_COMMIT.txt должен указывать на новый коммит main.

Чтобы после одобрения не нажимать Merge вручную, можно отдельно включить
Settings → General → Pull Requests → Allow auto-merge и Enable auto-merge в нужном PR.
Это необязательно для запуска проекта.

## 8. Наполняйте билетами

Для нового билета добавьте только tickets/ticket-XX.tex и укажите в нём
\ticket{XX}{Название}. Он автоматически попадёт в общий PDF в числовом порядке.
Временный generated/tickets.tex генерируется заново; main.tex не нужно менять.
Оглавление и страницы обновляет LaTeX.

README, CONTRIBUTING.md и шаблон Issue уже включены.
Автоматическая сборка не проверяет правильность математических доказательств.

## Справка

- Создание: https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository
- Защита веток: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule
- CODEOWNERS: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
