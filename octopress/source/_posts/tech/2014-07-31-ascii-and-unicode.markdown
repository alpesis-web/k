---
layout: post_tech
title:  "ASCII and Unicode"
date:   2014-07-31 22:20:00 +0800
comments: true
categories: [tech]
tags: [unicode, ascii]
---

## ASCII

ASCII: American Standard Code for Information Interchange (1960s)

### Bits

- 128 characters: upper-case, lower-case, sepecial characters
- binary digits/ bits: 0/1 or on/off
- 8 bits = 1 byte: representing 1 character (e.g., L: 01001100)

### Bytes

| Bytes                                   | Unit        |
|:----------------------------------------|:------------|
| 8 bits                                  | 1 byte      |
| 1,024,000 bytes                         | 1 megabyte  |
| one billion bytes                       | 1 gigabyte  |
| one trillion bytes                      | 1 terabyte  |
| 8 quadrillion bytes ( 15 zeros)         | 1 petabyte  |
| 1,208,925,819,614,629,174,706,176 bytes | 1 yottabyte |


| Storage              | Bytes          | Example                  |
|:---------------------|:---------------|:-------------------------|
| 3.5 inch floppy disk | 1.44 metabytes | 750 pages                |
| compact disc         | 700 metabytes  | an hour's worth of songs |
| a single-side DVD    | 4.7 gigabytes  | a regular old movie      |


### Table of ASCII Characters

    Char  Dec  Oct  Hex | Char  Dec  Oct  Hex | Char  Dec  Oct  Hex | Char Dec  Oct   Hex
    -------------------------------------------------------------------------------------
    (nul)   0 0000 0x00 | (sp)   32 0040 0x20 | @      64 0100 0x40 | `      96 0140 0x60
    (soh)   1 0001 0x01 | !      33 0041 0x21 | A      65 0101 0x41 | a      97 0141 0x61
    (stx)   2 0002 0x02 | "      34 0042 0x22 | B      66 0102 0x42 | b      98 0142 0x62
    (etx)   3 0003 0x03 | #      35 0043 0x23 | C      67 0103 0x43 | c      99 0143 0x63
    (eot)   4 0004 0x04 | $      36 0044 0x24 | D      68 0104 0x44 | d     100 0144 0x64
    (enq)   5 0005 0x05 | %      37 0045 0x25 | E      69 0105 0x45 | e     101 0145 0x65
    (ack)   6 0006 0x06 | &      38 0046 0x26 | F      70 0106 0x46 | f     102 0146 0x66
    (bel)   7 0007 0x07 | '      39 0047 0x27 | G      71 0107 0x47 | g     103 0147 0x67
    (bs)    8 0010 0x08 | (      40 0050 0x28 | H      72 0110 0x48 | h     104 0150 0x68
    (ht)    9 0011 0x09 | )      41 0051 0x29 | I      73 0111 0x49 | i     105 0151 0x69
    (nl)   10 0012 0x0a | *      42 0052 0x2a | J      74 0112 0x4a | j     106 0152 0x6a
    (vt)   11 0013 0x0b | +      43 0053 0x2b | K      75 0113 0x4b | k     107 0153 0x6b
    (np)   12 0014 0x0c | ,      44 0054 0x2c | L      76 0114 0x4c | l     108 0154 0x6c
    (cr)   13 0015 0x0d | -      45 0055 0x2d | M      77 0115 0x4d | m     109 0155 0x6d
    (so)   14 0016 0x0e | .      46 0056 0x2e | N      78 0116 0x4e | n     110 0156 0x6e
    (si)   15 0017 0x0f | /      47 0057 0x2f | O      79 0117 0x4f | o     111 0157 0x6f
    (dle)  16 0020 0x10 | 0      48 0060 0x30 | P      80 0120 0x50 | p     112 0160 0x70
    (dc1)  17 0021 0x11 | 1      49 0061 0x31 | Q      81 0121 0x51 | q     113 0161 0x71
    (dc2)  18 0022 0x12 | 2      50 0062 0x32 | R      82 0122 0x52 | r     114 0162 0x72
    (dc3)  19 0023 0x13 | 3      51 0063 0x33 | S      83 0123 0x53 | s     115 0163 0x73
    (dc4)  20 0024 0x14 | 4      52 0064 0x34 | T      84 0124 0x54 | t     116 0164 0x74
    (nak)  21 0025 0x15 | 5      53 0065 0x35 | U      85 0125 0x55 | u     117 0165 0x75
    (syn)  22 0026 0x16 | 6      54 0066 0x36 | V      86 0126 0x56 | v     118 0166 0x76
    (etb)  23 0027 0x17 | 7      55 0067 0x37 | W      87 0127 0x57 | w     119 0167 0x77
    (can)  24 0030 0x18 | 8      56 0070 0x38 | X      88 0130 0x58 | x     120 0170 0x78
    (em)   25 0031 0x19 | 9      57 0071 0x39 | Y      89 0131 0x59 | y     121 0171 0x79
    (sub)  26 0032 0x1a | :      58 0072 0x3a | Z      90 0132 0x5a | z     122 0172 0x7a
    (esc)  27 0033 0x1b | ;      59 0073 0x3b | [      91 0133 0x5b | {     123 0173 0x7b
    (fs)   28 0034 0x1c | <      60 0074 0x3c | \      92 0134 0x5c | |     124 0174 0x7c
    (gs)   29 0035 0x1d | =      61 0075 0x3d | ]      93 0135 0x5d | }     125 0175 0x7d
    (rs)   30 0036 0x1e | >      62 0076 0x3e | ^      94 0136 0x5e | ~     126 0176 0x7e
    (us)   31 0037 0x1f | ?      63 0077 0x3f | _      95 0137 0x5f | (del) 127 0177 0x7f


### ASCII control characters (character code 0-31)

The first 32 characters in the ASCII-table are unprintable control codes and are used to control peripherals such as printers.

    DEC	OCT	HEX	BIN	Symbol	HTML Number	HTML Name	Description
    0	000	00	00000000	NUL	&#000;	 	Null char
    1	001	01	00000001	SOH	&#001;	 	Start of Heading
    2	002	02	00000010	STX	&#002;	 	Start of Text
    3	003	03	00000011	ETX	&#003;	 	End of Text
    4	004	04	00000100	EOT	&#004;	 	End of Transmission
    5	005	05	00000101	ENQ	&#005;	 	Enquiry
    6	006	06	00000110	ACK	&#006;	 	Acknowledgment
    7	007	07	00000111	BEL	&#007;	 	Bell
    8	010	08	00001000	BS	&#008;	 	Back Space
    9	011	09	00001001	HT	&#009;	 	Horizontal Tab
    10	012	0A	00001010	LF	&#010;	 	Line Feed
    11	013	0B	00001011	VT	&#011;	 	Vertical Tab
    12	014	0C	00001100	FF	&#012;	 	Form Feed
    13	015	0D	00001101	CR	&#013;	 	Carriage Return
    14	016	0E	00001110	SO	&#014;	 	Shift Out / X-On
    15	017	0F	00001111	SI	&#015;	 	Shift In / X-Off
    16	020	10	00010000	DLE	&#016;	 	Data Line Escape
    17	021	11	00010001	DC1	&#017;	 	Device Control 1 (oft. XON)
    18	022	12	00010010	DC2	&#018;	 	Device Control 2
    19	023	13	00010011	DC3	&#019;	 	Device Control 3 (oft. XOFF)
    20	024	14	00010100	DC4	&#020;	 	Device Control 4
    21	025	15	00010101	NAK	&#021;	 	Negative Acknowledgement
    22	026	16	00010110	SYN	&#022;	 	Synchronous Idle
    23	027	17	00010111	ETB	&#023;	 	End of Transmit Block
    24	030	18	00011000	CAN	&#024;	 	Cancel
    25	031	19	00011001	EM	&#025;	 	End of Medium
    26	032	1A	00011010	SUB	&#026;	 	Substitute
    27	033	1B	00011011	ESC	&#027;	 	Escape
    28	034	1C	00011100	FS	&#028;	 	File Separator
    29	035	1D	00011101	GS	&#029;	 	Group Separator
    30	036	1E	00011110	RS	&#030;	 	Record Separator
    31	037	1F	00011111	US	&#031;	 	Unit Separator


### ASCII printable characters (character code 32-127)

Codes 32-127 are common for all the different variations of the ASCII table,
they are called printable characters, represent letters, digits, punctuation
marks, and a few miscellaneous symbols. You will find almost every character on
your keyboard. Character 127 represents the command DEL.


    DEC	OCT	HEX	BIN	Symbol	HTML Number	HTML Name	Description
    32	040	20	00100000	 	&#32;	 	Space
    33	041	21	00100001	!	&#33;	 	Exclamation mark
    34	042	22	00100010	"	&#34;	&quot;	Double quotes (or speech marks)
    35	043	23	00100011	#	&#35;	 	Number
    36	044	24	00100100	$	&#36;	 	Dollar
    37	045	25	00100101	%	&#37;	 	Procenttecken
    38	046	26	00100110	&	&#38;	&amp;	Ampersand
    39	047	27	00100111	'	&#39;	 	Single quote
    40	050	28	00101000	(	&#40;	 	Open parenthesis (or open bracket)
    41	051	29	00101001	)	&#41;	 	Close parenthesis (or close bracket)
    42	052	2A	00101010	*	&#42;	 	Asterisk
    43	053	2B	00101011	+	&#43;	 	Plus
    44	054	2C	00101100	,	&#44;	 	Comma
    45	055	2D	00101101	-	&#45;	 	Hyphen
    46	056	2E	00101110	.	&#46;	 	Period, dot or full stop
    47	057	2F	00101111	/	&#47;	 	Slash or divide
    48	060	30	00110000	0	&#48;	 	Zero
    49	061	31	00110001	1	&#49;	 	One
    50	062	32	00110010	2	&#50;	 	Two
    51	063	33	00110011	3	&#51;	 	Three
    52	064	34	00110100	4	&#52;	 	Four
    53	065	35	00110101	5	&#53;	 	Five
    54	066	36	00110110	6	&#54;	 	Six
    55	067	37	00110111	7	&#55;	 	Seven
    56	070	38	00111000	8	&#56;	 	Eight
    57	071	39	00111001	9	&#57;	 	Nine
    58	072	3A	00111010	:	&#58;	 	Colon
    59	073	3B	00111011	;	&#59;	 	Semicolon
    60	074	3C	00111100	<	&#60;	&lt;	Less than (or open angled bracket)
    61	075	3D	00111101	=	&#61;	 	Equals
    62	076	3E	00111110	>	&#62;	&gt;	Greater than (or close angled bracket)
    63	077	3F	00111111	?	&#63;	 	Question mark
    64	100	40	01000000	@	&#64;	 	At symbol
    65	101	41	01000001	A	&#65;	 	Uppercase A
    66	102	42	01000010	B	&#66;	 	Uppercase B
    67	103	43	01000011	C	&#67;	 	Uppercase C
    68	104	44	01000100	D	&#68;	 	Uppercase D
    69	105	45	01000101	E	&#69;	 	Uppercase E
    70	106	46	01000110	F	&#70;	 	Uppercase F
    71	107	47	01000111	G	&#71;	 	Uppercase G
    72	110	48	01001000	H	&#72;	 	Uppercase H
    73	111	49	01001001	I	&#73;	 	Uppercase I
    74	112	4A	01001010	J	&#74;	 	Uppercase J
    75	113	4B	01001011	K	&#75;	 	Uppercase K
    76	114	4C	01001100	L	&#76;	 	Uppercase L
    77	115	4D	01001101	M	&#77;	 	Uppercase M
    78	116	4E	01001110	N	&#78;	 	Uppercase N
    79	117	4F	01001111	O	&#79;	 	Uppercase O
    80	120	50	01010000	P	&#80;	 	Uppercase P
    81	121	51	01010001	Q	&#81;	 	Uppercase Q
    82	122	52	01010010	R	&#82;	 	Uppercase R
    83	123	53	01010011	S	&#83;	 	Uppercase S
    84	124	54	01010100	T	&#84;	 	Uppercase T
    85	125	55	01010101	U	&#85;	 	Uppercase U
    86	126	56	01010110	V	&#86;	 	Uppercase V
    87	127	57	01010111	W	&#87;	 	Uppercase W
    88	130	58	01011000	X	&#88;	 	Uppercase X
    89	131	59	01011001	Y	&#89;	 	Uppercase Y
    90	132	5A	01011010	Z	&#90;	 	Uppercase Z
    91	133	5B	01011011	[	&#91;	 	Opening bracket
    92	134	5C	01011100	\	&#92;	 	Backslash
    93	135	5D	01011101	]	&#93;	 	Closing bracket
    94	136	5E	01011110	^	&#94;	 	Caret - circumflex
    95	137	5F	01011111	_	&#95;	 	Underscore
    96	140	60	01100000	`	&#96;	 	Grave accent
    97	141	61	01100001	a	&#97;	 	Lowercase a
    98	142	62	01100010	b	&#98;	 	Lowercase b
    99	143	63	01100011	c	&#99;	 	Lowercase c
    100	144	64	01100100	d	&#100;	 	Lowercase d
    101	145	65	01100101	e	&#101;	 	Lowercase e
    102	146	66	01100110	f	&#102;	 	Lowercase f
    103	147	67	01100111	g	&#103;	 	Lowercase g
    104	150	68	01101000	h	&#104;	 	Lowercase h
    105	151	69	01101001	i	&#105;	 	Lowercase i
    106	152	6A	01101010	j	&#106;	 	Lowercase j
    107	153	6B	01101011	k	&#107;	 	Lowercase k
    108	154	6C	01101100	l	&#108;	 	Lowercase l
    109	155	6D	01101101	m	&#109;	 	Lowercase m
    110	156	6E	01101110	n	&#110;	 	Lowercase n
    111	157	6F	01101111	o	&#111;	 	Lowercase o
    112	160	70	01110000	p	&#112;	 	Lowercase p
    113	161	71	01110001	q	&#113;	 	Lowercase q
    114	162	72	01110010	r	&#114;	 	Lowercase r
    115	163	73	01110011	s	&#115;	 	Lowercase s
    116	164	74	01110100	t	&#116;	 	Lowercase t
    117	165	75	01110101	u	&#117;	 	Lowercase u
    118	166	76	01110110	v	&#118;	 	Lowercase v
    119	167	77	01110111	w	&#119;	 	Lowercase w
    120	170	78	01111000	x	&#120;	 	Lowercase x
    121	171	79	01111001	y	&#121;	 	Lowercase y
    122	172	7A	01111010	z	&#122;	 	Lowercase z
    123	173	7B	01111011	{	&#123;	 	Opening brace
    124	174	7C	01111100	|	&#124;	 	Vertical bar
    125	175	7D	01111101	}	&#125;	 	Closing brace
    126	176	7E	01111110	~	&#126;	 	Equivalency sign - tilde
    127	177	7F	01111111		&#127;	 	Delete


### The extended ASCII codes (character code 128-255)

There are several different variations of the 8-bit ASCII table. The table
below is according to ISO 8859-1, also called ISO Latin-1. Codes 129-159
contain the Microsoft Windows Latin-1 extended characters.


    DEC	OCT	HEX	BIN	Symbol	HTML Number	HTML Name	Description
    128	200	80	10000000	€	&#128;	&euro;	Euro sign
    129	201	81	10000001	 	 	 	 
    130	202	82	10000010	?	&#130;	&sbquo;	Single low-9 quotation mark
    131	203	83	10000011	?	&#131;	&fnof;	Latin small letter f with hook
    132	204	84	10000100	?	&#132;	&bdquo;	Double low-9 quotation mark
    133	205	85	10000101	…	&#133;	&hellip;	Horizontal ellipsis
    134	206	86	10000110	?	&#134;	&dagger;	Dagger
    135	207	87	10000111	?	&#135;	&Dagger;	Double dagger
    136	210	88	10001000	?	&#136;	&circ;	Modifier letter circumflex accent
    137	211	89	10001001	‰	&#137;	&permil;	Per mille sign
    138	212	8A	10001010	?	&#138;	&Scaron;	Latin capital letter S with caron
    139	213	8B	10001011	?	&#139;	&lsaquo;	Single left-pointing angle quotation
    140	214	8C	10001100	?	&#140;	&OElig;	Latin capital ligature OE
    141	215	8D	10001101	 	 	 	 
    142	216	8E	10001110	?	&#142;	 	Latin captial letter Z with caron
    143	217	8F	10001111	 	 	 	 
    144	220	90	10010000	 	 	 	 
    145	221	91	10010001	‘	&#145;	&lsquo;	Left single quotation mark
    146	222	92	10010010	’	&#146;	&rsquo;	Right single quotation mark
    147	223	93	10010011	“	&#147;	&ldquo;	Left double quotation mark
    148	224	94	10010100	”	&#148;	&rdquo;	Right double quotation mark
    149	225	95	10010101	?	&#149;	&bull;	Bullet
    150	226	96	10010110	–	&#150;	&ndash;	En dash
    151	227	97	10010111	—	&#151;	&mdash;	Em dash
    152	230	98	10011000	?	&#152;	&tilde;	Small tilde
    153	231	99	10011001	?	&#153;	&trade;	Trade mark sign
    154	232	9A	10011010	?	&#154;	&scaron;	Latin small letter S with caron
    155	233	9B	10011011	?	&#155;	&rsaquo;	Single right-pointing angle quotation mark
    156	234	9C	10011100	?	&#156;	&oelig;	Latin small ligature oe
    157	235	9D	10011101	 	 	 	 
    158	236	9E	10011110	?	&#158;	 	Latin small letter z with caron
    159	237	9F	10011111	?	&#159;	&yuml;	Latin capital letter Y with diaeresis
    160	240	A0	10100000	 	&#160;	&nbsp;	Non-breaking space
    161	241	A1	10100001	?	&#161;	&iexcl;	Inverted exclamation mark
    162	242	A2	10100010	￠	&#162;	&cent;	Cent sign
    163	243	A3	10100011	￡	&#163;	&pound;	Pound sign
    164	244	A4	10100100	¤	&#164;	&curren;	Currency sign
    165	245	A5	10100101	￥	&#165;	&yen;	Yen sign
    166	246	A6	10100110	|	&#166;	&brvbar;	Pipe, Broken vertical bar
    167	247	A7	10100111	§	&#167;	&sect;	Section sign
    168	250	A8	10101000	¨	&#168;	&uml;	Spacing diaeresis - umlaut
    169	251	A9	10101001	?	&#169;	&copy;	Copyright sign
    170	252	AA	10101010	a	&#170;	&ordf;	Feminine ordinal indicator
    171	253	AB	10101011	?	&#171;	&laquo;	Left double angle quotes
    172	254	AC	10101100	?	&#172;	&not;	Not sign
    173	255	AD	10101101		&#173;	&shy;	Soft hyphen
    174	256	AE	10101110	?	&#174;	&reg;	Registered trade mark sign
    175	257	AF	10101111	ˉ	&#175;	&macr;	Spacing macron - overline
    176	260	B0	10110000	°	&#176;	&deg;	Degree sign
    177	261	B1	10110001	±	&#177;	&plusmn;	Plus-or-minus sign
    178	262	B2	10110010	2	&#178;	&sup2;	Superscript two - squared
    179	263	B3	10110011	3	&#179;	&sup3;	Superscript three - cubed
    180	264	B4	10110100	′	&#180;	&acute;	Acute accent - spacing acute
    181	265	B5	10110101	μ	&#181;	&micro;	Micro sign
    182	266	B6	10110110	?	&#182;	&para;	Pilcrow sign - paragraph sign
    183	267	B7	10110111	·	&#183;	&middot;	Middle dot - Georgian comma
    184	270	B8	10111000	?	&#184;	&cedil;	Spacing cedilla
    185	271	B9	10111001	1	&#185;	&sup1;	Superscript one
    186	272	BA	10111010	o	&#186;	&ordm;	Masculine ordinal indicator
    187	273	BB	10111011	?	&#187;	&raquo;	Right double angle quotes
    188	274	BC	10111100	?	&#188;	&frac14;	Fraction one quarter
    189	275	BD	10111101	?	&#189;	&frac12;	Fraction one half
    190	276	BE	10111110	?	&#190;	&frac34;	Fraction three quarters
    191	277	BF	10111111	?	&#191;	&iquest;	Inverted question mark
    192	300	C0	11000000	à	&#192;	&Agrave;	Latin capital letter A with grave
    193	301	C1	11000001	á	&#193;	&Aacute;	Latin capital letter A with acute
    194	302	C2	11000010	?	&#194;	&Acirc;	Latin capital letter A with circumflex
    195	303	C3	11000011	?	&#195;	&Atilde;	Latin capital letter A with tilde
    196	304	C4	11000100	?	&#196;	&Auml;	Latin capital letter A with diaeresis
    197	305	C5	11000101	?	&#197;	&Aring;	Latin capital letter A with ring above
    198	306	C6	11000110	?	&#198;	&AElig;	Latin capital letter AE
    199	307	C7	11000111	?	&#199;	&Ccedil;	Latin capital letter C with cedilla
    200	310	C8	11001000	è	&#200;	&Egrave;	Latin capital letter E with grave
    201	311	C9	11001001	é	&#201;	&Eacute;	Latin capital letter E with acute
    202	312	CA	11001010	ê	&#202;	&Ecirc;	Latin capital letter E with circumflex
    203	313	CB	11001011	?	&#203;	&Euml;	Latin capital letter E with diaeresis
    204	314	CC	11001100	ì	&#204;	&Igrave;	Latin capital letter I with grave
    205	315	CD	11001101	í	&#205;	&Iacute;	Latin capital letter I with acute
    206	316	CE	11001110	?	&#206;	&Icirc;	Latin capital letter I with circumflex
    207	317	CF	11001111	?	&#207;	&Iuml;	Latin capital letter I with diaeresis
    208	320	D0	11010000	D	&#208;	&ETH;	Latin capital letter ETH
    209	321	D1	11010001	?	&#209;	&Ntilde;	Latin capital letter N with tilde
    210	322	D2	11010010	ò	&#210;	&Ograve;	Latin capital letter O with grave
    211	323	D3	11010011	ó	&#211;	&Oacute;	Latin capital letter O with acute
    212	324	D4	11010100	?	&#212;	&Ocirc;	Latin capital letter O with circumflex
    213	325	D5	11010101	?	&#213;	&Otilde;	Latin capital letter O with tilde
    214	326	D6	11010110	?	&#214;	&Ouml;	Latin capital letter O with diaeresis
    215	327	D7	11010111	×	&#215;	&times;	Multiplication sign
    216	330	D8	11011000	?	&#216;	&Oslash;	Latin capital letter O with slash
    217	331	D9	11011001	ù	&#217;	&Ugrave;	Latin capital letter U with grave
    218	332	DA	11011010	ú	&#218;	&Uacute;	Latin capital letter U with acute
    219	333	DB	11011011	?	&#219;	&Ucirc;	Latin capital letter U with circumflex
    220	334	DC	11011100	ü	&#220;	&Uuml;	Latin capital letter U with diaeresis
    221	335	DD	11011101	Y	&#221;	&Yacute;	Latin capital letter Y with acute
    222	336	DE	11011110	T	&#222;	&THORN;	Latin capital letter THORN
    223	337	DF	11011111	?	&#223;	&szlig;	Latin small letter sharp s - ess-zed
    224	340	E0	11100000	à	&#224;	&agrave;	Latin small letter a with grave
    225	341	E1	11100001	á	&#225;	&aacute;	Latin small letter a with acute
    226	342	E2	11100010	a	&#226;	&acirc;	Latin small letter a with circumflex
    227	343	E3	11100011	?	&#227;	&atilde;	Latin small letter a with tilde
    228	344	E4	11100100	?	&#228;	&auml;	Latin small letter a with diaeresis
    229	345	E5	11100101	?	&#229;	&aring;	Latin small letter a with ring above
    230	346	E6	11100110	?	&#230;	&aelig;	Latin small letter ae
    231	347	E7	11100111	?	&#231;	&ccedil;	Latin small letter c with cedilla
    232	350	E8	11101000	è	&#232;	&egrave;	Latin small letter e with grave
    233	351	E9	11101001	é	&#233;	&eacute;	Latin small letter e with acute
    234	352	EA	11101010	ê	&#234;	&ecirc;	Latin small letter e with circumflex
    235	353	EB	11101011	?	&#235;	&euml;	Latin small letter e with diaeresis
    236	354	EC	11101100	ì	&#236;	&igrave;	Latin small letter i with grave
    237	355	ED	11101101	í	&#237;	&iacute;	Latin small letter i with acute
    238	356	EE	11101110	?	&#238;	&icirc;	Latin small letter i with circumflex
    239	357	EF	11101111	?	&#239;	&iuml;	Latin small letter i with diaeresis
    240	360	F0	11110000	e	&#240;	&eth;	Latin small letter eth
    241	361	F1	11110001	?	&#241;	&ntilde;	Latin small letter n with tilde
    242	362	F2	11110010	ò	&#242;	&ograve;	Latin small letter o with grave
    243	363	F3	11110011	ó	&#243;	&oacute;	Latin small letter o with acute
    244	364	F4	11110100	?	&#244;	&ocirc;	Latin small letter o with circumflex
    245	365	F5	11110101	?	&#245;	&otilde;	Latin small letter o with tilde
    246	366	F6	11110110	?	&#246;	&ouml;	Latin small letter o with diaeresis
    247	367	F7	11110111	÷	&#247;	&divide;	Division sign
    248	370	F8	11111000	?	&#248;	&oslash;	Latin small letter o with slash
    249	371	F9	11111001	ù	&#249;	&ugrave;	Latin small letter u with grave
    250	372	FA	11111010	ú	&#250;	&uacute;	Latin small letter u with acute
    251	373	FB	11111011	?	&#251;	&ucirc;	Latin small letter u with circumflex
    252	374	FC	11111100	ü	&#252;	&uuml;	Latin small letter u with diaeresis
    253	375	FD	11111101	y	&#253;	&yacute;	Latin small letter y with acute
    254	376	FE	11111110	t	&#254;	&thorn;	Latin small letter thorn
    255	377	FF	11111111	?	&#255;	&yuml;	Latin small letter y with diaeresis

## Unicode

binary -> ascii -> unicode

| Binary | ASCII           | Unicode           |
|:-------|:----------------|:------------------|
| bit    | 8 bits = 1 byte | 16 bits = 2 bytes |
|        | 255             | 80,000            |

bits

| 80000 |       |      |      |      |      |     |     | | 255 |    |    |    |   |   |   |   |
|:-----:|:-----:|:----:|:----:|:----:|:----:|:---:|:---:|-|:---:|:--:|:--:|:--:|:-:|:-:|:-:|:-:|
| 32768 | 16384 | 8192 | 4096 | 2048 | 1024 | 512 | 256 | | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |


### Table of Unicode

    0000-001F Control character
    0020-007F Basic Latin
    0080-00FF Latin-1 Supplement
    0100-017F Latin extended A
    0180-024F Latin extended B
    0250-02AF IPA Extentions
    02B0-02FF Spacing Modifier Letters
    0300-036F Combining Diacritical Marks
    0370-03FF Greek and Coptic
    0400-04FF Cyrillic
    0500-052F Cyrillic Supplement
    0530-058F Armenian
    0590-05FF Hebrew
    0600-06FF Arabic
    0700-074F Syriac
    0750-077F Arabic Supplement
    0780-07BF Thaana
    07C0-07FF NKo
    0800-083F Samaritan
    0840-085F Mandaic
    08A0-08FF Arabic Extended-A
    0900-097F Devanagari
    0980-09FF Bengali
    0A00-0A7F Gurmukhi
    0A80-0AFF Gujarati
    0B00-0B7F Oriya
    0B80-0BFF Tamil
    0C00-0C7F Telugu
    0C80-0CFF Kannada
    0D00-0D7F Malayalam
    0D80-0DFF Sinhala
    0E00-0E7F Thai
    0E80-0EFF Lao
    0F00-0FFF Tibetan
    1000-109F Myanmar
    10A0-10FF Georgian
    1100-11FF Hangul Jamo
    1200-137F Ethiopic
    1380-139F Ethiopic Supplement
    13A0-13FF Cherokee
    1400-167F Unified Canadian Aboriginal Syllabics
    1680-169F Ogham
    16A0-16FF Runic
    1700-171F Tagalog
    1720-173F Hanunoo
    1740-175F Buhid
    1760-177F Tagbanwa
    1780-17FF Khmer
    1800-18AF Mongolian
    18B0-18FF Unified Canadian Aboriginal Syllabics Extended
    1900-194F Limbu
    1950-197F Tai Le
    1980-19DF Tai Lue
    19E0-19FF Khmer Symbols
    1A00-1A1F Buginese
    1A20-1AAF Tai Tham
    1B00-1B87 Balinese
    1B80-1BBF Sundanese
    1BC0-1BFF Batak
    1C00-1C4F Lepcha
    1C50-1C7F Ol Chiki
    1CC0-1CCF Sundanese Supplement
    1CD0-1CFF Vedic Extensions
    1D00-1D7F Phonetic Extensions
    1D80-1DBF Phonetic Extensions Supplement
    1DC0-1DFF Combining Diacritical Marks Supplement
    1E00-1EFF Latin extended additional
    1F00-1FFF Greek Extended
    2000-206F General Punctuation
    2070-209F Superscripts and Subscripts
    20A0-20CF Currency Symbols
    20D0-20FF Combining Diacritical Marks for Symbols
    2100-214F Letterlike Symbols
    2150-218F Number Forms
    2190-21FF Arrows
    2200-22FF Mathematical Operators
    2300-23FF Miscellaneous Technical
    2400-243F Control Pictures
    2440-245F Optical Character Recognition
    2460-24FF Enclosed Alphanumerics
    2500-257F Box Drawing
    2580-259F Block Elements
    25A0-25FF Geometric Shapes
    2600-26FF Miscellaneous Symbols
    2700-27BF Dingbats
    27C0-27EF Miscellaneous Mathematical Symbols-A
    27F0-27FF Supplemental Arrows-A
    2800-28FF Braille Patterns
    2900-297F Supplemental Arrows-B
    2980-29FF Miscellaneous Mathematical Symbols-B
    2A00-2AFF Supplemental Mathematical Operators
    2B00-2BFF Miscellaneous Symbols and Arrows
    2C00-2C5F Glagolitic
    2C60-2C7F Latin Extended-C
    2C80-2CFF Coptic
    2D00-2D2F Georgian Supplement
    2D30-2D7F Tifinagh
    2D80-2DDF Ethiopic Extended
    2DE0-2DFF Cyrillic Extended-A
    2E00-2E7F Supplemental Punctuation
    2E80-2EFF CJK Radicals Supplement
    2F00-2FDF Kangxi Radicals
    2FF0-2FFF Ideographic Description Characters
    3000-303F CJK Symbols and Punctuation
    3040-309F Hiragana
    30A0-30FF Katakana
    3100-312F Bopomofo
    3130-318F Hangul Compatibility Jamo
    3190-319F Kanbun
    31A0-31BF Bopomofo Extended
    31C0-31EF CJK Strokes
    31F0-31FF Katakana Phonetic Extensions
    3200-32FF Enclosed CJK Letters and Months
    3300-33FF CJK Compatibility
    3400-4DBF CJK Unified Ideographs Extension A
    4DC0-4DFF Yijing Hexagram Symbols
    4E00-9FFF CJK Unified Ideographs
    A000-A48F Yi Syllables
    A490-A4CF Yi Radicals
    A500-A63F Vai
    A640-A69F Cyrillic Extended-B
    A6A0-A6FF Bamum
    A700-A71F Modifier Tone Letters
    A720-A7FF Latin Extended-D
    A800-A82F Syloti Nagri
    A830-A83F Common Indic Number Forms
    A840-A87F Phags-pa
    A880-A8DF Saurashtra
    A8E0-A8FF Devanagari Extended
    A900-A92F Kayah Li
    A930-A95F Rejang
    A960-A97F Hangul Jamo Extended-A
    A980-A9DF Javanese
    AA00-AA5F Cham
    AA60-AA7F Myanmar Extended-A
    AA80-AADF Tai Viet
    AAE0-AAFF Meetei Mayek Extensions
    AB00-AB2F Ethiopic Extended-A
    ABC0-ABFF Meetei Mayek
    AC00-D7AF Hangul Syllables
    D800-DB7F High Surrogates
    DB80-DBFF High Private Use Surrogates
    DC00-DFFF Low Surrogates
    E000-F8FF Private Use Area
    F900-FAFF CJK Compatibility Ideographs
    FB00-FB4F Alphabetic Presentation Forms
    FB50-FDFF Arabic Presentation Forms-A
    FE00-FE0F Variation Selectors
    FE10-FE1F Vertical Forms
    FE20-FE2F Combining Half Marks
    FE30-FE4F CJK Compatibility Forms
    FE50-FE6F Small Form Variants
    FE70-FEFF Arabic Presentation Forms-B
    FF00-FFEF Halfwidth and Fullwidth Forms
    FFF0-FFFF Specials


## Tools

- [Unicode Character Table](http://unicode-table.com/en/#control-character)
- [Text Mechanic](http://textmechanic.com/ASCII-Hex-Unicode-Base64-Converter.html)


## References
1. [ASCII and Unicode to Represent Characters in Binary Code][video-ascii-unicode]
2. [Table of ASCII Characters][ascii-table]
3. [ASCII Code - The extended ASCII table][ascii-code]

  [video-ascii-unicode]: http://education-portal.com/academy/lesson/ascii-binary-digital-and-analog-representation-of-data.html#lesson
  [ascii-table]: http://web.cs.mun.ca/~michael/c/ascii-table.html
  [ascii-code]: http://www.ascii-code.com/

