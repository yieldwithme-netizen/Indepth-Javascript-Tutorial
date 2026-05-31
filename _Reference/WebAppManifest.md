# Web App Manifest

## Definition

Web app manifest **defines PWA properties**.

## Example

```json
{
    "name": "My App",
    "short_name": "App",
    "start_url": "/",
    "display": "standalone",
    "background_color": "#fff",
    "theme_color": "#000",
    "icons": [
        {
            "src": "/icon.png",
            "sizes": "192x192",
            "type": "image/png"
        }
    ]
}
```

## Quick Revision

- Manifest = PWA configuration
- Defines name, icons, theme
- Linked in HTML: `<link rel="manifest" href="manifest.json">`
- Required for PWA

---

## Related Topics

- [[What-is-WebAppManifest]] - [[What-is-WebAppManifest|Web app manifest]]
- [[WebAppManifest]] - [[WebAppManifest|Web app manifest]]
- [[What-is-ServiceWorkers]] - [[What-is-ServiceWorkers|Service workers]]
- [[OfflineSupport]] - [[OfflineSupport|Offline support]]
