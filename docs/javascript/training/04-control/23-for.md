# 23-for

***

## Introduction

For loop

## Increasing

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
}
```

Output:

```txt
0
1
2
```

***

## Decreasing

```js
for (let i = 2; i >= 0; i--) {
  console.log(i);
}
```

Output:

```txt
2
1
0
```

***

## Infinite

```js
for (let i = 2; i >= 0; i++) {
  console.log(i);
}
```

Output:

```txt
2
3
4
...
infinite loop -> crash browser!
```

In case of crash (e.g. above code) open a new tab in Chrome hit shift + esc to open task manager identify the process that use the more CPU and kill him, then go back to previous crashed tab and reload page (after having changing the faulty code, obviously)

***

## Break

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
  if (i === 1) {
    break;
  }
}
```

Output:

```txt
0
1
```

***

## Continue

Skip treatment after instruction to next iteration:

```js
for (let i = 0; i < 3; i++) {
  if (i === 1) {
    continue;
  }
  console.log(i);
}
```

Output:

```txt
0
2
```

***
