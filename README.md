<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/tonkite/tonkite/main/assets/logo-dark.svg">
    <img alt="tonkite logo" src="https://raw.githubusercontent.com/tonkite/tonkite/main/assets/logo-light.svg" width="384" height="auto">
  </picture>
</p>

<p align="center">
  <a href="https://ton.org"><img alt="Based on TON" src="https://img.shields.io/badge/Based%20on-TON-blue"></a>
  <a href="https://github.com/tonkite/tonkite"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/tonkite/tonkite"></a>
  <a href="https://t.me/tonkite"><img alt="Telegram Channel" src="https://img.shields.io/badge/Telegram%20-@tonkite-24A1DE"></a>
  <a href="https://opensource.org/licenses/MIT"><img alt="License" src="https://img.shields.io/badge/License-MIT-green.svg"></a>
</p>

---

# TL-B Parser

## Installation

```bash
pnpm add @tonkite/tlb-parser
```

## Usage

Create a file with TLB scheme according to the [documentation](https://docs.ton.org/develop/data-formats/tl-b-language). This is an example of such a file (call it `example.tlb`):

```
t$_ x:# y:(uint 5) = A;
```

Then do:

```bash
npx tlb-parser example.tlb
```

Or you can use the tool from inside JS or TS code.

```typescript
import { parse, NodeVisitor, ASTRootBase } from '@tonkite/tlb-parser';

class TestVisitor extends NodeVisitor {
  public visited: { [key: string]: number };

  constructor() {
    super();
    this.visited = {};
  }

  override genericVisit(node: nodes.ASTRootBase): void {
    if (this.visited[node.constructor.name] === undefined) {
      this.visited[node.constructor.name] = 0;
    }

    this.visited[node.constructor.name] += 1;
    return super.genericVisit(node);
  }
}

const schema = `
t$_ x:# y:(uint 5) = A;
`;

const tree = parse(schema);
const visitor = new TestVisitor();
visitor.visit(tree);

console.log(util.inspect(visitor.visited, { showHidden: false, depth: null, colors: true }));

console.log(util.inspect(tree, { showHidden: false, depth: null, colors: true }));
```

## Related

- IntelliJ plugin: https://github.com/ton-blockchain/intellij-ton

## License

<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"></a>
