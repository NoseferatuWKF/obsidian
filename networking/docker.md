## Best practices

- [Development best practices](https://docs.docker.com/develop/dev-best-practices/)
- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Security best practices](https://docs.docker.com/develop/security-best-practices/)

cache busting
```Dockerfile
ARG CACHE_BUST=1
```

[working with multiple compose files](https://docs.docker.com/compose/multiple-compose-files/)
```yaml
include: another.compose.yml
service:
	backend:
		depends_on: service_from_another_compose_file
		# ----
```
