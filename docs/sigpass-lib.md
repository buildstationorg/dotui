# Sigpass library

## Installation

Copy and paste the code in `sigpass.ts` into your project.

## Usage

You can use the functions in `sigpass.ts` as is.

```tsx
import { createSigpassWallet } from '@/lib/sigpass';

// ...

const uniqueHandle = await createSigpassWallet('My Wallet');
```

```tsx
import { getSigpassWallet } from '@/lib/sigpass';

// ...

const wallet = await getSigpassWallet('My Wallet');
```

## Understanding the library

