# IDX Source — Extracted Computed Styles

## Section Backgrounds (from getComputedStyle)
| Section | ID/Class | Background |
|---------|----------|------------|
| 0 - Promo | et_pb_section_0 | bg: #161a1d (solid dark) |
| 1 - MLS Confirm | et_pb_section_1 | bg: #0a7f28 (green) |
| 2 - Hero | et_pb_section_2 | bg: #003070, gradient: 230deg rgba(10,161,255,0.71) 0% → rgba(12,26,104,0.89) 80%, bg-image: hero-bg.webp |
| 3 - Features (white) | et_pb_section_3 | bg: white |
| 4 - Reviews preview | et_pb_section_4 | bg: #003070, gradient: 230deg same as hero, bg-image: hero-bg-reviews.webp |
| 5 - Consumers Love | et_pb_section_5 | bg: #eef0f1 |
| 6 - FB Integration | et_pb_section_6 | bg: white (NOT dark) |
| 7 - Testimonial | et_pb_section_7 | bg: gradient 316deg #003070 13% → #0aa1ff 100% |
| 8 - Checkout | et_pb_section_8 | bg: white |
| 9 - FAQ | et_pb_section_9 | bg: white |
| 10 - (script) | et_pb_section_10 | bg: white |
| 11 - Contact | et_pb_section_12 | bg: gradient #223949 → #161a1d, bg-image: hero-bg-contact.webp |
| 12 - Full Reviews | et_pb_section_13 | bg: #161a1d |

## CTA Buttons
- bg: #dc2209 (RED, not green!)
- color: white
- border-radius: 5px
- padding: 8px 24px
- font-size: 18px
- font-weight: 700
- letter-spacing: 1px

## Typography
- H1: 44.8px, weight 900, line-height 60.48px (1.35)
- Body: 18px, line-height 25.2px (1.4)
- Heading color: #223949 (dark blue-gray)
- Promo sale text gold: #d3b64f
- Sale code bg: #b01d00
- MLS name in checkout: color #f15f4c (coral)
- MLS label: color #5d9ed6 (blue)
- Highlighted quote text: .testimonial_highlight class

## Column Layout Ratios (from Divi classes)
- Hero: 1_2 + 1_2 (50/50) with gutters1
- MLS Search features: full-width heading row, then 2_5 + 3_5 (40/60 text left, image right)
- Never Miss A Lead: full-width heading row, then 3_5 + 2_5 (60/40 image left, text right) with gutters2
- Consumers Love: 1_4 + 1_2 + 1_4 (features|phone|features) with gutters1
- FB Integration: full-width heading, then 2_5 + 3_5 (text left, image right)
- Find a Home Tab: 3_5 + 2_5 (image left, text right) with gutters2
- Website Integration: 2_5 + 3_5 (text left, image right) with gutters2
- Testimonial: 2_5 + 3_5 (photo left, quote right)
- Checkout: 3_5 + 2_5 (form left, info right)
- FAQ: 1_2 + 1_2 (two columns, NOT accordion)
- Contact form: 1_5 + 3_5 + 1_5 (centered form)

## Wave Dividers (6 total)
Applied to sections 2, 4, 7, 12. All SVG base64 inline.

## Key Layout Note
- FAQ is NOT an accordion — it's 6 blurb cards in 2 columns (3 per side)
- Feature section headings are full-width rows ABOVE the content rows
- "Consumers Love" is a 3-column layout: features left, phone center, features right
