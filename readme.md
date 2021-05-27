Workaround for removed Smallrye Microprofile Implementations in Keycloak 13.0.x
---

# Build
```
docker build -t thomasdarimont/keycloak:13.0.1.MP . 
```

# Run
```
docker run \
  --rm \
  --name keycloak-mp \
  -it \
  -p 8080:8080 \
  -p 9990:9990 \
  thomasdarimont/keycloak:13.0.1.MP \
  -b 0.0.0.0 \
  -bmanagement 0.0.0.0 \
  -Dwildfly.statistics-enabled=true
```

# Smallrye Health
http://localhost:9990/health

# Smallrye Metrics
http://localhost:9990/metrics