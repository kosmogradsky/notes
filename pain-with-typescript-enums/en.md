# Two shades of Typescript enum

Each time I wanted to rescrict what values a variable can have I used enums:
```typescript
enum DayOfAWeek {
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday
}

const today: DayOfAWeek = DayOfAWeek.Wednesday;
```

If you don't explicitly assign values to members of an enums, Typescript will automatically give the first member a value of `0`. The value of each subsequent member will increase by one. If you assign the same values explicitly, it will look like this:
```typescript
enum DayOfAWeek {
  Monday = 0,
  Tuesday = 1,
  Wednesday = 2,
  Thursday = 3,
  Friday = 4,
  Saturday = 5,
  Sunday = 6
}

const today: DayOfAWeek = DayOfAWeek.Wednesday;
```

You can use strings instead of numbers for values:
```typescript
enum DayOfAWeek {
  Monday = 'Monday',
  Tuesday = 'Tuesday',
  Wednesday = 'Wednesday',
  Thursday = 'Thursday',
  Friday = 'Friday',
  Saturday = 'Saturday',
  Sunday = 'Sunday'
}

const today: DayOfAWeek = DayOfAWeek.Wednesday;
```

And here the Typscript starts showing its two-faced nature.

## For string enum

`today` constant can be assigned a value only using enum references. It is impossible to set the value directly, Typescript will swear at you:
```typescript
const today: DayOfAWeek = DayOfAWeek.Tuesday;
// or
const today: DayOfAWeek = 'Tuesday'; // Error: Type '"Tuesday"' is not assignable to type 'DayOfAWeek'.
```

## For number enum

`today` constant can be assigned a value using enum references or directly:
```typescript
const today: DayOfAWeek = DayOfAWeek.Tuesday;
// or
const today: DayOfAWeek = 1; // Compiles without errors
```

In fact it can be assigned any number value, **not even defined** in enum. So in this case using enum didn't restrict the values a variable can have:
```typescript
const today: DayOfAWeek = 42; // Compiles without errors
```

## But why?

Typescript maintainers [explain](https://github.com/Microsoft/TypeScript/issues/17734), that this behaviour is required to support [bit flags](https://basarat.gitbooks.io/typescript/docs/enums.html#number-enums-as-flags). Bit flags allow to store multiple boolean flags in a single variable which saves some memory:
```typescript
enum AnimalFlags {
  None = 0,
  HasClaws = 1 << 0,
  CanFly = 1 << 1,
  EatsFish = 1 << 2,
  Endangered = 1 << 3
}

const animalFlags: AnimalFlags = AnimalFlags.HasClaws | AnimalFlags.EatsFish; // an animal has claws and it eats fish
```

## How do I avoid this complexity?

If you're a simple man and you don't use bit flags then go for union types. This allows you to reliably restrict variable values without any surprises:
```typescript
const lightSwitch: 0 | 1 = 8; // Error: Type '8' is not assignable to type '0 | 1'.
// or
const lightSwitch: 'ON' | 'OFF' = 'WEDNESDAY'; // Error: Type '"WEDNESDAY"' is not assignable to type '"ON" | "OFF"'.
```
