# Pasmo

Emscripten conversion for Pasmo, a multi-platform Z80 cross-assembler.
It can be used to compile assembly language programs for the ZX Spectrum.

## Usage

```bash
npm install pasmo
```

```javascript
import pasmo from "pasmo";

const asm = `
    org 30000

tv_flag    equ 5C3Ch

start
    ; Directs rst 10h output to main screen.
    xor a
    ld (tv_flag),a

    ld b, 50

another

    push bc

    ld hl,hello
again    ld a,(hl)
    cp 0
    jr z, exit
    push hl
    rst 10h
    pop hl
    inc hl
    jr again

exit
    pop bc
    djnz another
    ret

hello    db "Hello, world.", 0Dh, 0

    end start`;

const tap = pasmo(asm);
```

## Build library

```bash
npm install
npm run build
npm run test
```

## CLI command notes

### Usage

```bash
pasmo [options] file.asm file.bin [file.symbol [file.publics]]
```

#### Options

```text
	-d         -->	Show debug info during assembly.

	-1         -->	Show debug info during assembly,
			also in first pass.

	-v         -->	Verbose. Show progress information
			during assembly.

	-I         -->	Add directory to the list for
			searching files in INCLUDE and INCBIN.

	--bin      -->	Generate the object file in pure binary
			format without headers.

	--hex      -->	Generate the object file in Intel HEX
			format.

	--prl      -->	Generate the object file in the PRL
			format. Useful for CP/M Plus RSX.

	--cmd      -->	Generate the object file in CP/M 86
			CMD format.

	--plus3dos -->	Generate the object file with PLUS3DOS
			header (Spectrum disk).

	--tap      -->	Generate a .tap file for Spectrum
			emulators (tape image).

	--tzx      -->	Generate a .tzx file for Spectrum
			emulators (tape image).

	--cdt      -->	Generate a .cdt file for Spectrum
			emulators (tape image).

	--tapbas   -->	Same as --tap option but adding a
			Basic loader.

	--tzxbas   -->	Same as --tzx option but adding a
			Basic loader.

	--cdtbas   -->	Same as --cdt option but adding a
			Basic loader.

	--amsdos   -->	Generate the object file with Amsdos
			header (Amstrad CPC disk).

	--msx      -->	Generate the object file with header
			for use with BLOAD in MSX Basic.

	--public   -->	The symbol table listing will include
			only symbols declared as PUBLIC.

	--name     -->	Name for the header in the formats that
			use it. If unspecified the object file
			name will be used.

	--err      -->	Direct error messages to standard ouptut
			instead of error output (except for errors
			in options).

	--nocase   -->	Make identifiers case insensitive.

	--alocal   -->	Autolocal mode: the labels than begins with
			a '_' are locals, and his ambit finishes in
			the next no local label or in the next PROC,
			LOCAL or MACRO directive.

	-B
	--bracket  -->	Bracket only mode: parenthesis are reserved
			for expressions.

	-E
	--equ	   -->	Predefine a label.

	-8
	-w8080     -->	Show warnings when using Z80 instructions
			that does not exist in 8080.

	--86       -->	Generate 8086 code.
```

If no code generation options are specified --bin is assumed.

Debug info goes to standard output, error messages to error output.
