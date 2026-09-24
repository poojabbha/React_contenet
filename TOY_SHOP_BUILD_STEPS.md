# Toy Shop App — Build Steps

Since App.js, Header.js, ToyForm.js, ToyList.js, and App.css already have a full working version, here's the build order that matches this project — follow it to rebuild/practice the same app from scratch.

## Step 1: Plan the data shape first (no code yet)
Decide what one "toy" object looks like — this drives everything else:
```
{ id, toyName, category, ageGroup, price, quantity, description, available, imageUrl }
```

## Step 2: Header.js — static component (easiest, do this first)
- Just a functional component returning a `<header>` with a title and subtitle.
- No props, no state. Good warm-up before touching logic.

## Step 3: App.js — set up state and handler functions (the "brain")
Work in this order inside `App`:
1. `useState` for the `toys` array, seeded with one sample toy.
2. `addToy(newToy)` — takes form data, adds an `id` (e.g. `Date.now()`), appends to array via `setToys`.
3. `deleteToy(toyId)` — filters the array by `id`.
4. Return JSX: `<Header />`, then `<ToyForm onAddToy={addToy} />` and `<ToyList toys={toys} onDeleteToy={deleteToy} />` inside a layout wrapper.

Do state/handlers here first, even before ToyForm/ToyList are complete — App.js is the contract both children rely on.

## Step 4: ToyForm.js — build field-by-field, not all at once
1. `useState(initialFormData)` for the form object + a generic `handleChange(event)` that updates by `name`.
2. Add the **Toy Name** text input first — wire it to `formData`/`handleChange` and confirm typing updates state (console.log or React DevTools).
3. Add **Category** `<select>` — same `handleChange` handles it automatically.
4. Add **Age Group** radios — note `checked={formData.ageGroup === "value"}` pattern.
5. Add **Price** and **Quantity** number inputs (wrap in a `.row` div).
6. Add **Description** textarea with a character counter (`{formData.description.length}/150`).
7. Add **Image upload** — separate `useState` for `selectedFile`/`imageUrl`, a `handleFileChange` that validates type/size and calls `URL.createObjectURL`. Add the `useEffect` cleanup (`URL.revokeObjectURL`) right after.
8. Add **Available** checkbox.
9. Write `validateForm()` last, once all fields exist — check each field and return an error string.
10. Write `handleSubmit`: call `validateForm`, bail with `setError` if invalid, else build `newToy`, call `onAddToy(newToy)`, then `resetForm()`.
11. Add the Submit/Reset buttons.

## Step 5: ToyList.js — display before delete
1. Render `toys.length` count and an empty-state message.
2. Map over `toys`, render each as a card: image (or placeholder), name, category, age group, price, quantity, availability status, description.
3. Add the **Delete** button last, wired to `onDeleteToy(toy.id)`.

## Step 6: App.css — style once everything renders
Style in this order: layout (`.app-container`, `.shop-layout` flex), then form (`.form-group`, inputs), then list (`.toy-grid`, `.toy-card`), then states (`.available`/`.not-available`, `.error-message`).

## Step 7: Test the full loop
Add a toy → confirm it appears in the list with correct fields → delete it → confirm it's removed → try submitting with empty/invalid fields to confirm validation blocks it.

---
**Why this order:** static → state/plumbing → input-by-input → display → delete → styling. Each step is independently testable, so you catch mistakes before they compound into the next component.
