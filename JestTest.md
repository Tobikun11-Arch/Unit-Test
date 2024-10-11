# Setting Up Jest for Unit Testing in Next.js

This guide will walk you through setting up Jest in your Next.js project for unit testing.

## Installation

Run the following commands to install the necessary packages:

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom @testing-library/user-event
npm install --save-dev @types/jest @types/testing-library__react
npm install --save-dev jest-environment-jsdom
```

Create a file named `jest.config.ts` in your root directory:

```typescript
import type { Config } from 'jest'
import nextJest from 'next/jest.js'

const createJestConfig = nextJest({
  // Path to your Next.js app to load next.config.mjs and .env files
  dir: './', // This is only if your next.config and .env are located in the main directory, not in a folder
})

const config: Config = {
  coverageProvider: 'v8',
  testEnvironment: 'jsdom',
  moduleNameMapper: {
    // Handle image imports
    '^.+\\.(png|jpg|jpeg|gif|webp|avif|svg)$': '<rootDir>/__mocks__/FileMock.ts', // FileMock.ts only if you are using Tailwind CSS
  },
}

export default createJestConfig(config)
```

Add a test script to your `package.json`:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

### Creating Your First Test

1. Create a folder called `__tests__` in your project's root directory.
2. Inside the `__tests__` folder, create a file named `page.test.tsx`:

```typescript
import { render, screen } from '@testing-library/react'
import Page from '../app/page'
import '@testing-library/jest-dom'

test('renders the correct heading', () => {
  // Render the Page component
  render(<Page />)
  
  // Get the h1 element with specific text content
    <!-- this name: Saju if specific or you can make it like this const heading = screen.getByRole('heading', { level: 1 }) -->
  const heading = screen.getByRole('heading', { level: 1, name: 'SajuBazaar' })
  
  // Assert that the heading is in the document
  expect(heading).toBeInTheDocument()
})
```

Then create a file named `snapshot.js` in the same folder as `page.test.tsx`:

```typescript
import { render } from '@testing-library/react'
import Page from '../app/page'

it('renders homepage unchanged', () => {
  const { container } = render(<Page />)
  expect(container).toMatchSnapshot()
})
```

### Last is FileMock

Create a folder named `__mocks__` in your root directory and make a file in that folder called `FileMock.ts`:

```typescript
// __mocks__/fileMock.ts
const fileMock = 'test-file-stub';
export default fileMock;
```

## Running Your Tests

To run your tests, use the following command:

```bash
npm run test
```

Or, if you're using Yarn:

```bash
yarn test
```

Or with pnpm:

```bash
pnpm test
```
