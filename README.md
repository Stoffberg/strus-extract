# strus-extract (deprecated)

> **This package has been merged into [`strus-middleware`](https://www.npmjs.com/package/strus-middleware).** Install `strus-middleware` instead.

```bash
npm install strus-middleware
```

The extraction logic is now bundled directly into `strus-middleware`. All types and functions are available from the main export:

```typescript
import { extractMetadata } from "strus-middleware";
import type { FieldSignal, ExtractionConfig } from "strus-middleware";
```

Or from the dedicated subpath:

```typescript
import { extractMetadata } from "strus-middleware/extract";
```

## License

MIT
