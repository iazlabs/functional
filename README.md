# FUNCTIONAL INTERFACES

Functional library exposes functional interface function for iterating over utterable, performing function composition, etc...

## Compose

Function composition interface for creating a stack of functions that are apply to a given input.

> Note : At each step, output from previous step is passed as input to the current step.

## Usage

### Create a top -> down stack of functions

```ts
const pipe = compose<number[], number>(
  (x: number[]) => x.map((x) => x * 3),
  (x: number[]) => x.filter((x) => x % 2 === 0),
  (x: number[]) =>
    x.reduce((carr, curr) => {
      carr += curr;
      return carr;
    }),
  (x) => {
    return x;
  }
);
```

### Create a bottom -> up stack of functions and apply it to the provided value

```ts
const pipe = reverseCompose<number[], number>(
  (x) => {
    return x;
  },
  (x: number[]) =>
    x.reduce((carr, curr) => {
      carr += curr;
      return carr;
    }),
  (x: number[]) => x.filter((x) => x % 2 === 0),
  (x: number[]) => x.map((x) => x * 3)
);
```

### Transform each element of the list and reduce it down to a single value

```ts
mapReduce(
  (x: number) => x * 3,
  (carr: number, curr: number) => {
    carr += curr;
    return carr;
  },
  0
)([1, 2, 3, 4, 5, 6, 7, 8, 9, 10] as number[]);
```

### vReverseCompose()

#### Example

> Return an array ["AZANDREW", "KOMI SIDOINE"] from "Azandrew, Komi Sidoine" input

```ts
const pipe = vReverseCompose<string[]>(
  (name: string) => name?.split(', '),
  (name: string) => name?.toUpperCase(),
  (firstname: string, lastname: string) => `${firstname}, ${lastname}`
);
expect(pipe('Azandrew', 'Komi Sidoine')).toEqual(['AZANDREW', 'KOMI SIDOINE']);
expect(vReverseCompose()('Azandrew', 'Komi Sidoine')).toEqual([
  'Azandrew',
  'Komi Sidoine',
]);
```

### vCompose()

#### Example

> Return "AZANDREW, KOMI SIDOINE" from provided input

```ts
const pipe = vCompose<string>(
  (firstname: string, lastname: string) => `${firstname}, ${lastname}`,
  (name: string) => name?.toUpperCase()
);
const result = pipe('Azandrew', 'Komi Sidoine');
expect(result).toEqual('AZANDREW, KOMI SIDOINE');

expect(vCompose()('Azandrew', 'Komi Sidoine')).toEqual([
  'Azandrew',
  'Komi Sidoine',
]);
```

## Transform

Apply a transformation function on a given value and reduce it down to an output.

> Note : using the reducer function.

## Usage

### IdentityFunc()

> Should return the same value provided by the caller

```ts
IdentityFunc({ lat: 3.084853, long: 1.29852 });
```

### MapTo()

> Should return the double of the integer value provided

```ts
MapTo(3, (x) => x * 2);
```

### Tap()

> Should call the provided function with the provided value

```ts
const mailSender = {
  sendMail: function (email: string) {
    if (email) {
      // Send email to remote user
    }
  },
};

const spy = jest.spyOn(mailSender, 'sendMail');

Tap('azandrewdevelopper@example.com', mailSender.sendMail);
```

### Each()

> Should call the provided function on all element of the list

```ts
Each(
  ['azandrewdevelopper@example.com', 'lordfera@example.com'],
  mailSender.sendMail
);
```
