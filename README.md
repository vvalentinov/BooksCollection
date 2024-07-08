# Books Collection

Course Exam Solution for Software Engineering and DevOps at SoftUni.

## Introduction

This project is a solution to the final exam for the Software Engineering and DevOps at SoftUni. The objective of the exam is to fix all of the UI tests and create a continuous integration pipeline that:

1. Builds and unit tests the application.
2. Builds and UI tests the application.
3. Deploys the application to Render.

## Setup and Installation

1. Clone the repository:

```bash
  git clone https://github.com/vvalentinov/BooksCollection.git
```

2. Navigate to the project directory:

```bash
  cd BooksCollection
```

3. Install the dependencies:

```bash
  npm install
```

## Running the tests

1. Unit Tests

```bash
  npm run test:unit
```

2. UI Tests

```bash
  npm run test:ui
```

## Continuous Integration Pipeline

The continuous integration pipeline is configured using GitHub Actions. It consists of three main jobs:

1. Build and unit test the application:

```bash
  build_and_unit-test:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4
    - name: Setup Node.js 20.x
      uses: actions/setup-node@v4
      with:
        node-version: 20.x
        cache: 'npm'
    - name: Install NPM Dependencies
      run: npm install
    - name: Execute Unit Tests
      run: npm run test:unit
    - name: Display SoftUni UserName
      run: echo 'Valentin_104'
```

2. Build and UI test the application:

```bash
  build_and_ui-test:
    needs: build_and_unit-test
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      - name: Setup Node.js 20.x
        uses: actions/setup-node@v4
        with:
          node-version: 20.x
          cache: 'npm'
      - name: Install Dependencies
        run: npm install
      - name: Install Playwright Browsers
        run: npx playwright install
      - name: Start Application
        run: npm start &
      - name: Execute UI Tests
        run: npm run test:ui
```

3. Deploy application

```bash
  deploy_application:
    needs: build_and_ui-test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        uses: johnbeynon/render-deploy-action@v0.0.8
        with:
          service-id: ${{ secrets.MY_RENDER_SERVICE_ID }}
          api-key: ${{ secrets.MY_RENDER_API_KEY }}
```

## License

This project is licensed under the [MIT License](https://choosealicense.com/licenses/mit/).