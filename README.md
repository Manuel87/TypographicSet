# CSS--Typographic-Set

**A proposal for multi-font typographic sets**
- This is a raw proposal on how typographic sets could be implemented in CSS
- eg. allowing to mix and match different fonts for different scripts (or inline typographic hierachies)
- avoiding overly complicated html- and css-markup


----------------------------------
#### Usecase
- live example: https://shop.typeproject.com/en/products/fitfont/


----------------------------------
#### Goal
- full control to match different fonts
  - basic control: scale, baseline shift, (weight)
   - advanced control: weight, contrast and vertical metrics (with variable fonts)
- no need to wrap strings of different scripts in separate spans
- ...
---------------------------------------------------------------------
#### Examples

----------------------------------
**#1 Example (simple)**

@Font-Face-Setup
- specify the fonts, their metrics and package them under one family-name
- scales are in 1000/UPM, a typical type design measurement (1000upm = 1em)

```css
@font-face {
	font-family: 			"MyTypographicSet"
	font-style: 			normal;
	font-weight-default: 	400;

	[Default, Arabic] {  /* Defining the base (primary script/font) */
		src: url('Nassim.ttf')[...];
		font-scale: 		1000; /* default scale */
		font-baseline: 		250;
		font-weight: 		400;
	}

	[Latin] {  /* all Latin characters get formated in Minion */
		src: url('Minion.ttf')[...];
		font-scale:  		920; /* scaled down to 92% matching the arabic */
		font-baseline: 		250;
		font-weight: 		500;
	}

}
```

Use - Straightforward use, just like a single font
```css
body {
	font-family: "MyTypographicSet", "Fallback", "serif";
	font-weight: 400;
}

strong, b { font-weight: 700; } /* strong emphasis */

em, i { font-style: italic; }
```

---------------------------------------------------------------------
---------------------------------------------------------------------



**#2 Example (complex)**

@Font-Face-Setup
Latin, Arabic, Chinese, Italics, Advanced Matching

```css
@font-face {
	font-family: 				"MyTypographicSet"
	font-style: 				normal;
	font-weight-default: 			400;
	script-order: 				"Latin, Arabic, Chinese, Japanese"

	[Default, Latin] {
		src: url('Minion.ttf')[...];
		font-scale[x]:  		500; //defining the scale by the x-height;
		font-[A–Z,0–9,.case]-size: 	700;
		font-[.smcp]-size: 			570;
		font-baseline: 			250;
		font-weight-thick: 		80;
		font-weight-thin: 		20;
	}

	[Arabic] {
		src: url('Nassim.ttf')[...];
		font-scale[–]: 			85;
		font-baseline: 			250;
		font-weight-thick: 		85;
		font-weight-thin: 		20;
	}

	[CJK] {
		src: url('TP-Mincho.ttf')[...];
		font-scale[cjk-base-char]: 900;
		font-baseline: 			500;
		font-weight-thick: 		78;
		font-weight-thin: 		20;
	}
}

// and setup for soft emphasis (italic)

@font-face {
	font-family: 			"MyTypographicSet"
	font-style: 			italic;
	font-weight-default: 		400;
	script-order: 			"Latin, Arabic, Chinese, Japanese"

	[Default, Latin] {
			src: url('Minion-Italic.ttf')[...];
			font-scale[x]:  		500; /* font-scale: 	1000 (= no change); */
			font-[A–Z,0–9,.case]-size: 	700;
			font-[.smcp]-size: 			570;
			font-baseline: 			250;
			font-weight-thick: 		75,
			font-weight-thin: 		20;
	}

	[Arabic] {
		src: url('Nassim-Italic.ttf')[...];
		font-scale[–]: 			85;
		font-baseline: 			250;
		font-weight-thick: 		80;
		font-weight-thin: 		20;
	}

	[Chinese, Japanese] {
		src: url('TP-Mincho-Italic.ttf')[...];
		font-scale[cjk-base-char]: 900;
		font-baseline: 			500;
		font-weight-thick: 		70;
		font-weight-thin: 		18;
	}
}
```

**Use - applying the new "Family"**
```css
body {
	font-family: "MyTypographicSet", "Fallback", "serif";
	font-weight: 400;
}

strong, b {
	font-weight: 700;
}

em, i {
	font-style: italic;
}

```

PS: All fonts in the complex example are variable fonts, containing a weight- and a contrast-axis; and the Latin additionally has a separate height-axis.



---------------------------------------------------------------------
---------------------------------------------------------------------

Disclaimer: The used fonts and all values are just rough examples and are not typographically tested at the time I’ve written this
