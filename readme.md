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
  -p 8787:8787 \
  -p 9990:9990 \
  -e KEYCLOAK_USER=admin \
  -e KEYCLOAK_PASSWORD=admin \
  thomasdarimont/keycloak:13.0.1.MP \
  --debug 0.0.0.0:8787 \
  -b 0.0.0.0 \
  -bmanagement 0.0.0.0 \
  -Dwildfly.statistics-enabled=true
```

# Smallrye Health
http://localhost:9990/health

# Smallrye Metrics
http://localhost:9990/metrics

# Misc

## Mount your extensions

To add your custom extensions that use Microprofile, you could use a volume mount like:
```
-v $PWD/tmp/keycloak-extensions-bundle.ear:/opt/jboss/keycloak/standalone/deployments/keycloak-extensions-bundle.ear:z \

-v $PWD/tmp/keycloak-extensions.jar:/opt/jboss/keycloak/standalone/deployments/keycloak-extensions.jar:z \
```

## Mount your extensions

In order to use eclipse-microprofile with smallrye I needed to add the following entries to the `jboss-deployment-structure.xml`.

```xml
    <!-- The following are needed for microprofile usage -->
    <module name="io.smallrye.metrics" export="true" />
    <module name="io.smallrye.config" export="true" />
    <module name="io.smallrye.health" export="true" />
    <module name="org.eclipse.microprofile.config.api" export="true" />
    <module name="org.eclipse.microprofile.health.api" export="true" />
    <module name="org.eclipse.microprofile.metrics.api" export="true" />
```

I also needed to declare the dependencies for smallrye-metrics,smallrye-config,smallrye-health in my extension module.
```xml
<dependency>
   <!-- used for metrics collection -->
   <groupId>io.smallrye</groupId>
   <artifactId>smallrye-metrics</artifactId>
   <version>${smallrye-metrics.version}</version>
   <scope>provided</scope>
</dependency>

<dependency>
   <!-- used for metrics collection -->
   <groupId>org.eclipse.microprofile.metrics</groupId>
   <artifactId>microprofile-metrics-api</artifactId>
   <version>${eclipse-metrics.version}</version>
   <scope>provided</scope>
</dependency>
```