@GetMapping("/cache/stats")
    public Object getCacheStats() {
        Cache<Object, Object> cache = (Cache<Object, Object>) cacheManager.getCache("employeesCache").getNativeCache();
        return cache.asMap();
    }
