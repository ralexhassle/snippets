# Some useful snippets...

## Contrasting text

To generate a contrasting text, we are using the following formula defined by the World Wide Web Consortium (W3C) to ensure that foreground and background color combinations provide sufficient contrast when viewed by someone having color deficits or when viewed on a black-and-white screen:

```ts
interface RGB {
    b: number;
    g: number;
    r: number;
}
function rgbToYIQ({ r, g, b }: RGB): number {
    return ((r * 299) + (g * 587) + (b * 114)) / 1000;
}
function hexToRgb(hex: string): RGB | undefined {
    if (!hex || hex === undefined || hex === '') {
        return undefined;
    }

    const result: RegExpExecArray | null =
          /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);

    return result ? {
        r: parseInt(result[1], 16),
        g: parseInt(result[2], 16),
        b: parseInt(result[3], 16)
    } : undefined;
}
export function contrast(colorHex: string | undefined,
                         threshold: number = 128): string {
    if (colorHex === undefined) {
        return '#000';
    }

    const rgb: RGB | undefined = hexToRgb(colorHex);

    if (rgb === undefined) {
        return '#000';
    }

    return rgbToYIQ(rgb) >= threshold ? '#000' : '#fff';
}
```

## Splitting arrays into chunks
This is a function for splitting arrays into chunks

```ts
export class ArrayUtils {
  static chunk<T>(array: T[], chunkSize: number): T[][] {
    const chunks: T[][] = [];

    let index = 0;
    while (index < array.length) {
      chunks.push(array.slice(index, index + chunkSize));
      index += chunkSize;
    }
    return chunks;
  }
}
```

## Reading a file, formating its content

```bash
while read line
do
   my_var="$my_var, '$line'"
done < list

my_var="${my_var#, }"                # Trim off leading comma and space

echo "SELECT id, email, last_name, first_name, bic, iban FROM users WHERE id IN($my_var) ORDER BY FIELD(id,$my_var);" | pbcopy
```
