# 🏗️ .NET Clean Architecture Template

Готовый к использованию шаблон для создания микросервисов на .NET с архитектурой Clean Architecture.

## 📋 Что создаёт шаблон

При использовании команды `dotnet new Capi -n MyProject` создаётся следующая структура:

```bash
MyProject/
├── README.md                 # Инструкции по проекту
├── MyProject.API/            # Слой Web API
├── MyProject.Application/    # Бизнес-логика
├── MyProject.Domain/         # Доменные модели
└── MyProject.Infrastructure/ # Данные и внешние сервисы
```

## 🚀 Быстрый старт

### 1. Добавить источник GitHub Packages

Создайте **[Personal Access Token](https://github.com/settings/tokens/new)** на GitHub с правами  
`read:packages` и выполните:

### Шаблон
```bash
dotnet nuget add source https://nuget.pkg.github.com/<GITHUB_USERNAME>/index.json \
  --name <CUSTOM_NUGET_NAME> \
  --username <YOUR_GITHUB_USERNAME> \
  --password <YOUR_PERSONAL_ACCESS_TOKEN> \
  --store-password-in-clear-text
```

### 2. Установить шаблон

```bash
dotnet new install vidankov.cleanarchitecture.template
```

### 3. Использовать шаблон

```bash
# Создать новый проект
dotnet new Capi -n MyMicroservice

# Перейти в проект и запустить
cd MyMicroservice
dotnet build
dotnet run --project MyMicroservice.API
```

## 🛠️ Управление шаблоном

- Проверить установку:
  ```bash
  dotnet new list
  ```
- Обновить шаблон:
  ```bash
  dotnet new install vidankov.cleanarchitecture.template --force
  ```
- Удалить шаблон:
  ```bash
  dotnet new uninstall vidankov.cleanarchitecture.template
  ```
- Удалить источник:
  ```bash
  dotnet nuget remove source github-vidankov
  ```

## 📚 Дополнительные команды

```bash
# Посмотреть все источники NuGet
dotnet nuget list source
```

## 📦 Публикация новых версий (для автора)

1. Внесите изменения в шаблон:
   - Отредактируйте файлы в `working/content/Capi/`
   - Обновите `PackageVersion` в `Template.csproj`
2. Закоммитьте и создайте тег:

   ```bash
   git add .
   git commit -m "Update template: добавлена новая функциональность"
   git push origin main

   # Создать и запушить тег версии
   git tag v1.0.0
   git push origin v1.0.0
   ```

3. Автоматическая публикация  
   GitHub Actions автоматически опубликует пакет в GitHub Packages при создании тега.

   Проверить публикацию: **Actions** → статус workflow, **Packages** → новая версия.

## 🔧 Локальная разработка шаблона

```bash
# Клонировать репозиторий
git clone https://github.com/vidankov/dotnet-clean-architecture-template.git
cd dotnet-clean-architecture-template

# Установить локально для тестирования
cd working
dotnet new install .

# Тестировать изменения
dotnet new Capi -n TestProject

# Удалить локальную версию
dotnet new uninstall "/полный/путь/к/working"
```

# Подготовка

```bash
▸ dotnet --version
9.0.102
```

## 📋 Создание проекта

```bash
# В папке шаблона (Capi)
dotnet new web -n "Capi.API"   
dotnet new classlib -n "Capi.Domain"  
dotnet new classlib -n "Capi.Application"  
dotnet new classlib -n "Capi.Infrastructure"

# В корне репозитория
dotnet new gitignore   
```

## ⚒️ Как прописать зависимости

```bash
# Из проекта Capi.API
dotnet add reference "../Capi.Application"
dotnet add reference "../Capi.Infrastructure"

# Из проекта Capi.Application
dotnet add reference "../Capi.Domain" 

# Проект Capi.Domain не имеет внешних зависимостей

# Из проекта Capi.Infrastructure
dotnet add reference "../Capi.Application"
```

## 🛠️ Зависимости для функционирования

### Swagger

1. Установка библиотеки:
   ```bash
   dotnet add package "Swashbuckle.AspNetCore" --version "9.0.3"
   ```
2. Регистрация сервисов:
   ```csharp
   builder.Services.AddEndpointsApiExplorer();
   builder.Services.AddSwaggerGen();
   ```
3. Конфигурация приложения:
   ```csharp
   if (app.Environment.IsDevelopment())
   {
       app.UseSwagger();
       app.UseSwaggerUI();
   }
   ```

### Для использования `IServiceCollection`

```bash
dotnet add package "Microsoft.Extensions.DependencyInjection.Abstractions" --version "9.0.7"
```

### Для использования `IConfiguration`

```bash
dotnet add package "Microsoft.Extensions.Configuration" --version "9.0.7"
```