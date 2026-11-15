# Paolo's UI Garden – Assignment 13

This is my UI component library from Assignment 12. For this assignment, I added code quality checks so that bad code can't be committed or pushed without being caught.

GitHub repo: https://github.com/pdivino/coding-assignment13

## What you need

- Node.js
- Git
- Docker Desktop (it needs to be running)

## How to run it on localhost:8018

1. Clone the repo and go into the folder:

```bash
git clone https://github.com/pdivino/coding-assignment13.git
cd coding-assignment13
```

2. Build the Docker image:

```bash
docker build -t divino_paolo_coding_assignment13 .
```

3. Start the container:

```bash
docker run -d -p 8018:8018 --name divino_paolo_coding_assignment13 divino_paolo_coding_assignment13
```

4. Open http://localhost:8018 in your browser.

## Docker setup

- The container is named `divino_paolo_coding_assignment13`.
- The site files are stored in `/divino_paolo_ui_garden_build_checks` inside the container.
- The first stage uses Node.js to create the production build.
- The second stage uses Nginx to host the site on port 8018.

## Code quality checks

I added three checks to the project:

- **Prettier** checks that the code is formatted the same way everywhere.
- **ESLint** checks the code for errors and warnings.
- **Tests** runs all of my component tests.

You can run them yourself with:

```bash
npm run format:check
npm run lint
npm run test:ci
```

## Husky (checks before every commit)

Husky runs all three checks every time I try to commit. If any check fails, the commit is stopped.

To test it, I changed a line in `src/index.tsx` to break the formatting and tried to commit. Husky stopped the commit with the message `husky - pre-commit script failed`.

## GitHub Actions (checks after every push)

Husky can be skipped with `git commit --no-verify`, so GitHub Actions runs the same checks again every time code is pushed. If a check fails, the build is marked as failed and GitHub sends me an email.

To test it, I made a branch called `test-bypass`, committed badly formatted code with `--no-verify`, and pushed it. The GitHub Actions run failed at the Prettier check.

You can see all the runs here: https://github.com/pdivino/coding-assignment13/actions