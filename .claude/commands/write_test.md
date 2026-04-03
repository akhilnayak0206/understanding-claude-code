# Write Tests for Claude Code

## Commands for Writing React Test Cases

### 1. Run Tests
```bash
npm test
```

### 2. Run Tests in Watch Mode
```bash
npm run test:watch
```

### 3. Run Tests with Coverage
```bash
npm run test:coverage
```

### 4. Run Specific Test File
```bash
npm test -- ChatInterface.test.tsx
```

### 5. Run Tests for Specific Component
```bash
npm test -- --testNamePattern="ChatInterface"
```

## React Testing Examples

### Basic Component Test
```typescript
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import YourComponent from './YourComponent';

describe('YourComponent', () => {
  it('renders correctly', () => {
    render(<YourComponent />);
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });
});
```

### Test with User Interactions
```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import YourComponent from './YourComponent';

describe('YourComponent', () => {
  it('handles button click', () => {
    render(<YourComponent />);
    const button = screen.getByRole('button');
    fireEvent.click(button);
    expect(screen.getByText('Clicked!')).toBeInTheDocument();
  });
});
```

### Test with Mock Functions
```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import YourComponent from './YourComponent';

describe('YourComponent', () => {
  it('calls callback on action', () => {
    const mockCallback = vi.fn();
    render(<YourComponent onAction={mockCallback} />);
    fireEvent.click(screen.getByText('Action'));
    expect(mockCallback).toHaveBeenCalled();
  });
});
```

### Test Async Operations
```typescript
import { render, screen, waitFor } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import YourComponent from './YourComponent';

describe('YourComponent', () => {
  it('loads data asynchronously', async () => {
    render(<YourComponent />);
    await waitFor(() => {
      expect(screen.getByText('Loaded Data')).toBeInTheDocument();
    });
  });
});
```

## How to Run Tests for Claude Code

1. **Navigate to project directory**
   ```bash
   cd **get location**/
   ```

2. **Run all tests**
   ```bash
   npm test
   ```

3. **Run tests in watch mode for development**
   ```bash
   npm run test:watch
   ```

4. **Generate coverage report**
   ```bash
   npm run test:coverage
   ```

## Testing Best Practices

- Write descriptive test names
- Test user behavior, not implementation details
- Use meaningful assertions
- Mock external dependencies
- Test edge cases and error states
- Keep tests simple and focused