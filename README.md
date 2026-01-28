<div align="center">
<img src="https://github.com/suseejs/dependencies/blob/main/susee.svg" width="160" height="160" alt="susee">
<h1>dependencies</h1>
</div>

A static analysis tool designed to examine TypeScript and JavaScript projects and produce dependency graphs,using TypeScript APIs.

## Key Features

- **Dependency Graph Generation:** Analyze TypeScript/JavaScript projects to map file dependencies.
- **Circular Dependency Detection:** Implements depth-first search (DFS) algorithms to identify cycles in the dependency graph, helping developers locate problematic import patterns.
- **Topological Sorting:** Files are ordered according to their dependency relationships, enabling build systems and bundlers to process files in the correct sequence.
- **Leaf & Mutual Dependencies:** Find files with no local imports and mutual (two-way) dependencies.
- **NPM & Node Built-in Detection:** List external and built-in module usage.

## Use

### Installation

Install package as a development dependency in your project:

```bash
npm i -D @suseejs/dependencies
```

### Basic Usage

#### ES Modules

```ts
import dependencies from "@suseejs/dependencies";

const entry = "src/index.ts";
const analysis = await dependencies(entry);

// Get topologically sorted files
const sorted = analysis.sort();

// Check for circular dependencies
const circular = analysis.circular();

// Find mutual dependencies
const mutual = analysis.mutual();
```

#### CommonJS

```js
const dependencies = require("@suseejs/dependencies");

const entry = "src/index.js";
dependencies(entry).then((analysis) => {
  console.log("Circular dependencies:", analysis.circular());
  console.log("NPM dependencies:", analysis.npm());
});
```

## API

**`dependencies(entry:string)`**

### Dependencies Interface Overview

The `Dependencies` interface defines 12 methods that provide access to different aspects of the dependency analysis.

### Method Categories

#### Structural Analysis Methods

These methods provide insights into the structural properties of the dependency graph, including ordering, cycles, and relationship patterns.

| Method          | Return Type                | Purpose                          |
| --------------- | -------------------------- | -------------------------------- |
| `sort()`        | `string[]`                 | Topological sort of dependencies |
| `circular()`    | `CircularDependency[]`     | Detected circular dependencies   |
| `mutual()`      | `string[][]`               | Bidirectional dependency pairs   |
| `leaf()`        | `string[]`                 | Files with no local imports      |
| `chain()`       | `Record<string, string[]>` | Complete dependency chains       |
| `entryToLeaf()` | `string[][]`               | Paths from entry to leaf files   |

#### Query Methods

These methods provide access to the raw dependency data and allow querying specific relationships.

| Method                    | Return Type                | Purpose                         |
| ------------------------- | -------------------------- | ------------------------------- |
| `deps()`                  | `Record<string, string[]>` | Raw dependency graph            |
| `dependents(file:string)` | `string[]`                 | Files that depend on given file |
| `npm()`                   | `string[]`                 | External NPM dependencies       |
| `node()`                  | `string[]`                 | Node.js built-in modules        |

#### Visualization and Metadata Methods

These methods provide formatted output and analysis metadata.

| Method        | Return Type | Purpose                      |
| ------------- | ----------- | ---------------------------- |
| `textGraph()` | `string`    | Text representation of graph |
| `warn()`      | `string[]`  | Analysis warnings            |

## License

[Apache-2.0][file-license] © [Pho Thin Mg][ptm]


[file-license]: LICENSE
[ptm]: https://github.com/phothinmg
