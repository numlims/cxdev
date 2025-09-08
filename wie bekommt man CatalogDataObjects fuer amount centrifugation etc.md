amount units:

```
this.workflowServiceDispatcher.getLoadCatalogDataService().findAllAmountUnits();
```

catalog data: (stammdaten > system > benutzerdefinierte kataloge)

```
this.workflowServiceDispatcher.getLoadCatalogDataService().searchCalogEntriesByCustomCatalog("MY_CATALOG_CODE");
```

zentrifugation: (stammdaten > probe > probendaten >
zentrifugationsart)

```
this.workflowServiceDispatcher.getLoadCatalogDataService().findStockProcessings(null);
```