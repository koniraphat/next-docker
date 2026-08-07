# Dockerfile Design

Create one single-stage `Dockerfile` for the existing npm-based Next.js app.

- Base image: Node.js 22 Alpine.
- Install dependencies with `npm ci` from `package-lock.json`.
- Copy the application and build it with `npm run build`.
- Expose port 3000 and start it with `npm start`.
- Do not add a multi-stage build, `.dockerignore`, or extra configuration.

Success means `docker build` completes using the repository as its build context.
