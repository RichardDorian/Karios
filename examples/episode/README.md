# Example: TV Show Episode

File: [`episode.krs`](episode.krs)

This example represents a tv show episode with 3 sections:

- Recap (type `3`)
- TV Show Introduction (type `0`)
- Credits (type `1`)

# JSON representation

```json
{
  "header": {
    "version": 1
  },
  "body": {
    "sectionSize": 3,
    "sections": [
      { "start": 42000, "end": 89000, "type": 3 },
      { "start": 98000, "end": 156000, "type": 0 },
      { "start": 182000, "end": 199000, "type": 1 }
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
      00 03         ... body
----------------
 Section amount
```

We read that the body contains `3` sections.

Let's read the first section

```
 00 00 00 00 00 00 a4 10   00 00 00 00 00 01 5b a8   00 03
------------------------- ------------------------- -------
          Start                      End             Type
```

The first section starts at `42000ms` and ends at `89000ms` and is of type `3` which is a recap.

We containue reading sections until we reach the amount of sections specified at the beginning of the body.
