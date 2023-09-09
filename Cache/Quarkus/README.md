# Cache with Quarkus

## Example 1: Using Quarkus' @CacheResult Annotation

```
import io.quarkus.cache.CacheResult;
import javax.enterprise.context.ApplicationScoped;

@ApplicationScoped
public class ProductService {

    @CacheResult(cacheName = "productCache")
    public Product getProductById(Long id) {
        // This method fetches a product from a database or another data source.
        // If the same product ID is requested again, it will be retrieved from the cache.
        return productRepository.findById(id);
    }
}
```

In this example, we are using Quarkus' @CacheResult annotation to enable caching for the getProductById method. This method is responsible for retrieving product information from a data source, such as a database. Here's a detailed breakdown of how it works:

* We annotate the ProductService class with @ApplicationScoped to indicate that it's a Quarkus-managed bean.
* Inside the ProductService class, the @CacheResult annotation is applied to the getProductById method. This annotation informs Quarkus to cache the results of this method based on its input parameters.
* "productCache" is specified as the cacheName attribute within the @CacheResult annotation. This attribute indicates the name of the cache to be used. If a product with a specific ID is requested, Quarkus checks whether it exists in the cache before executing the method. If it's in the cache, the cached result is returned, avoiding the need to execute the method again.
By using the @CacheResult annotation, you optimize the performance of your application by preventing redundant calls to expensive data retrieval operations, such as database queries.


## Example 2: Configuring Caching in Quarkus

```
import io.quarkus.cache.CacheInvalidateAll;
import javax.enterprise.context.ApplicationScoped;

@ApplicationScoped
public class ProductService {

    @CacheInvalidateAll(cacheName = "productCache")
    public void clearProductCache() {
        // This method clears the entire "productCache" cache.
    }
}
```

In this example, we demonstrate how to configure caching in a Quarkus application and use the @CacheInvalidateAll annotation to clear cache entries. Here's a detailed explanation:

* We annotate the ProductService class with @ApplicationScoped to specify that it's a Quarkus-managed bean.
* The clearProductCache method within the ProductService class is annotated with @CacheInvalidateAll.
* "productCache" is specified as the cacheName attribute within the @CacheInvalidateAll annotation. This attribute indicates the name of the cache to be cleared.
* When the clearProductCache method is called, Quarkus will invalidate and clear all entries within the "productCache" cache.
Using the @CacheInvalidateAll annotation allows you to control cache invalidation and clearance, ensuring that cached data remains up-to-date when needed.

These caching strategies provided by Quarkus enhance the performance and responsiveness of your application by reducing the need for repeated and resource-intensive data retrieval operations, such as database queries. They are especially valuable in scenarios where certain data is frequently accessed and can benefit from being cached for faster access.
