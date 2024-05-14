# Karios

Open source file format to hold key time codes for media files.

# Abstract

This document specifies the Karios file format. Karios is a file format that holds key time codes for media files. It is designed to be simple, easy to use and implement. It's meant to be used in conjunction with other media files, to provide a way to store and retrieve key time codes for those files (TV show introduction, movie credits, post credits scenes, etc).

# Introduction

The need for a file format such as Karios is obvious. Many streaming platforms use key time codes to provide a better user experience, but every one of them has their own implementation and personal media centers are missing this feature due to the lack of data. In order to unify the implementation of this feature and make easier the implementation on personal media centers Karios was created.

Karios is designed to do one job: hold time codes about a media file.

**Karios** (pronounced /ˈkariɒs/) is based on the word _Kairos_ (a Greek word meaning the right or critical moment). When the name was decided, we misspelled it but decided to keep Karios.

# Format

Karios uses a binary format. The file is divided into two sections:

- Header: Contains the name of the format and its version
- Body: Contains the time codes

> [!NOTE]  
> All integers are stored in big endian format.

## Header

A Karios file always starts with the bytes `0x064b4152494f53` (hex). Then comes the version of the format, a 16 bit unsigned integer (unsigned short). The version is currently `0x0001` (hex). Older versions of the format may be found in the `versions` folder.

| Field     | Size (in bytes) | Type       | Description                     |
| --------- | --------------- | ---------- | ------------------------------- |
| `Magic`   | `7`             | `UByte[7]` | Always `0x064b4152494f53` (hex) |
| `Version` | `2`             | `UShort`   | Version of the format           |

Header size: `9` bytes

## Body

Karios uses a section system. Each time code is a section of the media file. The body is an array of sections. The array of sections is prefixed by the number of sections, a 16 bit unsigned integer (unsigned short). The sections are then written one after the other.

| Field          | Size (in bytes) | Type                      | Description                               |
| -------------- | --------------- | ------------------------- | ----------------------------------------- |
| `SectionsSize` | `2`             | `UShort`                  | Number of sections in the following array |
| `Sections`     | _Varies_        | [`Section`](#section)`[]` | The array of sections                     |

Body size: `2 + SectionsSize * SectionSize` bytes

### Section

A section has 3 properties:

- Start timestamp (in milliseconds)
- End timestamp (in milliseconds)
- Type

The start and end timestamps are 64 bit unsigned integers (unsigned longs) representing the time in milliseconds, `0` being the first millisecond of the media file. The type is a 16 bit unsigned integer (unsigned short) representing the type of the section. More information about what each type means in the [Section Type](#section-type) paragraph.

| Field   | Size (in bytes) | Type     | Description                     |
| ------- | --------------- | -------- | ------------------------------- |
| `Start` | `8`             | `ULong`  | Start timestamp in milliseconds |
| `End`   | `8`             | `ULong`  | End timestamp in milliseconds   |
| `Type`  | `2`             | `UShort` | Section type                    |

Section size: `18` bytes

### Section Type

A section type is the nature of the section. It can be anything ranging from a TV show introduction, credits or post credits scenes. For convenience, Karios reserves the values from `0` to `256`. If the value is greater than `256` it is considered a custom type.

Here are the reserved types:

| Code | Type                 | Description                            |
| ---- | -------------------- | -------------------------------------- |
| `0`  | TV Show Introduction | The introduction of a TV show          |
| `1`  | Credits              | The credits of a movie/episode         |
| `2`  | Post Credits Scene   | A scene that happens after the credits |
| `3`  | Recap                | A recap of the previous episode        |
| `4`  | Advertisement        | An advertisement                       |

# Examples

A few examples can be found in the [`examples`](examples/) folder. Each subfolder contains a Karios file, a Markdown file explaining the file and a straightforward JSON representation of the Karios file.

# Notes

Why a binary format? A binary format is more efficient than a text format. It is smaller and faster to read and write. It is also easier to implement in different programming languages. We considered using a text format but decided against it as a Karios file, once created is not meant to be edited unless the content changes which is rare. We also thought that since the file format is simple, it wouldn't take much time to create a tool to make file creation and editing easy.

# Copyright

See `LICENSE` file
