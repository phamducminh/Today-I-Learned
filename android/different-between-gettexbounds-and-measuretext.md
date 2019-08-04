# Different between getTextBounds and measureText

In short,

1. `getTextBounds` is to get the RECT of the exact text. The `measureText` is the length of the text, including the extra gap on the left and right.

2. If there are spaces between the text, it is measured in `measureText` but not including in the length of the TextBounds, although the coordinate get shifted.

3. The text could be tilted (Skew) left. In this case, the bounding box left side would exceed outside the measurement of the measureText, and the overall length of the text bound would be bigger than `measureText`

4. The text could be tilted (Skew) right. In this case, the bounding box right side would exceed outside the measurement of the measureText, and the overall length of the text bound would be bigger than `measureText`

---

## Inspiration

Inspired by [Elye](https://stackoverflow.com/users/3286489/elye) and his answer about [Different between getTextBounds and measureText](https://stackoverflow.com/questions/7549182/android-paint-measuretext-vs-gettextbounds/57288746#57288746).