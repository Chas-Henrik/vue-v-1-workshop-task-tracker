# Workshop - Task Tracker med Vue.js

Idag bygger vi en **Task Tracker** (förlåt) med Vue.js där vi använder allt vi lärt oss hittills:
- Reaktiv data med `ref()`
- Metoder för användarinteraktioner
- Computed properties för beräkningar och filtrering
- Watchers för side effects
- v-model för formulär
- Direktiv (v-if, v-for, v-bind, etc.)

---

## Kom igång

### 1. Skapa ett nytt Vue-projekt med Vite
```bash
npm create vite@latest task-tracker -- --template vue
cd task-tracker
npm install
npm run dev
```

**Om ni vill använda TypeScript:** (vi kommer gå igenom detta nästa vecka Vue+TS alltså)
```bash
npm create vite@latest task-tracker -- --template vue-ts
cd task-tracker
npm install
npm run dev
```

### 2. Rensa upp projektet

- Ta bort exempelkod i `src/App.vue`
- Ta bort `src/components/HelloWorld.vue` (eller behåll för referens)

## Uppgift: Bygg en **Task Tracker** med följande funktionalitet


1. **Visa befintliga tasks**
   - En lista med tasks ska visas
   - Varje task ska ha: titel, beskrivning, kategori (Work/Personal/Other), status (completed/active)

2. **Lägga till nya tasks**
   - Input-fält för titel
   - Textarea för beskrivning
   - Select dropdown för kategori
   - Knapp för att lägga till task

3. **Markera tasks som klara**
   - Checkbox för varje task
   - Visuell indikation när task är klar (t.ex. genomstruken text eller färgändring)

4. **Ta bort tasks**
   - Knapp för att ta bort en task

5. **Filtrera tasks**
   - Knappar för att filtrera: "Alla", "Aktiva", "Klara"
   - Visa endast tasks som matchar vald filter

6. **Statistik**
   - Visa antal aktiva tasks
   - Visa antal klara tasks
   - Visa totalt antal tasks

### Tips för hur ni ska bygga:

Använd `ref()` för reaktiv data  
Använd metoder för att lägga till, togglea och ta bort tasks  
Använd computed properties för:
- Filtrering av tasks baserat på vald filter
- Beräkning av antal aktiva tasks
- Beräkning av antal klara tasks  
Använd `v-model` för formulärinputs  
Använd `v-for` för att visa tasks  
Använd `v-if` eller `v-show` för villkorlig rendering  
Använd `:class` för dynamisk styling av klara tasks

### Datastruktur (förslag):
```javascript
const tasks = ref([
  {
    id: 1,
    title: 'Lära mig Vue.js',
    description: 'Gå igenom dokumentationen',
    category: 'Personal',
    completed: false,
    createdAt: new Date()
  }
])
```

## Vidareutveckling om ni blir klara med grunden

När ni är klara med grunduppgiften, välj en eller flera av dessa utmaningar:

### LocalStorage
Spara tasks till localStorage så de finns kvar när användaren laddar om sidan.

- Använd `watch()` med `{ deep: true }` för att spara när tasks ändras
- Ladda tasks från localStorage när komponenten mountas (vi ska prata mer om lifecycle hooks nästa vecka)
- Kom ihåg att hantera fall när localStorage är tom

### Sökfunktion
Lägg till ett sökfält där användaren kan söka efter tasks baserat på titel eller beskrivning.

- Lägg till en `searchQuery` ref
- Uppdatera din `filteredTasks` computed för att även filtrera på söksträngen
- Använd `.toLowerCase()` och `.includes()` för case-insensitive sökning

### Sortering
Lägg till möjlighet att sortera tasks efter:
- Datum (nyast/äldst först)
- Titel (A-Ö / Ö-A)
- Status (aktiva först / klara först)

Tips:
- Använd en `sortBy` ref för att hålla koll på vald sortering
- Lägg till sorterings-logik i din computed property
- `.sort()` är din vän!

### Prioritetsnivåer
Lägg till prioritet (Låg, Medel, Hög) för varje task.

Tips:
- Lägg till `priority` i task-objektet
- Använd olika färger för olika prioriteter med `:class` eller `:style`
- Lägg till prioritet i formuläret (radio buttons eller select)

### Bulk Actions
Lägg till knappar för att:
- Markera alla som klara
- Ta bort alla klara tasks
- Återställ alla tasks till aktiva

### Animationer
Lägg till transitions när tasks läggs till eller tas bort.

- Använd Vue's `<TransitionGroup>` komponent
- Lägg till CSS transitions för smooth animations
- Kolla Vue-dokumentationen för transition-examples

## Resurser

- [Vue.js Dokumentation](https://vuejs.org/)
- [Vue.js Tutorial](https://vuejs.org/tutorial/)
- [Vite Dokumentation](https://vitejs.dev/)
- Shoppinglist-exemplet från lektionen
