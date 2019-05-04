# 2 оттенка тайпскриптового энума

Если нужно было ограничить количество значений переменной, я использовал энум:
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

Если явно не задать членам энума значения, Тайпскрипт присвоит первому члену значение `0`. Значение каждого последующего члена будет увеличиваться на один. Если присвоить те же самые значения явно, то будет выглядеть так:
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

Вместо чисел можно задать строки:
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

## Для строкового энума

Константе `today` можно присвоить значение только с помощью энума. Задать значение непосредственно нельзя, Тайпскрипт будет ругаться:
```typescript
const today: DayOfAWeek = DayOfAWeek.Tuesday;
// или
const today: DayOfAWeek = 'Tuesday'; // Type '"Tuesday"' is not assignable to type 'DayOfAWeek'.
```

## Для числового энума

Константе `today` можно присвоить значение с помощью энума, а можно непосредственно задать число из этого промежутка:
```typescript
const today: DayOfAWeek = DayOfAWeek.Tuesday;
// или
const today: DayOfAWeek = 1; // Скомпилируется без ошибок
```

Можно даже задать любое число **не** из промежутка. То есть энум никак не ограничил возможные значения контанты:
```typescript
const today: DayOfAWeek = 42; // Скомпилируется без ошибок
```

## Но почему?

Разработчики Тайпскрипта [говорят](https://github.com/Microsoft/TypeScript/issues/17734), что такое поведение нужно для поддержки [бит-флагов](https://basarat.gitbooks.io/typescript/docs/enums.html#number-enums-as-flags). Бит-флаги позволяют хранить в одной переменной несколько булевых флагов. Это позволяет группировать булевые значения и экономить память:
```typescript
enum AnimalFlags {
  None = 0,
  HasClaws = 1 << 0,
  CanFly = 1 << 1,
  EatsFish = 1 << 2,
  Endangered = 1 << 3
}

const animalFlags: AnimalFlags = AnimalFlags.HasClaws | AnimalFlags.EatsFish; // у животного есть когти, и оно ест рыбу
```

## Как избежать этих сложностей?

Если вы парень простой и вам не нужны бит-флаги, то лучше используйте юнион-типы. Так вы ограничите значение безо всяких сюрпризов:
```typescript
const lightSwitch: 0 | 1 = 8; // Type '8' is not assignable to type '0 | 1'.
// или
const lightSwitch: 'ON' | 'OFF' = 'WEDNESDAY'; // Type '"WEDNESDAY"' is not assignable to type '"ON" | "OFF"'.
```
