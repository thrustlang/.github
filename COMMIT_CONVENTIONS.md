<img src= "https://github.com/thrustlang/.github/blob/main/assets/logos/thrustlang-logo-name.png" alt= "logo" style= "width: 80%; height: 80%;"></img>

# The Thrust Assets Center

<img src="https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt="standard-separator" style="width: 1hv;">

There is a simple guide of standard conventions to follow in order to delivery a good Github commit for the Thrust Assets Center.

### Title

It needs to be detailed. It can be include a lot of technical slang. The base of a well designed Github commit title always will be and needs a specific syntax as:

#### Title - features

Following the syntax:

`feat(...)`

Valid locations:

- `logos` Any location that usually involucrates the logos of the project: PNG & SVG exports, GIMP source files (`.xcf`), banners and the `new logo` folder.
- `games` Any location that usually involucrates game related assets (Example: Minecraft skins).
- `profile` Any location that usually involucrates the GitHub Profile README (`profile/README.md`).
- `project` Any location that usually involucrates the repository itself: the root `README.md`, `CITATION.cff`, `LICENSE.txt`, Github actions or the conception of a new part of the assets center.

Example:

`feat(logos)` Adding a new SVG variant of the Thrust logo.

#### Title - fixes

Following the syntax:

`fix(...)`

Valid locations:

- `logos` Any location that usually involucrates the logos of the project: PNG & SVG exports, GIMP source files (`.xcf`), banners and the `new logo` folder.
- `games` Any location that usually involucrates game related assets (Example: Minecraft skins).
- `profile` Any location that usually involucrates the GitHub Profile README (`profile/README.md`).
- `project` Any location that usually involucrates the repository itself: the root `README.md`, `CITATION.cff`, `LICENSE.txt`, Github actions or the conception of a new part of the assets center.

Any consecutive location written to the next one needs to be follow for a COMMA character `,`.

Example:

`fix(profile)` Fixing a typo on the GitHub Profile README.

#### Title - Combinatory

In order to create a well disigned combinatory title, you need to use the following syntax:

`(feat(...), fix(...))`

- It needs to be encapsulated for a pair characters PAREN `()`.
- Each next feature or fix needs to be followed for a COMMA character `,`.

### Description

It needs to be concise, short, but detailed in the same time. It can be include a lot of technical slang.
