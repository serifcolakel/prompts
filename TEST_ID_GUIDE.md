You are a senior React Native engineer.

Your task is to add stable and consistent `testID` props to the React Native application to improve E2E test automation with Maestro and Playwright.

## Goal

Add `testID` to all interactive and important UI components without changing any existing behavior, styling, or business logic.

## Components that MUST have testID

- Button
- Pressable
- TouchableOpacity
- TouchableHighlight
- TouchableWithoutFeedback
- TextInput
- Switch
- Checkbox
- Radio Button
- Slider
- Picker / Select
- Search input
- Tabs
- Segmented controls
- List items that users interact with
- Cards that are clickable
- Floating Action Buttons
- Modal actions
- BottomSheet actions
- Navigation buttons
- Back buttons
- Close buttons
- Icons that are tappable
- Menu items
- Dropdown triggers
- Date picker trigger
- Time picker trigger
- Upload buttons
- Camera buttons
- Login/Register actions
- Form submit actions
- Empty state CTA buttons

Also add testIDs to important containers when it improves test stability:

- Screen root
- Modal root
- Bottom sheet root
- Loading indicator
- Error state
- Empty state
- Success state
- Toast container (if applicable)

---

## Naming Convention

Use lowercase kebab-case.

Format:

screen-element-action

Examples:

home-screen
login-screen
profile-screen

login-email-input
login-password-input
login-submit-button

settings-save-button
settings-logout-button

product-search-input
product-card
product-card-0
product-card-1

cart-checkout-button

profile-avatar-image

loading-spinner

error-message

success-banner

---

## Lists

For FlatList/SectionList/FlashList items use:

testID={`product-card-${index}`}

or if a stable id exists:

testID={`product-card-${item.id}`}

Prefer stable ids over indexes whenever possible.

---

## Duplicate IDs

Every testID on the same screen must be unique.

Avoid generic names like:

button
input
text

Instead use semantic names.

Good:

login-submit-button

Bad:

button

---

## Existing testIDs

- Never rename existing testIDs.
- Do not create duplicates.
- Reuse existing conventions.

---

## Accessibility

Do not replace accessibilityLabel.

Keep accessibility props untouched.

Simply add testID alongside them.

---

## Do NOT

- Do not modify logic
- Do not change styles
- Do not refactor components
- Do not rename variables
- Do not change JSX structure unless required to add testID

---

## Output

Only modify files where testIDs need to be added.

After finishing, provide a summary containing:

- Files modified
- Number of testIDs added
- Duplicate testIDs found (if any)
- Components intentionally skipped and why
