# ARCH_COMPONENTS - Component Layer

## Why Components Are Pure

Components display data and capture user interactions. Nothing else. This makes them trivial to test, easy to understand, and safely modifiable.

## Core Rules

1. **No business logic** - Zero data transformation
2. **No direct API calls** - Never import graphql client
3. **Only call hooks** - Get callbacks from hooks
4. **Render from store** - Use Zustand selectors

## The Pure Component Pattern

```typescript
// GOOD: Component only renders and calls hooks
function PhraseDetailScreen() {
    const responses = useEnquiryStore(state => state.enquiryResponses);
    const loadDetails = useLoadPhraseDetails();

    useEffect(() => {
        loadDetails(id);
    }, [id]);

    return responses.map(r => (
        <Text>{r.displayName}</Text>  // Uses ADT property
    ));
}
```

## Why This Matters

- **Trivial to test** - Just render and assert
- **Easy to modify** - No business logic to break
- **Separated concerns** - UI ≠ business rules

## Anti-Patterns

- ❌ Data transformation in component
- ❌ Direct API calls
- ❌ Business logic (name parsing, etc.)
- ❌ Complex conditionals

## Route Files vs Feature Components

| Aspect | Route File (`app/`) | Feature Component (`features/*/components/`) |
|--------|---------------------|----------------------------------------------|
| Purpose | View assembly | Reusable UI piece |
| Contains | Component composition only | Pure rendering + hook calls |
| State | NEVER defines | Pulls from hooks |
| Styles | NEVER defines | Defines own styles |
| See | ARCH_ROUTES | This file |

## Data Access Pattern

**Components pull their own data from hooks.** No prop drilling.

```typescript
// GOOD: Component is self-sufficient
export function QuestionsList() {
    const { questions, loading, viewQuestion } = useQuestions();
    
    if (loading) return <LoadingSpinner />;
    return questions.map(q => (
        <QuestionItem key={q.id} question={q} onPress={() => viewQuestion(q.id)} />
    ));
}

// BAD: Prop drilling from parent
export function QuestionsList({ questions, loading, onViewQuestion }) {
    // Parent needs to know about this component's data needs
}
```

## When Props ARE Appropriate

- **Primitive display data** for leaf components: `<QuestionItem question={q} />`
- **Callbacks for specific instances**: `onPress={() => viewQuestion(q.id)}`
- **Reusable UI primitives**: `<Button title="Save" onPress={onSave} />`

## Rule of Thumb

- **Container components** (feature-specific): Use hooks directly
- **Presentational components** (reusable): Receive props

## DRY Principle

- **Shared styles** → Extract to Theme or shared component
- **Repeated UI patterns** → Extract to reusable component
- **Duplicate logic** → Extract to hook or use-case
- **When unsure** → ASK USER for clarification before creating new file

## Template: TP_COMPONENT
