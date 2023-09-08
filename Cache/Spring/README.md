# Cache with Spring

## Example 1: Using Spring's @Cacheable Annotation
In this example, we are utilizing Spring's @Cacheable annotation to enable caching for the getProductById method. This method is typically used to retrieve product information from a database or another data source. Here's a breakdown of how it works:

* We annotate the ProductService class with @Service to indicate that it's a Spring-managed service.
* Inside the ProductService class, the @Cacheable annotation is applied to the getProductById method. This annotation tells Spring to cache the results of this method based on its input parameters.
* "productCache" is specified as the value within the @Cacheable annotation. This indicates the name of the cache to use. If a product with a specific ID is requested, Spring will check if it exists in the cache before executing the method. If it's in the cache, the cached result is returned instead of executing the method again.

This approach significantly improves performance for frequently requested data, as it avoids costly database queries when the same data is requested multiple times.

```
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class ProductService {

    @Cacheable("productCache")
    public Product getProductById(Long id) {
        // This method fetches a product from a database or another data source.
        // If the same product ID is requested again, it will be retrieved from the cache.
        return productRepository.findById(id);
    }
}

```

## Example 2: Configuring Caching in Spring Boot

In this example, we enable caching for a Spring Boot application by creating a CacheConfig configuration class. Here's how it works:

* We annotate the CacheConfig class with @Configuration to specify that it's a configuration class.
* We include the @EnableCaching annotation to enable caching for the entire application. This annotation activates Spring's caching infrastructure.

Enabling caching at the application level ensures that caching is available for all Spring-managed components, such as services and repositories, without the need for individual annotations on each method.

```
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableCaching
public class CacheConfig {

}
```

## Example 3: Evicting Cache Entries

In this example, we use the @CacheEvict annotation to clear cache entries. This can be helpful in scenarios where cached data needs to be invalidated or refreshed. Here's how it works:

* The clearProductCache method within the ProductService class is annotated with @CacheEvict.
* "productCache" is specified as the value attribute within the @CacheEvict annotation. This indicates the name of the cache to be cleared.
* allEntries = true is set within the @CacheEvict annotation, which means that all entries within the "productCache" cache will be removed when this method is called.

By using the @CacheEvict annotation, you can control when and how specific cache entries are invalidated or cleared, ensuring that cached data remains up-to-date.

```
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.stereotype.Service;

@Service
public class ProductService {

    @CacheEvict(value = "productCache", allEntries = true)
    public void clearProductCache() {
        // This method clears the entire "productCache" cache.
    }
}
```

These caching strategies enhance application performance and responsiveness by reducing the need for repeated data retrieval from slower data sources like databases. They are particularly useful in scenarios where certain data is accessed frequently and can benefit from being stored in memory for faster access.

