# Midnight Society - Playwright Automation Framework

A comprehensive Playwright testing framework using the Page Object Model (POM) pattern for testing the Midnight Society website (midnightsociety.com).

## Project Structure

```
├── pages/                    # Page Object Model classes
│   ├── basePage.ts          # Base class with common methods
│   ├── homePage.ts          # Home page object
│   └── navigationPage.ts    # Navigation/Header page object
├── tests/                    # Test files
│   └── home.spec.ts         # Home page tests
├── playwright.config.ts      # Playwright configuration
├── tsconfig.json            # TypeScript configuration
├── package.json             # Project dependencies
├── .env                     # Environment variables
├── .gitignore              # Git ignore rules
└── README.md               # This file
```

## Features

✅ **Page Object Model Pattern** - Maintainable and scalable test structure
✅ **TypeScript Support** - Full type safety and IntelliSense
✅ **Cross-browser Testing** - Chrome, Firefox, Safari, and mobile browsers
✅ **Screenshot & Video Capture** - Automatic capture on failure
✅ **Parallel Execution** - Run tests concurrently for faster execution
✅ **HTML Reports** - Detailed test reports with screenshots
✅ **Environment Configuration** - Easy configuration through .env files

## Prerequisites

- **Node.js** (v16 or higher)
- **npm** or **yarn**

## Installation

1. Install dependencies:
```bash
npm install
```

2. Install Playwright browsers (if not already installed):
```bash
npx playwright install
```

## Configuration

### Environment Variables

Edit `.env` file to customize:
```
BASE_URL=https://www.midnightsociety.com
HEADLESS=true
SCREENSHOT=true
VIDEO=true
```

### Playwright Configuration

Modify `playwright.config.ts` to:
- Add/remove browsers
- Change timeout settings
- Set screenshot/video capture options
- Configure reporting

## Running Tests

### Run all tests
```bash
npm test
```

### Run tests in chromium browser
```bash
npm run test:chrome
```

### Run tests in firefox browser
```bash
npm run test:firefox
```

### Run tests in webkit (Safari) browser
```bash
npm run test:safari
```

### Run tests in headed mode (see browser)
```bash
npm run test:headed
```

### Run tests in debug mode
```bash
npm run test:debug
```

### Run tests with UI
```bash
npm run test:ui
```

### View test report
```bash
npm run report
```

## Page Object Model (POM) Structure

### BasePage Class
The base class provides common functionality:
- Navigation methods (`goto`, `goHome`)
- Element interactions (`click`, `fill`, `getText`)
- Wait methods (`waitForElement`, `waitForPageLoad`)
- Utility methods (`takeScreenshot`, `scrollToElement`)

### HomePage Class
Extends BasePage and contains:
- Home page selectors
- Page-specific methods
- Element verification methods

### NavigationPage Class
Extends BasePage and contains:
- Navigation/header selectors
- Menu interaction methods
- Mobile menu handling

## Writing Tests

### Basic Test Structure
```typescript
import { test, expect } from '@playwright/test';
import { HomePage } from '../pages/homePage';

test.describe('Home Page Tests', () => {
  let homePage: HomePage;

  test.beforeEach(async ({ page }) => {
    homePage = new HomePage(page);
    await homePage.navigateToHome();
  });

  test('should load home page', async () => {
    const isLoaded = await homePage.isHomePageLoaded();
    expect(isLoaded).toBeTruthy();
  });
});
```

### Using Page Objects
```typescript
// Navigate
await homePage.navigateToHome();

// Interact with elements
await homePage.click(selector);
await homePage.fill(selector, 'text');

// Get information
const text = await homePage.getText(selector);
const title = await homePage.getPageTitle();

// Wait and verify
await homePage.waitForElement(selector);
const isVisible = await homePage.isElementVisible(selector);
```

## Creating New Page Objects

1. Create a new file in `pages/` directory:
```typescript
import { Page } from '@playwright/test';
import { BasePage } from './basePage';

export class MyPage extends BasePage {
  // Define selectors as private properties
  private readonly SELECTOR_NAME = 'selector';

  constructor(page: Page) {
    super(page);
  }

  // Define methods for page interactions
  async myMethod(): Promise<void> {
    // Implementation
  }
}
```

2. Use in tests:
```typescript
import { MyPage } from '../pages/myPage';

test('test name', async ({ page }) => {
  const myPage = new MyPage(page);
  await myPage.myMethod();
});
```

## Test Reports

After running tests, view the HTML report:
```bash
npm run report
```

Reports include:
- Test results and status
- Screenshots on failure
- Videos of test execution
- Detailed error messages
- Browser and OS information

## Best Practices

1. **Selectors**: Keep selectors specific and avoid brittle selectors
2. **Waits**: Use appropriate wait methods instead of arbitrary timeouts
3. **Page Objects**: Keep page objects focused on a single page/component
4. **Tests**: Write tests that are independent and can run in any order
5. **Error Handling**: Use meaningful assertions and error messages
6. **Screenshots**: Enable automatic screenshots for debugging

## Debugging

### Debug mode
```bash
npm run test:debug
```

### Run specific test
```bash
npx playwright test tests/home.spec.ts
```

### Run specific test case
```bash
npx playwright test -g "test name"
```

### Inspect selectors
Use Playwright Inspector:
```bash
npx playwright codegen https://www.midnightsociety.com
```

## Troubleshooting

### Tests timing out
- Increase timeout in `playwright.config.ts`
- Check if website is accessible
- Verify selectors are correct

### Screenshot not capturing
- Ensure `screenshot: 'only-on-failure'` is set in config
- Check screenshots/ directory exists

### Selector not found
- Use Playwright Inspector to verify selector
- Use page.pause() to debug in headed mode
- Check if element is dynamically loaded

## CI/CD Integration

The framework is ready for CI/CD pipelines:

```yaml
# Example GitHub Actions
- run: npm install
- run: npx playwright install
- run: npm test
- uses: actions/upload-artifact@v2
  if: always()
  with:
    name: playwright-report
    path: playwright-report/
```

## Contributing

1. Create feature branches
2. Write tests for new features
3. Update page objects as needed
4. Create pull requests with test results

## Support

For issues or questions:
1. Check Playwright documentation: https://playwright.dev
2. Review test reports for detailed error messages
3. Use debug mode to inspect elements and selectors

## License

Klixa

## Author

QA - Tests
