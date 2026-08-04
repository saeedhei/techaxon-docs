docker run --rm httpd:2.4-alpine htpasswd -nbB admin StrongPassword123
admin:$$2y$$05$$xNTLJdVwa.etzJrEfOsdw.ZH9ZKB5eWLltYxQbG9JvrxfcyKkykhG

Docker Compose recognizes the $ character as a system variable. So you need to double ($$) all $ signs in the terminal output.

docker compose down
docker compose up -d

docker compose down && docker compose up -d


docker compose down --remove-orphans
