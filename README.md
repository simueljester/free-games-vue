# Vue.js Functionalities Applied in This Project

## 📋 Overview
This document lists all Vue.js features, APIs, and patterns used in this codebase for interview preparation.

---

## 🎯 **Core Vue.js Features**

### 1. **Composition API with `<script setup>`**
- **Location**: All components (`Index.vue`, `GameCard.vue`, `LoadingOverlay.vue`, `HelloWorld.vue`, `App.vue`)
- **What it is**: Modern Vue 3 syntax that combines setup function and `<script setup>` compiler macro
- **Benefits**: 
  - Less boilerplate code
  - Better TypeScript support
  - Top-level bindings are automatically exposed to template
- **Example**: 
  ```vue
  <script setup lang="ts">
    const loading = ref(true)
    // automatically available in template
  </script>
  ```

### 2. **Reactive References (`ref`)**
- **Location**: `Index.vue` (lines 11-12), `HelloWorld.vue` (line 6)
- **Purpose**: Create reactive state that triggers re-renders when changed
- **Usage**: 
  ```ts
  const loading = ref(true)
  const games = ref<Game[]>([])
  const count = ref(0)
  ```
- **How it works**: `ref()` wraps a value and provides reactive access via `.value` property

### 3. **Props Definition (`defineProps`)**
- **Location**: All components using props
- **TypeScript syntax**: `defineProps<{ propName: Type }>()`
- **Examples**:
  - `Index.vue`: `defineProps<{ msg: string }>()`
  - `GameCard.vue`: `defineProps<{ game: Game }>()`
  - `LoadingOverlay.vue`: `defineProps<{ show: boolean }>()`
- **Note**: `defineProps` is a compile-time macro, no need to import

---

## 🎨 **Template Directives**

### 4. **Text Interpolation (`{{ }}`)**
- **Location**: Multiple components
- **Examples**:
  - `{{ msg }}` - Display prop value
  - `{{ game.title }}` - Display object properties
  - `{{ count }}` - Display reactive state

### 5. **Property Binding (`v-bind` or `:`)**
- **Location**: `Index.vue`, `GameCard.vue`
- **Examples**:
  - `:show="loading"` - Bind prop to reactive value
  - `:game="game"` - Pass object as prop
  - `:src="game.thumbnail"` - Bind image source
  - `:alt="game.title"` - Bind attribute
  - `:href="game.game_url"` - Bind link URL
  - `:key="game.id"` - Bind unique key for list items

### 6. **Conditional Rendering (`v-if`, `v-else`)**
- **Location**: `Index.vue` (lines 35, 37)
- **Usage**:
  ```vue
  <div v-if="loading">Loading games...</div>
  <div v-else class="games-container">...</div>
  ```
- **Difference**: `v-if` removes/adds elements from DOM, `v-show` only toggles visibility

### 7. **List Rendering (`v-for`)**
- **Location**: `Index.vue` (line 38)
- **Usage**:
  ```vue
  <GameCard v-for="game in games" :key="game.id" :game="game" />
  ```
- **Best practice**: Always use `:key` with `v-for` for efficient updates
- **Type**: Iterating over reactive array (`games.value`)

### 8. **Event Handling (`@click` or `v-on:click`)**
- **Location**: `HelloWorld.vue` (line 13)
- **Usage**: `@click="count++"` - Increment reactive value on click

---

## 🔄 **Lifecycle Hooks**

### 9. **`onMounted` Lifecycle Hook**
- **Location**: `Index.vue` (line 26-28)
- **Purpose**: Execute code after component is mounted to DOM
- **Usage**:
  ```ts
  import { onMounted } from 'vue'
  
  onMounted(() => {
    fetchGames()
  })
  ```
- **When to use**: API calls, DOM manipulation, subscriptions that need DOM to exist

---

## 🧩 **Component System**

### 10. **Component Registration & Usage**
- **Location**: All parent components
- **Pattern**: 
  1. Import component
  2. Use in template
- **Example** (`Index.vue`):
  ```ts
  import LoadingOverlay from './LoadingOverlay.vue';
  import GameCard from './GameCard.vue';
  ```
  ```vue
  <LoadingOverlay :show="loading" />
  <GameCard v-for="game in games" :key="game.id" :game="game" />
  ```

### 11. **Component Props Communication**
- **Pattern**: Parent passes data to child via props
- **Examples**:
  - `App.vue` → `Index.vue`: passes `msg` prop
  - `Index.vue` → `GameCard.vue`: passes `game` object
  - `Index.vue` → `LoadingOverlay.vue`: passes `show` boolean

---

## ✨ **Transitions & Animations**

### 12. **`<transition>` Component**
- **Location**: `LoadingOverlay.vue` (line 6)
- **Purpose**: Apply enter/leave transitions to elements
- **Usage**:
  ```vue
  <transition name="fade">
    <div v-if="show" class="overlay">...</div>
  </transition>
  ```
- **CSS Classes**: 
  - `fade-enter-active`, `fade-leave-active` - Transition duration
  - `fade-enter-from`, `fade-leave-to` - Initial/final states

---

## 🎨 **Styling**

### 13. **Scoped Styles**
- **Location**: All components with `<style scoped>`
- **Purpose**: Styles only apply to current component, preventing style conflicts
- **Usage**: `<style scoped>` in every component

---

## 🚀 **Application Setup**

### 14. **`createApp()` - Application Instance**
- **Location**: `main.ts` (line 5)
- **Purpose**: Create Vue application instance
- **Usage**:
  ```ts
  import { createApp } from 'vue'
  createApp(App).mount('#app')
  ```
- **Modern approach**: Replaces Vue 2's `new Vue()`

---

## 📝 **TypeScript Integration**

### 15. **TypeScript with Vue**
- **Language**: `<script setup lang="ts">`
- **Features used**:
  - Type annotations: `ref<Game[]>([])`
  - Interface definitions: `interface Game { ... }`
  - Type-safe props: `defineProps<{ game: Game }>()`
  - Type imports: `import type { Game } from '../types/game'`

### 16. **Shared Types/Interfaces**
- **Location**: `src/types/game.ts`
- **Pattern**: Export interfaces from shared files, import with `import type`
- **Benefits**: 
  - Single source of truth
  - Type safety across components
  - Better code organization

---

## 🔧 **Advanced Patterns**

### 17. **Async/Await with Reactive State**
- **Location**: `Index.vue` (lines 14-24)
- **Pattern**: Combining async functions with reactive refs
- **Example**:
  ```ts
  const fetchGames = async () => {
    loading.value = true
    try {
      const response = await axios.get('/api/games')
      games.value = response.data
    } catch (error) {
      console.error('Error fetching games:', error)
    } finally {
      loading.value = false
    }
  }
  ```

### 18. **Reactive State Updates**
- **Pattern**: Updating ref values triggers reactivity
- **Access pattern**: 
  - In script: `ref.value = newValue`
  - In template: `{{ ref }}` (automatic unwrapping)

### 19. **Computed-like Patterns**
- **Note**: While not explicitly using `computed()`, the reactive system automatically updates views when refs change

---

## 📊 **Summary by Component**

| Component | Features Used |
|-----------|--------------|
| **Index.vue** | `ref`, `onMounted`, `defineProps`, `v-if`/`v-else`, `v-for`, `:key`, prop binding, component imports |
| **GameCard.vue** | `defineProps`, property binding (`:src`, `:alt`, `:href`), text interpolation |
| **LoadingOverlay.vue** | `defineProps`, `<transition>`, `v-if`, transition CSS classes |
| **App.vue** | Component import, prop passing, `<script setup>` |
| **HelloWorld.vue** | `ref`, `defineProps`, `@click`, text interpolation |
| **main.ts** | `createApp`, app mounting |

---

## 🎯 **Interview Talking Points**

### Key Concepts You Can Discuss:

1. **Composition API vs Options API**
   - This project uses Composition API exclusively
   - Benefits: Better TypeScript support, code organization, reusability

2. **Reactivity System**
   - How `ref()` works and when to use it
   - Reactive updates propagate automatically

3. **Component Communication**
   - Props down pattern (parent → child)
   - Type-safe props with TypeScript

4. **Lifecycle Management**
   - Using `onMounted` for API calls
   - Understanding component lifecycle

5. **Template Directives**
   - `v-if` vs `v-show` (when to use which)
   - `v-for` with `:key` for list rendering
   - Event binding and property binding

6. **TypeScript Integration**
   - Type safety in Vue components
   - Shared type definitions
   - `import type` for type-only imports

---

## 📚 **Vue.js Version**

- **Vue 3** (Composition API, `<script setup>`, `createApp`)

---

## 💡 **Best Practices Demonstrated**

✅ Single File Components (SFC)  
✅ Component composition  
✅ TypeScript for type safety  
✅ Scoped styles  
✅ Reusable components  
✅ Proper key binding in lists  
✅ Error handling in async operations  
✅ Loading states management  

