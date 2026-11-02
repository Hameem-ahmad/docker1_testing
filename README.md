## Requirements
Node.js 22+, npm, Git, Docker

## Run locally
npm ci
npm start          # http://localhost:3000   (use the script name that worked for you)
npm test
npm run build

## Run with Docker
docker build -t reactapp:latest .
docker run -d -p 8080:80 --name reactapp reactapp:latest   # http://localhost:8080

## Branches
- master: stable
- dev: development (Jenkins runs on every push)
