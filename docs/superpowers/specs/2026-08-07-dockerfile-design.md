# Dockerfile Design

Create a production-oriented Docker setup for the existing npm-based Next.js app.

- Use a multi-stage Node.js 22 Alpine `Dockerfile` so build-only dependencies and source files are absent from the runtime image.
- Install reproducibly with `npm ci`, build with `npm run build`, and retain production dependencies only.
- Run `npm start` as the non-root `node` user on port 3000.
- Add `.dockerignore` rules for dependencies, build output, Git metadata, logs, environment files, private keys, Kubernetes/Talos configs, Terraform values/state, and common cloud credentials.
- Extend `.gitignore` with the same credential protections. Keep all `.env*` files ignored without exceptions.
- Do not enable Next.js standalone output or add other configuration.

Success means the ignore rules cover the stated sensitive-file categories and `docker build` completes using the repository as its build context.
