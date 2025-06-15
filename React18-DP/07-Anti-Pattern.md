# React 18 Design Patterns - Anti-Patterns

## 🚨 **Anti-Pattern Overview**
- **Common mistakes** developers make when writing React applications
- **Performance impacts** - anti-patterns can harm application performance
- **Behavioral issues** - unexpected component behavior and bugs
- **Best practice violations** - going against React's intended usage patterns
- **Learning opportunity** - understanding why certain approaches are problematic

## 🔄 **State Initialization Anti-Pattern**

### **The Problem:**
- **Props-to-state initialization** - setting initial state from props
- **Duplicated source of truth** - same data in both props and state
- **Stale state** - component doesn't update when prop changes
- **Unclear data ownership** - ambiguous which value is current

### **Problematic Code Example:**
```mermaid
graph TD
    A[Parent Component] --> B[count: 1 prop]
    B --> C[Counter Component]
    C --> D[useState with props.count]
    D --> E[State: count: 1]
    F[Parent Updates count: 10] --> G[Props: count: 10]
    G --> H[State Still: count: 1]
    H --> I[Inconsistent State]
```

### **Issues with Props-to-State:**
- **Initialization only** - useState only runs once on mount
- **Prop changes ignored** - subsequent prop updates don't affect state
- **Developer confusion** - unclear which value to trust
- **Debugging difficulty** - hard to track source of data

### **Solution Strategies:**
- **Explicit naming** - use `initialCount` instead of `count`
- **Controlled components** - parent manages state, child receives props
- **useEffect updates** - sync state with prop changes when necessary
- **Derived state** - calculate values from props instead of storing

### **When It's Acceptable:**
- **Truly initial values** - prop only sets initial state, never changes
- **Clear naming** - `initialValue`, `defaultValue` prefixes
- **Documentation** - clear comments about intended behavior
- **Team agreement** - consistent patterns across codebase

## 🔑 **Array Index as Key Anti-Pattern**

### **Key Property Purpose:**
- **Element identification** - React tracks elements across renders
- **Reconciliation optimization** - efficient DOM updates
- **Component identity** - maintains component state correctly
- **Performance improvement** - minimizes unnecessary re-renders

### **Index Key Problems:**
```mermaid
flowchart TD
    A[Initial List: foo, bar] --> B[Index Keys: 0, 1]
    C[Add 'baz' to Top] --> D[New List: baz, foo, bar]
    D --> E[Index Keys: 0, 1, 2]
    E --> F[React Sees: Change 0→baz, Change 1→foo, Add 2→bar]
    F --> G[Incorrect DOM Updates]
```

### **Index Key Issues:**
- **Non-stable identifiers** - same element gets different keys
- **Wrong reconciliation** - React updates existing elements instead of creating new ones
- **State confusion** - component state stays with wrong elements
- **Performance degradation** - unnecessary DOM manipulations

### **Visual Example:**
- **Input fields** maintain their values when items reorder
- **List items** get new text but input values stay in place
- **User confusion** - form data doesn't match displayed items
- **Data integrity** - user input associated with wrong items

### **Proper Key Strategies:**
- **Unique identifiers** - use database IDs or UUIDs
- **Stable values** - content-based keys if items don't repeat
- **Composite keys** - combine multiple fields for uniqueness
- **Generated IDs** - create unique identifiers when none exist

### **Key Selection Guidelines:**
```mermaid
mindmap
  root((Key Selection))
    Unique
      Database ID
      UUID
      Generated ID
      Composite Key
    Stable
      Same Element = Same Key
      Survives Reorders
      Consistent Across Renders
    Predictable
      Not Random
      Not Index-Based
      Meaningful to Data
```

## 📡 **Props Spreading Anti-Pattern**

### **Spreading Concept:**
- **Convenience pattern** - `<Component {...props} />` passes all props
- **Common practice** - widely used in React community
- **Hidden danger** - can pass invalid HTML attributes
- **DOM pollution** - unknown attributes added to DOM elements

### **The Problem:**
- **Unknown HTML attributes** - non-standard props become DOM attributes
- **Console warnings** - React warns about invalid props
- **HTML validation** - creates invalid HTML markup
- **Hidden properties** - unclear what props are being passed

### **Warning Example:**
```mermaid
graph TD
    A[Div with props] --> B[Props: foo: 'bar', className: 'baz']
    B --> C[DOM: <div foo="bar" class="baz">]
    C --> D[Console Warning: Unknown prop 'foo']
```

### **Spreading Issues:**
- **Lack of control** - can't filter out invalid props
- **Debugging difficulty** - hard to track which props cause issues
- **Maintainability** - changes to parent props affect DOM unexpectedly
- **HTML standards** - violates semantic HTML principles

### **Safe Spreading Solutions:**
- **Explicit DOM props** - `<div {...props.domProps} />`
- **Prop filtering** - extract known props, rest for DOM
- **Destructuring** - separate DOM props from component props
- **Prop validation** - use PropTypes or TypeScript to catch issues

### **Better Patterns:**
```mermaid
flowchart TD
    A[Parent Props] --> B{Separate Concerns}
    B --> C[DOM Props]
    B --> D[Component Props]
    C --> E[div with domProps]
    D --> F[Component Logic]
```

## 🛡️ **Anti-Pattern Prevention**

### **Development Practices:**
- **Code reviews** - catch anti-patterns before merge
- **Linting rules** - ESLint rules for common anti-patterns
- **TypeScript** - type safety prevents many issues
- **Testing** - unit tests reveal unexpected behaviors

### **Team Guidelines:**
- **Coding standards** - document acceptable patterns
- **Training** - educate team on React best practices
- **Pair programming** - knowledge sharing prevents mistakes
- **Documentation** - clear examples of dos and don'ts

### **Tool Support:**
```mermaid
graph TD
    A[Development Tools] --> B[ESLint Rules]
    A --> C[React DevTools]
    A --> D[TypeScript]
    A --> E[Testing Libraries]
    B --> F[Catch Anti-patterns]
    C --> G[Debug Issues]
    D --> H[Type Safety]
    E --> I[Verify Behavior]
```

## 🔍 **Debugging Anti-Patterns**

### **State Initialization Issues:**
- **React DevTools** - compare props vs state values
- **Console logging** - track prop changes over time
- **Component lifecycle** - understand when initialization occurs
- **State updates** - verify when and why state changes

### **Key-Related Problems:**
- **DOM inspection** - check actual key attributes
- **React DevTools** - see component tree with keys
- **Console warnings** - React warns about missing keys
- **List behavior** - test reordering and updates

### **Props Spreading Debug:**
- **Console warnings** - unknown prop messages
- **DOM inspection** - check for invalid attributes
- **Prop logging** - see what props are being passed
- **HTML validation** - use tools to check markup validity

## 📚 **Best Practice Alternatives**

### **State Management:**
- **Lifted state** - manage state in parent component
- **Controlled components** - parent controls child behavior
- **useEffect sync** - sync state with prop changes when needed
- **Derived state** - calculate from props instead of storing

### **List Rendering:**
- **Unique IDs** - generate or use existing unique identifiers
- **Content hashing** - create stable keys from item content
- **Database keys** - use server-provided unique identifiers
- **UUID generation** - create unique keys for client-side items

### **Props Handling:**
```mermaid
flowchart LR
    A[Component Props] --> B[Destructure]
    B --> C[Known Props]
    B --> D[Rest Props]
    C --> E[Component Logic]
    D --> F[Filter Valid DOM Props]
    F --> G[Element with validProps]
```

### **Prop Validation:**
- **TypeScript interfaces** - define expected prop types
- **PropTypes** - runtime prop validation
- **Default props** - provide sensible defaults
- **Required props** - mark essential props as required

## 🎯 **Anti-Pattern Detection**

### **Code Review Checklist:**
- **State initialization** - check for props-to-state patterns
- **List keys** - verify unique, stable keys
- **Props spreading** - ensure safe DOM prop spreading
- **Performance impact** - consider reconciliation efficiency

### **Automated Detection:**
- **ESLint rules** - `react/no-array-index-key`, `react/no-unknown-property`
- **TypeScript errors** - catch type mismatches
- **React warnings** - development mode console messages
- **Testing failures** - unexpected behavior in tests

### **Learning Approach:**
- **Understand why** - learn the reasoning behind anti-patterns
- **Practice identification** - recognize patterns in existing code
- **Refactoring skills** - know how to fix problematic code
- **Prevention mindset** - think about potential issues while coding