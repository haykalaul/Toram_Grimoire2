---
description: Technical Specification for Intlayer Integration
---

# Intlayer Integration Technical Specification

## Executive Summary

This document outlines the technical specification for integrating **Intlayer v5.8.1** into the Toram Grimoire Vue 3 application. The integration will allow for a hybrid approach, leveraging both the existing `vue-i18n` infrastructure and Intlayer's content declaration capabilities.

---

## Table of Contents

1. [Current State Analysis](#current-state-analysis)
2. [Integration Strategy](#integration-strategy)
3. [Implementation Plan](#implementation-plan)
4. [Migration Roadmap](#migration-roadmap)
5. [Best Practices](#best-practices)
6. [Troubleshooting](#troubleshooting)

---

## Current State Analysis

### Existing i18n Setup

**Technology Stack:**

- **Framework:** Vue 3 with Composition API
- **i18n Library:** `vue-i18n` v9.13.1
- **Build Tool:** Vite v6.3.5
- **Node Version:** v18.19.1 (requires Intlayer v5.x for compatibility)

**Supported Locales:**

- `en` (English)
- `ja` (Japanese)
- `zh-CN` (Simplified Chinese)
- `zh-TW` (Traditional Chinese - Default)

**Current Architecture:**

```
src/
├── locales/
│   ├── en/
│   │   ├── app.yaml
│   │   ├── common.yaml
│   │   ├── global.yaml
│   │   └── [view-specific].yaml
│   ├── ja/
│   ├── zh-CN/
│   └── zh-TW/
├── stores/app/language/
│   ├── index.ts (Language store)
│   ├── I18nStore.ts
│   └── enums.ts (Namespace definitions)
└── app/
    └── initI18n.ts (i18n initialization)
```

**Namespace Organization:**

1. **Global Namespaces** (always loaded):

   - `app`: Application-wide UI strings
   - `common`: Shared components
   - `global`: Global utilities

2. **View Namespaces** (lazy-loaded per route):
   - `skill-query`
   - `damage-calculation`
   - `skill-simulator`
   - `character-simulator`
   - `enchant-simulator`
   - `enchant-doll`
   - `crystal-query`
   - `item-query`
   - `registlet-query`
   - `main-quest-calc`
   - `book-template`

**Key Features:**

- Dynamic locale loading with retry logic
- Primary/fallback locale support
- Auto-detection based on browser language
- LocalStorage persistence
- Lazy-loading of view-specific translations

---

## Integration Strategy

### Phase 1: Hybrid Coexistence (Current)

**Status:** ✅ **COMPLETED**

**Configuration:**

```typescript
// vite.config.ts
import { intlayerPlugin } from 'vite-intlayer'

export default defineConfig({
  plugins: [
    intlayerPlugin(),
    // ... other plugins
  ],
})

// intlayer.config.ts
const config: IntlayerConfig = {
  internationalization: {
    locales: ['en', 'ja', 'zh-CN', 'zh-TW'],
    defaultLocale: 'en',
  },
  plugins: [
    // syncJSON plugin disabled for now (requires newer Node.js)
  ],
}
```

**Current Limitations:**

- Cannot use `@intlayer/sync-json-plugin` due to Node.js v18 compatibility
- Manual sync between Intlayer dictionaries and vue-i18n JSON files required

### Phase 2: Content Declaration Migration (Recommended Next Step)

Create Intlayer content declaration files alongside existing YAML files:

**File Structure:**

```
src/
├── content/           # NEW: Intlayer content declarations
│   ├── global/
│   │   ├── app.content.ts
│   │   ├── common.content.ts
│   │   └── global.content.ts
│   └── views/
│       ├── skill-query.content.ts
│       ├── enchant-simulator.content.ts
│       └── [other-views].content.ts
└── locales/           # EXISTING: Keep for backward compatibility
    └── [existing structure]
```

### Phase 3: Full Intlayer Adoption (Future)

**Prerequisites:**

- Upgrade Node.js to v20+ (LTS) or v22
- Enable `@intlayer/sync-json-plugin`
- Complete content migration

---

## Implementation Plan

### Step 1: Create Content Declaration Files

**Example: Global App Strings**

Create `src/content/global/app.content.ts`:

```typescript
import { type DeclarationContent, t } from 'intlayer'

const appContent = {
  key: 'app',
  content: {
    title: t({
      'en': 'Toram Grimoire',
      'ja': 'トーラムグリモワール',
      'zh-CN': 'Toram魔导书',
      'zh-TW': 'Toram魔導書',
    }),

    navigation: {
      home: t({
        'en': 'Home',
        'ja': 'ホーム',
        'zh-CN': '主页',
        'zh-TW': '主頁',
      }),
      tools: t({
        'en': 'Tools',
        'ja': 'ツール',
        'zh-CN': '工具',
        'zh-TW': '工具',
      }),
      // ... more navigation items
    },

    language: {
      primary: t({
        'en': 'Primary Language',
        'ja': 'プライマリ言語',
        'zh-CN': '主要语言',
        'zh-TW': '主要語言',
      }),
      fallback: t({
        'en': 'Fallback Language',
        'ja': 'フォールバック言語',
        'zh-CN': '备用语言',
        'zh-TW': '備用語言',
      }),
    },
  },
} satisfies DeclarationContent

export default appContent
```

**Example: Component-Specific Content**

Create `src/content/views/skill-query.content.ts`:

```typescript
import { type DeclarationContent, t } from 'intlayer'

const skillQueryContent = {
  key: 'skill-query',
  content: {
    pageTitle: t({
      'en': 'Skill Database',
      'ja': 'スキルデータベース',
      'zh-CN': '技能数据库',
      'zh-TW': '技能資料庫',
    }),

    filters: {
      skillType: t({
        'en': 'Skill Type',
        'ja': 'スキルタイプ',
        'zh-CN': '技能类型',
        'zh-TW': '技能類型',
      }),
      search: t({
        'en': 'Search skills...',
        'ja': 'スキルを検索...',
        'zh-CN': '搜索技能...',
        'zh-TW': '搜尋技能...',
      }),
    },

    // Nested objects for organization
    skillDetails: {
      level: t({
        'en': 'Level',
        'ja': 'レベル',
        'zh-CN': '等级',
        'zh-TW': '等級',
      }),
      mpCost: t({
        'en': 'MP Cost',
        'ja': 'MP消費',
        'zh-CN': 'MP消耗',
        'zh-TW': 'MP消耗',
      }),
    },
  },
} satisfies DeclarationContent

export default skillQueryContent
```

### Step 2: Build Intlayer Dictionaries

After creating content files, build the Intlayer dictionaries:

```bash
npm run intlayer:build
```

This generates:

- `.intlayer/dictionary/[locale]/[key].json`
- Type definitions for TypeScript

### Step 3: Use Intlayer in Components

**Option A: Using Intlayer Hooks (Recommended for New Components)**

```vue
<script setup lang="ts">
import { useIntlayer } from 'vue-intlayer'

const { pageTitle, filters, skillDetails } = useIntlayer('skill-query')
</script>

<template>
  <div class="skill-query-page">
    <h1>{{ pageTitle }}</h1>

    <div class="filters">
      <label>{{ filters.skillType }}</label>
      <input :placeholder="filters.search" />
    </div>

    <div class="skill-info">
      <span>{{ skillDetails.level }}: {{ skillLevel }}</span>
      <span>{{ skillDetails.mpCost }}: {{ mpCost }}</span>
    </div>
  </div>
</template>
```

**Option B: Continue Using vue-i18n (For Existing Components)**

Keep the current `vue-i18n` approach while Intlayer generates the JSON files:

```vue
<script setup lang="ts">
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
</script>

<template>
  <div class="skill-query-page">
    <h1>{{ t('skill-query.pageTitle') }}</h1>

    <div class="filters">
      <label>{{ t('skill-query.filters.skillType') }}</label>
      <input :placeholder="t('skill-query.filters.search')" />
    </div>
  </div>
</template>
```

### Step 4: Update Language Store (Hybrid Approach)

Modify `src/stores/app/language/index.ts` to support both sources:

```typescript
const loadLocaleMessages: LoadLocaleMessages = async namespaces => {
  const namespaceList = typeof namespaces === 'string' ? [namespaces] : namespaces
  if (!i18n.value) {
    console.warn('[Init language data] instance is no found')
    return
  }

  let unknownError = false

  const loadData = async (locale: string) => {
    const data = {} as Record<string, object>
    const promises = namespaceList.map(async namespace => {
      let count = 0
      let resultData!: object
      const retry = async () => {
        try {
          // Priority order:
          // 1. Intlayer-generated JSON (from .intlayer/dictionary)
          // 2. Manual JSON in locales folder
          // 3. Legacy YAML files
          try {
            const intlayerModule = await import(
              `../../../.intlayer/dictionary/${locale}/${namespace}.json`
            )
            resultData = intlayerModule.default ?? {}
          } catch {
            try {
              const dataModule = await import(`../../../locales/${locale}/${namespace}.json`)
              resultData = dataModule.default ?? {}
            } catch {
              const dataModule = await import(`../../../locales/${locale}/${namespace}.yaml`)
              resultData = dataModule.default ?? {}
            }
          }
        } catch {
          count += 1
        }
      }
      while (count < 3 && !resultData) {
        await retry()
      }
      if (!resultData) {
        unknownError = true
      }
      data[namespace] = resultData ?? {}
    })
    await Promise.all(promises)

    return data
  }

  // ... rest of the function remains the same
}
```

### Step 5: TypeScript Configuration

Ensure TypeScript recognizes Intlayer types:

```json
// tsconfig.json
{
  "compilerOptions": {
    "types": ["intlayer"]
  },
  "include": ["src/**/*", ".intlayer/**/*"]
}
```

---

## Migration Roadmap

### Immediate Actions (Week 1-2)

1. ✅ **Install Intlayer packages** (v5.8.1 for Node.js v18 compatibility)
2. ✅ **Configure Vite plugin**
3. ✅ **Set up intlayer.config.ts**
4. **Create pilot content declaration** for one global namespace (e.g., `common`)
5. **Test build and integration**

### Short-term (Month 1)

1. **Migrate all global namespaces** to Intlayer content declarations

   - `app.content.ts`
   - `common.content.ts`
   - `global.content.ts`

2. **Create automated sync script** (manual workaround until Node.js upgrade)

   ```bash
   npm run intlayer:build && npm run copy:intlayer-to-locales
   ```

3. **Document content declaration patterns** for team

### Medium-term (Month 2-3)

1. **Migrate 3-5 high-traffic view namespaces**

   - Start with `skill-query`, `damage-calculation`
   - Keep dual support (Intlayer + vue-i18n)

2. **Create component library** using `useIntlayer` hook
3. **Establish PR review checklist** for i18n changes

### Long-term (Month 4+)

1. **Plan Node.js upgrade** to v20 LTS
2. **Enable `@intlayer/sync-json-plugin`** after upgrade
3. **Complete migration** of all view namespaces
4. **Deprecate YAML files** (keep as archive)
5. **Update documentation** and onboarding materials

---

## Best Practices

### 1. Content Organization

**Group by Feature, Not by Locale:**

```
✅ GOOD:
src/content/
├── global/
│   ├── app.content.ts
│   └── common.content.ts
└── features/
    ├── skills/
    │   ├── query.content.ts
    │   └── simulator.content.ts
    └── enchant/
        ├── simulator.content.ts
        └── doll.content.ts

❌ BAD:
src/content/
├── en/
├── ja/
├── zh-CN/
└── zh-TW/
```

### 2. Naming Conventions

**Content Keys:**

- Use kebab-case for content file keys: `skill-query`, `damage-calculation`
- Match existing namespace enum values for compatibility

**Content Properties:**

- Use camelCase for nested properties: `pageTitle`, `skillType`
- Keep hierarchy shallow (max 3 levels deep)

### 3. Type Safety

**Always use `satisfies DeclarationContent`:**

```typescript
const content = {
  key: 'my-content',
  content: {
    // ...
  },
} satisfies DeclarationContent // ← Type-safe!

export default content
```

### 4. Dynamic Content

**For interpolated strings, use functions:**

```typescript
import { t } from 'intlayer'

const content = {
  key: 'messages',
  content: {
    // Static
    welcome: t({
      en: 'Welcome',
      ja: 'ようこそ',
    }),

    // Dynamic with parameters
    greeting: t({
      'en': (name: string) => `Hello, ${name}!`,
      'ja': (name: string) => `こんにちは、${name}さん！`,
      'zh-CN': (name: string) => `你好，${name}！`,
      'zh-TW': (name: string) => `你好，${name}！`,
    }),

    // Pluralization
    itemCount: t({
      en: (count: number) => (count === 1 ? '1 item' : `${count} items`),
      ja: (count: number) => `${count}個のアイテム`,
    }),
  },
} satisfies DeclarationContent
```

**Usage in component:**

```vue
<script setup lang="ts">
import { useIntlayer } from 'vue-intlayer'

const { greeting, itemCount } = useIntlayer('messages')
const userName = ref('Player')
const items = ref(5)
</script>

<template>
  <div>
    <p>{{ greeting(userName) }}</p>
    <p>{{ itemCount(items) }}</p>
  </div>
</template>
```

### 5. Avoid Duplication

**Extract common patterns:**

```typescript
// src/content/common/validation.content.ts
import { t } from 'intlayer'

export const validationMessages = {
  required: t({
    en: 'This field is required',
    ja: 'この項目は必須です',
    'zh-CN': '此字段为必填项',
    'zh-TW': '此欄位為必填項',
  }),

  invalidEmail: t({
    en: 'Invalid email address',
    ja: '無効なメールアドレス',
    'zh-CN': '无效的电子邮件地址',
    'zh-TW': '無效的電子郵件地址',
  }),
}

// Use in other content files
import { validationMessages } from './validation.content'

const formContent = {
  key: 'contact-form',
  content: {
    validation: validationMessages,
    // ...
  },
} satisfies DeclarationContent
```

---

## Troubleshooting

### Issue: "SyntaxError: Unexpected token 'with'"

**Cause:** Using Intlayer v7.x with Node.js v18

**Solution:**

```bash
npm install intlayer@5.8.1 vite-intlayer@5.8.1
```

Or upgrade Node.js to v20+ LTS.

---

### Issue: "does not provide an export named 'intlayer'"

**Cause:** API changed between versions

**Solution in vite.config.ts:**

```typescript
// v5.x API
import { intlayerPlugin } from 'vite-intlayer'

export default defineConfig({
  plugins: [
    intlayerPlugin(), // ← Use intlayerPlugin, not intlayer()
  ],
})
```

---

### Issue: Build succeeds but translations don't load

**Checklist:**

1. Run `npm run intlayer:build` after creating content files
2. Verify `.intlayer/dictionary/` contains generated JSON
3. Check import paths in language store
4. Ensure content key matches namespace enum value
5. Clear Vite cache: `rm -rf node_modules/.vite`

---

### Issue: TypeScript errors on `useIntlayer`

**Solution:**

Add to `tsconfig.json`:

```json
{
  "compilerOptions": {
    "types": ["intlayer", "vite/client"]
  }
}
```

Restart TypeScript server in IDE.

---

### Issue: Hot reload not working for content changes

**Workaround for v5.x:**

1. Save content file
2. Run `npm run intlayer:build`
3. Vite will auto-reload

**Better solution:** Upgrade to v7.x after Node.js upgrade for automatic HMR.

---

## Performance Considerations

### Bundle Size Impact

**Current Setup (YAML):**

- ~14 YAML files × 4 locales = 56 files
- Estimated: 150-200 KB uncompressed

**With Intlayer:**

- Generated JSON is tree-shakeable
- Only load needed namespaces
- Expected improvement: 10-20% smaller bundles

### Loading Strategy

**Keep existing lazy-loading:**

- Global namespaces: Loaded on app init
- View namespaces: Loaded on route navigation

**Intlayer benefits:**

- Type-safe imports reduce runtime errors
- Better IDE autocomplete = fewer typos
- Centralized content = easier auditing

---

## Team Workflow

### For Developers

1. **Adding new translations:**

   ```bash
   # 1. Create or edit content file
   src/content/views/new-feature.content.ts

   # 2. Build dictionaries
   npm run intlayer:build

   # 3. Test in dev server
   npm run dev
   ```

2. **Code review checklist:**
   - [ ] All 4 locales provided (`en`, `ja`, `zh-CN`, `zh-TW`)
   - [ ] Content key matches namespace enum
   - [ ] TypeScript types are satisfied
   - [ ] Build succeeds without errors

### For Translators

**Current workflow (v5.x):**

1. Edit `.ts` content files directly
2. OR: Edit JSON in `.intlayer/dictionary/` (auto-generated)
3. Notify dev team to rebuild

**Future workflow (v7.x with sync plugin):**

1. Edit JSON in `src/locales/` (familiar structure)
2. Plugin auto-syncs to Intlayer content
3. No dev intervention needed

---

## References

- [Intlayer Documentation](https://intlayer.org/doc)
- [Intlayer + Vue Integration](https://intlayer.org/doc/concept/intlayer-vite-plugin)
- [Migration Guide](https://intlayer.org/doc/guides/migrate-from-json)
- [API Reference](https://intlayer.org/doc/api/packages/intlayer/getTranslationContent)

---

## Appendix: Quick Win Scripts

### Copy Intlayer Output to Locales (Temporary)

Until Node.js upgrade, use this script to sync:

```json
// package.json
{
  "scripts": {
    "intlayer:build": "npx intlayer build",
    "intlayer:sync": "node scripts/sync-intlayer.js",
    "intlayer:all": "npm run intlayer:build && npm run intlayer:sync"
  }
}
```

```javascript
// scripts/sync-intlayer.js
const fs = require('fs')
const path = require('path')

const locales = ['en', 'ja', 'zh-CN', 'zh-TW']
const intlayerDir = '.intlayer/dictionary'
const targetDir = 'src/locales'

locales.forEach(locale => {
  const sourceDir = path.join(intlayerDir, locale)
  const destDir = path.join(targetDir, locale)

  if (!fs.existsSync(sourceDir)) {
    console.warn(`Skipping ${locale}: source not found`)
    return
  }

  fs.readdirSync(sourceDir)
    .filter(file => file.endsWith('.json'))
    .forEach(file => {
      const sourcePath = path.join(sourceDir, file)
      const destPath = path.join(destDir, file)

      fs.copyFileSync(sourcePath, destPath)
      console.log(`✓ Synced ${locale}/${file}`)
    })
})

console.log('✓ Intlayer sync complete')
```

---

**Document Version:** 1.0  
**Last Updated:** 2026-01-11  
**Maintainer:** Development Team  
**Status:** ACTIVE
