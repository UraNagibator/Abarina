Автор: Абарина Домимника Игоревна
Дата создания: 18.05.2026

```markdown
# test-repo – Git rebase и разрешение конфликтов

Демонстрационный репозиторий для отработки навыков работы с Git: клонирование, создание ветки, ребейз с разрешением конфликтов и отправка изменений через Visual Studio.

## Задача

1. Клонировать удалённый репозиторий
2. Создать ветку `test` и добавить файл `Test.cs`
3. Выполнить `git pull --rebase` для синхронизации с `main`
4. Разрешить конфликты (если возникнут)
5. Отправить изменения в удалённый репозиторий через Visual Studio

## Исходные данные

- **Удалённый репозиторий:** `https://github.com/user/test-repo.git`
- **Ветка для работы:** `test`
- **Файл:** `Test.cs`
- **Содержимое файла:** `Console.WriteLine("Test case");`

## Пошаговое выполнение

### 1. Клонирование репозитория

```bash
git clone https://github.com/user/test-repo.git
cd test-repo
```

**В Visual Studio:**  
`Файл → Клонировать репозиторий` → указать URL и локальный путь

---

### 2. Создание ветки `test` и добавление файла `Test.cs`

```bash
git checkout -b test
echo 'Console.WriteLine("Test case");' > Test.cs
git add Test.cs
git commit -m "feat: add Test.cs with test output"
```

**В Visual Studio:**  
- `Git Changes` → выпадающий список веток → `New Branch` → ввести `test`
- Добавить файл через Solution Explorer
- Закоммитить через окно Git Changes

---

### 3. Синхронизация с `main` через rebase

**Способ 1 (с переключением на `main`):**
```bash
git checkout main
git pull origin main
git checkout test
git pull --rebase origin main
```

**Способ 2 (без переключения, одной командой):**
```bash
git fetch origin
git rebase origin/main
```

Если конфликтов нет — ребейз завершится автоматически.

---

### 4. Разрешение конфликтов (если возникли)

При появлении сообщения:
```
CONFLICT (content): Merge conflict in Test.cs
```

**Алгоритм действий:**

1. Открыть файл `Test.cs` — в нём будут маркеры конфликта:
   ```
   <<<<<<< HEAD
   Console.WriteLine("Test case");
   =======
   (код из main)
   >>>>>>> some-commit-hash
   ```

2. Отредактировать файл, убрав маркеры и оставив нужный код:
   ```csharp
   Console.WriteLine("Test case");
   ```

3. Сохранить файл и добавить его в индекс:
   ```bash
   git add Test.cs
   ```

4. Продолжить ребейз:
   ```bash
   git rebase --continue
   ```

**При ошибке отменить ребейз:**
```bash
git rebase --abort
```

---

### 5. Отправка изменений через Visual Studio

**Через Team Explorer:**
1. `Вид → Team Explorer` (или вкладка рядом с Solution Explorer)
2. Выбрать раздел **Sync** (Синхронизация)
3. Нажать **Push** (если ветка `test` ещё не на сервере — подтвердить публикацию)

**Через Git Changes:**
1. Открыть окно `Git Changes`
2. Нажать кнопку **Push** (стрелка вверх)

**Альтернатива через терминал (для проверки):**
```bash
git push --set-upstream origin test
```

---

### 6. Проверка результата

- Команда `git log --oneline --graph` показывает линейную историю: коммит ветки `test` расположен поверх `origin/main`
- В удалённом репозитории на GitHub в ветке `test` присутствует файл `Test.cs`

---

## Граф коммитов (после успешного rebase)

```
* (test) feat: add Test.cs with test output
* (main) Последний коммит из main
* Предыдущие коммиты...
```

---

## Итоговый файл `Test.cs`

```csharp
Console.WriteLine("Test case");
```


## Примечания

- **Конфликт может не возникнуть**, если в `main` не было изменений, затрагивающих `Test.cs`. Алгоритм разрешения описан для общего случая.
- Решение универсально и работает как в Git Bash, так и в терминале Visual Studio.
- Для работы с репозиторием требуется установленный Git и Visual Studio 2019/2022 с компонентами Git.
```
