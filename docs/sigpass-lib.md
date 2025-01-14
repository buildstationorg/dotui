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

The core of the library are the 2 functions `createOrThrow` and `getOrThrow`.

```ts
/**
 * Use WebAuthn to store authentication-protected arbitrary bytes
 *
 * @param name user-friendly name for the data
 * @param data arbitrary data of 64 bytes or less
 * @returns handle to the data
 */
async function createOrThrow(name: string, data: Uint8Array) {
  try {
    const credential = await navigator.credentials.create({
      publicKey: {
        challenge: new Uint8Array([117, 61, 252, 231, 191, 241]),
        rp: {
          id: location.hostname,
          name: location.hostname,
        },
        user: {
          id: data,
          name: name,
          displayName: name,
        },
        pubKeyCredParams: [
          { type: "public-key", alg: -7 },
          { type: "public-key", alg: -8 },
          { type: "public-key", alg: -257 },
        ],
        authenticatorSelection: {
          authenticatorAttachment: "platform",
          residentKey: "required",
          requireResidentKey: true,
        },
      },
    });
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    return new Uint8Array((credential as any).rawId);
  } catch (error) {
    return null;
  }
}


/**
 * Use WebAuthn to retrieve authentication-protected arbitrary bytes
 *
 * @param id handle to the data
 * @returns data
 */
async function getOrThrow(id: Uint8Array) {
  try {
    const credential = await navigator.credentials.get({
      publicKey: {
        challenge: new Uint8Array([117, 61, 252, 231, 191, 241]),
        allowCredentials: [{ type: "public-key", id }],
      },
    });
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    return new Uint8Array((credential as any).response.userHandle);
  } catch (error) {
    return null;
  }
}
```

For `createOrThrow`, you pass in the `name` and `data` to store. `data` is arbitrary data of 64 bytes or less (there is a hard limit of 64 bytes with WebAuthn).

For `getOrThrow`, you pass in the `id` of the data you want to retrieve.

Other functions are just wrappers around these 2 functions to make it easier to use like `createSigpassWallet` and `getSigpassWallet`.

If you want a drop in component, you can use `SigpassKit` component. Read more about it [here](docs/sigpasskit.md).