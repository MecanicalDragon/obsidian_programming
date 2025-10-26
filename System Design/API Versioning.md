API Versioning should be implemented with the `MAJOR.MINOR.PATCH` template; increment the:
1. `MAJOR` version when you make incompatible API changes,
2. `MINOR` version when you add functionality in a backward-compatible manner, and
3. `PATCH` version when you make backward-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the `MAJOR.MINOR.PATCH` format.

[Semantic versioning](https://semver.org/)

| Breaking changes                    | Non-breaking changes             |
| ----------------------------------- | -------------------------------- |
| Adding required query parameter     | Adding a resource or method      |
| Adding required response field      | Adding optional response field   |
| Deleting a resource or method       | Adding optional query parameters |
| Deleting a response field           |                                  |
| Modifying a resource or method URI  |                                  |
| Modifying a field name              |                                  |
| Modifying required query parameters |                                  |
| Modifying authorization             |                                  |
| Modifying rate-limiting             |                                  |

Once you determine that you need a new version of your API, you need to decide how to handle it. Preferably, you have decided ahead of time and encouraged API consumers to request version 1 of your API. There are three common approaches to implement API versioning:

1. **Resource versioning** - version is a part of the Accept header in the HTTP request. `Accept: application/vnd.github.v3+json` is sent to `GET /customers`. This is considered the preferred form of versioning by many, as the resource representations are versioned while keeping resource URIs the same. Some APIs choose to provide the latest version as the default, if not provided in the Accept header
2. **URI versioning** - the version is part of the URI, either as a prefix or suffix. e.g. `/v1/customers` or `/customers/v1`. While URI-versioning isn’t as pure as content-based versioning, it tends to be the most common as it works across a variety of tools that may not support customized headers. The downside is that resource URIs change with each new version, which some consider counter to the intent of having a URI that never changes.
3. **Hostname versioning** - the version is part of the hostname rather than the URI. e.g. `https://v2.api.myapp.com/customers`. This approach is used when technology limitations prevent routing to the proper backend version of the API based on the URI or Accept header.

No matter which option you choose, API versions should only include the major number. Minor numbers should not be required (e.g. `/v1/customers`, not `/v1.1/customers`).

Also: see [deprecation header](https://tools.ietf.org/id/draft-dalal-deprecation-header-01.html)
