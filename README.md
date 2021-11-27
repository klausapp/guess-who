# Guess who!?

Powered by [Vite](https://vitejs.dev), built by and for [Klausians](https://klausapp.com).

<p align="center">
  <img src="https://guesswho.internal.klausapp.com/preview.png">
</p>

## Development

```
yarn dev
```

## Updating

To add a new employee's image to the game, add them as the last index to `public/klausmojis/` and add their name
to `src/answers.ts` - note that the name's index in `answers.ts` must match the image's number,
if increased by one (this is because while the answers array is 0-indexed, the image collection is 1-indexed).
