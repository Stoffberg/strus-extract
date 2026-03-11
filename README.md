# strus-extract

Response metadata extraction for [Strus](https://strus.dev). Walks JSON response bodies and produces structural signals (null rates, enum distributions, array cardinalities) while automatically excluding PHI/PII fields.

This package runs inside your process. No response data leaves your server. Only the extracted signals are sent to Strus.

## Install

```bash
npm install strus-extract
```

## Usage

```typescript
import { extractMetadata } from "strus-extract";

const result = extractMetadata(
  {
    status: "approved",
    items: [{ id: "1", name: "Alice" }],
    ssn: "123-45-6789", // automatically excluded
  },
  200
);

console.log(result.signals);
// [
//   { fieldPath: "status", signalType: "null_rate", value: { type: "null_rate", isNull: false } },
//   { fieldPath: "status", signalType: "enum_distribution", value: { type: "enum_distribution", value: "approved" } },
//   { fieldPath: "items", signalType: "array_cardinality", value: { type: "array_cardinality", length: 1 } },
//   ...
// ]
// Note: "ssn" is not present in the output
```

## PHI/PII Filtering

The default configuration excludes fields matching common PHI/PII patterns: SSN, date of birth, names, addresses, phone numbers, emails, medical record numbers, insurance IDs, and more. See [`src/patterns.ts`](./src/patterns.ts) for the full list.

You can add your own exclusions:

```typescript
const result = extractMetadata(responseBody, 200, {
  excludePaths: ["internal.debugInfo"],
  excludePatterns: [/\bsecret\b/i],
});
```

## Signal Types

| Signal | Description |
|---|---|
| `null_rate` | Whether a field is null or undefined |
| `enum_distribution` | Short string values likely to be enums |
| `array_cardinality` | Length of array fields |
| `new_value` | String values that may represent new enum variants |

## Configuration

| Option | Default | Description |
|---|---|---|
| `excludePaths` | `[]` | Exact field paths to skip |
| `excludePatterns` | PHI/PII patterns | Regex patterns to match against field names |
| `maxDepth` | `10` | Maximum object nesting depth |
| `maxArraySample` | `5` | Number of array elements to sample |
| `maxEnumCardinality` | `50` | Threshold for enum detection |

## License

MIT
