# Example: Movie

File: [`movie.krs`](movie.krs)

This example represents a movie with 2 sections:

- Credits (type `1`)
- Post Credits Scene (type `2`)

# JSON representation

```json
{
  "header": {
    "version": 1
  },
  "body": {
    "sectionSize": 2,
    "sections": [
      { "start": 6000, "end": 89000, "type": 1 },
      { "start": 90000, "end": 99000, "type": 2 }
    ]
  }
}
```

# Explanation

Here's a breakdown of the binary file. The following is in hexadecimal notation.

## Header

```
 06 4b 41 52 49 4f 53    00 01      ... body
---------------------- ---------
     Magic number       Version
```

We confirm that the file is a Karios file and that the version is `1`.

## Body

```
      00 02         ... body
----------------
 Section amount
```

We read that the body contains `2` sections.

Let's read the first section

```
 00 00 00 00 00 00 17 70   00 00 00 00 00 01 5b A8   00 01
------------------------- ------------------------- -------
          Start                      End             Type
```

The first section starts at `6000ms` and ends at `89000ms` and is of type `1` which is a credits section.

We containue reading sections until we reach the amount of sections specified at the beginning of the body.
