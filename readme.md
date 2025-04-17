# 🔍 openapi-ci-checks

Reusable GitHub Actions workflow для автоматической проверки OpenAPI-файлов (`openapi.yaml`) с помощью:

- ✅ [Spectral](https://github.com/stoplightio/spectral) — линтинг по правилам
- ✅ [Swagger CLI](https://github.com/APIDevTools/swagger-cli) — валидация спецификации

Используется в проектах, где OpenAPI-контракт лежит в отдельном репозитории (например, управляется аналитиками).

---

## 📦 Состав

- `.spectral.yaml` — базовые правила Spectral (можно дорабатывать)
- `package.json` — зависимости (`@stoplight/spectral-cli`, `swagger-cli`)
- `.github/workflows/lint-openapi.yml` — reusable workflow

---

## 🚀 Как использовать в другом репозитории

1. Убедитесь, что в вашем репозитории есть `openapi.yaml` (в корне или другой папке)
2. Создайте файл: `.github/workflows/openapi-check.yml`

### Пример:

```yaml
name: OpenAPI Check

on:
   push:
      paths:
         - 'openapi.yaml'
   pull_request:

jobs:
   openapi-check:
      uses: eugenezorkin/openapi-ci-checks/.github/workflows/lint-openapi.yml@main
      with:
         spec_path: openapi.yaml
```

📌 Путь в `spec_path` — относительный от корня вызывающего репозитория.

---

## ⚙️ Что происходит при запуске

1. Чекаутится ваш репозиторий (с контрактом)
2. Чекаутится этот репозиторий (`openapi-ci-checks`) в поддиректорию `checks`
3. Устанавливаются зависимости
4. Запускается:
   - `spectral lint` с кастомными правилами
   - `swagger-cli validate` — строгая проверка OpenAPI-структуры

---

## 🛡 Рекомендуется

- Защитить `main` через branch protection
- Сделать прохождение этого workflow обязательным перед merge
- Хранить OpenAPI отдельно от кода (внутренний контракт-репозиторий)

